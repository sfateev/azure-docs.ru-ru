---
title: Использование интеграции системы управления версиями в службе автоматизации Azure
description: В этой статье рассказывается, как синхронизировать систему управления версиями службы автоматизации Azure с другими репозиториями.
services: automation
ms.subservice: process-automation
ms.date: 12/10/2019
ms.topic: conceptual
ms.openlocfilehash: eea4de106fe566b55ae30330d4c9d101f7126bbf
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2020
ms.locfileid: "86229624"
---
# <a name="use-source-control-integration"></a>Использование интеграции системы управления версиями

 Интеграция системы управления версиями в службе автоматизации Azure поддерживает однонаправленную синхронизацию из репозитория системы управления версиями. Система управления версиями позволяет поддерживать модули runbook в учетной записи службы автоматизации в актуальном состоянии, используя сценарии в репозитории системы управления версиями GitHub или Azure Repos. Эта функция упрощает продвижение кода, который был протестирован в среде разработки, к учетной записи службы автоматизации Azure.
 
 Интеграция системы управления версиями позволяет работать вместе с коллегами, отслеживать изменения и выполнять откат к более ранним версиям модулей runbook. Например, система управления версиями позволяет синхронизировать различные ветви внутри себя с учетными записями службы автоматизации Azure, предназначенными для разработки, тестирования или рабочей среды. 

## <a name="source-control-types"></a>Типы системы управления версиями

Служба автоматизации Azure поддерживает три типа системы управления версиями.

* GitHub
* Azure Repos (Git)
* Azure Repos (TFVC)

## <a name="prerequisites"></a>Предварительные требования

* Репозиторий системы управления версиями (GitHub или Azure Repos)
* [Учетная запись запуска от имени](manage-runas-account.md)
* [Последние модули Azure](automation-update-azure-modules.md) в учетной записи службы автоматизации, включая модуль `Az.Accounts` (модуль Az, эквивалентный `AzureRM.Profile`)

> [!NOTE]
> Задания синхронизации системы управления версиями выполняются от имени учетной записи службы автоматизации Azure соответствующих пользователей и оплачиваются по тому же тарифу, что и другие задания службы автоматизации.

## <a name="configure-source-control"></a>Настройка системы управления версиями

В этом разделе описывается настройка системы управления версиями для учетной записи службы автоматизации. Можно использовать портал Azure или PowerShell.

### <a name="configure-source-control-in-azure-portal"></a>Настройка системы управления версиями на портале Azure

Используйте эту процедуру для настройки системы управления версиями с помощью портала Azure.

1. В учетной записи службы автоматизации Azure выберите **Система управления версиями** и нажмите кнопку **Добавить**.

    ![Выбор системы управления версиями](./media/source-control-integration/select-source-control.png)

2. Выберите **Тип системы управления версиями** и нажмите кнопку **Проверить подлинность**. 

3. В открывшемся окне браузера появится приглашение на вход. Последуйте приглашениям, чтобы завершить проверку подлинности.

4. На странице «Сводка системы управления версиями» используйте поля для заполнения свойств системы управления версиями, определенных ниже. По завершении щелкните **Сохранить**. 

    |Свойство  |Описание  |
    |---------|---------|
    |Имя системы управления версиями     | Понятное имя системы управления версиями. Имя должно содержать только буквы и цифры.        |
    |Тип системы управления версиями     | Тип механизма системы управления версиями. Доступные параметры:</br> * GitHub</br>* Azure Repos (Git)</br> * Azure Repos (TFVC)        |
    |Хранилище     | Имя репозитория проекта. Извлекаются первые 200 репозиториев. Чтобы найти репозиторий, введите его имя в поле и нажмите кнопку **Поиск в GitHub**.|
    |Ветвь     | Ветвь, из которой следует извлечь исходные файлы. Настройка целевой ветви недоступна для типа системы управления версиями TFVC.          |
    |Путь к папке     | Папка, содержащая модули runbook для синхронизации, например **/Runbooks**. Синхронизируются только модули runbook в указанной папке. Рекурсия не поддерживается.        |
    |Автосинхронизация<sup>1</sup>     | Параметр, который включает или выключает автоматическую синхронизацию при выполнении фиксации в репозитории системы управления версиями.        |
    |Публикация модуля Runbook     | Имеет значение «Вкл.», если модули runbook автоматически публикуются после синхронизации из системы управления версиями, и «Выкл.» в противном случае.           |
    |Описание     | Текст, указывающий дополнительные сведения о системе управления версиями.        |

    <sup>1</sup> Для включения автоматической синхронизации при настройке интеграции системы управления версиями с Azure Repos необходимо быть администратором проекта.

   ![Сводка по системе управления версиями](./media/source-control-integration/source-control-summary.png)

