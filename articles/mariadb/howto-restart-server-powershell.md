---
title: Перезапуск сервера-Azure PowerShell — база данных Azure для MariaDB
description: В этой статье описывается, как можно перезапустить сервер базы данных Azure для MariaDB с помощью PowerShell.
author: ajlam
ms.author: andrela
ms.service: mariadb
ms.topic: how-to
ms.date: 5/26/2020
ms.custom: devx-track-azurepowershell
ms.openlocfilehash: 37fb724b83e80c1265755e6440f152143d419051
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2020
ms.locfileid: "87503078"
---
# <a name="restart-azure-database-for-mariadb-server-using-powershell"></a>Перезапуск базы данных Azure для сервера MariaDB с помощью PowerShell

В этой статье объясняется, как перезапустить сервер Базы данных Azure для MariaDB. Может потребоваться перезапустить сервер для обслуживания, что приводит к кратковременному отключению во время выполнения операции.

Если служба занята, перезагрузка сервера блокируется. Например, служба может обрабатывать запрошенную ранее операцию, такую как масштабирование виртуальных ядер.

Количество времени, необходимое для выполнения перезагрузки, зависит от процесса восстановления MariaDB. Чтобы сократить время перезапуска, рекомендуется свести к минимальному объему действий, происходящих на сервере, до перезапуска.

## <a name="prerequisites"></a>Предварительные требования

Вот что вам нужно, чтобы выполнить инструкции, приведенные в этом руководстве:

- [Модуль AZ PowerShell](https://docs.microsoft.com/powershell/azure/install-az-ps) , установленный локально или [Azure Cloud Shell](https://shell.azure.com/) в браузере
- [Сервер базы данных Azure для MariaDB](quickstart-create-mariadb-server-database-using-azure-powershell.md)

> [!IMPORTANT]
> Так как модуль PowerShell Az.MariaDb предоставляется в предварительной версии, его нужно установить отдельно от модуля Az PowerShell с помощью команды `Install-Module -Name Az.MariaDb -AllowPrerelease`.
> Как только модуль PowerShell Az.MariaDb станет общедоступным, он будет включен в один из будущих выпусков модуля Az PowerShell и встроен в Azure Cloud Shell.

Если вы решили использовать PowerShell локально, подключитесь к учетной записи Azure с помощью командлета [Connect-азаккаунт](https://docs.microsoft.com/powershell/module/az.accounts/connect-azaccount) .

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="restart-the-server"></a>Перезапустите сервер.

Перезапустите сервер с помощью следующей команды:

```azurepowershell-interactive
Restart-AzMariaDbServer -Name mydemoserver -ResourceGroupName myresourcegroup
```

## <a name="next-steps"></a>Дальнейшие действия

> [!div class="nextstepaction"]
> [Создание сервера базы данных Azure для MariaDB с помощью PowerShell](quickstart-create-mariadb-server-database-using-azure-powershell.md)
