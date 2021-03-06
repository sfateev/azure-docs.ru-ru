---
title: Корпоративная безопасность
titleSuffix: Azure Machine Learning
description: 'Безопасное использование Машинного обучения Azure: проверка подлинности, авторизация, безопасность сети, шифрование данных и мониторинг.'
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: aashishb
author: aashishb
ms.reviewer: larryfr
ms.date: 09/09/2020
ms.openlocfilehash: 35b39ceb7ef54b0e00eaa53dad821c9336ea88ca
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2020
ms.locfileid: "91302627"
---
# <a name="enterprise-security-for-azure-machine-learning"></a>Корпоративная безопасность для Машинного обучения Azure

Эта статья содержит сведения о доступных функциях безопасности для Машинного обучения Azure.

При использовании облачной службы рекомендуется ограничить доступ только тем пользователям, которым она нужна. Начните с изучения модели проверки подлинности и авторизации, которую использует служба. Может потребоваться также ограничить сетевой доступ или безопасно присоединить ресурсы в локальной сети к облаку. Шифрование данных также крайне важно как при хранении, так и при их перемещении между службами. Наконец, необходимо иметь возможность отслеживать службу и создавать журнал аудита всех действий.

> [!NOTE]
> Сведения в этой статье относятся к пакету SDK Python для Машинного обучения Azure версии 1.0.83.1 или более поздней.

## <a name="authentication"></a>Аутентификация

Многофакторная проверка подлинности поддерживается, если она настроена в Azure Active Directory (Azure AD). Ниже представлен процесс проверки подлинности.

1. Клиент входит в Azure AD и получает маркер проверки подлинности Azure Resource Manager.  Пользователи и субъекты-службы поддерживаются полностью.
1. Клиент предоставляет маркер проверки подлинности для Azure Resource Manager и всех служб Машинного обучения Azure.
1. Служба машинного обучения предоставляет маркер Службы машинного обучения для целевого объекта вычислений пользователя (например, Вычислительной среды Машинного обучения). Этот маркер безопасности используется целевым объектом вычислений пользователя для обратного вызова в Службу машинного обучения после завершения выполнения. Область ограничена рабочим пространством.

