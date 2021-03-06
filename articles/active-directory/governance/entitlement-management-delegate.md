---
title: Делегирование и роли в управлении назначениями — Azure AD
description: Узнайте, как делегировать управление доступом от ИТ-администраторов руководителям отделов и руководителям проектов, чтобы они могли управлять доступом.
services: active-directory
documentationCenter: ''
author: barclayn
manager: daveba
editor: markwahl-msft
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.subservice: compliance
ms.date: 07/22/2020
ms.author: barclayn
ms.reviewer: mwahl
ms.collection: M365-identity-device-management
ms.openlocfilehash: c761be04c7d31567e727e93b84c98a1b3510e3d9
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2020
ms.locfileid: "90979877"
---
# <a name="delegation-and-roles-in-azure-ad-entitlement-management"></a>Делегирование и роли в управлении назначениями Azure AD

По умолчанию глобальные администраторы и администраторы пользователей могут создавать и администрировать все аспекты управления назначением Azure AD. Однако пользователи этих ролей могут не учитывать все случаи, когда требуются пакеты доступа. Обычно это пользователи в соответствующих отделах, командах или проектах, которые осведомлены о совместной работе с, использовании ресурсов и времени. Вместо того чтобы предоставлять неограниченные разрешения для пользователей, не являющихся администраторами, можно предоставить пользователям минимальные разрешения, необходимые для выполнения их работы, и избежать создания конфликтующих или нежелательных прав доступа.

В этом видео представлен обзор делегирования управления доступом от ИТ администратора пользователям, которые не являются администраторами.

