---
title: 'Сценарий: изоляция виртуальных сетей'
titleSuffix: Azure Virtual WAN
description: Сценарии для маршрутизации — изоляция виртуальных сетей
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: conceptual
ms.date: 09/22/2020
ms.author: cherylmc
ms.custom: fasttrack-edit
ms.openlocfilehash: f725932b30fad062123d6c752f2d563b84f98b2f
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2020
ms.locfileid: "91267641"
---
# <a name="scenario-isolating-vnets"></a>Сценарий: изоляция виртуальных сетей

При работе с маршрутизацией виртуальных сетей виртуальной глобальной сети существует несколько доступных сценариев. В этом сценарии цель заключается в том, чтобы запретить виртуальных сетей доступ к другому. Это называется изолированием виртуальных сетей. Дополнительные сведения о маршрутизации виртуальных концентраторов см. в статье [о маршрутизации виртуальных концентраторов](about-virtual-hub-routing.md).

## <a name="design"></a><a name="design"></a>Конструирование

В этом сценарии Рабочая нагрузка в определенной виртуальной сети остается изолированной и не может взаимодействовать с другими виртуальных сетей. Однако виртуальных сетей должны достигать всех ветвей (VPN, ER и VPN пользователя). Чтобы определить, сколько таблиц маршрутов потребуется, можно создать матрицу подключения. В этом сценарии он будет выглядеть, как в следующей таблице, где каждая ячейка показывает, может ли источник (строка) обмениваться данными с целевым объектом (столбцом):

| От |   Кому |  *Виртуальных сетей* | *Ветви* |
| -------------- | -------- | ---------- | ---|
| Виртуальные сети     | &#8594;|           |     X    |
| Ветви   | &#8594;|    X     |     X    |

В каждой ячейке в предыдущей таблице описывается, является ли подключение к виртуальной глобальной сети (на стороне "от" потока, заголовки строк) сведениями о префиксе назначения (стороне "to" последовательности, заголовках столбцов, выделенных курсивом) для конкретного потока трафика, где "X" означает, что подключение обеспечивает виртуальную глобальную сеть.

Эта матрица подключения дает нам два различных шаблона строк, которые транслируются в две таблицы маршрутов. Виртуальная глобальная сеть уже имеет таблицу маршрутов по умолчанию, поэтому нам потребуется другая таблица маршрутов. В этом примере мы будем наименовать таблицу маршрутов **RT_VNET**.

Виртуальных сетей будет связан с этой **RT_VNET** таблицей маршрутов. Так как им требуется подключение к ветвям, ветви необходимо распространить на **RT_VNET** (в противном случае виртуальных сетей не будет изучать префиксы ветвей). Так как ветви всегда связаны с таблицей маршрутов по умолчанию, виртуальных сетей потребуется распространить на таблицу маршрутов по умолчанию. В результате это окончательный проект:

* Виртуальные сети:
  * Связанная таблица маршрутов: **RT_VNET**
  * Распространение на таблицы маршрутов: **по умолчанию**
* Ветк
  * Связанная таблица маршрутов: **по умолчанию**
  * Распространение на таблицы маршрутов: **RT_VNET** и **по умолчанию**

Обратите внимание, что поскольку только ветви распространяются на таблицу маршрутов **RT_VNET**, это будут только те префиксы, которые виртуальных сетей будут изучать, а не другие виртуальных сетей.

Дополнительные сведения о маршрутизации виртуальных концентраторов см. в статье [о маршрутизации виртуальных концентраторов](about-virtual-hub-routing.md).

## <a name="workflow"></a><a name="workflow"></a>Рабочий процесс

Чтобы настроить этот сценарий, примите во внимание следующие шаги:

1. Создайте пользовательскую таблицу маршрутов в каждом концентраторе. В этом примере таблица маршрутов **RT_VNet**. Сведения о создании таблицы маршрутов см. [в статье Настройка маршрутизации виртуальных концентраторов](how-to-virtual-hub-routing.md). Дополнительные сведения о таблицах маршрутов см. в статье [о маршрутизации виртуальных концентраторов](about-virtual-hub-routing.md).
2. При создании таблицы **RT_VNet** маршрутов настройте следующие параметры.

   * **Ассоциация**. Выберите виртуальных сетей, которые необходимо изолировать.
   * **Распространение**. Выберите вариант для ветвей, что означает, что подключения Branch (VPN/ER/P2S) будут распространять маршруты в эту таблицу маршрутов.

:::image type="content" source="./media/routing-scenarios/isolated/isolated-vnets.png" alt-text="Изолированный виртуальных сетей":::

## <a name="next-steps"></a>Дальнейшие действия

* Дополнительные сведения о виртуальной глобальной сети см. в статье, содержащей [Часто задаваемые вопросы](virtual-wan-faq.md) о ней.
* Дополнительные сведения о маршрутизации виртуальных концентраторов см. в статье [о маршрутизации виртуальных концентраторов](about-virtual-hub-routing.md).