[![Проверка подлинности в Машинном обучении Azure](media/concept-enterprise-security/authentication.png)](media/concept-enterprise-security/authentication.png#lightbox)

Дополнительные сведения см. в статье [Настройка проверки подлинности ресурсов и рабочих процессов Машинного обучения Azure](how-to-setup-authentication.md). В этой статье содержатся сведения и примеры проверки подлинности, включая использование субъектов-служб и автоматических рабочих процессов.

### <a name="authentication-for-web-service-deployment"></a>Проверка подлинности для развертывания веб-служб

Машинное обучение Azure поддерживает две формы проверки подлинности для веб-служб: ключ и маркер проверки подлинности. Каждая веб-служба одновременно может включать только одну форму проверки подлинности.

|Метод проверки подлинности|Описание|Экземпляры контейнеров Azure|AKS|
|---|---|---|---|
|Клавиши|Ключи являются статическими и не нуждаются в обновлении. Ключи можно создавать повторно вручную.|Отключено по умолчанию| Включено по умолчанию|
|Токен|Срок действия маркеров проверки подлинности истекает через указанный период времени, их необходимо обновить.| Недоступно| Отключено по умолчанию |

Примеры кода см. в разделе [Проверка подлинности веб-службы](how-to-setup-authentication.md#web-service-authentication).

## <a name="authorization"></a>Авторизация

Вы можете создать несколько рабочих областей, и каждая рабочая область может совместно использоваться несколькими пользователями. При совместном использовании рабочей области вы управляете доступом к ней путем назначения пользователям следующих ролей:

* Владелец
* Участник
* Читатель

В следующей таблице перечислены некоторые основные операции Машинного обучения Azure и роли, которые могут их выполнять.

| Операции Машинного обучения Azure | Владелец | Участник | Читатель |
| ---- |:----:|:----:|:----:|
| Создание рабочей области | ✓ | ✓ | |
| Общая рабочая область | ✓ | |  |
| Создание целевого объекта вычислений | ✓ | ✓ | |
| Присоединение целевого объекта вычислений | ✓ | ✓ | |
| Подключение хранилищ данных | ✓ | ✓ | |
| Выполнение эксперимента | ✓ | ✓ | |
| Просмотр запусков и метрик | ✓ | ✓ | ✓ |
| Регистрация модели | ✓ | ✓ | |
| Создание образа | ✓ | ✓ | |
| Развертывание веб-службы | ✓ | ✓ | |
| Просмотр моделей и образов | ✓ | ✓ | ✓ |
| Вызов веб-службы | ✓ | ✓ | ✓ |

Если встроенные роли не соответствуют вашим потребностям, вы можете создать пользовательские роли. Пользовательские роли поддерживаются для управления всеми операциями в рабочей области, например для создания вычислений, отправки выполнения, регистрации хранилища данных или развертывания модели. Пользовательские роли могут иметь разрешения на чтение, запись или удаление для различных ресурсов рабочей области, таких как кластеры, хранилища данных, модели и конечные точки. Роль можно сделать доступной на определенном уровне рабочей области, определенном уровне группы ресурсов или определенном уровне подписки. Дополнительные сведения см. в статье [Управление доступом к рабочей области машинного обучения](how-to-assign-roles.md).

> [!WARNING]
> Машинное обучение Azure поддерживается службой Azure Active Directory для совместной работы типа "бизнес — бизнес", но в настоящее время не поддерживается службой Azure Active Directory для совместной работы типа "бизнес — потребитель".

### <a name="securing-compute-targets-and-data"></a>Обеспечение безопасности целевых объектов вычислений и данных

Владельцы и участники могут использовать все целевые объекты вычислений и хранилища данных, подключенные к рабочей области.  

У каждой рабочей области также есть связанное управляемое удостоверение, назначаемое системой, которое имеет то же имя, что и рабочая область. Управляемое удостоверение имеет следующие разрешения для подключенных ресурсов, которые используются в рабочей области.

Дополнительные сведения об управляемых удостоверениях для ресурсов Azure см. [в этой статье](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/overview).

| Ресурс | Разрешения |
| ----- | ----- |
| Рабочая область | Участник |
| Учетная запись хранения | Участник данных BLOB-объектов хранилища |
| Хранилище ключей | Доступ ко всем ключам, секретам, сертификатам |
| Реестр контейнеров Azure | Участник |
| Группа ресурсов, в которой содержится рабочая область | Участник |
| Группа ресурсов, в которой содержится хранилище ключей (если оно отличается от того, в котором находится рабочая область) | Участник |

Мы не рекомендуем администраторам отменять доступ управляемого удостоверения к ресурсам, упомянутым в предыдущей таблице. Вы можете восстановить доступ с помощью операции повторной синхронизации ключей.

Машинное обучение Azure создает дополнительное приложение (имя начинается с `aml-` или `Microsoft-AzureML-Support-App-`) с доступом на уровне участника в вашей подписке для каждого региона рабочей области. Например, если у вас есть одна рабочая область в восточной части США и одна в Северной Европе в той же подписке, вы увидите два таких приложения. Эти приложения предоставляют службу машинного обучения Azure, которая помогает управлять ресурсами вычислений.

## <a name="network-security"></a>Безопасность сети

Машинное обучение Azure использует вычислительные ресурсы других служб Azure. Вычислительные ресурсы (целевые объекты вычислений) используются для обучения и развертывания моделей. Вы можете создавать эти целевые объекты вычислений в виртуальной сети. Например, вы можете применить Виртуальную машину для обработки и анализа данных Azure, чтобы обучить модель, а затем развернуть ее в AKS.  

Дополнительные сведения см. в статье [Обзор изоляции и конфиденциальности в виртуальной сети](how-to-network-security-overview.md).

Вы также можете включить Приватный канал Azure для рабочей области. Приватный канал позволяет ограничить обмен данными с рабочей областью из виртуальной сети Azure. Дополнительные сведения см. в статье [о настройке Приватного канала](how-to-configure-private-link.md).

## <a name="data-encryption"></a>Шифрование данных

> [!IMPORTANT]
> Для шифрования производственного уровня во время __обучения__Майкрософт рекомендует использовать кластер машинное обучение Azure COMPUTE. Для шифрования производственного уровня во время __вывода__Корпорация Майкрософт рекомендует использовать службу Kubernetes Azure.
>
> Машинное обучение Azure вычислительным экземпляром является среда разработки и тестирования. При его использовании рекомендуется хранить файлы, такие как записные книжки и сценарии, в общей папке. Данные должны храниться в хранилище данных.

### <a name="encryption-at-rest"></a>Шифрование при хранении

> [!IMPORTANT]
> Если рабочая область будет содержать конфиденциальные данные, рекомендуем при ее создании установить [флаг hbi_workspace](https://docs.microsoft.com/python/api/azureml-core/azureml.core.workspace%28class%29?view=azure-ml-py&preserve-view=true#&preserve-view=truecreate-name--auth-none--subscription-id-none--resource-group-none--location-none--create-resource-group-true--sku--basic---friendly-name-none--storage-account-none--key-vault-none--app-insights-none--container-registry-none--cmk-keyvault-none--resource-cmk-uri-none--hbi-workspace-false--default-cpu-compute-target-none--default-gpu-compute-target-none--exist-ok-false--show-output-true-). `hbi_workspace`Флаг можно задать только при создании рабочей области. Его нельзя изменить для существующей рабочей области.

Этот `hbi_workspace` флаг управляет объемом [данных, собираемых корпорацией Майкрософт в целях диагностики](#microsoft-collected-data) , и обеспечивает [дополнительное шифрование в средах, управляемых корпорацией Майкрософт](../security/fundamentals/encryption-atrest.md). Кроме того, он включает следующие действия:

* Запускает шифрование локального рабочего диска в кластере Машинное обучение Azure COMPUTE, если вы не создали предыдущие кластеры в этой подписке. В противном случае необходимо создать запрос в службу поддержки, чтобы включить шифрование временных дисков для вычислительных кластеров. 
* Очищает локальный временный диск между запусками.
* Безопасно передает учетные данные для учетной записи хранения, реестра контейнеров и учетной записи SSH из уровня выполнения в вычислительные кластеры с помощью хранилища ключей.
* Включает IP-фильтрацию для того, чтобы базовые пулы пакетов невозможно было вызывать из внешних служб, отличных от AzureMachineLearningService.

#### <a name="azure-blob-storage"></a>Хранилище BLOB-объектов Azure

Машинное обучение Azure сохраняет моментальные снимки, выходные данные и журналы в учетной записи хранилища BLOB-объектов Azure, привязанной к рабочей области Машинного обучения Azure и вашей подписке. Все хранимые данные в хранилище BLOB-объектов Azure шифруются при хранении с помощью ключей под управлением корпорации Майкрософт.

Сведения о том, как использовать собственные ключи для хранимых данных в хранилище BLOB-объектов Azure, см. в статье [Настройка управляемых клиентом ключей с помощью Azure Key Vault на портале Azure](../storage/common/storage-encryption-keys-portal.md).

Данные для обучения, как правило, также хранятся в хранилище BLOB-объектов Azure, чтобы они были доступны для обучения целевых объектов вычислений. Это хранилище не управляется Машинным обучением Azure, а подключено к целевым объектам вычислений в качестве удаленной файловой системы.

__Сменить или отменить__ ключ можно в любое время. После смены ключа учетная запись хранения начнет использовать новый ключ (последняя версия) для шифрования неактивных данных. При отмене (отключении) ключа учетная запись обрабатывает неудачные запросы. Для вступления в силу операции смены или отмены обычно требуется час.

Дополнительные сведения см. в статье [Повторное создание ключей доступа к хранилищу](how-to-change-storage-access-key.md).

#### <a name="azure-cosmos-db"></a>Azure Cosmos DB

Машинное обучение Azure сохраняет метрики и метаданные в экземпляре Azure Cosmos DB. Этот экземпляр связан с подпиской Майкрософт, управляемой Машинным обучением Azure. Все хранимые данные в Azure Cosmos DB шифруются при хранении с помощью ключей под управлением корпорации Майкрософт.

Чтобы использовать собственные ключи (управляемые пользователем) для шифрования экземпляра Azure Cosmos DB, можно создать выделенный экземпляр Cosmos DB для использования с рабочей областью. Рекомендуем использовать этот подход, если вы хотите хранить данные, например данные журнала выполнения, за пределами мультитенантного экземпляра Cosmos DB, размещенного в подписке Майкрософт. 

Чтобы включить подготовку экземпляра Cosmos DB в подписке с помощью управляемых клиентом ключей, выполните следующие действия.

* Зарегистрируйте поставщики ресурсов Microsoft. MachineLearning и Microsoft.DocУментдб в подписке, если это еще не сделано.

* При создании рабочей области Машинного обучения Azure используйте следующие параметры. Оба параметра являются обязательными и поддерживаются в пакетах SDK, CLI, интерфейсах API и шаблонах Resource Manager.

    * `resource_cmk_uri`: Этот параметр является полным универсальным кодом ресурса (URI) для ключа, управляемого клиентом, в хранилище ключей, включая [сведения о версии для ключа](../key-vault/about-keys-secrets-and-certificates.md#objects-identifiers-and-versioning). 

    * `cmk_keyvault`: Этот параметр является ИД ресурса хранилища ключей в подписке. Это хранилище ключей должно находиться в одном регионе с подпиской, которая будет использоваться для рабочей области Машинного обучения Azure. 
    
        > [!NOTE]
        > Этот экземпляр хранилища ключей может отличаться от хранилища ключей, созданного Машинным обучением Azure при подготовке к работе рабочей области. Если вы хотите использовать один и тот же экземпляр хранилища ключей для рабочей области, передайте это же хранилище ключей при подготовке рабочей области с помощью [параметра key_vault](https://docs.microsoft.com/python/api/azureml-core/azureml.core.workspace%28class%29?view=azure-ml-py&preserve-view=true#&preserve-view=truecreate-name--auth-none--subscription-id-none--resource-group-none--location-none--create-resource-group-true--sku--basic---friendly-name-none--storage-account-none--key-vault-none--app-insights-none--container-registry-none--cmk-keyvault-none--resource-cmk-uri-none--hbi-workspace-false--default-cpu-compute-target-none--default-gpu-compute-target-none--exist-ok-false--show-output-true-). 

Этот Cosmos DB экземпляр создается в группе ресурсов, управляемой корпорацией Майкрософт, в вашей подписке вместе с любыми нужными ресурсами. Имя управляемой группы ресурсов указано в формате `<AML Workspace Resource Group Name><GUID>`. Если Рабочая область Машинное обучение Azure использует закрытую конечную точку, для экземпляра Cosmos DB также создается виртуальная сеть. Эта виртуальная сеть используется для защиты обмена данными между Cosmos DB и Машинное обучение Azure.

> [!IMPORTANT]
> * Не удаляйте группу ресурсов, содержащую этот Cosmos DB экземпляр, или любые ресурсы, автоматически созданные в этой группе. Если необходимо удалить группу ресурсов, Cosmos DB экземпляр и т. д., необходимо удалить рабочую область Машинное обучение Azure, которая его использует. Группа ресурсов, Cosmos DB экземпляр и другие автоматически созданные ресурсы удаляются при удалении связанной рабочей области.
> * По умолчанию [__единицы запросов__](../cosmos-db/request-units.md) для этой учетной записи Cosmos DB устанавливаются в количестве __8000__. Изменение этого значения не поддерживается.
> * Вы не можете предоставить собственную виртуальную сеть для использования с созданным экземпляром Cosmos DB. Вы также не можете изменить виртуальную сеть. Например, нельзя изменить используемый диапазон IP-адресов.

__Сменить или отменить__ ключ можно в любое время. После смены ключа Cosmos DB начнет использовать новый ключ (последняя версия) для шифрования неактивных данных. При отмене (отключении) ключа Cosmos DB обрабатывает неудачные запросы. Для вступления в силу операции смены или отмены обычно требуется час.

Дополнительные сведения см. в статье [Настройка управляемых клиентом ключей для учетной записи Azure Cosmos с помощью Azure Key Vault](../cosmos-db/how-to-setup-cmk.md).

#### <a name="azure-container-registry"></a>Реестр контейнеров Azure

Все образы контейнеров в реестре (Реестр контейнеров Azure) шифруются при хранении. Azure автоматически шифрует образ перед сохранением и расшифровывает его при извлечении Машинным обучением Azure.

Чтобы использовать собственные ключи (управляемые пользователем) для шифрования Реестра контейнеров Azure, необходимо создать собственную запись контроля доступа и подключить ее при подготовке рабочей области или шифровать экземпляр по умолчанию, который создается во время подготовки рабочей области.

> [!IMPORTANT]
> Для Машинное обучение Azure требуется включить учетную запись администратора в реестре контейнеров Azure. По умолчанию этот параметр отключен при создании реестра контейнеров. Сведения о включении учетной записи администратора см. в разделе [учетная запись администратора](/azure/container-registry/container-registry-authentication#admin-account).
>
> После создания реестра контейнеров Azure для рабочей области не удаляйте его. В противном случае будет нарушена функциональность рабочей области машинного обучения Azure.

Пример создания рабочей области с помощью существующего Реестра контейнеров Azure см. в следующих статьях:

* [Создание рабочей области для Машинного обучения Azure с помощью Azure CLI](how-to-manage-workspace-cli.md).
* [Создание рабочей области для Машинного обучения Azure с помощью шаблона Azure Resource Manager](how-to-create-workspace-template.md).

#### <a name="azure-container-instance"></a>Экземпляр контейнера Azure

Развернутый ресурс экземпляра контейнера Azure (ACI) можно зашифровать с помощью управляемых клиентом ключей. Управляемый клиентом ключ, используемый для ACI, может храниться в Azure Key Vault вашей рабочей области. Сведения о создании ключа см. в разделе [Создание ключа](../container-instances/container-instances-encrypt-data.md#generate-a-new-key).

Чтобы использовать ключ при развертывании модели в экземпляре контейнера Azure, создайте конфигурацию развертывания с помощью `AciWebservice.deploy_configuration()`. Укажите основные сведения, используя следующие параметры:

* `cmk_vault_base_url`: URL-адрес хранилища ключей, содержащего ключ.
* `cmk_key_name`: Имя ключа.
* `cmk_key_version`: версия ключа.

Дополнительные сведения о создании и использовании конфигурации развертывания см. в следующих статьях:

* Справочник по [AciWebservice.deploy_configuration()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice.aci.aciwebservice?view=azure-ml-py&preserve-view=true#&preserve-view=truedeploy-configuration-cpu-cores-none--memory-gb-none--tags-none--properties-none--description-none--location-none--auth-enabled-none--ssl-enabled-none--enable-app-insights-none--ssl-cert-pem-file-none--ssl-key-pem-file-none--ssl-cname-none--dns-name-label-none--primary-key-none--secondary-key-none--collect-model-data-none--cmk-vault-base-url-none--cmk-key-name-none--cmk-key-version-none-)
* [Где и как развернуть службы](how-to-deploy-and-where.md)
* [Развертывание веб-служб в Экземплярах контейнеров Azure](how-to-deploy-azure-container-instance.md).

Дополнительные сведения см. в разделе [Шифрование данных с помощью управляемого клиентом ключа](../container-instances/container-instances-encrypt-data.md#encrypt-data-with-a-customer-managed-key).

#### <a name="azure-kubernetes-service"></a>Служба Azure Kubernetes

Развернутый ресурс Службы Azure Kubernetes можно зашифровать с помощью управляемых клиентом ключей в любое время. Дополнительные сведения см. в статье [Использование собственных ключей (BYOK) с помощью Службы Azure Kubernetes (AKS)](../aks/azure-disk-customer-managed-keys.md). 

Этот процесс позволяет шифровать данные и диск операционной системы развернутых виртуальных машин в кластере Kubernetes.

> [!IMPORTANT]
> Он работает только с AKS K8s версии 1.17 или выше. 13 января 2020 г. в Машинное обучение Azure добавлена поддержка AKS 1.17.

#### <a name="machine-learning-compute"></a>Вычислительная среда Машинного обучения

Диск операционной системы для каждого вычислительного узла, хранящегося в службе хранилища Azure, шифруется с помощью управляемых корпорацией Майкрософт ключей в учетных записях хранения Машинного обучения Azure. Этот целевой объект вычислений является временным, и кластеры, как правило, уменьшаются в масштабе, если в очереди нет запусков. Базовая виртуальная машина освобождается, а диск ОС удаляется. Шифрование дисков Azure не поддерживается для диска ОС.

Каждая виртуальная машина также имеет локальный временный диск для операций ОС. При необходимости можно использовать диск для размещения данных для обучения. По умолчанию диск шифруется для рабочих областей с параметром `hbi_workspace`, для которого задано значение `TRUE`. Эта среда кратковременно используется только во время выполнения, а поддержка шифрования ограничена только ключами, управляемыми системой.

#### <a name="azure-databricks"></a>Azure Databricks

Azure Databricks можно использовать в конвейерах Машинного обучения. По умолчанию файловая система Databricks (DBFS), используемая Azure Databricks, шифруется с помощью ключа, управляемого корпорацией Майкрософт. Сведения о настройке Azure Databricks для использования управляемых клиентом ключей см. в статье [Настройка ключей, управляемых клиентом, в DBFS по умолчанию (корневой файловой системе)](/azure/databricks/security/customer-managed-keys-dbfs).

### <a name="encryption-in-transit"></a>Шифрование при передаче

Машинное обучение Azure использует TLS для защиты внутренней связи между различными микрослужбами Машинного обучения Azure. Доступ к службе хранилища Azure также осуществляется по безопасному каналу.

Для защиты внешних вызовов, сделанных в конечной точке оценки, Машинное обучение Azure использует TLS. Дополнительные сведения см. в статье [Использование TLS для защиты веб-службы с помощью Машинного обучения Azure](https://docs.microsoft.com/azure/machine-learning/how-to-secure-web-service).

### <a name="using-azure-key-vault"></a>Использование Azure Key Vault

Машинное обучение Azure использует экземпляр Azure Key Vault, связанный с рабочей областью, для хранения учетных данных разных видов:

* строка подключения связанной учетной записи хранения;
* пароли для экземпляров репозитория контейнеров Azure;
* строки подключения к хранилищам данных.

Пароли и ключи SSH для целевых объектов вычислений, таких как Azure HDInsight и виртуальные машины, хранятся в отдельном хранилище ключей, связанном с подпиской Майкрософт. Машинное обучение Azure не хранит пароли или ключи, предоставленные пользователями. Вместо этого оно создает, авторизует и сохраняет собственные ключи SSH для подключения к виртуальным машинам и HDInsight для выполнения экспериментов.

У каждой рабочей области есть связанное управляемое удостоверение, назначаемое системой, которое имеет то же имя, что и рабочая область. Это управляемое удостоверение имеет доступ ко всем ключам, секретам и сертификатам в хранилище ключей.

## <a name="data-collection-and-handling"></a>Сбор и обработка данных

### <a name="microsoft-collected-data"></a>Данные, собранные корпорацией Майкрософт

Майкрософт может собирать обезличенные данные, такие как имена ресурсов (например, имя набора данных или имя эксперимента машинного обучения) или переменные среды задания для диагностики. Все эти данные хранятся с помощью управляемых корпорацией Майкрософт ключей в хранилище, размещенном в собственных подписках Майкрософт, с соблюдением [стандартной политики конфиденциальности и стандартов обработки данных корпорации Майкрософт](https://privacy.microsoft.com/privacystatement).

Также рекомендуем не хранить конфиденциальные сведения (например, секретные ключи учетной записи) в переменных среды. Переменные среды записываются, шифруются и сохраняются. Аналогично при именовании [run_id](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run%28class%29?view=azure-ml-py&preserve-view=true) не следует включать конфиденциальные сведения, такие как имена пользователей или секретные имена проектов. Эти сведения могут отображаться в журналах телеметрии, доступных инженерам службы поддержки Майкрософт.

Вы можете отказаться от сбора диагностических данных, задав для параметра `hbi_workspace` значение `TRUE` при подготовке рабочей области. Эта функция поддерживается при использовании пакета SDK Python для AzureML, интерфейса командной строки, интерфейсов REST API или шаблонов Resource Manager.

### <a name="microsoft-generated-data"></a>Данные, созданные корпорацией Майкрософт

При использовании таких служб, как автоматизированное машинное обучение, корпорация Майкрософт может создавать временные предварительно обработанные данные для обучения нескольких моделей. Эти данные хранятся в хранилище данных в рабочей области, что позволяет применять управление доступом и шифрование соответствующим образом.

Вы также можете зашифровать [диагностические данные из развернутой конечной точки](how-to-enable-app-insights.md) в экземпляре Azure Application Insights.

## <a name="monitoring"></a>Наблюдение

### <a name="metrics"></a>Метрики

Вы можете использовать метрики Azure Monitor, чтобы просматривать и отслеживать метрики для рабочей области Машинного обучения Azure. На [портале Azure](https://portal.azure.com) выберите рабочую область, а затем — **Метрики**:

[![Снимок экрана с примером метрик для рабочей области](media/concept-enterprise-security/workspace-metrics.png)](media/concept-enterprise-security/workspace-metrics-expanded.png#lightbox)

Метрики включают сведения о запусках, развертываниях и регистрациях.

Дополнительные сведения см. в статье [Метрики в Azure Monitor](/azure/azure-monitor/platform/data-platform-metrics).

### <a name="activity-log"></a>Журнал действий

Вы можете просмотреть журнал действий рабочей области, чтобы увидеть выполняемые в ней различные операции. Журнал содержит основные сведения, такие как имя операции, инициатор события и метку времени.

На этом снимке экрана показан журнал действий рабочей области:

[![Снимок экрана с журналом действий рабочей области](media/concept-enterprise-security/workspace-activity-log.png)](media/concept-enterprise-security/workspace-activity-log-expanded.png#lightbox)

Сведения о запросе оценки хранятся в Application Insights. При создании рабочей области в подписке создается Application Insights. Записанные в журнал сведения включают такие поля, как:

* HTTPMethod
* UserAgent
* ComputeType
* RequestUrl
* StatusCode
* RequestId
* Duration

> [!IMPORTANT]
> Некоторые действия в рабочей области Машинного обучения Azure не регистрируются в журнале действий. Например, запуск выполнения обучения и регистрация модели.
>
> Некоторые из этих действий отображаются в области **Действия** рабочей области, но эти уведомления не указывают на инициатора действия.

### <a name="vulnerability-scanning"></a>Сканирование уязвимостей

Центр безопасности Azure обеспечивает унифицированное управление безопасностью и расширенную защиту от угроз для гибридных облачных рабочих нагрузок. Для машинного обучения Azure необходимо включить проверку ресурсов реестра контейнеров Azure и ресурсов службы Azure Kubernetes. См. статью Просмотр [изображений реестра контейнеров Azure по центру безопасности](https://docs.microsoft.com/azure/security-center/azure-container-registry-integration) и [интеграции Azure Kubernetes Services с центром безопасности](https://docs.microsoft.com/azure/security-center/azure-kubernetes-service-integration).

## <a name="data-flow-diagrams"></a>Схемы потока данных

### <a name="create-workspace"></a>Создание рабочей области

Рабочий процесс создания рабочей области показан на схеме ниже.

* Войдите в Azure AD с одного из поддерживаемых клиентов Машинного обучения Azure (Azure CLI, пакет SDK для Python, портал Azure) и запросите соответствующий маркер Azure Resource Manager.
* Чтобы создать рабочую область, вызовите Azure Resource Manager. 
* Azure Resource Manager обращается к поставщику ресурсов Машинного обучения Azure для подготовки рабочей области.

Во время создания рабочей области в подписке пользователя создаются дополнительные ресурсы:

* Key Vault (для хранения секретов);
* учетная запись хранения Azure (включая большой двоичный объект и общую папку);
* Реестр контейнеров Azure (для хранения образов Docker для экспериментирования, вывода и оценки);
* Application Insights (для хранения данных телеметрии).

При необходимости пользователь может также подготовить к работе другие целевые объекты вычислений, подключенные к рабочей области (например, Служба Azure Kubernetes или виртуальные машины).

[![Создание рабочего процесса рабочей области](media/concept-enterprise-security/create-workspace.png)](media/concept-enterprise-security/create-workspace.png#lightbox)

### <a name="save-source-code-training-scripts"></a>Сохранение исходного кода (сценариев обучения)

Рабочий процесс моментального снимка показан на схеме ниже.

Каталоги (эксперименты), которые содержат исходный код (сценарии обучения), связанные с рабочей областью Машинного обучения Azure. Эти сценарии хранятся на локальном компьютере и в облаке (в хранилище BLOB-объектов Azure вашей подписки). Моментальные снимки кода используются для выполнения или проверки исторического аудита.

[![Рабочий процесс моментального снимка кода](media/concept-enterprise-security/code-snapshot.png)](media/concept-enterprise-security/code-snapshot.png#lightbox)

### <a name="training"></a>Обучение

Рабочий процесс обучения показан на схеме ниже.

* Машинное обучение Azure вызывается с помощью идентификатора моментального снимка для моментального снимка кода, сохраненного в предыдущем разделе.
* Машинное обучение Azure создает идентификатор запуска (необязательно) и маркер проверки подлинности Службы машинного обучения, который позже используется целевыми объектами вычислений, такими как Вычислительная среда Машинного обучения или виртуальные машины для взаимодействия со Службой машинного обучения.
* Для выполнения заданий обучения можно выбрать управляемый целевой объект вычислений (например, Вычислительная среда Машинного обучения) или неуправляемый целевой объект вычислений (например, виртуальные машины). Ниже приведены потоки данных для обоих сценариев:
   * Виртуальные машины или HDInsight, доступ к которым осуществляется с помощью учетных данных SSH в хранилище ключей в подписке Майкрософт. Машинное обучение Azure выполняет код управления на целевом объекте вычислений, который:

   1. Подготавливает среду. (Docker является параметром для виртуальных машин и локальных компьютеров. Чтобы понять, как работают запущенные эксперименты в контейнерах Docker, ознакомьтесь со следующими шагами для Вычислительной среды Машинного обучения.)
   1. Скачивает код.
   1. Настраивает переменные и конфигурации среды.
   1. Запускает пользовательские сценарии (моментальный снимок кода, упомянутый в предыдущем разделе).

   * Вычислительная среда Машинного обучения, доступ к которой осуществляется с помощью удостоверения, управляемого рабочей областью.
Так как Вычислительная среда Машинного обучения является управляемым целевым объектом вычислений (т. е. управляется корпорацией Майкрософт), она выполняется в вашей подписке Майкрософт.

   1. При необходимости запускается удаленное построение Docker.
   1. Код управления записывается в общую папку пользователя Файлов Azure.
   1. Контейнер запускается с помощью начальной команды. Это и есть код управления, описанный в предыдущем шаге.

#### <a name="querying-runs-and-metrics"></a>Запросы метрик и выполнений

На приведенной ниже схеме потока этот шаг выполняется, когда целевой объект вычислений для обучения записывает метрики выполнения обратно в Машинное обучение Azure из хранилища в базе данных Cosmos DB. Машинное обучение Azure могут вызывать клиенты. Машинное обучение, в свою очередь, будет извлекать метрики из базы данных Cosmos DB и возвращать их обратно клиенту.

[![Рабочий процесс обучения](media/concept-enterprise-security/training-and-metrics.png)](media/concept-enterprise-security/training-and-metrics.png#lightbox)

### <a name="creating-web-services"></a>Создание веб-служб

Рабочий процесс вывода показан на схеме ниже. Вывод или оценка модели — это этап, в котором развернутая модель используется для прогнозирования, чаще всего для производственных данных.

Дополнительные сведения:

* Пользователь регистрирует модель с помощью клиента, например пакета SDK для Машинного обучения Azure.
* Пользователь создает образ с помощью модели, файла оценки и других зависимостей модели.
* Образ Docker создается и сохраняется в службе "Реестр контейнеров Azure".
* Веб-служба развертывается на целевом объекте вычислений (Экземпляры контейнеров или AKS) с помощью образа, созданного на предыдущем шаге.
* Сведения о запросе оценки хранятся в Application Insights, которая находится в подписке пользователя.
* Данные телеметрии также отправляются в подписку Microsoft или Azure.

[![Рабочий процесс вывода](media/concept-enterprise-security/inferencing.png)](media/concept-enterprise-security/inferencing.png#lightbox)

## <a name="audit-and-manage-compliance"></a>Аудит соответствия и управление им

[Политика Azure](/azure/governance/policy) — это средство управления, которое позволяет обеспечить соответствие ресурсов Azure политикам. С помощью Машинное обучение Azure можно назначить следующие политики:

* **Ключ, управляемый клиентом**: аудит или обеспечение того, должны ли рабочие области использовать ключ, управляемый клиентом.
* **Частная ссылка**: аудит того, используется ли в рабочих областях частная конечная точка для связи с виртуальной сетью.

Дополнительные сведения о политике Azure см. в [документации по политике Azure](/azure/governance/policy/overview).

Дополнительные сведения о политиках, характерных для Машинное обучение Azure, см. в разделе [Аудит и Управление соответствием политике Azure](how-to-integrate-azure-policy.md).

## <a name="next-steps"></a>Дальнейшие действия

* [Использование TLS для защиты веб-службы с помощью Машинного обучения Azure](how-to-secure-web-service.md).
* [Использование модели Машинного обучения Azure, развернутой в качестве веб-службы](how-to-consume-web-service.md)
* [Использование рабочей области за Брандмауэром Azure для Машинного обучения Azure](how-to-access-azureml-behind-firewall.md)
* [Защита жизненных циклов машинного обучения с помощью частных виртуальных сетей](how-to-network-security-overview.md)
* [Recommenders](https://github.com/Microsoft/Recommenders) (Системы рекомендаций)
* [Создание API рекомендаций в режиме реального времени в Azure](https://docs.microsoft.com/azure/architecture/reference-architectures/ai/real-time-recommendation)
