---
title: Поступления — распознаватель форм
titleSuffix: Azure Cognitive Services
description: Изучите основные понятия, связанные с анализом прихода, с помощью API-интерфейса распознавателя форм и ограничений.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: forms-recognizer
ms.topic: conceptual
ms.date: 08/17/2019
ms.author: pafarley
ms.openlocfilehash: 0382c7c7f7d068ea227397ae7accf4bc410de04a
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2020
ms.locfileid: "91761453"
---
# <a name="receipt-concepts"></a>Сведения о квитанциях

Средство распознавания форм Azure может анализировать уведомления с помощью одной из предварительно созданных моделей. API-интерфейс уведомления извлекает ключевые сведения из квитанций о продажах на английском языке, такие как название продавца, Дата транзакции, итоговая сумма, элементы строк и многое другое. 

## <a name="understanding-receipts"></a>Основные сведения о приемках 

Многие компании и лица по-прежнему полагаются на извлечение данных из своих чеков по продажам, будь то отчеты по расходам на бизнес, зачеты, аудит, налогообложение, бюджетирование, маркетинг или другие цели. Часто в этих сценариях для проверки потребуются образы физического получения.  

Автоматическое извлечение данных из этих поступлений может быть сложным. Уведомления могут быть крумплед, а печатные или рукописные фрагменты и изображения для смартфонов могут иметь низкий уровень качества. Кроме того, шаблоны и поля получения могут сильно различаться на рынке, регионе и в оптовом торговце. Эти трудности при извлечении данных и обнаружении полей делают обработку уникальной проблемы.  

С помощью оптического распознавания символов (OCR) и нашей предварительно созданной модели поступлений API-интерфейс поступлений позволяет использовать эти сценарии обработки уведомлений и извлекать данные из таких чеков, как имя получателя, Совет, итог, элементы строк и многое другое. При использовании этого API не нужно обучать модель, которую вы просто отправляете в API анализа чеков, и данные извлекаются.

![Пример квитанции](./media/contoso-receipt-small.png)

## <a name="what-does-the-receipt-api-do"></a>Что делает API поступления? 

Предварительно собранный API-интерфейс уведомления извлекает содержимое квитанций о продажах, &mdash; которое вы обычно получаете в ресторане, в розничной торговле или в магазине по магазину продуктов.

### <a name="fields-extracted"></a>Извлеченные поля

* Название продавца 
* Адрес продавца 
* Номер телефона продавца 
* Дата транзакции 
* Время транзакции 
* Промежуточный итог 
* Налог 
* Итог 
* Совет 
* Извлечение элементов строк (например, количество товара, Цена товара, имя номенклатуры)

### <a name="additional-features"></a>Дополнительные функции

API получения также возвращает следующие сведения:

* Тип уведомления (например, "детализированный", "кредитная карта" и т. д.)
* Уровень достоверности полей (каждое поле возвращает соответствующее значение достоверности)
* Необработанный текст OCR (текст, извлеченный OCR для получения всей квитанции)
* Ограничивающий прямоугольник для каждого значения, строки и слова

## <a name="input-requirements"></a>Требования к входным данным

[!INCLUDE [input reqs](./includes/input-requirements-receipts.md)]

## <a name="supported-locales"></a>Поддерживаемые языковые стандарты 

* **Предварительно созданная квитанция версии 2.0** (общедоступная версия) поддерживает поступления по продажам в национальной настройке en-US
* **Предварительно созданная квитанция версии 2.1 – Preview. 1** (общедоступная Предварительная версия) добавляет дополнительную поддержку для следующих языков EN-чека: 
  * EN-AU 
  * EN-CA 
  * EN-GB 
  * EN-IN 

  > [!NOTE]
  > Языковой ввод 
  >
  > Предварительно созданная квитанция версии 2.1-Preview. 1 имеет необязательный параметр запроса, который позволяет указать языковой стандарт для получения дополнительных английских рынков. Для получения уведомлений о продажах на английском языке из Австралии (EN-AU), Канады (EN-CA), Великобритании (EN-GB) и Индии (EN-IN) можно указать языковой стандарт, чтобы получить улучшенные результаты. Если языковой стандарт не указан в версии 2.1-Preview. 1, то модель будет по умолчанию использовать модель EN-US.


