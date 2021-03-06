---
title: Поддержка языков — API анализа текста
titleSuffix: Azure Cognitive Services
description: Список естественных языков, поддерживаемых API анализа текста. В этой статье объясняется, какие языки поддерживаются для каждой операции.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: conceptual
ms.date: 10/07/2020
ms.author: aahi
ms.openlocfilehash: 4a4058cc6317e863fa20406449e64aa877810a54
ms.sourcegitcommit: dbe434f45f9d0f9d298076bf8c08672ceca416c6
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/17/2020
ms.locfileid: "92147479"
---
# <a name="text-analytics-api-v3-language-support"></a>Поддержка языков API анализа текста v3 

> [!IMPORTANT]
> Версия 3. x API анализа текста сейчас недоступна в следующих регионах: Центральная Индия, север в ОАЭ, Северная Северный Китай 2 Восточный Китай.


#### <a name="sentiment-analysis"></a>[Анализ тональности](#tab/sentiment-analysis).

| Язык              | Код языка | Поддержка v2 | Поддержка v3 | Начальная версия модели V3: |              Примечания |
|:----------------------|:-------------:|:----------:|:----------:|:--------------------------:|-------------------:|
| Китайский (упрощенное письмо)    |   `zh-hans`   |     ✓      |     ✓      |         2019-10-01         | Также допускается `zh` |
| Китайский (традиционное письмо)   |   `zh-hant`   |            |     ✓      |         2019-10-01         |                    |
| Датский               |     `da`      |     ✓      |            |                            |                    |
| Нидерландский                 |     `nl`      |     ✓      |            |                            |                    |
| Английский               |     `en`      |     ✓      |     ✓      |         2019-10-01         |                    |
| Финский               |     `fi`      |     ✓      |            |                            |                    |
| Французский                |     `fr`      |     ✓      |     ✓      |         2019-10-01         |                    |
| Немецкий                |     `de`      |     ✓      |     ✓      |         2019-10-01         |                    |
| Греческий                 |     `el`      |     ✓      |            |                            |                    |
| Hindi                 |     `hi`      |           |      ✓      |          2020-04-01                  |                    |
| Итальянский               |     `it`      |     ✓      |     ✓      |         2019-10-01         |                    |
| Японский              |     `ja`      |     ✓      |     ✓      |         2019-10-01         |                    |
| Корейский                |     `ko`      |            |     ✓      |         2019-10-01         |                    |
| Норвежский (букмол)   |     `no`      |     ✓      |     ✓       |        01.07.2020         |                    |
| Польский                |     `pl`      |     ✓      |            |                            |                    |
| Португальский (Португалия) |    `pt-PT`    |     ✓      |     ✓      |         2019-10-01         | Также допускается `pt` |
| Русский               |     `ru`      |     ✓      |            |                            |                    |
| Испанский               |     `es`      |     ✓      |     ✓      |         2019-10-01         |                    |
| Шведский               |     `sv`      |     ✓      |            |                            |                    |
| Турецкий               |     `tr`      |     ✓      |     ✓       |         01.07.2020        |                    |

### <a name="opinion-mining-v31-preview-only"></a>Интеллектуальный анализ данных с мнением (версия 3.1 — только предварительная версия)

| Язык              | Код языка | Начиная с версии модели V3: |              Примечания |
|:----------------------|:-------------:|:------------------------------------:|-------------------:|
| Английский               |     `en`      |              2020-04-01              |                    |


#### <a name="named-entity-recognition-ner"></a>[Распознавание именованных сущностей (NER)](#tab/named-entity-recognition)

> [!NOTE]
> * В настоящее время NER v3 поддерживает только английский и испанский языки. Если вы вызываете NER v3 с другим языком, API вернет результаты версии 2.1, если язык поддерживает версию 2,1.
> * Версия 2.1 возвращает только полный набор доступных сущностей для английского, упрощенного, французского, немецкого и испанского языков.  Сущности «Person», «Location» и «Organization» возвращаются для других поддерживаемых языков.

