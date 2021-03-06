---
title: Ограничения службы по уровням и номерам SKU
titleSuffix: Azure Cognitive Search
description: Ограничения для службы "Когнитивный поиск Azure", применяемые при планировании ресурсов, а также ограничения по запросам и ответам.
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 10/14/2020
ms.openlocfilehash: f3763857af1df8f34f38b36835a667c6610e1909
ms.sourcegitcommit: ae6e7057a00d95ed7b828fc8846e3a6281859d40
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/16/2020
ms.locfileid: "92107833"
---
# <a name="service-limits-in-azure-cognitive-search"></a>Ограничения службы "Когнитивный поиск Azure"

Максимальные ограничения хранилища, рабочих нагрузок, количества индексов и других объектов зависят от того, с какой ценовой категорией вы [подготовили службу "Когнитивный поиск Azure"](search-create-service-portal.md): **Бесплатный**, **Базовый**, **Стандартный** или **Оптимизировано для хранилища**.

+ **Бесплатный** представляет собой мультитенантную службу, которая предоставляется вместе с подпиской Azure. 

+ **Базовый** предоставляет выделенные вычислительные ресурсы для небольших рабочих нагрузок в рабочей среде, но вам придется делить часть сетевой инфраструктуры с другими арендаторами.

+ Для уровня **Стандартный** используются выделенные виртуальные машины. Это обеспечивает увеличение емкости хранилища и вычислительной мощности на всех уровнях. В категории "Стандартный" доступно четыре уровня: S1, S2, S3 и S3 HD (S3, высокая плотность). Уровень S3 HD (S3, высокая плотность) разработан для рабочих нагрузок с [мультитенантностью](search-modeling-multitenant-saas-applications.md) и большим количеством небольших индексов (до трех тысяч индексов на каждую службу). Уровень S3 HD не поддерживает [функцию индексатора](search-indexer-overview.md), поэтому прием данных придется выполнять через API, которые отправляют данные из источника в индекс. 

+ **Оптимизировано для хранилища** выполняется на выделенных компьютерах с более высокими объемами хранилища, пропускной способности хранилища и памяти, чем в категории **Стандартный**. Эта ценовая категория предназначена для больших и медленно меняющихся индексов. В категории "Оптимизировано для хранилища" доступно два уровня: L1 и L2.

## <a name="subscription-limits"></a>Ограничения подписки
[!INCLUDE [azure-search-limits-per-subscription](../../includes/azure-search-limits-per-subscription.md)]

## <a name="storage-limits"></a>Ограничения хранилища
[!INCLUDE [azure-search-limits-per-service](../../includes/azure-search-limits-per-service.md)]

<a name="index-limits"></a>

## <a name="index-limits"></a>Ограничения индексов

| Ресурс | Free | Базовый&nbsp;<sup>1</sup>  | S1 | S2 | S3 | S3&nbsp;HD | L1 | L2 |
| -------- | ---- | ------------------- | --- | --- | --- | --- | --- | --- |
| Максимальное число индексов |3 |5 или 15 |50 |200 |200 |1000 на секцию или 3000 на службу |10 |10 |
| Максимальное число простых полей на индекс |1000 |100 |1000 |1000 |1000 |1000 |1000 |1000 |
| Максимальное число сложных полей коллекции на индекс |40 |40 |40 |40 |40 |40 |40 |40 |
| Максимальное число элементов во всех сложных коллекциях на документ&nbsp;<sup>2</sup> |3000 |3000 |3000 |3000 |3000 |3000 |3000 |3000 |
| Максимальная глубина сложных полей |10 |10 |10 |10 |10 |10 |10 |10 |
| Максимальное число [средств подбора](/rest/api/searchservice/suggesters) на индекс |1 |1 |1 |1 |1 |1 |1 |1 |
| Максимальное число [профилей оценки](/rest/api/searchservice/add-scoring-profiles-to-a-search-index) на индекс |100 |100 |100 |100 |100 |100 |100 |100 |
| Максимальное число функций на профиль |8 |8 |8 |8 |8 |8 |8 |8 |

<sup>1</sup> Службы категории "Базовый", созданные до декабря 2017 г., имеют более низкие ограничения индексов (5 вместо 15). Уровень "Базовый" — единственный SKU с ограничением в минимум 100 полей на индекс.

