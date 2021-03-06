---
title: 'Azure Cosmos DB: заметки о выпуске и материалы по пакету SDK Java версии 4 для API-интерфейса SQL'
description: Сведения о пакете SDK версии 4 и API-интерфейсе SQL для Azure Cosmos DB, включая даты выхода и прекращения использования, а также изменения, внесенные в каждую версию пакета SDK SQL Async Java для Azure Cosmos DB.
author: anfeldma-ms
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.devlang: java
ms.topic: reference
ms.date: 08/12/2020
ms.author: anfeldma
ms.custom: devx-track-java
ms.openlocfilehash: 686c1e37e093a358bb5122fbfeccdfd98eb301ce
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2020
ms.locfileid: "91804237"
---
# <a name="azure-cosmos-db-java-sdk-v4-for-core-sql-api-release-notes-and-resources"></a>Azure Cosmos DB: заметки о выпуске и материалы по пакету SDK Java версии 4 для API-интерфейса Core (SQL)
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

Пакет SDK Java версии 4 для Azure Cosmos DB для Core (SQL) объединяет асинхронный и синхронный интерфейсы API в один артефакт Maven. Пакет SDK версии 4 обеспечивает более высокую производительность, новые функции API и поддержку асинхронных операций на основе проекта Reactor и [библиотеки Netty](https://netty.io/). При использовании пакета SDK Java версии 4 для Azure Cosmos DB пользователи могут получить более высокую производительность по сравнению с [пакетом SDK Async Java версии 2 для Azure Cosmos DB](sql-api-sdk-async-java.md) и [пакетом SDK Sync Java версии 2 для Azure Cosmos DB](sql-api-sdk-java.md).

> [!IMPORTANT]  
> Эти заметки о выпуске применимы только к пакету SDK Java для Azure Cosmos DB версии 4. Если сейчас вы используете более раннюю версию, чем версия 4, руководство [Перевод приложения на использование пакета средств разработки Java для Azure Cosmos DB версии 4](migrate-java-v4-sdk.md) поможет вам обновить его до версии 4.
>
> Ниже приведены шаги, позволяющие быстро приступить к работе.
> 1. Установите [минимальную поддерживаемую среду выполнения Java (JDK 8)](/java/azure/jdk/?view=azure-java-stable&preserve-view=true), чтобы можно было использовать пакет SDK.
> 2. Ознакомьтесь с [кратким руководством по пакету SDK Java версии 4 для Azure Cosmos DB](https://docs.microsoft.com/azure/cosmos-db/create-sql-api-java), чтобы получить доступ к артефакту Maven и изучить основные запросы к Azure Cosmos DB.
> 3. Ознакомьтесь с [советами по повышению производительности](performance-tips-java-sdk-v4-sql.md) пакета SDK Java версии 4 для Azure Cosmos DB и руководствами по [устранению неполадок](troubleshoot-java-sdk-v4-sql.md), чтобы оптимизировать пакет SDK для приложения.
>
> [Семинары и лабораторные занятия по Azure Cosmos DB](https://aka.ms/cosmosworkshop) — еще один отличный ресурс для освоения пакета SDK Java версии 4 для Azure Cosmos DB.
>

## <a name="helpful-content"></a>Полезное содержимое

| Content | Ссылка |
|---|---|
|**Скачивание пакета SDK**| [Maven](https://mvnrepository.com/artifact/com.azure/azure-cosmos) |
|**Документация по API** | [Справочная документация по API Java](https://docs.microsoft.com/java/api/overview/azure/cosmosdb/client?view=azure-java-stable&preserve-view=true) |
|**Участие в разработке пакета SDK** | [Центральный репозиторий пакета SDK Azure для Java в GitHub](https://github.com/Azure/azure-sdk-for-java/tree/master/sdk/cosmos/azure-cosmos) | 
|**Начало работы** | [Краткое руководство. Создание приложения Java для управления данными API SQL для Azure Cosmos DB](https://docs.microsoft.com/azure/cosmos-db/create-sql-api-java) <br> [Репозиторий GitHub с кодом краткого руководства](https://github.com/Azure-Samples/azure-cosmos-java-getting-started) | 
|**Простые примеры кода** | [Azure Cosmos DB. Примеры Java для API SQL](sql-api-java-sdk-samples.md) <br> [Репозиторий GitHub с примером кода](https://github.com/Azure-Samples/azure-cosmos-java-sql-api-samples)|
|**Консольное приложение с каналом изменений**| [Веб-канал изменений — пример пакета SDK для Java v4](create-sql-api-java-changefeed.md) <br> [Репозиторий GitHub с примером кода](https://github.com/Azure-Samples/azure-cosmos-java-sql-app-example)| 
|**Пример веб-приложения**| [Создание веб-приложения с помощью пакета SDK для Java v4](sql-api-java-application.md) <br> [Репозиторий GitHub с примером кода](https://github.com/Azure-Samples/azure-cosmos-java-sql-api-todo-app)|
| **Советы по улучшению производительности**| [Советы по повышению производительности при использовании пакета SDK для Java версии 4](performance-tips-java-sdk-v4-sql.md)| 
| **Устранение неполадок** | [Устранение неполадок с пакетом SDK для Java версии 4](troubleshoot-java-sdk-v4-sql.md) |
| **Переход на пакет SDK версии 4 с более ранней версии** | [Переход на пакет SDK для Java версии 4](migrate-java-v4-sdk.md) |
| **Минимальная поддерживаемая среда выполнения**|[JDK 8](/java/azure/jdk/?view=azure-java-stable&preserve-view=true); | 
| **Семинары и лабораторные занятия по Azure Cosmos DB** |[Домашняя страница семинаров по Cosmos DB](https://aka.ms/cosmosworkshop)

[!INCLUDE[Release notes](~/azure-sdk-for-java-cosmos-db/sdk/cosmos/azure-cosmos/CHANGELOG.md)]

## <a name="faq"></a>ВОПРОСЫ И ОТВЕТЫ
[!INCLUDE [cosmos-db-sdk-faq](../../includes/cosmos-db-sdk-faq.md)] 

## <a name="next-steps"></a>Дальнейшие действия
Дополнительные сведения о Cosmos DB см. на странице службы [Microsoft Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/).