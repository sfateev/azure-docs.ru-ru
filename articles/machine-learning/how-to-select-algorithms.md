---
title: Выбор алгоритма машинного обучения
titleSuffix: Azure Machine Learning
description: Выбор алгоритмов Машинное обучение Azure для контролируемого и неконтролируемого обучения в процессе кластеризации, классификации или регрессии.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.custom: how-to
author: FrancescaLazzeri
ms.author: lazzeri
ms.reviewer: cgronlun
ms.date: 05/07/2020
ms.openlocfilehash: 5ad0d2c520d5347e36a9cead4ed6c5526d885ca4
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2020
ms.locfileid: "87319530"
---
# <a name="how-to-select-algorithms-for-azure-machine-learning"></a>Выбор алгоритмов для Машинное обучение Azure

Распространенный вопрос: «какой алгоритм машинного обучения следует использовать?». Выбранный алгоритм зависит главным образом от двух различных аспектов сценария обработки и анализа данных:

 - **Что вы хотите сделать с данными?** В частности, каков бизнес-вопрос, который вы хотите ответить, обучение из прошлых данных?

 - **Каковы требования к сценарию обработки и анализа данных?** В частности, Какова точность, время обучения, линейность, количество параметров и число функций, поддерживаемых решением?

 ![Рекомендации по выбору алгоритмов: что вы хотите знать? Каковы требования к сценариям?](./media/how-to-select-algorithms/how-to-select-algorithms.png)

## <a name="business-scenarios-and-the-machine-learning-algorithm-cheat-sheet"></a>Бизнес-сценарии и лист Машинное обучение Algorithm Памятка по

