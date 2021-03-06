---
title: Что такое проверки доступа Azure Active Directory | Документы Майкрософт
description: Служба проверок доступа Azure Active Directory позволяет управлять членством в группе и доступом к приложениям, чтобы обеспечить удовлетворение требованиям к контролю, управлению рисками и соответствию в вашей организации.
services: active-directory
documentationcenter: ''
author: barclayn
manager: daveba
editor: markwahl-msft
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: overview
ms.subservice: compliance
ms.date: 09/08/2020
ms.author: barclayn
ms.reviewer: mwahl
ms.collection: M365-identity-device-management
ms.custom: contperfq1
ms.openlocfilehash: b454ced085ec3d73f3ca0f761abb6c5de44244ab
ms.sourcegitcommit: eb6bef1274b9e6390c7a77ff69bf6a3b94e827fc
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/05/2020
ms.locfileid: "89594345"
---
# <a name="what-are-azure-ad-access-reviews"></a>Что собой представляют проверки доступа Azure AD?

Служба проверок доступа Azure Active Directory (Azure AD) позволяет организациям эффективно управлять членством в группе, доступом к корпоративным приложениям и назначением ролей. Доступ пользователей можно проверять на регулярной основе, чтобы только у авторизованных пользователей был постоянный доступ.

Вот видео, в котором представлен краткий обзор проверки доступа:

