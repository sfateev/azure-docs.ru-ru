---
title: Руководством по миграции для версии 2 API анализа текста
titleSuffix: Azure Cognitive Services
description: Узнайте, как переместить приложения для использования версии 3 API анализа текста.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: article
ms.date: 06/25/2020
ms.author: aahi
ms.openlocfilehash: 12c09ad8e1db3914263fcc864c9c2d09069d63a6
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2020
ms.locfileid: "85412589"
---
# <a name="migrate-to-version-3x-of-the-text-analytics-api"></a>Переход на версию 3. x API анализа текста

[!INCLUDE [v3 region availability](includes/v3-region-availability.md)]

Если вы используете версию 2,1 API анализа текста, эта статья поможет вам обновить приложение для использования версии 3. x. Версия 3,0 является общедоступной и содержит новые функции, такие как расширенное [Распознавание сущностей (NER)](how-tos/text-analytics-how-to-entity-linking.md#named-entity-recognition-versions-and-features) и [Управление версиями модели](concepts/model-versioning.md). Также доступна предварительная версия версии 3.1 (версия 3.1-Preview. x), которая добавляет такие функции, как [интеллектуальный анализ данных об мнениях](how-tos/text-analytics-how-to-sentiment-analysis.md#sentiment-analysis-versions-and-features). Модели, используемые в версии 2, не получат будущие обновления. 

#### <a name="sentiment-analysis"></a>[Пример. Как определить тональность с помощью Анализа текста](#tab/sentiment-analysis)

## <a name="feature-changes"></a>Изменения функций 

Анализ тональности в версии 2,1 возвращает баллы тональности в диапазоне от 0 до 1 для каждого документа, отправленного в API, с оценками ближе к 1, что означает более положительный тональности. Версия 3 вместо этого возвращает метки тональности (например, "положительный" или "отрицательный") как для предложений, так и для документа в целом и связанных с ними оценок достоверности. 

## <a name="steps-to-migrate"></a>Шаги для миграции

### <a name="rest-api"></a>REST API

Если приложение использует REST API, обновите конечную точку запроса до конечной точки v3 для анализа тональности. Например: `https://<your-custom-subdomain>.cognitiveservices.azure.com/text/analytics/v3.0/sentiment` . Кроме того, необходимо обновить приложение, чтобы использовать метки тональности, возвращаемые в [ответе JSON](how-tos/text-analytics-how-to-sentiment-analysis.md#view-the-results). 

### <a name="client-libraries"></a>Клиентские библиотеки

[!INCLUDE [Client library migration information](includes/client-library-migration-section.md)]

#### <a name="ner-and-entity-linking"></a>[NER и связывание сущностей](#tab/named-entity-recognition)

## <a name="feature-changes"></a>Изменения функций

> [!NOTE] 
> В настоящее время [категории сущностей v3](named-entity-types.md) возвращаются только в тексте на английском и испанском языках. API возвращает версии 2,1 для запросов на других языках, если они поддерживаются в версии 2,1.

В версии 2,1 API анализа текста использует одну конечную точку для распознавания именованных сущностей (NER) и связывания сущностей. Версия 3 предоставляет Расширенное обнаружение именованных сущностей и использует отдельные конечные точки для запросов NER и связывания сущностей. Начиная с версии 3.1 – Preview. 1, NER может дополнительно определять персональные `pii` данные и `phi` сведения о работоспособности. 

## <a name="steps-to-migrate"></a>Шаги для миграции

### <a name="rest-api"></a>REST API

Если приложение использует REST API, обновите конечную точку запроса до конечных точек v3 для связывания NER и (или) сущности.

Связывание сущностей
* `https://<your-custom-subdomain>.cognitiveservices.azure.com/text/analytics/v3.0/entities/linking`

NER
* `https://<your-custom-subdomain>.cognitiveservices.azure.com/text/analytics/v3.0/entities/recognition/general`

Также потребуется обновить приложение, чтобы использовать [категории сущностей](named-entity-types.md) , возвращенные в [ответе JSON](how-tos/text-analytics-how-to-entity-linking.md#view-results).

### <a name="client-libraries"></a>Клиентские библиотеки

[!INCLUDE [Client library migration information](includes/client-library-migration-section.md)]


#### <a name="language-detection"></a>[Пример. Как определить язык с помощью Анализа текста](#tab/language-detection)

## <a name="feature-changes"></a>Изменения функций 

Функция обнаружения языка не изменилась в версии v3 за пределами конечной точки, но ответ JSON будет содержать `ConfidenceScore` вместо `score` . V3 также возвращает только один язык в выходных данных. 

## <a name="steps-to-migrate"></a>Шаги для миграции

### <a name="rest-api"></a>REST API

Если приложение использует REST API, обновите конечную точку запроса до конечной точки v3 для определения языка. Например: `https://<your-custom-subdomain>.cognitiveservices.azure.com/text/analytics/v3.0/languages` . Также потребуется обновить приложение для использования `ConfidenceScore` вместо `score` в [ответе JSON](how-tos/text-analytics-how-to-language-detection.md#step-3-view-the-results). 

### <a name="client-libraries"></a>Клиентские библиотеки

[!INCLUDE [Client library migration information](includes/client-library-migration-section.md)]


#### <a name="key-phrase-extraction"></a>[Пример. Как извлечь ключевые фразы с помощью Анализа текста](#tab/key-phrase-extraction)

## <a name="feature-changes"></a>Изменения функций 

Функция извлечения ключевых фраз не изменилась в версии v3 за пределами конечной точки.

## <a name="steps-to-migrate"></a>Шаги для миграции

### <a name="rest-api"></a>REST API

Если приложение использует REST API, обновите конечную точку запроса до конечной точки v3 для извлечения ключевых фраз. Например: `https://<your-custom-subdomain>.api.cognitiveservices.azure.com/text/analytics/v3.0/keyPhrases`

### <a name="client-libraries"></a>Клиентские библиотеки

[!INCLUDE [Client library migration information](includes/client-library-migration-section.md)]

---


## <a name="see-also"></a>См. также

* [Справочник по API анализа текста v2](https://westcentralus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v2-1/)
* [Что такое API "Анализ текста"?](overview.md)
* [Поддержка языков](language-support.md)
* [управления версиями моделей;](concepts/model-versioning.md)