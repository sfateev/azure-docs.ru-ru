---
title: Защита данных в центре безопасности Azure | Документация Майкрософт
description: В этой статье объясняется, каким образом обеспечивается управление данными и их защита в центре безопасности Azure.
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.assetid: 33f2c9f4-21aa-4f0c-9e5e-4cd1223e39d7
ms.service: security-center
ms.devlang: na
ms.topic: overview
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/13/2020
ms.author: memildin
ms.openlocfilehash: d829ffb9d3a264052e3f688018acd7afa854578e
ms.sourcegitcommit: 2c586a0fbec6968205f3dc2af20e89e01f1b74b5
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/14/2020
ms.locfileid: "92018276"
---
# <a name="azure-security-center-data-security"></a>Защита данных в Центре безопасности Azure

Чтобы помочь клиентам предотвращать и выявлять угрозы, а также реагировать на них, центр безопасности Azure собирает и обрабатывает данные о безопасности, в том числе сведения о конфигурации, метаданные, журналы событий и многое другое. Корпорация Майкрософт следует строгим нормативным требованиям и указаниям по безопасности — от создания кода до эксплуатации служб.

В этой статье показано, как осуществляется управление данными и их защита в Центре безопасности.

## <a name="data-sources"></a>Источники данных
Для отслеживания состояния безопасности, определения уязвимостей, предоставления рекомендаций по их устранению и обнаружения активных угроз Центр безопасности анализирует данные из следующих источников:

- **Службы Azure**: Из развернутых служб Azure считываются сведения о конфигурации путем взаимодействия с поставщиком ресурсов соответствующей службы.
- **Сетевой трафик.** Из инфраструктуры Майкрософт считывается выборка метаданных сетевого трафика, например IP-адрес и порт источника и назначения, размер пакета и сетевой протокол.
- **Решения партнеров.** Из интегрированных решений партнеров, например брандмауэров и решений по защите от вредоносных программ, собираются оповещения системы безопасности.
- **Виртуальные машины.** С виртуальных машин собираются сведения о конфигурации и событиях безопасности, например события Windows, журналы аудита и сообщения системного журнала.


## <a name="data-protection"></a>Защита данных

### <a name="data-segregation"></a>Разделение данных
Данные логическим путем отделяются для каждого компонента службы. Все данные отмечаются тегами по организациям. Эти теги существуют в течение всего жизненного цикла данных и используются на каждом уровне службы.

### <a name="data-access"></a>Доступ к данным
Чтобы предоставить рекомендации по обеспечению безопасности и обнаружению потенциальных угроз безопасности, сотрудники корпорации Майкрософт могут получать доступ к сведениям, которые собираются или анализируются службами Azure. К таким данным относятся сведения о событиях создания процесса и другие артефакты, которые могут содержать данные клиента или персональные данные, хранимые на компьютерах. 

Мы соблюдаем положения [Дополнения к Условиям использования веб-служб, регулирующее защиту данных](https://www.microsoftvolumelicensing.com/Downloader.aspx?DocumentId=17880), которые гласят, что корпорация Майкрософт не будет использовать данные клиента или извлекать из них сведения для рекламных или других коммерческих целей. Мы используем данные клиента только в рамках предоставления служб Azure, а также для совместимых с этим целей. Вы сохраняете все права на данные клиента.

### <a name="data-use"></a>Использование данных
Корпорация Майкрософт использует шаблоны и данные анализа угроз, которые наблюдались у нескольких клиентов, чтобы улучшить возможности предотвращения и обнаружения. При этом соблюдаются обязательства по безопасности, описанные в [заявлении о конфиденциальности](https://privacy.microsoft.com/privacystatement).

## <a name="manage-data-collection-from-machines"></a>Управление сбором данных с компьютеров
При включении центра безопасности в Azure сбор данных включается для всех подписок Azure. Сбор данных можно также включить для подписок в Центре безопасности. Когда он включен, в Центре безопасности подготавливается агент Log Analytics на всех существующих и создаваемых поддерживаемых виртуальных машинах Azure.

Агент Log Analytics проверяет систему на наличие конфигураций, связанных с безопасностью, и передает их в формате событий в [компонент трассировки событий Windows](https://docs.microsoft.com/windows/win32/etw/event-tracing-portal) (ETW). Кроме того, операционная система вызывает события журнала событий во время работы виртуальной машины. Примеры таких данных — тип и версия операционной системы, журналы операционной системы (журналы событий Windows), выполняющиеся процессы, имя компьютера, IP-адреса, имя пользователя, выполнившего вход, и идентификатор клиента. Агент Log Analytics считывает записи журнала событий и данные трассировки ETW, а затем копирует их в рабочую область для анализа. Агент Log Analytics также поддерживает события создания процессов и аудит командной строки.

Если вы не используете Azure Defender, сбор данных с виртуальных машин можно также отключить в разделе "Политика безопасности". Сбор данных требуется для подписок, защищенных Azure Defender. Функция создания моментальных снимков диска виртуальной машины и сбора артефактов работает даже в том случае, если сбор данных был отключен.

Вы можете указать рабочую область и регион для хранения данных, собираемых с компьютеров. По умолчанию собираемые данные хранятся в ближайшей к вам рабочей области, как показано в следующей таблице.

| Геообъект виртуальной машины                                      | Геообъект рабочей области  |
|---------------------------------------------|----------------|
| США, Бразилия, ЮАР         | США  |
| Canada                                      | Canada         |
| Европа (за исключением Соединенного Королевства)           | Европа         |
| United Kingdom                              | United Kingdom |
| Азия (за исключением Индии, Японии, Кореи, Китая) | Азиатско-Тихоокеанский регион   |
| Корея                                       | Азиатско-Тихоокеанский регион   |
| Индия                                       | Индия          |
| Япония                                       | Япония          |
| Китай                                       | Китай          |
| Австралия                                   | Австралия      |
|                                             |                |

> [!NOTE]
> **Azure Defender для службы хранилища** хранит артефакты в том регионе, где размещен соответствующий ресурс Azure. Дополнительные сведения см. в статье [Общие сведения об Azure Defender для службы хранилища](defender-for-storage-introduction.md).


## <a name="data-consumption"></a>Использование данных

Клиенты могут получать данные Центра безопасности из следующих источников:


| STREAM                                                                                | Типы данных                                                                                                                                                                                                          |
|---------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [Журнал действий Azure](../azure-monitor/platform/activity-log.md)                       | Все оповещения системы безопасности, утвержденные [JIT-запросы](security-center-just-in-time.md) на доступ к Центру безопасности и все оповещения, созданные [адаптивными элементами управления приложениями](security-center-adaptive-application.md).|
| [Журналы Azure Monitor](../azure-monitor/platform/data-platform.md)                      | Все оповещения системы безопасности.                                                                                                                                                                                                |
| [Azure Resource Graph](../governance/resource-graph/overview.md)                      | Оповещения системы безопасности, рекомендации по обеспечению безопасности, результаты оценки уязвимостей, данные об оценке безопасности, состояние проверок соответствия и т. д.                                                                       |
| [REST API для Центра безопасности Azure](https://docs.microsoft.com/rest/api/securitycenter/) | Оповещения системы безопасности, рекомендации по обеспечению безопасности и т. д.                                                                                                                                                                |
|                                                                                       |                                                                                                                                                                                                                     |

## <a name="next-steps"></a>Дальнейшие действия

Из этой статьи вы узнали, каким образом обеспечивается управление данными и их защита в центре безопасности Azure. 

Дополнительные сведения о Центре безопасности Azure см. в статье [Что такое Центр безопасности Azure](security-center-introduction.md).
