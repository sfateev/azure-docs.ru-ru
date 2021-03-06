---
title: Краткое руководство. Создание пула Apache Spark (предварительная версия) с помощью Synapse Studio
description: Создайте пул Apache Spark с помощью Synapse Studio, выполнив действия из этого руководства.
services: synapse-analytics
author: saveenr
ms.service: synapse-analytics
ms.topic: quickstart
ms.subservice: spark
ms.date: 3/19/2020
ms.author: saveenr
ms.reviewer: jrasnick
ms.openlocfilehash: cdf9a1f211e889146c9a6986eeea7502d3177057
ms.sourcegitcommit: eb6bef1274b9e6390c7a77ff69bf6a3b94e827fc
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/05/2020
ms.locfileid: "91260841"
---
# <a name="quickstart-create-an-apache-spark-pool-preview-using-synapse-studio"></a>Краткое руководство. Создание пула Apache Spark (предварительная версия) с помощью Synapse Studio

Azure Synapse Analytics предлагает различные аналитические модули, упрощающие прием, преобразование, моделирование, анализ и предоставление данных. Пул Apache Spark предоставляет возможности для вычислений больших данных из открытых источников. После создания пула Apache Spark в рабочей области Synapse данные можно загружать, моделировать, обрабатывать и обслуживать для получения аналитических сведений.  

Из этого краткого руководства вы узнаете, как создать пул Apache Spark в рабочей области Synapse с помощью Synapse Studio.

> [!IMPORTANT]
> Экземпляры Spark оплачиваются пропорционально за каждую минуту, независимо от их использования. Не забудьте завершить работу экземпляра Spark после его использования или задайте короткое время ожидания. Дополнительные сведения см. в разделе **Очистка ресурсов** этой статьи.

