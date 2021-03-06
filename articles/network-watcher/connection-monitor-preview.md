---
title: Монитор подключения (Предварительная версия) | Документация Майкрософт
description: Узнайте, как использовать монитор подключений (Предварительная версия) для мониторинга сетевого взаимодействия в распределенной среде.
services: network-watcher
documentationcenter: na
author: vinynigam
manager: agummadi
editor: ''
tags: azure-resource-manager
ms.service: network-watcher
ms.devlang: na
ms.topic: how-to
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/27/2020
ms.author: vinigam
ms.custom: mvc
ms.openlocfilehash: 80934dca73d7f8a205c62a49c418828cab1820e7
ms.sourcegitcommit: 7dacbf3b9ae0652931762bd5c8192a1a3989e701
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/16/2020
ms.locfileid: "92123738"
---
# <a name="network-connectivity-monitoring-with-connection-monitor-preview"></a>Мониторинг сетевых подключений с помощью монитора подключений (Предварительная версия)

Монитор подключения (Предварительная версия) обеспечивает единый мониторинг сквозного подключения в наблюдателе за сетями Azure. Функция монитора подключений (Предварительная версия) поддерживает Гибридные развертывания и облачное развертывание Azure. Наблюдатель за сетями предоставляет средства для мониторинга, диагностики и просмотра метрик, связанных с подключением, для развертываний Azure.

Ниже приведены некоторые варианты использования монитора подключения (Предварительная версия).

- Клиентская виртуальная машина веб-сервера взаимодействует с виртуальной машиной сервера базы данных в многоуровневом приложении. Необходимо проверить сетевое подключение между двумя виртуальными машинами.
- Вы хотите, чтобы виртуальные машины в регионе "Восточная часть США" могли проверить связь с виртуальными машинами в центральном регионе США и вы хотите сравнить задержки сети между регионами.
- У вас есть несколько локальных сайтов Office в Сиэтле, штат Вашингтон и Эшберн, Виргиния. Сайты Office подключаются к Microsoft 365 URL-адресам. Для пользователей Microsoft 365 URL-адресов Сравните задержки между Сиэтл и Эшберн.
- Гибридному приложению требуется подключение к конечной точке службы хранилища Azure. Локальный сайт и приложение Azure подключаются к одной конечной точке службы хранилища Azure. Необходимо сравнить задержки локального сайта с задержкой приложения Azure.
- Необходимо проверить подключение между локальными настройками и виртуальными машинами Azure, на которых размещается ваше облачное приложение.