> [!NOTE]
> Имя входа для репозитория системы управления версиями может отличаться от имени входа для портала Azure. Не забудьте выполнить вход с использованием правильной учетной записи при настройке системы управления версиями. Если есть сомнения, откройте новую вкладку в браузере, выйдите из **dev.azure.com**, **visualstudio.com** или **github.com** и повторите попытку подключиться к системе управления версиями.

### <a name="configure-source-control-in-powershell"></a>Настройка системы управления версиями в PowerShell

Для настройки системы управления версиями в службе автоматизации Azure можно также использовать PowerShell. Чтобы использовать командлеты PowerShell для этой операции, необходим личный маркер доступа (PAT). Используйте командлет [New-AzAutomationSourceControl](/powershell/module/az.automation/new-azautomationsourcecontrol?view=azps-3.5.0), чтобы создать подключение к системе управления версиями. Этот командлет требует защищенной строки для PAT. Сведения о создании защищенной строки см. в [ConvertTo-SecureString](/powershell/module/microsoft.powershell.security/convertto-securestring?view=powershell-6).

В следующих подразделах показано создание в PowerShell подключения к системе управления версиями для GitHub, Azure Repos (Git) и Azure Repos (TFVC). 

#### <a name="create-source-control-connection-for-github"></a>Создание подключения к системе управления версиями для GitHub

```powershell-interactive
New-AzAutomationSourceControl -Name SCGitHub -RepoUrl https://github.com/<accountname>/<reponame>.git -SourceType GitHub -FolderPath "/MyRunbooks" -Branch master -AccessToken <secureStringofPAT> -ResourceGroupName <ResourceGroupName> -AutomationAccountName <AutomationAccountName>
```

#### <a name="create-source-control-connection-for-azure-repos-git"></a>Создание подключения к системе управления версиями для Azure Repos (Git)

> [!NOTE]
> Azure Repos (Git) использует URL-адрес, который обращается к **dev.azure.com** вместо **visualstudio.com**, использовавшегося в более ранних форматах. Старый формат URL-адреса `https://<accountname>.visualstudio.com/<projectname>/_git/<repositoryname>` устарел, но по-прежнему поддерживается. Предпочтительно использовать новый формат.


```powershell-interactive
New-AzAutomationSourceControl -Name SCReposGit -RepoUrl https://dev.azure.com/<accountname>/<adoprojectname>/_git/<repositoryname> -SourceType VsoGit -AccessToken <secureStringofPAT> -Branch master -ResourceGroupName <ResourceGroupName> -AutomationAccountName <AutomationAccountName> -FolderPath "/Runbooks"
```

#### <a name="create-source-control-connection-for-azure-repos-tfvc"></a>Создание подключения к системе управления версиями для Azure Repos (TFVC)

> [!NOTE]
> Azure Repos (TFVC) использует URL-адрес, который обращается к **dev.azure.com** вместо **visualstudio.com**, использовавшегося в более ранних форматах. Старый формат URL-адреса `https://<accountname>.visualstudio.com/<projectname>/_versionControl` устарел, но по-прежнему поддерживается. Предпочтительно использовать новый формат.

```powershell-interactive
New-AzAutomationSourceControl -Name SCReposTFVC -RepoUrl https://dev.azure.com/<accountname>/<adoprojectname>/_git/<repositoryname> -SourceType VsoTfvc -AccessToken <secureStringofPAT> -ResourceGroupName <ResourceGroupName> -AutomationAccountName <AutomationAccountName> -FolderPath "/Runbooks"
```

#### <a name="personal-access-token-pat-permissions"></a>Разрешения личного маркера доступа (PAT)

Система управления версиями требует некоторых минимальных разрешений для личных маркеров доступа. В следующих таблицах представлены минимальные разрешения, необходимые для GitHub и Azure Repos.

##### <a name="minimum-pat-permissions-for-github"></a>Минимальные разрешения PAT для GitHub

