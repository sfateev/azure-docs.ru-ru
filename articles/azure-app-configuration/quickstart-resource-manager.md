---
title: Краткое руководство. Автоматизированное развертывание виртуальной машины с помощью службы "Конфигурация приложений Azure"
description: В этом кратком руководстве показано, как использовать модуль Azure PowerShell и шаблоны Azure Resource Manager для развертывания хранилища службы "Конфигурация приложений Azure". Затем используйте значения в хранилище для развертывания виртуальной машины.
author: lisaguthrie
ms.author: lcozzens
ms.date: 08/11/2020
ms.topic: quickstart
ms.service: azure-app-configuration
ms.custom:
- mvc
- subject-armqs
ms.openlocfilehash: 7b7dd00d3495c24733ecdc213e0e25f8bc9640eb
ms.sourcegitcommit: eb6bef1274b9e6390c7a77ff69bf6a3b94e827fc
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/05/2020
ms.locfileid: "88661475"
---
# <a name="quickstart-automated-vm-deployment-with-app-configuration-and-resource-manager-template-arm-template"></a>Краткое руководство. Автоматизированное развертывание виртуальной машины с помощью службы "Конфигурация приложений" и шаблона Resource Manager (шаблон ARM)

Узнайте, как использовать шаблоны Azure Resource Manager и Azure PowerShell для развертывания хранилища службы "Конфигурация приложений Azure", как добавлять пары "ключ-значение" в хранилище и использовать пары "ключ-значение" в хранилище для развертывания ресурсов Azure, таких как виртуальная машина Azure в этом примере.

[!INCLUDE [About Azure Resource Manager](../../includes/resource-manager-quickstart-introduction.md)]

Если среда соответствует предварительным требованиям и вы знакомы с использованием шаблонов ARM, нажмите кнопку **Развертывание в Azure**. Шаблон откроется на портале Azure.

[![Развертывание в Azure](../media/template-deployments/deploy-to-azure.svg)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-app-configuration-store%2Fazuredeploy.json)

## <a name="prerequisites"></a>Предварительные требования

