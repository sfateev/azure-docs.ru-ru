---
title: Настройка диагностики
titleSuffix: Azure Digital Twins
description: См. раздел Включение ведения журнала с помощью параметров диагностики.
author: baanders
ms.author: baanders
ms.date: 7/28/2020
ms.topic: troubleshooting
ms.service: digital-twins
ms.openlocfilehash: f4abf78c153bd3d61068e4b7607794d6ccf1ed04
ms.sourcegitcommit: 2e72661f4853cd42bb4f0b2ded4271b22dc10a52
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/14/2020
ms.locfileid: "92047681"
---
# <a name="troubleshooting-azure-digital-twins-diagnostics-logging"></a>Устранение неполадок в Azure Digital двойников: ведение журнала диагностики

Azure Digital двойников собирает [метрики](troubleshoot-metrics.md) для экземпляра службы, которые предоставляют сведения о состоянии ресурсов. Эти метрики можно использовать для оценки общей работоспособности службы цифровых двойников Azure и подключенных к ней ресурсов. Эти статистические данные позволяют увидеть, что происходит с цифровым двойников Azure и помогает выполнять анализ основных причин проблем без необходимости обращения в службу поддержки Azure.

В этой статье показано, как включить **ведение журнала диагностики** для данных метрик из вашего экземпляра Azure Digital двойников. Эти журналы можно использовать для устранения неполадок, связанных со службами, и настройки параметров диагностики для отправки метрик Digital двойников в разные места назначения. Дополнительные сведения об этих параметрах см. в статье [*Создание параметров диагностики для отправки журналов платформы и метрик в разные места назначения*](../azure-monitor/platform/diagnostic-settings.md).

## <a name="turn-on-diagnostic-settings-with-the-azure-portal"></a>Включение параметров диагностики с помощью портал Azure

Вот как включить параметры диагностики для своего экземпляра Azure Digital двойников:

