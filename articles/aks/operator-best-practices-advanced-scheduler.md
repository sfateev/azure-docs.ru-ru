---
title: Рекомендации по использованию функций планировщика
titleSuffix: Azure Kubernetes Service
description: Ознакомьтесь с рекомендациями для операторов по использованию расширенных возможностей планировщика в службе Azure Kubernetes (AKS), включая отметки и толерантности, селекторы узлов, подобие, подобие модулей pod и анти-подобие
services: container-service
ms.topic: conceptual
ms.date: 11/26/2018
ms.openlocfilehash: b8077a772d6fdc4b911fabdfa893a15dcd7615db
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2020
ms.locfileid: "87530067"
---
# <a name="best-practices-for-advanced-scheduler-features-in-azure-kubernetes-service-aks"></a>Рекомендации по расширенным возможностям планировщика в службе Azure Kubernetes (AKS)

При управлении кластерами в Cлужбе Azure Kubernetes (AKS) часто возникает необходимость изолировать команды и рабочие нагрузки. Планировщик Kubernetes предоставляет дополнительные возможности, позволяющие управлять тем, какие модули управления питанием могут быть запланированы на определенных узлах, а также как можно распределять приложения из нескольких модулей в кластере. 

В этой статье с рекомендациями главное внимание уделяется расширенным возможностям планирования Kubernetes для операторов кластера. Вы узнаете, как выполнять следующие задачи:

> [!div class="checklist"]
> * Использование отметок и толерантностей для ограничения модулей pod, которые могут назначаться на узлы
> * Назначение модулям pod предпочтений для запуска на определенных узлах с помощью селекторов узлов или подобия узлов
> * Разделение или группирование модулей pod на основе их подобия или анти-подобия

## <a name="provide-dedicated-nodes-using-taints-and-tolerations"></a>Выделение узлов на основе отметок и толерантностей

**Рекомендация.** Ограничьте ресурсоемким приложениям (например, контроллерам входящего трафика) доступ к определенным узлам. Сохраните ресурсы для нуждающихся в них важных рабочих нагрузок, не разрешая назначать другие нагрузки на эти узлы.

При создании кластера AKS можно развернуть узлы с поддержкой GPU или множеством мощных процессоров. Такие узлы часто используются для рабочих нагрузок по обработке больших массивов данных, например для задач машинного обучения или искусственного интеллекта. Так как обычно подобное оборудование — дорогостоящий для развертывания ресурс, имеет смысл ограничить рабочую нагрузку, которую можно назначить на эти узлы. Или же можно выделить определенные узлы в кластере специально для служб входящего трафика, исключив для них другую рабочую нагрузку.

Эта поддержка для различных узлов предоставляется с помощью нескольких пулов узлов. Кластер AKS предоставляет один или несколько пулов узлов.

Для ограничения рабочих нагрузок, которые могут выполняться на узлах, в планировщике Kubernetes используются параметры taint (отметка) и toleration (толерантность).

* **Отметка** применяется к узлу, указывая, что на него могут назначаться только определенные модули pod.
* Затем к модулям pod применяется параметр **toleration**, указывающий, что они *допускают* метку узла.

При развертывании модуля pod в кластере AKS модули назначаются только на те узлы, толерантность которых соответствует имеющейся отметке. Например, предположим, что у вас есть пул узлов в кластере AKS для узлов с поддержкой GPU. Задаем имя, например *gpu*, и значение для назначения. Если задано значение *NoSchedule*, планировщик Kubernetes не может назначить на этот узел модули pod, для которых не задана соответствующая толерантность.

```console
kubectl taint node aks-nodepool1 sku=gpu:NoSchedule
```

Применив отметку к узлам, зададим толерантность в спецификации модулей pod, разрешающую назначение на такие узлы. В следующем примере задаются значения `sku: gpu` и `effect: NoSchedule`, допускающие отметку, примененную к узлу на предыдущем шаге:

```yaml
kind: Pod
apiVersion: v1
metadata:
  name: tf-mnist
spec:
  containers:
  - name: tf-mnist
    image: microsoft/samples-tf-mnist-demo:gpu
    resources:
      requests:
        cpu: 0.5
        memory: 2Gi
      limits:
        cpu: 4.0
        memory: 16Gi
  tolerations:
  - key: "sku"
    operator: "Equal"
    value: "gpu"
    effect: "NoSchedule"
```

При развертывании этого модуля pod, например с помощью `kubectl apply -f gpu-toleration.yaml`, Kubernetes может успешно назначать данный модуль на узлы с примененной отметкой. Такое логическое ограничение позволяет управлять доступом к ресурсам в пределах кластера.

