---
title: Управление репликами чтения — Azure CLI, REST API — база данных Azure для MySQL
description: Узнайте, как настроить реплики чтения и управлять ими в базе данных Azure для MySQL с помощью Azure CLI или REST API.
author: ajlam
ms.author: andrela
ms.service: mysql
ms.topic: how-to
ms.date: 6/10/2020
ms.custom: devx-track-azurecli
ms.openlocfilehash: 74e62c39295d36132abdce0abc033162fa22cb64
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2020
ms.locfileid: "91531638"
---
# <a name="how-to-create-and-manage-read-replicas-in-azure-database-for-mysql-using-the-azure-cli-and-rest-api"></a>Как создавать реплики чтения и управлять ими в базе данных Azure для MySQL с помощью Azure CLI и REST API

В этой статье вы узнаете, как создавать реплики чтения и управлять ими в службе "база данных Azure для MySQL" с помощью Azure CLI и REST API. Дополнительные сведения о репликах чтения см. в [этой статье](concepts-read-replicas.md).

## <a name="azure-cli"></a>Azure CLI
Вы можете создавать реплики чтения и управлять ими с помощью Azure CLI.

### <a name="prerequisites"></a>Предварительные требования

- [Установите Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest).
- [Сервер базы данных Azure для MySQL](quickstart-create-mysql-server-database-using-azure-portal.md) , который будет использоваться в качестве исходного сервера. 

> [!IMPORTANT]
> Функция создания реплики чтения доступна только для серверов базы данных Azure для MySQL в ценовой категории "Общее назначение" или "Оптимизированная для операций в памяти". Убедитесь, что исходный сервер находится в одной из этих ценовых категорий.

### <a name="create-a-read-replica"></a>Создание реплики чтения

> [!IMPORTANT]
> При создании реплики для источника, не имеющего существующих реплик, источник сначала перезапускается для подготовки к репликации. Это необходимо учесть, то есть выполнять такие операции в период низкой нагрузки.

Чтобы создать сервер реплики чтения, выполните следующую команду:

```azurecli-interactive
az mysql server replica create --name mydemoreplicaserver --source-server mydemoserver --resource-group myresourcegroup
```

Для команды `az mysql server replica create` обязательны указанные ниже параметры.

| Параметр | Пример значения | Описание  |
| --- | --- | --- |
| resource-group |  myresourcegroup |  Группа ресурсов, в которой будет создан сервер реплики.  |
| name | mydemoreplicaserver | Имя нового сервера реплики, который создается. |
| source-server | mydemoserver | Имя или идентификатор существующего исходного сервера, с которого выполняется репликация. |

Чтобы создать реплику чтения между регионами, используйте `--location` параметр. Приведенный ниже пример интерфейса командной строки создает реплику в западной части США.

```azurecli-interactive
az mysql server replica create --name mydemoreplicaserver --source-server mydemoserver --resource-group myresourcegroup --location westus
```

> [!NOTE]
> Дополнительные сведения о том, в каких регионах можно создать реплику, см. в статье [об основных понятиях реплики чтения](concepts-read-replicas.md). 

> [!NOTE]
> Реплики чтения создаются с той же конфигурацией сервера, что и у главного сервера. Вы можете изменить созданную конфигурацию сервера-реплики. Рекомендуется, чтобы конфигурация сервера реплики хранилась в значении, превышающем значение источника, чтобы реплика могла поддерживать базу данных master.


### <a name="list-replicas-for-a-source-server"></a>Вывод списка реплик для исходного сервера

Чтобы просмотреть все реплики для данного исходного сервера, выполните следующую команду: 

```azurecli-interactive
az mysql server replica list --server-name mydemoserver --resource-group myresourcegroup
```

Для команды `az mysql server replica list` обязательны указанные ниже параметры.

| Параметр | Пример значения | Описание  |
| --- | --- | --- |
| resource-group |  myresourcegroup |  Группа ресурсов, в которой будет создан сервер реплики.  |
| server-name | mydemoserver | Имя или идентификатор исходного сервера. |

### <a name="stop-replication-to-a-replica-server"></a>Остановка репликации на сервер-реплику

> [!IMPORTANT]
> Остановка репликации на сервер является необратимой операцией. После остановки репликации между источником и репликой отменить ее нельзя. Сервер-реплика становится автономным и начинает поддерживает операции чтения и записи. Это сервер нельзя снова преобразовать в реплику.

