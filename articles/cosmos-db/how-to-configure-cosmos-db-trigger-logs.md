---
title: Настройка и чтение журналов с помощью триггеров функций Azure для Cosmos DB
description: Узнайте, как предоставить журналы в конвейер ведения журнала "Функции Azure" при использовании активации функций Azure для Cosmos DB
author: ealsur
ms.service: cosmos-db
ms.topic: how-to
ms.date: 07/17/2019
ms.author: maquaran
ms.openlocfilehash: 69a8f44831f1af13158261bedb19a254c6a565a6
ms.sourcegitcommit: 419c8c8061c0ff6dc12c66ad6eda1b266d2f40bd
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/18/2020
ms.locfileid: "92165304"
---
# <a name="how-to-configure-and-read-the-logs-when-using-azure-functions-trigger-for-cosmos-db"></a>Процесс настройки и чтения журналов при использовании активации функций Azure для Cosmos DB

В этой статье описывается, как можно настроить среду функций Azure для отправки триггеров функций Azure для журналов Cosmos DB в настроенное [решение мониторинга](../azure-functions/functions-monitoring.md).

## <a name="included-logs"></a>Доступные журналы

При активации функций Azure для Cosmos DB для используется [библиотека обработчика канала изменений](./change-feed-processor.md) на внутреннем уровне. Библиотека создает набор журналов работоспособности, которые могут использоваться для мониторинга внутренних операций в целях [устранения неполадок](./troubleshoot-changefeed-functions.md).

В журналах работоспособности описывается, как ведет себя активация функций Azure для Cosmos DB при выполнении операций в сценариях балансировки нагрузки или инициализации.

## <a name="enabling-logging"></a>Включение ведения журналов

Чтобы включить ведение журналов при использовании активации функций Azure для Cosmos DB, найдите файл `host.json` в проекте Функций Azure или приложении Функций Azure и [настройте требуемый уровень ведения журнала](../azure-functions/configure-monitoring.md#configure-log-levels). Необходимо включить данные трассировки `Host.Triggers.CosmosDB`, как показано в следующем примере:

```js
{
  "version": "2.0",
  "logging": {
    "fileLoggingMode": "always",
    "logLevel": {
      "Host.Triggers.CosmosDB": "Trace"
    }
  }
}
```

Когда будут развернуты Функции Azure с обновленной конфигурацией, журналы активации функций Azure для Cosmos DB отобразятся как часть трассировки. Журналы можно просмотреть в настроенном поставщике ведения журнала в разделе *Категории* `Host.Triggers.CosmosDB`.

## <a name="query-the-logs"></a>Выполнение запросов к журналам

Выполните следующий запрос к журналам, созданным с помощью активации функций Azure для Cosmos DB в [Overview of log queries in Azure Monitor](../azure-monitor/app/analytics.md) (Обзор запросов журнала в Azure Monitor):

```sql
traces
| where customDimensions.Category == "Host.Triggers.CosmosDB"
```

## <a name="next-steps"></a>Дальнейшие действия

* [Включение мониторинга](../azure-functions/functions-monitoring.md) в приложениях Функций Azure.
* Узнайте, как выполнять [диагностику и устранять неполадки при использовании активации функций Azure для Cosmos DB](./troubleshoot-changefeed-functions.md).
