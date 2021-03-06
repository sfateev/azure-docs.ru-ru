---
title: Преобразование «ранжирование» в потоке данных сопоставления
description: Использование преобразования ранжирования потока данных в фабрике данных Azure создание столбца ранжирования
author: djpmsft
ms.author: daperlov
ms.reviewer: makromer
ms.service: data-factory
ms.topic: conceptual
ms.custom: seo-lt-2019
ms.date: 10/05/2020
ms.openlocfilehash: 8584d1b64191cc5258c6eeeef9ae4125bf1a2c65
ms.sourcegitcommit: 2e72661f4853cd42bb4f0b2ded4271b22dc10a52
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/14/2020
ms.locfileid: "92044740"
---
# <a name="rank-transformation-in-mapping-data-flow"></a>Преобразование «ранжирование» в потоке данных сопоставления 

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

Используйте преобразование «ранжирование» для формирования упорядоченного ранжирования на основе условий сортировки, заданных пользователем. 

> [!VIDEO https://www.microsoft.com/en-us/videoplayer/embed/RE4GGJo]

## <a name="configuration"></a>Параметр Configuration

![Параметры ранжирования](media/data-flow/rank-configuration.png "Параметры ранжирования")

**Без учета регистра:** Если столбец сортировки имеет тип String, то Регистр будет разделяться на ранжирование. 

**Сжатый:** Если параметр включен, столбец ранг будет иметь сжатый ранг. Каждый счетчик рангов будет иметь последовательный номер, а значения ранга не будут пропущены после каждой из них.

**Столбец ранга:** Имя созданного столбца Rank. Этот столбец будет иметь тип long.

**Условия сортировки:** Выберите столбцы, по которым выполняется сортировка, и порядок сортировки. Порядок определяет приоритет сортировки.

Приведенная выше конфигурация принимает входящие данные баскетбол и создает столбец Rank с именем "Поинтсранкинг". Строка с самым высоким значением столбца *PTS* будет иметь значение *поинтсранкинг* , равное 1.

## <a name="data-flow-script"></a>Скрипт потока данных

### <a name="syntax"></a>Синтаксис

```
<incomingStream>
    rank(
        desc(<sortColumn1>),
        asc(<sortColumn2>),
        ...,
        caseInsensitive: { true | false }
        dense: { true | false }
        output(<rankColumn> as long)
    ) ~> <sortTransformationName<>
```

### <a name="example"></a>Пример

![Параметры ранжирования](media/data-flow/rank-configuration.png "Параметры ранжирования")

Скрипт потока данных для указанной выше конфигурации ранжирования находится в следующем фрагменте кода.

```
PruneColumns
    rank(
        desc(PTS, true),
        caseInsensitive: false,
        output(pointsRanking as long),
        dense: false
    ) ~> RankByPoints
```

## <a name="next-steps"></a>Дальнейшие действия

Фильтрация строк на основе значений ранжирования с помощью [преобразования «фильтр](data-flow-filter.md)».
