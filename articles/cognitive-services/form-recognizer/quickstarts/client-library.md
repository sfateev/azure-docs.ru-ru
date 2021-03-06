---
title: Краткое руководство. Клиентская библиотека Распознавателя документов
titleSuffix: Azure Cognitive Services
description: Узнайте, как использовать клиентскую библиотеку Распознавателя документов для создания приложения для обработки форм, которое извлекает из пользовательских документов пары "ключ — значение" и табличные данные.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: forms-recognizer
ms.topic: quickstart
ms.date: 09/21/2020
ms.author: pafarley
zone_pivot_groups: programming-languages-set-ten
ms.custom: devx-track-js, devx-track-csharp, cog-serv-seo-aug-2020
keywords: Обработка форм, автоматизированная обработка данных
ms.openlocfilehash: eb3fc3ebe5c3d9f659758a14ba620018671d95f5
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/25/2020
ms.locfileid: "91318947"
---
# <a name="quickstart-use-the-form-recognizer-client-library"></a>Краткое руководство. Использование клиентской библиотеки Распознавателя документов

Начните работу с клиентской библиотекой службы "Распознаватель документов" на языке по вашему выбору. Распознаватель документов Azure — это служба Cognitive Service, которая позволяет создавать автоматизированное программное обеспечение для обработки данных с помощью технологии машинного обучения. Эта служба идентифицирует и извлекает пары "ключ — значение" и табличные данные из документов с формами &mdash; она выводит структурированные данные, включая связи в исходном файле. Выполните приведенные здесь действия, чтобы установить пакет SDK и протестировать пример кода для выполнения базовых задач.

Используйте клиентскую библиотеку Распознавателя документов для следующих целей:

* [распознавание содержимого формы;](#recognize-form-content)
* [распознавание квитанций](#recognize-receipts);
* [обучение пользовательской модели](#train-a-custom-model);
* [анализ документов с помощью пользовательской модели](#analyze-forms-with-a-custom-model);
* [управление пользовательскими моделями.](#manage-your-custom-models)

::: zone pivot="programming-language-csharp"

[!INCLUDE [C# SDK quickstart](../includes/quickstarts/csharp-sdk.md)]

::: zone-end

::: zone pivot="programming-language-java"

[!INCLUDE [Java SDK quickstart](../includes/quickstarts/java-sdk.md)]

::: zone-end

::: zone pivot="programming-language-javascript"

[!INCLUDE [NodeJS SDK quickstart](../includes/quickstarts/javascript-sdk.md)]

::: zone-end

::: zone pivot="programming-language-python"

[!INCLUDE [Python SDK quickstart](../includes/quickstarts/python-sdk.md)]

::: zone-end