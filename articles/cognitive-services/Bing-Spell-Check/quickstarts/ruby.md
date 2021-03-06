---
title: Краткое руководство. Проверка орфографии с помощью REST API и Ruby — Проверка орфографии Bing
titleSuffix: Azure Cognitive Services
description: Научитесь проверять орфографию и грамматику с помощью REST API проверки орфографии Bing и Ruby.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-spell-check
ms.topic: quickstart
ms.date: 05/21/2020
ms.author: aahi
ms.openlocfilehash: 0b29ba1b09123784bfbac6fda4f5a1c6953f4e49
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/25/2020
ms.locfileid: "91327397"
---
# <a name="quickstart-check-spelling-with-the-bing-spell-check-rest-api-and-ruby"></a>Краткое руководство. Проверка орфографии с помощью REST API Проверки орфографии Bing и Ruby

В этом кратком руководстве показано, как с помощью Ruby отправить вызов к REST API Проверки орфографии Bing. Это простое приложение отправляет запрос к API и возвращает список предлагаемых исправлений. 

Хотя приложение написано на Ruby, API представляет собой веб-службу на основе REST, совместимую с большинством языков программирования. Исходный код этого приложения доступен на [GitHub](https://github.com/Azure-Samples/cognitive-services-REST-api-samples/blob/master/ruby/Search/BingSpellCheckv7.rb).

## <a name="prerequisites"></a>Предварительные требования

* [Ruby 2.4](https://www.ruby-lang.org/en/downloads/) или более поздней версии.

[!INCLUDE [cognitive-services-bing-spell-check-signup-requirements](../../../../includes/cognitive-services-bing-spell-check-signup-requirements.md)]


## <a name="create-and-initialize-the-application"></a>Создание и инициализация приложения

1. Создайте файл Ruby в выбранном редакторе или интегрированной среде разработки и добавьте следующие требования: 

    ```ruby
    require 'net/http'
    require 'uri'
    require 'json'
    ```

2. Создайте переменные для вашего ключа подписки, URI конечной точки и путь. Вы можете использовать глобальную конечную точку, указанную в коде ниже, или конечную точку [личного поддомена](../../../cognitive-services/cognitive-services-custom-subdomains.md), которая отображается на портале Azure для вашего ресурса. Создайте параметры запроса:

   1. Назначьте код рынка для параметра `mkt` с помощью оператора `=`. Код рынка — это код страны или региона, из которого выполняется запрос. 

   1. Добавьте параметр `mode` с оператором `&` и назначьте режим проверки орфографии. Можно указать режим `proof` (выявляет большинство орфографических и грамматических ошибок) или `spell` (выявляет большинство орфографических ошибок, но не так много грамматических ошибок). 

    ```ruby
    key = 'ENTER YOUR KEY HERE'
    uri = 'https://api.cognitive.microsoft.com'
    path = '/bing/v7.0/spellcheck?'
    params = 'mkt=en-us&mode=proof'
    ```

## <a name="send-a-spell-check-request"></a>Отправка запроса проверки орфографии

1. Создайте объект URI, указав универсальный код ресурса, путь и строку параметров узла. Передайте в запросе текст, для которого необходимо проверить орфографию.

   ```ruby
   uri = URI(uri + path + params)
   uri.query = URI.encode_www_form({
      # Request parameters
   'text' => 'Hollo, wrld!'
   })
   ```

2. Создайте запрос с помощью URI, созданного ранее. Добавьте ключ в заголовок запроса `Ocp-Apim-Subscription-Key`.

    ```ruby
    request = Net::HTTP::Post.new(uri)
    request['Content-Type'] = "application/x-www-form-urlencoded"
    request['Ocp-Apim-Subscription-Key'] = key
    ```

3. Отправьте запрос.

    ```ruby
    response = Net::HTTP.start(uri.host, uri.port, :use_ssl => uri.scheme == 'https') do |http|
        http.request(request)
    end
    ```

4. Получите ответ в JSON и выведите его в консоль. 

    ```ruby
    result = JSON.pretty_generate(JSON.parse(response.body))
    puts result
    ```

## <a name="run-the-application"></a>Выполнение приложения

Выполните сборку проекта и запустите его. При использовании командной строки выполните приведенную ниже команду для запуска приложения.

   ```bash
   ruby <FILE_NAME>.rb
   ```

## <a name="example-json-response"></a>Пример ответа в формате JSON

Успешный ответ возвращается в формате JSON, как показано в примере ниже. 

```json
{
   "_type": "SpellCheck",
   "flaggedTokens": [
      {
         "offset": 0,
         "token": "Hollo",
         "type": "UnknownToken",
         "suggestions": [
            {
               "suggestion": "Hello",
               "score": 0.9115257530801
            },
            {
               "suggestion": "Hollow",
               "score": 0.858039839213461
            },
            {
               "suggestion": "Hallo",
               "score": 0.597385084464481
            }
         ]
      },
      {
         "offset": 7,
         "token": "wrld",
         "type": "UnknownToken",
         "suggestions": [
            {
               "suggestion": "world",
               "score": 0.9115257530801
            }
         ]
      }
   ]
}
```

## <a name="next-steps"></a>Дальнейшие действия

> [!div class="nextstepaction"]
> [Создание одностраничного веб-приложения](../tutorials/spellcheck.md)

- [Что такое API проверки орфографии Bing?](../overview.md)
- [Справочник по API "Проверка орфографии Bing" версии 7](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-spell-check-api-v7-reference)
