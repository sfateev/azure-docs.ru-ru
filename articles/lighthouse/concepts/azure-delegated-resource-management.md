---
title: Делегированное управление ресурсами Azure
description: Делегированное управление ресурсами Azure является ключевой частью Azure Лигхсаусе, позволяющей поставщикам услуг управлять делегированными ресурсами в масштабе с гибкостью и точностью.
ms.date: 08/12/2020
ms.topic: conceptual
ms.openlocfilehash: 9a499ceda546b7ea5c71cd8c770f1a4b99001b08
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2020
ms.locfileid: "88163532"
---
# <a name="azure-delegated-resource-management"></a>Делегированное управление ресурсами Azure

Делегированное управление ресурсами Azure является одним из ключевых компонентов [Azure лигхсаусе](../overview.md). С помощью делегированного управления ресурсами Azure поставщики услуг могут упростить привлечение и подключение клиентов, одновременно управляя делегированными ресурсами в нужном масштабе с гибкостью и точностью.

## <a name="what-is-azure-delegated-resource-management"></a>Что такое делегированное управление ресурсами Azure?

Делегированное управление ресурсами Azure позволяет логически проецировать ресурсы одного арендатора на другого. Это позволяет полномочным пользователям в одном арендаторе Azure Active Directory (Azure AD) выполнять операции управления различными арендаторами Azure AD, принадлежащим их клиентам. Поставщики услуг могут войти в свой собственный арендатор Azure AD и иметь полномочия для работы с делегированными подписками клиентов и группами ресурсов. Это позволяет им выполнять операции управления от имени своих клиентов без необходимости входить в систему каждого отдельного арендатора клиента.

> [!TIP]
> Делегированное управление ресурсами Azure также можно использовать на [предприятии с несколькими собственными арендаторами Azure AD](enterprise.md), чтобы упростить управление ими.

Благодаря делегированному управлению ресурсами Azure полномочные пользователи могут работать напрямую с подпиской клиента, не имея учетной записи в арендаторе этого клиента или не являясь его совладельцем.

[Возможности управления между клиентами](cross-tenant-management-experience.md) позволяют более эффективно работать со службами управления Azure, такими как политика Azure, центр безопасности Azure и многое другое. Все действия поставщика услуг будут отслеживаться в журнале действий, который хранится в клиенте клиента (и может быть просмотрен пользователями в управляющем клиенте). Это означает, что пользователи в управлении и управляемом клиенте могут легко найти пользователя, связанного с любыми изменениями.

Вы можете [опубликовать новый тип предложения управляемой службы в Azure Marketplace](../how-to/publish-managed-services-offers.md) , чтобы легко подключить пользователей к Azure лигхсаусе. Также можно [подключать клиента к делегированному управлению ресурсами Azure](../how-to/onboard-customer.md).

## <a name="how-azure-delegated-resource-management-works"></a>Как работает делегированное управление ресурсами Azure

Работа делегированного управления ресурсами Azure на высоком уровне.

1. Во-первых, вы определяете доступ (роли), которым должны управлять ваши группы, субъекты-службы или пользователи, для управления ресурсами Azure клиента. Определение доступа содержит идентификатор управляемого клиента вместе с удостоверениями **principalId** из клиента, сопоставленный со [встроенными значениями **определения роли** ](../../role-based-access-control/built-in-roles.md) (участник, участник ВМ, читатель и т. д.).
2. Вы указываете этот доступ и подключение клиента к Azure Лигхсаусе одним из двух способов:
   - [Публикация предложения управляемой службы Azure Marketplace](../how-to/publish-managed-services-offers.md) (закрытое или общедоступное), которое будет принимать клиент
   - [Развертывание шаблона Azure Resource Manager в арендаторе клиента](../how-to/onboard-customer.md) для одной или нескольких подписок или групп ресурсов
3. После подключения клиента полномочные пользователи смогут войти в управляемый клиент и выполнить задачи в заданной области клиента на основе определенного вами уровня доступа.

> [!NOTE]
> Вы можете управлять делегированными ресурсами, расположенными в разных [регионах](../../availability-zones/az-overview.md#regions). Однако делегирование подписок в [национальной облаке](../../active-directory/develop/authentication-national-cloud.md) и общедоступном облаке Azure или в двух отдельных национальных облаках не поддерживается.

## <a name="support-for-azure-delegated-resource-management"></a>Поддержка делегированного управления ресурсами Azure

Если вам нужна помощь, связанная с делегированным управлением ресурсами Azure, можете подать запрос в службу поддержки на портале Azure. **Тип проблемы** укажите как **Техническая проблема**. Выберите подписку, а затем выберите **лигхсаусе** (в разделе **мониторинг & управление**).

## <a name="next-steps"></a>Дальнейшие действия

- Узнайте больше об [интерфейсах управления для различных клиентов](cross-tenant-management-experience.md).
- Дополнительные сведения о [предложениях управляемых служб в Azure Marketplace](managed-services-offers.md).