<sup>2</sup> верхний предел существует для элементов, так как наличие большого количества их значительно увеличивает объем хранилища, необходимый для индекса. Элемент сложной коллекции определяется как член этой коллекции. Например, предположим, что [документ отеля со сложной коллекцией комнат](search-howto-complex-data-types.md#indexing-complex-types), каждая комната в коллекции помещений считается элементом. Во время индексирования механизм индексирования может безопасно обрабатывать не более 3000 элементов в документе в целом. [Это ограничение](search-api-migration.md#upgrade-to-2019-05-06) было введено в `api-version=2019-05-06` и применяется только к сложным коллекциям, а не к коллекциям строк или к сложным полям.

<a name="document-limits"></a>

## <a name="document-limits"></a>Ограничения документов 

С октября 2018 года не существует ограничений на количество документов для новых служб, созданных на любом платном уровне (Basic, S1, S2, S3, S3 HD) в любом регионе. Для более старых служб, то есть созданных до октября 2018 года, могут по-прежнему применяться ограничения на количество документов.

Чтобы определить, существуют ли ограничения на количество документов для вашей службы, примените [REST API получения статистики по службе](/rest/api/searchservice/get-service-statistics). Ограничения по документам будут указаны в ответе, а значение `null` обозначает отсутствие ограничений.

> [!NOTE]
> Несмотря на то, что служба не накладывает ограничений на число документов, существует предел сегментирования на уровне примерно 24 миллиарда документов для каждого индекса в службах поиска уровней "Базовый", S1, S2 и S3. Для уровня S3 HD предел сегментирования составляет 2 миллиарда документов на индекс. С точки зрения пределов сегментирования каждый элемент сложной коллекции считается отдельным документом.

### <a name="document-size-limits-per-api-call"></a>Ограничения размера документа на один вызов API

Максимальный размер документа при вызове API индекса составляет примерно 16 МБ.

Фактически размер документа ограничивает размер текста запроса API индекса. Так как API индекса позволяет одновременно передать пакет из нескольких документов, фактически предельный размер зависит от количества документов в таком пакете. Для пакета с одним документом максимальный размер документа составляет 16 МБ данных JSON.

При оценке размера документов следует учитывать только те поля, которые могут использоваться службой поиска. Не нужно включать в эти вычисления двоичные или графические данные в исходных документах.  

## <a name="indexer-limits"></a>Ограничения индексатора

Для поддержания баланса и стабильности службы в целом применяется максимально допустимое время выполнения, но некоторые большие наборы данных требуют на индексирование больше времени, чем предусмотрено. Если задание индексирования не может завершиться в течение максимально допустимого времени ожидания, попробуйте запустить его по расписанию. Планировщик сохраняет сведения о состоянии индексирования. Если запланированное задание индексирования прервано по какой-либо причине, индексатор может продолжить выполнение с того же места при следующем запланированном выполнении.


| Ресурс | Бесплатный&nbsp;<sup>1</sup> | Базовый&nbsp;<sup>2</sup>| S1 | S2 | S3 | S3&nbsp;HD&nbsp;<sup>3</sup>|L1 |L2 |
| -------- | ----------------- | ----------------- | --- | --- | --- | --- | --- | --- |
| Максимальное число индексаторов |3 |5 или 15|50 |200 |200 |Недоступно |10 |10 |
| Максимальное количество источников данных |3 |5 или 15 |50 |200 |200 |Недоступно |10 |10 |
| Максимальное число наборов квалификационных навыков <sup>4</sup> |3 |5 или 15 |50 |200 |200 |Недоступно |10 |10 |
| Максимальная нагрузка индексирования на вызов |10 000 документов |Ограничивается только максимальным числом документов |Ограничивается только максимальным числом документов |Ограничивается только максимальным числом документов |Ограничивается только максимальным числом документов |Недоступно |Без ограничений |Без ограничений |
| Минимальное расписание | 5 мин |5 мин |5 мин |5 мин |5 мин |5 мин |5 мин | 5 мин |
| Максимальное время выполнения| 1–3 мин |24 часа |24 часа |24 часа |24 часа |Недоступно  |24 часа |24 часа |
| Максимальное время выполнения индексаторов с набором навыков <sup>5</sup> | 3–10 минут |2 часа |2 часа |2 часа |2 часа |Недоступно  |2 часа |2 часа |
| Индексатор BLOB-объектов: максимальный размер BLOB-объектов в МБ |16 |16 |128 |256 |256 |Недоступно  |256 |256 |
| Индексатор BLOB-объектов: максимальное число символов в содержимом, извлеченном из BLOB-объекта |32 000 |64 000 |4&nbsp;млн |8&nbsp;млн |16&nbsp;млн |Недоступно |4&nbsp;млн |4&nbsp;млн |

<sup>1</sup> Максимальное время выполнения индексатора для служб уровня "Бесплатный" составляет 3 минуты для источников больших двоичных объектов или 1 минуту для прочих источников данных. Для индексирования с использованием искусственного интеллекта и вызовами к Cognitive Services службам бесплатного уровня предоставляются 20 бесплатных транзакций в день, где транзакцией считается каждый документ, успешно прошедший через конвейер обогащения.

<sup>2</sup> Службы категории "Базовый", созданные до декабря 2017 г., имеют более низкие ограничения для индексов (5 вместо 15), источников данных и наборов навыков.

<sup>3</sup> Службы уровня S3 HD не поддерживают индексаторы.

<sup>4</sup> До 30 навыков на набор квалификационных навыков.

<sup>5</sup> Обогащение с помощью ИИ и анализ изображений требуют много вычислительных ресурсов и потребляют непропорциональные объемы доступных вычислительных мощностей. Время выполнения для таких рабочих нагрузок было уменьшено, чтобы оставить больше возможностей для других заданий в очереди.

> [!NOTE]
> Как указано в [ограничениях для индексов](#index-limits), индексаторы также применяют верхний предел в 3000 элементов по всем сложным коллекциям на документ, начиная с последней общедоступной версии API, которая поддерживает сложные типы (`2019-05-06`), и более поздних версий. Это означает, что ограничение не применяется к индексаторам, созданным с помощью API более ранних версий. Чтобы сохранить максимальную совместимость, ограничения **не будут применяться** даже к тем индексаторам, которые были созданы с помощью API более ранних версий, а затем обновлены с применением API версии `2019-05-06` или более поздней. Клиентам следует помнить о неблагоприятном влиянии очень больших сложных коллекций (о чем уже упоминалось ранее), и мы настоятельно рекомендуем создавать новые индексаторы только через API последней общедоступной версии.

## <a name="shared-private-link-resource-limits"></a>Ограничения общего ресурса частной ссылки

Индексаторы могут получать доступ к другим ресурсам Azure [через частные конечные точки](search-indexer-howto-access-private.md) , управляемые через [общий API ресурсов частной ссылки](/rest/api/searchmanagement/sharedprivatelinkresources). В этом разделе описываются ограничения, связанные с этой возможностью.

| Ресурс | Free | Basic | S1 | S2 | S3 | S3 HD | L1 | L2
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Поддержка индексатора закрытых конечных точек | Нет | Да | Да | Да | Да | Нет | Да | Да |
| Поддержка закрытых конечных точек для индексаторов с набором навыков<sup>1</sup> | Нет | Нет | Нет | Да | Да | Нет | Да | Да |
| Максимальное число частных конечных точек | Недоступно | 10 или 30 | 100 | 400 | 400 | Недоступно | 20 | 20 |
| Максимальное число различных типов ресурсов<sup>2</sup> | Недоступно | 4 | 7 | 15 | 15 | Недоступно | 4 | 4 |

<sup>1</sup> обогащение и анализ данных искусственного интеллекта являются ресурсоемкими и потребляют непропорциональные объемы доступной вычислительной мощности. По этой причине закрытые подключения на более низких уровнях отключаются, чтобы избежать негативного воздействия на производительность и стабильность самой службы поиска.

<sup>2</sup> количество уникальных типов ресурсов вычислено в виде числа уникальных `groupId` значений, используемых во всех общих ресурсах частной связи для данной службы поиска, независимо от состояния ресурса.

## <a name="synonym-limits"></a>Ограничения синонимов

Максимальное число сопоставлений синонимов зависит от уровня службы. Каждое правило может иметь до 20 расширений, которые обозначают эквивалентные понятия. Например, созданные ассоциации "cat" с "kitty", "feline" и "felis" (разговорные и биологические именования кошачьих) будут учитываться как 3 расширения.

| Ресурс | Free | Basic | S1 | S2 | S3 | S3-HD |L1 | L2 |
| -------- | -----|------ |----|----|----|-------|---|----|
| Максимальное число сопоставлений синонимов |3 |3|5 |10 |20 |20 | 10 | 10 |
| Максимальное количество правил на сопоставление |5000 |20 000|20 000 |20 000 |20 000 |20 000 | 20 000 | 20 000  |

## <a name="queries-per-second-qps"></a>Число запросов в секунду (QPS)

Оценка QPS должна отдельно проводиться каждым клиентом. Основными определителями QPS являются размер и сложность индексов, размер и сложность запросов, а также объем трафика. Если такие факторы неизвестны, оценка не будет информативной.

Прогнозируемость показателей повышается, если оцениваются службы, работающие в выделенных ресурсах (категорий "Базовый" и "Стандартный"). Можно точнее оценить QPS, так как вы можете контролировать больше параметров. Рекомендации по оценке см. в статье [Производительность и оптимизация службы "Когнитивный поиск Azure"](search-performance-optimization.md).

Для уровней "Оптимизировано для хранилища" (L1 и L2) следует ожидать меньшую пропускную способность обработки запросов и более высокую задержку по сравнению с уровнями "Стандартный".

## <a name="data-limits-ai-enrichment"></a>Ограничения по данным (обогащение с помощью ИИ)

Ограничение по данным применяется к [конвейеру обогащения с помощью ИИ](cognitive-search-concept-intro.md), который обращается к ресурсу Анализа текста для [распознавания сущностей](cognitive-search-skill-entity-recognition.md), [извлечения ключевых фраз](cognitive-search-skill-keyphrases.md), [анализа тональности](cognitive-search-skill-sentiment.md), [распознавания языка](cognitive-search-skill-language-detection.md) и [выявления конфиденциальной информации](cognitive-search-skill-pii-detection.md). Максимальный размер записи — 50 000 знаков по оценке [`String.Length`](/dotnet/api/system.string.length). Если вам нужно разбить данные перед отправкой в анализатор тональности, можно воспользоваться [навыком разделения текста](cognitive-search-skill-textsplit.md).

## <a name="throttling-limits"></a>Ограничения регулирования

К запросам поиска и индексирования будет применяться регулирование, когда система приближается к пиковой загрузке. Регулирование работает по-разному для разных API. API запросов (поиск, предложения и автозавершение) и API индексирования регулируются динамически в зависимости от нагрузки на службу. Для интерфейсов API индекса установлены статические ограничения частоты запросов. 

Действуют следующие ограничения по запросам для операций, связанных с индексом.

+ Получение списка индексов (GET /indexes): 5 запросов в секунду на единицу поиска.
+ Получение индекса (GET /indexes/myindex): 10 запросов в секунду на единицу поиска.
+ Создание индекса (POST /indexes): 12 запросов в минуту на единицу поиска.
+ Создание или обновление индекса (PUT /indexes/myindex): 6 запросов в секунду на единицу поиска.
+ Удаление индекса (DELETE /indexes/myindex): 12 запросов в минуту на единицу поиска. 

## <a name="api-request-limits"></a>Ограничения запросов к API
* Максимум 16 МБ на один запрос <sup>1</sup>
* Максимальная длина URL-адреса — 8 КБ.
* Максимум 1000 документов на одну операцию загрузки индексов, объединения или удаления.
* Максимум 32 поля в предложении $orderby.
* Максимальный размер поискового запроса — 32 766 байтов (32 КБ минус 2 байта) текста в кодировке UTF-8.

<sup>1</sup> В службе "Когнитивный поиск Azure" размер текста запроса не должен превышать 16 МБ. Это накладывает фактическое ограничение на отдельные поля или коллекции, для которых теоретические ограничения не установлены (дополнительные сведения о составе и ограничениях полей см. в [списке поддерживаемых типов данных](/rest/api/searchservice/supported-data-types)).

## <a name="api-response-limits"></a>Ограничения ответов API
* Максимум 1000 документов на одну страницу результатов поиска.
* Максимум 100 предложений на один запрос API предложений.

## <a name="api-key-limits"></a>Ограничения ключей API
Ключи API используются для проверки подлинности в службах. Существует два типа ключей. Ключи администратора указываются в заголовке запроса и предоставляют доступ к службе на чтение и запись. Ключи запросов доступны только для чтения, они указываются в URL-адресе и обычно передаются в клиентские приложения.

* Максимум 2 ключа администратора на одну службу.
* Максимум 50 ключей запросов на одну службу.