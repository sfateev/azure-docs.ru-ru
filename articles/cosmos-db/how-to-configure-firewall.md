---
title: Настройка брандмауэра IP-адресов для учетной записи Azure Cosmos DB
description: Узнайте, как настроить политики управления доступом на основе IP-адресов для включения поддержки брандмауэра в учетных записях Azure Cosmos.
author: markjbrown
ms.service: cosmos-db
ms.topic: how-to
ms.date: 08/24/2020
ms.author: mjbrown
ms.custom: devx-track-azurecli
ms.openlocfilehash: 69c39d2478ed7d488c1209c2c7e16c241c59bcef
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2020
ms.locfileid: "88814184"
---
# <a name="configure-ip-firewall-in-azure-cosmos-db"></a>Настройка брандмауэра IP-адресов в Azure Cosmos DB

Вы можете защитить данные, хранимые в учетной записи Azure Cosmos DB, с помощью брандмауэров IP-адресов. Для фильтрации входящего трафика Azure Cosmos DB поддерживает политики управления доступом на основе IP-адресов. Брандмауэр IP-адресов можно настроить в учетной записи Azure Cosmos DB любым из следующих способов:

* на портале Azure;
* декларативно с помощью шаблона Azure Resource Manager;
* Программно с помощью Azure CLI или Azure PowerShell путем обновления свойства **ipRangeFilter**

## <a name="configure-an-ip-firewall-by-using-the-azure-portal"></a><a id="configure-ip-policy"></a> Настройка брандмауэра IP-адресов с помощью портала Azure

Чтобы настроить политику контроля доступа на основе IP-адресов на портале Azure, перейдите на страницу учетной записи Azure Cosmos DB и выберите в меню навигации **Брандмауэр и виртуальные сети**. Измените значение параметра **Разрешить доступ из**, указав **Выбранные сети**, и нажмите **Сохранить**.

:::image type="content" source="./media/how-to-configure-firewall/azure-portal-firewall.png" alt-text="Снимок экрана, показывающий, как открыть страницу &quot;Брандмауэр&quot; на портале Azure":::

При включенном контроле доступа на основе IP-адресов портал Azure предоставляет возможность указать IP-адреса, диапазоны IP-адресов и параметры. Параметры обеспечивают доступ к другим службам Azure и порталу Azure. Сведения об этих параметрах приведены в указанных ниже разделах.

> [!NOTE]
> После включения политики контроля доступа на основе IP-адресов для учетной записи Azure Cosmos DB отклоняются все запросы к этой учетной записи Azure Cosmos DB с компьютеров, IP-адреса которых не входят в список диапазонов разрешенных IP-адресов. В соответствии с этой политикой также блокируется возможность просмотра ресурсов Azure Cosmos DB на портале.

### <a name="allow-requests-from-the-azure-portal"></a>Разрешение запросов на портале Azure

Чтобы программным способом обеспечить доступ к порталу Azure при включении политики управления доступом на основе IP-адресов, необходимо добавить его IP-адрес к свойству **ipRangeFilter**. IP-адрес портала:

|Регион|IP-адрес|
|------|----------|
|Германия|51.4.229.218|
|Китай|139.217.8.252|
|US Gov|52.244.48.71|
|Все другие регионы|104.42.195.92,40.76.54.131,52.176.6.30,52.169.50.45,52.187.184.26|

Вы можете включить запросы на доступ к портал Azure, выбрав параметр **Разрешить доступ из портал Azure** , как показано на следующем снимке экрана:

:::image type="content" source="./media/how-to-configure-firewall/enable-azure-portal.png" alt-text="Снимок экрана, показывающий, как открыть страницу &quot;Брандмауэр&quot; на портале Azure":::

### <a name="allow-requests-from-global-azure-datacenters-or-other-sources-within-azure"></a>Разрешение запросов от глобальных центров обработки данных Azure или других источников в пределах Azure

Если вы выполняете доступ к учетной записи Azure Cosmos DB из служб, которые не предоставляют статический IP-адрес (например, Azure Stream Analytics и "Функции Azure"), вы все равно можете использовать брандмауэр IP-адресов для ограничения доступа. Вы можете включить доступ из других источников в Azure, выбрав параметр **принять подключения из центров обработки данных Azure** , как показано на следующем снимке экрана:

