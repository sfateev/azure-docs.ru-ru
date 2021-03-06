---
title: Создание контроллера данных с помощью Azure Data CLI (аздата)
description: Создайте контроллер данных Arc Azure в обычном кластере Kubernetes с несколькими узлами, который вы уже создали с помощью интерфейса командной строки Azure (аздата).
services: azure-arc
ms.service: azure-arc
ms.subservice: azure-arc-data
author: twright-msft
ms.author: twright
ms.reviewer: mikeray
ms.date: 09/22/2020
ms.topic: how-to
ms.openlocfilehash: a405a7a32b1f54732589fb14e34ff52db4992028
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2020
ms.locfileid: "91761691"
---
# <a name="create-azure-arc-data-controller-using-the-azure-data-cli-azdata"></a>Создание контроллера данных ARC в Azure с помощью Azure Data CLI (аздата)

[!INCLUDE [azure-arc-data-preview](../../../includes/azure-arc-data-preview.md)]

## <a name="prerequisites"></a>Предварительные требования

Ознакомьтесь с разделом [Создание контроллера данных ARC в Azure](create-data-controller.md) для получения общих сведений.

Для создания контроллера данных ARC в Azure с помощью интерфейса командной строки Azure необходимо установить интерфейс командной строки Azure.

   [Установка интерфейса командной строки Azure Data CLI](install-client-tools.md)

Независимо от выбранной целевой платформы перед созданием учетной записи администратора контроллера данных необходимо задать следующие переменные среды. Эти учетные данные можно предоставить другим пользователям, которым требуется доступ администратора к контроллеру данных по мере необходимости.

**AZDATA_USERNAME** — имя пользователя, выбранного для администратора контроллера данных. Например, `arcadmin`.

**AZDATA_PASSWORD** — пароль, выбранный для пользователя администратора контроллера данных. Длина пароля должна составлять не менее восьми символов и содержать символы из трех из следующих четырех наборов: прописные буквы, строчные буквы, числа и символы.

### <a name="linux-or-macos"></a>Linux и macOS

```console
export AZDATA_USERNAME="<your username of choice>"
export AZDATA_PASSWORD="<your password of choice>"
```

### <a name="windows-powershell"></a>Windows PowerShell

```console
$ENV:AZDATA_USERNAME="<your username of choice>"
$ENV:AZDATA_PASSWORD="<your password of choice>"
```

Вам потребуется подключиться к кластеру Kubernetes и пройти проверку подлинности, после чего будет выбран существующий контекст Kubernetes перед началом создания контроллера данных ARC в Azure. Способ подключения к кластеру или службе Kubernetes меняется. См. документацию по дистрибутиву Kubernetes или службе, которую вы используете для подключения к серверу API Kubernetes.

Вы можете проверить наличие текущего подключения Kubernetes и проверить текущий контекст с помощью следующих команд.

```console
kubectl get namespace

kubectl config current-context
```

## <a name="create-the-azure-arc-data-controller"></a>Создание контроллера данных ARC в Azure

> [!NOTE]
> Можно использовать другое значение для `--namespace` параметра команды аздата Arc DC Create в приведенных ниже примерах, но обязательно используйте это имя пространства имен для `--namespace parameter` всех остальных приведенных ниже команд.

Чтобы настроить создание, следуйте указаниям в соответствующем разделе в зависимости от целевой платформы.

