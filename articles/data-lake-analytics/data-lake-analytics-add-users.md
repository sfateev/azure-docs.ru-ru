---
title: Добавление пользователей к учетной записи Azure Data Lake Analytics
description: Узнайте, как правильно добавить пользователей в учетную запись Data Lake Analytics с помощью мастера добавления пользователей и Azure PowerShell.
services: data-lake-analytics
ms.service: data-lake-analytics
ms.reviewer: jasonh
ms.assetid: db35f16e-1565-4873-a851-bd987accdc58
ms.topic: how-to
ms.date: 05/24/2018
ms.openlocfilehash: 4819b5cb252d2c646905d277fcbedc57a55fe692
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2020
ms.locfileid: "91450206"
---
# <a name="adding-a-user-in-the-azure-portal"></a>Добавление пользователя на портале Azure

## <a name="start-the-add-user-wizard"></a>Запуск мастера добавления пользователей
1. Откройте Azure Data Lake Analytics через портал https://portal.azure.com.
2. Щелкните **Мастер добавления пользователей**.
3. На шаге **выбора пользователя** найдите пользователя, которого нужно добавить. Нажмите кнопку **Выбрать**.
4. На шаге **выбора роли** выберите **Разработчик Data Lake Analytics**. Эта роль имеет минимальный набор разрешений, необходимых для отправки и отслеживания заданий U-SQL, а также управления ими. Назначьте эту роль, если группа не предназначена для управления службами Azure.
5. На шаге **выбора каталога разрешений** выберите дополнительные базы данных, к которым пользователю понадобится доступ. Для отправки заданий требуется доступ на чтение и запись к статической базе данных по умолчанию с именем "Master". По завершении нажмите кнопку **ОК**.
6. На последнем шаге, который называется **Назначение выбранных разрешений**, просмотрите изменения, которые внесет мастер. Нажмите кнопку **ОК**.


## <a name="configure-acls-for-data-folders"></a>Настройка списков управления доступом для папок данных
Предоставьте нужные разрешения ("R-X" или "RWX") по отношению к папкам, содержащим входные и выходные данные.


## <a name="optionally-add-the-user-to-the-azure-data-lake-storage-gen1-role-reader-role"></a>При необходимости добавьте пользователя в роль **читателя** в Azure Data Lake Storage 1-го поколения.
1.  Найдите вашу учетную запись Azure Data Lake Storage 1-го поколения.
2.  Щелкните **Пользователи**.
3. Нажмите кнопку **Добавить**.
4.  Выберите роль Azure для назначения этой группы.
5.  Присвойте роль читателя. Эта роль имеет минимальный набор разрешений, необходимых для просмотра данных, хранящихся в ADLSGen1, и управления ими. Назначьте эту роль, если группа не предназначена для управления службами Azure.
6.  Введите имя группы.
7.  Нажмите кнопку **ОК**.

## <a name="adding-a-user-using-powershell"></a>Добавление пользователя с помощью PowerShell

1. Следуйте инструкциям в руководстве по [установке и настройке Azure PowerShell](/powershell/azure/).
2. Скачайте сценарий PowerShell [Add-AdlaJobUser.ps1](https://github.com/Azure/AzureDataLake/blob/master/Samples/PowerShell/ADLAUsers/Add-AdlaJobUser.ps1).
3. Запустите этот скрипт PowerShell. 

Пример команды, предоставляющий пользователю доступ к отправке заданий, просмотру новых и старых метаданных задания:

`Add-AdlaJobUser.ps1 -Account myadlsaccount -EntityToAdd 546e153e-0ecf-417b-ab7f-aa01ce4a7bff -EntityType User -FullReplication`


## <a name="next-steps"></a>Дальнейшие шаги

* [Обзор Azure Data Lake Analytics](data-lake-analytics-overview.md)
* [Начало работы с Azure Data Lake Analytics с помощью портала Azure](data-lake-analytics-get-started-portal.md)
* [Управление Azure Data Lake Analytics с помощью Azure PowerShell](data-lake-analytics-manage-use-powershell.md)
