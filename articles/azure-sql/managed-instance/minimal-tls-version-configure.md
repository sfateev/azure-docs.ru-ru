---
title: Настройка экземпляра, управляемого минимальной версией TLS
description: Узнайте, как настроить минимальную версию TLS для управляемого экземпляра.
services: sql-database
ms.service: sql-managed-instance
ms.subservice: security
ms.custom: ''
ms.topic: how-to
author: srdan-bozovic-msft
ms.author: srbozovi
ms.reviewer: ''
ms.date: 05/25/2020
ms.openlocfilehash: 2dbd4b9af3db122703a7f2b4e0140ec3305f1c3b
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2020
ms.locfileid: "91620058"
---
# <a name="configure-minimal-tls-version-in-azure-sql-managed-instance"></a>Настройка минимальной версии TLS в Управляемый экземпляр SQL Azure
Параметр минимальной версии протокола [TLS](https://support.microsoft.com/help/3135244/tls-1-2-support-for-microsoft-sql-server) позволяет клиентам управлять версией TLS, используемой управляемый экземпляр Azure SQL.

В настоящее время мы поддерживаем TLS 1,0, 1,1 и 1,2. Установка минимальной версии TLS гарантирует, что будут поддерживаться следующие более новые версии TLS. Например, выберите версию TLS более 1,1. означает, что принимаются только соединения с TLS 1,1 и 1,2, и TLS 1,0 отклоняется. После тестирования, чтобы убедиться, что приложения поддерживаются, рекомендуется установить минимальную версию TLS 1,2, так как она включает исправления уязвимостей, обнаруженных в предыдущих версиях, и является самой высокой версией протокола TLS, поддерживаемой в Управляемый экземпляр Azure SQL.

Для клиентов с приложениями, которые используют более старые версии TLS, рекомендуется установить минимальную версию TLS в соответствии с требованиями приложений. Для клиентов, которые используют приложения для подключения с использованием незашифрованного соединения, рекомендуется не задавать минимальную версию TLS. 

Дополнительные сведения см. в статье [рекомендации по TLS для подключения к базе данных SQL](../database/connect-query-content-reference-guide.md#tls-considerations-for-database-connectivity).

После установки минимальной версии TLS попытки входа от клиентов, использующих версию TLS ниже минимальной версии сервера, будут завершаться сбоем со следующей ошибкой:

```output
Error 47072
Login failed with invalid TLS version
```

## <a name="set-minimal-tls-version-via-powershell"></a>Установка минимальной версии TLS с помощью PowerShell

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]
> [!IMPORTANT]
> Модуль PowerShell Azure Resource Manager по-прежнему поддерживается базой данных SQL Azure, но вся будущая разработка сосредоточена на модуле Az.Sql. Сведения об этих командлетах см. в разделе [AzureRM.Sql](https://docs.microsoft.com/powershell/module/AzureRM.Sql/). Аргументы команд в модулях Az и AzureRm практически идентичны. Для следующего скрипта требуется [модуль Azure PowerShell](/powershell/azure/install-az-ps).

Следующий скрипт PowerShell показывает, как `Get` и `Set` минимальное свойство **версии TLS** на уровне экземпляра:

```powershell
#Get the Minimal TLS Version property
(Get-AzSqlInstance -Name sql-instance-name -ResourceGroupName resource-group).MinimalTlsVersion

# Update Minimal TLS Version Property
Set-AzSqlInstance -Name sql-instance-name -ResourceGroupName resource-group -MinimalTlsVersion "1.2"
```

## <a name="set-minimal-tls-version-via-azure-cli"></a>Установка минимальной версии TLS с помощью Azure CLI

> [!IMPORTANT]
> Для всех скриптов в этом разделе требуется [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli).

### <a name="azure-cli-in-a-bash-shell"></a>Azure CLI в оболочке bash

В следующем сценарии интерфейса командной строки показано, как изменить параметр **минимальной версии протокола TLS** в оболочке bash:

```azurecli-interactive
# Get current setting for Minimal TLS Version
az sql mi show -n sql-instance-name -g resource-group --query "minimalTlsVersion"

# Update setting for Minimal TLS Version
az sql mi update -n sql-instance-name -g resource-group --set minimalTlsVersion="1.2"
```
