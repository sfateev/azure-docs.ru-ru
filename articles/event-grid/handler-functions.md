---
title: Использование функции Azure как обработчика событий Сетки событий Azure
description: Узнайте, как можно использовать функции Azure в качестве обработчиков событий Сетки событий Azure.
ms.topic: conceptual
ms.date: 09/18/2020
ms.openlocfilehash: 9e04fd3e04dab7a50940c2a4a799a56d447fbb6e
ms.sourcegitcommit: dbe434f45f9d0f9d298076bf8c08672ceca416c6
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/17/2020
ms.locfileid: "92145755"
---
# <a name="azure-function-as-an-event-handler-for-event-grid-events"></a>Использование функции Azure как обработчика событий Сетки событий

Обработчик событий — это место, куда отправляются события. Обработчик выполняет действие для обработки события. Некоторые службы Azure автоматически настроены для обработки событий. Одна из них — это **Функции Azure**. 


Чтобы использовать функцию Azure в качестве обработчика событий, выполните один из следующих подходов: 

-   Использовать [триггер сетки событий](../azure-functions/functions-bindings-event-grid-trigger.md).  Укажите **функцию Azure** в качестве **типа конечной точки**. Затем укажите приложение функции Azure и функцию, которая будет выполнять обработку событий. 
-   Использовать [триггер HTTP](../azure-functions/functions-bindings-http-webhook.md).  Укажите **веб-перехватчик** в качестве **типа конечной точки**. Затем укажите URL-адрес для функции Azure, которая будет выполнять обработку событий. 

Рекомендуется использовать первый подход (триггер сетки событий), так как он имеет следующие преимущества по сравнению с вторым подходом:
-   Сетка событий автоматически проверяет триггеры Сетки событий. При использовании универсальных триггеров HTTP вам нужно самостоятельно реализовать [ответ проверки](webhook-event-delivery.md).
-   Сетка событий автоматически корректирует скорость, с которой события доставляются в функцию, активируемую событием сетки событий на основе наблюдаемой скорости, с которой функция может обрабатывать события. Это значение соответствует ошибкам доставки авертс, которые возникают из-за невозможности функции обрабатывать события, так как скорость обработки событий функции может меняться с течением времени. Для повышения эффективности при высокой пропускной способности включите пакетную обработку в подписке на события. Дополнительные сведения см. в разделе [Включение пакетной обработки](#enable-batching).

    > [!NOTE]
    > В настоящее время нельзя использовать триггер службы "Сетка событий" для приложения "функции Azure", когда событие доставляется в схему **клаудевентс** . В этом случае необходимо использовать триггер HTTP.

## <a name="tutorials"></a>Учебники

|Title  |Описание  |
|---------|---------|
| [Краткое руководство. Обработка событий с помощью функции](custom-event-to-function.md) | Отправка пользовательского события в функцию для обработки. |
| [Руководство. Автоматическое изменение размера переданных изображений с помощью службы "Сетка событий"](resize-images-on-storage-blob-upload-event.md) | Пользователи отправляют изображения в учетную запись хранения через веб-приложение. При создании большого двоичного объекта в хранилище служба "Сетка событий" отправляет событие приложению-функции, которое изменяет размер отправленного изображения. |
| [Потоковая передача больших данных в хранилище данных](event-grid-event-hubs-integration.md) | Когда Центры событий создают файл сбора, служба "Сетка событий" отправляет событие приложению-функции. Это приложение извлекает файл сбора и переносит данные в хранилище данных. |
| [Руководство. с примерами интеграции служебной шины Azure со службой "Сетка событий Azure"](../service-bus-messaging/service-bus-to-event-grid-integration-example.md?toc=%2fazure%2fevent-grid%2ftoc.json) | Сетка событий отправляет сообщения из раздела Служебной шины в приложение-функцию и приложение логики. |

## <a name="rest-example-for-put"></a>Пример REST (для PUT)

```json
{
    "properties": 
    {
        "destination": 
        {
            "endpointType": "AzureFunction",
            "properties": 
            {
                "resourceId": "/subscriptions/<AZURE SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP NAME>/providers/Microsoft.Web/sites/<FUNCTION APP NAME>/functions/<FUNCTION NAME>",
                "maxEventsPerBatch": 10,
                "preferredBatchSizeInKilobytes": 6400
            }
        },
        "eventDeliverySchema": "EventGridSchema"
    }
}
```

## <a name="enable-batching"></a>Включение пакетной обработки
Для более высокой пропускной способности включите пакетную обработку в подписке. При использовании портал Azure можно задать максимальное количество событий в пакете и предпочтительный размер пакета в килобайтах байт во время создания подписки или после ее создания. 

Параметры пакетной службы можно настроить с помощью шаблона портал Azure, PowerShell, CLI или диспетчер ресурсов. 

### <a name="azure-portal"></a>Портал Azure
Когда вы создаете подписку в пользовательском интерфейсе, на странице **Создание подписки на события** перейдите на вкладку **Дополнительные функции** и задайте для параметра **Максимальное число событий в пакете** и **предпочтительный размер пакета в килобайтах**. 
    
:::image type="content" source="./media/custom-event-to-function/enable-batching.png" alt-text="Включить пакетную обработку во время создания подписки&quot;:::

Вы можете обновить эти значения для существующей подписки на вкладке &quot; **функции** " страницы **раздела "Сетка событий** ". 

:::image type="content" source="./media/custom-event-to-function/features-batch-settings.png" alt-text="Включить пакетную обработку во время создания подписки&quot;:::

Вы можете обновить эти значения для существующей подписки на вкладке &quot; **функции** ":::

### <a name="azure-resource-manager-template"></a>Шаблон Azure Resource Manager
**Максевентспербатч** и **преферредбатчсизеинкилобитес** можно задать в шаблоне Azure Resource Manager. Дополнительные сведения см. в [справочнике по шаблонам Microsoft. EventGrid eventSubscriptions](/azure/templates/microsoft.eventgrid/eventsubscriptions).

### <a name="azure-cli"></a>Azure CLI
Для настройки параметров, связанных с пакетной службой, можно использовать команду [AZ eventgrid Event-Subscription Create](/cli/azure/eventgrid/event-subscription?view=azure-cli-latest#az_eventgrid_event_subscription_create&preserve-view=true) или [AZ eventgrid Event-Subscription Update](/cli/azure/eventgrid/event-subscription?view=azure-cli-latest#az_eventgrid_event_subscription_update&preserve-view=true) . для этого используйте следующие параметры: `--max-events-per-batch` или `--preferred-batch-size-in-kilobytes` .

### <a name="azure-powershell"></a>Azure PowerShell
С помощью командлета [New-азевентгридсубскриптион](/powershell/module/az.eventgrid/new-azeventgridsubscription) или [Update-азевентгридсубскриптион](/powershell/module/az.eventgrid/update-azeventgridsubscription) можно настроить параметры, связанные с пакетной службой, используя следующие параметры: `-MaxEventsPerBatch` или `-PreferredBatchSizeInKiloBytes` .

## <a name="next-steps"></a>Дальнейшие действия
См. список поддерживаемых обработчиков событий в статье [Обработчики событий](event-handlers.md).