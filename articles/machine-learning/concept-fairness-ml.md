---
title: Снижение равноправия в моделях машинного обучения (Предварительная версия)
titleSuffix: Azure Machine Learning
description: Сведения об объективности в моделях машинного обучения и о том, как пакет Fairlearn Python может помочь в создании более объективных моделей.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: luquinta
author: luisquintanilla
ms.date: 08/05/2020
ms.openlocfilehash: 3f051d9fc1599c0877e1e8a58935d09d224ce22b
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2020
ms.locfileid: "88689683"
---
# <a name="mitigate-fairness-in-machine-learning-models-preview"></a>Снижение равноправия в моделях машинного обучения (Предварительная версия)

Узнайте о равноправии в машинном обучении и о том, как пакет Python с открытым кодом [фаирлеарн](https://fairlearn.github.io/) может помочь в устранении проблем с равноправием в моделях машинного обучения. Если вы не собираетесь понимать проблемы с равноправием и не можете оценить равноправие при построении моделей машинного обучения, можно создать модели, которые выдают нечестные результаты.

Приведенная ниже сводка [руководства пользователя](https://fairlearn.github.io/user_guide/index.html) по пакету фаирлеарн с открытым исходным кодом описывает, как использовать его для оценки равноправия создаваемых систем AI.  Пакет Фаирлеарн с открытым исходным кодом может также предложить варианты, помогающие снизить или уменьшить количество проблем с равноправием.  Ознакомьтесь с [примерами записных книжек](https://github.com/Azure/MachineLearningNotebooks/tree/master/contrib/fairness) [, чтобы](how-to-machine-learning-fairness-aml.md) включить оценку равноправия для систем AI во время обучения на машинное обучение Azure.


## <a name="what-is-fairness-in-machine-learning-models"></a>Что такое равноправие в моделях машинного обучения?

>[!NOTE]
> Объективность — это социально-техническая задача. Многие аспекты объективности, такие как справедливость и правосудие, не сохраняются в количественных метриках объективности. Кроме того, многие количественные метрики объективности не могут выполняться одновременно. Цель с пакетом Фаирлеарн с открытым исходным кодом заключается в том, чтобы дать возможность людям оценивать различные стратегии влияния и риска. В конечном счете, ИТ-отдел создает модели искусственного интеллекта и машинного обучения, чтобы принимать компромиссы, соответствующие их сценариям.

Системы машинного обучения и искусственного интеллекта могут вести себя не объективно. Одним из способов определения необъективного поведения является его вредное воздействие или влияние на людей. Существует множество типов вредного воздействия, к которым может привести использование системы ИИ. Дополнительные сведения см. в [выступлении неурипс 2017 Кейт кравфорд](https://www.youtube.com/watch?v=fMym_BKWQzk) .

Два распространенных типа вредного воздействия, причинами которого является искусственный интеллект:

- Вред при выделении: система AI расширяет или удерживает возможности, ресурсы или сведения для определенных групп. В качестве примеров можно привести наем сотрудников, прием в учебные заведения и кредитование, в которых модель при выборе подходящих кандидатов может отдавать предпочтение определенной группе людей.

- Вредное воздействие качества обслуживания. Система ИИ не работает одинаково для всех групп людей. Например, система распознавания речи может не работать так же хорошо для женщин, как для мужчин.

Чтобы снизить необъективное поведение в системах ИИ, необходимо оценить и устранить эти вредные воздействия.

## <a name="fairness-assessment-and-mitigation-with-fairlearn"></a>Оценка и устранение рисков объективности с помощью Fairlearn

Fairlearn — это пакет Python с открытым кодом, позволяющий разработчикам систем машинного обучения оценивать объективность своих систем и устранять наблюдаемые проблемы объективности.

Пакет Фаирлеарн с открытым исходным кодом содержит два компонента:

- Панель мониторинга оценки. Мини-приложение записной книжки Jupyter для выполнения оценки влияния прогнозирования модели на различные группы. Оно также позволяет сравнивать несколько моделей, используя метрики объективности и производительности.
- Алгоритм устранения рисков. Набор алгоритмов для снижения необъективности в двоичной классификации и регрессии.

Вместе эти компоненты позволяют специалистам по обработке и анализу данных и руководителям достигать компромисса между объективностью и производительностью, а также определять подходящую стратегию устранения рисков.

## <a name="assess-fairness-in-machine-learning-models"></a>Оценка равноправия в моделях машинного обучения

В пакете Фаирлеарн с открытым исходным кодом равноправие является концептуальным, несмотря на то, что такой подход называется **равноправием группы**, который запрашивает: какие группы лиц подвергаются риску в случае возникновения ущерба? Соответствующие группы, также известны как подгруппы, определяются с помощью **конфиденциальных функций** или конфиденциальных атрибутов. Конфиденциальные функции передаются средству оценки в пакете Фаирлеарн с открытым исходным кодом в виде вектора или матрицы с именем  `sensitive_features` . Термин предполагает, что конструктор систем должен быть чувствительным к этим функциям при оценке объективности группы. 

Учитывать в том, что эти функции содержат влияние на конфиденциальность из-за конфиденциальных данных. Но слово "конфиденциальный" не подразумевает, что эти функции не могут использоваться для создания прогнозов.

>[!NOTE]
> Оценка равноправия не является чисто техническим упражнением.  Пакет Фаирлеарн с открытым исходным кодом может помочь оценить равноправие модели, но не будет выполнять оценку.  Пакет Фаирлеарн с открытым исходным кодом помогает определить количественные метрики для оценки равноправия, но разработчики также должны выполнить качественный анализ, чтобы оценить равноправие собственных моделей.  Конфиденциальные функции, упомянутые выше, являются примером такого рода качественного анализа.     

На этапе оценки объективность измеряется с помощью метрик различия. **Метрики различий** могут оценивать и сравнивать поведение модели в разных группах как в виде соотношений, так и в виде отличий. Пакет Фаирлеарн с открытым исходным кодом поддерживает два класса метрик нарушения четности:


- Различия в производительности модели. Эти наборы метрик вычисляют различие (отличие) в значениях выбранной метрики производительности в разных подгруппах. Некоторые примеры:

  - различие в степени правильности;
  - различие в частоте ошибок;
  - различие в точности;
  - различие в полноте;
  - различие в средней абсолютной погрешности (MAE);
  - многое другое.

- Различия в квоте отбора. В этой метрике содержатся отличия в квоте отбора между разными подгруппами. Примером этого может быть различие в квоте одобрения кредита. Квота выбора подразумевает долю точек данных в каждом классе, классифицированном как 1 (в двоичной классификации), или распределение значений прогнозирования (в регрессии).

## <a name="mitigate-unfairness-in-machine-learning-models"></a>Устранение недостоверности в моделях машинного обучения

### <a name="parity-constraints"></a>Ограничения четности

Пакет Фаирлеарн с открытым исходным кодом включает различные алгоритмы защиты от недостоверности. Эти алгоритмы поддерживают набор ограничений для поведения прогнозирования, которое называется **ограничением четности** или критерием. Для ограничений четности требуется, чтобы некоторые аспекты поведения прогнозирования сравнивались между группами, определяемыми конфиденциальными функциями (например, разные состояния гонки). Алгоритмы устранения рисков в пакете Фаирлеарн с открытым исходным кодом используют такие ограничения четности для устранения наблюдаемых проблем равноправия.

>[!NOTE]
> Устранение недостоверности в модели означает сокращение недостоверности, но это не позволяет полностью устранить эту недостоверность.  Алгоритмы устранения недостоверности в пакете Фаирлеарн с открытым исходным кодом могут предоставить предлагаемые стратегии по устранению рисков, чтобы снизить вероятность неуверенности в модели машинного обучения, но они не являются решениями для полного устранения недостоверности.  Могут существовать другие ограничения на четность или критерии, которые следует учитывать для каждой модели машинного обучения конкретного разработчика. Разработчики, использующие Машинное обучение Azure, должны самостоятельно определить, если устранение рисков позволяет устранить все недостоверности в предполагаемом использовании и развертывании моделей машинного обучения.  

Пакет Фаирлеарн с открытым исходным кодом поддерживает следующие типы ограничений четности: 

|Ограничение четности  | Назначение  |Задача машинного обучения  |
|---------|---------|---------|
|Демографическое равенство     |  Устранение вредных воздействий на распределение | Двоичная классификация, регрессия |
|Уравнивание шансов  | Диагностика причин вредных воздействий распределения и качества обслуживания | Двоичная классификация        |
|Равная возможная сделка | Диагностика причин вредных воздействий распределения и качества обслуживания | Двоичная классификация        |
|Потери ограниченной группы     |  Устранение вредного воздействие качества обслуживания | Регрессия |



### <a name="mitigation-algorithms"></a>Алгоритм устранения рисков

Пакет Фаирлеарн с открытым исходным кодом предоставляет алгоритмы предотвращения неуверенности в обработке и уменьшении недостоверности:

- Сокращение. Эти алгоритмы принимают стандартный механизм оценки машинного обучения (например, модель LightGBM) и создают набор переученных моделей с помощью последовательности повторного взвешивания наборов данных для обучения. Например, кандидаты определенного пола могут быть завышено взвешенными или занижено взвешенными, чтобы переучить модели и уменьшить различия в разных группах по половой принадлежности. Затем пользователи могут выбрать модель, обеспечивающую оптимальный компромисс между правильностью (или другой метрикой производительности) и различием, которая обычно должна основываться на бизнес-правилах и вычислениях затрат.  
- Постобработка. Эти алгоритмы принимают в качестве входных данных существующий классификатор и конфиденциальную функцию. Затем они наследуют преобразование прогнозирования классификатора, чтобы применить указанные ограничения объективности. Главным преимуществом оптимизации порогов является простота и гибкость, так как не требуется переучить модель. 

| Алгоритм | Описание | Задача машинного обучения | Конфиденциальные функции | Поддерживаемые ограничения четности | Тип алгоритма |
| --- | --- | --- | --- | --- | --- |
| `ExponentiatedGradient` | Сведения о справедливой классификации с использованием подхода по принципу "черный ящик" см. в [этой статье](https://arxiv.org/abs/1803.02453) | Двоичная классификация | категориальные; | [Демографическое равенство](#parity-constraints), [уравнивание шансов](#parity-constraints) | Сокращение |
| `GridSearch` | Подход с использованием принципа "черный ящик" описан в статье [Подход сокращения к справедливой классификации](https://arxiv.org/abs/1803.02453)| Двоичная классификация | Двоичные данные | [Демографическое равенство](#parity-constraints), [уравнивание шансов](#parity-constraints) | Сокращение |
| `GridSearch` | Подход с использованием принципа "черный ящик", который реализует вариант поиска в сетке справедливой регрессии с помощью алгоритма для потери ограниченной группы, описывается в [этой статье](https://arxiv.org/abs/1905.12843) | Регрессия | Двоичные данные | [Потери ограниченной группы](#parity-constraints) | Сокращение |
| `ThresholdOptimizer` | Сведения о постобработке на основе алгоритма см. в статье [Равенство возможностей в контролируемом обучении](https://arxiv.org/abs/1610.02413). Этот метод принимает в качестве входных данных существующий классификатор и конфиденциальную функцию и создает монотонное преобразование прогнозирования классификатора, чтобы применить указанные ограничения четности. | Двоичная классификация | категориальные; | [Демографическое равенство](#parity-constraints), [уравнивание шансов](#parity-constraints) | постобработка. |

## <a name="next-steps"></a>Дальнейшие действия

- Узнайте, как использовать различные компоненты, извлекая сведения о [GitHub](https://github.com/fairlearn/fairlearn/), [Руководство пользователя](https://fairlearn.github.io/user_guide/index.html), [примеры](https://fairlearn.github.io/auto_examples/)и примеры [записных книжек](https://github.com/fairlearn/fairlearn/tree/master/notebooks)фаирлеарн.
- Узнайте [, как](how-to-machine-learning-fairness-aml.md) включить оценку равноправия моделей машинного обучения в машинное обучение Azure.
- Дополнительные сценарии оценки равноправия в Машинное обучение Azure см. в [примерах записных книжек](https://github.com/Azure/MachineLearningNotebooks/tree/master/contrib/fairness) . 