На этапе предварительной версии монитор подключения объединяет лучшие два компонента: функция [монитора подключения](https://docs.microsoft.com/azure/network-watcher/network-watcher-monitoring-overview#monitor-communication-between-a-virtual-machine-and-an-endpoint) наблюдателя за сетями и [монитор подключения службы](https://docs.microsoft.com/azure/azure-monitor/insights/network-performance-monitor-service-connectivity)монитор производительности сети (NPM), [мониторинг ExpressRoute](https://docs.microsoft.com/azure/expressroute/how-to-npm)и функции [мониторинга производительности](https://docs.microsoft.com/azure/azure-monitor/insights/network-performance-monitor-performance-monitor) .

Ниже приведены некоторые преимущества монитора подключений (Предварительная версия).

* Унифицированный, интуитивно понятный интерфейс для потребностей в Azure и гибридного мониторинга
* Мониторинг подключения между регионами и между рабочими областями
* Более высокие частоты проверки и более наглядное представление о производительности сети
* Более быстрое оповещение для гибридных развертываний
* Поддержка проверок подключения, основанных на HTTP, TCP и ICMP 
* Метрики и поддержка Log Analytics для настройки тестирования Azure и не в Azure

![Диаграмма, показывающая, как монитор подключения взаимодействует с виртуальными машинами Azure, узлами, конечными точками и местами хранения данных, отличными от Azure.](./media/connection-monitor-2-preview/hero-graphic.png)

Чтобы начать работу с монитором подключений (Предварительная версия) для мониторинга, выполните следующие действия. 

1. Установите агенты наблюдения.
1. Включите наблюдатель за сетями в подписке.
1. Создайте монитор подключения.
1. Настройка анализа данных и оповещений.
1. Диагностика проблем в сети.

В следующих разделах приводятся подробные сведения по этим действиям.

## <a name="install-monitoring-agents"></a>Установка агентов наблюдения

Монитор подключения использует упрощенные исполняемые файлы для выполнения проверок подключения.  Она поддерживает проверки подключения как из среды Azure, так и из локальных сред. Используемый исполняемый файл зависит от того, размещена ли виртуальная машина в Azure или в локальной среде.

### <a name="agents-for-azure-virtual-machines"></a>Агенты для виртуальных машин Azure

Чтобы монитор подключения распознал виртуальные машины Azure в качестве источников мониторинга, установите на них расширение виртуальной машины агента наблюдателя за сетями. Это расширение также называется *расширением наблюдателя за сетями*. Для виртуальных машин Azure требуется расширение, чтобы активировать сквозное наблюдение и другие расширенные функции. 

Расширение наблюдателя за сетями можно установить при [создании виртуальной машины](https://docs.microsoft.com/azure/network-watcher/connection-monitor#create-the-first-vm). Вы также можете отдельно установить, настроить и устранить неполадки расширения наблюдателя за сетями для [Linux](https://docs.microsoft.com/azure/virtual-machines/extensions/network-watcher-linux) и [Windows](https://docs.microsoft.com/azure/virtual-machines/extensions/network-watcher-windows).

Правила для группы безопасности сети (NSG) или брандмауэра могут блокировать обмен данными между источником и назначением. Монитор подключения обнаруживает эту ошибку и показывает ее как диагностическое сообщение в топологии. Чтобы включить мониторинг подключения, убедитесь, что правила NSG и брандмауэра разрешают пакеты по протоколу TCP или ICMP между источником и назначением.

### <a name="agents-for-on-premises-machines"></a>Агенты для локальных компьютеров

Чтобы монитор подключения распознал локальные компьютеры как источники для мониторинга, установите агент Log Analytics на компьютерах. Затем включите решение Монитор производительности сети. Эти агенты связаны с рабочими областями Log Analytics, поэтому необходимо настроить идентификатор и первичный ключ рабочей области, прежде чем агенты смогут начать мониторинг.

Сведения об установке агента Log Analytics для компьютеров Windows см. в разделе [Azure Monitor расширение виртуальной машины для Windows](https://docs.microsoft.com/azure/virtual-machines/extensions/oms-windows).

Если путь включает брандмауэры или сетевые виртуальные устройства (NVA), убедитесь, что назначение доступно.

## <a name="enable-network-watcher-on-your-subscription"></a>Включение наблюдателя за сетями в подписке

Для всех подписок с виртуальной сетью используется наблюдатель за сетями. При создании виртуальной сети в подписке наблюдатель за сетями автоматически включается в регионе и подписке виртуальной сети. Это автоматическое включение не влияет на ресурсы или стоимость. Убедитесь, что наблюдатель за сетями не отключен явным образом в вашей подписке. 

Дополнительные сведения см. в разделе [Включение наблюдателя за сетями](https://docs.microsoft.com/azure/network-watcher/network-watcher-create).

## <a name="create-a-connection-monitor"></a>Создание монитора подключения 

Монитор подключений отслеживает связь через регулярные интервалы. Он информирует вас об изменениях в достижимости и задержке. Вы также можете проверить текущую и прошлую топологию сети между исходными агентами и конечными точками назначения.

Источники могут быть виртуальными машинами Azure или локальными компьютерами, на которых установлен агент мониторинга. Целевыми конечными точками могут быть Microsoft 365 URL-адреса, URL-адреса Dynamics 365, пользовательские URL-адреса, идентификаторы ресурсов виртуальных машин Azure, IPv4, IPv6, FQDN или любое доменное имя.

### <a name="access-connection-monitor-preview"></a>Доступ к монитору подключений (Предварительная версия)

1. На домашней странице портал Azure перейдите к **наблюдателю за сетями**.
1. Слева в разделе **мониторинг** выберите **монитор подключения (Предварительная версия)**.
1. Вы увидите все мониторы подключений, созданные в мониторе подключений (Предварительная версия). Чтобы просмотреть мониторы подключения, созданные в классическом интерфейсе монитора подключения, перейдите на вкладку **монитор подключения** .
    
  :::image type="content" source="./media/connection-monitor-2-preview/cm-resource-view.png" alt-text="Снимок экрана, показывающий мониторы подключений, созданные в мониторе подключений (Предварительная версия)" lightbox="./media/connection-monitor-2-preview/cm-resource-view.png":::

### <a name="create-a-connection-monitor"></a>Создание монитора подключения

В мониторах подключений, создаваемых в мониторе подключений (Предварительная версия), можно добавить локальные компьютеры и виртуальные машины Azure в качестве источников. Эти мониторы соединений также могут отслеживать подключение к конечным точкам. Конечные точки могут находиться в Azure или на любом другом URL-адресе или IP.

Монитор подключения (Предварительная версия) включает следующие сущности:

* **Ресурс монитора подключения** — ресурс Azure определенного региона. Все следующие сущности являются свойствами ресурса монитора подключения.
* **Конечная точка** — источник или назначение, участвующие в проверках подключения. Примеры конечных точек: виртуальные машины Azure, локальные агенты, URL-адреса и IP-адрес.
* **Конфигурация теста** — конфигурация, зависящая от протокола, для теста. В зависимости от выбранного протокола можно определить порт, пороговые значения, частоту проверки и другие параметры.
* **Тестовая группа** — группа, содержащая конечные точки источника, конечные точки назначения и конфигурации тестов. Монитор подключения может содержать более одной тестовой группы.
* **Тест** — сочетание исходной конечной точки, конечной точки назначения и конфигурации теста. Тест — это наиболее детализированный уровень доступности данных мониторинга. Данные мониторинга включают в себя процент проверок, завершившихся сбоем, и время приема-передачи (RTT).

 ![Схема, показывающая монитор подключения, определение связи между группами тестов и тестами](./media/connection-monitor-2-preview/cm-tg-2.png)

Предварительный просмотр монитора подключения можно создать с помощью [портал Azure](connection-monitor-preview-create-using-portal.md) или [ARMClient](connection-monitor-preview-create-using-arm-client.md)

Все источники, назначения и конфигурации тестов, добавленные в тестовую группу, будут разбиты на отдельные тесты. Ниже приведен пример разрыва работы источников и назначений.

* Тестовая группа: TG1
* Источники: 3 (A, B, C)
* Назначения: 2 (D, E)
* Конфигурации тестов: 2 (конфигурация 1, конфигурация 2)
* Всего тестов создано: 12

| Номер теста | Источник | Назначение | Конфигурация теста |
| --- | --- | --- | --- |
| 1 | A | D | Конфигурация 1 |
| 2 | A | D | Конфигурация 2 |
| 3 | A | E | Конфигурация 1 |
| 4 | A | E | Конфигурация 2 |
| 5 | B | D | Конфигурация 1 |
| 6 | B | D | Конфигурация 2 |
| 7 | B | E | Конфигурация 1 |
| 8 | B | E | Конфигурация 2 |
| 9 | C | D | Конфигурация 1 |
| 10 | C | D | Конфигурация 2 |
| 11 | C | E | Конфигурация 1 |
| 12 | C | E | Конфигурация 2 |

### <a name="scale-limits"></a> Ограничения масштабирования

Мониторы подключений имеют следующие ограничения масштабирования:

* Максимальное число мониторов подключения на подписку в одном регионе: 100
* Максимальное число групп тестов на монитор каждого подключения: 20
* Максимальное число источников и назначений на монитор подключения: 100
* Максимальное число конфигураций тестов на монитор каждого подключения: 20

## <a name="analyze-monitoring-data-and-set-alerts"></a>Анализ данных мониторинга и Настройка оповещений

После создания монитора подключения источники проверяют подключение к местам назначения на основе конфигурации теста.

### <a name="checks-in-a-test"></a>Проверки в тесте

В зависимости от протокола, выбранного в конфигурации теста, монитор подключения (Предварительная версия) выполняет ряд проверок для пары "источник-назначение". Проверки выполняются в соответствии с выбранной частотой тестирования.

При использовании протокола HTTP служба вычисляет количество HTTP-ответов, которые возвращали допустимый код ответа. Допустимые коды ответов можно задать с помощью PowerShell и CLI. Результат определяет процент неудачных проверок. Для вычисления RTT служба измеряет время между вызовом HTTP и ответом.

При использовании протокола TCP или ICMP служба вычисляет процент потерь пакетов, чтобы определить процент неудачных проверок. Для вычисления RTT служба измеряет время, затраченное на получение подтверждения (ACK) отправленных пакетов. Если вы включили данные traceroute для проверки сети, вы можете увидеть потери прыжков и задержки для локальной сети.

### <a name="states-of-a-test"></a>Состояния теста

На основе данных, возвращаемых проверками, тесты могут иметь следующие состояния.

* **Pass** — фактические значения для процента неудачных проверок и RTT находятся в пределах указанных пороговых значений.
* **Fail** — фактические значения для процента неудачных проверок или RTT превысили указанные пороговые значения. Если пороговое значение не указано, тест достигает состояния сбоя, если процент неудачных проверок равен 100.
* **Предупреждение** — 
     * Если задано пороговое значение и монитор подключения (Предварительная версия) наблюдает за ошибками в процентах более 80% от порога, тест помечается как предупреждение.
     * При отсутствии указанных пороговых значений монитор подключения (Предварительная версия) автоматически назначает пороговое значение. При превышении этого порога состояние теста изменится на Warning (предупреждение).Для времени кругового пути в проверках TCP или ICMP пороговое значение — 750msec. Для проверок с ошибками в процентах порог равен 10%. 
* Не **определено**   — Нет данных в Log Analytics рабочей области.Проверьте метрики. 
* **Не работает**   — Отключено путем отключения тестовой группы  

### <a name="data-collection-analysis-and-alerts"></a>Сбор, анализ и оповещения данных

Данные, собираемые монитором подключения (Предварительная версия), хранятся в Log Analytics рабочей области. Эта Рабочая область настраивается при создании монитора подключения. 

Данные мониторинга также доступны в метриках Azure Monitor. Вы можете использовать Log Analytics для хранения данных мониторинга до тех пор, пока хотите. По умолчанию Azure Monitor хранит метрики за 30 дней. 

[Для данных можно задать оповещения на основе метрик](https://azure.microsoft.com/blog/monitor-at-scale-in-azure-monitor-with-multi-resource-metric-alerts/).

#### <a name="monitoring-dashboards"></a>Мониторинг панелей мониторинга

На панелях мониторинга отображаются список мониторов подключений, к которым можно получить доступ для подписок, регионов, меток времени, источников и типов назначения.

Когда вы переходите к монитору подключений (Предварительная версия) из наблюдателя за сетями, вы можете просматривать данные следующим образом.

* **Монитор подключения** — список всех мониторов подключения, созданных для подписок, регионов, меток времени, источников и типов назначения. Это представление используется по умолчанию.
* **Группы тестов** — список всех групп тестов, созданных для подписок, регионов, меток времени, источников и типов назначения. Эти группы тестов не фильтруются мониторами подключения.
* **Тест** — список всех тестов, выполняемых для подписок, регионов, меток времени, источников и типов назначения. Эти тесты не фильтруются с помощью мониторов подключения или тестовых групп.

На следующем рисунке три представления данных обозначаются стрелкой 1.

На панели мониторинга можно развернуть каждый монитор подключения, чтобы просмотреть его тестовые группы. Затем можно развернуть каждую тестовую группу, чтобы увидеть тесты, которые выполняются в ней. 

Вы можете отфильтровать список на основе:

* **Фильтры верхнего уровня** — список поиска по тексту, типу сущности (монитору подключения, тестовой группе или тесту) отметка времени и область. Область действия включает подписки, регионы, источники и типы назначения. См. Box 1 на следующем рисунке.
* **Фильтры на основе состояния** — фильтрация по состоянию монитора подключения, тестовой группы или теста. См. Box 2 на следующем рисунке.
* **Фильтр на основе оповещений** — фильтрация по предупреждениям, запущенным в ресурсе монитора подключения. См. поле 3 на следующем рисунке.

  :::image type="content" source="./media/connection-monitor-2-preview/cm-view.png" alt-text="Снимок экрана, показывающий мониторы подключений, созданные в мониторе подключений (Предварительная версия)" lightbox="./media/connection-monitor-2-preview/cm-view.png":::
    
Например, чтобы просмотреть все тесты в мониторе подключения (Предварительная версия), где исходный IP-адрес — 10.192.64.56:
1. Измените представление для **проверки**.
1. В поле поиска введите *10.192.64.56* .
1. В **области** в фильтре верхнего уровня выберите **источники**.

Для отображения только неудачных тестов в мониторе подключения (Предварительная версия) с исходным IP-адресом 10.192.64.56:
1. Измените представление для **проверки**.
1. Для фильтра, основанного на состоянии, выберите **сбой**.
1. В поле поиска введите *10.192.64.56* .
1. В **области** в фильтре верхнего уровня выберите **источники**.

Для отображения только неудачных тестов в мониторе подключения (Предварительная версия), где назначением является outlook.office365.com:
1. Измените представление на " **тестирование**".
1. Для фильтра, основанного на состоянии, выберите **сбой**.
1. В поле поиска введите *Outlook.Office365.com* .
1. В **области** в фильтре верхнего уровня выберите **назначения**.
  
  :::image type="content" source="./media/connection-monitor-2-preview/tests-view.png" alt-text="Снимок экрана, показывающий мониторы подключений, созданные в мониторе подключений (Предварительная версия)" lightbox="./media/connection-monitor-2-preview/tests-view.png":::

Чтобы узнать причину сбоя монитора подключения или тестовой группы или теста, щелкните столбец с именем Reason.  Это указывает, какое пороговое значение (Проверка сбоев% или RTT) было нарушено, и связанные с ним диагностические сообщения
  
  :::image type="content" source="./media/connection-monitor-2-preview/cm-reason-of-failure.png" alt-text="Снимок экрана, показывающий мониторы подключений, созданные в мониторе подключений (Предварительная версия)" lightbox="./media/connection-monitor-2-preview/cm-reason-of-failure.png":::
    
Для просмотра тенденций в режиме приема-передачи и процента неудачных проверок для монитора подключения:
1. Выберите монитор подключения, который необходимо исследовать.

    :::image type="content" source="./media/connection-monitor-2-preview/cm-drill-landing.png" alt-text="Снимок экрана, показывающий мониторы подключений, созданные в мониторе подключений (Предварительная версия)" lightbox="./media/connection-monitor-2-preview/cm-drill-landing.png":::

1. Вы увидите следующие разделы.  
    1. Основные свойства выбранного монитора подключения для конкретных ресурсов 
    1. Суммар 
        1. Агрегированные линии тренда для RTT и процент неудачных проверок для всех тестов в мониторе подключения. Для просмотра сведений можно задать определенное время.
        1. 5 основных групп тестов, источников и назначений, основанных на RTT или проценте неудачных проверок. 
    1. Вкладки для групп тестов, источников, назначений и конфигураций тестов — список тестовых групп, источников или назначений в мониторе подключения. Проверка сбоев тестов, агрегирование RTT и проверки со сбоем% значений.  Вы также можете вернуться назад во времени, чтобы просмотреть данные. 
    1. Проблемы — проблемы уровня прыжков для каждого теста в мониторе подключения. 

    :::image type="content" source="./media/connection-monitor-2-preview/cm-drill-landing-2.png" alt-text="Снимок экрана, показывающий мониторы подключений, созданные в мониторе подключений (Предварительная версия)" lightbox="./media/connection-monitor-2-preview/cm-drill-landing-2.png":::

1. Вы можете выполнить следующие действия:
    * Нажмите кнопку Просмотреть все тесты, чтобы просмотреть все тесты в мониторе подключения.
    * Щелкните Просмотреть все группы тестов, конфигурации тестов, источники и назначения, чтобы просмотреть сведения, относящиеся к каждому из них. 
    * Выберите тестовую группу, конфигурацию теста, источник или назначение, чтобы просмотреть все тесты в сущности.

1. В представлении Просмотр всех тестов можно выполнить следующие действия.
    * Выберите тесты и нажмите кнопку сравнить.
    
    :::image type="content" source="./media/connection-monitor-2-preview/cm-compare-test.png" alt-text="Снимок экрана, показывающий мониторы подключений, созданные в мониторе подключений (Предварительная версия)" lightbox="./media/connection-monitor-2-preview/cm-compare-test.png":::
    
    * Использование кластера для расширения составных ресурсов, таких как виртуальная сеть, подсети и дочерние ресурсы
    * Просмотрите топологию для всех тестов, щелкнув топология.

Для просмотра тенденций в режиме приема-передачи и процента неудачных проверок для тестовой группы:
1. Выберите тестовую группу, которую необходимо исследовать. 
1. Вы будете просматривать данные, аналогичные монитору подключения — Essentials, Summary, таблица для групп тестов, источников, назначений и конфигураций тестов. Перемещаться по ним, как это делается для монитора подключения

Чтобы просмотреть тренды в RTT и процент неудачных проверок для теста, выполните следующие действия.
1. Выберите тест, который необходимо исследовать. Вы увидите топологию сети и диаграммы комплексного тренда для проверок со сбоем% и временем кругового пути. Чтобы просмотреть выявленные проблемы, в топологии выберите любой прыжок в пути. (Эти прыжки являются ресурсами Azure.) В настоящее время эта функция недоступна для локальных сетей

  :::image type="content" source="./media/connection-monitor-2-preview/cm-test-topology.png" alt-text="Снимок экрана, показывающий мониторы подключений, созданные в мониторе подключений (Предварительная версия)" lightbox="./media/connection-monitor-2-preview/cm-test-topology.png":::

#### <a name="log-queries-in-log-analytics"></a>Запросы к журналу в Log Analytics

Используйте Log Analytics для создания пользовательских представлений данных мониторинга. Все данные, отображаемые в пользовательском интерфейсе, — из Log Analytics. Данные в репозитории можно анализировать в интерактивном режиме. Сопоставьте данные из Работоспособность агентов или других решений, основанных на Log Analytics. Экспортируйте данные в Excel или Power BI или создайте ссылку с общим доступом.

#### <a name="metrics-in-azure-monitor"></a>Метрики в Azure Monitor

В мониторах соединений, созданных до работы монитора подключений (Предварительная версия), доступны все четыре метрики:% Зонды Failed, Аверажераундтрипмс, Чекксфаиледперцент (Предварительная версия) и Раундтриптимемс (Предварительная версия). В мониторах соединений, созданных в интерфейсе монитора подключений (Предварительная версия), данные доступны только для тех метрик, которые помечены *(Предварительная версия)*.

  :::image type="content" source="./media/connection-monitor-2-preview/monitor-metrics.png" alt-text="Снимок экрана, показывающий мониторы подключений, созданные в мониторе подключений (Предварительная версия)" lightbox="./media/connection-monitor-2-preview/monitor-metrics.png":::

При использовании метрик задайте тип ресурса Microsoft. Network/Нетворкватчерс/Коннектионмониторс.

| Метрика | Отображаемое имя | Unit | Тип агрегирования | Описание | Измерения |
| --- | --- | --- | --- | --- | --- |
| ProbesFailedPercent | Доля проб с ошибками, в процентах | Процент | Среднее | Сбой пробы мониторинга подключения. | Нет измерений |
| AverageRoundtripMs | Среднее Время приема-передачи (мс) | Миллисекунды | Среднее | Среднее время приема сетевого соединения для проверок мониторинга подключения, отправляемых между источником и назначением. |             Нет измерений |
| Чекксфаиледперцент (Предварительная версия) | % Проверок с ошибками (Предварительная версия) | Процент | Среднее | Процент неудачных проверок для теста. | коннектионмониторресаурцеид <br>саурцеаддресс <br>SourceName <br>Значение sourceresourceid <br>Тип источника <br>Протокол <br>DestinationAddress <br>DestinationName <br>дестинатионресаурцеид <br>DestinationType <br>DestinationPort <br>тестграупнаме <br>тестконфигуратионнаме <br>Регион |
| Раундтриптимемс (Предварительная версия) | Время приема-передачи (МС) (Предварительная версия) | Миллисекунды | Среднее | RTT для проверок, отправленных между источником и назначением. Это значение не является средним. | коннектионмониторресаурцеид <br>саурцеаддресс <br>SourceName <br>Значение sourceresourceid <br>Тип источника <br>Протокол <br>DestinationAddress <br>DestinationName <br>дестинатионресаурцеид <br>DestinationType <br>DestinationPort <br>тестграупнаме <br>тестконфигуратионнаме <br>Регион |

#### <a name="metric-based-alerts-for-connection-monitor"></a>Оповещения на основе метрик для монитора подключения

Вы можете создавать оповещения метрики для мониторов подключения, используя приведенные ниже методы. 

1. В мониторе подключения (Предварительная версия) во время создания монитора подключения [с помощью портал Azure](connection-monitor-preview-create-using-portal.md#) 
1. В мониторе подключения (Предварительная версия) с помощью команды "настроить оповещения" на панели мониторинга 
1. Из Azure Monitor — чтобы создать оповещение в Azure Monitor: 
    1. Выберите ресурс монитора подключения, созданный в мониторе подключения (Предварительная версия).
    1. Убедитесь, что **Метрика** отображается как тип сигнала для монитора подключения.
    1. В поле **Добавить условие**для **имени сигнала**выберите **чекксфаиледперцент (Предварительная версия)** или **раундтриптимемс (Предварительная версия)**.
    1. В качестве **типа сигнала**выберите **метрики**. Например, выберите **чекксфаиледперцент (Предварительная версия)**.
    1. Перечислены все измерения для метрики. Выберите имя измерения и значение измерения. Например, выберите **адрес источника** , а затем введите IP-адрес любого источника в мониторе подключения.
    1. В окне **логика оповещения**введите следующие сведения:
        * **Тип условия**: **статический**.
        * **Условие** и **пороговое значение**.
        * **Детализация статистической обработки и частота оценки**: монитор подключения (Предварительная версия) обновляет данные каждую минуту.
    1. В списке **действия**выберите группу действий.
    1. Укажите сведения о предупреждении.
    1. Создайте правило генерации оповещений.

  :::image type="content" source="./media/connection-monitor-2-preview/mdm-alerts.jpg" alt-text="Снимок экрана, показывающий мониторы подключений, созданные в мониторе подключений (Предварительная версия)" lightbox="./media/connection-monitor-2-preview/mdm-alerts.jpg":::

## <a name="diagnose-issues-in-your-network"></a>Диагностика проблем в сети

Монитор подключения (Предварительная версия) помогает диагностировать проблемы в мониторе подключения и в сети. Проблемы в гибридной сети обнаруживаются установленными ранее агентами Log Analytics. Проблемы в Azure обнаруживаются расширением наблюдателя за сетями. 

Вы можете просматривать проблемы в сети Azure в топологии сети.

Для сетей, источники которых являются локальными виртуальными машинами, можно обнаружить следующие проблемы.

* Истекло время ожидания для запроса.
* Конечная точка не разрешается DNS-временными или постоянными. Недопустимый URL-адрес.
* Узлы не найдены.
* Источнику не удалось подключиться к назначению. Целевой объект недоступен по протоколу ICMP.
* Проблемы, связанные с сертификатом: 
    * Для проверки подлинности агента требуется сертификат клиента. 
    * Список перерасположения сертификатов недоступен. 
    * Имя узла конечной точки не соответствует субъекту или альтернативному имени субъекта сертификата. 
    * Корневой сертификат отсутствует в хранилище доверенных центров сертификации на локальном компьютере исходного компьютера. 
    * Истек срок действия SSL-сертификата, недопустимый, отозванный или несовместимый.

Для сетей, источники которых являются виртуальными машинами Azure, можно обнаружить следующие проблемы.

* Проблемы с агентом:
    * Агент остановлен.
    * Сбой разрешения DNS.
    * Ни приложение, ни прослушиватель не ожидают передачи данных на порт назначения.
    * Не удалось открыть сокет.
* Проблемы состояния виртуальной машины: 
    * Запуск
    * Остановка
    * Остановлена
    * Отмена выделения
    * Освобождено
    * Перезагрузки
    * Не выделено
* Отсутствует запись в таблице ARP.
* Трафик заблокирован из-за проблем с локальным брандмауэром или NSG правил.
* Проблемы шлюза виртуальной сети: 
    * Отсутствующие маршруты.
    * Туннель между двумя шлюзами отключен или отсутствует.
    * Второй шлюз не найден в туннеле.
    * Сведения об пиринга не найдены.
* В Microsoft погранично отсутствует маршрут.
* Трафик остановлен из-за системных маршрутов или UDR.
* BGP не включен в подключении шлюза.
* Проверка DIP не работает в подсистеме балансировки нагрузки.

## <a name="next-steps"></a>Next Steps
    
   * Узнайте [, как создать монитор подключений (Предварительная версия) с помощью портал Azure](connection-monitor-preview-create-using-portal.md)  
   * Узнайте [, как создать монитор подключений (Предварительная версия) с помощью ARMClient](connection-monitor-preview-create-using-arm-client.md)  
