---
title: Краткое руководство. Поиск с помощью Python и API Bing для поиска в Интернете
titleSuffix: Azure Cognitive Services
description: В этом кратком руководстве описано, как отправлять запросы в REST API Bing для поиска в Интернете с помощью Python и получать ответы в формате JSON.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-web-search
ms.topic: quickstart
ms.date: 05/22/2020
ms.author: aahi
ms.custom: seodec2018, devx-track-python
ms.openlocfilehash: 1adb273cfedd2342a1429f0d21cd00e9a5e602a8
ms.sourcegitcommit: eb6bef1274b9e6390c7a77ff69bf6a3b94e827fc
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/05/2020
ms.locfileid: "87851177"
---
# <a name="quickstart-use-python-to-call-the-bing-web-search-api"></a>Краткое руководство. Вызов API Bing для поиска в Интернете с использованием Python  

Используйте это краткое руководство, чтобы вызвать API Поиска в Интернете Bing. Это приложение Python отправляет поисковый запрос к API и показывает ответ в формате JSON. Это приложение создано на языке Python. Но API представляет собой веб-службу на основе REST, совместимую с большинством языков программирования.

Этот пример запускается как записная книжка Jupyter в [MyBinder](https://mybinder.org). Чтобы запустить его, щелкните эмблему запуска Binder.

[![Binder](https://mybinder.org/badge.svg)](https://mybinder.org/v2/gh/Microsoft/cognitive-services-notebooks/master?filepath=BingWebSearchAPI.ipynb)

## <a name="prerequisites"></a>Предварительные требования

* [Python версии 2.x или 3.x](https://www.python.org/)

[!INCLUDE [bing-web-search-quickstart-signup](../../../../includes/bing-web-search-quickstart-signup.md)]

## <a name="define-variables"></a>Определение переменных

1. Замените значение `subscription_key` действительным ключом подписки из своей учетной записи Azure.

   ```python
   subscription_key = "YOUR_ACCESS_KEY"
   assert subscription_key
   ```

2. Объявите конечную точку API Bing для поиска в Интернете. Вы можете использовать глобальную конечную точку, указанную в коде ниже, или конечную точку [личного поддомена](../../../cognitive-services/cognitive-services-custom-subdomains.md), которая отображается на портале Azure для вашего ресурса.

   ```python
   search_url = "https://api.cognitive.microsoft.com/bing/v7.0/search"
   ```

3. Кроме того, вы можете настроить поисковый запрос, заменив значение параметра `search_term`.

   ```python
   search_term = "Azure Cognitive Services"
   ```

## <a name="make-a-request"></a>Выполнение запроса

В этом коде для вызова API Поиска в Интернете Bing и получения результатов в виде объекта JSON используется библиотека `requests`. Ключ API передается в словарь `headers`, а условие поиска и параметры запроса — в словарь `params`. 

Полный список вариантов и параметров см. в разделе [API Поиска в Интернете Bing версии 7](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-web-api-v7-reference).

```python
import requests

headers = {"Ocp-Apim-Subscription-Key": subscription_key}
params = {"q": search_term, "textDecorations": True, "textFormat": "HTML"}
response = requests.get(search_url, headers=headers, params=params)
response.raise_for_status()
search_results = response.json()
```

## <a name="format-and-display-the-response"></a>Форматирование и отображение ответа

Объект `search_results` включает результаты поиска и метаданные, описывающие связанные запросы и страницы. Этот код использует библиотеку `IPython.display`, чтобы форматировать и отображать ответ в браузере.

```python
from IPython.display import HTML

rows = "\n".join(["""<tr>
                       <td><a href=\"{0}\">{1}</a></td>
                       <td>{2}</td>
                     </tr>""".format(v["url"], v["name"], v["snippet"])
                  for v in search_results["webPages"]["value"]])
HTML("<table>{0}</table>".format(rows))
```

## <a name="sample-code-on-github"></a>Пример кода в GitHub

Чтобы выполнить этот код локально, см. полный [пример на сайте GitHub](https://github.com/Azure-Samples/cognitive-services-REST-api-samples/blob/master/python/Search/BingWebSearchv7.py).

## <a name="next-steps"></a>Дальнейшие действия

> [!div class="nextstepaction"]
> [Руководство по одностраничным приложениям для API Поиска в Интернете Bing](../tutorial-bing-web-search-single-page-app.md)

[!INCLUDE [bing-web-search-quickstart-see-also](../../../../includes/bing-web-search-quickstart-see-also.md)]
