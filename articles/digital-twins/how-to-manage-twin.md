---
title: Управление цифровыми двойниками
titleSuffix: Azure Digital Twins
description: Узнайте, как извлекать, обновлять и удалять отдельные двойников и связи.
author: baanders
ms.author: baanders
ms.date: 4/10/2020
ms.topic: how-to
ms.service: digital-twins
ms.openlocfilehash: c522ac9e1aedbcdfdb4564d17b506b1b490da0c3
ms.sourcegitcommit: dbe434f45f9d0f9d298076bf8c08672ceca416c6
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/17/2020
ms.locfileid: "92150405"
---
# <a name="manage-digital-twins"></a>Управление цифровыми двойниками

Сущности в вашей среде представлены [цифровым двойников](concepts-twins-graph.md). Управление цифровым двойников может включать создание, изменение и удаление. Для выполнения этих операций можно использовать [**API дигиталтвинс**](how-to-use-apis-sdks.md), [пакет SDK для .NET (C#)](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/digitaltwins/Azure.DigitalTwins.Core)или [Azure Digital двойников CLI](how-to-use-cli.md).

Эта статья посвящена управлению цифровыми двойников; сведения о работе со связями и [графом двойника](concepts-twins-graph.md) в целом см. в разделе [*руководство. Управление диаграммой двойника с помощью связей*](how-to-manage-graph.md).

> [!TIP]
> Все функции пакета SDK имеют синхронные и асинхронные версии.

## <a name="create-a-digital-twin"></a>Создание цифрового двойника

Чтобы создать двойника, используйте `CreateDigitalTwin` метод в клиенте службы следующим образом:

```csharp
await client.CreateDigitalTwinAsync("myNewTwinID", initData);
```

Чтобы создать цифровой двойника, необходимо предоставить:
* Требуемый идентификатор для цифрового двойника
* [Модель](concepts-models.md) , которую вы хотите использовать 

При необходимости можно указать начальные значения для всех свойств цифрового двойника. 

Значения модели и начальных свойств предоставляются с помощью `initData` параметра, который представляет собой строку JSON, содержащую соответствующие данные. Чтобы получить дополнительные сведения о структурировании этого объекта, перейдите к следующему разделу.

> [!TIP]
> После создания или обновления двойника может возникнуть задержка до 10 секунд, прежде чем изменения будут отражены в [запросах](how-to-query-graph.md). В `GetDigitalTwin` API (описанном [Далее в этой статье](#get-data-for-a-digital-twin)) Эта задержка не возникает, поэтому используйте вызов API вместо запроса, чтобы увидеть недавно созданный двойников, если требуется мгновенный ответ. 

### <a name="initialize-model-and-properties"></a>Инициализация модели и свойств

API создания двойника принимает объект, который сериализуется в допустимое описание JSON свойств двойника. Описание формата JSON для двойника см. [*в статье основные понятия: Digital двойников и двойника Graph*](concepts-twins-graph.md) . 

Сначала необходимо создать объект данных, который будет представлять двойника и данные его свойств. Затем можно использовать `JsonSerializer` для передачи сериализованной версии этого объекта в вызов API для `initdata` параметра.

Вы можете создать объект параметра либо вручную, либо с помощью предоставленного вспомогательного класса. Ниже приведен пример каждого из них.

#### <a name="create-twins-using-manually-created-data"></a>Создание двойников с помощью данных, созданных вручную

Без использования каких-либо настраиваемых вспомогательных классов можно представить свойства двойника в `Dictionary<string, object>` , где `string` — это имя свойства, а `object` — объект, представляющий свойство и его значение.

[!INCLUDE [Azure Digital Twins code: create twin](../../includes/digital-twins-code-create-twin.md)]

#### <a name="create-twins-with-the-helper-class"></a>Создание двойников с помощью вспомогательного класса

Вспомогательный класс служб `BasicDigitalTwin` позволяет более точно хранить поля свойств в объекте "двойника". Вы по-прежнему можете создать список свойств с помощью `Dictionary<string, object>` , который затем можно добавить в объект двойника в качестве `CustomProperties` непосредственного объекта.

```csharp
BasicDigitalTwin twin = new BasicDigitalTwin();
twin.Metadata = new DigitalTwinMetadata();
twin.Metadata.ModelId = "dtmi:example:Room;1";
// Initialize properties
Dictionary<string, object> props = new Dictionary<string, object>();
props.Add("Temperature", 25.0);
props.Add("Humidity", 50.0);
twin.CustomProperties = props;

client.CreateDigitalTwin("myNewRoomID", JsonSerializer.Serialize<BasicDigitalTwin>(twin));
```

>[!NOTE]
> `BasicDigitalTwin` объекты поступают с `Id` полем. Это поле можно оставить пустым, но если добавить значение идентификатора, оно должно соответствовать параметру идентификатора, переданному в `CreateDigitalTwin` вызов. В приведенном выше примере это будет выглядеть следующим образом:
>
>```csharp
>twin.Id = "myNewRoomID";
>```

## <a name="get-data-for-a-digital-twin"></a>Получение данных для цифрового двойника

Вы можете получить доступ ко всем данным любого цифрового двойника, вызвав:

```csharp
object result = await client.GetDigitalTwin(id);
```

Этот вызов возвращает двойника данные в виде строки JSON. 

При получении двойника с помощью возвращаются только те свойства, которые были установлены хотя бы один раз `GetDigitalTwin` .

>[!TIP]
>`displayName`Для двойника является частью метаданных модели, поэтому она не будет отображаться при получении данных для экземпляра двойника. Чтобы увидеть это значение, можно [извлечь его из модели](how-to-manage-model.md#retrieve-models).

Чтобы получить несколько двойников с помощью одного вызова API, ознакомьтесь с примерами API запросов в разделе [*инструкции. запрос к графу двойника*](how-to-query-graph.md).

Рассмотрим следующую модель (написанную на [языке определения цифровых двойников (дтдл)](https://github.com/Azure/opendigitaltwins-dtdl/tree/master/DTDL)), которая определяет *Луна*:

```json
{
    "@id": " dtmi:com:contoso:Moon;1",
    "@type": "Interface",
    "@context": "dtmi:dtdl:context;2",
    "contents": [
        {
            "@type": "Property",
            "name": "radius",
            "schema": "double",
            "writable": true
        },
        {
            "@type": "Property",
            "name": "mass",
            "schema": "double",
            "writable": true
        }
    ]
}
```

Результат вызова `object result = await client.DigitalTwins.GetByIdAsync("my-moon");` для типа *Луны*двойника может выглядеть следующим образом:

```json
{
  "$dtId": "myMoon-001",
  "$etag": "W/\"e59ce8f5-03c0-4356-aea9-249ecbdc07f9\"",
  "radius": 1737.1,
  "mass": 0.0734,
  "$metadata": {
    "$model": "dtmi:com:contoso:Moon;1",
    "radius": {
      "desiredValue": 1737.1,
      "desiredVersion": 5,
      "ackVersion": 4,
      "ackCode": 200,
      "ackDescription": "OK"
    },
    "mass": {
      "desiredValue": 0.0734,
      "desiredVersion": 8,
      "ackVersion": 8,
      "ackCode": 200,
      "ackDescription": "OK"
    }
  }
}
```

Определенные свойства цифровых двойника возвращаются в виде свойств верхнего уровня в цифровом двойника. Метаданные или системные сведения, не входящие в определение ДТДЛ, возвращаются с `$` префиксом. Свойства метаданных включают:
* Идентификатор цифрового двойника в этом экземпляре Azure Digital двойников, например `$dtId` .
* `$etag`, стандартное поле HTTP, назначенное веб-сервером
* Другие свойства в `$metadata` разделе. Сюда входит следующее.
    - ДТМИ модели цифрового двойника.
    - Состояние синхронизации для каждого записываемого свойства. Это наиболее полезно для устройств, где служба и устройство могут иметь состояние рассогласования (например, если устройство находится в автономном режиме). В настоящее время это свойство применяется только к физическим устройствам, подключенным к центру Интернета вещей. С данными в разделе метаданных можно понять полное состояние свойства, а также отметку времени последнего изменения. Дополнительные сведения о состоянии синхронизации см. в [этом руководстве центра Интернета вещей](../iot-hub/tutorial-device-twins.md) по синхронизации состояния устройства.
    - Зависящие от службы метаданные, например из центра Интернета вещей или Azure Digital двойников. 

Вы можете проанализировать возвращенный JSON для двойника, используя библиотеку анализа JSON по своему усмотрению, например `System.Text.Json` .

Можно также использовать вспомогательный класс сериализации `BasicDigitalTwin` , включенный в пакет SDK, который возвращает основные метаданные и свойства двойника в предварительно проанализированной форме. Например:

```csharp
Response<string> res = client.GetDigitalTwin(twin_id);
BasicDigitalTwin twin = JsonSerializer.Deserialize<BasicDigitalTwin>(res.Value);
Console.WriteLine($"Model id: {twin.Metadata.ModelId}");
foreach (string prop in twin.CustomProperties.Keys)
{
    if (twin.CustomProperties.TryGetValue(prop, out object value))
        Console.WriteLine($"Property '{prop}': {value}");
}
```

Дополнительные сведения о вспомогательных классах сериализации см. в статье [*Использование интерфейсов API и пакетов SDK для цифровых двойников Azure*](how-to-use-apis-sdks.md).

## <a name="update-a-digital-twin"></a>Обновление цифрового двойника

Чтобы обновить свойства цифровой двойника, необходимо записать сведения, которые необходимо заменить в формате [исправления JSON](http://jsonpatch.com/) . Таким образом можно заменить несколько свойств одновременно. Затем вы передаете документ исправления JSON в `Update` метод:

```csharp
await client.UpdateDigitalTwin(id, patch);
```

Вызов Patch может обновлять столько свойств одного двойника, сколько необходимо (даже все они). Если необходимо обновить свойства в нескольких двойников, потребуется отдельный вызов Update для каждого двойника.

> [!TIP]
> После создания или обновления двойника может возникнуть задержка до 10 секунд, прежде чем изменения будут отражены в [запросах](how-to-query-graph.md). В `GetDigitalTwin` API (описанном [ранее в этой статье](#get-data-for-a-digital-twin)) Эта задержка не возникает, поэтому используйте вызов API вместо запроса, чтобы увидеть обновленный двойников, если требуется мгновенный ответ. 

Ниже приведен пример кода исправления JSON. Этот документ заменяет значения свойств *масса* и *радиус* цифрового двойника, к которому он применяется.

```json
[
  {
    "op": "replace",
    "path": "/mass",
    "value": 0.0799
  },
  {
    "op": "replace",
    "path": "/radius",
    "value": 0.800
  }
]
```

Вы можете создавать исправления вручную или с помощью вспомогательного класса сериализации в [пакете SDK](how-to-use-apis-sdks.md). Ниже приведен пример каждого из них.

#### <a name="create-patches-manually"></a>Создание исправлений вручную

```csharp
List<object> twinData = new List<object>();
twinData.Add(new Dictionary<string, object>() {
    { "op", "add"},
    { "path", "/Temperature"},
    { "value", 25.0}
});

await client.UpdateDigitalTwinAsync(twinId, JsonConvert.SerializeObject(twinData));
```

#### <a name="create-patches-using-the-helper-class"></a>Создание исправлений с помощью вспомогательного класса

```csharp
UpdateOperationsUtility uou = new UpdateOperationsUtility();
uou.AppendAddOp("/Temperature", 25.0);
await client.UpdateDigitalTwinAsync(twinId, uou.Serialize());
```

### <a name="update-properties-in-digital-twin-components"></a>Обновление свойств в компонентах Digital двойника

Помните, что модель может содержать компоненты, что позволяет состоять из других моделей. 

Чтобы исправить свойства в компонентах Digital двойника, вы будете использовать синтаксис пути в исправлении JSON:

```json
[
  {
    "op": "replace",
    "path": "/mycomponentname/mass",
    "value": 0.0799
  }
]
```

### <a name="update-a-digital-twins-model"></a>Обновление модели цифрового двойника

`Update`Функция также может использоваться для переноса цифрового двойника в другую модель. 

Например, рассмотрим следующий документ с исправлением JSON, который заменяет поле метаданных Digital двойника `$model` :

```json
[
  {
    "op": "replace",
    "path": "/$metadata/$model",
    "value": "dtmi:com:contoso:foo;1"
  }
]
```

Эта операция будет выполнена только в том случае, если двойника, измененный исправлением, соответствует новой модели. 

Рассмотрим следующий пример.
1. Представьте себе цифровой двойника с моделью *foo_old*. *foo_old* определяет требуемое свойство *массы*.
2. Новая модель *foo_new* определяет масса свойств и добавляет новую *температуру*требуемого свойства.
3. После установки исправления цифровое двойника должно иметь свойство масса и температура. 

Исправление для этой ситуации должно обновить свойство двойника и Model, следующим образом:

```json
[
  {
    "op": "replace",
    "path": "$metadata.$model",
    "value": "dtmi:com:contoso:foo_new"
  },
  {
    "op": "add",
    "path": "temperature",
    "value": 60
  }
]
```

### <a name="handle-conflicting-update-calls"></a>Обработку конфликтующих вызовов обновления

Azure Digital двойников гарантирует, что все входящие запросы обрабатываются один за другим. Это означает, что даже если несколько функций пытаются одновременно обновить одно и то же свойство в двойника, **нет необходимости** писать явный код блокировки для обработки конфликта.

Это поведение применяется для каждого двойника. 

В качестве примера представьте себе ситуацию, в которой эти три вызова поступают в одно и то же время: 
*   Запись свойства A в *Twin1*
*   Запись свойства B на *Twin1*
*   Запись свойства A в *Twin2*

Два вызова, которые изменяют *Twin1* , выполняются один за другим, а сообщения об изменениях формируются для каждого изменения. Вызов функции Modify *Twin2* может выполняться параллельно без конфликта, как только он поступает.

## <a name="delete-a-digital-twin"></a>Удаление цифрового двойника

Двойников можно удалить с помощью `DeleteDigitalTwin(ID)` . Однако можно удалить только двойника, если он не имеет больше связей. Сначала необходимо удалить все связи. 

Ниже приведен пример кода для этого:

```csharp
static async Task DeleteTwin(string id)
{
    await FindAndDeleteOutgoingRelationshipsAsync(id);
    await FindAndDeleteIncomingRelationshipsAsync(id);
    try
    {
        await client.DeleteDigitalTwin(id);
    } catch (RequestFailedException exc)
    {
        Console.WriteLine($"*** Error:{exc.Error}/{exc.Message}");
    }
}

public async Task FindAndDeleteOutgoingRelationshipsAsync(string dtId)
{
    // Find the relationships for the twin

    try
    {
        // GetRelationshipsAsync will throw an error if a problem occurs
        AsyncPageable<string> relsJson = client.GetRelationshipsAsync(dtId);

        await foreach (string relJson in relsJson)
        {
            var rel = System.Text.Json.JsonSerializer.Deserialize<BasicRelationship>(relJson);
            await client.DeleteRelationshipAsync(dtId, rel.Id).ConfigureAwait(false);
            Log.Ok($"Deleted relationship {rel.Id} from {dtId}");
        }
    }
    catch (RequestFailedException ex)
    {
        Log.Error($"*** Error {ex.Status}/{ex.ErrorCode} retrieving or deleting relationships for {dtId} due to {ex.Message}");
    }
}

async Task FindAndDeleteIncomingRelationshipsAsync(string dtId)
{
    // Find the relationships for the twin

    try
    {
        // GetRelationshipssAsync will throw an error if a problem occurs
        AsyncPageable<IncomingRelationship> incomingRels = client.GetIncomingRelationshipsAsync(dtId);

        await foreach (IncomingRelationship incomingRel in incomingRels)
        {
            await client.DeleteRelationshipAsync(incomingRel.SourceId, incomingRel.RelationshipId).ConfigureAwait(false);
            Log.Ok($"Deleted incoming relationship {incomingRel.RelationshipId} from {dtId}");
        }
    }
    catch (RequestFailedException ex)
    {
        Log.Error($"*** Error {ex.Status}/{ex.ErrorCode} retrieving or deleting incoming relationships for {dtId} due to {ex.Message}");
    }
}
```

### <a name="delete-all-digital-twins"></a>Удалить все цифровые двойников

Пример того, как удалить все двойников одновременно, можно скачать с помощью примера приложения, используемого в [*учебнике. Изучите основные сведения с примером клиентского приложения*](tutorial-command-line-app.md). Файл *CommandLoop.CS* делает это в `CommandDeleteAllTwins` функции.

## <a name="manage-twins-with-cli"></a>Управление двойников с помощью интерфейса командной строки

Двойников также можно управлять с помощью цифрового интерфейса командной строки Azure двойников. Команды можно найти в [*этом пошаговом окне. Используйте интерфейс командной строки Azure Digital двойников*](how-to-use-cli.md).

[!INCLUDE [digital-twins-known-issue-cloud-shell](../../includes/digital-twins-known-issue-cloud-shell.md)]

## <a name="view-all-digital-twins"></a>Просмотреть все цифровые двойников

Чтобы просмотреть все цифровые двойников в вашем экземпляре, используйте [запрос](how-to-query-graph.md). Можно выполнить запрос с помощью [API запросов](how-to-use-apis-sdks.md) или [команд интерфейса командной строки](how-to-use-cli.md).

Ниже приведен основной текст запроса, который вернет список всех цифровых двойников в экземпляре:

```sql
SELECT *
FROM DIGITALTWINS
``` 

## <a name="next-steps"></a>Дальнейшие действия

Узнайте, как создавать связи между цифровым двойников и управлять ими:
* [*Руководство. Управление графом двойника с помощью связей*](how-to-manage-graph.md)