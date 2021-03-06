---
title: Общие сведения о безопасности в Azure Data Share
description: Общие сведения о безопасности в Azure Data Share
author: jifems
ms.author: jife
ms.service: data-share
ms.topic: how-to
ms.date: 06/05/2020
ms.openlocfilehash: 10f31b74b461941b15f13e45f90b5fbc408c90fe
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2020
ms.locfileid: "86108419"
---
# <a name="security-overview-for-azure-data-share"></a>Общие сведения о безопасности в Azure Data Share

Эта статья содержит общие сведения о безопасности службы общих ресурсов Azure.

## <a name="security-overview"></a>Общие сведения о безопасности

Служба Azure Data Share использует предоставляемую Azure систему безопасности для защиты неактивных и передаваемых данных. Неактивные данные шифруются, если это поддерживается хранилищем данных. Данные также шифруются при передаче. Метаданные об общем ресурсе также шифруются при передаче и в неактивном состоянии. 

Элементы управления доступом можно настроить на уровне Azure Data Share, чтобы обеспечить доступ к службе для авторизованных пользователей. 

Общая папка данных Azure использует управляемое удостоверение (ранее известное как MSI) для доступа к хранилищам данных, которые используются для совместного использования данных. Поставщики и потребители данных не обмениваются учетными данными. Дополнительные сведения об управляемом удостоверении см. в разделе [управляемые удостоверения для ресурсов Azure](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/services-support-managed-identities). Дополнительные сведения о ролях и разрешениях, необходимых для совместного использования данных, см. в разделе [роли и требования](concepts-roles-permissions.md).

## <a name="next-steps"></a>Дальнейшие действия

Чтобы узнать, как приступить к обмену данными, перейдите к [этому](share-your-data.md) руководству.




