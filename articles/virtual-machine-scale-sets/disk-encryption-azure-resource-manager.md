---
title: Создание и шифрование масштабируемого набора виртуальных машин с помощью шаблонов Azure Resource Manager
description: С помощью этого краткого руководства вы узнаете, как создать и зашифровать масштабируемый набор виртуальных машин, используя шаблоны Azure Resource Manager
author: ju-shim
ms.author: jushiman
ms.topic: how-to
ms.service: virtual-machine-scale-sets
ms.subservice: disks
ms.date: 10/10/2019
ms.reviewer: mimckitt
ms.custom: mimckitt
ms.openlocfilehash: 4284e94f8d8d0effd160c5048f54fcbede417e38
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2020
ms.locfileid: "86129757"
---
# <a name="encrypt-virtual-machine-scale-sets-with-azure-resource-manager"></a>Шифрование масштабируемых наборов виртуальных машин с помощью Azure Resource Manager

Масштабируемые наборы виртуальных машин Linux можно шифровать и расшифровывать с помощью шаблонов Azure Resource Manager.

## <a name="deploying-templates"></a>Развертывание шаблонов

Сначала выберите шаблон, соответствующий вашему сценарию.

- [Включение шифрования диска в запущенном масштабируемом наборе виртуальных машин Linux](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-running-vmss-linux)

- [Включение шифрования диска в запущенном масштабируемом наборе виртуальных машин Windows](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-running-vmss-windows)

  - [Развертывание масштабируемого набора виртуальных машин Linux с помощью jumpbox и включение шифрования в этих масштабируемых наборах Linux](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-vmss-linux-jumpbox)

  - [Развертывание масштабируемого набора виртуальных машин Windows с помощью jumpbox и включение шифрования в этих масштабируемых наборах Windows](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-vmss-windows-jumpbox)

- [Отключение шифрования диска в запущенном масштабируемом наборе виртуальных машин Linux](https://github.com/Azure/azure-quickstart-templates/tree/master/201-decrypt-vmss-linux)

- [Отключение шифрования диска в запущенном масштабируемом наборе виртуальных машин Windows](https://github.com/Azure/azure-quickstart-templates/tree/master/201-decrypt-vmss-windows)

Затем выполните следующие действия.

1. Нажмите кнопку **Развернуть в Azure**.
2. Заполните обязательные поля, а затем согласитесь с условиями.
3. Щелкните **Приобрести**, чтобы развернуть шаблон.

## <a name="next-steps"></a>Дальнейшие действия

- [Azure Disk Encryption for Virtual Machine Scale Sets](disk-encryption-overview.md) (Шифрование дисков Azure для масштабируемых наборов виртуальных машин)
- [Encrypt OS and attached data disks in a virtual machine scale set with the Azure CLI](disk-encryption-cli.md) (Шифрование ОС и подключенных дисков данных в масштабируемом наборе виртуальных машин с помощью Azure CLI)
- [Encrypt OS and attached data disks in a virtual machine scale set with Azure PowerShell](disk-encryption-powershell.md) (Шифрование ОС и подключенных дисков данных в масштабируемом наборе виртуальных машин с помощью Azure PowerShell)
- [Создание и настройка хранилища ключей для шифрования дисков Azure](disk-encryption-key-vault.md)
- [Use Azure Disk Encryption with virtual machine scale set extension sequencing](disk-encryption-extension-sequencing.md) (Использование шифрования дисков Azure с помощью виртуализации расширения масштабируемого набора виртуальных машин)