Если у вас еще нет подписки Azure, [создайте бесплатную учетную запись](https://azure.microsoft.com/free/), прежде чем начинать работу.

## <a name="prerequisites"></a>Предварительные требования

- Подписка Azure — [создайте бесплатную учетную запись](https://azure.microsoft.com/free/).
- [Рабочая область Synapse](./quickstart-create-workspace.md)

## <a name="sign-in-to-the-azure-portal"></a>Вход на портал Azure

Войдите на [портал Azure](https://portal.azure.com/)

## <a name="navigate-to-the-synapse-workspace"></a>Переход в рабочую область Synapse

1. Перейдите к рабочей области Synapse, в которой будет создан пул Apache Spark, введя имя службы (или напрямую имя ресурса) в строке поиска.
![Панель поиска на портале Azure с введенными рабочими областями Synapse.](media/quickstart-create-sql-pool/create-sql-pool-00a.png)
1. В списке рабочих областей введите имя (или часть имени) рабочей области, которую нужно открыть. В нашем примере будет использоваться рабочая область с именем **contosoanalytics**.
![Список рабочих областей Synapse, где отображаются только области с именем Contoso.](media/quickstart-create-sql-pool/create-sql-pool-00b.png)

## <a name="launch-synapse-studio"></a>Запуск Synapse Studio 

1. В окне обзора рабочей области выберите **Запуск Synapse Studio**, чтобы открыть расположение, в котором будет создан пул Apache Spark. В строке поиска введите имя службы или непосредственно ресурса.
![Окно обзора рабочей области Synapse на портале Azure, команда "Запуск Synapse Studio" выделена цветом.](media/quickstart-create-apache-spark-pool/create-spark-pool-studio-20.png)

## <a name="create-the-apache-spark-pool-in-synapse-studio"></a>Создание пула Apache Spark в Synapse Studio

1. На домашней странице Synapse Studio перейдите в **центр управления**, щелкнув в области навигации слева значок **Управление**.
![Домашняя страница Synapse Studio, раздел "Управление" выделен цветом.](media/quickstart-create-apache-spark-pool/create-spark-pool-studio-21.png)

1. В центре управления перейдите в раздел **Пулы Apache Spark** и просмотрите текущий список пулов Apache Spark, доступных в этой рабочей области.
![Центр управления Synapse Studio, выбран раздел пулов Apache Spark](media/quickstart-create-apache-spark-pool/create-spark-pool-studio-22.png)

1. Выберите **+ Создать**. Запустится мастер создания пула Apache Spark. 
![Список пулов Spark в центре управления Synapse Studio.](media/quickstart-create-apache-spark-pool/create-spark-pool-studio-23.png)

1. Введите следующие сведения на вкладке **Основные сведения**:

    | Параметр | Рекомендуемое значение | Описание |
    | :------ | :-------------- | :---------- |
    | **Имя пула Apache Spark** | contosospark | Это имя, которое будет у пула Apache Spark. |
    | **Размер узла** | "Small (4 vCPU / 32 GB)" (Малый (4 виртуальных ЦП / 32 ГБ)) | Задайте минимальный размер, чтобы сократить затраты по работе в рамках этого краткого руководства. |
    | **Автомасштабирование** | Выключено | Для целей этого краткого руководства автомасштабирование не требуется. |
    | **Количество узлов** | 8 | Задайте минимальный размер, чтобы сократить затраты на выполнение этого руководства.|
    
    ![Форма создания пула Apache Spark в Synapse Studio.](media/quickstart-create-apache-spark-pool/create-spark-pool-studio-24.png)
    > [!IMPORTANT]
    > Обратите внимание, что для имен, которые могут использовать пулы Apache Spark, существуют определенные ограничения. Имя должно содержать только буквы и цифры, его длина не должна превышать 15 символов. Имя должно начинаться с буквы, быть уникальными в рабочей области, но не должно содержать зарезервированные слова.

1. На вкладке "Дополнительные параметры" (следующая вкладка), оставьте значения по умолчанию и нажмите кнопку **Просмотр и создание** (мы не будем добавлять теги).
 ![Форма создания пула Apache Spark в Synapse Studio.](media/quickstart-create-apache-spark-pool/create-spark-pool-studio-25.png)

1. В этот раз мы не будем добавлять теги, поэтому выберите **Просмотр и создание**.

1. На вкладке **Просмотр и создание** убедитесь, что подробные сведения на основе ранее введенных данных указаны правильно, и нажмите кнопку **Создать**. 
 ![Форма создания пула Apache Spark в Synapse Studio.](media/quickstart-create-apache-spark-pool/create-spark-pool-studio-26.png)

1. Пул Apache Spark начнет процесс подготовки к работе.
![Форма создания пула Apache Spark в Synapse Studio.](media/quickstart-create-apache-spark-pool/create-spark-pool-studio-27.png)

1. После завершения подготовки новый пул Apache Spark появится в списке.
![Форма создания пула Apache Spark в Synapse Studio.](media/quickstart-create-apache-spark-pool/create-spark-pool-studio-28.png)

## <a name="clean-up-apache-spark-pool-resources-using-synapse-studio"></a>Очистка ресурсов пула Apache Spark с помощью Synapse Studio

Чтобы удалить пул Apache Spark из рабочей области с помощью Synapse Studio, выполните приведенные ниже действия.
> [!WARNING]
> При удалении пула Spark аналитический модуль будет удален из рабочей области. Вы больше не сможете подключиться к пулу, а все запросы, конвейеры и записные книжки, использующие этот пул Spark, перестанут работать.

Если вы хотите удалить пул Apache Spark, выполните следующие действия:

1. Перейдите к пулам Apache Spark в центре управления Synapse Studio.
1. Щелкните многоточие рядом с удаляемым пулом Apache (в данном случае **contosospark**), чтобы отобразить команды для пула Apache Spark.
![Список пулов Apache Spark с выбранным недавно созданным пулом](media/quickstart-create-apache-spark-pool/create-spark-pool-studio-29.png)
1. Нажмите кнопку **Удалить**
1. Подтвердите удаление и нажмите кнопку **Удалить**.
 ![Диалоговое окно для подтверждения удаления выбранного пула Apache Spark](media/quickstart-create-apache-spark-pool/create-spark-pool-studio-30.png)
1. После успешного завершения процесса пул Apache Spark больше не будет указываться в ресурсах рабочей области. 

## <a name="next-steps"></a>Дальнейшие действия

- См. [Краткое руководство. Создание пула Apache Spark в Synapse Studio с помощью веб-средств](quickstart-apache-spark-notebook.md).
- См. [Краткое руководство. Создание пула Apache Spark с помощью портала Azure](quickstart-create-apache-spark-pool-portal.md).
