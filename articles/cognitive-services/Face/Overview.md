---
title: Что такое служба "Распознавание лиц" Azure?
titleSuffix: Azure Cognitive Services
description: Служба "Распознавание лиц" Azure предоставляет алгоритмы искусственного интеллекта, используемые для обнаружения, распознавания и анализа человеческих лиц на изображениях.
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: face-api
ms.topic: overview
ms.date: 9/17/2020
ms.author: pafarley
ms.custom: cog-serv-seo-aug-2020
keywords: Распознавание лиц, программное обеспечение для распознавания лиц, анализ лиц, сопоставление лиц, приложение распознавания лиц, поиск лиц по изображениям, поиск распознавания лиц
ms.openlocfilehash: 0a7e242add9fdaa9e169a4003e8ad8f39b1fb111
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/25/2020
ms.locfileid: "91262490"
---
# <a name="what-is-the-azure-face-service"></a>Что такое служба "Распознавание лиц" Azure?

> [!WARNING]
> 11 июня 2020 г. корпорация Майкрософт объявила о том, что она не будет продавать технологию распознавания лиц полицейским управлениям в США до тех пор, пока не вступят силу строгие правовые нормы, гарантирующие защиту прав человека. Таким образом, клиенты не смогут использовать возможности по распознаванию лиц или функции, включенные в службы Azure, такие как Распознавание лиц или Индексатор видео, если клиент является сотрудником полицейского управления США или разрешает использование таких служб управлением или для управления.

[!INCLUDE [TLS 1.2 enforcement](../../../includes/cognitive-services-tls-announcement.md)]

Служба "Распознавание лиц" Azure предоставляет алгоритмы искусственного интеллекта для обнаружения, распознавания и анализа человеческих лиц на изображениях. Программное обеспечение для распознавания лиц имеет важное значение в различных сценариях, таких как обеспечение безопасности, анализ содержимого изображений и управление им, а также использование естественных пользовательских интерфейсов, мобильных приложений и робототехники.

Служба "Распознавание лиц" предоставляет несколько разных функций анализа лиц, каждая из которых описана в следующих разделах.

## <a name="face-detection"></a>Определение лиц

Служба "Распознавание лиц" может выявлять лица на изображениях и возвращать координаты прямоугольника, в котором они расположены. При необходимости функция определения лиц извлекает ряд атрибутов, связанных с лицом, таких как положение головы, пол, возраст, выражение, волосяной покров лица и очки.

