---
title: Автоматическое увеличение хранилища Azure CLI — база данных Azure для PostgreSQL — один сервер
description: В этой статье описывается, как можно настроить автоматическое увеличение хранилища с помощью Azure CLI в базе данных Azure для PostgreSQL-Single Server.
author: ambhatna
ms.author: ambhatna
ms.service: postgresql
ms.topic: how-to
ms.date: 8/7/2019
ms.custom: devx-track-azurecli
ms.openlocfilehash: 4d4d9c6ac3eb4e9b0642f1ecb714b19a5029a314
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2020
ms.locfileid: "87493794"
---
# <a name="auto-grow-azure-database-for-postgresql-storage---single-server-using-the-azure-cli"></a>Автоматическое увеличение базы данных Azure для хранилища PostgreSQL — один сервер с помощью Azure CLI
В этой статье описывается, как можно настроить расширение хранилища базы данных Azure для PostgreSQL, не влияя на рабочую нагрузку.

Сервер достигает [предельного размера хранилища](https://docs.microsoft.com/azure/postgresql/concepts-pricing-tiers#reaching-the-storage-limit)и устанавливается в значение "только для чтения". Если автоматическое увеличение размера хранилища включено, то для серверов с подготовленным хранилищем объемом менее 100 ГБ размер подготовленного хранилища увеличивается на 5 ГБ, как только объем свободного хранилища превысит 1 ГБ или 10% подготовленного хранилища. Для серверов с более чем 100 ГБ подготовленного хранилища размер подготовленного хранилища увеличивается на 5%, если объем свободного пространства превышает 5% от подготовленного объема хранилища. Максимальный объем хранилища, указанный [здесь](https://docs.microsoft.com/azure/postgresql/concepts-pricing-tiers#storage) , применим.

## <a name="prerequisites"></a>Предварительные требования
Вот что вам нужно, чтобы выполнить инструкции, приведенные в этом руководстве:
- [сервер базы данных Azure для PostgreSQL](quickstart-create-server-database-azure-cli.md);

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

> [!IMPORTANT]
> Для работы с этим руководством вам понадобится Azure CLI 2.0 или более поздней версии. Чтобы проверить версию, в командной строке Azure CLI введите `az --version`. Чтобы выполнить установку или обновление, см. сведения в статье [Установка Azure CLI]( /cli/azure/install-azure-cli).

## <a name="enable-postgresql-server-storage-auto-grow"></a>Включить автоматическое увеличение хранилища сервера PostgreSQL

Включите автоматическое расширение хранилища на сервере на существующем сервере с помощью следующей команды:

```azurecli-interactive
az postgres server update --name mydemoserver --resource-group myresourcegroup --auto-grow Enabled
```

Включите автоматическое увеличение размера сервера при создании нового сервера с помощью следующей команды:

```azurecli-interactive
az postgres server create --resource-group myresourcegroup --name mydemoserver  --auto-grow Enabled --location westus --admin-user myadmin --admin-password <server_admin_password> --sku-name GP_Gen5_2 --version 9.6
```

## <a name="next-steps"></a>Дальнейшие действия

Узнайте [, как создавать оповещения по метрикам](howto-alert-on-metric.md).
