---
title: Мониторинг хранилища таблиц Azure | Документация Майкрософт
description: Узнайте, как отслеживать производительность и доступность хранилища таблиц Azure. Мониторинг данных хранилища таблиц Azure, изучение конфигурации и анализ данных метрик и журналов.
author: normesta
services: storage
ms.service: storage
ms.topic: conceptual
ms.date: 10/02/2020
ms.author: normesta
ms.reviewer: fryu
ms.custom: monitoring, devx-track-csharp
ms.openlocfilehash: 8104d1d1f8864f8b7c5a6add6c602007f2d04822
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2020
ms.locfileid: "91711512"
---
# <a name="monitoring-azure-table-storage"></a>Мониторинг табличного хранилища Azure

При наличии критически важных приложений и бизнес-процессов, использующих ресурсы Azure, необходимо отслеживать эти ресурсы на предмет их доступности, производительности и функционирования. В этой статье описываются данные мониторинга, созданные хранилищем таблиц Azure, и способы использования функций Azure Monitor для анализа оповещений об этих данных.

> [!NOTE]
> Журналы службы хранилища Azure в Azure Monitor предоставляются в общедоступной предварительной версии. Они также доступны для предварительного тестирования во всех регионах общедоступного облака. Чтобы зарегистрироваться для использования предварительной версии, см. [эту страницу](https://forms.microsoft.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbRxW65f1VQyNCuBHMIMBV8qlUM0E0MFdPRFpOVTRYVklDSE1WUTcyTVAwOC4u). Эта предварительная версия включает журналы для больших двоичных объектов (в том числе Azure Data Lake Storage 2-го поколения), файлов, очередей и таблиц. Эта функция доступна для всех учетных записей хранения, созданных с помощью модели развертывания Azure Resource Manager. См. раздел [Общие сведения об учетной записи хранения](../common/storage-account-overview.md).

## <a name="monitor-overview"></a>Общие сведения о мониторинге

На странице **Обзор** в портал Azure для каждого ресурса хранилища таблиц содержится краткое представление об использовании ресурсов, таких как запросы и почасовая оплата. Эти сведения полезны, но лишь небольшое количество данных мониторинга доступно. Некоторые из этих данных собираются автоматически и доступны для анализа сразу после создания ресурса. Вы можете включить дополнительные типы сбора данных с помощью определенной конфигурации.

## <a name="what-is-azure-monitor"></a>Общие сведения об Azure Monitor
Хранилище таблиц Azure создает данные мониторинга с помощью [Azure Monitor](../../azure-monitor/overview.md), которая представляет собой полную службу мониторинга стека в Azure. Azure Monitor предоставляет полный набор функций для мониторинга ресурсов Azure и ресурсов в других облаках, а также в локальной среде. 

Начните с статьи [мониторинг ресурсов Azure с помощью Azure Monitor](../../azure-monitor/insights/monitor-azure-resource.md) , который описывает следующее:

- Общие сведения об Azure Monitor
- затраты, связанные с мониторингом;
- данные мониторинга, собираемые в Azure;
- настройка сбора данных;
- стандартные средства Azure для анализа данных мониторинга и оповещения.

В следующих разделах этой статьи описываются конкретные данные, собранные из службы хранилища Azure. В примерах показано, как настроить сбор данных и анализировать эти данные с помощью средств Azure.

## <a name="monitoring-data"></a>Данные мониторинга

Хранилище таблиц Azure собирает те же данные мониторинга, что и другие ресурсы Azure, которые описаны в разделе [мониторинг данных из ресурсов Azure](../../azure-monitor/insights/monitor-azure-resource.md#monitoring-data). 

Подробные сведения о метриках и журналах, созданных хранилищем таблиц Azure, см. в [справочнике по данным мониторинга хранилища таблиц Azure](monitor-table-storage-reference.md) .

Метрики и журналы в Azure Monitor поддерживают только учетные записи хранения Azure Resource Manager. Azure Monitor не поддерживает классические учетные записи хранения. Если вы хотите использовать метрики или журналы в классической учетной записи хранения, необходимо выполнить миграцию в учетную запись хранения Azure Resource Manager. См. статью [Поддерживаемый платформой перенос ресурсов IaaS из классической модели в модель Azure Resource Manager](https://docs.microsoft.com/azure/virtual-machines/windows/migration-classic-resource-manager-overview).

При необходимости вы можете продолжить использование классических метрик и журналов. На самом деле, классические метрики и журналы доступны параллельно с метриками и журналами в Azure Monitor. Предусмотрена та же поддержка, пока в службе хранилища Azure обслуживаются устаревшие метрики и журналы.

## <a name="collection-and-routing"></a>Сбор и маршрутизация

Метрики платформы и журнал действий собираются автоматически, но их можно направить в другие расположения с помощью параметра диагностики. Необходимо создать параметр диагностики для сбор журналов ресурсов. 

Сведения о создании параметра диагностики с помощью портал Azure, Azure CLI или PowerShell см. в статье [Создание параметров диагностики для сбора журналов и метрик платформы в Azure](../../azure-monitor/platform/diagnostic-settings.md). 

Чтобы просмотреть шаблон Azure Resource Manager, который создает параметр диагностики, см. раздел [параметр диагностики для службы хранилища Azure](https://docs.microsoft.com/azure/azure-monitor/samples/resource-manager-diagnostic-settings#diagnostic-setting-for-azure-storage).

При создании параметра диагностики выберите тип хранилища, для которого необходимо включить журналы, например большой двоичный объект, очередь, таблица или файл. Для табличного хранилища выберите **Таблица**. 

При создании параметра диагностики на портале Azure можно выбрать ресурс из списка. При использовании PowerShell или Azure CLI необходимо использовать идентификатор ресурса конечной точки хранилища таблиц. ИД ресурса можно найти на портале Azure, открыв страницу **свойств** учетной записи хранения.

Также необходимо указать одну из следующих категорий операций, для которых требуется вести журнал. 

| Категория | Описание |
|:---|:---|
| StorageRead | Операции чтения с объектами. |
| StorageWrite | Операции записи в объекты. |
| StorageDelete | Удаление операций с объектами. |

## <a name="analyzing-metrics"></a>Анализ метрик

Вы можете анализировать метрики для службы хранилища Azure с помощью метрик из других служб Azure, используя обозреватель метрик. Откройте обозреватель метрик, выбрав **Метрики** в меню **Azure Monitor**. Подробные сведения об использовании этого средства см. на странице [Начало работы с обозревателем метрик Azure](../../azure-monitor/platform/metrics-getting-started.md). 

В этом примере показано, как просмотреть **транзакции** на уровне учетной записи.

![Снимок экрана осуществления доступа к метрикам на портале Azure](./media/monitor-table-storage/access-metrics-portal.png)

Для метрик с поддержкой измерений можно выполнить фильтрацию по нужному значению измерения. В этом примере объясняется, как просмотреть **транзакции** на уровне учетной записи для определенной операции, выбрав значения для измерения **Имя API**.

![Снимок экрана осуществления доступа к метрикам с поддержкой измерений на портале Azure](./media/monitor-table-storage/access-metrics-portal-with-dimension.png)

Полный список измерений, поддерживаемых службой хранилища Azure, см. в разделе [Измерения метрик](monitor-table-storage-reference.md#metrics-dimensions).

Метрики для хранилища таблиц Azure находятся в следующих пространствах имен: 

- Microsoft.Storage/storageAccounts
- Microsoft.Storage/storageAccounts/tableServices

Список всех метрик поддержки Azure Monitor, включая хранилище таблиц Azure, см. в статье [Azure Monitor поддерживаемые метрики](https://docs.microsoft.com/azure/azure-monitor/platform/metrics-supported).


### <a name="accessing-metrics"></a>Доступ к метрикам

> [!TIP]
> Чтобы просмотреть примеры Azure CLI или .NET, выберите соответствующую вкладку ниже.

### <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

#### <a name="list-the-metric-definition"></a>Отображение определения метрики

Вы можете составить список определений метрик учетной записи хранения или службы хранилища таблиц. Используйте командлет [Get-AzMetricDefinition](https://docs.microsoft.com/powershell/module/az.monitor/get-azmetricdefinition).

В этом примере замените `<resource-ID>` заполнитель идентификатором ресурса всей учетной записи хранения или идентификатором ресурса службы хранилища таблиц.  Эти ИД ресурса можно найти на странице **свойств** учетной записи хранения на портале Azure.

```powershell
   $resourceId = "<resource-ID>"
   Get-AzMetricDefinition -ResourceId $resourceId
```

#### <a name="reading-metric-values"></a>Считывание значений метрик

Вы можете читать значения метрик уровня учетной записи хранения или службы хранилища таблиц. Используйте командлет [Get-AzMetric](https://docs.microsoft.com/powershell/module/Az.Monitor/Get-AzMetric).

```powershell
   $resourceId = "<resource-ID>"
   Get-AzMetric -ResourceId $resourceId -MetricNames "UsedCapacity" -TimeGrain 01:00:00
```

### <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

#### <a name="list-the-account-level-metric-definition"></a>Отображение определения метрик на уровне учетной записи

Вы можете составить список определений метрик учетной записи хранения или службы хранилища таблиц. Используйте команду [az monitor metrics list-definitions](https://docs.microsoft.com/cli/azure/monitor/metrics#az-monitor-metrics-list-definitions).
 
В этом примере замените `<resource-ID>` заполнитель идентификатором ресурса всей учетной записи хранения или идентификатором ресурса службы хранилища таблиц. Эти ИД ресурса можно найти на странице **свойств** учетной записи хранения на портале Azure.

```azurecli-interactive
   az monitor metrics list-definitions --resource <resource-ID>
```

#### <a name="read-account-level-metric-values"></a>Считывание значений метрик на уровне учетной записи

Вы можете прочитать значения метрик вашей учетной записи хранения или службы хранилища таблиц. Используйте команду [az monitor metrics list](https://docs.microsoft.com/cli/azure/monitor/metrics#az-monitor-metrics-list).

```azurecli-interactive
   az monitor metrics list --resource <resource-ID> --metric "UsedCapacity" --interval PT1H
```

### <a name="net"></a>[.NET](#tab/dotnet)

Azure Monitor предоставляет [пакет SDK для .NET](https://www.nuget.org/packages/Microsoft.Azure.Management.Monitor/) для считывания определения и значений метрик. [Пример кода](https://azure.microsoft.com/resources/samples/monitor-dotnet-metrics-api/) демонстрирует использование пакета SDK с различными параметрами. Необходимо использовать `0.18.0-preview` или более позднюю версию для метрик хранилища.
 
В этих примерах замените `<resource-ID>` заполнитель идентификатором ресурса всей учетной записи хранения или службы хранилища таблиц. Эти ИД ресурса можно найти на странице **свойств** учетной записи хранения на портале Azure.

Замените переменную `<subscription-ID>` на ИД своей подписки. Инструкции по получению значений для `<tenant-ID>`, `<application-ID>` и `<AccessKey>` см. на странице [Создание приложения Azure Active Directory и субъекта-службы с доступом к ресурсам с помощью портала](https://azure.microsoft.com/documentation/articles/resource-group-create-service-principal-portal/). 

#### <a name="list-the-account-level-metric-definition"></a>Отображение определения метрик на уровне учетной записи

В приведенном ниже примере показано, как получить список определений метрик на уровне учетной записи.

```csharp
    public static async Task ListStorageMetricDefinition()
    {
        var resourceId = "<resource-ID>";
        var subscriptionId = "<subscription-ID>";
        var tenantId = "<tenant-ID>";
        var applicationId = "<application-ID>";
        var accessKey = "<AccessKey>";


        MonitorManagementClient readOnlyClient = AuthenticateWithReadOnlyClient(tenantId, applicationId, accessKey, subscriptionId).Result;
        IEnumerable<MetricDefinition> metricDefinitions = await readOnlyClient.MetricDefinitions.ListAsync(resourceUri: resourceId, cancellationToken: new CancellationToken());

        foreach (var metricDefinition in metricDefinitions)
        {
            // Enumrate metric definition:
            //    Id
            //    ResourceId
            //    Name
            //    Unit
            //    MetricAvailabilities
            //    PrimaryAggregationType
            //    Dimensions
            //    IsDimensionRequired
        }
    }

```

#### <a name="reading-account-level-metric-values"></a>Чтение значений метрик уровня учетной записи

В приведенном ниже примере показано, как считать данные `UsedCapacity` на уровне учетной записи.

```csharp
    public static async Task ReadStorageMetricValue()
    {
        var resourceId = "<resource-ID>";
        var subscriptionId = "<subscription-ID>";
        var tenantId = "<tenant-ID>";
        var applicationId = "<application-ID>";
        var accessKey = "<AccessKey>";

        MonitorClient readOnlyClient = AuthenticateWithReadOnlyClient(tenantId, applicationId, accessKey, subscriptionId).Result;

        Microsoft.Azure.Management.Monitor.Models.Response Response;

        string startDate = DateTime.Now.AddHours(-3).ToUniversalTime().ToString("o");
        string endDate = DateTime.Now.ToUniversalTime().ToString("o");
        string timeSpan = startDate + "/" + endDate;

        Response = await readOnlyClient.Metrics.ListAsync(
            resourceUri: resourceId,
            timespan: timeSpan,
            interval: System.TimeSpan.FromHours(1),
            metricnames: "UsedCapacity",

            aggregation: "Average",
            resultType: ResultType.Data,
            cancellationToken: CancellationToken.None);

        foreach (var metric in Response.Value)
        {
            // Enumrate metric value
            //    Id
            //    Name
            //    Type
            //    Unit
            //    Timeseries
            //        - Data
            //        - Metadatavalues
        }
    }

```

#### <a name="reading-multidimensional-metric-values"></a>Считывание значений многомерных метрик

Для многомерных метрик необходимо определить фильтры по метаданным, если требуется считать данные метрик для конкретных значений измерений.

В приведенном ниже примере показано, как считать данные в метрике, поддерживающей многомерные значения.

```csharp
    public static async Task ReadStorageMetricValueTest()
    {
        // Resource ID for table storage
        var resourceId = "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Storage/storageAccounts/{storageAccountName}/tableServices/default";
        var subscriptionId = "<subscription-ID}";
        // How to identify Tenant ID, Application ID and Access Key: https://azure.microsoft.com/documentation/articles/resource-group-create-service-principal-portal/
        var tenantId = "<tenant-ID>";
        var applicationId = "<application-ID>";
        var accessKey = "<AccessKey>";

        MonitorManagementClient readOnlyClient = AuthenticateWithReadOnlyClient(tenantId, applicationId, accessKey, subscriptionId).Result;

        Microsoft.Azure.Management.Monitor.Models.Response Response;

        string startDate = DateTime.Now.AddHours(-3).ToUniversalTime().ToString("o");
        string endDate = DateTime.Now.ToUniversalTime().ToString("o");
        string timeSpan = startDate + "/" + endDate;
        // It's applicable to define meta data filter when a metric support dimension
        // More conditions can be added with the 'or' and 'and' operators, example: BlobType eq 'BlockBlob' or BlobType eq 'PageBlob'
        ODataQuery<MetadataValue> odataFilterMetrics = new ODataQuery<MetadataValue>(
            string.Format("BlobType eq '{0}'", "BlockBlob"));

        Response = readOnlyClient.Metrics.List(
                        resourceUri: resourceId,
                        timespan: timeSpan,
                        interval: System.TimeSpan.FromHours(1),
                        metricnames: "BlobCapacity",
                        odataQuery: odataFilterMetrics,
                        aggregation: "Average",
                        resultType: ResultType.Data);

        foreach (var metric in Response.Value)
        {
            //Enumrate metric value
            //    Id
            //    Name
            //    Type
            //    Unit
            //    Timeseries
            //        - Data
            //        - Metadatavalues
        }
    }

```

---

## <a name="analyzing-logs"></a>анализ журналов;

Доступ к журналам ресурсов можно получить с помощью большого двоичного объекта в учетной записи хранения, данных событий или запросов Log Analytic.

Подробные справочные сведения о полях, которые отображаются в этих журналах, см. в статье [Справочник по данным мониторинга хранилища таблиц Azure](monitor-table-storage-reference.md).

> [!NOTE]
> Журналы службы хранилища Azure в Azure Monitor предоставляются в общедоступной предварительной версии. Они также доступны для предварительного тестирования во всех регионах общедоступного облака. Чтобы зарегистрироваться для использования предварительной версии, см. [эту страницу](https://forms.microsoft.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbRxW65f1VQyNCuBHMIMBV8qlUM0E0MFdPRFpOVTRYVklDSE1WUTcyTVAwOC4u). Эта предварительная версия включает журналы для больших двоичных объектов (в том числе Azure Data Lake Storage 2-го поколения), файлов, очередей, таблиц, учетных записей хранения класса Premium общего назначения версии 1 и учетных записей хранения общего назначения версии 2. Классические учетные записи хранения не поддерживаются.

Записи журнала создаются только при получении запроса к конечной точке службы. Например, если учетная запись хранения имеет действие в конечной точке таблицы, но не находится в конечных точках BLOB или очереди, то создаются только журналы, относящиеся к службе таблиц. Журналы службы хранилища Azure содержат подробные сведения об успешных и неудачных запросах к службе хранилища. Эта информация может использоваться для мониторинга отдельных запросов и диагностики неполадок в службе хранилища. Запросы вносятся в журнал наилучшим возможным образом.

### <a name="log-authenticated-requests"></a>Ведение журналов запросов, прошедших аутентификацию

 Регистрируются запросы, прошедшие аутентификацию, следующих типов:

- Успешные запросы.
- Неудачные запросы, в том числе из-за ошибок, связанных с временем ожидания, регулированием, сетью, авторизацией и др.
- Запросы, в которых используется подписанный URL-адрес (SAS) или OAuth, в том числе неудачные и успешные запросы.
- Запросы к данным аналитики (классические данные журнала в контейнере **$logs** и данные метрик класса в таблицах **$metric**).

Запросы, выполняемые самой службой хранилища таблиц, например создание или удаление журнала, не регистрируются. Полный список регистрируемых данных приведен на страницах об [операциях с протоколированием и сообщениях о состоянии службы хранилища](/rest/api/storageservices/storage-analytics-logged-operations-and-status-messages) и [формате журналов службы хранилища](monitor-table-storage-reference.md).

### <a name="log-anonymous-requests"></a>Ведение журналов анонимных запросов

 Регистрируются анонимные запросы следующих типов:

- Успешные запросы.
- ошибки сервера;
- Ошибки времени ожидания для клиента и сервера.
- Неудачные запросы GET с кодом ошибки 304 (не изменено).

Остальные неудачные анонимные запросы не регистрируются. Полный список регистрируемых данных приведен на страницах об [операциях с протоколированием и сообщениях о состоянии службы хранилища](/rest/api/storageservices/storage-analytics-logged-operations-and-status-messages) и [формате журналов службы хранилища](monitor-table-storage-reference.md).

### <a name="accessing-logs-in-a-storage-account"></a>Доступ к журналам в учетной записи хранения

Журналы отображаются как большие двоичные объекты, хранящиеся в контейнере в целевой учетной записи хранения. Данные собираются и хранятся внутри одного большого двоичного объекта в качестве полезных данных JSON с разделителем-строкой. Имя большого двоичного объекта соответствует следующему соглашению об именовании.

`https://<destination-storage-account>.blob.core.windows.net/insights-logs-<storage-operation>/resourceId=/subscriptions/<subscription-ID>/resourceGroups/<resource-group-name>/providers/Microsoft.Storage/storageAccounts/<source-storage-account>/tableServices/default/y=<year>/m=<month>/d=<day>/h=<hour>/m=<minute>/PT1H.json`

Ниже приведен пример:

`https://mylogstorageaccount.blob.core.windows.net/insights-logs-storagewrite/resourceId=/subscriptions/`<br>`208841be-a4v3-4234-9450-08b90c09f4/resourceGroups/myresourcegroup/providers/Microsoft.Storage/storageAccounts/mystorageaccount/tableServices/default/y=2019/m=07/d=30/h=23/m=12/PT1H.json`

### <a name="accessing-logs-in-an-event-hub"></a>Доступ к журналам в концентраторе событий

Журналы, отправленные в концентратор событий, не сохраняются в виде файла, однако вы можете проверить, получил ли концентратор событий данные журнала. На портале Azure перейдите в концентратор событий и убедитесь, что количество **входящих сообщений** больше нуля. 

![Журналы аудита](media/monitor-table-storage/event-hub-log.png)

Вы можете получать доступ к данным журнала, отправляемым в концентратор событий, и считывать их, используя сведения о безопасности, а также средства мониторинга и управления событиями. Дополнительные сведения см. на [этой странице](https://docs.microsoft.com/azure/azure-monitor/platform/stream-monitoring-data-event-hubs#what-can-i-do-with-the-monitoring-data-being-sent-to-my-event-hub).

### <a name="accessing-logs-in-a-log-analytics-workspace"></a>Доступ к журналам в рабочей области Log Analytics

Доступ к журналам, отправляемым в рабочую область Log Analytics, можно получить с помощью запросов журналов Azure Monitor.

Дополнительные сведения см. в статье [Начало работы с Log Analytics в Azure Monitor](https://docs.microsoft.com/azure/azure-monitor/log-query/get-started-portal).

Данные хранятся в таблице **сторажетаблелогс** . 

#### <a name="sample-kusto-queries"></a>Примеры запросов Kusto

Ниже приведены некоторые запросы, которые можно ввести на панели **поиска журналов** , чтобы упростить мониторинг хранилища таблиц. Эти запросы поддерживают [новый язык](https://docs.microsoft.com/azure/azure-monitor/log-query/log-query-overview).

> [!IMPORTANT]
> При выборе **журналов** в меню группы ресурсов учетной записи хранения log Analytics открывается с областью запроса, заданной для текущей группы ресурсов. Это означает, что запросы журнала будут содержать только данные из этой группы ресурсов. Если требуется выполнить запрос, который включает данные из других ресурсов или данных из других служб Azure, выберите **журналы** в меню **Azure Monitor** . Подробные сведения см. в статье [Область запросов журнала и временной диапазон в Azure Monitor Log Analytics](/azure/azure-monitor/log-query/scope/).

Используйте следующие запросы для мониторинга учетных записей хранения Azure:

* Для отображения 10 наиболее распространенных ошибок за последние три дня.

    ```Kusto
    StorageTableLogs
    | where TimeGenerated > ago(3d) and StatusText !contains "Success"
    | summarize count() by StatusText
    | top 10 by count_ desc
    ```
* Для отображения основных 10 операций, вызвавших наибольшее количество ошибок за последние три дня.

    ```Kusto
    StorageTableLogs
    | where TimeGenerated > ago(3d) and StatusText !contains "Success"
    | summarize count() by OperationName
    | top 10 by count_ desc
    ```
* Для отображения основных 10 операций с наибольшей сквозной задержкой за последние три дня.

    ```Kusto
    StorageTableLogs
    | where TimeGenerated > ago(3d)
    | top 10 by DurationMs desc
    | project TimeGenerated, OperationName, DurationMs, ServerLatencyMs, ClientLatencyMs = DurationMs - ServerLatencyMs
    ```
* Для отображения всех операций, вызвавших ошибки регулирования на стороне сервера за последние три дня.

    ```Kusto
    StorageTableLogs
    | where TimeGenerated > ago(3d) and StatusText contains "ServerBusy"
    | project TimeGenerated, OperationName, StatusCode, StatusText
    ```
* Для отображения всех запросов с анонимным доступом за последние три дня.

    ```Kusto
    StorageTableLogs
    | where TimeGenerated > ago(3d) and AuthenticationType == "Anonymous"
    | project TimeGenerated, OperationName, AuthenticationType, Uri
    ```
* Для создания круговой диаграммы операций, используемых за последние три дня.
    ```Kusto
    StorageTableLogs
    | where TimeGenerated > ago(3d)
    | summarize count() by OperationName
    | sort by count_ desc 
    | render piechart
    ```
## <a name="faq"></a>ВОПРОСЫ И ОТВЕТЫ

**Поддерживает ли служба хранилища Microsoft Azure метрики для управляемых и неуправляемых дисков?**

Нет. Служба "Вычисления Azure" поддерживает метрики на дисках. Дополнительные сведения см. на странице [Дисковые метрики для управляемых и неуправляемых дисков](https://azure.microsoft.com/blog/per-disk-metrics-managed-disks/).

## <a name="next-steps"></a>Дальнейшие действия

- Справочные сведения о журналах и метриках, созданных хранилищем таблиц Azure, см. в статье [Справочник по данным мониторинга хранилища таблиц Azure](monitor-table-storage-reference.md).
- Подробные сведения о мониторинге ресурсов Azure см. на странице [Мониторинг ресурсов Azure с помощью Azure Monitor](../../azure-monitor/insights/monitor-azure-resource.md).
- Дополнительные сведения о миграции метрик см. на [этой странице](../common/storage-metrics-migration.md).
