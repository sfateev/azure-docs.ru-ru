---
title: Проблема при создании приложения прокси приложения | Документы Майкрософт
description: Сведения об устранении неполадок при создании приложений прокси приложения на портале администрирования Azure AD.
services: active-directory
documentationcenter: ''
author: kenwith
manager: celestedg
ms.assetid: ''
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: troubleshooting
ms.date: 05/21/2018
ms.author: kenwith
ms.reviewer: asteen
ms.collection: M365-identity-device-management
ms.openlocfilehash: 2593f80db4c271d6ae4773f324cc9777e5400550
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2020
ms.locfileid: "84764967"
---
# <a name="problem-creating-an-application-proxy-application"></a>Проблема при создании приложения прокси приложения 

Ниже приведены некоторые наиболее распространенные проблемы, возникающие при создании приложения прокси приложения.

## <a name="recommended-documents"></a>Рекомендуемые документы 

Дополнительные сведения о создании приложения прокси приложения на портале администрирования см. в статье [Публикация приложений с помощью прокси приложения Azure AD](application-proxy-add-on-premises-application.md).

Если при создании приложения по инструкциям из этого документа возникает ошибка, см. описание ошибки для получения сведений о ее исправлении. Большинство сообщений об ошибках включают в себя предлагаемое исправление. 

## <a name="specific-things-to-check"></a>Что нужно проверить

Чтобы избежать распространенных ошибок, проверьте следующее:

-   Используется учетная запись администратора с разрешением на создание приложения прокси приложения.

-   Внутренний URL-адрес является уникальным.

-   Внешний URL-адрес является уникальным.

-   URL-адрес начинается с http или https и заканчивается на "/".

-   URL-адрес должен включать имя домена, а не IP-адрес.

Сообщение об ошибке при создании приложения должно отображаться в правом верхнем углу. Для просмотра сообщений об ошибках также можно выбрать значок уведомления.

   ![Уведомление](./media/application-proxy-config-problem/error-message.png)

## <a name="next-steps"></a>Дальнейшие действия
[Enable Application Proxy in the Azure portal](application-proxy-add-on-premises-application.md) (Включение прокси приложения на портале Azure)
