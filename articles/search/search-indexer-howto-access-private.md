---
title: Подключение индексатора через закрытую конечную точку
titleSuffix: Azure Cognitive Search
description: Настройте подключения индексатора для доступа к содержимому других ресурсов Azure, защищенных с помощью частной конечной точки.
manager: nitinme
author: arv100kri
ms.author: arjagann
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 10/14/2020
ms.openlocfilehash: b81d3f74c20f42620ceeae08bec5d484909377a7
ms.sourcegitcommit: 419c8c8061c0ff6dc12c66ad6eda1b266d2f40bd
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/18/2020
ms.locfileid: "92167480"
---
# <a name="indexer-connections-through-a-private-endpoint-azure-cognitive-search"></a>Подключение индексатора через частную конечную точку (Azure Когнитивный поиск)

Многие ресурсы Azure (например, учетные записи хранения Azure) могут быть настроены для приема подключений из определенного списка виртуальных сетей и отказа от подключений, исходящих из общедоступной сети. Если вы используете индексатор для индексирования данных в Когнитивный поиск Azure, а источник данных находится в частной сети, можно создать (исходящее) [Подключение к частной конечной точке](../private-link/private-endpoint-overview.md) для достижения данных.

Чтобы использовать этот метод соединения индексатора, необходимо выполнить два требования.