| Язык               | Код языка | Поддержка версии 2.1 | Поддержка v3 | Начиная с версии модели V3: |       Примечания        |
|:-----------------------|:-------------:|:----------:|:----------:|:-------------------------------:|:------------------:|
| Арабский                |     `ar`      |     ✓      |            |                                 |                    |
| Чешский                 |     `cs`      |     ✓      |            |                                 |                    |
| Китайский (упрощенное письмо)     |   `zh-hans`   |     ✓      |            |                                 | Также допускается `zh` |
| Китайский (традиционное письмо)   |   `zh-hant`   |     ✓      |            |                                 |                    |
| Датский                |     `da`      |     ✓      |            |                                 |                    |
| Нидерландский                 |     `nl`      |     ✓      |            |                                 |                    |
| Английский                |     `en`      |     ✓      |     ✓      |           2019-10-01            |                    |
| Финский               |     `fi`      |     ✓      |            |                                 |                    |
| Французский                 |     `fr`      |     ✓      |            |                                 |                    |
| Немецкий                 |     `de`      |     ✓      |            |                                 |                    |
| Иврит                |     `he`      |     ✓      |            |                                 |                    |
| Венгерский             |     `hu`      |     ✓      |            |                                 |                    |
| Итальянский               |     `it`      |     ✓      |            |                                 |                    |
| Японский              |     `ja`      |     ✓      |            |                                 |                    |
| Корейский                |     `ko`      |     ✓      |            |                                 |                    |
| Норвежский (букмол)   |     `no`      |     ✓      |            |                                 | Также допускается `nb` |
| Польский                |     `pl`      |     ✓      |            |                                 |                    |
| Португальский (Португалия) |    `pt-PT`    |     ✓      |            |                                 | Также допускается `pt` |
| Португальский (Бразилия)   |    `pt-BR`    |     ✓      |            |                                 |                    |
| Русский              |     `ru`      |     ✓      |            |                                 |                    |
| Испанский               |     `es`      |     ✓      |     ✓       |              2020-04-01                   |                    |
| Шведский               |     `sv`      |     ✓      |            |                                 |                    |
| Турецкий               |     `tr`      |     ✓      |            |                                 |                    |

#### <a name="key-phrase-extraction"></a>[Пример. Как извлечь ключевые фразы с помощью Анализа текста](#tab/key-phrase-extraction)

> [!NOTE]
> Версии модели извлечение ключевых фраз до 2020-07-01 имеют ограничение в 64 символов. Это ограничение отсутствует в более поздних версиях модели.

| Язык              | Код языка | Поддержка v2 | Поддержка v3 | Доступно начиная с версии модели V3: |       Примечания        |
|:----------------------|:-------------:|:----------:|:----------:|:-----------------------------------------:|:------------------:|
| Нидерландский                 |     `nl`      |     ✓      |     ✓      |                2019-10-01                 |                    |
| Английский               |     `en`      |     ✓      |     ✓      |                2019-10-01                 |                    |
| Финский               |     `fi`      |     ✓      |     ✓      |                2019-10-01                 |                    |
| Французский                |     `fr`      |     ✓      |     ✓      |                2019-10-01                 |                    |
| Немецкий                |     `de`      |     ✓      |     ✓      |                2019-10-01                 |                    |
| Итальянский               |     `it`      |     ✓      |     ✓      |                2019-10-01                 |                    |
| Японский              |     `ja`      |     ✓      |     ✓      |                2019-10-01                 |                    |
| Корейский                |     `ko`      |     ✓      |     ✓      |                2019-10-01                 |                    |
| Норвежский (букмол)   |     `no`      |     ✓      |     ✓      |                2019-10-01                 | Также допускается `nb` |
| Польский                |     `pl`      |     ✓      |     ✓      |                2019-10-01                 |                    |
| Португальский (Португалия) |    `pt-PT`    |     ✓      |     ✓      |                2019-10-01                 | Также допускается `pt` |
| Португальский (Бразилия)   |    `pt-BR`    |     ✓      |     ✓      |                2019-10-01                 |                    |
| Русский               |     `ru`      |     ✓      |     ✓      |                2019-10-01                 |                    |
| Испанский               |     `es`      |     ✓      |     ✓      |                2019-10-01                 |                    |
| Шведский               |     `sv`      |     ✓      |     ✓      |                2019-10-01                 |                    |

#### <a name="entity-linking"></a>[Связывание сущностей](#tab/entity-linking)

| Язык | Код языка | Поддержка v2 | Поддержка v3 | Доступно начиная с версии модели V3: | Примечания |
|:---------|:-------------:|:----------:|:----------:|:-----------------------------------------:|:-----:|
| Английский  |     `en`      |     ✓      |     ✓      |                2019-10-01                 |       |
| Испанский  |     `es`      |     ✓      |     ✓      |                2019-10-01                 |       |

#### <a name="language-detection"></a>[распознавание языка](#tab/language-detection);

