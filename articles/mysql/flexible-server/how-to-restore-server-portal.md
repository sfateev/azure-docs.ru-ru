---
title: Restore-портал Azure — гибкий сервер базы данных Azure для MySQL
description: В этой статье описывается, как выполнять операции восстановления в базе данных Azure для MySQL с помощью портал Azure.
author: kummanish
ms.author: manishku
ms.service: mysql
ms.topic: how-to
ms.date: 09/21/2020
ms.openlocfilehash: 1c81ddad8a11cbad361ff84caf6f7200a0c010d5
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2020
ms.locfileid: "90937295"
---
# <a name="point-in-time-restore-of-a-azure-database-for-mysql---flexible-server-preview"></a>Восстановление базы данных Azure для MySQL — гибкого сервера до точки во времени (Предварительная версия)


> [!IMPORTANT]
> Сейчас предоставляется общедоступная предварительная версия Гибкого сервера Базы данных Azure для MySQL.

Эта статья содержит пошаговые инструкции по выполнению восстановления на момент времени на гибком сервере с помощью резервных копий.

## <a name="prerequisites"></a>Предварительные требования

Вот что вам нужно, чтобы выполнить инструкции, приведенные в этом руководстве:

-   Необходимо иметь гибкий сервер базы данных Azure для MySQL.

## <a name="restore-to-the-latest-restore-point"></a>Восстановление до последней точки восстановления

Выполните следующие действия, чтобы восстановить гибкий сервер с помощью самой ранней существующей резервной копии.

1.  В [портал Azure](https://portal.azure.com/)выберите гибкий сервер, с которого требуется восстановить резервную копию.

2.  На панели слева щелкните **Обзор** .

3.  На странице Обзор нажмите кнопку **восстановить**.

    Заполнителе

4.  Будет показана страница восстановления с возможностью выбора между **последней точкой восстановления** и пользовательской точкой восстановления.

5.  Выберите **последнюю точку восстановления**.


6.  Укажите новое имя сервера в поле **восстановить в новый сервер** .

    :::image type="content" source="./media/concept-backup-restore/restore-blade-latest.png" alt-text="Самое раннее время восстановления":::

8.  Нажмите кнопку **ОК**.

9.  Будет отображено уведомление о том, что операция восстановления была инициирована.

## <a name="restoring-to-a-custom-restore-point"></a>Восстановление в настраиваемую точку восстановления

Выполните следующие действия, чтобы восстановить гибкий сервер с помощью самой ранней существующей резервной копии.

1.  В [портал Azure](https://portal.azure.com/)выберите гибкий сервер, с которого требуется восстановить резервную копию.

2.  На странице Обзор нажмите кнопку **восстановить**.

    Заполнителе

3.  Отобразится страница восстановление с возможностью выбора самой ранней точки восстановления и пользовательской точки восстановления.

4.  Выберите **настраиваемую точку восстановления**.

5.  Выберите дату и время.

6.  Укажите новое имя сервера в поле **восстановить в новый сервер** .

6.  Укажите новое имя сервера в поле **восстановить в новый сервер** . 
   
    :::image type="content" source="./media/concept-backup-restore/restore-blade-custom.png" alt-text="Самое раннее время восстановления":::
 
7.  Нажмите кнопку **ОК**.

8.  Будет отображено уведомление о том, что операция восстановления была инициирована.

## <a name="next-steps"></a>Дальнейшие шаги

Заполнитель
