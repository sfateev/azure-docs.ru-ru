---
title: Удаление назначений ролей из группы в Azure Active Directory | Документация Майкрософт
description: Предварительная версия настраиваемых ролей Azure AD для делегирования управления удостоверениями. Управляйте ролями Azure на портале Azure, в PowerShell или API Graph.
services: active-directory
author: curtand
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.subservice: users-groups-roles
ms.topic: article
ms.date: 07/27/2020
ms.author: curtand
ms.reviewer: vincesm
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: 15312159bc0487f7d03c06fa947f69b1f6f9600c
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2020
ms.locfileid: "87513406"
---
# <a name="remove-role-assignments-from-a-group-in-azure-active-directory"></a>Удаление назначений ролей из группы в Azure Active Directory

В этой статье описывается, как ИТ – администратор может удалять роли Azure AD, назначенные группам. В портал Azure теперь можно удалить как прямые, так и косвенные назначения ролей пользователю. Если пользователю назначена роль по членству в группе, удалите пользователя из группы, чтобы удалить назначение ролей.

## <a name="using-azure-admin-center"></a>Использование центра администрирования Azure

1. Войдите в [центр администрирования Azure AD](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/Overview) с правами администратора привилегированных ролей или глобального администратора в Организации Azure AD.

1. Выберите **роли и**  >  ***имя роли***администраторов.

1. Выберите группу, из которой необходимо удалить назначение ролей, и щелкните **удалить назначение**.

   ![Удаление назначения роли из выбранной группы.](./media/roles-groups-remove-assignment/remove-assignment.png)

1. При появлении запроса на подтверждение действия выберите **Да**.

## <a name="using-powershell"></a>Использование PowerShell

### <a name="create-a-group-that-can-be-assigned-to-role"></a>Создание группы, которой можно назначить роль

```powershell
$group = New-AzureADMSGroup -DisplayName "Contoso_Helpdesk_Administrators" -Description "This group is assigned to Helpdesk Administrator built-in role in Azure AD." -MailEnabled $true -SecurityEnabled $true -MailNickName "contosohelpdeskadministrators" -IsAssignableToRole $true
```

### <a name="get-the-role-definition-you-want-to-assign-the-group-to"></a>Получение определения роли, для которой нужно назначить группу

```powershell
$roleDefinition = Get-AzureADMSRoleDefinition -Filter "displayName eq 'Helpdesk Administrator'"
```

### <a name="create-a-role-assignment"></a>Создание назначения роли

```powershell
$roleAssignment = New-AzureADMSRoleAssignment -ResourceScope '/' -RoleDefinitionId $roleDefinition.Id -PrincipalId $group.objectId
```

### <a name="remove-the-role-assignment"></a>Удаление назначения роли

```powershell
Remove-AzureAdMSRoleAssignment -Id $roleAssignment.Id 
```

## <a name="using-microsoft-graph-api"></a>Использование Microsoft Graph API

### <a name="create-a-group-that-can-be-assigned-an-azure-ad-role"></a>Создание группы, которой можно назначить роль Azure AD

```powershell
POST https://graph.microsoft.com/beta/groups

{
"description": "This group is assigned to Helpdesk Administrator built-in role of Azure AD",
"displayName": "Contoso_Helpdesk_Administrators",
"groupTypes": [
"Unified"
],
"mailEnabled": true,
"securityEnabled": true
"mailNickname": "contosohelpdeskadministrators",
"isAssignableToRole": true,
}
```

### <a name="get-the-role-definition"></a>Получение определения роли

```powershell
GET https://graph.microsoft.com/beta/roleManagement/directory/roleDefinitions?$filter = displayName eq ‘Helpdesk Administrator’
```

### <a name="create-the-role-assignment"></a>Создание назначения роли

```powershell
POST https://graph.microsoft.com/beta/roleManagement/directory/roleAssignments
{
"principalId":"<Object Id of Group>",
"roleDefinitionId":"<Id of role definition>",
"directoryScopeId":"/"
}
```

### <a name="delete-role-assignment"></a>Удаление назначения роли

```powershell
DELETE https://graph.microsoft.com/beta/roleManagement/directory/roleAssignments/<Id of role assignment>
```

## <a name="next-steps"></a>Дальнейшие действия

- [Использование облачных групп для управления назначениями ролей](roles-groups-concept.md)
- [Устранение неполадок ролей, назначенных облачным группам](roles-groups-faq-troubleshooting.md)
