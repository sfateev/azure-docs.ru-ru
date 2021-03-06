---
title: Краткое руководство. Связывание рабочей области Power BI с рабочей областью Synapse
description: Действия, описанные в этом руководстве, позволят связать рабочую область Power BI с рабочей областью Azure Synapse Analytics.
services: synapse-analytics
author: jocaplan
ms.service: synapse-analytics
ms.topic: quickstart
ms.subservice: ''
ms.date: 04/29/2020
ms.author: jocaplan
ms.reviewer: jrasnick
ms.openlocfilehash: 9b4b9da37b753418773141f832f253003ad40014
ms.sourcegitcommit: eb6bef1274b9e6390c7a77ff69bf6a3b94e827fc
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/05/2020
ms.locfileid: "91260466"
---
# <a name="quickstart-linking-a-power-bi-workspace-to-a-synapse-workspace"></a>Краткое руководство. Связывание рабочей области Power BI с рабочей областью Synapse

В этом кратком руководстве вы узнаете, как подключить рабочую область Power BI к рабочей области Synapse Analytics для создания новых отчетов и наборов данных Power BI с помощью Synapse Studio (предварительная версия).

Если у вас еще нет подписки Azure, [создайте бесплатную учетную запись, прежде чем начинать работу](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Предварительные требования

- [Создайте рабочую область Azure Synapse и связанную с ней учетную запись хранения](quickstart-create-workspace.md).
- [Рабочая область Power BI Professional или Premium](https://docs.microsoft.com/power-bi/service-create-the-new-workspaces)

## <a name="link-power-bi-workspace-to-your-synapse-workspace"></a>Связывание рабочей области Power BI с рабочей областью Synapse

1. Откройте Synapse Studio и щелкните **Управление**.

    ![Действие "управление" в Synapse Studio.](media/quickstart-link-powerbi/synapse-studio-click-manage.png)

2. В разделе **Внешние подключения** щелкните **Связанные службы**.

    ![Выделен элемент "Связанные службы".](media/quickstart-link-powerbi/manage-click-linked-services.png)

3. Щелкните **+ Создать**.

    ![Выделен элемент "+ Новые связанные службы".](media/quickstart-link-powerbi/new-highlighted.png)

4. Щелкните **Power BI** и выберите **Продолжить**.

    ![Отображение связанной службы Power BI.](media/quickstart-link-powerbi/powerbi-linked-service.png)

5. Введите имя для связанной службы и выберите рабочую область из раскрывающегося списка.

    ![Отображение конфигурации связанной службы Power BI.](media/quickstart-link-powerbi/workspace-link-dialog.png)

6. Нажмите кнопку **Создать**.

## <a name="view-power-bi-workspace-in-synapse-studio"></a>Просмотр рабочей области Power BI в Synapse Studio

После связывания рабочих областей вы сможете просматривать наборы данных Power BI, изменять и создавать новые отчеты Power BI из Synapse Studio.

1. Щелкните **Develop** (Разработка).

    ![Действие "Разработка" в Synapse Studio.](media/quickstart-link-powerbi/synapse-studio-click-develop.png)

2. Разверните Power BI и рабочую область, которую вы намерены использовать.

    ![Разверните Power BI и рабочую область.](media/quickstart-link-powerbi/develop-expand-powerbi.png)

Новые отчеты можно создать, щелкнув **+** в верхней части вкладки **Разработка**. Чтобы изменить существующий отчет, щелкните имя этого отчета. Все сохраненные изменения будут записаны обратно в рабочую область Power BI.

![Просмотр и редактирование отчета Power BI](media/quickstart-link-powerbi/powerbi-report.png)


## <a name="next-steps"></a>Дальнейшие действия

Дополнительные сведения: [Создание отчета Power BI о файлах, хранящихся в службе хранилища Azure](sql/tutorial-connect-power-bi-desktop.md).