Репликацию на сервер реплики чтения можно остановить, выполнив следующую команду:

```azurecli-interactive
az mysql server replica stop --name mydemoreplicaserver --resource-group myresourcegroup
```

Для команды `az mysql server replica stop` обязательны указанные ниже параметры.

| Параметр | Пример значения | Описание  |
| --- | --- | --- |
| resource-group |  myresourcegroup |  Группа ресурсов, в которой находится сервер реплики.  |
| name | mydemoreplicaserver | Имя сервера реплики для остановки репликации. |

### <a name="delete-a-replica-server"></a>Удаление сервера-реплики

Сервер реплики чтения можно удалить, выполнив команду **[az mysql server delete](/cli/azure/mysql/server)**.

```azurecli-interactive
az mysql server delete --resource-group myresourcegroup --name mydemoreplicaserver
```

### <a name="delete-a-source-server"></a>Удаление исходного сервера

> [!IMPORTANT]
> Удаление исходного сервера приводит к остановке репликации на все серверы-реплики и удалению самого исходного сервера. Серверы-реплики становятся автономными серверами, которые начинают поддерживать операции чтения и записи.

Чтобы удалить исходный сервер, можно выполнить команду **[AZ MySQL Server DELETE](/cli/azure/mysql/server)** .

```azurecli-interactive
az mysql server delete --resource-group myresourcegroup --name mydemoserver
```


## <a name="rest-api"></a>REST API
Вы можете создавать реплики чтения и управлять ими с помощью [REST API Azure](/rest/api/azure/).

### <a name="create-a-read-replica"></a>Создание реплики чтения
Вы можете создать реплику чтения с помощью [API создания](/rest/api/mysql/servers/create):

```http
PUT https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.DBforMySQL/servers/{replicaName}?api-version=2017-12-01
```

```json
{
  "location": "southeastasia",
  "properties": {
    "createMode": "Replica",
    "sourceServerId": "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.DBforMySQL/servers/{masterServerName}"
  }
}
```

> [!NOTE]
> Дополнительные сведения о том, в каких регионах можно создать реплику, см. в статье [об основных понятиях реплики чтения](concepts-read-replicas.md). 

Если параметр не задан `azure.replication_support` для **реплики** на общего назначения или на исходном сервере, оптимизированном для памяти, и сервер перезагружен, появится сообщение об ошибке. Перед созданием реплики выполните эти два действия.

Реплика создается с использованием тех же параметров вычислений и хранилища, что и у главного сервера. После создания реплики некоторые параметры могут быть изменены независимо от исходного сервера: поколение вычислений, виртуальных ядер, хранилище и срок хранения резервной копии. Изменить также можно ценовую категорию (за исключением уровня "Базовый").


> [!IMPORTANT]
> Перед обновлением параметра сервера источника до нового значения измените значение параметра реплики на равное или большее. Это действие помогает реплике справиться с изменениями, внесенными в главную базу.

### <a name="list-replicas"></a>Список реплик
Список реплик исходного сервера можно просмотреть с помощью [API списка реплик](/rest/api/mysql/replicas/listbyserver).

```http
GET https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.DBforMySQL/servers/{masterServerName}/Replicas?api-version=2017-12-01
```

### <a name="stop-replication-to-a-replica-server"></a>Остановка репликации на сервер-реплику
Вы можете отключить репликацию между исходным сервером и репликой чтения с помощью [API обновления](/rest/api/mysql/servers/update).

После того как репликация на исходный сервер и реплика чтения будет прервана, отменить ее невозможно. Реплика чтения становится отдельным сервером, который поддерживает операции чтения и записи. Это сервер нельзя снова преобразовать в реплику.

```http
PATCH https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.DBforMySQL/servers/{masterServerName}?api-version=2017-12-01
```

```json
{
  "properties": {
    "replicationRole":"None"  
   }
}
```

### <a name="delete-a-source-or-replica-server"></a>Удаление сервера источника или реплики
Чтобы удалить сервер-источник или реплику, используйте [API удаления](/rest/api/mysql/servers/delete):

При удалении исходного сервера репликация на все реплики чтения останавливается. Реплики чтения становятся автономными серверами, которые начинают поддерживать операции чтения и записи.

```http
DELETE https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.DBforMySQL/servers/{serverName}?api-version=2017-12-01
```


## <a name="next-steps"></a>Дальнейшие действия

- Узнайте больше о [репликах чтения](concepts-read-replicas.md)
