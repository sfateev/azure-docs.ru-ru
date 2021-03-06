---
title: Основные понятия проверок, рабочих процессов и заданий — Content Moderator
titleSuffix: Azure Cognitive Services
description: В этой статье вы узнаете об основных понятиях средства проверки. обзоры, рабочие процессы и задания.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: content-moderator
ms.topic: conceptual
ms.date: 03/14/2019
ms.author: pafarley
ms.openlocfilehash: 1aba86efb9ea76fbf060e80b47f9f2f6cdf8ee71
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2020
ms.locfileid: "91872057"
---
# <a name="content-moderation-reviews-workflows-and-jobs"></a>Обзоры, рабочие процессы и задания для контроля содержимого

Content Moderator сочетает ролевую поддержку с возможностями "от человека", чтобы создать оптимальный процесс контроля для реальных сценариев. Это делается с помощью облачного [средства проверки](https://contentmoderator.cognitive.microsoft.com). В этом руководство вы узнаете об основных понятиях средства проверки: обзоры, рабочие процессы и задания.

## <a name="reviews"></a>Проверки

В ходе проверки содержимое загружается в средство проверки и отображается на вкладке " **Проверка** ". Отсюда пользователи могут изменять примененные Теги и применять собственные пользовательские теги. Когда пользователь отправляет отзыв, результаты отправляются в указанную конечную точку обратного вызова, а содержимое удаляется с сайта.

![Открыть веб-сайт средства проверки в браузере на вкладке "Проверка"](./Review-Tool-user-Guide/images/image-workflow-review.png)

Дополнительные сведения о том, как это сделать, см. в разделе [руководство по ознакомлению с руководством](./review-tool-user-guide/review-moderated-images.md) по [REST API](./try-review-api-review.md) .

## <a name="workflows"></a>Рабочие процессы

Рабочий процесс — это настроенный в облаке настраиваемый фильтр содержимого. Рабочие процессы могут подключаться к различным службам для фильтрации содержимого различными способами, а затем предпринимать соответствующие действия. С помощью соединителя Content Moderator рабочий процесс может автоматически применять теги контроля и создавать рецензии с помощью отправленного содержимого.

### <a name="view-workflows"></a>Просмотр рабочих процессов

Чтобы просмотреть существующие рабочие процессы, перейдите к [средству проверки](https://contentmoderator.cognitive.microsoft.com/) и выберите **Параметры**  >  **рабочие процессы**.

![Рабочий процесс по умолчанию](images/default-workflow-listed.PNG)

Рабочие процессы можно полностью описать как строки JSON, что делает их доступными программно. Если выбрать параметр **изменить** для рабочего процесса, а затем выбрать вкладку **JSON** , отобразится выражение JSON следующего вида:

```json
{
    "Type": "Logic",
    "If": {
        "ConnectorName": "moderator",
        "OutputName": "isAdult",
        "Operator": "eq",
        "Value": "true",
        "Type": "Condition"
        },
    "Then": {
    "Perform": [
    {
        "Name": "createreview",
        "CallbackEndpoint": null,
        "Tags": []
    }
    ],
    "Type": "Actions"
    }
}
```

[Чтобы](./review-tool-user-guide/workflows.md) приступить к созданию и использованию рабочих процессов, см. руководство по [REST API](./try-review-api-workflow.md) , чтобы узнать, как это сделать программным способом.

## <a name="jobs"></a>Задания

Задание по созадаче служит разновидностью оболочки для функций контроля содержимого, рабочих процессов и проверок. Задание сканирует содержимое с помощью Content Moderator API контроля изображений или API-интерфейса контроля текста, а затем проверяет его на назначенный рабочий процесс. На основе результатов рабочего процесса он может или не может создать проверку содержимого в [средстве проверки](./review-tool-user-guide/human-in-the-loop.md). Хотя проверки и рабочие процессы можно создавать и настраивать с помощью соответствующих API-интерфейсов, API задания позволяет получить подробный отчет по всему процессу (который можно отправить в указанную конечную точку обратного вызова).

Чтобы приступить к работе с заданиями, ознакомьтесь с [руководством по REST API](./try-review-api-job.md) .

## <a name="next-steps"></a>Дальнейшие действия

* Проверьте в работе [консоль API заданий](try-review-api-job.md) и примеры кода для REST API. Также изучите [краткое руководство по работе с заданиями с помощью .NET](moderation-jobs-quickstart-dotnet.md), если вы уже знакомы с Visual Studio и C#. 
* Чтобы использовать проверки, начните работу с [консолью API проверки](try-review-api-review.md) и воспользуйтесь примерами кода для REST API. Затем см. раздел "обзоры" в [кратком руководстве по .NET](dotnet-sdk-quickstart.md).
* Чтобы использовать проверки видео, изучите [краткое руководство по проверке видео](video-reviews-quickstart-dotnet.md) и узнайте, как [добавить расшифровки в проверку видео](video-transcript-reviews-quickstart-dotnet.md).
