---
title: Обнаружение цветовых схем — Компьютерное зрение
titleSuffix: Azure Cognitive Services
description: Понятия, связанные с определением цветовых схем на изображениях с помощью API компьютерного зрения.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: conceptual
ms.date: 02/08/2019
ms.author: pafarley
ms.custom: seodec18
ms.openlocfilehash: af0c39ed8211ac2041d143112437ad5d6b384259
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2020
ms.locfileid: "80244738"
---
# <a name="detect-color-schemes-in-images"></a>Обнаружение цветовых схем на изображениях

Компьютерное зрение анализирует цвета на изображениях, чтобы указать следующие три различные атрибуты: преобладающий цвет переднего плана, преобладающий цвет фона и набор преобладающих цветов изображения в целом. Возвращаемые цвета, которые содержатся в наборе: черный, синий, коричневый, серый, зеленый, оранжевый, розовый, сиреневый, красный, сине-зеленый, белый и желтый. 

Компьютерное зрение также извлекает акцентный цвет, то есть наиболее впечатляющий цвет, на основе сочетания преобладающих цветов и насыщенности. Акцентный цвет возвращается в виде шестнадцатеричного HTML-кода для цвета. 

Компьютерное зрение также возвращает логическое значение, которое указывает, является ли изображение черно-белым.

## <a name="color-scheme-detection-examples"></a>Примеры определения цветовой схемы

В приведенном ниже примере показан ответ JSON, возвращаемый компьютерным зрением при определении цветовой схемы на образце изображения. В этом случае изображение не является черно-белым, но преобладающий цвет переднего плана и фона — черный, а преобладающие цвета изображения в целом — черный и белый.

![Наружный Mountain на закате с силуэтом человека](./Images/mountain_vista.png)

```json
{
    "color": {
        "dominantColorForeground": "Black",
        "dominantColorBackground": "Black",
        "dominantColors": ["Black", "White"],
        "accentColor": "BB6D10",
        "isBwImg": false
    },
    "requestId": "0dc394bf-db50-4871-bdcc-13707d9405ea",
    "metadata": {
        "height": 202,
        "width": 300,
        "format": "Jpeg"
    }
}
```

### <a name="dominant-color-examples"></a>Примеры преобладающих цветов

В следующей таблице отображаются цвета переднего плана, фона и изображения, возвращаемые для каждого примера изображения.

| Образ — | Преобладающие цвета |
|-------|-----------------|
|![Белый цветок на зеленом фоне](./Images/flower.png)| Передний план: черный<br/>Фон: белый<br/>Цвета: черный, белый, зеленый|
![Поезд, проходящий через станцию](./Images/train_station.png) | Передний план: черный<br/>Фон: черный<br/>Цвета: черный |

### <a name="accent-color-examples"></a>Примеры акцентных цветов

 В приведенной ниже таблице представлены акцентные цвета, возвращаемые для каждого примера изображения, в виде шестнадцатеричных значений цвета HTML.

| Образ — | Цвет элементов |
|-------|--------------|
|![Человек, стоящий на скале на закате](./Images/mountain_vista.png) | #BB6D10 |
|![Белый цветок на зеленом фоне](./Images/flower.png) | #C6A205 |
|![Поезд, проходящий через станцию](./Images/train_station.png) | #474A84 |

### <a name="black--white-detection-examples"></a>Примеры определения черно-белых изображений

В приведенной ниже таблице представлены результаты оценки черно-белых изображений для примеров изображений с помощью службы "Компьютерное зрение".

| Образ — | Черно-белое |
|-------|----------------|
|![Черно-белая фотография здания на Манхэттене](./Images/bw_buildings.png) | Да |
|![Синий дом и передний двор](./Images/house_yard.png) | false |

## <a name="use-the-api"></a>Использование API

Функция обнаружения цветовых схем является частью API [анализа изображений](https://westcentralus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fa) . Вы можете вызывать этот API с помощью собственного пакета SDK или с помощью вызовов REST. Включите `Color` в параметр запроса **висуалфеатурес** . Затем, когда вы получаете полный ответ JSON, просто Проанализируйте строку для содержимого `"color"` раздела.

* [Краткое руководство. Компьютерное зрение пакета SDK для .NET](./quickstarts-sdk/client-library.md?pivots=programming-language-csharp)
* [Краткое руководство. Анализ изображения (REST API)](./quickstarts/csharp-analyze.md)