С помощью [памятка по машинное обучение Azureного алгоритма](https://docs.microsoft.com/azure/machine-learning/algorithm-cheat-sheet?WT.mc_id=docs-article-lazzeri) вы будете в первую очередь: **что нужно сделать с данными**? На листе Машинное обучение Algorithm Памятка по (анализ алгоритма) найдите нужную задачу, а затем найдите алгоритм [конструктора машинное обучение Azure](https://docs.microsoft.com/azure/machine-learning/concept-designer?WT.mc_id=docs-article-lazzeri) для решения прогнозной аналитики. 

Машинное обучение Designer предоставляет комплексный портфель алгоритмов, таких как [лес решений в многоклассовых решениях](https://docs.microsoft.com/azure/machine-learning/algorithm-module-reference/multiclass-decision-forest?WT.mc_id=docs-article-lazzeri), [системы рекомендаций](https://docs.microsoft.com/azure/machine-learning/algorithm-module-reference/evaluate-recommender?WT.mc_id=docs-article-lazzeri), [регрессия нейронных сетей](https://docs.microsoft.com/azure/machine-learning/algorithm-module-reference/neural-network-regression?WT.mc_id=docs-article-lazzeri), [Многоклассовая нейронная сеть](https://docs.microsoft.com/azure/machine-learning/algorithm-module-reference/multiclass-neural-network?WT.mc_id=docs-article-lazzeri)и [кластеризация на основе K-средних](https://docs.microsoft.com/azure/machine-learning/algorithm-module-reference/k-means-clustering?WT.mc_id=docs-article-lazzeri). Каждый алгоритм предназначен для решения различных типов проблем машинного обучения. Полный список, а также описание работы каждого алгоритма и настройки параметров для оптимизации алгоритма см. в [справочнике по алгоритму конструктора машинное обучение и модулю](https://docs.microsoft.com/azure/machine-learning/algorithm-module-reference/module-reference?WT.mc_id=docs-article-lazzeri) .

> [!NOTE]
> Чтобы скачать таблицу алгоритма машинного обучения Памятка по, перейдите на [лист Памятка по алгоритма машинного обучения Azure](https://docs.microsoft.com/azure/machine-learning/algorithm-cheat-sheet?WT.mc_id=docs-article-lazzeri).
> 
> 

Вместе с рекомендациями на листе Машинное обучение Azure Algorithm Памятка по следует учитывать другие требования при выборе алгоритма машинного обучения для решения. Ниже приведены дополнительные факторы, которые следует учитывать, такие как точность, время обучения, линейность, количество параметров и число функций.

## <a name="comparison-of-machine-learning-algorithms"></a>Сравнение алгоритмов машинного обучения

Некоторые алгоритмы обучения делают определенные предположения о структуре данных или желаемых результатов. Если вы сможете найти тот алгоритм, который соответствует вашим потребностям, с ним вы сможете получить более точные результаты, более точные прогнозы и сократить время обучения.

В следующей таблице перечислены некоторые из наиболее важных характеристик алгоритмов из семейств классификации, регрессии и кластеризации.

| **Алгоритм** | **Точность** | **Время обучения** | **Линейность** | **Параметры** | **Примечания** |
| --- |:---:|:---:|:---:|:---:| --- |
| **Семейство классификаций** | | | | | |
| [Логистическая регрессия двух классов](https://docs.microsoft.com/azure/machine-learning/algorithm-module-reference/two-class-logistic-regression?WT.mc_id=docs-article-lazzeri) |Хорошо  |быстрый; |Да |4 | |
| [Лес решений с двумя классами](https://docs.microsoft.com/azure/machine-learning/algorithm-module-reference/two-class-decision-forest?WT.mc_id=docs-article-lazzeri) |Высокая |Средняя |Нет |5 |Показывает меньшее время оценки. Предложение не работает с Многоклассовый классификатор "один — все" из-за более медленных оценок, вызванных действовать блокировкой в накоплении прогнозов дерева |
| [Дерево решений, увеличивающееся двумя классами](https://docs.microsoft.com/azure/machine-learning/algorithm-module-reference/two-class-boosted-decision-tree?WT.mc_id=docs-article-lazzeri) |Высокая |Средняя |Нет |6 |Большой объем памяти |
| [Нейронная сеть с двумя классами](https://docs.microsoft.com/azure/machine-learning/algorithm-module-reference/two-class-neural-network?WT.mc_id=docs-article-lazzeri) |Хорошо |Средняя |Нет |8 | |
| [Среднее перцептрона, основанное на двух классах](https://docs.microsoft.com/azure/machine-learning/algorithm-module-reference/two-class-averaged-perceptron?WT.mc_id=docs-article-lazzeri) |Хорошо |Средняя |Да |4 | |
| [Поддержка векторного компьютера с двумя классами](https://docs.microsoft.com/azure/machine-learning/algorithm-module-reference/two-class-support-vector-machine?WT.mc_id=docs-article-lazzeri) |Хорошо |быстрый; |Да |5 |Подходит для больших наборов функций |
| [Логистическая регрессия в многоклассовой](https://docs.microsoft.com/azure/machine-learning/algorithm-module-reference/multiclass-logistic-regression?WT.mc_id=docs-article-lazzeri) |Хорошо |быстрый; |Да |4 | |
| [Лес решений в многоклассовых решениях](https://docs.microsoft.com/azure/machine-learning/algorithm-module-reference/multiclass-decision-forest?WT.mc_id=docs-article-lazzeri) |Высокая |Средняя |Нет |5 |Показывает меньшее время оценки |
| [Многоклассическое дерево принятия решений](https://docs.microsoft.com/azure/machine-learning/algorithm-module-reference/multiclass-boosted-decision-tree?WT.mc_id=docs-article-lazzeri) |Высокая |Средняя |Нет |6 | Как правило, повышение точности с небольшим риском меньшего объема |
| [Многоклассовая нейронная сеть](https://docs.microsoft.com/azure/machine-learning/algorithm-module-reference/multiclass-neural-network?WT.mc_id=docs-article-lazzeri) |Хорошо |Средняя |Нет |8 | |
| [Многоклассовая, одна-VS-все](https://docs.microsoft.com/azure/machine-learning/algorithm-module-reference/one-vs-all-multiclass?WT.mc_id=docs-article-lazzeri) | - | - | - | - |Просмотрите свойства выбранного двухклассового метода |
| **Семейство регрессии** | | | | | |
| [Линейная регрессия](https://docs.microsoft.com/azure/machine-learning/algorithm-module-reference/linear-regression?WT.mc_id=docs-article-lazzeri) |Хорошо |быстрый; |Да |4 | |
| [Регрессия леса принятия решений](https://docs.microsoft.com/azure/machine-learning/algorithm-module-reference/decision-forest-regression?WT.mc_id=docs-article-lazzeri)|Высокая |Средняя |Нет |5 | |
| [Регрессия повышенного дерева принятия решений](https://docs.microsoft.com/azure/machine-learning/algorithm-module-reference/boosted-decision-tree-regression?WT.mc_id=docs-article-lazzeri) |Высокая |Средняя |Нет |6 |Большой объем памяти |
| [Регрессия нейронной сети](https://docs.microsoft.com/azure/machine-learning/algorithm-module-reference/neural-network-regression?WT.mc_id=docs-article-lazzeri) |Хорошо |Средняя |Нет |8 | |
| **Семейство кластеров** | | | | | |
| [Кластеризация K-средних](https://docs.microsoft.com/azure/machine-learning/algorithm-module-reference/k-means-clustering?WT.mc_id=docs-article-lazzeri) |Высокая |Средняя |Да |8 |Алгоритм кластеризации |

## <a name="requirements-for-a-data-science-scenario"></a>Требования к сценарию обработки и анализа данных

Зная, что вы хотите сделать с данными, необходимо определить дополнительные требования для решения. 

Сделайте выбор и, возможно, компромиссы для следующих требований:

- Точность
- Время обучения
- Линейность
- Количество параметров
- Количество функций

## <a name="accuracy"></a>Точность

Точность в машинном обучении измеряет эффективность модели в виде отношения истинных результатов к общему количеству вариантов. В конструкторе Машинное обучение, [модуль «анализ модели](https://docs.microsoft.com/azure/machine-learning/algorithm-module-reference/evaluate-model?WT.mc_id=docs-article-lazzeri) » вычисляет набор отраслевых метрик оценки. Этот модуль можно использовать для измерения точности обученной модели.

Получить наиболее точный возможный ответ не всегда нужно. Иногда достаточно приближенного ответа в зависимости от того, для чего он используется. Если это так, вы можете значительно сократить время обработки, заменив более приблизительными методами. Приблизительные методы также обычно позволяют избежать чрезмерного перегонки.

Существует три способа использования модуля «анализ модели»:

- Создание оценок для обучающих данных с целью оценки модели
- Создание оценок для модели, но сравнение этих оценок с показателями в зарезервированном проверочном наборе
- Сравнение оценок для двух различных, но связанных моделей с использованием одного набора данных

Полный список метрик и подходов, которые можно использовать для вычисления точности моделей машинного обучения, см. в разделе [Вычисление модуля модели](https://docs.microsoft.com/azure/machine-learning/algorithm-module-reference/evaluate-model?WT.mc_id=docs-article-lazzeri).

## <a name="training-time"></a>Время обучения

В защищенном учебном обучении обозначает использование исторических данных для создания модели машинного обучения, которая позволяет избежать ошибок. Количество минут или часов, необходимых для обучения модели, сильно отличается для различных алгоритмов. Время обучения часто тесно связано с точностью; один из них обычно сопровождается другим. 

Кроме того, некоторые алгоритмы более чувствительны к количеству точек данных, чем другие. Вы можете выбрать конкретный алгоритм, так как имеется ограничение по времени, особенно если набор данных большой.

В Машинное обучение Designer создание и использование модели машинного обучения обычно выполняется в три этапа:

1.  Настройте модель, выбрав определенный тип алгоритма, а затем определив его параметры или параметры. 

2.  Предоставьте набор данных с меткой и совместимый с алгоритмом. Подключите данные и модель к [модулю обучение модели](https://docs.microsoft.com/azure/machine-learning/algorithm-module-reference/train-model?WT.mc_id=docs-article-lazzeri).

3.  После завершения обучения используйте обученную модель с одним из [модулей оценки](https://docs.microsoft.com/azure/machine-learning/algorithm-module-reference/score-model?WT.mc_id=docs-article-lazzeri) для создания прогнозов по новым данным.

## <a name="linearity"></a>Линейность

Линейность в статистике и машинном обучении означает, что между переменной и константой в наборе данных существует линейная связь. Например, алгоритмы линейной классификации предполагают, что классы могут быть разделены прямой линией (или ее самым большим объемом).

Линейность используется во многих алгоритмах машинного обучения. В Машинное обучение Azure Designer они включают: 

- [Логистическая регрессия в многоклассовой](https://docs.microsoft.com/azure/machine-learning/algorithm-module-reference/multiclass-logistic-regression?WT.mc_id=docs-article-lazzeri)
- [Логистическая регрессия двух классов](https://docs.microsoft.com/azure/machine-learning/algorithm-module-reference/two-class-logistic-regression?WT.mc_id=docs-article-lazzeri)
- [Поддержка векторных компьютеров](https://docs.microsoft.com/azure/machine-learning/algorithm-module-reference/two-class-support-vector-machine?WT.mc_id=docs-article-lazzeri)  

Алгоритмы линейной регрессии предполагают, что тренды данных следуют прямой линии. Это предположение неплохо для некоторых проблем, но для других снижает точность. Несмотря на их недостатки, в качестве первой стратегии популярны линейные алгоритмы. Обычно они алгоритмически просты и быстро осваиваются.

![Нелинейная граница класса](./media/how-to-select-algorithms/nonlinear-class-boundary.png)

***Нелинейная граница класса***. *полагается на алгоритм линейной классификации, что приведет к низкой точности.*

![Данные с нелинейным трендом](./media/how-to-select-algorithms/nonlinear-trend.png)

***Данные с нелинейной тенденцией***: *использование метода линейной регрессии приведет к значительно большему количеству ошибок, чем необходимо.*

## <a name="number-of-parameters"></a>Количество параметров

Параметры являются теми регуляторами, которые специалист по данным поворачивает при настройке алгоритма. Это числа, влияющие на поведение алгоритма, например погрешность ошибок или число итераций, а также варианты поведения алгоритма. Время обучения и точность алгоритма иногда могут быть конфиденциальными для получения только правильных настроек. Как правило, алгоритмы с большим количеством параметров занимают больше всего проб и ошибок, чтобы найти хорошее сочетание.

Кроме того, в конструкторе Машинное обучение доступен [Модуль Настройка модели](https://docs.microsoft.com/azure/machine-learning/algorithm-module-reference/tune-model-hyperparameters?WT.mc_id=docs-article-lazzeri) . Цель этого модуля — определить оптимальные параметры для модели машинного обучения. Модуль создает и тестирует несколько моделей, используя различные сочетания параметров. Он сравнивает метрики по всем моделям, чтобы получить сочетания параметров. 

Хотя это отличный способ убедиться в том, что вы разделили пространство параметров, время, необходимое для обучения модели, увеличивается экспоненциально с числом параметров. Плюсом является то, что наличие большого количества параметров обычно означает, что алгоритм имеет большую гибкость. Это часто достигает очень хорошей точности, при условии, что вы можете найти правильное сочетание настроек параметров.

## <a name="number-of-features"></a>Количество функций

В машинном обучении компонент — это количественная переменная, которую вы пытаетесь проанализировать. Для некоторых типов данных количество функций может быть очень большим по сравнению с количеством точек данных. Это часто происходит с генетическими или текстовыми данными. 

Большое количество функций может затормозить некоторые алгоритмы обучения, что делает время обучения унфеасибли длинным. [Поддержка векторных компьютеров](https://docs.microsoft.com/azure/machine-learning/algorithm-module-reference/two-class-support-vector-machine?WT.mc_id=docs-article-lazzeri) особенно хорошо подходит для сценариев с большим количеством функций. По этой причине они использовались во многих приложениях для извлечения информации в классификацию текста и изображений. Для задач классификации и регрессии можно использовать векторные компьютеры поддержки.

Выбор компонентов относится к процессу применения статистических тестов к входным данным с учетом заданных выходных данных. Цель состоит в том, чтобы определить, какие столбцы являются более прогнозируемыми для выходных данных. [Модуль выбора компонентов на основе фильтра](https://docs.microsoft.com/azure/machine-learning/algorithm-module-reference/filter-based-feature-selection?WT.mc_id=docs-article-lazzeri) в конструкторе машинное обучение предоставляет несколько алгоритмов выбора компонентов для выбора. Модуль включает методы корреляции, такие как корреляция Пирсона и значения хи-квадрат.

Можно также воспользоваться [модулем важность функции перестановки](https://docs.microsoft.com/azure/machine-learning/algorithm-module-reference/permutation-feature-importance?WT.mc_id=docs-article-lazzeri) , чтобы вычислить набор оценок важности признаков для набора данных. Эти показатели можно использовать для определения лучших функций, используемых в модели.

## <a name="next-steps"></a>Дальнейшие действия

 - [Дополнительные сведения о конструкторе Машинное обучение Azure](https://docs.microsoft.com/azure/machine-learning/concept-designer?WT.mc_id=docs-article-lazzeri)
 - Описание всех алгоритмов машинного обучения, доступных в Машинное обучение Azure Designer, см. в разделе [Справочник по алгоритмам и модулям конструктора машинное обучение](https://docs.microsoft.com/azure/machine-learning/algorithm-module-reference/module-reference?WT.mc_id=docs-article-lazzeri) .
 - Сведения о связи между глубоким обучением, машинным обучением и AI см. в статье [глубокое обучение и машинное обучение](https://docs.microsoft.com/azure/machine-learning/concept-deep-learning-vs-machine-learning?WT.mc_id=docs-article-lazzeri)
