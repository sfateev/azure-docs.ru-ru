---
title: 'Сценарий: маршрутизация трафика через NVA с помощью пользовательских параметров'
titleSuffix: Azure Virtual WAN
description: Этот сценарий помогает маршрутизировать трафик через NVA, используя другой NVA для трафика, связанного с Интернетом.
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: conceptual
ms.date: 09/22/2020
ms.author: cherylmc
ms.custom: fasttrack-edit
ms.openlocfilehash: e1cf9faeab60264d491539256828151e496ade8f
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2020
ms.locfileid: "91267505"
---
# <a name="scenario-route-traffic-through-nvas---custom-preview"></a>Сценарий: маршрутизация трафика через NVA — пользовательский (Предварительная версия)

При работе с маршрутизацией виртуальных сетей виртуальной глобальной сети существует несколько доступных сценариев. В этом сценарии NVA (сетевое виртуальное устройство) целью является маршрутизация трафика через NVA для обмена данными между виртуальных сетей и ветвями и использование другого NVA для трафика, связанного с Интернетом. Дополнительные сведения о маршрутизации виртуальных концентраторов см. в статье [о маршрутизации виртуальных концентраторов](about-virtual-hub-routing.md).

## <a name="design"></a><a name="design"></a>Конструирование

В этом сценарии мы будем использовать соглашение об именовании:

* "Виртуальная сеть службы" для виртуальных сетей, в которых пользователи развернули NVA (VNet 4 на **рис. 1**) для проверки трафика, не входящего в Интернет.
* "DMZ VNet" для виртуальных сетей, в которых пользователи развернули NVA для проверки трафика, связанного с Интернетом (виртуальная сеть 5 на **рис. 1**).
* "NVA спицы" для виртуальных сетей, подключенных к виртуальной сети NVA (VNet 1, VNet 2 и VNet 3 на **рис. 1**).
* "Концентраторы" для виртуальных WAN-концентраторов, управляемых Майкрософт.

Следующая матрица подключения содержит сводные данные о потоках, поддерживаемых в этом сценарии:

**Матрица подключения**

| Как получить          | на:|*NVA спицы*|*Виртуальная сеть службы*|*Виртуальная сеть DMZ*|*Статические ветви*|
|---|---|---|---|---|---|
| **NVA спицы**| &#8594;|      X |            X |   Пиринг |    Статические    |
| **Виртуальная сеть службы**| &#8594;|    X |            X |      X    |      X       |
| **Виртуальная сеть DMZ** | &#8594;|       X |            X |      X    |      X       |
| **Ветви** | &#8594;|  Статические |            X |      X    |      X       |

Каждая ячейка в матрице подключения описывает, является ли подключение к виртуальной глобальной сети (на стороне потока "от", заголовки строк), определяет префикс назначения (сторону "до" последовательности, заголовки столбцов в курсиве) для конкретного потока трафика. "X" означает, что подключение предоставляется в собственном режиме виртуальной глобальной сети, а "статический" означает, что подключение обеспечивает виртуальную глобальную сеть с использованием статических маршрутов. Давайте подробно рассмотрим различные строки:

* NVA периферийные серверы:
  * Периферийные серверы будут достигать других периферийных устройств непосредственно через виртуальные концентраторы глобальной сети.
  * Периферийные серверы получают возможность подключения к ветвям через статический маршрут, указывающий на виртуальную сеть службы. Они не должны изучать конкретные префиксы из ветвей (в противном случае они будут более конкретными и переопределять сводку).
  * Периферийные серверы отправляют Интернет-трафик в виртуальную сеть DMZ через пиринг прямого подключения к виртуальной сети.
* Ветк
  * Ветви будут получать периферийные серверы через статическую маршрутизацию, указывающую на виртуальную сеть службы. Они не должны изучать конкретные префиксы из виртуальных сетей, которые переопределяют обобщенный статический маршрут.
* Виртуальная сеть службы будет похожа на виртуальную сеть общих служб, которая должна быть доступна из каждой виртуальной сети и каждой ветви.
* Для виртуальной сети DMZ не требуется подключение через виртуальную сеть WAN, так как трафик, который он будет поддерживать, будет поступать через одноранговые пирингы виртуальной сети. Однако для упрощения настройки мы будем использовать ту же модель подключения, что и для виртуальной сети DMZ.

Таким образом, наша матрица подключения предоставляет три различных шаблона подключения, которые преобразуются в три таблицы маршрутов. Связи с различными виртуальных сетей будут выглядеть следующим образом:

* NVA периферийные серверы:
  * Связанная таблица маршрутов: **RT_V2B**
  * Распространение на таблицы маршрутов: **RT_V2B** и **RT_SHARED**
* NVA виртуальных сетей (внутренний и Интернет):
  * Связанная таблица маршрутов: **RT_SHARED**
  * Распространение на таблицы маршрутов: **RT_SHARED**
* Ветк
  * Связанная таблица маршрутов: **по умолчанию**
  * Распространение на таблицы маршрутов: **RT_SHARED** и **по умолчанию**

Эти статические маршруты необходимы для того, чтобы трафик между ветвями и виртуальными сетями проходит через NVA в виртуальной сети службы (VNet 4):

| Описание | Таблица маршрутов | Статический маршрут              |
| ----------- | ----------- | ------------------------- |
| Ветви    | RT_V2B      | 10.2.0.0/16-> vnet4conn  |
| NVA спицы  | По умолчанию     | 10.1.0.0/16-> vnet4conn  |

Теперь виртуальная Глобальная сеть знает, в какое подключение следует отправить пакеты, но соединение должно знать, что делать при получении этих пакетов: это место, где используются таблицы маршрутов подключения.

