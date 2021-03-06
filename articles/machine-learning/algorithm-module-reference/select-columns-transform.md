---
title: 'Преобразование "Выбор столбцов": ссылка на модуль'
titleSuffix: Azure Machine Learning
description: Узнайте, как использовать модуль преобразования Select Columns в Машинное обучение Azure для создания преобразования, которое выбирает то же подмножество столбцов, что и в данном наборе данных.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: likebupt
ms.author: keli19
ms.date: 10/10/2020
ms.openlocfilehash: 14f08502f35afdc8a9a2cdc741b539b5f9cca712
ms.sourcegitcommit: ba7fafe5b3f84b053ecbeeddfb0d3ff07e509e40
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/12/2020
ms.locfileid: "91945603"
---
# <a name="select-columns-transform"></a>Преобразование "Выбор столбцов"

В этой статье описывается, как использовать модуль преобразования Select Columns в конструкторе Машинное обучение Azure. Целью модуля преобразования Select Columns является обеспечение того, что в нисходящих операциях машинного обучения используется прогнозируемый, последовательный набор столбцов.

Этот модуль полезен для таких задач, как оценка, для которой требуются определенные столбцы. Изменения в доступных столбцах могут нарушить работу конвейера или изменить результаты.

Для создания и сохранения набора столбцов используется преобразование выбор столбцов. Затем используйте модуль [Применить преобразование](apply-transformation.md) , чтобы применить эти параметры к новым данным.

## <a name="how-to-use-select-columns-transform"></a>Использование преобразования «выбор столбцов»

В этом сценарии предполагается, что вы хотите использовать выбор компонентов для создания динамического набора столбцов, который будет использоваться для обучения модели. Чтобы убедиться, что выбранные столбцы одинаковы для процесса оценки, используйте модуль преобразование выбор столбцов, чтобы записать выбор столбцов и применить их в любом расположении в конвейере.

1. Добавьте входной набор данных в конвейер в конструкторе.

2. Добавление экземпляра [компонента на основе фильтра](filter-based-feature-selection.md).

3. Подключите модули и настройте модуль выбора компонентов, чтобы автоматически найти ряд лучших компонентов во входном наборе данных.

4. Добавьте экземпляр [обучения модели](train-model.md) и используйте выходные данные [выбора компонентов на основе фильтра](filter-based-feature-selection.md) в качестве входных данных для обучения.

    > [!IMPORTANT]
    > Поскольку важность признаков основана на значениях в столбце, невозможно заранее выяснить, какие столбцы могут быть доступны для [обучения модели](train-model.md).  
5. Присоедините экземпляр модуля преобразования Select Columns. 

    На этом шаге создается выбор столбца как преобразование, которое можно сохранить или применить к другим наборам данных. Этот шаг гарантирует, что столбцы, указанные в выборе компонентов, будут сохранены для повторного использования другими модулями.

6. Добавьте модуль [Оценка модели](score-model.md) . 

   *Не подключайте входной набор данных.* Вместо этого добавьте модуль « [Применить преобразование](apply-transformation.md) » и Соедините выходные данные преобразования «выбор компонентов».

   Структура конвейера должна выглядеть следующим образом:

   > [!div class="mx-imgBorder"]
   > ![Пример конвейера](media/module/filter-based-feature-selection-score.png)

   > [!IMPORTANT]
   > Вы не можете применить [Выбор компонентов на основе фильтра](filter-based-feature-selection.md) к набору данных оценки и получить те же результаты. Поскольку выбор компонентов основан на значениях, он может выбрать другой набор столбцов, что приведет к сбою операции оценки.
    
7. Отправьте конвейер.

Этот процесс сохранения и применения выбора столбцов гарантирует, что одна и та же схема данных будет доступна для обучения и оценки.


## <a name="next-steps"></a>Дальнейшие действия

Ознакомьтесь с [набором доступных модулей](module-reference.md) в службе Машинного обучения Azure. 
