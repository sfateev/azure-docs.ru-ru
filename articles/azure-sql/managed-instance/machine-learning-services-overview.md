---
title: Службы машинного обучения в Управляемый экземпляр SQL Azure (Предварительная версия)
description: В этой статье содержится обзор или Службы машинного обучения в Azure SQL Управляемый экземпляр.
services: sql-database
ms.service: sql-managed-instance
ms.subservice: machine-learning
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: garyericson
ms.author: garye
ms.reviewer: sstein, davidph
manager: cgronlun
ms.date: 06/03/2020
ms.openlocfilehash: d7a3c86f3d9cf083a8746f753b8c5287c774a93e
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2020
ms.locfileid: "91263273"
---
# <a name="machine-learning-services-in-azure-sql-managed-instance-preview"></a>Службы машинного обучения в Управляемый экземпляр SQL Azure (Предварительная версия)

Службы машинного обучения — это функция Управляемый экземпляр SQL Azure (Предварительная версия), которая обеспечивает машинное обучение в базе данных, поддерживающее скрипты Python и R. Эта функция включает в себя пакеты Microsoft Python и R для высокопроизводительной прогнозной аналитики и машинного обучения. Реляционные данные можно использовать в скриптах с помощью хранимых процедур, скрипта T-SQL, содержащего инструкции Python или R, или кода Python или R, содержащего T-SQL.

> [!IMPORTANT]
> Службы машинного обучения — это функция Управляемого экземпляра SQL Azure, которая в настоящее время предоставляется в общедоступной предварительной версии.
> Эта функция предварительной версии изначально доступна в ограниченном числе регионов в США, Азии и Австралии, а дополнительные регионы добавляются позже.
>
> Эта предварительная версия предоставляется без соглашения об уровне обслуживания и не рекомендована для использования рабочей среде. Некоторые функции могут не поддерживаться или их возможности могут быть ограничены.
> Дополнительные сведения см. в статье [Дополнительные условия использования предварительных выпусков Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).
>
> См. раздел [Зарегистрируйтесь для получения предварительной версии](#signup), указанный ниже.

## <a name="what-is-machine-learning-services"></a>Что такое Службы машинного обучения?

Службы машинного обучения в Azure SQL Управляемый экземпляр позволяет выполнять скрипты Python и R в базе данных. С их помощью можно подготавливать и очищать данные, выполнять проектирование признаков, а также обучать, оценивать и развертывать модели машинного обучения в базе данных. Этот компонент выполняет скрипты там, где хранятся данные, и устраняет необходимость перемещения данных по сети на другой сервер.

Используйте Службы машинного обучения с поддержкой R/Python в Управляемый экземпляр SQL Azure:

- **Выполнение скриптов r и Python для подготовки данных и обработки данных общего назначения** . Теперь вы можете перенести сценарии r/Python в Azure SQL управляемый экземпляр, где хранятся ваши данные, вместо того чтобы перемещать данные на другой сервер для выполнения скриптов R и Python. Можно исключить необходимость в перемещении данных и связанных с ними проблемах, связанных с задержкой, безопасностью и соответствием.

- **Обучение моделей машинного обучения в базе данных** . Вы можете обучить модели с помощью любых алгоритмов с открытым исходным кодом. Вы можете легко масштабировать обучение по всему набору данных, не полагаясь на образцы наборов данных, извлеченных из базы.

- **Развертывание моделей и скриптов в рабочей среде в хранимых процедурах** . скрипты и обученные модели могут работать просто путем их внедрения в хранимые процедуры T-SQL. Приложения, подключающиеся к Azure SQL Управляемый экземпляр могут воспользоваться преимуществами прогнозов и аналитики в этих моделях, просто вызвав хранимую процедуру. Можно также использовать собственную функцию ПРОГНОЗИРОВАНИя T-SQL, чтобы эксплуатацию модели для быстрой оценки в сценариях оценки с высокой степенью актуальности в реальном времени.

Базовые распределения Python и R включены в Службы машинного обучения. Вы можете установить и использовать платформы и пакеты с открытым исходным кодом, такие как PyTorch, TensorFlow и scikit-learn, в дополнение к пакетам Microsoft [revoscalepy](https://docs.microsoft.com/sql/advanced-analytics/python/ref-py-revoscalepy) и [microsoftml](https://docs.microsoft.com/sql/advanced-analytics/python/ref-py-microsoftml) для Python и [RevoScaleR](https://docs.microsoft.com/sql/advanced-analytics/r/ref-r-revoscaler), [MicrosoftML](https://docs.microsoft.com/sql/advanced-analytics/r/ref-r-microsoftml), [OLAP](https://docs.microsoft.com/sql/advanced-analytics/r/ref-r-olapr) и [sqlrutils](https://docs.microsoft.com/sql/advanced-analytics/r/ref-r-sqlrutils) для R.

<a name="signup"></a>

## <a name="sign-up-for-the-preview"></a>Зарегистрироваться для получения предварительной версии

Эта ограниченная общедоступная Предварительная версия подпадает под [условия предварительной версии Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/). 

Если вы хотите присоединиться к программе предварительной версии и принять эти условия, вы можете запросить регистрацию, создав запрос в службу поддержки Azure по адресу [**https://azure.microsoft.com/support/create-ticket/**](https://azure.microsoft.com/support/create-ticket/) . 

1. Выберите следующие параметры.
   - Тип проблемы — **Техническая**
   - Подписка — *выберите подписку* .
   - Служба — **управляемый экземпляр базы данных SQL**
   - Сводка — *введите краткое описание запроса* .
   - Тип проблемы — **службы машинного обучения для управляемый экземпляр SQL (Предварительная версия)**
   - Подтип проблемы — **другие проблемы или вопросы**

1. Нажмите кнопку **Далее: решения**.

1. Прочтите сведения о предварительной версии и нажмите кнопку **сведения**.

1. В поле **Описание**введите особенности запроса, включая имя логического сервера, регион и идентификатор подписки, которые вы хотите зарегистрировать в предварительной версии. При необходимости введите другие сведения.

1. По завершении нажмите кнопку **Далее: Проверка + создать**, а затем нажмите кнопку **создать**.

Как только вы зарегистрируетесь в программе, корпорация Майкрософт подключит вас к общедоступной предварительной версии и позволит использование Служб машинного обучения для вашей существующей или новой базы данных.

Не следует использовать Службы машинного обучения с Управляемыми экземплярами SQL для рабочих нагрузок в общедоступной предварительной версии.

## <a name="next-steps"></a>Дальнейшие шаги

- Ознакомьтесь с [основными отличиями от SQL Server службы машинного обучения](machine-learning-services-differences.md).
- Сведения об использовании Python в Службы машинного обучения см. в разделе [выполнение скриптов Python](https://docs.microsoft.com/sql/machine-learning/tutorials/quickstart-python-create-script?context=/azure/azure-sql/managed-instance/context/ml-context&view=sql-server-ver15).
- Сведения об использовании R в Службы машинного обучения см. в разделе [выполнение скриптов r](https://docs.microsoft.com/sql/machine-learning/tutorials/quickstart-r-create-script?context=/azure/azure-sql/managed-instance/context/ml-context&view=sql-server-ver15).
- Дополнительные сведения о машинном обучении на других платформах SQL см. в [документации по машинному обучению SQL](https://docs.microsoft.com/sql/machine-learning/).
