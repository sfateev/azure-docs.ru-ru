---
title: Скрипт PowerShell для создания блокировки ресурсов для базы данных и графа API Gremlin в Azure Cosmos
description: Создание блокировки ресурсов для базы данных или графа API Gremlin в Azure Cosmos
author: markjbrown
ms.author: mjbrown
ms.service: cosmos-db
ms.subservice: cosmosdb-graph
ms.topic: sample
ms.date: 06/12/2020
ms.openlocfilehash: 32b5a02064ec987250f640ab6ea63515bac8385f
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2020
ms.locfileid: "87505150"
---
# <a name="create-a-resource-lock-for-azure-cosmos-gremlin-api-database-and-graph-using-azure-powershell"></a>Создание блокировки ресурсов для базы данных и графа API Gremlin в Azure Cosmos с помощью Azure PowerShell

[!INCLUDE [updated-for-az](../../../../../includes/updated-for-az.md)]

[!INCLUDE [sample-powershell-install](../../../../../includes/sample-powershell-install-no-ssh.md)]

> [!IMPORTANT]
> Блокировки ресурсов не работают для изменений, внесенных пользователями, которые подключаются с помощью любого пакета SDK Gremlin или портала Azure, если только учетная запись Cosmos DB сначала не будет заблокирована с включенным свойством `disableKeyBasedMetadataWriteAccess`. Дополнительные сведения о том, как включить это свойство, см. в разделе [Предотвращение изменений в пакетах SDK](../../../role-based-access-control.md#prevent-sdk-changes).

## <a name="sample-script"></a>Пример скрипта

[!code-powershell[main](../../../../../powershell_scripts/cosmosdb/gremlin/ps-gremlin-lock.ps1 "Create, list, and remove resource locks")]

## <a name="clean-up-deployment"></a>Очистка развертывания

После выполнения примера сценария можно удалить группу ресурсов и все связанные с ней ресурсы, выполнив следующую команду.

```powershell
Remove-AzResourceGroup -ResourceGroupName "myResourceGroup"
```

## <a name="script-explanation"></a>Описание скрипта

Этот скрипт использует следующие команды. Для каждой команды в таблице приведены ссылки на соответствующую документацию.

| Get-Help | Примечания |
|---|---|
|**Ресурс Azure**| |
| [New-AzResourceLock](https://docs.microsoft.com/powershell/module/az.resources/new-azresourcelock) | Создает блокировку ресурса. |
| [Get-AzResourceLock](https://docs.microsoft.com/powershell/module/az.resources/get-azresourcelock) | Возвращает блокировку ресурса или список блокировок ресурсов. |
| [Remove-AzResourceLock](https://docs.microsoft.com/powershell/module/az.resources/remove-azresourcelock) | Удаляет блокировку ресурса. |
|||

## <a name="next-steps"></a>Дальнейшие действия

Дополнительные сведения об Azure PowerShell см. в [документации по Azure PowerShell](https://docs.microsoft.com/powershell/).

Дополнительные примеры скриптов PowerShell для базы данных Azure Cosmos DB можно найти [здесь](../../../powershell-samples.md).