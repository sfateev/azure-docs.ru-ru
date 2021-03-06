---
title: Ведение журнала Аналитики Службы хранилища Azure
description: Используйте Аналитика Службы хранилища, чтобы заносить в журнал сведения о запросах к службе хранилища Azure. Узнайте, какие запросы заносятся в журнал, как хранятся журналы, как включить ведение журнала хранилища и многое другое.
author: normesta
ms.service: storage
ms.subservice: common
ms.topic: conceptual
ms.date: 07/23/2020
ms.author: normesta
ms.reviewer: fryu
ms.custom: monitoring, devx-track-csharp
ms.openlocfilehash: 5b4e2fa95b9a5eebf393d7c64feecd3997b7ecfd
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2020
ms.locfileid: "91280034"
---
# <a name="azure-storage-analytics-logging"></a>Ведение журнала Аналитики Службы хранилища Azure

В аналитике хранилища регистрируется подробная информация об успешных и неудачных запросах к службе хранилища. Эта информация может использоваться для мониторинга отдельных запросов и диагностики неполадок в службе хранилища. Запросы вносятся в журнал наилучшим возможным образом.

 Ведение журнала "Аналитики Службы хранилища" по умолчанию для учетной записи хранения не предусмотрено. Это можно сделать на [портале Azure](https://portal.azure.com/). Дополнительные сведения см. в статье [Мониторинг учетной записи хранения на портале Azure](/azure/storage/storage-monitor-storage-account). Аналитику хранилища также можно включить программно через REST API или клиентскую библиотеку. Чтобы включить Аналитику Службы хранилища для каждой службы, можно использовать операции [Получить свойства службы BLOB-объектов](https://docs.microsoft.com/rest/api/storageservices/Blob-Service-REST-API), [Получить свойства службы очередей](https://docs.microsoft.com/rest/api/storageservices/Get-Queue-Service-Properties) [Получить свойства службы таблиц](https://docs.microsoft.com/rest/api/storageservices/Get-Table-Service-Properties).

 Записи журнала создаются только при получении запроса к конечной точке службы. Например, если обнаруживается действие учетной записи хранения в конечной точке службы BLOB-объектов, но не в ее конечных точках служб таблиц или очередей, то будут созданы журналы, которые относятся только к службе BLOB-объектов.

> [!NOTE]
>  Ведение журнала Аналитики Службы хранилища доступно только для служб BLOB-объектов, очередей и таблиц. Ведение журнала Аналитики Службы хранилища также доступно для высокопроизводительных учетных записей [BlockBlobStorage](../blobs/storage-blob-create-account-block-blob.md). Однако оно недоступно для высокопроизводительных учетных записей общего назначения версии 2.

## <a name="requests-logged-in-logging"></a>Регистрация запросов
### <a name="logging-authenticated-requests"></a>Ведение журналов запросов, прошедших аутентификацию

 Регистрируются запросы, прошедшие аутентификацию, следующих типов:

- Успешные запросы.
- Неудачные запросы, в том числе из-за ошибок, связанных со временем ожидания, регулированием, сетью, авторизацией и т. п.
- Запросы, в которых используется подписанный URL-адрес (SAS) или OAuth, в том числе неудачные и успешные запросы.
- Запросы к данным аналитики.

  Запросы, выполненные в самой аналитике хранилища, такие как запросы на создание или удаление журнала, не регистрируются. Полный список регистрируемых данных приведен в разделах [Операции с протоколированием и сообщения о состоянии аналитики хранилища](/rest/api/storageservices/storage-analytics-logged-operations-and-status-messages) и [Формат журналов аналитики хранилища](/rest/api/storageservices/storage-analytics-log-format).

### <a name="logging-anonymous-requests"></a>Ведение журналов анонимных запросов

 Регистрируются анонимные запросы следующих типов:

- Успешные запросы.
- ошибки сервера;
- Ошибки времени ожидания для клиента и сервера.
- Неудачные запросы GET с кодом ошибки 304 (не изменено).

  Остальные неудачные анонимные запросы не регистрируются. Полный список регистрируемых данных приведен в разделах [Операции с протоколированием и сообщения о состоянии аналитики хранилища](/rest/api/storageservices/storage-analytics-logged-operations-and-status-messages) и [Формат журналов аналитики хранилища](/rest/api/storageservices/storage-analytics-log-format).

## <a name="how-logs-are-stored"></a>Хранение журналов

Все журналы хранятся в блочных BLOB-объектах в контейнере с именем `$logs`, который автоматически создается при включении Аналитики Службы хранилища для учетной записи хранилища. Контейнер `$logs` находится в пространстве имен BLOB-объекта учетной записи хранения, например: `http://<accountname>.blob.core.windows.net/$logs`. После включения аналитики хранилища этот контейнер нельзя удалить, но вы можете удалить его содержимое. Если вы используете средство просмотра хранилища для непосредственного перехода к контейнеру, вы увидите все BLOB-объекты, содержащие данные журнала.

> [!NOTE]
>  Контейнер `$logs` не отображается при выполнении операции перечисления контейнеров, например с использованием операции перечисления контейнеров. Доступ к нему можно получить только непосредственно. Например, для доступа к BLOB-объектам в контейнере `$logs` можно использовать операцию перечисления BLOB-объектов.

По мере регистрации запросов в аналитике хранилища промежуточные результаты передаются в качестве блоков. Периодически эти блоки фиксируются в аналитике хранилища и к ним предоставляется доступ как к большим двоичным объектам. Появление данных журнала в BLOB-объектах в контейнере **$logs** может занимать до одного часа, что определяется периодичностью очистки задач записи в журнал Службой хранилища. В журналах, созданных в один и тот же час, могут присутствовать повторяющиеся записи. Вы можете определить, является ли запись двойной, проверив идентификатор запроса **RequestId** и **номер операции**.

Если каждый час появляется большой объем данных журнала в нескольких файлах, то для определения того, какие данные должны содержаться в журнале, следует руководствоваться метаданными BLOB-объектов в полях метаданных. Этот прием также полезен по той причине, что иногда при записи данных в файл журнала имеет место задержка. Метаданные содержат более точные сведения о содержимом BLOB-объекта, чем имя BLOB-объекта.

Большая часть средств обзора позволяет просматривать метаданные BLOB-объектов; кроме того, эти сведения также можно считывать с использованием PowerShell или программным способом. Следующий фрагмент команды PowerShell является примером фильтрации списка BLOB-объектов журнала по имени для указания времени, а также по метаданным с целью определения только тех журналов, которые содержат операции **записи**.  

 ```powershell
 Get-AzStorageBlob -Container '$logs' |  
 Where-Object {  
     $_.Name -match 'table/2014/05/21/05' -and   
     $_.ICloudBlob.Metadata.LogType -match 'write'  
 } |  
 ForEach-Object {  
     "{0}  {1}  {2}  {3}" –f $_.Name,   
     $_.ICloudBlob.Metadata.StartTime,   
     $_.ICloudBlob.Metadata.EndTime,   
     $_.ICloudBlob.Metadata.LogType  
 }  
 ```  

Сведения о программном методе отображения BLOB-объектов см. в статьях [Перечисление ресурсов хранилища BLOB-объектов](https://msdn.microsoft.com/library/azure/hh452233.aspx) и [Настройка и получение свойств и метаданных для ресурсов службы BLOB-объектов](https://msdn.microsoft.com/library/azure/dd179404.aspx).  

### <a name="log-naming-conventions"></a>Соглашения об именовании журналов

 Каждый журнал записывается в следующем формате.

 `<service-name>/YYYY/MM/DD/hhmm/<counter>.log`

 В следующей таблице описан каждый атрибут имени журнала.

|attribute|Описание|
|---------------|-----------------|
|`<service-name>`|имя службы хранилища. Например, `blob`, `table` или `queue`.|
|`YYYY`|Четырехзначное обозначение года создания журнала. Например: `2011`|
|`MM`|Двузначное обозначение месяца создания журнала. Например: `07`|
|`DD`|Двузначное обозначение дня создания журнала. Например: `31`|
|`hh`|Двузначное обозначение часа, которое указывает час начала ведения журналов, в 24-часовом формате UTC. Например: `18`|
|`mm`|Двузначное обозначение минуты, которое указывает минуту начала ведения журналов. **Примечание.**  Это значение не поддерживается в текущей версии Аналитики Службы хранилища, а его значение всегда равно `00`.|
|`<counter>`|Шестизначный счетчик с отсчетом от нуля, который показывает количество больших двоичных объектов журнала, созданных для службы хранилища в течение часа. Отсчет этого счетчика начинается с `000000`. Например: `000001`|

 Ниже приведено полное имя журнала, в котором представлены вместе все упомянутые примеры.

 `blob/2011/07/31/1800/000001.log`

 Ниже приведен пример URI, который может использоваться для получения доступа к указанному журналу.

 `https://<accountname>.blob.core.windows.net/$logs/blob/2011/07/31/1800/000001.log`

 При регистрации запроса хранилища имя результирующего журнала сопоставляется с часом завершения запрошенной операции. Например, если запрос GetBlob был завершен в 18:30 31.07.2011 г., то журнал должен быть записан со следующим префиксом: `blob/2011/07/31/1800/`.

### <a name="log-metadata"></a>Метаданные журнала

 Все большие двоичные объекты журнала хранятся с метаданными, с помощью которых можно определить, какие данные журнала содержит большой двоичный объект. В следующей таблице описан каждый атрибут метаданных.

|attribute|Описание|
|---------------|-----------------|
|`LogType`|Описывает, содержит ли журнал информацию, относящуюся к операциям чтения, записи или удаления. Это значение может содержать данные одного типа или сочетание всех трех типов данных, разделенных запятыми.<br /><br /> Пример 1: `write`<br /><br /> Пример 2: `read,write`<br /><br /> Пример 3: `read,write,delete`|
|`StartTime`|Время первой записи в журнале в форме `YYYY-MM-DDThh:mm:ssZ`. Например: `2011-07-31T18:21:46Z`|
|`EndTime`|Время самой последней записи в журнале в форме `YYYY-MM-DDThh:mm:ssZ`. Например: `2011-07-31T18:22:09Z`|
|`LogVersion`|Версия формата журнала.|

 В следующем списке отображается полный образец метаданных, в котором используются приведенные выше примеры.

-   `LogType=write`
-   `StartTime=2011-07-31T18:21:46Z`
-   `EndTime=2011-07-31T18:22:09Z`
-   `LogVersion=1.0`

## <a name="enable-storage-logging"></a>Включение ведения журнала службы хранилища

Ведение журнала службы хранилища можно включить с помощью портала Azure, PowerShell и пакетов SDK для службы хранилища.

### <a name="enable-storage-logging-using-the-azure-portal"></a>Включение ведения журнала Службы хранилища с помощью портала Azure  

На портале Azure используйте колонку **Параметры диагностики (классические)** для управления ведением журнала службы хранилища, доступного в разделе **Мониторинг (классический)** в **колонке меню** учетной записи хранения.

Можно указать службы хранилища, которые требуется регистрировать, и срок хранения записанных данных (в днях).  

### <a name="enable-storage-logging-using-powershell"></a>Включение ведения журнала службы хранилища с помощью PowerShell  

 PowerShell можно использовать на локальном компьютере для настройки ведения журнала хранилища в учетной записи хранения с помощью командлета Azure PowerShell **Get-азсторажесервицелоггингпроперти** для получения текущих параметров, а командлет **Set-азсторажесервицелоггингпроперти —** для изменения текущих параметров.  

 В командлетах, позволяющих управлять ведением журнала службы хранилища, используется параметр **LoggingOperations**, представляющий собой строку, содержащую разделенный запятыми список типов запросов, подлежащих занесению в журнал. Всего используются запросы трех типов — **чтение**, **запись** и **удаление**. Чтобы отключить ведение журнала, укажите для параметра **LoggingOperations** значение **none**.  

 Следующая команда включает занесение в журнал записей о запросах на чтение, запись и удаление службой очередей в учетной записи хранения по умолчанию, при этом устанавливается время хранения, равное пяти дням:  

```powershell
Set-AzStorageServiceLoggingProperty -ServiceType Queue -LoggingOperations read,write,delete -RetentionDays 5  
```  

 Следующая команда отключает ведение журнала для службы таблиц в учетной записи по умолчанию:  

```powershell
Set-AzStorageServiceLoggingProperty -ServiceType Table -LoggingOperations none  
```  

 Дополнительные сведения о настройке командлетов Azure PowerShell для работы с подпиской Azure и о выборе учетной записи хранения по умолчанию см. в статье с [общими сведениями об Azure PowerShell](https://azure.microsoft.com/documentation/articles/install-configure-powershell/).  

### <a name="enable-storage-logging-programmatically"></a>Программное включение ведения журнала службы хранилища  

 Помимо управления ведением журнала службы хранилища с помощью портала Azure и командлетов Azure PowerShell, можно также использовать один из API-интерфейсов службы хранилища Azure. Например, если используется язык .NET, можно использовать клиентскую библиотеку хранилища.  

# <a name="net-v12-sdk"></a>[\.NET (пакет SDK версии 12)](#tab/dotnet)

:::code language="csharp" source="~/azure-storage-snippets/queues/howto/dotnet/dotnet-v12/Monitoring.cs" id="snippet_EnableDiagnosticLogs":::

# <a name="net-v11-sdk"></a>[\..NET (пакет SDK версии 11)](#tab/dotnet11)

```csharp
var storageAccount = CloudStorageAccount.Parse(connStr);  
var queueClient = storageAccount.CreateCloudQueueClient();  
var serviceProperties = queueClient.GetServiceProperties();  

serviceProperties.Logging.LoggingOperations = LoggingOperations.All;  
serviceProperties.Logging.RetentionDays = 2;  

queueClient.SetServiceProperties(serviceProperties);  
```  

---


 Дополнительные сведения о настройке ведения журнала службы хранилища с помощью языка .NET см. в [справочнике по клиентской библиотеке хранилища](https://msdn.microsoft.com/library/azure/dn261237.aspx).  

 Общие сведения о настройке ведения журнала службы хранилища с использованием REST API см. в статье [Включение и настройка Аналитики Службы хранилища](https://msdn.microsoft.com/library/azure/hh360996.aspx).  

## <a name="download-storage-logging-log-data"></a>Скачивание данных журнала службы хранилища

 Для просмотра и анализа данных журнала необходимо скачать BLOB-объекты, содержащие требуемые данные журнала, на локальный компьютер. Многие средства обзора хранилища позволяют скачивать BLOB-объекты из учетной записи хранения. Кроме того, для скачивания данных журнала можно использовать средство командной строки Azure Copy Tool ([AzCopy](storage-use-azcopy-v10.md)), предоставляемое группой службы хранилища Azure.  
 
>[!NOTE]
> Контейнер `$logs` не интегрирован в Сетку событий, поэтому вы не будете получать уведомления при записи файлов журнала. 

 Чтобы скачать только нужные данные и избежать повторного скачивания тех же данных журнала, сделайте следующее.  

-   Примените соглашение об именовании по дате и времени к BLOB-объектам, чтобы отслеживать, какие из них уже были скачаны для анализа, и предотвратить их повторное скачивание.  

-   С помощью метаданных в BLOB-объектах, содержащих данные журнала, определите конкретный период, которому соответствуют содержащиеся в BLOB-объекте данные журнала. Это позволяет найти требуемый для скачивания BLOB-объект.  

Чтобы приступить к работе с AzCopy, см. сведения в статье [Начало работы с AzCopy](storage-use-azcopy-v10.md). 

В следующем примере показано, как можно скачать данные журнала для службы очереди, время начала которых соответствует 9:00, 10:00 и 11:00 20 мая 2014 г.

```
azcopy copy 'https://mystorageaccount.blob.core.windows.net/$logs/queue' 'C:\Logs\Storage' --include-path '2014/05/20/09;2014/05/20/10;2014/05/20/11' --recursive
```

Дополнительные сведения о скачивании конкретных файлов см. в [этом разделе](https://docs.microsoft.com/azure/storage/common/storage-use-azcopy-blobs?toc=%2fazure%2fstorage%2fblobs%2ftoc.json#download-specific-files).

После скачивания данных журнала можно просматривать записи журнала в файлах. В этих файлах журнала используется текстовый формат с разделителями, в котором могут анализироваться многие средства чтения журнала (Дополнительные сведения см. в разделе Руководство по [мониторингу, диагностике и устранению неполадок служба хранилища Microsoft Azure](storage-monitoring-diagnosing-troubleshooting.md)). Такие средства предоставляют различные возможности форматирования, фильтрации, сортировки и поиска содержимого файлов журнала. Дополнительные сведения о формате и содержимом файлов журнала службы хранилища см. в статьях [Формат журнала Аналитики Службы хранилища](/rest/api/storageservices/storage-analytics-log-format) и [Операции и сообщения о состоянии, заносимые в журнал Аналитики Службы хранилища](/rest/api/storageservices/storage-analytics-logged-operations-and-status-messages).

## <a name="next-steps"></a>Дальнейшие действия

* [Формат журналов аналитик хранилища](/rest/api/storageservices/storage-analytics-log-format)
* [Операции с протоколированием и сообщения о состоянии аналитик хранилища](/rest/api/storageservices/storage-analytics-logged-operations-and-status-messages)
* [Метрики Аналитики Службы хранилища (классические)](storage-analytics-metrics.md)