## <a name="the-analyze-receipt-operation"></a>Операция анализа получения

При [проверке поступлений](https://westcentralus.dev.cognitive.microsoft.com/docs/services/form-recognizer-api-v2-1-preview-1/operations/AnalyzeReceiptAsync) в качестве входных данных принимается изображение или PDF-документ, а также извлекаются интересующие значения и текст. Вызов возвращает поле заголовка ответа с именем `Operation-Location` . `Operation-Location`Значение — это URL-адрес, СОДЕРЖАЩИЙ идентификатор результата, который будет использоваться на следующем шаге.

|Заголовок ответа| URL-адрес результата |
|:-----|:----|
|Operation-Location | `https://cognitiveservice/formrecognizer/v2.0/prebuilt/receipt/analyzeResults/56a36454-fc4d-4354-aa07-880cfbf0064f` |

## <a name="the-get-analyze-receipt-result-operation"></a>Операция получения результатов анализа

Второй шаг — вызов операции [получения результата анализа](https://westcentralus.dev.cognitive.microsoft.com/docs/services/form-recognizer-api-v2-1-preview-1/operations/GetAnalyzeReceiptResult) . Эта операция принимает в качестве входных данных идентификатор результата, созданный операцией Analyze оприходования. Он возвращает ответ JSON, содержащий поле **состояния** со следующими возможными значениями. Эта операция вызывается итеративно, пока она не вернется со значением, которое было **выполнено** . Используйте интервал от 3 до 5 секунд, чтобы избежать превышения скорости запросов в секунду (RPS).

|Поле| Тип | Возможные значения |
|:-----|:----:|:----|
|status | строка | notStarted: операция анализа не началась. |
| |  | выполняется: выполняется операция анализа. |
| |  | Сбой: не удалось выполнить операцию анализа. |
| |  | успешно: операция анализа успешно выполнена. |

Если поле **Status** имеет значение **успех** , ответ JSON будет включать результаты получения и распознавания текста. Результат получения сведений об уведомлении организован как словарь значений именованных полей, где каждое значение содержит извлеченный текст, нормализованное значение, ограничивающий прямоугольник, достоверность и соответствующие элементы слова. Результат распознавания текста организован в виде иерархии строк и слов с текстом, ограничивающим прямоугольником и сведениями о достоверности.

![Пример результатов получения](./media/contoso-receipt-2-information.png)

### <a name="sample-json-output"></a>Пример выходных данных JSON

См. Следующий пример успешного ответа JSON: узел "Реадресултс" содержит весь распознанный текст. Текст упорядочивается по страницам, затем по строкам, а затем по отдельным словам. Узел "Документресултс" содержит зависящие от компании значения, которые обнаруживает модель. Здесь вы найдете полезные пары "ключ-значение", такие как имя, фамилия, название компании и многое другое.

```json
{ 
  "status":"succeeded",
  "createdDateTime":"2019-12-17T04:11:24Z",
  "lastUpdatedDateTime":"2019-12-17T04:11:32Z",
  "analyzeResult":{ 
    "version":"2.0.0",
    "readResults":[ 
      { 
        "page":1,
        "angle":0.6893,
        "width":1688,
        "height":3000,
        "unit":"pixel",
        "language":"en",
        "lines":[ 
          { 
            "text":"Contoso",
            "boundingBox":[ 
              635,
              510,
              1086,
              461,
              1098,
              558,
              643,
              604
            ],
            "words":[ 
              { 
                "text":"Contoso",
                "boundingBox":[ 
                  639,
                  510,
                  1087,
                  461,
                  1098,
                  551,
                  646,
                  604
                ],
                "confidence":0.955
              }
            ]
          },
          ...
        ]
      }
    ],
    "documentResults":[ 
      { 
        "docType":"prebuilt:receipt",
        "pageRange":[ 
          1,
          1
        ],
        "fields":{ 
          "ReceiptType":{ 
            "type":"string",
            "valueString":"Itemized",
            "confidence":0.692
          },
          "MerchantName":{ 
            "type":"string",
            "valueString":"Contoso Contoso",
            "text":"Contoso Contoso",
            "boundingBox":[ 
              378.2,
              292.4,
              1117.7,
              468.3,
              1035.7,
              812.7,
              296.3,
              636.8
            ],
            "page":1,
            "confidence":0.613,
            "elements":[ 
              "#/readResults/0/lines/0/words/0",
              "#/readResults/0/lines/1/words/0"
            ]
          },
          "MerchantAddress":{ 
            "type":"string",
            "valueString":"123 Main Street Redmond, WA 98052",
            "text":"123 Main Street Redmond, WA 98052",
            "boundingBox":[ 
              302,
              675.8,
              848.1,
              793.7,
              809.9,
              970.4,
              263.9,
              852.5
            ],
            "page":1,
            "confidence":0.99,
            "elements":[ 
              "#/readResults/0/lines/2/words/0",
              "#/readResults/0/lines/2/words/1",
              "#/readResults/0/lines/2/words/2",
              "#/readResults/0/lines/3/words/0",
              "#/readResults/0/lines/3/words/1",
              "#/readResults/0/lines/3/words/2"
            ]
          },
          "MerchantPhoneNumber":{ 
            "type":"phoneNumber",
            "valuePhoneNumber":"+19876543210",
            "text":"987-654-3210",
            "boundingBox":[ 
              278,
              1004,
              656.3,
              1054.7,
              646.8,
              1125.3,
              268.5,
              1074.7
            ],
            "page":1,
            "confidence":0.99,
            "elements":[ 
              "#/readResults/0/lines/4/words/0"
            ]
          },
          "TransactionDate":{ 
            "type":"date",
            "valueDate":"2019-06-10",
            "text":"6/10/2019",
            "boundingBox":[ 
              265.1,
              1228.4,
              525,
              1247,
              518.9,
              1332.1,
              259,
              1313.5
            ],
            "page":1,
            "confidence":0.99,
            "elements":[ 
              "#/readResults/0/lines/5/words/0"
            ]
          },
          "TransactionTime":{ 
            "type":"time",
            "valueTime":"13:59:00",
            "text":"13:59",
            "boundingBox":[ 
              541,
              1248,
              677.3,
              1261.5,
              668.9,
              1346.5,
              532.6,
              1333
            ],
            "page":1,
            "confidence":0.977,
            "elements":[ 
              "#/readResults/0/lines/5/words/1"
            ]
          },
          "Items":{ 
            "type":"array",
            "valueArray":[ 
              { 
                "type":"object",
                "valueObject":{ 
                  "Quantity":{ 
                    "type":"number",
                    "text":"1",
                    "boundingBox":[ 
                      245.1,
                      1581.5,
                      300.9,
                      1585.1,
                      295,
                      1676,
                      239.2,
                      1672.4
                    ],
                    "page":1,
                    "confidence":0.92,
                    "elements":[ 
                      "#/readResults/0/lines/7/words/0"
                    ]
                  },
                  "Name":{ 
                    "type":"string",
                    "valueString":"Cappuccino",
                    "text":"Cappuccino",
                    "boundingBox":[ 
                      322,
                      1586,
                      654.2,
                      1601.1,
                      650,
                      1693,
                      317.8,
                      1678
                    ],
                    "page":1,
                    "confidence":0.923,
                    "elements":[ 
                      "#/readResults/0/lines/7/words/1"
                    ]
                  },
                  "TotalPrice":{ 
                    "type":"number",
                    "valueNumber":2.2,
                    "text":"$2.20",
                    "boundingBox":[ 
                      1107.7,
                      1584,
                      1263,
                      1574,
                      1268.3,
                      1656,
                      1113,
                      1666
                    ],
                    "page":1,
                    "confidence":0.918,
                    "elements":[ 
                      "#/readResults/0/lines/8/words/0"
                    ]
                  }
                }
              },
              ...
            ]
          },
          "Subtotal":{ 
            "type":"number",
            "valueNumber":11.7,
            "text":"11.70",
            "boundingBox":[ 
              1146,
              2221,
              1297.3,
              2223,
              1296,
              2319,
              1144.7,
              2317
            ],
            "page":1,
            "confidence":0.955,
            "elements":[ 
              "#/readResults/0/lines/13/words/1"
            ]
          },
          "Tax":{ 
            "type":"number",
            "valueNumber":1.17,
            "text":"1.17",
            "boundingBox":[ 
              1190,
              2359,
              1304,
              2359,
              1304,
              2456,
              1190,
              2456
            ],
            "page":1,
            "confidence":0.979,
            "elements":[ 
              "#/readResults/0/lines/15/words/1"
            ]
          },
          "Tip":{ 
            "type":"number",
            "valueNumber":1.63,
            "text":"1.63",
            "boundingBox":[ 
              1094,
              2479,
              1267.7,
              2485,
              1264,
              2591,
              1090.3,
              2585
            ],
            "page":1,
            "confidence":0.941,
            "elements":[ 
              "#/readResults/0/lines/17/words/1"
            ]
          },
          "Total":{ 
            "type":"number",
            "valueNumber":14.5,
            "text":"$14.50",
            "boundingBox":[ 
              1034.2,
              2617,
              1387.5,
              2638.2,
              1380,
              2763,
              1026.7,
              2741.8
            ],
            "page":1,
            "confidence":0.985,
            "elements":[ 
              "#/readResults/0/lines/19/words/0"
            ]
          }
        }
      }
    ]
  }
}
```


## <a name="customer-scenarios"></a>Сценарии для клиентов  

Данные, извлеченные с помощью API уведомления, можно использовать для выполнения различных задач. Ниже приведено несколько примеров того, что наши клиенты выполнили с помощью API получения уведомлений. 

### <a name="business-expense-reporting"></a>Отчеты по бизнес-расходам  

Часто для хранения бизнес-расходов требуется тратить время на ручное ввод данных из изображений чеков. С помощью API получения можно использовать извлеченные поля, чтобы частично автоматизировать этот процесс и быстро анализировать квитанции.  

Так как в API прихода имеются простые выходные данные JSON, извлеченные значения полей можно использовать несколькими способами. Интеграция с внутренними приложениями расходов для предварительного заполнения отчетов о расходах. Дополнительные сведения об этом сценарии см. в статье о том, как Acumatica использует API получения, чтобы [сделать отчет о расходах менее непростым](https://customers.microsoft.com/en-us/story/762684-acumatica-partner-professional-services-azure).  

### <a name="auditing-and-accounting"></a>Аудит и учет 

Выходные данные API прихода также можно использовать для анализа большого количества расходов в различных точках в отчете о расходах и возмещении. Вы можете обрабатывать уведомления, чтобы рассмотреть их для ручного аудита или быстрого утверждения.  

Выходные данные уведомления также полезны для общей книги, предназначенной для бизнеса или личных целей. Используйте API-интерфейс уведомления, чтобы преобразовать любые данные изображения или PDF необработанного поступления в цифровой выход, который является обрабатываемым.

### <a name="consumer-behavior"></a>Поведение потребителей 

Уведомления содержат полезные данные, которые можно использовать для анализа поведения потребителей и тенденций покупок.

API прихода также включает [функцию обработки поступления аибуилдер](https://docs.microsoft.com/ai-builder/prebuilt-receipt-processing).

## <a name="next-steps"></a>Дальнейшие шаги

- Заполните [Краткое руководство по клиентской библиотеке распознавателя форм](quickstarts/client-library.md) , чтобы начать писать приложение обработки чеков с распознавателем форм на любом языке.
- Или следуйте инструкциям в [кратком руководстве по API для получения уведомлений](./quickstarts/python-receipts.md) для распознавания уведомлений с помощью REST API.

## <a name="see-also"></a>См. также

* [Что такое Распознаватель документов?](./overview.md)
* [Справочная документация по REST API](https://docs.microsoft.com/azure/cognitive-services/form-recognizer)
