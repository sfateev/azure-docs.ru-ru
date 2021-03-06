---
title: Узнайте, как пользователи назначаются приложениям в Azure Active Directory
description: Узнайте, как пользователи назначаются в приложение, использующее Azure Active Directory для управления удостоверениями.
services: active-directory
author: kenwith
manager: celestedg
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.topic: conceptual
ms.date: 07/11/2017
ms.author: kenwith
ms.openlocfilehash: 411e9a6e059075dfc2928ed09c0c604d600fd9be
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2020
ms.locfileid: "90604161"
---
# <a name="understand-how-users-are-assigned-to-apps-in-azure-active-directory"></a>Узнайте, как пользователи назначаются приложениям в Azure Active Directory
В этой статье представлены общие сведения о назначении пользователей для приложения в клиенте.

## <a name="how-do-users-get-assigned-to-an-application-in-azure-ad"></a>Как пользователи назначаются для приложений в Azure AD?
Чтобы пользователь смог получить доступ к приложению, сначала его нужно любым образом назначить этому приложению. Это может сделать администратор, бизнес-делегат или, в некоторых случаях, сам пользователь. Ниже описаны способы назначения пользователя для приложения.

*  Администратор [назначает пользователей](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal) непосредственно для приложения.
*  Администратор [назначает для приложения группу](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal), в которую входит пользователь. Ею может быть одна из следующих:
    * Группа, синхронизированная из локальной среды.
    * Статическая группа безопасности, созданная в облаке.
    * [Динамическая группа безопасности](https://docs.microsoft.com/azure/active-directory/active-directory-groups-dynamic-membership-azure-portal), созданная в облаке.
    * Группа Microsoft 365, созданная в облаке
    * Группа ["Все пользователи"](https://docs.microsoft.com/azure/active-directory/active-directory-accessmanagement-dedicated-groups).
*  Администратор включает [самостоятельный доступ к приложениям](https://docs.microsoft.com/azure/active-directory/active-directory-self-service-application-access) , позволяющий пользователю добавлять приложения с помощью функции " [Мои приложения](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) " **добавить** приложение **без бизнес-утверждения** .
*  Администратор включает [самостоятельный доступ к приложениям](https://docs.microsoft.com/azure/active-directory/active-directory-self-service-application-access) , позволяющий пользователю добавлять приложения с помощью функции " [Мои приложения](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) " **добавить** приложение, но только **при предыдущем утверждении от выбранного набора утверждающих бизнес-лиц**
*  Администратор включает [самостоятельное управление группами](https://docs.microsoft.com/azure/active-directory/active-directory-accessmanagement-self-service-group-management), чтобы позволить пользователям присоединяться к группам, которым назначено приложение, **без бизнес-утверждения**.
*  Администратор включает [самостоятельное управление группами](https://docs.microsoft.com/azure/active-directory/active-directory-accessmanagement-self-service-group-management), чтобы позволить пользователям присоединяться к группам, которым назначено приложение, только **с предварительного разрешения выбранных утверждающих лиц**.
*  Администратор назначает лицензию пользователю непосредственно для приложения первого разработчика, например [Microsoft 365](https://products.office.com/)
*  Администратор назначает лицензию группе, членом которой является пользователь, в приложение первого разработчика, например [Microsoft 365](https://products.office.com/)
*  [Администратор дает свое согласие на использование приложения](https://docs.microsoft.com/azure/active-directory/develop/active-directory-devhowto-multi-tenant-overview) всеми пользователями, а затем пользователь входит в это приложение.
* Пользователь сам [дает согласие на использование приложения](https://docs.microsoft.com/azure/active-directory/develop/active-directory-devhowto-multi-tenant-overview) при входе в него.

## <a name="next-steps"></a>Дальнейшие действия
* [Серия кратких руководств по управлению приложениями](view-applications-portal.md)
* [Что такое управление приложениями?](what-is-application-management.md)
* [Сведения о едином входе](what-is-single-sign-on.md)