+ Ресурс Azure, предоставляющий содержимое или код, должен быть предварительно зарегистрирован в [службе частной связи Azure](https://azure.microsoft.com/services/private-link/).

+ Служба Когнитивный поиск Azure должна быть базовой или более поздней (недоступна на уровне Free). Кроме того, если у индексатора есть набор навыков, уровень должен быть Standard 2 (S2) или выше. Дополнительные сведения см. в разделе [ограничения службы](search-limits-quotas-capacity.md#shared-private-link-resource-limits).

## <a name="shared-private-link-resources-management-apis"></a>Общие интерфейсы API управления ресурсами частной связи

Частные конечные точки защищенных ресурсов, созданные с помощью Azure Когнитивный поиск API, называются *общими ресурсами частной связи* , так как вы предоставляете общий доступ к ресурсу (например, учетной записи хранения), который был подключен к [службе частной связи Azure](https://azure.microsoft.com/services/private-link/).

С помощью REST API управления Azure Когнитивный поиск предоставляет операцию [CreateOrUpdate](/rest/api/searchmanagement/sharedprivatelinkresources/createorupdate) , которую можно использовать для настройки доступа из индексатора когнитивный Поиск Azure.

Подключения к частным конечным точкам для некоторых ресурсов можно создать только с предварительной версией API управления поиском ( `2020-08-01-Preview` или более поздней версии), обозначенным тегом "Preview" в таблице ниже. Ресурсы без тега "Предварительная версия" можно создавать с помощью предварительной версии или общедоступной версией API ( `2020-08-01` или более поздней).

Ниже приведен список ресурсов Azure, для которых можно создать исходящие частные конечные точки из Когнитивный поиск Azure. `groupId`Значения, перечисленные в таблице ниже, должны использоваться в точности как записанные (с учетом регистра) в API для создания общего ресурса частной ссылки.

| Ресурс Azure | Идентификатор группы. |
| --- | --- |
| Хранилище Azure — BLOB-объект (или) ADLS Gen 2 | `blob`|
| Службы хранилища Azure — таблицы | `table`|
| API Azure Cosmos DB-SQL | `Sql`|
| База данных SQL Azure | `sqlServer`|
| База данных Azure для MySQL (Предварительная версия) | `mysqlServer`|
| Azure Key Vault | `vault` |
| Функции Azure (Предварительная версия) | `sites` |

Список ресурсов Azure, для которых поддерживаются исходящие подключения к частной конечной точке, также можно запросить с помощью [API List Supported](/rest/api/searchmanagement/privatelinkresources/listsupported).

В оставшейся части этой статьи для демонстрации REST APIных вызовов используется сочетание [ARMClient](https://github.com/projectkudu/ARMClient) и [POST](https://www.postman.com/) .

> [!NOTE]
> В этой статье предполагается, что имя службы поиска — __contoso-Search__ , существующий в группе ресурсов __contoso__ подписки с идентификатором __00000000-0000-0000-0000-000000000000__. Идентификатор ресурса этой службы поиска будет `/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/contoso/providers/Microsoft.Search/searchServices/contoso-search`

В остальных примерах будет показано, как можно настроить службу __поиска Contoso__ , чтобы ее индексаторы могли получать доступ к данным из защищенной учетной записи хранения. `/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/contoso/providers/Microsoft.Storage/storageAccounts/contoso-storage`

## <a name="securing-your-storage-account"></a>Защита учетной записи хранения

Настройте учетную запись хранения, [разрешающую доступ только из конкретных подсетей](../storage/common/storage-network-security.md#grant-access-from-a-virtual-network). Если установить этот флажок в портал Azure и оставить набор пустым, это означает, что трафик из любой виртуальной сети не разрешен.

   ![Доступ к виртуальной сети](media\search-indexer-howto-secure-access\storage-firewall-noaccess.png "Доступ к виртуальной сети")

> [!NOTE]
> [Подход с доверенной службой Майкрософт](../storage/common/storage-network-security.md#trusted-microsoft-services) можно использовать для обхода ограничений виртуальной сети или IP-адресов в такой учетной записи хранения и может предоставить службе поиска доступ к данным в учетной записи хранения, как описано в статье [доступ индексатора к службе хранилища Azure с помощью исключения доверенной службы ](search-indexer-howto-access-trusted-service-exception.md). Однако при использовании этого подхода обмен данными между Azure Когнитивный поиск и учетная запись хранения осуществляется через общедоступный IP-адрес учетной записи хранения через безопасную магистральную сеть Майкрософт.

## <a name="step-1-create-a-shared-private-link-resource-to-the-storage-account"></a>Шаг 1. Создание общего ресурса частной ссылки для учетной записи хранения

Выполните следующий вызов API, чтобы запросить Когнитивный поиск Azure, чтобы создать исходящее подключение частной конечной точки к учетной записи хранения.

`armclient PUT https://management.azure.com/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/contoso/providers/Microsoft.Search/searchServices/contoso-search/sharedPrivateLinkResources/blob-pe?api-version=2020-08-01 create-pe.json`

Содержимое `create-pe.json` файла (который представляет текст запроса для API) выглядит следующим образом:

```json
{
      "name": "blob-pe",
      "properties": {
        "privateLinkResourceId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/contoso/providers/Microsoft.Storage/storageAccounts/contoso-storage",
        "groupId": "blob",
        "requestMessage": "please approve"
      }
}
```

`202 Accepted`Ответ возвращается при успешном выполнении. процесс создания исходящей частной конечной точки — это длительная (асинхронная) операция. Включает развертывание следующих ресурсов.

1. Частная конечная точка, выделенная с частным IP-адресом в `"Pending"` состоянии. Частный IP-адрес получается из адресного пространства, выделенного виртуальной сети для среды выполнения частного индексатора, предназначенного для службы поиска. После утверждения частной конечной точки любое взаимодействие Когнитивный поиск Azure с учетной записью хранения происходит от частного IP-адреса и защищенного канала частной связи.
2. Частная зона DNS для типа ресурса на основе `groupId` . Это обеспечит, чтобы все DNS-запросы к частному ресурсу использовали IP-адрес, связанный с частной конечной точкой.

Обязательно укажите правильный `groupId` тип ресурса, для которого создается частная конечная точка. Любое несоответствие приведет к неудачному ответному сообщению.

Как и все асинхронные операции Azure, `PUT` вызов возвращает `Azure-AsyncOperation` значение заголовка, которое будет выглядеть следующим образом:

`"Azure-AsyncOperation": "https://management.azure.com/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/contoso/providers/Microsoft.Search/searchServices/contoso-search/sharedPrivateLinkResources/blob-pe/operationStatuses/08586060559526078782?api-version=2020-08-01"`

Этот URI можно периодически опрашивать для получения состояния операции. Перед продолжением рекомендуется подождать, пока состояние операции ресурса общей частной ссылки не достигнет состояния терминала (то есть `succeeded` ).

`armclient GET https://management.azure.com/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/contoso/providers/Microsoft.Search/searchServices/contoso-search/sharedPrivateLinkResources/blob-pe/operationStatuses/08586060559526078782?api-version=2020-08-01"`

```json
{
    "status": "running" | "succeeded" | "failed"
}
```

## <a name="step-2a-approve-the-private-endpoint-connection-for-the-storage-account"></a>Шаг 2A. Утверждение подключения к частной конечной точке для учетной записи хранения

> [!NOTE]
> В этом разделе используется портал Azure для прохода по потоку утверждения частной конечной точки в хранилище. Вместо этого можно также использовать [REST API](/rest/api/storagerp/privateendpointconnections) , доступные через поставщик ресурсов хранилища (RP).
>
> Другие поставщики, такие как CosmosDB, Azure SQL Server и т. д., также предлагают аналогичные API-интерфейсы RP для управления подключениями к частным конечным точкам.

Перейдите на вкладку "**подключения к частным конечным точкам**" учетной записи хранения на портал Azure. Должен быть запрос на подключение к частной конечной точке с сообщением запроса от предыдущего вызова API (после __успешной__асинхронной операции).

   ![Утверждение закрытой конечной точки](media\search-indexer-howto-secure-access\storage-privateendpoint-approval.png "Утверждение закрытой конечной точки")

Выберите закрытую конечную точку, созданную Когнитивный поиск Azure (используйте столбец "Частная конечная точка", чтобы определить подключение к частной конечной точке по имени, указанному в предыдущем API), и выберите "утвердить" с соответствующим сообщением (сообщение не имеет значения). Убедитесь, что подключение к частной конечной точке выглядит следующим образом (оно может находиться в диапазоне от 1-2 минут до обновления состояния на портале).

![Закрытая конечная точка утверждена](media\search-indexer-howto-secure-access\storage-privateendpoint-after-approval.png "Закрытая конечная точка утверждена")

После утверждения запроса на подключение частной конечной точки это означает, что трафик *может* проходить через закрытую конечную точку. После утверждения частной конечной точки Когнитивный поиск Azure создаст необходимые сопоставления зон DNS в зоне DNS, созданной для нее.

## <a name="step-2b-query-the-status-of-the-shared-private-link-resource"></a>Шаг 2b. запрос состояния общего ресурса частной ссылки

 Чтобы убедиться, что общий ресурс частной ссылки был обновлен после утверждения, получите его состояние с помощью [API Get](/rest/api/searchmanagement/sharedprivatelinkresources/get).

`armclient GET https://management.azure.com/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/contoso/providers/Microsoft.Search/searchServices/contoso-search/sharedPrivateLinkResources/blob-pe?api-version=2020-08-01`

Если `properties.provisioningState` ресурс имеет значение `Succeeded` `properties.status` , а —, то это `Approved` означает, что общий ресурс частной связи функционирует и Индексаторы могут быть настроены для взаимодействия с закрытой конечной точкой.

```json
{
      "name": "blob-pe",
      "properties": {
        "privateLinkResourceId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/contoso/providers/Microsoft.Storage/storageAccounts/contoso-storage",
        "groupId": "blob",
        "requestMessage": "please approve",
        "status": "Approved",
        "resourceRegion": null,
        "provisioningState": "Succeeded"
      }
}

```

## <a name="step-3-configure-indexer-to-run-in-the-private-environment"></a>Шаг 3. Настройка индексатора для запуска в частной среде

> [!NOTE]
> Этот шаг можно выполнить даже перед утверждением частного подключения к конечной точке. Пока подключение к частной конечной точке не будет утверждено, любой индексатор, который пытается связаться с защищенным ресурсом (например, учетной записью хранения), завершится как временное состояние сбоя. Новые индексаторы создаваться не будут. Как только подключение к частной конечной точке будет утверждено, индексаторы смогут получить доступ к частной учетной записи хранения.

1. [Создайте источник данных](/rest/api/searchservice/create-data-source) , указывающий на защищенную учетную запись хранения и соответствующий контейнер в учетной записи хранения. Ниже показан этот запрос в POST.
![Создание источника данных](media\search-indexer-howto-secure-access\create-ds.png "Создание источника данных")

1. Аналогичным образом [создайте индекс](/rest/api/searchservice/create-index) и при необходимости [Создайте набор навыков](/rest/api/searchservice/create-skillset) с помощью REST API.

1. [Создайте индексатор](/rest/api/searchservice/create-indexer) , указывающий на источник данных, индекс и набор навыков, созданный выше. Кроме того, выполните принудительное выполнение индексатора в закрытой среде выполнения, задав для свойства конфигурации индексатора значение `executionEnvironment` `"Private"` .
![Создание индексатора](media\search-indexer-howto-secure-access\create-idr.png "Создание индексатора")

Работа индексатора должна быть успешно создана, и следует сделать так, чтобы индексирование содержимого из учетной записи хранения выполнялось через подключение к частной конечной точке. Состояние индексатора можно отслеживать с помощью [API состояния индексатора](/rest/api/searchservice/get-indexer-status).

> [!NOTE]
> Если у вас уже есть индексаторы, их можно просто обновить с помощью [API размещения](/rest/api/searchservice/create-indexer) , чтобы задать `executionEnvironment` для значение `"Private"` .

## <a name="troubleshooting-issues"></a>Устранение неполадок

- При создании индексатора, если создание завершается ошибкой с сообщением об ошибке "учетные данные источника данных недопустимы", это означает, что подключение к частной конечной точке не было *утверждено* или не работает.
Получите состояние общего ресурса частной ссылки с помощью [API Get](/rest/api/searchmanagement/sharedprivatelinkresources/get). Если *утверждение утверждено* , проверьте `properties.provisioningState` ресурс. Если это так `Incomplete` , это означает, что некоторые из базовых зависимостей для ресурса не удалось подготавливать `PUT` . Повторите запрос на повторное создание общего ресурса частной ссылки, который должен устранить проблему. Может потребоваться повторное утверждение. Проверьте состояние ресурса еще раз для проверки.
- Если индексатор создается без задания его значения `executionEnvironment` , создание индексатора может быть успешным, но в журнале выполнения будет показано, что работа индексатора завершается неудачно. Необходимо [Обновить индексатор](/rest/api/searchservice/update-indexer) , указав среду выполнения.
- Если индексатор создается без настройки `executionEnvironment` и выполняется успешно, это означает, что когнитивный Поиск Azure решила, что среда выполнения — это частная среда, соответствующая службе поиска. Однако это может измениться в зависимости от ряда факторов (ресурсов, потребляемых индексатором, нагрузки на службу поиска и т. д.) и может завершиться сбоем позже. Мы настоятельно рекомендуем установить значение `executionEnvironment` как, `"Private"` чтобы гарантировать, что в будущем оно не будет работать.
- [Квоты и ограничения](search-limits-quotas-capacity.md) определяют, сколько общих ресурсов частной ссылки могут быть созданы и зависят от номера SKU службы поиска.

## <a name="next-steps"></a>Дальнейшие действия

Дополнительные сведения о частных конечных точках:

- [Что такое частные конечные точки?](../private-link/private-endpoint-overview.md)
- [Конфигурации DNS, необходимые для частных конечных точек](../private-link/private-endpoint-dns.md)