:::image type="content" source="./media/how-to-configure-firewall/enable-azure-services.png" alt-text="Снимок экрана, показывающий, как открыть страницу &quot;Брандмауэр&quot; на портале Azure":::

При включении этого параметра IP-адрес `0.0.0.0` добавляется в список разрешенных IP-адресов. `0.0.0.0`IP-адрес позволяет ограничивать запросы учетной записи Azure Cosmos DB из диапазона IP-адресов центра обработки данных Azure. Этот параметр неприменим для получения доступа к учетной записи Azure Cosmos DB из других диапазонов IP-адресов.

> [!NOTE]
> Этот параметр настраивает брандмауэр на разрешение всех запросов из Azure, в том числе из подписок других пользователей, развернутых в Azure. Этот параметр разрешает большой список IP-адресов, что существенно снижает эффективность политики брандмауэра. Его следует использовать, только если запросы будут поступать из расположений без статических IP-адресов или подсетей в виртуальных сетях. При выборе этого варианта автоматически разрешается доступ с портала Azure, так как сам портал Azure развернут в Azure.

### <a name="requests-from-your-current-ip"></a>Запросы с текущего IP-адреса

Для упрощения разработки портал Azure помогает определить и добавить IP-адрес клиентского компьютера в список разрешенных адресов. Приложения, работающие на вашем компьютере, смогут после этого получить доступ к учетной записи Azure Cosmos DB.

Портал автоматически определяет IP-адрес клиента. Это может быть IP-адрес клиента вашего компьютера или сетевого шлюза. Удалите этот IP-адрес, прежде чем переносить рабочие нагрузки в рабочую среду.

Чтобы добавить текущий IP-адрес в список IP-адресов, выберите **Добавить мой текущий IP-адрес**. Затем нажмите кнопку **Save** (Сохранить).

:::image type="content" source="./media/how-to-configure-firewall/enable-current-ip.png" alt-text="Снимок экрана, показывающий, как открыть страницу &quot;Брандмауэр&quot; на портале Azure":::

### <a name="requests-from-cloud-services"></a>Запросы из облачных служб

