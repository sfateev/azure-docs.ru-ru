---
title: Получение видео из пользовательского представления (Пользовательский поиск Bing)
titleSuffix: Azure Cognitive Services
description: Общий обзор использования Пользовательского поиска Bing для получения видео из пользовательского представления веб-сайта.
services: cognitive-services
author: swhite-msft
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-custom-search
ms.topic: conceptual
ms.date: 09/10/2018
ms.author: scottwhi
ms.openlocfilehash: 222256036a59c7df302546bbf82648c4d524d43f
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2020
ms.locfileid: "68405092"
---
# <a name="get-videos-from-your-custom-view"></a>Получение видео из пользовательского представления

Пользовательский поиск видео Bing расширяет возможности пользовательского поиска, добавляя в него видео. Аналогично веб-результатам, пользовательский поиск поддерживает поиск видео в списке веб-сайтов вашего экземпляра. Чтобы получить видео, воспользуйтесь API Bing для пользовательского поиска видео или функцией размещенного пользовательского интерфейса. Пользоваться функцией размещенного пользовательского интерфейса очень просто; именно эта функция позволяет быстро начать и осуществлять поиск. Сведения о настройке размещенного пользовательского интерфейса для включения видео приведены в разделе [Настройка размещенного пользовательского интерфейса](hosted-ui.md).

Если вы хотите более полно контролировать отображение результатов поиска, можно воспользоваться API Bing для пользовательского поиска видео. Так как вызов этого API аналогичен вызову API поиска видео Bing, примеры вызова этого API вы найдете в разделе [Поиск видео Bing](../Bing-Video-Search/search-the-web.md). Однако перед этим ознакомьтесь с содержимым [справочника по API для пользовательского поиска видео](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-custom-videos-api-v7-reference). Основные различия заключаются в поддерживаемых параметрах запроса (необходимо включить параметр запроса customConfig) и конечной точке, которой отправляется запрос.

<!--
## Next steps

[Call your custom view](search-your-custom-view.md)
-->
