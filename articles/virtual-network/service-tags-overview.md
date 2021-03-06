---
title: Общие сведения о тегах службы Azure
titlesuffix: Azure Virtual Network
description: Подробные сведения о тегах служб. Теги службы позволяют упростить создание правил безопасности.
services: virtual-network
documentationcenter: na
author: allegradomel
ms.service: virtual-network
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/18/2020
ms.author: kumud
ms.reviewer: kumud
ms.openlocfilehash: 863ab9b600b81006cdeb670811c61ed961e8c623
ms.sourcegitcommit: 94ca9e89501e65f4dcccc3789249357c7d5e27e5
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/19/2020
ms.locfileid: "92170262"
---
# <a name="virtual-network-service-tags"></a>Теги службы виртуальной сети
<a name="network-service-tags"></a>

Тег службы представляет группу префиксов IP-адресов из определенной службы Azure. Корпорация Майкрософт управляет префиксами адресов, входящих в тег службы, и автоматически обновляет этот тег при изменении адресов, сводя к минимуму сложность частых обновлений правил сетевой безопасности.

Теги службы можно использовать для определения элементов управления доступом к сети в [группах безопасности сети](https://docs.microsoft.com/azure/virtual-network/security-overview#security-rules)  или  [Брандмауэре Azure](https://docs.microsoft.com/azure/firewall/service-tags). Теги служб можно использовать вместо определенных IP-адресов при создании правил безопасности. Указав имя тега службы, например **ApiManagement**, в соответствующем поле *источника*   или *назначения*   правила, можно разрешить или запретить трафик для соответствующей службы.

Теги службы можно использовать для обеспечения изоляции сети и защиты ресурсов Azure от общего доступа через Интернет при доступе к службам Azure, имеющим общедоступные конечные точки. Создайте правила группы безопасности сети для входящих и исходящих подключений, чтобы запретить передачу трафика **Интернета** и разрешить входящий и исходящий трафик **AzureCloud** или других [доступных тегов](#available-service-tags) служб Azure.

## <a name="available-service-tags"></a>Доступные теги службы
В следующей таблице перечислены все теги службы, доступные для использования в правилах [группы безопасности сети](https://docs.microsoft.com/azure/virtual-network/security-overview#security-rules).

Столбцы указывают на следующее:

- Подходит ли тег для правил, охватывающих входящий или исходящий трафик.
- Поддерживает ли тег [региональные](https://azure.microsoft.com/regions) области.
- Можно ли использовать тег в правилах [Брандмауэра Azure](https://docs.microsoft.com/azure/firewall/service-tags).

По умолчанию теги службы отображают диапазоны для всего облака. Некоторые теги службы также обеспечивают более детализированный контроль за счет запрета соответствующих диапазонов IP-адресов для указанного региона. Например, тег службы **Storage** представляет службу хранилища Azure для всего облака, тогда как тег **Storage.WestUS** ограничивает диапазон только диапазонами IP-адресов хранилища из региона WestUS. В следующей таблице показано, поддерживает ли каждый тег службы такую региональную область.  

| Тег | Назначение | Может ли использовать входящий или исходящий трафик? | Может быть региональным? | Можно ли использовать с Брандмауэром Azure? |
| --- | -------- |:---:|:---:|:---:|:---:|:---:|:---:|:---:|
| **ActionGroup** | Группа действий. | Входящий трафик | Нет | Нет |
| **ApiManagement** | Трафик управления для развертываний, выделенных для управления API Azure. <br/><br/>*Примечание.* Этот тег представляет конечную точку службы управления API Azure для уровня управления для каждого региона. Это позволяет клиентам выполнять операции управления с API-интерфейсами, операциями, политиками, именованными значениями, настроенными в службе управления API.  | Входящий трафик | Да | Да |
| **ApplicationInsightsAvailability** | Проверка доступности Application Insights. | Входящий трафик | Нет | Нет |
| **AppConfiguration** | Конфигурация приложений. | Исходящие | Нет | Нет |
| **AppService**    | служба приложений Azure; Этот тег рекомендуется использовать для исходящих правил безопасности для веб-приложений и приложений функций.  | Исходящие | Да | Да |
| **AppServiceManagement** | Трафик управления для развертываний, выделенных для Среды службы приложений. | both | Нет | Да |
| **AzureActiveDirectory** | Azure Active Directory; | Исходящие | Нет | Да |
| **AzureActiveDirectoryDomainServices** | Трафик управления для развертываний, выделенных для доменных служб Azure Active Directory. | both | Нет | Да |
| **AzureAdvancedThreatProtection** | Расширенная защита от угроз Azure | Исходящие | Нет | Нет |
| **AzureBackup** |Azure Backup.<br/><br/>*Примечание.* Этот тег зависит от тегов **Storage** и **AzureActiveDirectory**. | Исходящие | Нет | Да |
| **AzureBotService** | Служба Azure Bot. | Исходящие | Нет | Нет |
| **AzureCloud** | Все [общедоступные IP-адреса центра обработки данных](https://www.microsoft.com/download/details.aspx?id=56519). | Исходящие | Да | Да |
| **AzureCognitiveSearch** | Когнитивный поиск Azure. <br/><br/>Этот тег или IP-адреса, охватываемые этим тегом, можно использовать для предоставления индексаторам безопасного доступа к источникам данных. Дополнительные сведения см. в [документации по подключению индексатора](https://docs.microsoft.com/azure/search/search-indexer-troubleshooting#connection-errors) . <br/><br/> *Примечание.* IP-адрес службы поиска не включен в список диапазонов IP-адресов для этого тега службы, и его **также необходимо добавить** в брандмауэр IP-адресов источников данных. | Входящий трафик | Нет | Нет |
| **AzureConnectors** | Соединители Azure Logic Apps для пробных и серверных подключений. | Входящий трафик | Да | Да |
| **AzureContainerRegistry** | Реестр контейнеров Azure. | Исходящие | Да | Да |
| **AzureCosmosDB** | Azure Cosmos DB. | Исходящие | Да | Да |
| **AzureDatabricks** | Azure Databricks. | both | Нет | Нет |
| **AzureDataExplorerManagement** | Управление обозревателем Azure Data Explorer. | Входящий трафик | Нет | Нет |
| **AzureDataLake** | Azure Data Lake Storage 1-го поколения. | Исходящие | Нет | Да |
| **AzureDevSpaces** | Azure Dev Spaces. | Исходящие | Нет | Нет |
| **AzureEventGrid** | Сетка событий Azure. | both | Нет | Нет |
| **AzureFrontDoor.Frontend** <br/> **AzureFrontDoor.Backend** <br/> **AzureFrontDoor.FirstParty**  | Azure Front Door. | both | Нет | Нет |
| **AzureInformationProtection** | Azure Information Protection.<br/><br/>*Примечание.* Этот тег зависит от тегов **AzureActiveDirectory**, **AzureFrontDoor.Frontend** и **AzureFrontDoor.FirstParty**. | Исходящие | Нет | Нет |
| **AzureIoTHub** | Центр Интернета вещей Azure. | Исходящие | Нет | Нет |
| **AzureKeyVault** | Azure Key Vault.<br/><br/>*Примечание.* Этот тег зависит от тега **AzureActiveDirectory**. | Исходящие | Да | Да |
| **AzureLoadBalancer**. | Подсистема балансировки нагрузки инфраструктуры Azure. Этот тег преобразуется в [виртуальный IP-адрес узла](security-overview.md#azure-platform-considerations) (168.63.129.16), из которого поступают пробы работоспособности Azure. Сюда входит только пробный трафик, а не реальный трафик к внутреннему ресурсу. Если Azure Load Balancer не используется, это правило можно переопределить. | both | Нет | Нет |
| **AzureMachineLearning** | Машинное обучение Azure. | both | Нет | Да |
| **AzureMonitor** | Log Analytics, Application Insights, AzMon и настраиваемые метрики (конечные точки GiG).<br/><br/>*Примечание.* Для Log Analytics этот тег зависит от тега **Storage**. | Исходящие | Нет | Да |
| **AzureOpenDatasets** | Открытые наборы данных Azure.<br/><br/>*Примечание.* Этот тег зависит от тегов **AzureFrontDoor.Frontend** и **Storage**. | Исходящие | Нет | Нет |
| **AzurePlatformDNS** | Служба DNS базовой инфраструктуры (по умолчанию).<br/><br>Этот тег можно использовать для отключения DNS по умолчанию. Это тег следует использовать с осторожностью. Ознакомьтесь с [рекомендациями по использованию платформы Azure](https://docs.microsoft.com/azure/virtual-network/security-overview#azure-platform-considerations). Также рекомендуется протестировать этот тег, прежде чем использовать его. | Исходящие | Нет | Нет |
| **AzurePlatformIMDS** | Служба метаданных экземпляров Azure (IMDS), которая является базовой службой инфраструктуры.<br/><br/>Этот тег можно использовать для отключения IMDS по умолчанию. Это тег следует использовать с осторожностью. Ознакомьтесь с [рекомендациями по использованию платформы Azure](https://docs.microsoft.com/azure/virtual-network/security-overview#azure-platform-considerations). Также рекомендуется протестировать этот тег, прежде чем использовать его. | Исходящие | Нет | Нет |
| **AzurePlatformLKM** | Лицензирование Windows или служба управления ключами.<br/><br/>С помощью этого тега можно отключить значения по умолчанию для лицензирования. Это тег следует использовать с осторожностью. Ознакомьтесь с [рекомендациями по использованию платформы Azure](https://docs.microsoft.com/azure/virtual-network/security-overview#azure-platform-considerations).  Также рекомендуется протестировать этот тег, прежде чем использовать его. | Исходящие | Нет | Нет |
| **AzureResourceManager** | Azure Resource Manager. | Исходящие | Нет | Нет |
| **AzureSignalR** | Служба Azure SignalR. | Исходящие | Нет | Нет |
| **AzureSiteRecovery** | Azure Site Recovery.<br/><br/>*Примечание.* Этот тег зависит от тегов **AzureActiveDirectory**, **AzureKeyVault**, **EventHub**, **GuestAndHybridManagement** и **Storage**. | Исходящие | Нет | Нет |
| **AzureTrafficManager** | IP-адреса пробы Диспетчера трафика Azure.<br/><br/>Дополнительные сведения об IP-адресах пробы Диспетчера трафика см. в статье [Диспетчер трафика Azure: вопросы и ответы](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-faqs). | Входящий трафик | Нет | Да |  
| **BatchNodeManagement** | Трафик управления для развертываний, выделенных для пакетной службы Azure. | both | Нет | Да |
| **CognitiveServicesManagement** | Диапазоны адресов для трафика Cognitive Services Azure. | both | Нет | Нет |
| **DataFactory**  | Фабрика данных Azure | both | Нет | Нет |
| **DataFactoryManagement** | Трафик управления для Фабрики данных Azure. | Исходящие | Нет | Нет |
| **Dynamics365ForMarketingEmail** | Диапазоны адресов для службы маркетинговой электронной почты Dynamics 365. | Исходящие | Да | Нет |
| **ElasticAFD** | Эластичная служба Azure Front Door. | both | Нет | Нет |
| **EventHub** | . | Исходящие | Да | Да |
| **GatewayManager** | Трафик управления для развертываний, выделенных для VPN-шлюза Azure и шлюза приложений. | Входящий трафик | Нет | Нет |
| **GuestAndHybridManagement** | Служба автоматизации Azure и гостевая конфигурация. | Исходящие | Нет | Да |
| **HDInsight** | Azure HDInsight; | Входящий трафик | Да | Нет |
| **Интернет**; | Пространство IP-адресов, которые находятся за пределами виртуальной сети и к которым можно получить доступ из общедоступного сегмента Интернета.<br/><br/>К этим адресам относится [общедоступное пространство IP-адресов, принадлежащее Azure](https://www.microsoft.com/download/details.aspx?id=41653). | both | Нет | Нет |
| **LogicApps** | Azure Logic Apps. | both | Нет | Нет |
| **LogicAppsManagement** | Трафик управления для Logic Apps. | Входящий трафик | Нет | Нет |
| **MicrosoftCloudAppSecurity** | Microsoft Cloud App Security. | Исходящие | Нет | Нет |
| **MicrosoftContainerRegistry** | Реестр контейнеров для образов контейнеров Майкрософт. <br/><br/>*Примечание.* Этот тег зависит от тега **AzureFrontDoor.FirstParty**. | Исходящие | Да | Да |
| **PowerQueryOnline** | Power Query Online. | both | Нет | Нет |
| **Служебная шина** | Трафик служебной шины Azure, использующий уровень служб "Премиум". | Исходящие | Да | Да |
| **Service Fabric** | Azure Service Fabric.<br/><br/>*Примечание.* Этот тег представляет конечную точку службы Service Fabric для уровня управления для каждого региона. Это позволяет клиентам выполнять операции управления для своих кластеров Service Fabric из виртуальной сети (конечная точка, например, https://westus.servicefabric.azure.com). | both | Нет | Нет |
| **SQL** | База данных SQL Azure, база данных Azure для MySQL, база данных Azure для PostgreSQL и Azure синапсе Analytics.<br/><br/>*Примечание.* Этот тег представляет службу, но не определенные экземпляры службы. Например, тег представляет службу "База данных SQL Microsoft Azure", но не определенную базу данных или сервер SQL Azure. Этот тег не применяется к управляемому экземпляру SQL. | Исходящие | Да | Да |
| **SqlManagement** | Трафик управления для развертываний, выделенных для SQL. | both | Нет | Да |
| **Память** | служба хранилища Azure. <br/><br/>*Примечание.* Этот тег представляет службу, но не определенные экземпляры службы. Например, тег представляет службу хранилища Azure, но не определенную учетную запись хранения Azure. | Исходящие | Да | Да |
| **StorageSyncService** | Служба хранилища Azure. | both | Нет | Нет |
| **WindowsVirtualDesktop** | Виртуальный рабочий стол Windows. | both | Нет | Да |
| **VirtualNetwork**; | Адресное пространство виртуальной сети (все диапазоны IP-адресов, определенные для виртуальной сети), все адресное пространство подключенных локальных сетей, [пиринговые](virtual-network-peering-overview.md) виртуальные сети, виртуальные сети, подключенные к [шлюзу виртуальной сети](../vpn-gateway/vpn-gateway-about-vpngateways.md?toc=%2fazure%2fvirtual-network%3ftoc.json), [виртуальный IP-адрес узла](security-overview.md#azure-platform-considerations) и префиксы адресов, используемые в [определенных пользователем маршрутах](virtual-networks-udr-overview.md). Этот тег также может содержать маршруты по умолчанию. | both | Нет | Нет |

>[!NOTE]
>В классической модели развертывания (до Azure Resource Manager) поддерживается подмножество тегов, перечисленных в предыдущей таблице. Написание этих тегов различается:
>
>| Классическое написание | Эквивалентный тег Resource Manager |
>|---|---|
>| AZURE_LOADBALANCER | AzureLoadBalancer |
>| ИНТЕРНЕТ | Интернет |
>| VIRTUAL_NETWORK | Виртуальная сеть |

> [!NOTE]
> Теги службы для служб Azure обозначают используемые префиксы адресов из определенного облака. Например, базовые диапазоны IP-адресов, соответствующие значению тега **Sql** в общедоступном облаке Azure, будут отличаться от базовых диапазонов в облаке Azure для Китая.

> [!NOTE]
> Если вы реализуете [конечную точку службы для виртуальной сети](virtual-network-service-endpoints-overview.md) для службы, такой как служба хранилища Azure или "База данных SQL Azure", Azure добавляет для нее [маршрут](virtual-networks-udr-overview.md#optional-default-routes) в подсеть виртуальной сети. Префиксы адресов для маршрута — это те же префиксы или диапазоны CIDR, которые заданы в теге соответствующей службы.

## <a name="service-tags-on-premises"></a>Теги служб в локальной среде  
Вы можете получить текущий тег службы и сведения о диапазоне, которые будут включены в конфигурации локального брандмауэра. Эта информация является актуальным списком точек во времени для диапазонов IP-адресов, соответствующих каждому тегу службы. Эти сведения можно получить программно или скачав файл JSON, как описано в следующих разделах.

### <a name="use-the-service-tag-discovery-api-public-preview"></a>Использование API обнаружения тегов служб (общедоступная предварительная версия)
Актуальный список тегов служб вместе со сведениями о диапазоне IP-адресов можно получить программным способом:

- [REST](https://docs.microsoft.com/rest/api/virtualnetwork/servicetags/list)
- [Azure PowerShell](https://docs.microsoft.com/powershell/module/az.network/Get-AzNetworkServiceTag?view=azps-2.8.0&viewFallbackFrom=azps-2.3.2)
- [Azure CLI](https://docs.microsoft.com/cli/azure/network?view=azure-cli-latest#az-network-list-service-tags)

> [!NOTE]
> В общедоступной предварительной версии API обнаружения может возвращать менее актуальную информацию, чем сведения из скачиваемых файлов JSON. (См. следующий раздел.)


### <a name="discover-service-tags-by-using-downloadable-json-files"></a>Обнаружение тегов службы с помощью скачиваемых файлов JSON 
Вы можете скачать файлы JSON, которые содержат актуальный список тегов служб вместе со сведениями о диапазоне IP-адресов. Эти списки обновляются и публикуются еженедельно. Расположения для каждого облака:

- [Azure Public](https://www.microsoft.com/download/details.aspx?id=56519)
- [Azure для US Gov организаций.](https://www.microsoft.com/download/details.aspx?id=57063)  
- [Azure для Китая](https://www.microsoft.com/download/details.aspx?id=57062) 
- [Azure для Германии](https://www.microsoft.com/download/details.aspx?id=57064)   

Диапазоны IP-адресов в этих файлах находятся в нотации CIDR. 

> [!NOTE]
>Подмножество этих сведений опубликовано в файлах XML для [Azure Public](https://www.microsoft.com/download/details.aspx?id=41653), [Azure для Китая](https://www.microsoft.com/download/details.aspx?id=42064) и [Azure для Германии](https://www.microsoft.com/download/details.aspx?id=54770). Эти загрузки XML станут устаревшими 30 июня 2020 г. и больше не будут доступны после этой даты. Необходимо выполнить миграцию для использования API обнаружения или скачивания файла JSON, как описано в предыдущих разделах.

### <a name="tips"></a>Советы 
- Наличие обновления можно отслеживать по увеличению значения *changeNumber* в файле JSON. Для каждого подраздела (например, **Storage.WestUS**) имеется собственное значение *changeNumber*, которое увеличивается по мере появления изменений. Значение *changeNumber* в файле увеличивается при изменении любого из подразделов.
- Примеры синтаксического анализа сведений о теге службы (например, получение всех диапазонов адресов для хранилища в WestUS) см. в [документации по API обнаружения тегов служб PowerShell](https://aka.ms/discoveryapi_powershell).

## <a name="next-steps"></a>Дальнейшие действия
- Узнайте, как [создать группу безопасности сети](tutorial-filter-network-traffic.md).
