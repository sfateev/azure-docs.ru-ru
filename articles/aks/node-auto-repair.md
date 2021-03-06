---
title: Автоматическое восстановление узлов Azure Kubernetes Service (AKS)
description: Сведения о функции автоматического восстановления узла и о том, как AKS устраняет неисправные рабочие узлы.
services: container-service
ms.topic: conceptual
ms.date: 08/24/2020
ms.openlocfilehash: 781a1ffebb40b0cce9f18699d308db90633e8626
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2020
ms.locfileid: "89490111"
---
# <a name="azure-kubernetes-service-aks-node-auto-repair"></a>Автоматическое восстановление узла службы Azure Kubernetes Service (AKS)

AKS постоянно проверяет состояние работоспособности рабочих узлов и выполняет автоматическое восстановление узлов, если они становятся неработоспособными. Этот документ информирует операторов о том, как автоматическая функция восстановления узла ведет себя как для узлов Windows, так и для Linux. В дополнение к восстановлению AKS, платформа виртуальной машины Azure [выполняет обслуживание на виртуальных машинах][vm-updates] , которые также испытывают проблемы. AKS и виртуальные машины Azure совместно работают, чтобы минимизировать перерывы в обслуживании для кластеров.

## <a name="how-aks-checks-for-unhealthy-nodes"></a>Как AKS проверяет наличие неработоспособных узлов

AKS использует правила, чтобы определить, является ли узел неработоспособным, и требуется ли его восстановление. AKS использует следующие правила, чтобы определить, требуется ли автоматическое восстановление.

* Узел сообщает о состоянии **NotReady** при последовательных проверках в течение 10 минут.
* Узел не сообщает о состоянии в течение 10 минут.

Состояние работоспособности узлов можно проверить вручную с помощью kubectl.

```
kubectl get nodes
```

## <a name="how-automatic-repair-works"></a>Как работает автоматическое восстановление

> [!Note]
> AKS инициирует операции восстановления с учетной записью пользователя **AKS-ремедиатор**.

Если узел находится в неработоспособном состоянии в соответствии с приведенными выше правилами и остается неработоспособным в течение 10 минут подряд, выполняются следующие действия.

1. Перезагрузка узла
1. Если перезагрузка завершилась неудачно, переобразировать узел
1. Если повторное создание образа завершилось неудачно, создайте и переобразировать новый узел

Если ни одно из действий не прошло успешно, инженеры AKS изучает дополнительные исправления. В случае неработоспособности нескольких узлов во время проверки работоспособности каждый узел восстанавливается отдельно перед началом восстановления.

## <a name="next-steps"></a>Дальнейшие действия

Примените [Зоны доступности][availability-zones], чтобы повысить уровень доступности для рабочих нагрузок кластера AKS.

<!-- LINKS - External -->

<!-- LINKS - Internal -->
[availability-zones]: ./availability-zones.md
[vm-updates]: ../virtual-machines/maintenance-and-updates.md