>[!VIDEO https://www.youtube.com/embed/kDRjQQ22Wkk]

## <a name="why-are-access-reviews-important"></a>Почему важны проверки доступа

Azure AD позволяет работать вместе с коллегами из организации и внешними пользователями. Пользователи могут присоединяться к группам, приглашать гостей, подключаться к облачным приложениям и удаленно работать со своих рабочих или личных устройств. Удобство самообслуживания привели к потребности в более широких возможностях управления доступом.

- Как обеспечить новым сотрудникам необходимый доступ, чтобы они работали максимально производительно?
- Люди переходят из одной команды в другою и уходят из компании. Как отозвать у них старые права доступа?
- Чрезмерные права доступа могут привести к компрометации данных.
- Они также могут стать поводом для проведения тщательного аудита, так как укажут на отсутствие контроля доступа.
- Вам нужно заблаговременно обратиться к владельцам ресурсов и убедиться, что они регулярно проверяют, у кого есть доступ к их ресурсам.

## <a name="when-should-you-use-access-reviews"></a>Когда следует использовать проверки доступа?

- **Когда слишком много пользователей обладают привилегированными ролями.** Хорошей практикой считается проверять, у скольких пользователей есть административный доступ, сколько из них являются глобальными администраторами, есть ли приглашенные гости или партнеры, которые не были удалены после назначения им административных задач. Можно повторно сертифицировать назначение пользователям [ролей Azure AD](../privileged-identity-management/pim-how-to-perform-security-review.md?toc=%2fazure%2factive-directory%2fgovernance%2ftoc.json), таких как глобальные администраторы, или [ролей ресурсов Azure](../privileged-identity-management/pim-resource-roles-perform-access-review.md?toc=%2fazure%2factive-directory%2fgovernance%2ftoc.json), таких как администратор доступа пользователей, в [Azure AD Privileged Identity Management (PIM)](../privileged-identity-management/pim-configure.md).
- **Когда автоматизация невозможна.** Предположим, вы создали правила для динамического членства в группах безопасности или группах Microsoft 365. Но что делать, если данные отдела кадров не хранятся в Azure AD или пользователям требуется возможность доступа после выхода из группы, например для обучения пришедших им на замену сотрудников? Можно создать проверку для этой группы, чтобы убедиться, что доступ есть у пользователей, которым он по-прежнему необходим.
- **Когда группа используется в новых целях.** Если у вас есть группа, которая будет синхронизироваться с Azure AD или если вы планируете включить приложение Salesforce для всех пользователей в группе продаж, будет полезно попросить владельца группы проверить членство в группе, прежде чем она будет использоваться для другого содержимого.
- **Доступ к данным, критически важным для бизнеса.** Для определенных ресурсов может потребоваться регулярный выход из системы всех сотрудников, не относящихся к подразделению ИТ, и обоснование необходимости доступа для целей аудита.
- **Когда нужно поддерживать список исключений политики.** В идеальном мире все пользователи соблюдают политики доступа для защиты доступа к ресурсам организации. Но иногда в интересах бизнеса приходится делать исключения. ИТ-администраторы могут управлять этой задачей, чтобы не просмотреть исключения политик и предоставить аудиторам доказательства регулярной проверки этих исключений.
- **Попросите владельцев группы подтвердить, что им все еще нужны гости в группах.** С помощью локальной Системы управления идентификацией и доступом (IAM) можно автоматизировать предоставление доступа сотрудникам, но не приглашенным гостям. Если группа предоставляет гостям доступ к конфиденциальному бизнес-содержимому, то владелец группы должен подтвердить, что гостям по-прежнему на законных основаниях требуется такой доступ для бизнеса.
- **Периодически повторяйте проверки.** Вы можете настроить повторяющиеся проверки доступа пользователей с заданной частотой (например, еженедельно, ежемесячно, ежеквартально или ежегодно). Перед каждой новой проверкой рецензентам будет отправляться соответствующее уведомление. Рецензенты могут подтверждать или отклонять доступ с помощью удобного интерфейса и смарт-рекомендаций.

>[!NOTE]
>Если вы готовы попробовать проверки доступа, ознакомьтесь с разделом [Создание проверки доступа групп или приложений](create-access-review.md).

## <a name="where-do-you-create-reviews"></a>Где создавать проверки

В зависимости от того, что именно нужно проверить, создавайте проверку доступа в службе проверок доступа Azure AD, в корпоративных приложениях Azure AD (предварительная версия) или в Azure AD PIM.

| Права доступа пользователей | Типы рецензентов | Где создана проверка | Работа рецензентов |
| --- | --- | --- | --- |
| Члены группы безопасности</br>Члены группы Office | Указанные рецензенты</br>Владельцы группы</br>Самостоятельная проверка | Проверки доступа Azure AD</br>Группы Azure AD | Панель доступа |
| Назначенные для подключенного приложения | Указанные рецензенты</br>Самостоятельная проверка | Проверки доступа Azure AD</br>Корпоративные приложения Azure AD (в предварительной версии) | Панель доступа |
| Роль Azure AD | Указанные рецензенты</br>Самостоятельная проверка | [Azure AD PIM](../privileged-identity-management/pim-how-to-start-security-review.md?toc=%2fazure%2factive-directory%2fgovernance%2ftoc.json) | Портал Azure |
| Роль ресурса Azure | Указанные рецензенты</br>Самостоятельная проверка | [Azure AD PIM](../privileged-identity-management/pim-resource-roles-start-access-review.md?toc=%2fazure%2factive-directory%2fgovernance%2ftoc.json) | Портал Azure |

## <a name="license-requirements"></a>Требования лицензий

[!INCLUDE [Azure AD Premium P2 license](../../../includes/active-directory-p2-license.md)]

### <a name="how-many-licenses-must-you-have"></a>Сколько лицензий потребуется?

В каталоге должно быть по меньшей мере столько же лицензий Azure AD Premium P2, сколько у вас сотрудников со следующими ролями и полномочиями:

- участники и гостевые пользователи, назначенные в качестве рецензентов;
- участники и гостевые пользователи, выполняющие самостоятельную проверку;
- владельцы групп, выполняющие проверку доступа;
- владельцы приложений, выполняющие проверку доступа.

Лицензии Azure AD Premium P2 **не** требуются для пользователей с ролями глобального администратора или администратора пользователей, которые настраивают проверки доступа и параметры или применяют решения из проверок.

Каждая платная лицензия Azure AD Premium P2, которая присваивается пользователям из вашей организации, позволяет пригласить с помощью Azure Active Directory B2B до пяти гостевых пользователей в пределах квоты внешних пользователей. Эти гостевые пользователи также могут использовать функции Azure AD Premium P2. Дополнительные сведения см. в статье [Руководство по лицензированию службы совместной работы Azure Active Directory B2B](../external-identities/licensing-guidance.md).

Дополнительные сведения о лицензиях см. в статье [Назначение или удаление лицензий с помощью портала Azure Active Directory](../fundamentals/license-users-groups.md).

### <a name="example-license-scenarios"></a>Примеры сценариев лицензирования

Описанные ниже примеры сценариев лицензирования помогут вам определить необходимое количество лицензий.

| Сценарий | Вычисление | Количество лицензий |
| --- | --- | --- |
| Администратор создает проверку доступа для группы A с 75 пользователями и одним владельцем группы, а этого владельца группы назначает в качестве рецензента. | Одна лицензия для владельца группы в роли рецензента | 1 |
| Администратор создает проверку доступа для группы B с 500 пользователями и тремя владельцами группы, а всех владельцев группы назначает в качестве рецензентов. | Три лицензии для каждого владельца группы в роли рецензента | 3 |
| Администратор создает проверку доступа для группы B с 500 пользователями. Для этой группы он включает самостоятельную проверку. | 500 лицензий для каждого пользователя в роли самостоятельного рецензента | 500 |
| Администратор создает проверку доступа для группы C с 50 пользователями и 25 гостевыми пользователями. Для этой группы он включает самостоятельную проверку. | 50 лицензий для каждого пользователя в роли самостоятельного рецензента*. | 50 |
| Администратор создает проверку доступа для группы D с шестью пользователями-участниками и 108 гостевыми пользователями. Для этой группы он включает самостоятельную проверку. | 6 лицензий для каждого пользователя в роли самостоятельного рецензента. За гостевых пользователей плата взимается как за ежемесячно активных пользователей (MAU). Дополнительные лицензии не требуются. *  | - |

\* Плата за внешние удостоверения Azure AD (гостевой пользователь) взимается как за ежемесячно активных пользователей (MAU). Это число уникальных пользователей с действиями проверки подлинности в пределах календарного месяца. Эта модель заменяет модель выставления счетов в соотношении 1:5, когда на каждую лицензию Azure AD Premium в клиенте разрешено до пяти гостевых пользователей. Если клиент связан с подпиской и вы используете возможности внешних удостоверений для совместной работы с гостевыми пользователями, вам автоматически будет выставляться счет с использованием модели на основе MAU. Дополнительные сведения см. в статье "Модель выставления счетов для внешних удостоверений Azure AD".

## <a name="next-steps"></a>Дальнейшие действия

- [Создание проверки доступа групп или приложений](create-access-review.md)
- [Создание проверки доступа для пользователей в роли администратора Azure AD](../privileged-identity-management/pim-how-to-start-security-review.md?toc=%2fazure%2factive-directory%2fgovernance%2ftoc.json)
- [Проверка доступа к группам или приложениям](perform-access-review.md)
- [Выполнение проверки доступа для групп или приложений](complete-access-review.md)