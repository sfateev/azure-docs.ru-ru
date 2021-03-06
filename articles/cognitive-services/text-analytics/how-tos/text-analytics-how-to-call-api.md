---
title: Вызов API "Анализ текста"
titleSuffix: Azure Cognitive Services
description: В этой статье объясняется, как можно вызвать Анализ текста Azure Cognitive Services REST API и POST.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: conceptual
ms.date: 07/30/2019
ms.author: aahi
ms.openlocfilehash: e17f2015ed4428cfd3c1a6c8a7bc4f92854a6b71
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2020
ms.locfileid: "91710606"
---
# <a name="how-to-call-the-text-analytics-rest-api"></a>Как вызвать REST API службы "Анализ текста"

К **API анализа текста** отправляются вызовы HTTP POST или GET, которые можно сформулировать на любом языке. В этой статье для демонстрации основных концепций используется REST и [Postman](https://www.postman.com/downloads/).

Каждый запрос должен содержать ключ доступа и конечную точку HTTP. Конечная точка определяет регион, который вы выбираете во время регистрации, URL-адрес службы и ресурс, используемый в запросе: `sentiment`, `keyphrases`, `languages` и `entities`. 

Напомним, что служба "Анализ текста" не учитывает состояние, поэтому ресурсы данных, которыми необходимо управлять, отсутствуют. Текст будет отправлен и проанализирован после получения, а результаты немедленно будут возвращены в вызывающее приложение.

[!INCLUDE [text-analytics-api-references](../includes/text-analytics-api-references.md)]

[!INCLUDE [v3 region availability](../includes/v3-region-availability.md)]

## <a name="prerequisites"></a>Предварительные требования

[!INCLUDE [cognitive-services-text-analytics-signup-requirements](../../../../includes/cognitive-services-text-analytics-signup-requirements.md)]

<a name="json-schema"></a>

## <a name="json-schema-definition"></a>Определение схемы JSON

Входные данные должны быть в виде необработанного неструктурированного текста в формате JSON. XML не поддерживается. Схема проста и состоит из элементов, описанных в приведенном ниже списке. 

В настоящее время можно отправить те же документы для всех операций службы "Анализ текста": определение тональности, ключевых фраз, распознавание языка и идентификация сущности. (Схема может отличаться для каждого будущего анализа.)

| Элемент | Допустимые значения | Необходим? | Использование |
|---------|--------------|-----------|-------|
|`id` |Типом данных выступает строка, однако обычно идентификаторы документов являются целыми числами. | Обязательно | Система использует предоставленные идентификаторы, чтобы структурировать выходные данные. Коды языков, ключевые фразы и оценка тональности создаются для каждого идентификатора в запросе.|
|`text` | Неструктурированный необработанный текст, не более 5 120 символов. | Обязательно | Для распознавания языка текст может быть выражен на любом языке. Для анализа тональности, извлечения ключевых фраз и идентификации сущности текст должен быть на [поддерживаемом языке](../text-analytics-supported-languages.md). |
|`language` | Двухзначный код [ISO 639-1](https://en.wikipedia.org/wiki/List_of_ISO_639-1_codes) для [поддерживаемого языка](../text-analytics-supported-languages.md). | Различается | Требуется для анализа тональности, извлечения ключевых фраз и связывания сущностей. Необязательно для распознавания языка. Код можно исключить, и это не будет ошибкой, однако без него анализ будет менее точным. Код языка должен соответствовать предоставленному вами элементу `text`. |

Дополнительные сведения см. в разделе [Ограничения данных](../overview.md#data-limits). 


```json
{
  "documents": [
    {
      "language": "en",
      "id": "1",
      "text": "Sample text to be sent to the text analytics api."
    },
    {
      "language": "en",
      "id": "2",
      "text": "It's incredibly sunny outside! I'm so happy."
    },
    {
      "language": "en",
      "id": "3",
      "text": "Pike place market is my favorite Seattle attraction."
    }
  ]
}
```


## <a name="set-up-a-request-in-postman"></a>Настройка запроса в Postman

Служба принимает запрос размером до 1 МБ. Если вы используете Postman (или другой инструмент тестирования для веб-API), настройте конечную точку так, чтобы она содержала нужный ресурс, и предоставьте ключ доступа в заголовке запроса. Каждая операция требует, чтобы вы добавляли соответствующий ресурс в конечную точку. 

1. В Postman:

   + выберите **Post** в качестве типа запроса;
   + вставьте конечную точку, скопированную со страницы портала;
   + добавьте ресурс.

   Конечные точки ресурсов имеют такой вид (ваш регион может отличаться):

   + `https://westus.api.cognitive.microsoft.com/text/analytics/v3.0/sentiment`
   + `https://westus.api.cognitive.microsoft.com/text/analytics/v3.0/keyPhrases`
   + `https://westus.api.cognitive.microsoft.com/text/analytics/v3.0/languages`
   + `https://westus.api.cognitive.microsoft.com/text/analytics/v3.0/entities/recognition/general`

2. Задайте три заголовка запроса:

   + `Ocp-Apim-Subscription-Key` — ключ доступа, полученный с портала Azure;
   + `Content-Type` — приложение/JSON.
   + `Accept` — приложение/JSON.

   При использовании ресурса **/keyPhrases** запрос должен выглядеть, как на следующем снимке экрана.

   ![Снимок экрана запроса с конечной точкой и заголовками](../media/postman-request-keyphrase-1.png)

4. Щелкните **Текст** и выберите **Без форматирования** для формата.

   ![Снимок экрана запроса с параметрами текста](../media/postman-request-body-raw.png)

5. Вставьте несколько документов JSON в формате, который допустим для предполагаемого анализа. Дополнительные сведения о конкретном анализе см. в статьях ниже:

  + [Пример. Как определить язык с помощью Анализа текста](text-analytics-how-to-language-detection.md)  
  + [Пример. Как извлечь ключевые фразы с помощью Анализа текста](text-analytics-how-to-keyword-extraction.md)  
  + [Пример. Как определить тональность с помощью Анализа текста](text-analytics-how-to-sentiment-analysis.md)  
  + [Распознавание сущностей](text-analytics-how-to-entity-linking.md)  


6. Щелкните **Отправить**, чтобы отправить запрос. Сведения о количестве запросов, которые можно отправить в минуту и секунду, см. в разделе [ограничения данных](../overview.md#data-limits) в обзоре.

   В Postman ответ отображается в следующем расположенном ниже окне как один документ JSON с элементом для каждого идентификатора документа, предоставленного в запросе.

## <a name="see-also"></a>См. также 

 [Обзор Анализ текста](../overview.md)  
 [Часто задаваемые вопросы](../text-analytics-resource-faq.md)

## <a name="next-steps"></a>Дальнейшие шаги

> [!div class="nextstepaction"]
> [Пример. Как определить язык с помощью Анализа текста](text-analytics-how-to-language-detection.md)
