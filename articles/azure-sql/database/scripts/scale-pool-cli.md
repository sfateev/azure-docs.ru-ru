---
title: 'Интерфейс командной строки Azure: Масштабирование эластичного пула'
description: Использование примера скрипта Azure CLI для масштабирования эластичного пула в Базе данных SQL Azure.
services: sql-database
ms.service: sql-database
ms.subservice: elastic-pools
ms.custom: sqldbrb=1
ms.devlang: azurecli
ms.topic: sample
author: stevestein
ms.author: sstein
ms.reviewer: ''
ms.date: 06/25/2019
ms.openlocfilehash: 01ccf7117cd17118941a00cc0480216bc436bc40
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2020
ms.locfileid: "86514770"
---
# <a name="use-the-azure-cli-to-scale-an-elastic-pool-in-azure-sql-database"></a>Масштабирование эластичного пула в Базе данных SQL Azure с помощью Azure CLI

[!INCLUDE[appliesto-sqldb](../../includes/appliesto-sqldb.md)]

Этот пример скрипта Azure CLI создает эластичные пулы в Базе данных SQL Azure и перемещает базы данных в пуле, а также изменяет вычислительную мощность эластичных пулов.

Если вы решили установить и использовать Azure CLI локально, то для работы с этим руководством вам понадобится Azure CLI 2.0 или более поздней версии. Чтобы узнать версию, выполните команду `az --version`. Если вам необходимо выполнить установку или обновление, см. статью [Установка Azure CLI]( /cli/azure/install-azure-cli).

## <a name="sample-script"></a>Пример скрипта

### <a name="sign-in-to-azure"></a>Вход в Azure

[!INCLUDE [quickstarts-free-trial-note](../../../../includes/quickstarts-free-trial-note.md)]

### <a name="run-the-script"></a>Выполнение скрипта

[!code-azurecli-interactive[main](../../../../cli_scripts/sql-database/scale-pool/scale-pool.sh "Move database between pools")]

### <a name="clean-up-deployment"></a>Очистка развертывания

Используйте следующую команду, чтобы удалить группу ресурсов и все связанные с ней ресурсы.

```azurecli-interactive
az group delete --name $resource
```

## <a name="sample-reference"></a>Примеры

Этот скрипт использует следующие команды. Для каждой команды в таблице приведены ссылки на соответствующую документацию.

| Get-Help | Описание |
|---|---|
| [az sql server](/cli/azure/sql/server) | Команды сервера. |
| [az sql db](/cli/azure/sql/db) | Команды базы данных. |
| [az sql elastic-pools](/cli/azure/sql/elastic-pool) | Команды эластичного пула. |

## <a name="next-steps"></a>Дальнейшие действия

Дополнительные сведения об Azure CLI см. в [документации по Azure CLI](/cli/azure).

Дополнительные примеры сценариев интерфейса командной строки для Базы данных SQL Azure см. в [документации по Базе данных SQL](../az-cli-script-samples-content-guide.md).