В следующей таблице представлены минимальные разрешения личных маркеров доступа, необходимые для GitHub. Дополнительные сведения о создании личных маркеров доступа в GitHub см. в разделе [Создание личного маркера доступа для командной строки](https://help.github.com/articles/creating-a-personal-access-token-for-the-command-line/).

|Область  |Описание  |
|---------|---------|
|**`repo`**     |         |
|`repo:status`     | Доступ к фиксации состояния         |
|`repo_deployment`      | Доступ к состоянию развертывания         |
|`public_repo`     | Доступ к общим репозиториям         |
|**`admin:repo_hook`**     |         |
|`write:repo_hook`     | Запись перехватчиков репозитория         |
|`read:repo_hook`|Чтение перехватчиков репозитория|

##### <a name="minimum-pat-permissions-for-azure-repos"></a>Минимальные разрешения личных маркеров доступа для Azure Repos

В следующем списке представлены минимальные разрешения личных маркеров доступа, необходимые для Azure Repos. Дополнительные сведения о создании PAT в Azure Repos см. в разделе [Проверка подлинности доступа с помощью личных маркеров доступа](/azure/devops/organizations/accounts/use-personal-access-tokens-to-authenticate?view=azure-devops&tabs=preview-page).

| Область  |  Тип доступа  |
|---------| ----------|
| `Code`      | Чтение  |
| `Project and team` | Чтение |
| `Identity` | Чтение     |
| `User profile` | Чтение     |
| `Work items` | Чтение    |
| `Service connections` | Чтение, запрос, управление <sup>1</sup>    |

<sup>1</sup> Разрешение `Service connections` требуется только в том случае, если включена автосинхронизация.

## <a name="synchronize-with-source-control"></a>Синхронизация с системой управления версиями

Выполните следующие действия для синхронизации с системой управления версиями. 

1. Выберите источник из таблицы на странице системы управления версиями. 

2. Нажмите кнопку **Начать синхронизацию**, чтобы запустить процесс. 

3. Просмотрите состояние текущего задания синхронизации или предыдущих заданий, щелкнув вкладку **Задания синхронизации**. 

4. В раскрывающемся меню **Система управления версиями** выберите механизм системы управления версиями.

    ![Состояние синхронизации](./media/source-control-integration/sync-status.png)

5. Если щелкнуть какое-либо задание, можно просмотреть его выходные данные. Следующий пример иллюстрирует выходные данные задания синхронизации системы управления версиями.

    ```output
    ===================================================================

    Azure Automation Source Control.
    Supported runbooks to sync: PowerShell Workflow, PowerShell Scripts, DSC Configurations, Graphical, and Python 2.

    Setting AzEnvironment.

    Getting AzureRunAsConnection.

    Logging in to Azure...

    Source control information for syncing:

    [Url = https://ContosoExample.visualstudio.com/ContosoFinanceTFVCExample/_versionControl] [FolderPath = /Runbooks]

    Verifying url: https://ContosoExample.visualstudio.com/ContosoFinanceTFVCExample/_versionControl

    Connecting to VSTS...

    Source Control Sync Summary:

    2 files synced:
     - ExampleRunbook1.ps1
     - ExampleRunbook2.ps1

    ==================================================================

    ```

6. Дополнительное ведение журнала можно получить, выбрав **Все журналы** на странице «Сводка по заданию синхронизации системы управления версиями». Эти дополнительные записи журнала могут помочь в устранении неполадок, которые могут возникнуть при использовании системы управления версиями.

## <a name="disconnect-source-control"></a>Отключение системы управления версиями

Чтобы отключиться от репозитория системы управления версиями, выполните следующие действия.

1. Откройте **Систему управления версиями** в разделе **Параметры учетной записи** учетной записи службы автоматизации Azure.

2. Выберите механизм системы управления версиями, который требуется удалить. 

3. На странице «Сводка системы управления версиями» щелкните **Удалить**.

## <a name="handle-encoding-issues"></a>Решение проблем кодирования

Если несколько пользователей редактируют модули runbook в репозитории системы управления версиями с помощью различных редакторов, могут возникнуть проблемы с кодированием. Дополнительные сведения об этой ситуации см. в разделе [Распространенные причины проблем кодирования](/powershell/scripting/components/vscode/understanding-file-encoding?view=powershell-7#common-causes-of-encoding-issues).

## <a name="update-the-pat"></a>Обновление личного маркера доступа

В настоящее время портал Azure нельзя использовать для обновления личного маркера доступа в системе управления версиями. После истечения срока действия маркера доступа или его отзыва систему управления версиями можно обновить, добавив новый маркер доступа одним из следующих способов.

* Используя [REST API](/rest/api/automation/sourcecontrol/update).
* Используя командлет [Update-AzAutomationSourceControl](/powershell/module/az.automation/update-azautomationsourcecontrol).

## <a name="next-steps"></a>Дальнейшие действия

* Сведения об интеграции системы управления версиями со службой автоматизации Azure см. в [Служба автоматизации Azure. Интеграция системы управления версиями и службы автоматизации Azure](https://azure.microsoft.com/blog/azure-automation-source-control-13/).  
* Сведения об интеграции системы управления версиями runbook с помощью Visual Studio Online см. в [Служба автоматизации Azure. Интеграция системы управления версиями runbook с использованием службы Visual Studio Online](https://azure.microsoft.com/blog/azure-automation-integrating-runbook-source-control-using-visual-studio-online/).