API анализа текста может обнаруживать широкий спектр языков, разновидностей, диалектов и некоторых региональных и культурных языков, а также возвращать обнаруженные языки с их именем и кодом. Анализ текста параметры кода распознавание языка языка соответствуют стандарту [bcp-47](https://tools.ietf.org/html/bcp47) , большинство из которых соответствует идентификаторам [ISO-639-1](https://www.iso.org/iso-639-language-codes.html) . 

Если у вас имеется содержимое на менее распространенном языке, вы можете попробовать применить распознавание языка, чтобы узнать, вернет ли эта функция код. Для языков, которые не удалась распознать, возвращается ответ `unknown`.

| Язык | Код языка |  Поддержка v3 | Доступно начиная с версии модели V3: |
|:---------|:-------------:|:----------:|:-----------------------------------------:|
|Африкаанс|`af`|✓|    |
|Албанский|`sq`|✓|    |
|Арабский|`ar`|✓|    |
|Армянский|`hy`|✓|    |
|Баскский|`eu`|✓|    |
|Белорусский|`be`|✓|    |
|Бенгальский|`bn`|✓|    |
|Боснийский|`bs`|✓|2020-09-01|
|Болгарский|`bg`|✓|    |
|Barmština|`my`|✓|    |
|Каталанский, валенсийский|`ca`|✓|    |
|Центральная кхмерский|`km`|✓|    |
|Китайский|`zh`|✓|    |
|Китайский (упрощенный)|`zh_chs`|✓|    |
|Китайский (традиционное письмо)|`zh_cht`|✓|    |
|Хорватский|`hr`|✓|    |
|Чешский|`cs`|✓|    |
|Датский|`da`|✓|    |
|Дари|`prs`|✓|2020-09-01|
|Дивехи, дхивехи, мальдивский|`dv`|✓|    |
|Нидерландский, фламандский|`nl`|✓|    |
|Английский|`en`|✓|    |
|Эсперанто|`eo`|✓|    |
|Эстонский|`et`|✓|    |
|Фиджийский|`fj`|✓|2020-09-01|
|Финский|`fi`|✓|    |
|Французский|`fr`|✓|    |
|Галисийский|`gl`|✓|    |
|Грузинский|`ka`|✓|    |
|Немецкий|`de`|✓|    |
|Греческий|`el`|✓|    |
|Гуджарати|`gu`|✓|    |
|Гаитянский, гаитянский креольский|`ht`|✓|    |
|Иврит|`he`|✓|    |
|Hindi|`hi`|✓|    |
|Хмонг дау|`mww`|✓|2020-09-01|
|Венгерский|`hu`|✓|    |
|Исландский|`is`|✓|    |
|Индонезийский|`id`|✓|    |
|Инуктитут|`iu`|✓|    |
|Ирландский|`ga`|✓|    |
|Итальянский|`it`|✓|    |
|Японский|`ja`|✓|    |
|Каннада|`kn`|✓|    |
|Казахский|`kk`|✓|2020-09-01|
|Корейский|`ko`|✓|    |
|Курдский|`ku`|✓|    |
|Лаосский|`lo`|✓|    |
|Латиница|`la`|✓|    |
|Латышский|`lv`|✓|    |
|Литовский|`lt`|✓|    |
|Macedonian|`mk`|✓|    |
|Малагасийский|`mg`|✓|2020-09-01|
|Малайский|`ms`|✓|    |
|Малаялам|`ml`|✓|    |
|Мальтийский|`mt`|✓|    |
|Маори|`mi`|✓|2020-09-01|
|Маратхи|`mr`|✓|2020-09-01|
|Норвежский|`no`|✓|    |
|Норвежский (нюнорск)|`nn`|✓|    |
|Ория|`or`|✓|    |
|Пушту, пушту|`ps`|✓|    |
|Персидский|`fa`|✓|    |
|Польский|`pl`|✓|    |
|Португальский|`pt`|✓|    |
|Панджаби, пенджаби|`pa`|✓|    |
|Керетарский диалект отоми|`otq`|✓|2020-09-01|
|Румынский, Молдавиан, молдавский|`ro`|✓|    |
|Русский|`ru`|✓|    |
|Самоанский|`sm`|✓|2020-09-01|
|Сербский|`sr`|✓|    |
|Синхала, сингальский|`si`|✓|    |
|Словацкий|`sk`|✓|    |
|Словенский|`sl`|✓|    |
|Сомалийский|`so`|✓|    |
|Испанский, кастильский|`es`|✓|    |
|Суахили|`sw`|✓|    |
|Шведский|`sv`|✓|    |
|Тагальский|`tl`|✓|    |
|Таитянский|`ty`|✓|2020-09-01|
|Тамильский|`ta`|✓|    |
|Телугу|`te`|✓|    |
|Тайский|`th`|✓|    |
|Тонганский|`to`|✓|2020-09-01|
|Турецкий|`tr`|✓|    |
|Украинский|`uk`|✓|    |
|Урду|`ur`|✓|    |
|Узбекский|`uz`|✓|    |
|Вьетнамский|`vi`|✓|    |
|Валлийский|`cy`|✓|    |
|Идиш|`yi`|✓|    |
|Юкатекский майя|`yua`|✓|    |


---


## <a name="see-also"></a>См. также

* [Что такое API анализа текста?](overview.md)   