> [!NOTE]
> Функция определения лиц также доступна через [службу "Компьютерное зрение"](https://docs.microsoft.com/azure/cognitive-services/computer-vision/home). Однако, если вы хотите выполнить последующие операции с данными распознавания лиц, следует использовать эту службу.

![Изображение женщины и мужчины с прямоугольниками, нарисованными вокруг их лиц, а также сведения о возрасте и поле](./Images/Face.detection.jpg)

См. подробнее об [определении лиц](concepts/face-detection.md). См. также справочную документацию по [API определения](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236).

## <a name="face-verification"></a>Проверка лиц

API проверки выполняет проверку идентичности двух обнаруженных лиц или одного обнаруженного лица по отношению к одному человеку. Практически оно оценивает, принадлежат ли два лица одному человеку. Это может быть полезно в сценариях, связанных с обеспечением безопасности. Дополнительные сведения см. в руководстве по [распознаванию лиц](concepts/face-recognition.md) или справочной документации по [API Проверки](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523a).

## <a name="find-similar-faces"></a>Поиск похожих лиц

API поиска похожих лиц сравнивает целевое лицо и набор потенциальных лиц, после чего находит небольшое количество лиц, очень похожих на целевое. Это удобно для поиска лиц по изображениям. 

Поддерживаются два режима работы: **matchPerson** и **matchFace**. Режим **matchPerson** возвращает похожие лица после фильтрации для одного пользователя с помощью [API Проверки](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523a). Режим **matchFace** игнорирует такой фильтр. Он возвращает список обнаруженных лиц, которые могут или не могут принадлежать тому же человеку.

В следующем примере показано целевое лицо:

![Улыбающаяся женщина](./Images/FaceFindSimilar.QueryFace.jpg)

А здесь изображены лица-кандидаты:

![Пять изображений улыбающихся людей. Изображения А и Б: изображения одного человека](./Images/FaceFindSimilar.Candidates.jpg)

При поиске похожих лиц режим **matchPerson** возвращает фотографии А и Б, на которых изображен тот же человек, что и на фотографии с целевым лицом. Режим **matchFace** возвращает фотографии А, Б, В, Г, т. е. четырех кандидатов, даже если некоторые из них не совпадают с целевым лицом или имеют низкое сходство. Дополнительные сведения см. в руководстве по [распознаванию лиц](concepts/face-recognition.md) или справочной документации по [API Поиска похожих лиц](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395237).

## <a name="face-grouping"></a>Группировка лиц

API группы делит неизвестные лица на несколько групп, основываясь на сходстве. Каждая группа является несвязанным подмножеством исходного набора лиц. Все лица в одной группе, скорее всего, будут принадлежать одному и тому же человеку. Для одного человека может быть несколько таких групп. Группы различаются по разным факторам, например по выражению лица. Дополнительные сведения см. в руководстве по [распознаванию лиц](concepts/face-recognition.md) или справочной документации по [API Группирования](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395238).

## <a name="person-identification"></a>Идентификация личности

API Идентификации можно использовать для определения обнаруженных лиц в базе данных (поиск по распознаванию лиц). Это может быть полезно для автоматического добавления тегов в ПО для управления фотографиями. Эту базу данных можно создать заранее и редактировать по мере использования.

Ниже показан пример базы данных с именем `"myfriends"`. Каждая группа может содержать до 1 млн объектов, соответствующих разным людям. В свою очередь, для каждого объекта, соответствующего одному человеку, можно зарегистрировать до 248 лиц.

![Таблица с тремя столбцами для разных людей, каждый с тремя записями изображений лиц.](./Images/person.group.clare.jpg)

Создав и обучив базу данных, вы можете идентифицировать новое обнаруженное лицо путем сравнения с группой. Если лицо определяется как принадлежащее человеку в группе, то возвращается объект, соответствующий этому человеку.

Дополнительные сведения об идентификации людей см. в руководстве по [распознаванию лиц](concepts/face-recognition.md) или справочной документации по [API Идентификации](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395239).

## <a name="containers"></a>Контейнеры

[Контейнер API Распознавания лиц](face-how-to-install-containers.md) можно использовать для обнаружения, распознавания и идентификации лиц, установив стандартные контейнеры Docker в непосредственной близости к своим данным.

## <a name="sample-apps"></a>Примеры приложений

Следующие примеры приложений демонстрируют несколько способов использования службы "Распознавание лиц":

- [API Распознавания лиц: клиентская библиотека Windows с примерами](https://github.com/Microsoft/Cognitive-Face-Windows). Это приложение WPF, демонстрирующее несколько сценариев определения, анализа и идентификации лиц.
- [Приложение UWP FamilyNotes](https://github.com/Microsoft/Windows-appsample-familynotes).Это приложение универсальной платформы Windows (UWP), которое использует идентификацию лиц вместе с речью, рукописным вводом, Кортаной и камерами в сценарии совместного доступа к семейным заметкам.

## <a name="data-privacy-and-security"></a>Конфиденциальность и безопасность данных

Как и в случае с другими ресурсами Cognitive Services, разработчикам, использующим API Распознавания лиц, следует учитывать политику корпорации Майкрософт в отношении клиентских данных. См. подробнее на [странице Cognitive Services](https://www.microsoft.com/trustcenter/cloudservices/cognitiveservices) в Центре управления безопасностью Майкрософт.

## <a name="next-steps"></a>Дальнейшие действия

Следуйте инструкциям в кратком руководстве, чтобы запрограммировать основные компоненты приложения для распознавания лиц на любом языке.

- [Краткое руководство по использованию клиентской библиотеки](quickstarts/client-libraries.md).
