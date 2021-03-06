---
title: Анализ тональности — LUIS
titleSuffix: Azure Cognitive Services
description: Если настроен анализ тональности, он входит в ответ JSON LUIS.
services: cognitive-services
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: reference
ms.date: 07/01/2020
ms.openlocfilehash: 7c3c0f603b9e7d83e40f43ab00d31f4ca63f2b4f
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2020
ms.locfileid: "91535378"
---
# <a name="sentiment-analysis"></a>Анализ мнений
Если настроен анализ тональности, он входит в ответ JSON LUIS. Дополнительные сведения об анализе тональности см. в документации по [анализу текста](https://docs.microsoft.com/azure/cognitive-services/text-analytics/).

LUIS использует Анализ текста v2. 

## <a name="resolution-for-sentiment"></a>Решение для тональности

Данные тональности представляют собой оценку между 1 и 0, означающую положительную (ближе к 1) или отрицательную (ближе к 0) тональность данных.

#### <a name="english-language"></a>[Английский язык](#tab/english)

При использовании языка и региональных параметров `en-us` выводится следующий ответ:

```JSON
"sentimentAnalysis": {
  "label": "positive",
  "score": 0.9163064
}
```

#### <a name="other-languages"></a>[Другие языки](#tab/other-languages)

Для всех других языков и региональных параметров выводится ответ:

```JSON
"sentimentAnalysis": {
  "score": 0.9163064
}
```
* * *

## <a name="next-steps"></a>Дальнейшие шаги

Дополнительные сведения о [конечной точке прогнозирования V3](luis-migration-api-v3.md).