Если вы применяете отметки, обсудите с владельцами и разработчиками приложений, какие толерантности нужны им для развертывания.

Дополнительные сведения об использовании нескольких пулов узлов в AKS см. в статье [Создание пулов нескольких узлов для кластера в AKS и управление ими][use-multiple-node-pools].

### <a name="behavior-of-taints-and-tolerations-in-aks"></a>Поведение таинтс и допусков в AKS

При обновлении пула узлов в AKS таинтс и допуски следуют шаблону набора, так как они применяются к новым узлам:

- **Кластеры по умолчанию, использующие масштабируемые наборы виртуальных машин**
  - Вы можете [таинт нодепул][taint-node-pool] из API AKS, чтобы вновь масштабируемые узлы получали указанный API node таинтс.
  - Предположим, что у вас есть два узла: Cluster- *NODE1* и *NODE2*. Вы обновляете пул узлов.
  - Создаются два дополнительных узла, *Node3* и *node4*, а таинтс передаются соответственно.
  - Исходные *NODE1* и *NODE2* удаляются.

- **Кластеры без поддержки масштабируемого набора виртуальных машин**
  - Опять же, предположим, что у вас есть два узла: Cluster- *NODE1* и *NODE2*. При обновлении создается дополнительный узел (*Node3*).
  - Таинтс из *NODE1* применяются к *Node3*, затем *NODE1* удаляется.
  - Создается еще один новый узел (с именем *NODE1*, так как предыдущий *NODE1* был удален), а *NODE2* таинтс применяется к новому *NODE1*. Затем *NODE2* удаляется.
  - В сущности *NODE1* превращается в *Node3*, а *NODE2* преобразуется в *NODE1*.

При масштабировании пула узлов в AKS таинтс и допуски не переносятся в проект.

## <a name="control-pod-scheduling-using-node-selectors-and-affinity"></a>Управление назначением моделей pod с помощью селекторов узлов и подобия

**Рекомендация.** Управляйте назначением модулей pod на узлы с помощью таких параметров, как node selector (селектор узла), node affinity (подобие узла) и inter-pod affinity (подобие между модулями pod). Эти параметры позволяют планировщику Kubernetes логически ограничивать рабочие нагрузки, например, аппаратно в узле.

Отметки и толерантности используются для жесткой логической изоляции ресурсов: если модуль pod не допускает отметку узла, он не назначается на этот узел. Альтернативный подход — использование селекторов узла. В этом случае вы помечаете узлы, например, чтобы указать наличие локального хранилища SSD или большого объема памяти, а затем задаете селектор узла в спецификации модуля pod. После этого планировщик Kubernetes назначает модули pod на соответствующий узел. В отличие от толерантности, модули pod без соответствующего селектора узла могут назначаться на помеченные узлы. Такое поведение позволяет задействовать неиспользуемые ресурсы на узлах, отдавая приоритет модулям pod, для которых задан соответствующий селектор узла.

Рассмотрим пример узлов с большим объемом памяти. Эти узлы могут отдавать предпочтение модулям pod, запрашивающим большой объем памяти. Но чтобы ресурсы не простаивали, они разрешают запускать и другие модули pod.

```console
kubectl label node aks-nodepool1 hardware=highmem
```

Затем спецификация pod добавляет свойство `nodeSelector`, чтобы задать селектор узла, соответствующий метке на узле:

```yaml
kind: Pod
apiVersion: v1
metadata:
  name: tf-mnist
spec:
  containers:
  - name: tf-mnist
    image: microsoft/samples-tf-mnist-demo:gpu
    resources:
      requests:
        cpu: 0.5
        memory: 2Gi
      limits:
        cpu: 4.0
        memory: 16Gi
  nodeSelector:
      hardware: highmem
```

Если вы используете эти параметры планировщика, обсудите с владельцами и разработчиками приложений, как правильно задать спецификации pod.

Дополнительные сведения об использовании селекторов узла см. в статье [Assigning Pods to Nodes][k8s-node-selector] (Назначение модулей pod на узлы).

### <a name="node-affinity"></a>Подобие узла

Селектор узла — это базовый способ назначения модулей pod на определенные узлы. Использование параметра *node affinity* (подобие узла) предоставляет дополнительные возможности. С помощью этого параметра можно указать, что происходит в случае, когда модуль pod не соответствует узлу. Вы можете *требовать*, чтобы планировщик Kubernetes подбирал pod в соответствии с помеченным хостом. Или же вы можете *отдавать предпочтение* такому соответствию, но разрешать назначать pod на другой хост, если нет соответствующего.