В Azure облачные службы — это стандартный способ размещения логики службы среднего уровня с помощью Azure Cosmos DB. Чтобы разрешить доступ к учетной записи Azure Cosmos DB из облачной службы, обязательно добавьте общедоступный IP-адрес этой службы в список разрешенных IP-адресов, связанных с используемой учетной записью Azure Cosmos DB. Для этого [настройте политику контроля доступа на основе IP-адресов](#configure-ip-policy). Так вы предоставите всем экземплярам ролей облачных служб доступ к учетной записи Azure Cosmos DB.

IP-адреса для ваших облачных служб можно получить на портале Azure, как показано на следующем снимке экрана:

:::image type="content" source="./media/how-to-configure-firewall/public-ip-addresses.png" alt-text="Снимок экрана, показывающий, как открыть страницу &quot;Брандмауэр&quot; на портале Azure":::

При расширении облачной службы за счет добавления экземпляров ролей каждый новый экземпляр автоматически получит доступ к учетной записи Azure Cosmos DB, так как он входит в ту же облачную службу.

### <a name="requests-from-virtual-machines"></a>Запросы из виртуальных машин

Можно использовать также [виртуальные машины](https://azure.microsoft.com/services/virtual-machines/) или [масштабируемые наборы виртуальных машин](../virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md) для размещения служб среднего уровня с помощью Azure Cosmos DB. Чтобы настроить доступ к учетной записи Cosmos DB из виртуальных машин, включите общедоступные IP-адреса этих виртуальных машин и (или) масштабируемого набора виртуальных машин в список разрешенных IP-адресов для этой учетной записи. Для этого [настройте политику контроля доступа на основе IP-адресов](#configure-ip-policy).

IP-адреса для виртуальных машин можно узнать на портале Azure, как показано на следующем снимке экрана:

:::image type="content" source="./media/how-to-configure-firewall/public-ip-addresses-dns.png" alt-text="Снимок экрана, показывающий, как открыть страницу &quot;Брандмауэр&quot; на портале Azure":::

Добавленные в группу экземпляры виртуальных машин автоматически получают доступ к учетной записи Azure Cosmos DB.

### <a name="requests-from-the-internet"></a>Запросы из Интернета

Если вы выполняете доступ к учетной записи Azure Cosmos DB с компьютера через Интернет, необходимо добавить IP-адрес или диапазон IP-адресов клиента этого компьютера в список разрешенных IP-адресов для этой учетной записи.

## <a name="configure-an-ip-firewall-by-using-a-resource-manager-template"></a><a id="configure-ip-firewall-arm"></a>Настройка брандмауэра IP-адресов с помощью шаблона Resource Manager

Чтобы настроить контроль доступа к учетной записи Azure Cosmos DB, убедитесь, что в шаблоне диспетчер ресурсов указано свойство **ипрулес** с массивом допустимых диапазонов IP-адресов. При настройке брандмауэра для IP-адресов в уже развернутой учетной записи Cosmos убедитесь, что массив `locations` соответствует ей. Вы не можете одновременно изменять массив `locations` и другие свойства. Дополнительные сведения и примеры шаблонов Azure Resource Manager для Azure Cosmos DB см. в [Azure Resource Manager шаблонах для Azure Cosmos DB](resource-manager-samples.md)

> [!IMPORTANT]
> Свойство **ипрулес** было введено с API версии 2020-04-01. В предыдущих версиях вместо этого было предоставлено свойство **ipRangeFilter** , которое представляет собой список IP-адресов, разделенных запятыми.

В приведенном ниже примере показано, как свойство **ипрулес** предоставляется в API версии 2020-04-01 или более поздней.

```json
{
  "type": "Microsoft.DocumentDB/databaseAccounts",
  "name": "[variables('accountName')]",
  "apiVersion": "2020-04-01",
  "location": "[parameters('location')]",
  "kind": "GlobalDocumentDB",
  "properties": {
    "consistencyPolicy": "[variables('consistencyPolicy')[parameters('defaultConsistencyLevel')]]",
    "locations": "[variables('locations')]",
    "databaseAccountOfferType": "Standard",
    "enableAutomaticFailover": "[parameters('automaticFailover')]",
    "ipRules": [
      {
        "ipAddressOrRange": "40.76.54.131"
      },
      {
        "ipAddressOrRange": "52.176.6.30"
      },
      {
        "ipAddressOrRange": "52.169.50.45"
      },
      {
        "ipAddressOrRange": "52.187.184.26"
      }
    ]
  }
}
```

Ниже приведен тот же пример для любой версии API, более ранней, чем 2020-04-01:

```json
{
  "type": "Microsoft.DocumentDB/databaseAccounts",
  "name": "[variables('accountName')]",
  "apiVersion": "2019-08-01",
  "location": "[parameters('location')]",
  "kind": "GlobalDocumentDB",
  "properties": {
    "consistencyPolicy": "[variables('consistencyPolicy')[parameters('defaultConsistencyLevel')]]",
    "locations": "[variables('locations')]",
    "databaseAccountOfferType": "Standard",
    "enableAutomaticFailover": "[parameters('automaticFailover')]",
    "ipRangeFilter":"40.76.54.131,52.176.6.30,52.169.50.45,52.187.184.26"
  }
}
```

## <a name="configure-an-ip-access-control-policy-by-using-the-azure-cli"></a><a id="configure-ip-firewall-cli"></a>Настройка политики контроля доступа на основе IP-адресов с помощью интерфейса командной строки Azure

Следующая команда демонстрирует, как создать учетную запись Azure Cosmos DB с использованием контроля доступа на основе IP-адресов:

```azurecli-interactive
# Create a Cosmos DB account with default values and IP Firewall enabled
resourceGroupName='MyResourceGroup'
accountName='mycosmosaccount'
ipRangeFilter='192.168.221.17,183.240.196.255,40.76.54.131'

# Make sure there are no spaces in the comma-delimited list of IP addresses or CIDR ranges.
az cosmosdb create \
    -n $accountName \
    -g $resourceGroupName \
    --locations regionName='West US 2' failoverPriority=0 isZoneRedundant=False \
    --locations regionName='East US 2' failoverPriority=1 isZoneRedundant=False \
    --ip-range-filter $ipRangeFilter
```

## <a name="configure-an-ip-access-control-policy-by-using-powershell"></a><a id="configure-ip-firewall-ps"></a>Настройка политики контроля доступа на основе IP-адресов с помощью PowerShell

Следующий сценарий демонстрирует, как создать учетную запись Azure Cosmos DB с использованием контроля доступа на основе IP-адресов:

```azurepowershell-interactive
# Create a Cosmos DB account with default values and IP Firewall enabled
$resourceGroupName = "myResourceGroup"
$accountName = "mycosmosaccount"
$ipRules = @("192.168.221.17","183.240.196.255","40.76.54.131")

$locations = @(
    @{ "locationName"="West US 2"; "failoverPriority"=0; "isZoneRedundant"=False },
    @{ "locationName"="East US 2"; "failoverPriority"=1, "isZoneRedundant"=False }
)

# Make sure there are no spaces in the comma-delimited list of IP addresses or CIDR ranges.
$CosmosDBProperties = @{
    "databaseAccountOfferType"="Standard";
    "locations"=$locations;
    "ipRules"=$ipRules
}

New-AzResource -ResourceType "Microsoft.DocumentDb/databaseAccounts" `
    -ApiVersion "2020-04-01" -ResourceGroupName $resourceGroupName `
    -Name $accountName -PropertyObject $CosmosDBProperties
```

## <a name="troubleshoot-issues-with-an-ip-access-control-policy"></a><a id="troubleshoot-ip-firewall"></a>Устранение неполадок с политикой контроля доступа на основе IP-адресов

Для устранения неполадок с политикой контроля доступа на основе IP-адресов у вас есть следующие возможности и средства:

### <a name="azure-portal"></a>Портал Azure

Включив политику контроля доступа на основе IP-адресов для учетной записи Azure Cosmos DB, вы заблокируете все запросы к этой учетной записи с любых компьютеров, IP-адреса которых не входят в список диапазонов разрешенных IP-адресов. Чтобы разрешить операции плоскости данных на портале (например, просмотр контейнеров и запрашивание документов), следует явным образом разрешить доступ к порталу Azure на панели **Брандмауэр** портала.

### <a name="sdks"></a>Пакеты SDK

Если доступ к ресурсам Azure Cosmos DB осуществляется с помощью пакетов SDK с компьютеров, не включенных в список разрешенных, возвращается общий отклик **403 Запрещено** без дополнительных сведений. Проверьте список разрешенных IP-адресов для учетной записи и убедитесь, что к учетной записи Azure Cosmos DB применяется правильная конфигурация политики.

### <a name="source-ips-in-blocked-requests"></a>IP-адреса источника в заблокированных запросах

Включение журнала ведения диагностики для учетной записи Azure Cosmos DB. Эти журналы отображают все запросы и отклики. Сообщения, имеющие отношение к брандмауэру, помещаются в журнал с кодом ответа 403. Отфильтровав эти сообщения, вы получите список исходных IP-адресов для заблокированных запросов. См. дополнительные сведения о [журнале ведения диагностики Azure Cosmos DB](logging.md).

### <a name="requests-from-a-subnet-with-a-service-endpoint-for-azure-cosmos-db-enabled"></a>Запросы из подсети, в которой включена конечная точка службы для Azure Cosmos DB

Запросы из подсети виртуальной сети, в которой включена конечная точка службы для Azure Cosmos DB, передают в учетную запись Azure Cosmos DB идентификаторы виртуальной сети и подсети. Такие запросы не имеют общедоступного IP-адреса источника, поэтому они отклоняются фильтрами IP-адресов. Чтобы разрешить доступ из определенных подсетей в виртуальных сетях, добавьте список управления доступом, как описано в руководстве по [настройке доступа на основе подсети и виртуальной сети для учетной записи Azure Cosmos DB](how-to-configure-vnet-service-endpoint.md). Применение правил брандмауэра может занять до 15 минут после изменения.

### <a name="private-ip-addresses-in-list-of-allowed-addresses"></a>Частные IP-адреса в списке разрешенных адресов

Создание или обновление учетной записи Azure Cosmos со списком разрешенных адресов, содержащих частные IP-адреса, завершится ошибкой. Убедитесь, что в списке не указан частный IP-адрес.

## <a name="next-steps"></a>Дальнейшие действия

Сведения о том, как настроить конечную точку службы для виртуальной сети для учетной записи Azure Cosmos DB, см. в следующих статьях:

* [Контроль доступа к учетной записи Azure Cosmos DB с использованием виртуальной сети и подсети](vnet-service-endpoint.md)
* [Настройка доступа на основе подсети и виртуальной сети для учетной записи Azure Cosmos DB](how-to-configure-vnet-service-endpoint.md)
