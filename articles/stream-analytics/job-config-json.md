---
title: Azure Stream Analytics JobConfig.jsв полях
description: В этой статье перечислены поддерживаемые поля Azure Stream Analytics JobConfig.jsв файле, используемом для создания заданий в Visual Studio Code.
author: mamccrea
ms.author: mamccrea
ms.service: stream-analytics
ms.topic: how-to
ms.date: 02/14/2020
ms.openlocfilehash: f2dd759203655746601699f665436c78ee0758f8
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2020
ms.locfileid: "90885505"
---
# <a name="azure-stream-analytics-jobconfigjson-fields"></a>Azure Stream Analytics JobConfig.jsв полях

В *JobConfig.jsв* файле, используемом для [создания задания Azure Stream Analytics с помощью Visual Studio Code](quick-create-visual-studio-code.md), поддерживаются следующие поля.

```json
{
    "DataLocale": "string",
    "OutputErrorPolicy": "string",
    "EventsLateArrivalMaxDelayInSeconds": "integer",
    "EventsOutOfOrderMaxDelayInSeconds": "integer",
    "EventsOutOfOrderPolicy": "string",
    "StreamingUnits": "integer",
    "CompatibilityLevel": "string",
    "UseSystemAssignedIdentity": "boolean",
    "GlobalStorage": {
        "AccountName": "string",
        "AccountKey": "string",
    },
    "DataSourceCredentialDomain": "string",
    "ScriptType": "string",
    "Tags": {}
}
```

|Имя|Тип|Обязательно|Значение|
|----|----|--------|-----|
|Локальное|Строка|Нет|Языковой стандарт данных задания Stream Analytics. Значение должно быть именем поддерживаемого. Если значение не указано, по умолчанию используется "en-US".|
|аутпутеррорполици|Строка|Нет|Указывает политику, применяемую к событиям, которые поступают на выходные данные и не могут быть записаны во внешнее хранилище из-за неправильного формата (отсутствующие значения столбцов, значения столбцов неправильного типа или размера). -Останавливаться или Drop|
|евентслатеарривалмаксделайинсекондс|Целое число|Нет|Максимальная приемлемой задержка в секундах, в течение которой события, поступающие в конце, могут быть добавлены. Поддерживаемый диапазон — от-1 до 1814399 (20.23:59:59 дней) и-1 используется для указания неограниченного времени ожидания. Если свойство отсутствует, оно интерпретируется как значение-1.|
|евентсаутофордермаксделайинсекондс|Целое число|Нет|Максимальная приемлемой задержка в секундах, в течение которой события, находящиеся вне порядка, могут быть скорректированы по порядку.|
|евентсаутофордерполици|Строка|Нет|Указывает политику, применяемую к событиям, которые прибывают в потоке входных событий в неупорядоченном виде. -Изменить или удалить|
|стреамингунитс|Целое число|Да|Указывает количество единиц потоковой передачи, используемых заданием потоковой передачи.|
|CompatibilityLevel|Строка|Нет|Управляет определенными поведениями среды выполнения для задания потоковой передачи. — Допустимые значения: "1,0", "1,1", "1,2"|
|усесистемассигнедидентити|Логическое|Нет|Установите значение true, чтобы разрешить этому заданию взаимодействовать с другими службами Azure, используя управляемое удостоверение Azure Active Directory.|
|Глобалстораже. AccountName|Строка|Нет|Глобальная учетная запись хранения используется для хранения содержимого, связанного с заданием Stream Analytics, например для моментальных снимков ссылочных данных SQL.|
|Глобалстораже. AccountKey|Строка|Нет|Соответствующий ключ для глобальной учетной записи хранения.|
|датасаурцекредентиалдомаин|Строка|Нет|Зарезервированное свойство для локального хранилища учетных данных.|
|ScriptType|строка|Да|Зарезервированное свойство для указанного типа этого исходного файла. Допустимое значение — "Жобконфиг" для JobConfig.js.|
|Теги|Пары "ключ-значение" JSON|Нет|Теги — это пары имя — значение, которые можно назначать различным ресурсам и группам ресурсов для их категоризации и консолидированного отображения счетов. В именах тегов не учитывается регистр, а в значениях тегов учитывается регистр.|

## <a name="next-steps"></a>Дальнейшие шаги

* [Создание задания Azure Stream Analytics в Visual Studio Code](quick-create-visual-studio-code.md)
* [Локальное тестирование запросов Stream Analytics с использованием примера данных и Visual Studio Code](visual-studio-code-local-run.md)
* [Тестирование Stream Analytics запросов локально для входа в динамический поток с помощью Visual Studio Code](visual-studio-code-local-run-live-input.md) 
* [Развертывание задания Azure Stream Analytics с помощью пакета CI/CD NPM](setup-cicd-vs-code.md)