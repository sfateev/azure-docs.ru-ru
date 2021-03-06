---
title: Что такое групповое лицензирование в Azure Active Directory? | Документация Майкрософт
description: Сведения о лицензиях групп Azure Active Directory, принципы их работы и рекомендации.
services: active-directory
keywords: Лицензирование Azure AD
author: ajburnle
manager: daveba
ms.service: active-directory
ms.subservice: fundamentals
ms.topic: conceptual
ms.workload: identity
ms.date: 10/29/2018
ms.author: ajburnle
ms.reviewer: krbain
ms.custom: it-pro, seodec18
ms.collection: M365-identity-device-management
ms.openlocfilehash: 9bb0c1773a08bc934eebc4f110cec43e4b07e49e
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2020
ms.locfileid: "89565061"
---
# <a name="what-is-group-based-licensing-in-azure-active-directory"></a>Что такое лицензии групп в Azure Active Directory?

Для платных облачных служб Майкрософт, таких как Microsoft 365, Enterprise Mobility + Security, Dynamics 365 и других аналогичных продуктов, требуются лицензии. Эти лицензии назначаются каждому пользователю, которому нужен доступ к этим службам. Для управления лицензиями администраторы используют один из порталов управления (Office или Azure) и командлеты PowerShell. Azure Active Directory (Azure AD) — это базовая инфраструктура, обеспечивающая управление удостоверениями для всех облачных служб Майкрософт. Azure AD хранит сведения о состоянии назначения лицензии для пользователей.

До настоящего момента лицензии можно было назначать только на уровне отдельного пользователя, что могло усложнять крупномасштабное управление. Например, чтобы добавить или удалить лицензии пользователей в случае изменений в организации (например, когда пользователи приходят в организацию или отдел или покидают его), администратору часто приходится создавать сложный сценарий PowerShell, выполняющий отдельные вызовы к облачной службе.

Для решения этих задач Azure AD теперь имеет групповое лицензирование. Вы можете назначить одну или несколько лицензий продуктов группе. Azure AD обеспечит назначение лицензий всем участникам этой группы. Всем новым участникам, присоединившимся к группе, назначаются соответствующие лицензии. При выходе участников из группы эти лицензии удаляются. Это устраняет необходимость автоматизировать управление лицензиями через PowerShell для отражения изменений в организации и структуре отделов на уровне отдельного пользователя.

## <a name="licensing-requirements"></a>Требования к лицензированию
Чтобы использовать групповое лицензирование, нужно наличие одной из следующих лицензий:

- Платная или пробная подписка для Azure AD Premium P1 и более поздних версий

- Платный или пробный выпуск Office 365 Enterprise E3 или Office 365 a3 или Office 365 GCC G3 или Office 365 E3 для ГКЧ или Office 365 E3 для DOD и более поздних версий

### <a name="required-number-of-licenses"></a>Необходимое количество лицензий
Если каким-либо группам назначена лицензия, то их каждому уникальному участнику тоже нужно назначить лицензию. Хотя назначить лицензии каждому участнику группы по-отдельности не требуется, необходимо иметь достаточное количество лицензий для всех участников. Например, если имеются 1000 уникальных участников, которые входят в лицензированные группы в клиенте, необходимо иметь по крайней мере 1000 лицензий, чтобы выполнить условия лицензионного соглашения.

## <a name="features"></a>Компоненты

Ниже приведены основные возможности группового лицензирования.

- Лицензии можно назначить любой группе безопасности в Azure AD. С помощью Azure AD Connect можно синхронизировать группы безопасности в локальной среде. Группы безопасности можно создать непосредственно в Azure AD (т. н. облачные группы) или автоматически с помощью функции динамических групп Azure AD.

- При назначении группе лицензии продукта администратор может отключить один или несколько планов обслуживания в продукте. Обычно это назначение делается, если организация еще не готова к использованию какой-либо службы, входящей в продукт. Например, администратор может назначить Microsoft 365 Отделу, но временно отключить службу Yammer.

- Поддерживаются все облачные службы Майкрософт, требующие лицензирования на уровне пользователя. Эта поддержка включает в себя все продукты Microsoft 365, Enterprise Mobility + Security и Dynamics 365.

- Лицензирование на основе групп в настоящее время доступно только через [портал Azure](https://portal.azure.com). Если вы в основном используете другие порталы управления для управления пользователями и группами, например [центр администрирования Microsoft 365](https://admin.microsoft.com), можно продолжить. Однако управлять лицензиями на уровне группы нужно с помощью портала Azure.

- Azure AD автоматически управляет изменениями лицензий, возникающими в результате изменений членства в группах. Как правило, изменение лицензий происходит через несколько минут после изменения членства.

- Пользователь может быть участником нескольких групп с настроенными политиками лицензирования. Кроме того, ему может быть непосредственно назначены некоторые лицензии вне групп, в которых он участвует. Итоговое состояние пользователя представляет собой сочетание всех назначенных лицензий продуктов и служб. Если пользователю назначена одна и та же лицензия из нескольких источников, она будет использоваться только один раз.

- В некоторых случаях лицензии не могут быть назначены пользователю. Например, из-за того, что в клиенте не хватает лицензий или одновременно были назначены конфликтующие службы. Администраторы имеют доступ к сведениям о пользователях, для которых Azure AD не удалось полностью обработать лицензии групп. На основе этих данных администраторы могут выполнять действия по исправлению.

## <a name="your-feedback-is-welcome"></a>Мы будем рады вашим отзывам!

Если вы хотите оставить отзыв или запрос на создание функции, это можно сделать на [администраторском форуме Azure AD](https://feedback.azure.com/forums/169401-azure-active-directory?category_id=162510).

## <a name="next-steps"></a>Дальнейшие шаги

Дополнительные сведения о других сценариях управления лицензиями на основе группы см. в следующих статьях:

* [Назначение лицензий группе в Azure Active Directory](../users-groups-roles/licensing-groups-assign.md)
* [Поиск и устранение проблем лицензирования группы в Azure Active Directory](../users-groups-roles/licensing-groups-resolve-problems.md)
* [Как перевести отдельных лицензированных пользователей на групповое лицензирование в Azure Active Directory](../users-groups-roles/licensing-groups-migrate-users.md)
* [Как безопасно перевести пользователей с отдельных лицензий продуктов с использованием группового лицензирования](../users-groups-roles/licensing-groups-change-licenses.md)
* [Дополнительные сценарии лицензирования на основе группы в Azure Active Directory](../users-groups-roles/licensing-group-advanced.md)
* [Примеры PowerShell для группового лицензирования в Azure AD](../users-groups-roles/licensing-ps-examples.md)