> [!VIDEO https://www.microsoft.com/videoplayer/embed/RE3Lq00]

## <a name="delegate-example"></a>Пример делегата

Чтобы понять, как вы можете делегировать управление доступом с помощью управления назначением, можно рассмотреть пример. Предположим, что Организация имеет следующих администраторов и руководителей.

![Делегирование ИТ администратора руководителям](./media/entitlement-management-delegate/delegate-admin-dept-managers.png)

Как ИТ-администратор, Hana имеет контакты в каждом отделе — МАМТА в маркетинге, марку в финансах и Джо в юридическом лице, отвечающем за ресурсы Отдела и важное содержимое для бизнеса.

С помощью управления назначением можно делегировать управление доступом этим пользователям, не являющимся администраторами, так как это те, кто знает, какие пользователи нуждаются в доступе, сколько времени и какие ресурсы. Это гарантирует, что сотрудники будут управлять доступом для своих отделов.

Ниже приведен один из способов делегирования управления доступом к маркетинговым, финансовым и юридическим отделам.

1. Hana создает новую группу безопасности Azure AD и добавляет МАМТА, марку и Джо в качестве членов группы.

1. Hana добавляет эту группу к роли создателей каталога.

    МАМТА, Марк и Джо теперь могут создавать каталоги для своих отделов, добавлять ресурсы, необходимые для их отделов, и выполнять дальнейшие делегирования в каталоге.

    Обратите внимание, что МАМТА, Mark и Джо не видят каталоги друг друга.

1. МАМТА создает **маркетинговый** каталог, который представляет собой контейнер ресурсов.

1. МАМТА добавляет ресурсы, которыми владеет отдел маркетинга, в этот каталог.

1. МАМТА может добавить дополнительных пользователей из своего отдела в качестве владельцев каталога для этого каталога. Это помогает совместно использовать обязанности по управлению каталогом.

1. МАМТА может еще больше делегировать создание пакетов доступа и управление ими в маркетинговом каталоге для руководителей проектов в отделе маркетинга. Это можно сделать, назначив их роли диспетчера пакетов доступа. Диспетчер пакетов доступа может создавать пакеты доступа и управлять ими. 

На следующей диаграмме показаны каталоги с ресурсами для маркетинговых, финансовых и юридических отделов. С помощью этих каталогов руководители проектов могут создавать пакеты Access для своих команд или проектов.

![Пример делегата управления обслуживанием](./media/entitlement-management-delegate/elm-delegate.png)

После делегирования отдел маркетинга может иметь роли, аналогичные приведенным в следующей таблице.

| Пользователь | Роль задания | Роль Azure AD | Роль управления назначениями |
| --- | --- | --- | --- |
| Hana | ИТ – администратор | глобальный администратор или администратор пользователей. |  |
| мамта | Менеджер по маркетингу | Пользователь | Создатель каталога и владелец каталога |
| Владимир | Ведущий маркетинг | Пользователь | Владелец каталога |
| Джессика | Менеджер по маркетингу проекта | Пользователь | Доступ к диспетчеру пакетов |

## <a name="entitlement-management-roles"></a>Роли управления назначениями

Управление назначением имеет следующие роли, относящиеся к управлению назначением.

| Роль управления назначениями | Описание |
| --- | --- |
| Создатель каталога | Создание каталогов и управление ими. Обычно это ИТ-администратор, не являющийся глобальным администратором, или владелец ресурса для коллекции ресурсов. Пользователь, который создает каталог, автоматически станет первым владельцем каталога каталога и может добавить дополнительных владельцев каталога. Создатель каталога не может управлять каталогами, которыми они не владеют, и не может добавлять ресурсы, не принадлежащие каталогу. Если создателю каталога необходимо управлять другим каталогом или добавить ресурсы, которыми они не владеют, они могут запросить совладельца этого каталога или ресурса. |
| Владелец каталога | Изменение существующих каталогов и управление ими. Обычно это ИТ-администратор или владельцы ресурсов, или пользователь, которому назначен владелец каталога. |
| Доступ к диспетчеру пакетов | Изменение и управление всеми существующими пакетами доступа в каталоге. |
| Доступ к диспетчеру назначения пакетов | Изменение назначений всех существующих пакетов доступа и управление ими. |

Кроме того, назначенный утверждающий и запрашивающий пакет Access также имеют права, хотя они не являются ролями.

| Правый | Описание |
| --- | --- |
| Утверждающий | Авторизация политикой для утверждения или запрета запросов на доступ к пакетам, однако они не могут изменять определения пакетов доступа. |
| Requestor | Авторизация с помощью политики пакета доступа для запроса этого пакета доступа. |

В следующей таблице перечислены задачи, которые могут выполнять роли управления обслуживанием.

| Задача | Администратор | Создатель каталога | Владелец каталога | Доступ к диспетчеру пакетов | Доступ к диспетчеру назначения пакетов |
| --- | :---: | :---: | :---: | :---: | :---: |
| [Делегирование автору каталога](entitlement-management-delegate-catalog.md) | :heavy_check_mark: |  |  |  |  |
| [Добавление подключенной организации](entitlement-management-organization.md) | :heavy_check_mark: |  |  |  |  |
| [Создать новый каталог](entitlement-management-catalog-create.md) | :heavy_check_mark: | :heavy_check_mark: |  |  |  |
| [Добавление ресурса в каталог](entitlement-management-catalog-create.md#add-resources-to-a-catalog) | :heavy_check_mark: |  | :heavy_check_mark: |  |  |
| [Добавление владельца каталога](entitlement-management-catalog-create.md#add-additional-catalog-owners) | :heavy_check_mark: |  | :heavy_check_mark: |  |  |
| [Изменение каталога](entitlement-management-catalog-create.md#edit-a-catalog) | :heavy_check_mark: |  | :heavy_check_mark: |  |  |
| [Удаление каталога](entitlement-management-catalog-create.md#delete-a-catalog) | :heavy_check_mark: |  | :heavy_check_mark: |  |  |
| [Делегирование в Диспетчер пакетов Access](entitlement-management-delegate-managers.md) | :heavy_check_mark: |  | :heavy_check_mark: |  |  |
| [Удаление диспетчера пакетов Access](entitlement-management-delegate-managers.md#remove-an-access-package-manager) | :heavy_check_mark: |  | :heavy_check_mark: |  |  |
| [Создание нового пакета Access в каталоге](entitlement-management-access-package-create.md) | :heavy_check_mark: |  | :heavy_check_mark:  | :heavy_check_mark:  |  |
| [Изменение ролей ресурсов в пакете Access](entitlement-management-access-package-resources.md) | :heavy_check_mark: |  | :heavy_check_mark: | :heavy_check_mark: |  |
| [Создание и изменение политик](entitlement-management-access-package-request-policy.md) | :heavy_check_mark: |  | :heavy_check_mark: | :heavy_check_mark: |  |
| [Напрямую назначить пользователя пакету Access](entitlement-management-access-package-assignments.md#directly-assign-a-user) | :heavy_check_mark: |  | :heavy_check_mark: | :heavy_check_mark: |  :heavy_check_mark: |
| [Удаление пользователя из пакета Access напрямую](entitlement-management-access-package-assignments.md#remove-an-assignment) | :heavy_check_mark:  |  | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| [Просмотр пользователей, которым назначен доступ к пакету Access](entitlement-management-access-package-assignments.md#view-who-has-an-assignment) | :heavy_check_mark: |  | :heavy_check_mark: | :heavy_check_mark: |  :heavy_check_mark: |
| [Просмотр запросов пакета доступа](entitlement-management-access-package-requests.md#view-requests) | :heavy_check_mark: |  | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark:  |
| [Просмотр ошибок доставки запроса](entitlement-management-troubleshoot.md#view-a-requests-delivery-errors) | :heavy_check_mark: |  | :heavy_check_mark: | :heavy_check_mark: |  :heavy_check_mark: |
| [Повторная обработка запроса](entitlement-management-troubleshoot.md#reprocess-a-request) | :heavy_check_mark: |  | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark:  |
| [Отмена ожидающего запроса](entitlement-management-troubleshoot.md#cancel-a-pending-request) | :heavy_check_mark: |  | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| [Скрыть пакет Access](entitlement-management-access-package-edit.md#change-the-hidden-setting) | :heavy_check_mark: |  | :heavy_check_mark: | :heavy_check_mark: |  |
| [Удаление пакета Access](entitlement-management-access-package-edit.md#delete-an-access-package) | :heavy_check_mark: |  | :heavy_check_mark: | :heavy_check_mark: |  |

## <a name="required-roles-to-add-resources-to-a-catalog"></a>Роли, необходимые для добавления ресурсов в каталог

Глобальный администратор может добавлять или удалять любые группы (созданные в облаке группы безопасности или облачные группы Microsoft 365), приложения или сайты SharePoint Online в каталоге. Администратор пользователей может добавлять или удалять любые группы или приложения в каталоге, кроме группы, настроенной как назначаемая роли каталога.

Для пользователя, который не является глобальным администратором или администратором, для добавления групп, приложений или сайтов SharePoint Online в каталог этот пользователь должен иметь роль каталога Azure AD *и права управления* правами владельца каталога. В следующей таблице перечислены сочетания ролей, необходимые для добавления ресурсов в каталог. Чтобы удалить ресурсы из каталога, необходимо иметь одни и те же роли.

| Роль каталога Azure AD | Роль управления назначениями | Можно добавить группу безопасности | Можно добавить группу Microsoft 365 | Можно добавить приложение | Можно добавить сайт SharePoint Online |
| --- | :---: | :---: | :---: | :---: | :---: |
| [Глобальный администратор](../users-groups-roles/directory-assign-admin-roles.md) | Недоступно |  :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| [Администратор пользователей](../users-groups-roles/directory-assign-admin-roles.md) | Недоступно |  :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |  |
| [Администратор Intune](../users-groups-roles/directory-assign-admin-roles.md) | Владелец каталога | :heavy_check_mark: | :heavy_check_mark: |  |  |
| [Администратор Exchange](../users-groups-roles/directory-assign-admin-roles.md) | Владелец каталога |  | :heavy_check_mark: |  |  |
| [Администратор служб Team Services](../users-groups-roles/directory-assign-admin-roles.md) | Владелец каталога |  | :heavy_check_mark: |  |  |
| [Администратор SharePoint](../users-groups-roles/directory-assign-admin-roles.md) | Владелец каталога |  | :heavy_check_mark: |  | :heavy_check_mark: |
| [администратор приложений;](../users-groups-roles/directory-assign-admin-roles.md) | Владелец каталога |  |  | :heavy_check_mark: |  |
| [Администратор облачных приложений](../users-groups-roles/directory-assign-admin-roles.md) | Владелец каталога |  |  | :heavy_check_mark: |  |
| Пользователь | Владелец каталога | Только если владелец группы | Только если владелец группы | Только если владелец приложения |  |

> [!NOTE]
> Если пользователь добавляет группу безопасности или группу Microsoft 365, эта группа не может быть назначена ролью. Если пользователь добавляет группу, которая является назначаемой с помощью роли при создании пакета доступа, они также должны быть владельцами этой группы, назначаемой ролью. Дополнительные сведения см. [в статье Создание назначаемой роли группы в Azure Active Directory](../users-groups-roles/roles-groups-create-eligible.md).

Чтобы определить минимально привилегированную роль для задачи, можно также ссылаться на [роли администратора по задаче администрирования в Azure Active Directory](../users-groups-roles/roles-delegate-by-task.md#entitlement-management).

## <a name="next-steps"></a>Дальнейшие шаги

- [Делегирование управления доступом к авторам каталогов](entitlement-management-delegate-catalog.md)
- [Создание каталога ресурсов и управление им](entitlement-management-catalog-create.md)
