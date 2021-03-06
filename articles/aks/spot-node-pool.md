---
title: Предварительный просмотр. Добавление пула узлов с плашечными узлами в кластер Azure Kubernetes Service (AKS)
description: Узнайте, как добавить пул узлов с плашечными узлами в кластер Azure Kubernetes Service (AKS).
services: container-service
ms.service: container-service
ms.topic: article
ms.date: 02/25/2020
ms.openlocfilehash: dbb003c287a18810c2c14c4f2ea401fa55cca427
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2020
ms.locfileid: "87987296"
---
# <a name="preview---add-a-spot-node-pool-to-an-azure-kubernetes-service-aks-cluster"></a>Предварительный просмотр. Добавление пула узлов с плашечными узлами в кластер Azure Kubernetes Service (AKS)

Пул узлов — это пул узлов, поддерживаемый [плашечным масштабируемым набором виртуальных машин][vmss-spot]. Использование плашечных виртуальных машин для узлов с кластером AKS позволяет использовать преимущества неиспользованной емкости в Azure с значительной экономией затрат. Объем доступной неиспользуемой емкости зависит от многих факторов, включая размер узла, регион и время суток.

При развертывании пула плашечных узлов Azure выделит узлы смесевых ресурсов, если они доступны. Но соглашение об уровне обслуживания для узлов смесевых цветов отсутствует. Масштабируемый набор, который обеспечивает резервное копирование пула узлов, развертывается в одном домене сбоя и не предоставляет гарантии высокого уровня доступности. В любой момент, когда Azure потребуется резервная копия, инфраструктура Azure выполнит исключение узлов смесевых цветов.

Узлы смесевых цветов отлично подходят для рабочих нагрузок, которые могут справляться с прерываниями, ранними завершениями или вытеснениями. Например, рабочие нагрузки, такие как задания пакетной обработки, среды разработки и тестирования, а также крупные вычислительные рабочие нагрузки, могут быть наиболее подходящими для планирования в пуле плашечных узлов.

В этой статье вы добавите вторичный пул узлов с плашечными узлами в существующий кластер Azure Kubernetes Service (AKS).

В этой статье предполагается наличие базового понимания принципов работы Kubernetes и Azure Load Balancer. Дополнительные сведения см. в статье [Ключевые концепции Kubernetes для службы Azure Kubernetes (AKS)][kubernetes-concepts].

Эта функция в настоящее время находится на стадии предварительной версии.