[Создание в службе Kubernetes Azure (AKS)](#create-on-azure-kubernetes-service-aks)

[Создание в AKS Engine в центре Azure Stack](#create-on-aks-engine-on-azure-stack-hub)

[Создание в AKS на Azure Stack ХЦИ](#create-on-aks-on-azure-stack-hci)

[Создание в Azure Red Hat OpenShift (АТО)](#create-on-azure-red-hat-openshift-aro)

[Создание на платформе контейнеров Red Hat OpenShift (OCP)](#create-on-red-hat-openshift-container-platform-ocp)

[Создание в открытом источнике, вышестоящем Kubernetes (кубеадм)](#create-on-open-source-upstream-kubernetes-kubeadm)

[Создание службы AWS эластичных Kubernetes (ЕКС)](#create-on-aws-elastic-kubernetes-service-eks)

[Создание службы в Google Cloud Kubernetes Engine (ГКЕ)](#create-on-google-cloud-kubernetes-engine-service-gke)

### <a name="create-on-azure-kubernetes-service-aks"></a>Создание в службе Kubernetes Azure (AKS)

По умолчанию профиль развертывания AKS использует `managed-premium` класс хранения. `managed-premium`Класс хранения будет работать только при наличии виртуальных машин, развернутых с помощью образов виртуальных машин, имеющих диски класса Premium.

Если вы собираетесь использовать `managed-premium` в качестве класса хранения, можно выполнить следующую команду, чтобы создать контроллер данных. Замените заполнители в команде на имя группы ресурсов, идентификатор подписки и расположение Azure.

```console
azdata arc dc create --profile-name azure-arc-aks-premium-storage --namespace arc --name arc --subscription <subscription id> --resource-group <resource group name> --location <location> --connectivity-mode indirect

#Example:
#azdata arc dc create --profile-name azure-arc-aks-premium-storage --namespace arc --name arc --subscription 1e5ff510-76cf-44cc-9820-82f2d9b51951 --resource-group my-resource-group --location eastus --connectivity-mode indirect
```

Если вы не знаете, какой класс хранения следует использовать, следует использовать `default` поддерживаемый класс хранения независимо от типа используемой виртуальной машины. Он просто не обеспечивает максимальную производительность.

Если вы хотите использовать `default` класс Storage, можно выполнить следующую команду:

```console
azdata arc dc create --profile-name azure-arc-aks-default-storage --namespace arc --name arc --subscription <subscription id> --resource-group <resource group name> --location <location> --connectivity-mode indirect

#Example:
#azdata arc dc create --profile-name azure-arc-aks-premium-storage --namespace arc --name arc --subscription 1e5ff510-76cf-44cc-9820-82f2d9b51951 --resource-group my-resource-group --location eastus --connectivity-mode indirect
```

После выполнения команды продолжите [наблюдение за состоянием создания](#monitoring-the-creation-status).

### <a name="create-on-aks-engine-on-azure-stack-hub"></a>Создание в AKS Engine в центре Azure Stack

По умолчанию профиль развертывания использует `managed-premium` класс хранения. `managed-premium`Класс хранения будет работать только при наличии рабочих виртуальных машин, развернутых с помощью образов виртуальных машин с дисков уровня "Премиум" в центре Azure Stack.

Вы можете выполнить следующую команду, чтобы создать контроллер данных с помощью класса хранилища Managed-Premium:

```console
azdata arc dc create --profile-name azure-arc-aks-premium-storage --namespace arc --name arc --subscription <subscription id> --resource-group <resource group name> --location <location> --connectivity-mode indirect

#Example:
#azdata arc dc create --profile-name azure-arc-aks-premium-storage --namespace arc --name arc --subscription 1e5ff510-76cf-44cc-9820-82f2d9b51951 --resource-group my-resource-group --location eastus --connectivity-mode indirect
```

Если вы не знаете, какой класс хранения следует использовать, следует использовать `default` поддерживаемый класс хранения независимо от типа используемой виртуальной машины. В центре Azure Stack диски уровня "Премиум" и "Стандартный" поддерживаются одной инфраструктурой хранилища. Поэтому они должны обеспечивать одинаковую общую производительность, но с разными ограничениями операций ввода-вывода.

Если вы хотите использовать `default` класс Storage, то можете выполнить эту команду.

```console
azdata arc dc create --profile-name azure-arc-aks-default-storage --namespace arc --name arc --subscription <subscription id> --resource-group <resource group name> --location <location> --connectivity-mode indirect

#Example:
#azdata arc dc create --profile-name azure-arc-aks-premium-storage --namespace arc --name arc --subscription 1e5ff510-76cf-44cc-9820-82f2d9b51951 --resource-group my-resource-group --location eastus --connectivity-mode indirect
```

После выполнения команды продолжите [наблюдение за состоянием создания](#monitoring-the-creation-status).

### <a name="create-on-aks-on-azure-stack-hci"></a>Создание в AKS на Azure Stack ХЦИ

По умолчанию профиль развертывания использует класс хранения с именем `default` и тип службы `LoadBalancer` .

Чтобы создать контроллер данных с помощью `default` класса хранения и типа службы, можно выполнить следующую команду `LoadBalancer` .

```console
azdata arc dc create --profile-name azure-arc-aks-hci --namespace arc --name arc --subscription <subscription id> --resource-group <resource group name> --location <location> --connectivity-mode indirect

#Example:
#azdata arc dc create --profile-name azure-arc-aks-hci --namespace arc --name arc --subscription 1e5ff510-76cf-44cc-9820-82f2d9b51951 --resource-group my-resource-group --location eastus --connectivity-mode indirect
```

После выполнения команды продолжите [наблюдение за состоянием создания](#monitoring-the-creation-status).


### <a name="create-on-azure-red-hat-openshift-aro"></a>Создание в Azure Red Hat OpenShift (АТО)

Чтобы создать контроллер данных в Azure Red Hat OpenShift, необходимо выполнить следующие команды в кластере, чтобы ослабить ограничения безопасности. Это временное требование, которое будет удалено в будущем.
> [!NOTE]
>   Используйте то же пространство имен здесь и в `azdata arc dc create` приведенной ниже команде. Пример: `arc` .

Сначала скачайте настраиваемое ограничение контекста безопасности (SCC) из [GitHub](https://github.com/microsoft/azure_arc/tree/master/arc_data_services/deploy/yaml) и примените его к кластеру.

Для создания контроллера данных можно выполнить следующую команду:
> [!NOTE]
>   Используйте то же пространство имен здесь и в `oc adm policy add-scc-to-user` командах выше. Пример: `arc` .

```console
azdata arc dc create --profile-name azure-arc-azure-openshift --namespace arc --name arc --subscription <subscription id> --resource-group <resource group name> --location <location> --connectivity-mode indirect

#Example
#azdata arc dc create --profile-name azure-arc-azure-openshift --namespace arc --name arc --subscription 1e5ff510-76cf-44cc-9820-82f2d9b51951 --resource-group my-resource-group --location eastus --connectivity-mode indirect
```

После выполнения команды продолжите [наблюдение за состоянием создания](#monitoring-the-creation-status).

### <a name="create-on-red-hat-openshift-container-platform-ocp"></a>Создание на платформе контейнеров Red Hat OpenShift (OCP)


> [!NOTE]
> Если вы используете платформу контейнеров Red Hat OpenShift в Azure, рекомендуется использовать последнюю доступную версию.

Чтобы создать контроллер данных на платформе контейнеров Red Hat OpenShift, необходимо выполнить следующие команды в кластере, чтобы ослабить ограничения безопасности. Это временное требование, которое будет удалено в будущем.
> [!NOTE]
>   Используйте то же пространство имен здесь и в `azdata arc dc create` приведенной ниже команде. Пример: `arc` .

```console
oc adm policy add-scc-to-user arc-data-scc --serviceaccount default --namespace arc
```

Кроме того, необходимо определить, какой класс хранения следует использовать, выполнив следующую команду.

```console
kubectl get storageclass
```

Сначала начните с создания нового файла пользовательского профиля развертывания на основе профиля развертывания Azure-Arc-openshift, выполнив следующую команду. Эта команда создаст каталог `custom` в текущем рабочем каталоге и файл пользовательского профиля развертывания `control.json` в этом каталоге.

Используйте профиль `azure-arc-openshift` для платформы контейнера OpenShift.

```console
azdata arc dc config init --source azure-arc-openshift --path ./custom
```
Используйте профиль `azure-arc-azure-openshift` для Azure RedHat Open Shift.

```console
azdata arc dc config init --source azure-arc-azure-openshift --path ./custom
```

Теперь задайте требуемый класс хранения, заменив приведенную `<storageclassname>` ниже команду на имя класса хранения, который вы хотите использовать, выполнив указанную `kubectl get storageclass` выше команду.

```console
azdata arc dc config replace --path ./custom/control.json --json-values "spec.storage.data.className=<storageclassname>"
azdata arc dc config replace --path ./custom/control.json --json-values "spec.storage.logs.className=<storageclassname>"

#Example:
#azdata arc dc config replace --path ./custom/control.json --json-values "spec.storage.data.className=mystorageclass"
#azdata arc dc config replace --path ./custom/control.json --json-values "spec.storage.logs.className=mystorageclass"
```

По умолчанию в качестве типа службы используется профиль развертывания Azure-Arc-openshift `NodePort` . Если вы используете кластер OpenShift, интегрированный с подсистемой балансировки нагрузки, можно изменить конфигурацию для использования типа службы балансировщика нагрузки с помощью следующей команды:

```console
azdata arc dc config replace --path ./custom/control.json --json-values "$.spec.services[*].serviceType=LoadBalancer"
```

При использовании OpenShift может потребоваться выполнить с политиками безопасности по умолчанию в OpenShift или нужно, чтобы среда была заблокирована чаще, чем обычная. При необходимости можно отключить некоторые функции, чтобы минимумить разрешения, необходимые во время развертывания, и во время выполнения, выполнив следующие команды.

Эта команда отключает сбор метрик для модулей Pod. Вы не сможете увидеть метрики для модулей Pod на панелях мониторинга Grafana, если эта функция отключена. Значение по умолчанию — true.

```console
azdata arc dc config replace -p ./custom/control.json --json-values spec.security.allowPodMetricsCollection=false
```

Эта команда отключает коллекции метрик о узлах. Вы не сможете увидеть метрики для узлов на панелях мониторинга Grafana, если эта функция отключена. Значение по умолчанию — true.

```console
azdata arc dc config replace --path ./custom/control.json --json-values spec.security.allowNodeMetricsCollection=false
```

Эта команда отключает возможность создавать дампы памяти для устранения неполадок.
```console
azdata arc dc config replace --path ./custom/control.json --json-values spec.security.allowDumps=false
```

Теперь все готово для создания контроллера данных с помощью следующей команды.
> [!NOTE]
>   Используйте то же пространство имен здесь и в `oc adm policy add-scc-to-user` командах выше. Пример: `arc` .

> [!NOTE]
>   `--path`Параметр должен указывать на _Каталог_ , содержащий control.jsв файле, а не на control.jsсамого файла.


```console
azdata arc dc create --path ./custom --namespace arc --name arc --subscription <subscription id> --resource-group <resource group name> --location <location> --connectivity-mode indirect

#Example:
#azdata arc dc create --path ./custom --namespace arc --name arc --subscription 1e5ff510-76cf-44cc-9820-82f2d9b51951 --resource-group my-resource-group --location eastus --connectivity-mode indirect
```

После выполнения команды продолжите [наблюдение за состоянием создания](#monitoring-the-creation-status).

### <a name="create-on-open-source-upstream-kubernetes-kubeadm"></a>Создание в открытом источнике, вышестоящем Kubernetes (кубеадм)

По умолчанию профиль развертывания кубеадм использует класс хранения с именем `local-storage` и типом службы `NodePort` . Если это допустимо, можно пропустить приведенные ниже инструкции, которые задают требуемый класс хранения и тип службы, и сразу же выполнять `azdata arc dc create` следующую команду.

Если вы хотите настроить профиль развертывания, указав конкретный класс хранения и (или) тип службы, начните с создания нового файла пользовательского профиля развертывания на основе профиля развертывания кубеадм, выполнив следующую команду. Эта команда создаст каталог `custom` в текущем рабочем каталоге и файл пользовательского профиля развертывания `control.json` в этом каталоге.

```console
azdata arc dc config init --source azure-arc-kubeadm --path ./custom
```

Можно найти доступные классы хранения, выполнив следующую команду.

```console
kubectl get storageclass
```

Теперь задайте требуемый класс хранения, заменив приведенную `<storageclassname>` ниже команду на имя класса хранения, который вы хотите использовать, выполнив указанную `kubectl get storageclass` выше команду.

```console
azdata arc dc config replace --path ./custom/control.json --json-values "spec.storage.data.className=<storageclassname>"
azdata arc dc config replace --path ./custom/control.json --json-values "spec.storage.logs.className=<storageclassname>"

#Example:
#azdata arc dc config replace --path ./custom/control.json --json-values "spec.storage.data.className=mystorageclass"
#azdata arc dc config replace --path ./custom/control.json --json-values "spec.storage.logs.className=mystorageclass"
```

По умолчанию в качестве типа службы используется профиль развертывания кубеадм `NodePort` . Если вы используете кластер Kubernetes, интегрированный с подсистемой балансировки нагрузки, можно изменить конфигурацию с помощью следующей команды.

```console
azdata arc dc config replace --path ./custom/control.json --json-values "$.spec.services[*].serviceType=LoadBalancer"
```

Теперь все готово для создания контроллера данных с помощью следующей команды.

```console
azdata arc dc create --path ./custom --namespace arc --name arc --subscription <subscription id> --resource-group <resource group name> --location <location> --connectivity-mode indirect

#Example:
#azdata arc dc create --path ./custom --namespace arc --name arc --subscription 1e5ff510-76cf-44cc-9820-82f2d9b51951 --resource-group my-resource-group --location eastus --connectivity-mode indirect
```

После выполнения команды продолжите [наблюдение за состоянием создания](#monitoring-the-creation-status).

### <a name="create-on-aws-elastic-kubernetes-service-eks"></a>Создание службы AWS эластичных Kubernetes (ЕКС)

По умолчанию класс хранения ЕКС имеет значение, `gp2` а тип службы — `LoadBalancer` .

Выполните следующую команду, чтобы создать контроллер данных с помощью указанного профиля развертывания ЕКС.

```console
azdata arc dc create --profile-name azure-arc-eks --namespace arc --name arc --subscription <subscription id> --resource-group <resource group name> --location <location> --connectivity-mode indirect

#Example:
#azdata arc dc create --profile-name azure-arc-eks --namespace arc --name arc --subscription 1e5ff510-76cf-44cc-9820-82f2d9b51951 --resource-group my-resource-group --location eastus --connectivity-mode indirect
```

После выполнения команды продолжите [наблюдение за состоянием создания](#monitoring-the-creation-status).

### <a name="create-on-google-cloud-kubernetes-engine-service-gke"></a>Создание службы в Google Cloud Kubernetes Engine (ГКЕ)

По умолчанию класс хранения ГКЕ имеет значение, `standard` а тип службы — `LoadBalancer` .

Выполните следующую команду, чтобы создать контроллер данных с помощью указанного профиля развертывания ГКЕ.

```console
azdata arc dc create --profile-name azure-arc-gke --namespace arc --name arc --subscription <subscription id> --resource-group <resource group name> --location <location> --connectivity-mode indirect

#Example:
#azdata arc dc create --profile-name azure-arc-gke --namespace arc --name arc --subscription 1e5ff510-76cf-44cc-9820-82f2d9b51951 --resource-group my-resource-group --location eastus --connectivity-mode indirect
```

После выполнения команды продолжите [наблюдение за состоянием создания](#monitoring-the-creation-status).

## <a name="monitoring-the-creation-status"></a>Наблюдение за состоянием создания

Создание контроллера займет несколько минут. Вы можете отслеживать ход выполнения в другом окне терминала с помощью следующих команд:

> [!NOTE]
>  В примерах команд ниже предполагается, что вы создали контроллер данных и пространство имен Kubernetes с именем `arc` . Если вы использовали другое имя пространства имен или контроллера данных, вы можете заменить его на `arc` свое имя.

```console
kubectl get datacontroller/arc --namespace arc
```

```console
kubectl get pods --namespace arc
```

Вы также можете проверить состояние создания любого конкретного модуля, выполнив команду, как показано ниже. Это особенно полезно для устранения любых проблем.

```console
kubectl describe po/<pod name> --namespace arc

#Example:
#kubectl describe po/control-2g7bl --namespace arc
```

## <a name="troubleshooting-creation-problems"></a>Устранение неполадок при создании

Если у вас возникли роняли с созданием, см. [руководство по устранению неполадок](troubleshoot-guide.md).
