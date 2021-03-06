---
title: Краткое руководство. Создание первого статического веб-приложения с помощью службы "Статические веб-приложения Azure" в Azure CLI
description: Узнайте, как создать экземпляр приложения службы "Статические веб-приложения Azure" с помощью Azure CLI.
services: static-web-apps
author: craigshoemaker
ms.service: static-web-apps
ms.topic: quickstart
ms.date: 08/13/2020
ms.author: cshoe
ms.openlocfilehash: 7e0fdbc50dd36e4ea23903a5929735c1c83bd394
ms.sourcegitcommit: eb6bef1274b9e6390c7a77ff69bf6a3b94e827fc
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/05/2020
ms.locfileid: "88752873"
---
# <a name="quickstart-building-your-first-static-web-app-using-the-azure-cli"></a>Краткое руководство. Создание первого статического веб-приложения с помощью Azure CLI

Служба "Статические веб-приложения Azure" публикует веб-сайты в рабочей среде, создавая приложения из репозитория GitHub. В этом кратком руководстве рассказывается о том, как развернуть веб-приложение в службе "Статические веб-приложения Azure" с помощью Azure CLI.

Если у вас еще нет подписки Azure, создайте [бесплатную пробную учетную запись](https://azure.microsoft.com/free).

## <a name="prerequisites"></a>Предварительные требования

- [GitHub](https://github.com) .
- [Личный маркер доступа GitHub](https://docs.github.com/github/authenticating-to-github/creating-a-personal-access-token)
- Учетная запись [Azure](https://portal.azure.com)
- Установленное средство [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) (версии 2.8.0 или выше)

[!INCLUDE [create repository from template](../../includes/static-web-apps-get-started-create-repo.md)]

[!INCLUDE [clone the repository](../../includes/static-web-apps-get-started-clone-repo.md)]

Затем перейдите в новую папку с помощью следующей команды.

```bash
cd my-first-static-web-app
```

## <a name="create-a-static-web-app"></a>Создание статического веб-приложения

Теперь, когда репозиторий создан, можно создать статическое веб-приложение в Azure CLI.

> [!IMPORTANT]
> Убедитесь, что текущая папка в терминале — _my-first-static-web-app_.

1. Войти в Azure CLI с помощью следующей команды.

    ```bash
    az login
    ```

1. Создайте новое статическое веб-приложение из репозитория.

    # <a name="no-framework"></a>[Без платформы](#tab/vanilla-javascript)

    ```bash
    az staticwebapp create \
        -n my-first-static-web-app \
        -g <RESOURCE_GROUP_NAME> \
        -s https://github.com/<YOUR_GITHUB_ACCOUNT_NAME>/my-first-static-web-app \
        -l <LOCATION> \
        -b master \
        --token <YOUR_GITHUB_PERSONAL_ACCESS_TOKEN>
    ```

    # <a name="angular"></a>[Angular](#tab/angular)

    ```bash
    az staticwebapp create \
        -n my-first-static-web-app \
        -g <RESOURCE_GROUP_NAME> \
        -s https://github.com/<YOUR_GITHUB_ACCOUNT_NAME>/my-first-static-web-app \
        -l <LOCATION> \
        -b master \
        --app-artifact-location "dist/angular-basic" \
        --token <YOUR_GITHUB_PERSONAL_ACCESS_TOKEN>
    ```

    # <a name="react"></a>[React](#tab/react)

    ```bash
    az staticwebapp create \
        -n my-first-static-web-app \
        -g <RESOURCE_GROUP_NAME> \
        -s https://github.com/<YOUR_GITHUB_ACCOUNT_NAME>/my-first-static-web-app \
        -l <LOCATION> \
        -b master \
        --app-artifact-location "build" \
        --token <YOUR_GITHUB_PERSONAL_ACCESS_TOKEN>
    ```

    # <a name="vue"></a>[Vue](#tab/vue)

    ```bash
    az staticwebapp create \
        -n my-first-static-web-app \
        -g <RESOURCE_GROUP_NAME> \
        -s https://github.com/<YOUR_GITHUB_ACCOUNT_NAME>/my-first-static-web-app \
        -l <LOCATION> \
        -b master \
        --app-artifact-location "dist" \
        --token <YOUR_GITHUB_PERSONAL_ACCESS_TOKEN>
    ```

    ---

    - `<RESOURCE_GROUP_NAME>`: Замените это значение именем имеющейся группы ресурсов Azure.

    - `<YOUR_GITHUB_ACCOUNT_NAME>`: Замените это значение именем пользователя GitHub.

    - `<LOCATION>`: Замените это значение ближайшим к вам расположением. Доступные параметры:  _CentralUS_, _EastAsia_, _EastUS2_, _WestEurope_ и _WestUS2_.

    - `<YOUR_GITHUB_PERSONAL_ACCESS_TOKEN>`: Замените это значение [личным маркером доступа GitHub](https://docs.github.com/github/authenticating-to-github/creating-a-personal-access-token), который вы создали ранее.

    Теперь вы можете просмотреть созданное приложение в Azure.

1. Откройте [портал Azure](https://portal.azure.com).

1. Выполните поиск **my-first-web-static-app** с помощью верхней строки поиска.

1. Выберите **my-first-web-static-app**.

[!INCLUDE [view website](../../includes/static-web-apps-get-started-view-website.md)]

## <a name="clean-up-resources"></a>Очистка ресурсов

Если вы не собираетесь использовать это приложение в дальнейшем, вы можете удалить экземпляр службы "Статические веб-приложения Azure" с помощью следующей команды.

```bash
az staticwebapp delete \
    --name my-first-static-web-app
    --resource-group my-first-static-web-app
```

## <a name="next-steps"></a>Дальнейшие действия

> [!div class="nextstepaction"]
> [Добавление API](add-api.md)