Если у вас еще нет подписки Azure, [создайте бесплатную учетную запись](https://azure.microsoft.com/free/?WT.mc_id=A261C142F), прежде чем начинать работу.

## <a name="before-you-begin"></a>Перед началом

При создании кластера для использования пула плашечных узлов этот кластер также должен использовать масштабируемые наборы виртуальных машин для пулов узлов и балансировщика нагрузки уровня " *стандартный* ". Кроме того, необходимо добавить дополнительный пул узлов после создания кластера для использования пула узлов с плашечными узлами. Добавление дополнительного пула узлов рассматривается на более позднем этапе, но сначала необходимо включить предварительную версию функции.

[!INCLUDE [preview features callout](./includes/preview/preview-callout.md)]

### <a name="register-spotpoolpreview-preview-feature"></a>Регистрация функции предварительной версии спотпулпревиев

Чтобы создать кластер AKS, использующий пул узлов с плашечными узлами, необходимо включить флаг компонента *спотпулпревиев* в подписке. Эта функция предоставляет последний набор усовершенствований службы при настройке кластера.

Зарегистрируйте флаг функции *спотпулпревиев* с помощью команды [AZ Feature Register][az-feature-register] , как показано в следующем примере:

```azurecli-interactive
az feature register --namespace "Microsoft.ContainerService" --name "spotpoolpreview"
```

Через несколько минут отобразится состояние *Registered* (Зарегистрировано). Состояние регистрации можно проверить с помощью команды [az feature list][az-feature-list].

```azurecli-interactive
az feature list -o table --query "[?contains(name, 'Microsoft.ContainerService/spotpoolpreview')].{Name:name,State:properties.state}"
```

Когда все будет готово, обновите регистрацию поставщика ресурсов *Microsoft. ContainerService* с помощью команды [AZ Provider Register][az-provider-register] :

```azurecli-interactive
az provider register --namespace Microsoft.ContainerService
```

### <a name="install-aks-preview-cli-extension"></a>Установка расширения интерфейса командной строки предварительной версии AKS

Для создания кластера AKS, использующего пул узлов с плашечными узлами, требуется расширение CLI *AKS-Preview* версии 0.4.32 или выше. Установите расширение Azure CLI *aks-preview* с помощью команды [az extension add][az-extension-add], а затем проверьте наличие доступных обновлений с помощью команды [az extension update][az-extension-update].

```azurecli-interactive
# Install the aks-preview extension
az extension add --name aks-preview
 
# Update the extension to make sure you have the latest version installed
az extension update --name aks-preview
```

### <a name="limitations"></a>Ограничения

При создании кластеров AKS и управлении ими с пулом плашечных узлов применяются следующие ограничения.

* Пул узлов не может быть пулом узлов по умолчанию кластера. Пул узлов с плашечными узлами можно использовать только для вторичного пула.
* Невозможно обновить пул плашечных узлов, так как пулы плашечных узлов не гарантируют Cordon и сток. Для выполнения таких операций, как обновление версии Kubernetes, необходимо заменить существующий пул плашечных узлов новым. Чтобы заменить пул узлов с плашечными узлами, создайте пул узлов с другой версией Kubernetes, дождитесь его *состояния, а*затем удалите старый пул узлов.
* Невозможно обновить плоскость управления и пулы узлов одновременно. Необходимо обновить их отдельно или удалить пул узлов, чтобы одновременно обновить плоскость управления и остальные пулы узлов.
* Пул узлов смесевых цветов должен использовать масштабируемые наборы виртуальных машин.
* Невозможно изменить Скалесетприорити или Спотмаксприце после создания.
* При задании Спотмаксприце значение должно быть равно-1 или иметь положительное значение с длиной до пяти десятичных знаков.
* У пула узлов в плашечных узлах будет метка *kubernetes.Azure.com/scalesetpriority:Spot*, таинт *kubernetes.Azure.com/scalesetpriority=Spot:NoSchedule*и системные модули, которые будут иметь сглаживание.
* Необходимо добавить [соответствующее][spot-toleration] допустимое значение для планирования рабочих нагрузок в пуле плашечных узлов.

## <a name="add-a-spot-node-pool-to-an-aks-cluster"></a>Добавление пула точечных узлов в кластер AKS

Пул узлов смесевых цветов необходимо добавить в существующий кластер, в котором включено несколько пулов узлов. Дополнительные сведения о создании кластера AKS с несколькими пулами узлов доступны [здесь][use-multiple-node-pools].

Создайте пул узлов с помощью команды [AZ AKS нодепул Add][az-aks-nodepool-add].
```azurecli-interactive
az aks nodepool add \
    --resource-group myResourceGroup \
    --cluster-name myAKSCluster \
    --name spotnodepool \
    --priority Spot \
    --eviction-policy Delete \
    --spot-max-price -1 \
    --enable-cluster-autoscaler \
    --min-count 1 \
    --max-count 3 \
    --no-wait
```

По умолчанию пул узлов создается с *приоритетом* *обычного* в кластере AKS при создании кластера с несколькими пулами узлов. Приведенная выше команда добавляет пул вспомогательных узлов в существующий кластер AKS с *приоритетом* *точки*. *Приоритет* *точки* позволяет пулу узлов пул узлов. В приведенном выше примере параметр *политики вытеснения* имеет значение *Delete* , что является значением по умолчанию. При установке [политики вытеснения][eviction-policy] для *удаления*узлы базового масштабируемого набора пула узлов удаляются при их удалении. Можно также задать политику вытеснения для *освобождения*. При настройке политики вытеснения для *освобождения*узлы в базовом масштабируемом наборе задаются как остановленное состояние после вытеснения. Узлы в остановленном состоянии с предельным распределением по квоте вычислений и могут вызвать проблемы с масштабированием или обновлением кластера. Значения политики *приоритет* и *вытеснение* можно задать только во время создания пула узлов. Эти значения невозможно обновить позже.

Команда также включает [Автомасштабирование кластера][cluster-autoscaler], которое рекомендуется использовать с пулами плашечных узлов. В зависимости от рабочих нагрузок, выполняемых в кластере, автомасштабирование кластера масштабируется и масштабируется по количеству узлов в пуле узлов. Для пулов смесевых узлов Автомасштабирование кластера будет масштабировать количество узлов после вытеснения, если все еще нужны дополнительные узлы. При изменении максимального количества узлов, которое может иметь пул узлов, также необходимо настроить `maxCount` значение, связанное с автомасштабированием кластера. Если Автомасштабирование кластера не используется, при вытеснении плашечный пул в конечном итоге уменьшится до нуля и потребует ручной операции для получения каких-либо дополнительных узлов.

> [!Important]
> Планируйте рабочие нагрузки только для пулов смесевых узлов, которые могут обрабатывать прерывания, такие как задания пакетной обработки и тестовые среды. Рекомендуется настроить [таинтс и][taints-tolerations] допуски в пуле плашечных узлов, чтобы в пуле узлов в плашечных узлах планировались только рабочие нагрузки, которые могут обрабатывать вытеснение узла. Например, приведенная выше команда по умолчанию складывает таинт *kubernetes.Azure.com/scalesetpriority=Spot:NoSchedule* , поэтому на этом узле запланированы только модули с соответствующим допуском.

## <a name="verify-the-spot-node-pool"></a>Проверка пула узлов смесевых цветов

Чтобы проверить, добавлен ли пул узлов в пул плашечных узлов, сделайте следующее:

```azurecli
az aks nodepool show --resource-group myResourceGroup --cluster-name myAKSCluster --name spotnodepool
```

Убедитесь, что *скалесетприорити* является *плашечным*.

Чтобы запланировать выполнение модуля на плашечном узле, добавьте отклонения, соответствующие таинт, примененному к узлу узла. В следующем примере показана часть файла YAML, определяющая допустимость, которая соответствует *kubernetes.Azure.com/scalesetpriority=Spot:NoSchedule* таинт, использованному на предыдущем шаге.

```yaml
spec:
  containers:
  - name: spot-example
  tolerations:
  - key: "kubernetes.azure.com/scalesetpriority"
    operator: "Equal"
    value: "spot"
    effect: "NoSchedule"
   ...
```

При развертывании Pod с этим допуском Kubernetes может успешно запланировать модуль Pod на узлах с примененным таинт.

## <a name="max-price-for-a-spot-pool"></a>Максимальная цена для плашечного пула
[Цены на экземпляры смесевых переменных][pricing-spot]основаны на регионе и номере SKU. Дополнительные сведения см. в статье цены на [Linux][pricing-linux] и [Windows][pricing-windows].

Переменное ценообразование позволяет вам указать максимальную цену в долларах США с точностью до 5 знаков после запятой. Например, значение *0,98765* будет максимальной ценой $0,98765 долларов США в час. Если для параметра Максимальная цена задано значение *-1*, то экземпляр не будет удален на основе цены. Цена за экземпляр будет представлять собой текущую цену на точку или цену для стандартного экземпляра, в зависимости от того, сколько ресурсов и квоты доступны.

## <a name="next-steps"></a>Дальнейшие шаги

В этой статье вы узнали, как добавить пул узлов с плашечными узлами в кластер AKS. Дополнительные сведения об управлении модулями Pod в пулах узлов см. в разделе рекомендации [по использованию расширенных функций планировщика в AKS][operator-best-practices-advanced-scheduler].

<!-- LINKS - External -->
[kubernetes-services]: https://kubernetes.io/docs/concepts/services-networking/service/

<!-- LINKS - Internal -->
[aks-support-policies]: support-policies.md
[aks-faq]: faq.md
[az-extension-add]: /cli/azure/extension#az-extension-add
[az-extension-update]: /cli/azure/extension#az-extension-update
[az-feature-list]: /cli/azure/feature#az-feature-list
[az-feature-register]: /cli/azure/feature#az-feature-register
[az-group-deploy-create]: /cli/azure/group/deployment?view=azure-cli-latest#az-group-deployment-create
[az-aks-nodepool-add]: /cli/azure/aks/nodepool?view=azure-cli-latest#az-aks-nodepool-add
[az-provider-register]: /cli/azure/provider#az-provider-register
[az-template-deploy]: ../azure-resource-manager/templates/deploy-cli.md#deployment-scope
[cluster-autoscaler]: cluster-autoscaler.md
[eviction-policy]: ../virtual-machine-scale-sets/use-spot.md#eviction-policy
[kubernetes-concepts]: concepts-clusters-workloads.md
[operator-best-practices-advanced-scheduler]: operator-best-practices-advanced-scheduler.md
[pricing-linux]: https://azure.microsoft.com/pricing/details/virtual-machine-scale-sets/linux/
[pricing-spot]: ../virtual-machine-scale-sets/use-spot.md#pricing
[pricing-windows]: https://azure.microsoft.com/pricing/details/virtual-machine-scale-sets/windows/
[spot-toleration]: #verify-the-spot-node-pool
[taints-tolerations]: operator-best-practices-advanced-scheduler.md#provide-dedicated-nodes-using-taints-and-tolerations
[use-multiple-node-pools]: use-multiple-node-pools.md
[vmss-spot]: ../virtual-machine-scale-sets/use-spot.md