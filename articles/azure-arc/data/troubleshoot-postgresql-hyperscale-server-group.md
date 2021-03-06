---
title: Устранение неполадок с PostgreSQL масштабируемыми группами серверов
description: Устранение неполадок при PostgreSQLии групп серверов с помощью Jupyter Notebook
services: azure-arc
ms.service: azure-arc
ms.subservice: azure-arc-data
author: TheJY
ms.author: jeanyd
ms.reviewer: mikeray
ms.date: 09/22/2020
ms.topic: how-to
ms.openlocfilehash: 8e91a611084d201e6609f7e203eaa08c81e19a00
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2020
ms.locfileid: "91569997"
---
# <a name="troubleshooting-postgresql-hyperscale-server-groups"></a>Устранение неполадок PostgreSQL в масштабируемых группах серверов
В этой статье описываются некоторые методы, которые можно использовать для устранения неполадок в группе серверов. В дополнение к этой статье вы можете узнать, как использовать [Kibana](monitor-grafana-kibana.md) для поиска в журналах или использовать [Grafana](monitor-grafana-kibana.md) для визуализации метрик по группе серверов. 

## <a name="getting-more-details-about-the-execution-of-an-azdata-command"></a>Получение дополнительных сведений о выполнении команды аздата
Вы можете добавить параметр **--Debug** в любую выполняемую команду аздата. Это позволит отобразить в консоли дополнительные сведения о выполнении этой команды. Рекомендуется получить сведения, которые помогут понять поведение этой команды.
Например, можно запустить
```console
azdata arc postgres server create -n postgres01 -w 2 --debug
```

или
```console
azdata arc postgres server edit -n postgres01 --extension SomeExtensionName --debug
```

Кроме того, вы можете использовать параметр--Help для любой команды аздата, чтобы отобразить некоторую справку, а также список параметров для конкретной команды. Пример:
```console
azdata arc postgres server create --help
```


## <a name="collecting-logs-of-the-data-controller-and-your-server-groups"></a>Сбор журналов контроллера данных и групп серверов
Прочитайте статью о [получении журналов для служб данных с поддержкой ARC в Azure](troubleshooting-get-logs.md) .



## <a name="interactive-troubleshooting-with-jupyter-notebooks-in-azure-data-studio"></a>Интерактивное устранение неполадок с записными книжками Jupyter в Azure Data Studio
Включив в записные книжки содержимое Markdown для описания необходимых действий и порядка их выполнения, можно документировать в них процедуры. Это содержимое также может предоставлять исполняемый код для автоматизации процедуры.  Такая схема подходит для любых задач: от стандартных операционных процедур до руководств по устранению неполадок.

Например, давайте устраняем неполадки в PostgreSQLной группе серверов, которая может привести к проблемам с Azure Data Studio.

[!INCLUDE [azure-arc-data-preview](../../../includes/azure-arc-data-preview.md)]

### <a name="install-tools"></a>Средства установки

Установите Azure Data Studio `kubectl` и `azdata` на клиентский компьютер, который используется для запуска записной книжки в Azure Data Studio. Для этого следуйте инструкциям в [статье Установка клиентских средств](install-client-tools.md) .

### <a name="update-the-path-environment-variable"></a>Обновление переменной среды PATH

Убедитесь, что эти средства можно вызывать из любого места на этом клиентском компьютере. Например, на клиентском компьютере Windows обновите переменную системной среды PATH и добавьте папку, в которую вы установили kubectl.

### <a name="sign-in-with-azdata"></a>Войти с помощью `azdata`

Войдите в контроллер данных ARC с этого клиентского компьютера и перед запуском Azure Data Studio. Для этого выполните следующую команду:

```console
azdata login --endpoint https://<IP address>:<port>
```

Замените `<IP address>` IP-адресом кластера Kubernetes и `<port>` портом, на котором Kubernetes прослушивается. Вам будет предложено ввести имя пользователя и пароль. Чтобы просмотреть дополнительные сведения, выполните команду: _.

```console
azdata login --help
```

### <a name="log-into-your-kubernetes-cluster-with-kubectl"></a>Войдите в кластер Kubernetes с помощью kubectl.

Для этого может потребоваться использовать примеры команд, приведенные в [этой](https://blog.christianposta.com/kubernetes/logging-into-a-kubernetes-cluster-with-kubectl/) записи блога.
Выполните такие команды, как:

```console
kubectl config view
kubectl config set-credentials kubeuser/my_kubeuser --username=<your Arc Data Controller Admin user name> --password=<password>
kubectl config set-cluster my_kubeuser --server=https://<IP address>:<port>
kubectl config set-context default/my_kubeuser/ArcDataControllerAdmin --user=ArcDataControllerAdmin/my_kubeuser --namespace=arc --cluster=my_kubeuser
kubectl config use-context default/my_kubeuser/ArcDataControllerAdmin
```

#### <a name="the-troubleshooting-notebook"></a>Записная книжка для устранения неполадок

Запустите Azure Data Studio и откройте записную книжку устранение неполадок. 

Реализуйте шаги, описанные в разделе  [033-Manage-postgres-with-AzureDataStudio.md](manage-postgresql-hyperscale-server-group-with-azure-data-studio.md) to:

1. Подключение к контроллеру данных Arc
2. Щелкните правой кнопкой мыши экземпляр postgres и выберите **[Управление]** .
3. Выберите **панель мониторинга [диагностика и устранение проблем]**
4. Выберите **ссылку [Устранение неполадок]** .

:::image type="content" source="media/postgres-hyperscale/ads-controller-postgres-troubleshooting-notebook.jpg" alt-text="Azure Data Studio — Открытие записной книжки для устранения неполадок PostgreSQL&quot;":::

Откроется **Записная книжка &quot;TSG100&quot; в службе &quot;Дуга Azure с поддержкой PostgreSQL** " Azure Data Studio. Использование записной книжки для устранения :::image type="content" source="media/postgres-hyperscale/ads-controller-postgres-troubleshooting-notebook2.jpg" alt-text="Azure Data Studio — Открытие записной книжки для устранения неполадок PostgreSQL&quot;":::

Откроется **Записная книжка &quot;TSG100&quot; в службе &quot;Дуга Azure с поддержкой PostgreSQL** ":::

#### <a name="run-the-scripts"></a>Выполнение сценариев
Нажмите кнопку "запустить все" в верхней части, чтобы запустить записную книжку одновременно, или пошаговое выполнение каждой ячейки кода по одной.

Просмотр результатов выполнения ячеек кода для любых потенциальных проблем.

Мы добавим дополнительные сведения в записную книжку, чтобы узнать, как распознать распространенные проблемы и как их решить.

## <a name="next-step"></a>Следующий шаг
- Ознакомьтесь со сведениями о [получении журналов для служб данных с поддержкой ARC в Azure](troubleshooting-get-logs.md) .
- Прочитайте о [поиске журналов с помощью Kibana](monitor-grafana-kibana.md)
- Сведения о [мониторинге с помощью Grafana](monitor-grafana-kibana.md)
- Создание собственных записных книжек
