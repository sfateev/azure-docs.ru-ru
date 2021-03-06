---
title: Мониторинг функций Azure с помощью журналов Azure Monitor
description: Узнайте, как использовать журналы Azure Monitor с помощью функций Azure для наблюдения за выполнением функций.
author: craigshoemaker
ms.topic: conceptual
ms.date: 04/15/2020
ms.author: cshoe
ms.custom: devx-track-csharp, devx-track-python
ms.openlocfilehash: 51c611b2565ae0a5a054a45f0aedcb039351b46b
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2020
ms.locfileid: "88208372"
---
# <a name="monitoring-azure-functions-with-azure-monitor-logs"></a>Мониторинг функций Azure с помощью журналов Azure Monitor

Функции Azure предлагают интеграцию с [журналами Azure Monitor](../azure-monitor/platform/data-platform-logs.md) для мониторинга функций. В этой статье показано, как настроить функции Azure для отправки сформированных системой и созданных пользователем журналов в Azure Monitor журналы.

Журналы Azure Monitor дают возможность консолидировать журналы из разных ресурсов в одной рабочей области, где их можно анализировать с помощью [запросов](../azure-monitor/log-query/log-query-overview.md) для быстрого извлечения, консолидации и анализа собранных данных.  Вы можете создавать и тестировать запросы с помощью [Log Analytics](../azure-monitor/log-query/log-query-overview.md) на портале Azure, а затем либо напрямую анализировать данные с помощью этих средств, либо сохранять запросы для использования с [визуализациями](../azure-monitor/visualizations.md) или [правилами генерации оповещений](../azure-monitor/platform/alerts-overview.md).

Azure Monitor использует версию [языка запросов Kusto](/azure/kusto/query/), который применяется в Azure Data Explorer. Он позволяет выполнять простые запросы к журналам, но также включает и расширенную функциональность, например функции агрегирования, объединения и интеллектуальную аналитику. Доступно [множество уроков](../azure-monitor/log-query/get-started-queries.md) для быстрого изучения этого языка.

> [!NOTE]
> Интеграция с журналами Azure Monitor в настоящее время доступна в общедоступной предварительной версии для приложений-функций, работающих в планах использования Windows, Premium и на основе специальных планов размещения.

## <a name="setting-up"></a>Настройка

1. В разделе **мониторинг** приложения функции в [портал Azure](https://portal.azure.com)выберите **параметры диагностики**, а затем щелкните **Добавить параметр диагностики**.

   :::image type="content" source="media/functions-monitor-log-analytics/diagnostic-settings-add.png" alt-text="Выбор параметров диагностики":::

1. На странице **параметры диагностики** в разделе **сведения о категории** и **Журнал**выберите **функтионапплогс**.

   Таблица **функтионапплогс** содержит нужные журналы.

1. В разделе **сведения о назначении**выберите **Отправить log Analytics**. затем выберите свою **рабочую область log Analytics**. 

1. Введите **имя параметров диагностики**и нажмите кнопку **сохранить**.

   :::image type="content" source="media/functions-monitor-log-analytics/choose-table.png" alt-text="Выбор параметров диагностики":::

## <a name="user-generated-logs"></a>Журналы, созданные пользователем

Чтобы создать пользовательские журналы, используйте инструкцию ведения журнала, относящуюся к вашему языку. Ниже приведены примеры фрагментов кода.


# <a name="c"></a>[C#](#tab/csharp)

```csharp
log.LogInformation("My app logs here.");
```

# <a name="java"></a>[Java](#tab/java)

```java
context.getLogger().info("My app logs here.");
```

# <a name="javascript"></a>[JavaScript](#tab/javascript)

```javascript
context.log('My app logs here.');
```

# <a name="powershell"></a>[PowerShell](#tab/powershell)

```powershell
Write-Host "My app logs here."
```

# <a name="python"></a>[Python](#tab/python)

```python
logging.info('My app logs here.')
```

---

## <a name="querying-the-logs"></a>Запрос журналов

Для создания запросов к созданным журналам:
 
1. В приложении функции выберите **параметры диагностики**. 

1. В списке **параметры диагностики** выберите рабочую область log Analytics, в которую будут отправлены журналы функций. 

1. На странице **log Analytics Рабочая область** выберите **журналы**.

   Функции Azure записывают все журналы в таблицу **функтионапплогс** в разделе **логманажемент**. 

   :::image type="content" source="media/functions-monitor-log-analytics/querying.png" alt-text="Выбор параметров диагностики":::

Ниже приведены некоторые примеры запросов.

### <a name="all-logs"></a>Все журналы

```

FunctionAppLogs
| order by TimeGenerated desc

```

### <a name="specific-function-logs"></a>Журналы конкретных функций

```

FunctionAppLogs
| where FunctionName == "<Function name>" 

```

### <a name="exceptions"></a>Исключения

```

FunctionAppLogs
| where ExceptionDetails != ""  
| order by TimeGenerated asc

```

## <a name="next-steps"></a>Дальнейшие шаги

- Ознакомьтесь с [обзором функций Azure](functions-overview.md).
- Дополнительные сведения о [журналах Azure Monitor](../azure-monitor/platform/data-platform-logs.md).
- Узнайте больше о [языке запросов](../azure-monitor/log-query/get-started-queries.md).