Если у вас еще нет подписки Azure, [создайте бесплатную учетную запись](https://azure.microsoft.com/free/?WT.mc_id=A261C142F), прежде чем начинать работу.

## <a name="review-the-templates"></a>Ознакомьтесь с шаблонами

Шаблоны, используемые в этом кратком руководстве, взяты на [этой странице](https://azure.microsoft.com/resources/templates/). [Первый шаблон](https://azure.microsoft.com/resources/templates/101-app-configuration-store/) создает хранилище службы "Конфигурация приложений":

:::code language="json" source="~/quickstart-templates/101-app-configuration-store/azuredeploy.json" range="1-37" highlight="27-35":::

В этом шаблоне определяется один ресурс Azure.

- [Microsoft.AppConfiguration/configurationStores](/azure/templates/microsoft.appconfiguration/2019-10-01/configurationstores): создает хранилище службы "Конфигурация приложений".

[Второй шаблон](https://azure.microsoft.com/resources/templates/101-app-configuration/) создает виртуальную машину с помощью пар "ключ-значение" в хранилище. Перед выполнением этого шага необходимо добавить пары "ключ-значение" с помощью портала или Azure CLI.

:::code language="json" source="~/quickstart-templates/101-app-configuration/azuredeploy.json" range="1-217" highlight="77, 181,189":::

## <a name="deploy-the-templates"></a>Развертывание шаблонов.

### <a name="create-an-app-configuration-store"></a>Создание хранилища Конфигурации приложений

1. Выберите следующее изображение, чтобы войти на портал Azure и открыть шаблон. Шаблон создает хранилище Конфигурации приложений.

    [![Развертывание в Azure](../media/template-deployments/deploy-to-azure.svg)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-app-configuration-store%2Fazuredeploy.json)

1. Введите или выберите следующие значения.

    - **Подписка**. Выберите подписку Azure, которая используется для создания хранилища Конфигурации приложений.
    - **Группа ресурсов**. Выберите **Создать**, чтобы создать группу ресурсов, если не хотите использовать имеющуюся.
    - **Регион**. Выберите расположение группы ресурсов.  Например, **Восточная часть США**.
    - **Config Store Name** (Имя хранилища Конфигурации приложения). Введите имя нового хранилища Конфигурации приложений.
    - **Расположение**. Укажите расположение хранилища Конфигурации приложений.  Используйте значение по умолчанию.
    - **Имя номера SKU**. Укажите имя номера SKU хранилища Конфигурации приложений. Используйте значение по умолчанию.

1. Выберите **Проверить и создать**.
1. Убедитесь, что на странице отображается состояние **Проверка пройдена**, а затем выберите **Создать**.

Запишите имя группы ресурсов и имя хранилища Конфигурации приложений.  Эти значения понадобятся при развертывании виртуальной машины.
### <a name="add-vm-configuration-key-values"></a>Добавление пары "ключ-значение" в конфигурацию виртуальной машины

После создания хранилища Конфигурации приложений можно добавите в него пары "ключ-значение" с помощью портала Azure или Azure CLI.

1. Войдите на [портал Azure](https://portal.azure.com) и перейдите к созданному хранилищу Конфигурации приложений.
1. В меню слева выберите **Обозреватель конфигураций**.
1. Выберите **Создать**, чтобы добавить следующие пары "ключ-значение":

   |Клавиши|Значение|Метка|
   |-|-|-|
   |windowsOsVersion|2019-Datacenter|шаблон|
   |diskSizeGB|1023|шаблон|

   **Тип содержимого** оставьте пустым.

Сведения об использовании Azure CLI см. в статье [Использование пар "ключ-значение" в хранилище конфигураций приложения Azure](./scripts/cli-work-with-keys.md).

### <a name="deploy-vm-using-stored-key-values"></a>Развертывание виртуальной машины с использованием сохраненных пар "ключ-значение"

Теперь, когда вы добавили пары "ключ-значение" в хранилище, вы можете развернуть виртуальную машину с помощью шаблона Azure Resource Manager. Шаблон ссылается на созданные ключи **windowsOsVersion** и **diskSizeGB**.

> [!WARNING]
> Шаблоны Resource Manager не могут ссылаться на ключи в хранилище Конфигурации приложений, у которого включен Приватный канал.

1. Выберите следующее изображение, чтобы войти на портал Azure и открыть шаблон. Шаблон создает виртуальную машину с помощью пар "ключ-значение" в хранилище Конфигурации приложений.

    [![Развертывание в Azure](../media/template-deployments/deploy-to-azure.svg)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-app-configuration%2Fazuredeploy.json)

1. Введите или выберите следующие значения.

    - **Подписка**. Выберите подписку Azure, которая используется для создания виртуальной машины.
    - **Группа ресурсов**. Укажите ту же группу ресурсов, что и для хранилища Конфигурации приложений, или выберите **Создать**, чтобы создать новую.
    - **Регион**. Выберите расположение группы ресурсов.  Например, **Восточная часть США**.
    - **Расположение**. Укажите расположение виртуальной машины. Используйте значение по умолчанию.
    - **Имя администратора**. Укажите имя администратора для виртуальной машины.
    - **Пароль администратора**. Введите пароль администратора для виртуальной машины.
    - **Метка доменного имени**. Введите уникальное доменное имя.
    - **Имя учетной записи хранения**. Введите уникальное имя для учетной записи хранения, которая связана с виртуальной машиной.
    - **App Config Store Resource Group** (Группа ресурсов хранилища Конфигурации приложений). Введите группу ресурсов, содержащую хранилище Конфигурации приложений.
    - **App Config Store Name** (Имя хранилища Конфигурации приложений). Введите имя хранилища Конфигурации приложений Azure.
    - **VM Sku Key** (Ключ номера SKU виртуальной машины). Введите **windowsOsVersion**.  Это имя значения ключа, которое вы добавили в хранилище.
    - **Disk Size Key** (Ключ размера диска). Введите **diskSizeGB**. Это имя значения ключа, которое вы добавили в хранилище.

1. Выберите **Проверить и создать**.
1. Убедитесь, что на странице отображается состояние **Проверка пройдена**, а затем выберите **Создать**.

## <a name="review-deployed-resources"></a>Просмотр развернутых ресурсов

1. Войдите на [портал Azure](https://portal.azure.com) и перейдите к созданной виртуальной машине.
1. В меню слева выберите **Обзор** и убедитесь, что для **номера SKU** указано **2019-Datacenter**.
1. В меню слева выберите **Диски** и убедитесь, что для размера диска данных указано **2013**.

## <a name="clean-up-resources"></a>Очистка ресурсов

Ставшие ненужными группу ресурсов, хранилище Конфигурации приложений, виртуальную машину и все связанные ресурсы можно удалить. Если вы планируете использовать хранилище Конфигурации приложений или виртуальную машину в дальнейшем, не удаляйте их. Если вам больше не нужно это задание, удалите все ресурсы, созданные в ходе работы с этим кратким руководством, выполнив следующий командлет.

```azurepowershell-interactive
Remove-AzResourceGroup `
  -Name $resourceGroup
```

## <a name="next-steps"></a>Дальнейшие действия

В рамках этого краткого руководства вы развернули виртуальную машину с помощью шаблона Azure Resource Manager и пар "ключ-значение" из службы "Конфигурация приложений Azure".

Сведения о создании других приложений с помощью службы "Конфигурация приложений Azure" см. в этой статье:

> [!div class="nextstepaction"]
> [Краткое руководство. Создание приложения ASP.NET Core с помощью службы "Конфигурация приложений Azure"](quickstart-aspnet-core-app.md)
