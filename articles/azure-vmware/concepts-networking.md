---
title: Основные понятия — сетевое взаимодействие
description: Узнайте о ключевых аспектах и вариантах использования сети и взаимосвязи в решении VMware для Azure.
ms.topic: conceptual
ms.date: 09/21/2020
ms.openlocfilehash: f8e9ed143d53afe2f7a24c832c69390c6ffcb36b
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2020
ms.locfileid: "91575764"
---
# <a name="azure-vmware-solution-networking-and-interconnectivity-concepts"></a>Сети решения Azure VMware и основные понятия взаимодействия

[!INCLUDE [avs-networking-description](includes/azure-vmware-solution-networking-description.md)]

Полезной перспективой взаимодействия является рассмотрение двух типов реализаций частных облаков решения Azure VMware:

1. [**Простое взаимодействие только с Azure**](#azure-virtual-network-interconnectivity) позволяет управлять частным облаком и использовать его только с одной виртуальной сетью в Azure. Эта реализация лучше всего подходит для оценок и реализаций решений VMware для Azure, которые не нуждаются в доступе из локальных сред.

1. [**Возможность взаимодействия между локальными и частным облакоми**](#on-premises-interconnectivity) расширяет базовую реализацию только Azure, включая взаимодействие между локальными и частными облаками решения Azure VMware.
 
В этой статье мы рассмотрим несколько ключевых концепций, которые устанавливают сетевые подключения и взаимосвязь, в том числе требования и ограничения. Мы также расскажем больше информации о двух типах реализаций взаимодействия частного облака Azure VMware с частным облаком. Эта статья содержит сведения, необходимые для настройки сети для правильной работы с решением VMware в Azure.

## <a name="azure-vmware-solution-private-cloud-use-cases"></a>Варианты использования частного облака решения Azure VMware

Ниже перечислены варианты использования частных облаков решений Azure VMware.
- Новые рабочие нагрузки виртуальных машин VMware в облаке
- Передача рабочей нагрузки виртуальной машины в облако (только для решения VMware из локальной среды в Azure)
- Миграция рабочей нагрузки виртуальной машины в облако (только для решения VMware из локальной среды в Azure)
- Аварийное восстановление (решение Azure VMware для Azure VMware или локальное решение для Azure VMware)
- Использование служб Azure

> [!TIP]
> Все варианты использования службы решения Azure VMware включены с подключением между локальными и частными облаками.

## <a name="azure-virtual-network-interconnectivity"></a>Взаимодействие виртуальной сети Azure

В реализации частного облака из виртуальной сети вы можете управлять частным облаком решения Azure VMware, использовать рабочие нагрузки в частном облаке и получать доступ к службам Azure через подключение ExpressRoute. 

На схеме ниже показана базовая сетевая связь, установленная во время развертывания частного облака. В нем показана логическая сеть на основе ExpressRoute между виртуальной сетью в Azure и частным облаком. Взаимодействие выполняет три основных варианта использования:
* Входящий доступ к vCenter Server и НСКС-T Manager, доступный из виртуальных машин в подписке Azure, а не из локальных систем. 
* Исходящий доступ из виртуальных машин к службам Azure. 
* Входящий доступ и использование рабочих нагрузок, использующих частное облако.

:::image type="content" source="media/concepts/adjacency-overview-drawing-single.png" alt-text="Подключение базовой виртуальной сети к частному облаку" border="false":::

## <a name="on-premises-interconnectivity"></a>Локальное взаимодействие

В виртуальной сети и в локальной среде до полной реализации частного облака вы можете получить доступ к частным облакам решения Azure VMware из локальных сред. Эта реализация является расширением базовой реализации, описанной в предыдущем разделе. Как и базовая реализация, требуется канал ExpressRoute, но при такой реализации он используется для подключения из локальных сред к частному облаку в Azure. 

На схеме ниже показано взаимодействие между локальными и частным облаком, которое позволяет использовать следующие варианты использования:
* Горячий/холодный кросс-vCenter vMotion
* Доступ из локальной среды в Azure VMware решение для управления частным облаком

:::image type="content" source="media/concepts/adjacency-overview-drawing-double.png" alt-text="Подключение базовой виртуальной сети к частному облаку" border="false":::

Чтобы обеспечить полное взаимодействие с частным облаком, включите ExpressRoute Global Reach а затем запросите ключ авторизации и идентификатор частного пиринга для Global Reach в портал Azure. Ключ авторизации и идентификатор пиринга используются для установления Global Reach между каналом ExpressRoute в подписке и каналом ExpressRoute для нового частного облака. После связывания две цепи ExpressRoute направляют сетевой трафик между локальными средами в частное облако.  Инструкции по запросу и использованию ключа авторизации и идентификатора пиринга см. в [руководстве по созданию подключения ExpressRoute Global REACH к частному облаку](tutorial-expressroute-global-reach-private-cloud.md) .



## <a name="next-steps"></a>Дальнейшие шаги 
Узнайте о [концепциях частного облачного хранилища](concepts-storage.md).


<!-- LINKS - external -->
[enable Global Reach]: ../expressroute/expressroute-howto-set-global-reach.md

<!-- LINKS - internal -->