В следующем примере для подобия узла задано значение *requiredDuringSchedulingIgnoredDuringExecution*. Этот параметр требует, чтобы планировщик Kubernetes использовал узел с соответствующей меткой. Если нет ни одного подходящего узла, модуль pod ожидает назначения. Чтобы разрешить планирование Pod на другом узле, можно присвоить этому параметру значение *преферреддурингсчедулингигноредуринжексекутион*:

```yaml
kind: Pod
apiVersion: v1
metadata:
  name: tf-mnist
spec:
  containers:
  - name: tf-mnist
    image: microsoft/samples-tf-mnist-demo:gpu
    resources:
      requests:
        cpu: 0.5
        memory: 2Gi
      limits:
        cpu: 4.0
        memory: 16Gi
  affinity:
    nodeAffinity:
      requiredDuringSchedulingIgnoredDuringExecution:
        nodeSelectorTerms:
        - matchExpressions:
          - key: hardware
            operator: In
            values: highmem
```

Часть *IgnoredDuringExecution* указывает, что при смене меток узла не нужно удалять с него pod. Планировщик Kubernetes использует обновленные метки узла только для назначения новых модулей pod, а не для тех, которые уже назначены на этот узел.

Дополнительные сведения см. в статье [Affinity and anti-affinity][k8s-affinity] (Подобие и анти-подобие).

### <a name="inter-pod-affinity-and-anti-affinity"></a>Подобие между модулями pod и анти-подобие

Последний способ логической изоляции рабочих нагрузок планировщиком Kubernetes — использование параметров inter-pod affinity (подобие между модулями pod) и anti-affinity (анти-подобие). Эти параметры указывают, что либо *не следует* назначать модули pod на узел, для которого уже есть соответствующий pod, либо же что это *следует* делать. По умолчанию планировщик Kubernetes пытается распределить множество модулей pod в наборе реплик по узлам. Вы можете задать это поведение более конкретными правилами.

Хороший пример — веб-приложение, использующее кэш Redis для Azure. Вы можете использовать правила анти-подобия модулей pod, чтобы планировщик Kubernetes распределял реплики по узлам. Затем можно использовать правила сходства, чтобы убедиться, что каждый компонент веб-приложения запланирован на одном узле с соответствующим кэшем. Вот как выглядит распределение модулей pod между узлами:

| **Узел 1** | **Узел 2** | **Узел 3** |
|------------|------------|------------|
| веб-приложение-1   | веб-приложение-2   | веб-приложение-3   |
| кэш-1    | кэш-2    | кэш-3    |

Это пример более сложного развертывания, чем в случае использования селекторов узла или подобия узлов. Такое развертывание позволяет управлять тем, как Kubernetes назначает модули pod на узлы, и логически изолировать ресурсы. Полный пример этого веб-приложения с примером использования кэша Azure для Redis см. в разделе [совместное размещение модулей Pod на одном узле][k8s-pod-affinity].

## <a name="next-steps"></a>Дальнейшие действия

Эта статья посвящена расширенным возможностям планировщика Kubernetes. Дополнительную информацию об операциях кластера в AKS см. в рекомендациях на такие темы:

* [Мультитенантность и изоляция кластера][aks-best-practices-scheduler]
* [Основные функции планировщика Kubernetes][aks-best-practices-scheduler]
* [Аутентификация и авторизация][aks-best-practices-identity]

<!-- EXTERNAL LINKS -->
[k8s-taints-tolerations]: https://kubernetes.io/docs/concepts/configuration/taint-and-toleration/
[k8s-node-selector]: https://kubernetes.io/docs/concepts/configuration/assign-pod-node/
[k8s-affinity]: https://kubernetes.io/docs/concepts/configuration/assign-pod-node/#affinity-and-anti-affinity
[k8s-pod-affinity]: https://kubernetes.io/docs/concepts/configuration/assign-pod-node/#always-co-located-in-the-same-node

<!-- INTERNAL LINKS -->
[aks-best-practices-scheduler]: operator-best-practices-scheduler.md
[aks-best-practices-cluster-isolation]: operator-best-practices-cluster-isolation.md
[aks-best-practices-identity]: operator-best-practices-identity.md
[use-multiple-node-pools]: use-multiple-node-pools.md
[taint-node-pool]: use-multiple-node-pools.md#specify-a-taint-label-or-tag-for-a-node-pool
