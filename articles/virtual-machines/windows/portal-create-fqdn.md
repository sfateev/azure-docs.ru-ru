---
title: Создание полного доменного имени для виртуальной машины Windows в портал Azure
description: Узнайте, как создать полное доменное имя (FQDN) для виртуальной машины на основе Resource Manager на портале Azure.
author: cynthn
ms.service: virtual-machines-windows
ms.subservice: networking
ms.topic: how-to
ms.workload: infrastructure-services
ms.date: 08/15/2018
ms.author: cynthn
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 7708e2ad7e84c4449d425e8bd6eb7d4d1f6a27eb
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2020
ms.locfileid: "87288247"
---
# <a name="create-a-fully-qualified-domain-name-in-the-azure-portal-for-a-windows-vm"></a>Создание полного доменного имени на портале Azure для виртуальной машины Windows

При создании виртуальной машины на [портале Azure](https://portal.azure.com) для нее автоматически создается ресурс общедоступного IP-адреса. Этот IP-адрес используется для удаленного доступа к данной виртуальной машине. Несмотря на то что портал не создает [полное доменное имя](https://en.wikipedia.org/wiki/Fully_qualified_domain_name), его можно создать после создания виртуальной машины. В этой статье показан процесс создания DNS-имени или полного доменного имени.

## <a name="create-a-fqdn"></a>Создание полного доменного имени
Для работы с руководством требуется виртуальная машина. При необходимости ее можно создать [на портале](quick-create-portal.md) или с помощью [Azure PowerShell](quick-create-powershell.md). Когда виртуальная машина будет готова, выполните следующие действия:

[!INCLUDE [virtual-machines-common-portal-create-fqdn](../../../includes/virtual-machines-common-portal-create-fqdn.md)]

Теперь вы можете удаленно подключиться к виртуальной машине с помощью DNS-имени, например, используя протокол удаленного рабочего стола (RDP).

## <a name="next-steps"></a>Дальнейшие действия
Теперь, когда у виртуальной машины имеется общедоступный IP-адрес и DNS-имя, можно развернуть общие программные платформы или службы, такие как IIS, SQL или SharePoint.

Изучите дополнительные сведения об [использовании Resource Manager](../../azure-resource-manager/management/overview.md), чтобы получить советы по созданию развертываний Azure.

