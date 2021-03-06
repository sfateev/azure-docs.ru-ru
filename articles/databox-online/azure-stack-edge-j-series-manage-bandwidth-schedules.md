---
title: Azure Stackный графический процессор пограничной Pro Управление расписаниями пропускной способности | Документация Майкрософт
description: Описание использования портал Azure для управления расписаниями пропускной способности в графическом процессоре Azure Stack.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: how-to
ms.date: 08/28/2020
ms.author: alkohli
ms.openlocfilehash: a0d596c7c1046ea26ac389a48c17fa5abccbfd12
ms.sourcegitcommit: a2d8acc1b0bf4fba90bfed9241b299dc35753ee6
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/12/2020
ms.locfileid: "91951610"
---
# <a name="use-the-azure-portal-to-manage-bandwidth-schedules-on-your-azure-stack-edge-pro-gpu"></a>Использование портал Azure для управления расписаниями пропускной способности в видеопроцессоре Azure Stack ребра Pro 

<!--[!INCLUDE [applies-to-skus](../../includes/azure-stack-edge-applies-to-all-sku.md)]-->

В этой статье описывается, как управлять расписаниями пропускной способности на Azure Stack пограничных Pro. Расписания пропускной способности позволяют настраивать использование пропускной способности сети в разных графиках в разное время дня. Эти расписания можно применить к операциям передачи и загрузки с устройства в облако.

Вы можете добавлять, изменять или удалять расписания пропускной способности для Azure Stack пограничных Pro с помощью портал Azure.

Вы узнаете, как выполнять следующие задачи:

> [!div class="checklist"]
> * Добавление расписания
> * изменение расписания;
> * Удаление расписания


## <a name="add-a-schedule"></a>Добавление расписания

Чтобы добавить пользователя, выполните на портале Azure приведенные ниже шаги.

1. В портал Azure для ресурса Azure Stackного периметра перейдите в раздел **пропускная способность**.
2. В правой области нажмите кнопку **+ Добавить расписание**.

    ![Выбор пропускной способности](media/azure-stack-edge-j-series-manage-bandwidth-schedules/add-schedule-1.png)

3. В разделе **Добавить расписание** выполните действия ниже. 

   1. Укажите **начальный день**, **день окончания**, **время начала**и **время окончания** расписания.
   2. Установите флажок " **весь день** ", если расписание должно выполняться весь день.
   3. **Пропускная способность** — это пропускная способность в мегабит в секунду (Мбит/с), используемая устройством в операциях, в которых участвует облако (отправка и загрузка). Для этого поля укажите число от 20 до 2 147 483 647.
   4. Установите флажок в поле **Неограниченная пропускная способность**, чтобы избежать регулирования передачи и скачивания данных.
   5. Выберите **Добавить**.

      ![Добавление расписания](media/azure-stack-edge-j-series-manage-bandwidth-schedules/add-schedule-2.png)

3. Расписание будет создано с указанными параметрами. Это расписание затем появится в списке расписаний пропускной способности на портале.

    ![Обновленный список расписаний пропускной способности](media/azure-stack-edge-j-series-manage-bandwidth-schedules/add-schedule-3.png)

## <a name="edit-schedule"></a>Изменение расписания

Выполните следующие действия, чтобы изменить расписание пропускной способности.

1. В портал Azure перейдите к ресурсу Azure Stackного периметра и перейдите к **пропускной способности**. 
2. В списке расписаний пропускной способности выберите расписание, которое требуется изменить.
    ![Выбор расписания пропускной способности](media/azure-stack-edge-j-series-manage-bandwidth-schedules/modify-schedule-1.png)

3. Внесите необходимые изменения и сохраните их.

    ![Изменение пользователя](media/azure-stack-edge-j-series-manage-bandwidth-schedules/modify-schedule-2.png)

4. После изменения расписания список расписаний обновится.

    ![Изменение пользователя 2](media/azure-stack-edge-j-series-manage-bandwidth-schedules/modify-schedule-3.png)


## <a name="delete-a-schedule"></a>Удаление расписания

Выполните следующие действия, чтобы удалить расписание пропускной способности, связанное с устройством Azure Stack ребра Pro.

1. В портал Azure перейдите к ресурсу Azure Stackного периметра и перейдите к **пропускной способности**.  

2. В списке расписаний пропускной способности выберите расписание, которое требуется удалить. В окне **Изменить расписание** выберите **Удалить**. При появлении запроса на подтверждение нажмите кнопку **Да**.

   ![Удаление пользователя](media/azure-stack-edge-j-series-manage-bandwidth-schedules/delete-schedule-2.png)

3. После удаления расписания список расписаний обновится.


## <a name="next-steps"></a>Дальнейшие действия

- Сведения об управлении общими папками см. [здесь](azure-stack-edge-j-series-manage-shares.md).