1. Войдите в [портал Azure](https://portal.azure.com) и перейдите к своему экземпляру Azure Digital двойников. Его можно найти, введя его имя на панели поиска портала. 

2. Выберите **параметры диагностики** в меню и **добавьте параметр диагностики**.

    :::image type="content" source="media/troubleshoot-diagnostics/diagnostic-settings.png" alt-text="Снимок экрана со страницей параметров диагностики и кнопкой &quot;Добавить&quot;":::

3. На следующей странице введите следующие значения:
     * **Имя параметра диагностики**: задайте имя для параметров диагностики.
     * **Сведения о категории**: выберите операции, которые требуется отслеживать, и установите флажки, чтобы включить диагностику для этих операций. Ниже перечислены операции, для которых доступны параметры диагностики:
        - дигиталтвинсоператион
        - евентраутесоператион
        - моделсоператион
        - Queryoperation класса DataService
        - AllMetrics
        
        Дополнительные сведения об этих параметрах см. в разделе [*сведения о категории*](#category-details) ниже.
     * **Сведения о назначении**: выберите, куда вы хотите отправить журналы. Можно выбрать любое сочетание трех параметров:
        - Отправка в Log Analytics
        - "Архивировать в учетной записи хранения";
        - "Передать в концентратор событий";

        Возможно, вам будет предложено ввести дополнительные сведения, если они необходимы для выбора назначения.  
    
4. Сохраните новые настройки. 

    :::image type="content" source="media/troubleshoot-diagnostics/diagnostic-settings-details.png" alt-text="Снимок экрана со страницей параметров диагностики и кнопкой &quot;Добавить&quot;":::

Новые параметры вступят в силу в течение 10 минут. После этого журналы отобразятся на настроенном целевом объекте на странице **параметры диагностики** для вашего экземпляра. 

## <a name="category-details"></a>Сведения о категории

Ниже приведены дополнительные сведения о категориях журналов, которые можно выбрать в разделе **сведения о категории** при настройке параметров диагностики.

| Категория журнала | Описание |
| --- | --- |
| адтмоделсоператион | Регистрировать все вызовы API, относящиеся к моделям |
| адткуерйоператион | Регистрировать все вызовы API, относящиеся к запросам |
| адтевентраутесоператион | Регистрация всех вызовов API, относящихся к маршрутам событий, а также выхода событий из Azure Digital двойников в службу конечной точки, например "Сетка событий", "концентраторы событий" и "служебная шина" |
| адтдигиталтвинсоператион | Регистрация всех вызовов API, относящихся к Azure Digital двойников |

Каждая категория журнала состоит из операций записи, чтения, удаления и действия.  Эти данные сопоставляются с REST API вызовами следующим образом:

| Тип события | REST API операции |
| --- | --- |
| запись | РАЗМЕЩЕНИЕ и исправление |
| Чтение | GET |
| DELETE | DELETE |
| Действие | POST |

Ниже приведен полный список операций и соответствующих [вызовов Azure Digital двойников REST API](/rest/api/azure-digitaltwins/) , зарегистрированных в каждой категории. 

>[!NOTE]
> Каждая категория журнала содержит несколько операций или вызовов REST API. В приведенной ниже таблице каждая категория журналов сопоставляется со всеми операциями и REST API находящиеся под ней вызовы до тех пор, пока не появится следующая категория журнала. 

| Категория журнала | Операция | REST APIные вызовы и другие события |
| --- | --- | --- |
| адтмоделсоператион | Microsoft. Дигиталтвинс/Models/Write | API обновления моделей цифровых двойника |
|  | Microsoft. Дигиталтвинс/Models/Read | Цифровые двойника модели Get по идентификаторам API-интерфейсам ID и List |
|  | Microsoft. Дигиталтвинс/Models/Delete | API удаления моделей Digital двойника |
|  | Microsoft. Дигиталтвинс/Models/Action | Добавление API цифровых двойника моделей |
| адткуерйоператион | Microsoft. Дигиталтвинс/запрос/действие | API двойников запросов |
| адтевентраутесоператион | Microsoft. Дигиталтвинс/евентраутес/запись | API добавления маршрутов событий |
|  | Microsoft. Дигиталтвинс/евентраутес/Read | Маршруты событий получают интерфейсы API ИДЕНТИФИКАТОРов и списков |
|  | Microsoft. Дигиталтвинс/евентраутес/Delete | API удаления маршрутов событий |
|  | Microsoft. Дигиталтвинс/евентраутес/действие | Сбой при попытке публикации событий в службе конечной точки (не вызове API) |
| адтдигиталтвинсоператион | Microsoft. Дигиталтвинс/дигиталтвинс/запись | Цифровой двойников Добавление, Добавление связи, обновление, обновление компонента |
|  | Microsoft. Дигиталтвинс/дигиталтвинс/Read | Digital двойников получает по ИДЕНТИФИКАТОРу, получение компонента, получение отношения по ИДЕНТИФИКАТОРу, список входящих отношений, список отношений |
|  | Microsoft. Дигиталтвинс/дигиталтвинс/Delete | Цифровые двойников удаление, удаление связи |
|  | Microsoft. Дигиталтвинс/дигиталтвинс/действие | Отправка данных телеметрии компонента Digital двойников, отправка данных телеметрии |

## <a name="log-schemas"></a>Схемы журналов 

Каждая категория журнала имеет схему, определяющую, как сообщается о событиях в этой категории. Каждая отдельная запись журнала хранится в виде текста и форматируется как большой двоичный объект JSON. Поля в журнале и примеры тела JSON приведены для каждого типа журнала ниже. 

`ADTDigitalTwinsOperation`, `ADTModelsOperation` и `ADTQueryOperation` используют последовательную схему журнала API; `ADTEventRoutesOperation` имеет собственную отдельную схему.

### <a name="api-log-schemas"></a>Схемы журналов API

Эта схема журнала является постоянной для `ADTDigitalTwinsOperation` , `ADTModelsOperation` и `ADTQueryOperation` . Он содержит сведения, относящиеся к вызовам API для экземпляра Digital двойников Azure.

Ниже приведены описания полей и свойств для журналов API.

| Имя поля | Тип данных | Описание |
|-----|------|-------------|
| `Time` | Дата и время | Дата и время возникновения этого события в формате UTC |
| `ResourceID` | Строка | Идентификатор ресурса Azure Resource Manager для ресурса, в котором произошло событие |
| `OperationName` | Строка  | Тип действия, выполняемого во время события |
| `OperationVersion` | Строка | Версия API, использованная во время события |
| `Category` | Строка | Тип выдаваемый ресурса |
| `ResultType` | Строка | Результат события |
| `ResultSignature` | Строка | Код состояния HTTP для события |
| `ResultDescription` | Строка | Дополнительные сведения о событии |
| `DurationMs` | Строка | Время, затраченное на выполнение события в миллисекундах |
| `CallerIpAddress` | Строка | Маскированный исходный IP-адрес для события |
| `CorrelationId` | Guid | Клиент предоставил уникальный идентификатор для события |
| `Level` | Строка | Серьезность ведения журнала события |
| `Location` | Строка | Регион, в котором произошло событие |
| `RequestUri` | URI | Конечная точка, используемая во время события |

Ниже приведены примеры фрагментов JSON для этих типов журналов.

#### <a name="adtdigitaltwinsoperation"></a>адтдигиталтвинсоператион

```json
{
  "time": "2020-03-14T21:11:14.9918922Z",
  "resourceId": "/SUBSCRIPTIONS/BBED119E-28B8-454D-B25E-C990C9430C8F/RESOURCEGROUPS/MYRESOURCEGROUP/PROVIDERS/MICROSOFT.DIGITALTWINS/DIGITALTWINSINSTANCES/MYINSTANCENAME",
  "operationName": "Microsoft.DigitalTwins/digitaltwins/write",
  "operationVersion": "2020-05-31-preview",
  "category": "DigitalTwinOperation",
  "resultType": "Success",
  "resultSignature": "200",
  "resultDescription": "",
  "durationMs": "314",
  "callerIpAddress": "13.68.244.*",
  "correlationId": "2f6a8e64-94aa-492a-bc31-16b9f0b16ab3",
  "level": "4",
  "location": "southcentralus",
  "uri": "https://myinstancename.api.scus.digitaltwins.azure.net/digitaltwins/factory-58d81613-2e54-4faa-a930-d980e6e2a884?api-version=2020-05-31-preview"
}
```

#### <a name="adtmodelsoperation"></a>адтмоделсоператион

```json
{
  "time": "2020-10-29T21:12:24.2337302Z",
  "resourceId": "/SUBSCRIPTIONS/BBED119E-28B8-454D-B25E-C990C9430C8F/RESOURCEGROUPS/MYRESOURCEGROUP/PROVIDERS/MICROSOFT.DIGITALTWINS/DIGITALTWINSINSTANCES/MYINSTANCENAME",
  "operationName": "Microsoft.DigitalTwins/models/write",
  "operationVersion": "2020-05-31-preview",
  "category": "ModelsOperation",
  "resultType": "Success",
  "resultSignature": "201",
  "resultDescription": "",
  "durationMs": "935",
  "callerIpAddress": "13.68.244.*",
  "correlationId": "9dcb71ea-bb6f-46f2-ab70-78b80db76882",
  "level": "4",
  "location": "southcentralus",
  "uri": "https://myinstancename.api.scus.digitaltwins.azure.net/Models?api-version=2020-05-31-preview",
}
```

#### <a name="adtqueryoperation"></a>адткуерйоператион

```json
{
  "time": "2020-12-04T21:11:44.1690031Z",
  "resourceId": "/SUBSCRIPTIONS/BBED119E-28B8-454D-B25E-C990C9430C8F/RESOURCEGROUPS/MYRESOURCEGROUP/PROVIDERS/MICROSOFT.DIGITALTWINS/DIGITALTWINSINSTANCES/MYINSTANCENAME",
  "operationName": "Microsoft.DigitalTwins/query/action",
  "operationVersion": "2020-05-31-preview",
  "category": "QueryOperation",
  "resultType": "Success",
  "resultSignature": "200",
  "resultDescription": "",
  "durationMs": "255",
  "callerIpAddress": "13.68.244.*",
  "correlationId": "1ee2b6e9-3af4-4873-8c7c-1a698b9ac334",
  "level": "4",
  "location": "southcentralus",
  "uri": "https://myinstancename.api.scus.digitaltwins.azure.net/query?api-version=2020-05-31-preview",
}
```

### <a name="egress-log-schemas"></a>Схемы исходящего журнала

Это схема для `ADTEventRoutesOperation` журналов. Они содержат подробные сведения об исключениях и операциях API, связанных с конечными точками исходящего трафика, подключенными к экземпляру Digital двойников Azure.

|Имя поля | Тип данных | Описание |
|-----|------|-------------|
| `Time` | Дата и время | Дата и время возникновения этого события в формате UTC |
| `ResourceId` | Строка | Идентификатор ресурса Azure Resource Manager для ресурса, в котором произошло событие |
| `OperationName` | Строка  | Тип действия, выполняемого во время события |
| `Category` | Строка | Тип выдаваемый ресурса |
| `ResultDescription` | Строка | Дополнительные сведения о событии |
| `Level` | Строка | Серьезность ведения журнала события |
| `Location` | Строка | Регион, в котором произошло событие |
| `EndpointName` | Строка | Имя конечной точки исходящего трафика, созданной в Azure Digital двойников |

Ниже приведены примеры фрагментов JSON для этих типов журналов.

#### <a name="adteventroutesoperation"></a>адтевентраутесоператион

```json
{
  "time": "2020-11-05T22:18:38.0708705Z",
  "resourceId": "/SUBSCRIPTIONS/BBED119E-28B8-454D-B25E-C990C9430C8F/RESOURCEGROUPS/MYRESOURCEGROUP/PROVIDERS/MICROSOFT.DIGITALTWINS/DIGITALTWINSINSTANCES/MYINSTANCENAME",
  "operationName": "Microsoft.DigitalTwins/eventroutes/action",
  "category": "EventRoutesOperation",
  "resultDescription": "Unable to send EventGrid message to [my-event-grid.westus-1.eventgrid.azure.net] for event Id [f6f45831-55d0-408b-8366-058e81ca6089].",
  "correlationId": "7f73ab45-14c0-491f-a834-0827dbbf7f8e",
  "level": "3",
  "location": "southcentralus",
  "properties": {
    "endpointName": "endpointEventGridInvalidKey"
  }
}
```

## <a name="next-steps"></a>Дальнейшие действия

* Дополнительные сведения о настройке диагностики см. в статье [*Получение и использование данных журнала из ресурсов Azure*](../azure-monitor/platform/platform-logs-overview.md).
* Сведения о метриках цифровых двойников Azure см. в разделе [*Устранение неполадок: Просмотр метрик с помощью Azure Monitor*](troubleshoot-metrics.md).
* Сведения о том, как включить оповещения для метрик, см. в разделе [*Устранение неполадок: Настройка оповещений*](troubleshoot-alerts.md).