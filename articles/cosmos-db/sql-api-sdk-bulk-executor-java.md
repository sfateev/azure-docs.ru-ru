---
title: Azure Cosmos DB — API-интерфейс Java, пакет SDK и ресурсы для исполнителя массовых операций
description: Сведения о пакете SDK и API Java массового исполнителя, включая даты выхода и прекращения использования, а также изменения, внесенные в каждую версию пакета SDK Java массового исполнителя в Azure Cosmos DB.
author: anfeldma-ms
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.devlang: java
ms.topic: reference
ms.date: 08/12/2020
ms.author: anfeldma
ms.custom: devx-track-java
ms.openlocfilehash: 72a224d4ad1807d095baa8db819878bf2e22e133
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2020
ms.locfileid: "91802724"
---
# <a name="java-bulk-executor-library-download-information"></a>Библиотека Bulk Executor для Java: информация о скачивании

> [!div class="op_single_selector"]
> * [Пакет SDK версии 3 для .NET](sql-api-sdk-dotnet-standard.md)
> * [Пакет SDK для .NET версии 2](sql-api-sdk-dotnet.md)
> * [Пакет SDK для .NET Core версии 2](sql-api-sdk-dotnet-core.md)
> * [Пакет SDK для веб-канала изменений .NET версии 2](sql-api-sdk-dotnet-changefeed.md)
> * [Node.js](sql-api-sdk-node.md)
> * [Пакет SDK для Java версии 4](sql-api-sdk-java-v4.md)
> * [Пакет SDK для Async Java версии 2](sql-api-sdk-async-java.md)
> * [Пакет SDK для Sync Java версии 2](sql-api-sdk-java.md)
> * [Пружинные данные v2](sql-api-sdk-java-spring-v2.md)
> * [Spring Data версии 3](sql-api-sdk-java-spring-v3.md)
> * [Соединитель Spark](sql-api-sdk-java-spark.md)
> * [Python](sql-api-sdk-python.md)
> * [REST](/rest/api/cosmos-db/)
> * [Поставщик ресурсов REST](/rest/api/cosmos-db-resource-provider/)
> * [SQL](sql-api-query-reference.md)
> * [Групповой исполнитель — .NET v2](sql-api-sdk-bulk-executor-dot-net.md)
> * [Исполнитель массовых операций — Java](sql-api-sdk-bulk-executor-java.md)

| |  |
|---|---|
|**Описание**|Библиотека массового исполнителя позволяет клиентским приложениям выполнять массовые операции в учетных записях Azure Cosmos DB. Библиотека массового исполнителя предоставляет пространства имен BulkImport и BulkUpdate. Модуль BulkImport может оптимизировать массовый прием документов, обеспечивая использование максимального объема пропускной способности, подготовленной для коллекции. Модуль BulkUpdate позволяет выполнять массовое обновление существующих данных в контейнерах Azure Cosmos в виде исправлений.|
|**Скачивание пакета SDK**|[Maven](https://search.maven.org/#search%7Cga%7C1%7Cdocumentdb-bulkexecutor)|
|**Библиотека исполнителя массовых операций в GitHub**|[GitHub](https://github.com/Azure/azure-cosmosdb-bulkexecutor-java-getting-started)|
| **Документация по API**| [Справочная документация по API Java](https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb.bulkexecutor)|
|**Начало работы**|[Начало работы с пакетом SDK для Java для библиотеки массового исполнителя](bulk-executor-java.md)|
|**Минимальная поддерживаемая среда выполнения**|[Пакет средств разработки Java (JDK) 7+](/java/azure/jdk/?view=azure-java-stable&preserve-view=true)|

## <a name="release-notes"></a>Заметки о выпуске

### <a name="2100"></a><a name="2.10.0"></a>2.10.0

* Внесено исправление для DocumentAnalyzer.java, чтобы значения ключей вложенных разделов правильно извлекались из JSON.

### <a name="294"></a><a name="2.9.4"></a>2.9.4

* Добавлены дополнительные функции BulkDelete для повторного выполнения операции в случае конкретных сбоев, а также для получения пользователем списка сбоев операций, которые можно повторить.

### <a name="293"></a><a name="2.9.3"></a>2.9.3

* Обновление для пакета SDK для Cosmos версии 2.4.7.

### <a name="292"></a><a name="2.9.2"></a>2.9.2

* Исправлена работа функции mergeAll, чтобы продолжить выполнение операции на основе идентификатора и значения ключа раздела, чтобы все обновляемые свойства документа, которые расположены после id и значения ключа раздела, добавлялись в список обновленных элементов.

### <a name="291"></a><a name="2.9.1"></a>2.9.1

* Обновлена начальная степень параллелизма до значения 1 и добавлены журналы отладки для мини-пакета.