| Описание | Соединение | Статический маршрут            |
| ----------- | ---------- | ----------------------- |
| VNet2Branch | vnet4conn  | 10.2.0.0/16-> 10.4.0.5 |
| Branch2VNet | vnet4conn  | 10.1.0.0/16-> 10.4.0.5 |

На этом этапе все должно быть готово.

Дополнительные сведения о маршрутизации виртуальных концентраторов см. в статье [о маршрутизации виртуальных концентраторов](about-virtual-hub-routing.md).

## <a name="architecture"></a><a name="architecture"></a>Архитектура

На **рис. 1**имеется один концентратор, **концентратор 1**.

* **Концентратор 1** напрямую подключен к NVA виртуальных сетей **vnet 4** и **vnet 5**.

* Трафик между виртуальных сетей 1, 2 и 3 и ветвями (VPN/ER/P2S) должен проходить через **виртуальную сеть 4 NVA** 10.4.0.5.

* Весь Интернет-трафик, связанный с виртуальных сетей 1, 2 и 3, должен проходить через **виртуальную сеть 5 NVA** 10.5.0.5.

**Рис. 1**

:::image type="content" source="./media/routing-scenarios/nva-custom/figure-1.png" alt-text="Рис. 1":::

## <a name="workflow"></a><a name="workflow"></a>Рабочий процесс

Чтобы настроить маршрутизацию через NVA, выполните следующие действия:

1. Чтобы трафик, привязанный к Интернету, мог проходить через виртуальную сеть 5, требуется виртуальных сетей 1, 2 и 3, чтобы напрямую подключаться через пиринг виртуальных сетей к виртуальной сети 5. Кроме того, необходимо настроить UDR в виртуальных сетей для 0.0.0.0/0 и следующего прыжка 10.5.0.5. В настоящее время Виртуальная глобальная сеть не допускает NVA следующего прыжка в виртуальном концентраторе для 0.0.0.0/0.

1. В портал Azure перейдите к виртуальному концентратору и создайте настраиваемую таблицу маршрутов **RT_Shared** , которая будет изучать маршруты через распространение из всех виртуальных сетей и подключений филиала. На **рис. 2**это является пустой таблицей настраиваемого маршрута **RT_Shared**.

   * **Маршруты:** Нет необходимости добавлять статические маршруты.

   * **Ассоциация:** Выберите виртуальных сетей 4 и 5, что означает, что подключения виртуальных сетей 4 и 5 будут связаны с таблицей маршрутов **RT_Shared**.

   * **Распространение:** Так как все ветви и подключения к виртуальной сети должны динамически распространять свои маршруты в эту таблицу маршрутов, выберите пункт ветви и все виртуальных сетей.

1. Создание пользовательской таблицы маршрутов **RT_V2B** для направления трафика от виртуальных сетей 1, 2 и 3 к ветвям.

   * **Маршруты:** Добавьте агрегированную статическую запись маршрута для ветвей (VPN/ER/P2S) (10.2.0.0/16 на **рис. 2**) со следующим прыжком в качестве подключения к виртуальной сети 4. Кроме того, необходимо настроить статический маршрут в подключении к виртуальной сети 4 для префиксов филиалов и указать следующий прыжок в качестве конкретного IP-адреса NVA в виртуальной сети 4.

   * **Ассоциация:** Выберите все виртуальных сетей 1, 2 и 3. Это подразумевает, что подключения к виртуальной сети 1, 2 и 3 будут связаны с этой таблицей маршрутов и смогут изучать маршруты (статические и динамические через распространение) в этой таблице маршрутов.

   * **Распространение:** Подключения распространяют маршруты в таблицы маршрутов. Выбор виртуальных сетей 1, 2 и 3 позволит распространить маршруты от виртуальных сетей 1, 2 и 3 к этой таблице маршрутов. Нет необходимости распространять маршруты от подключений филиала к RT_V2B, так как трафик виртуальной сети филиала проходит через NVA в виртуальной сети 4.
  
1. Измените таблицу маршрутов по умолчанию **дефаултраутетабле**.

   Все VPN-подключения ExpressRoute и VPN-соединения пользователей связаны с таблицей маршрутов по умолчанию. Все VPN-подключения ExpressRoute и User VPN распространяют маршруты на один и тот же набор таблиц маршрутов.

   * **Маршруты:** Добавьте агрегированную статическую запись маршрута для виртуальных сетей 1, 2 и 3 (10.1.0.0/16 на рис. **2**) со следующим прыжком в качестве подключения к виртуальной сети 4. Кроме того, необходимо настроить статический маршрут в подключении к виртуальной сети 4 для VNet 1, 2 и 3 обобщенных префиксов и указать следующий прыжок в качестве конкретного IP-адреса NVA в виртуальной сети 4.

   * **Ассоциация:** Убедитесь, что выбран параметр для ветвей (VPN/ER/P2S), и что локальные подключения филиала связаны с *дефаултраутетабле*.

   * **Распространение из:** Убедитесь, что выбран параметр для ветвей (VPN/ER/P2S), чтобы локальные подключения распространяют маршруты в *дефаултраутетабле*.

**Рис. 2**

:::image type="content" source="./media/routing-scenarios/nva-custom/figure-2.png" alt-text="Рис. 1" lightbox="./media/routing-scenarios/nva-custom/figure-2.png":::

## <a name="next-steps"></a>Дальнейшие действия

* Дополнительные сведения о виртуальной глобальной сети см. в статье, содержащей [Часто задаваемые вопросы](virtual-wan-faq.md) о ней.
* Дополнительные сведения о маршрутизации виртуальных концентраторов см. в статье [о маршрутизации виртуальных концентраторов](about-virtual-hub-routing.md).
