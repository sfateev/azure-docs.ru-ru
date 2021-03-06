---
title: Оповещения метрик от Azure Monitor для контейнеров
description: В этой статье рассматриваются рекомендуемые оповещения метрик, доступные в Azure Monitor для контейнеров в общедоступной предварительной версии.
ms.topic: conceptual
ms.date: 09/24/2020
ms.openlocfilehash: 83394faf3d7296522151b815bddd910d47e45d24
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2020
ms.locfileid: "91619956"
---
# <a name="recommended-metric-alerts-preview-from-azure-monitor-for-containers"></a>Рекомендуемые оповещения метрик (Предварительная версия) из Azure Monitor для контейнеров

Чтобы предупредить о проблемах с системными ресурсами, возникающих при пиковом спросе и выполнении практических мощностей, при Azure Monitor для контейнеров необходимо создать оповещение журнала на основе данных о производительности, хранящихся в журналах Azure Monitor. Azure Monitor для контейнеров теперь включает в себя предварительно настроенные правила генерации оповещений метрик для AKS и кластера Kubernetes с поддержкой дуги Azure, который находится в общедоступной предварительной версии.

В этой статье рассматриваются принципы работы и даются рекомендации по настройке этих правил генерации оповещений и управлению ими.

Если вы не знакомы с Azure Monitor оповещениями, ознакомьтесь со статьей [Обзор оповещений в Microsoft Azure](../platform/alerts-overview.md) перед началом работы. Дополнительные сведения об оповещениях метрик см. [в разделе оповещения метрик в Azure Monitor](../platform/alerts-metric-overview.md).

## <a name="prerequisites"></a>Предварительные требования

Прежде чем начать, подтвердите следующее.

* Пользовательские метрики доступны только в подмножестве регионов Azure. Список поддерживаемых регионов приведен в [поддерживаемых регионах](../platform/metrics-custom-overview.md#supported-regions).

* Для поддержки оповещений метрик и появления дополнительных метрик требуется минимальная версия агента — **Microsoft/OMS: ciprod05262020** for AKS and **Microsoft/OMS: Ciprod09252020** для кластера Kubernetes с поддержкой дуги Azure.

    Чтобы убедиться, что кластер работает под управлением новой версии агента, можно выполнить одно из следующих действий.

    * Выполните команду: `kubectl describe <omsagent-pod-name> --namespace=kube-system` . В возвращенном состоянии Обратите внимание на значение в разделе **Image** for omsagent в разделе *Containers (контейнеры* ) выходных данных. 
    * На вкладке **узлы** выберите узел кластера и на панели **свойства** справа запишите значение в поле **тег образа агента**.

    Значение, отображаемое для AKS, должно иметь версию **ciprod05262020** или более позднюю. Значение, отображаемое для кластера Kubernetes с включенной службой Arc Azure, должно иметь версию **ciprod09252020** или более позднюю. Если у кластера более старая версия, см. [инструкции по обновлению Azure Monitor для агента контейнеров](container-insights-manage-agent.md#upgrade-agent-on-aks-cluster) , чтобы получить последнюю версию.

    Дополнительные сведения о выпуске агента см. в разделе [Журнал выпусков агентов](https://github.com/microsoft/docker-provider/tree/ci_feature_prod). Чтобы проверить сбор метрик, можно использовать Azure Monitor обозреватель метрик и проверить из **пространства имен метрик** **, которое отображается** в списке. Если это так, можно приступить к настройке оповещений. Если собранные метрики не отображаются, субъекту-службе кластера или MSI-файла не хватает необходимых разрешений. Чтобы проверить, является ли SPN или MSI членом роли **издателя метрики мониторинга** , выполните действия, описанные в разделе [Обновление для каждого кластера с помощью Azure CLI](container-insights-update-metrics.md#upgrade-per-cluster-using-azure-cli) , чтобы подтвердить и задать назначение ролей.

## <a name="alert-rules-overview"></a>Общие сведения о правилах оповещений

Чтобы предупредить о важности, Azure Monitor для контейнеров содержит следующие оповещения метрики для кластеров Kubernetes с поддержкой AKS и дуги Azure.

|Имя| Описание |Пороговое значение по умолчанию |
|----|-------------|------------------|
|Среднее время ЦП контейнера (%) |Вычисляет среднее использование ЦП на контейнер.|Если средняя загрузка ЦП на контейнер превышает 95%.| 
|Средний объем памяти рабочего множества контейнера (в%) |Вычисляет среднее используемую память рабочего множества для каждого контейнера.|Если среднее использование памяти рабочего набора на контейнер превышает 95%. |
|средняя загрузка ЦП (%); |Вычисляет среднее время ЦП, используемое на каждом узле. |Если средняя загрузка ЦП узла превышает 80% |
|Среднее время использования диска,% |Вычисляет среднее использование дискового пространства для узла.|Когда использование диска для узла превышает 80%. |
|Средний объем памяти рабочего множества (%) |Вычисляет среднее значение памяти рабочего множества для узла. |Если средний объем памяти рабочего множества для узла превышает 80%. |
|Перезапуск счетчика контейнеров |Вычисляет количество перезапускаемых контейнеров. | При перезапуске контейнера больше 0. |
|Счетчики Pod со сбоем |Вычисляет, если какой либо модуль Pod находится в неисправном состоянии.|Если число модулей Pod в состоянии Failed больше 0. |
|Состояние NotReady узла |Вычисляет, находится ли узел в состоянии NotReady.|Если число узлов в состоянии NotReady больше 0. |
|Контейнеры с нехваткой памяти |Вычисляет число незавершенных контейнеров. |Когда число незавершенных контейнеров больше 0. |
|Готовность к использованию модулей |Вычисляет среднее состояние готовности для модулей Pod. |При готовности к состоянию модулей Pod менее 80%.|
|Завершенное число заданий |Вычисляет число заданий, выполненных более шести часов назад. |Если число устаревших заданий старше шести часов больше 0.|

Существуют общие свойства для всех этих правил генерации оповещений:

* Все правила генерации оповещений основаны на метриках.

* По умолчанию все правила оповещений отключены.

* Все правила генерации оповещений оцениваются один раз в минуту и возвращаются за последние 5 минут данных.

* По умолчанию для правил оповещений не назначена группа действий. [Группу действий](../platform/action-groups.md) можно добавить к предупреждению, выбрав существующую группу действий или создав новую группу действий при изменении правила генерации оповещений.

* Вы можете изменить пороговое значение правил генерации оповещений, непосредственно отредактировав их. Однако перед изменением порогового значения обратитесь к руководству, указанному в каждом правиле генерации оповещений.

Следующие метрики на основе оповещений имеют уникальные характеристики поведения по сравнению с другими метриками.

* метрика *комплетеджобскаунт* отправляется только при наличии заданий, завершенных более шести часов назад.

* метрика *контаинеррестарткаунт* отправляется только при перезапуске контейнеров.

* метрика *умкилледконтаинеркаунт* отправляется, только если есть незавершенные контейнеры.

* метрики *кпуексцеедедперцентаже*, *меморирссексцеедедперцентаже*и *мемориворкингсетексцеедедперцентаже* отправляются, когда значения ЦП, RSS-канала памяти и рабочего набора памяти превышают заданное пороговое значение (пороговое значение по умолчанию — 95%). Эти пороговые значения являются эксклюзивными для порогового значения условия предупреждения, указанного для соответствующего правила генерации оповещений. Это означает, что если вы хотите получать эти метрики и анализировать их из [обозревателя метрик](../platform/metrics-getting-started.md), рекомендуется настроить пороговое значение ниже порогового значения предупреждения. Конфигурация, связанная с параметрами коллекции для их пороговых значений использования ресурсов контейнера, может быть переопределена в файле Конфигмапс в разделе `[alertable_metrics_configuration_settings.container_resource_utilization_thresholds]` . Дополнительные сведения о настройке файла конфигурации ConfigMap см. в разделе [Настройка конфигмапсных метрик](#configure-alertable-metrics-in-configmaps) .

## <a name="metrics-collected"></a>Собранные метрики

Следующие метрики включаются и собираются, если не указано иное, как часть этой функции:

|Пространство имен метрик |Метрика |Описание |
|---------|----|------------|
|Аналитика. контейнеры и узлы |кпуусажемилликорес |Загрузка ЦП в миллиардах по узлу.|
|Аналитика. контейнеры и узлы |кпуусажеперцентаже |Процент использования ЦП по узлам.|
|Аналитика. контейнеры и узлы |меморирссбитес |Использование RSS памяти в байтах узлом.|
|Аналитика. контейнеры и узлы |меморирссперцентаже |Процент использования RSS в памяти узлом.|
|Аналитика. контейнеры и узлы |мемориворкингсетбитес |Использование рабочего набора памяти в байтах узлом.|
|Аналитика. контейнеры и узлы |мемориворкингсетперцентаже |Процент использования рабочего набора памяти узлом.|
|Аналитика. контейнеры и узлы |нодескаунт |Число узлов по состоянию.|
|Аналитика. контейнеры и узлы |дискуседперцентаже |Процент дискового пространства, используемого на узле устройством.|
|Аналитика. контейнеры и модули |подкаунт |Число модулей Pod по контроллеру, пространству имен, узлу и этапу.|
|Аналитика. контейнеры и модули |комплетеджобскаунт |Завершенные задания подсчитывает значение более старого настраиваемого порога (по умолчанию — шесть часов) по пространству имен Kubernetes. |
|Аналитика. контейнеры и модули |рестартингконтаинеркаунт |Число перезапусков контейнера контроллером, пространством имен Kubernetes.|
|Аналитика. контейнеры и модули |умкилледконтаинеркаунт |Число контейнеров Умкиллед по контроллеру, пространство имен Kubernetes.|
|Аналитика. контейнеры и модули |подреадиперцентаже |Процентное отношение модулей Pod в состоянии готовности к пространству имен контроллера, Kubernetes.|
|Аналитика. контейнеры и контейнеры |кпуексцеедедперцентаже |Процент использования ЦП для контейнеров, превышающий настраиваемый пользователем порог (по умолчанию — 95,0) по имени контейнера, имени контроллера, пространству имен Kubernetes, имени Pod.<br> Суммировать  |
|Аналитика. контейнеры и контейнеры |меморирссексцеедедперцентаже |Процент RSS-канала памяти для контейнеров, превышающий настраиваемый пользователем порог (по умолчанию — 95,0) по имени контейнера, имени контроллера, пространству имен Kubernetes, имени Pod.|
|Аналитика. контейнеры и контейнеры |мемориворкингсетексцеедедперцентаже |Объем рабочего набора памяти в процентах для контейнеров превышает пороговое значение, настраиваемое пользователем (по умолчанию — 95,0) по имени контейнера, имени контроллера, пространству имен Kubernetes, имени Pod.|

## <a name="enable-alert-rules"></a>Включение правил генерации оповещений

Выполните следующие действия, чтобы включить оповещения метрик в Azure Monitor из портал Azure. Чтобы включить использование шаблона диспетчер ресурсов, см. статью [Включение с помощью шаблона диспетчер ресурсов](#enable-with-a-resource-manager-template).

### <a name="from-the-azure-portal"></a>на портале Azure;

В этом разделе описывается включение Azure Monitor для оповещения метрик контейнеров (Предварительная версия) из портал Azure.

1. Войдите на [портал Azure](https://portal.azure.com/).

2. Доступ к функции оповещения о метриках Azure Monitor для контейнеров (Предварительная версия) доступен непосредственно из кластера AKS. для этого выберите **аналитика** на левой панели в портал Azure.

3. На панели команд выберите **Рекомендуемые оповещения**.

    ![Параметр Рекомендуемые оповещения в Azure Monitor для контейнеров](./media/container-insights-metric-alerts/command-bar-recommended-alerts.png)

4. Панель свойств **Рекомендуемые предупреждения** автоматически отображается в правой части страницы. По умолчанию все правила генерации оповещений в списке отключены. После выбора параметра **включить**создается правило генерации оповещений, а имя правила обновляется для включения ссылки на ресурс предупреждения.

    ![Панель свойств рекомендуемых предупреждений](./media/container-insights-metric-alerts/recommended-alerts-pane.png)

    После выбора переключателя **включить или отключить** для включения предупреждения создается правило генерации оповещений, а имя правила обновляется для включения ссылки на фактический ресурс предупреждения.

    ![Включение правила генерации оповещений](./media/container-insights-metric-alerts/recommended-alerts-pane-enable.png)

5. Правила генерации оповещений не связаны с [группой действий](../platform/action-groups.md) для уведомления пользователей о том, что оповещение активировано. Выберите **нет назначенной группы действий** и на странице **группы действий** укажите существующую или создайте группу действий, выбрав **добавить** или **создать**.

    ![Выбор группы действий](./media/container-insights-metric-alerts/select-action-group.png)

### <a name="enable-with-a-resource-manager-template"></a>Включение с помощью шаблона диспетчер ресурсов

Чтобы создать включаемые оповещения метрик в Azure Monitor, можно использовать шаблон Azure Resource Manager и файл параметров.

Основными шагами являются следующие:

1. Скачайте один или все доступные шаблоны, которые описывают, как создать оповещение из [GitHub](https://github.com/microsoft/Docker-Provider/tree/ci_dev/alerts/recommended_alerts_ARM).

2. Создайте и используйте [файл параметров](../../azure-resource-manager/templates/parameter-files.md) в виде JSON, чтобы задать значения, необходимые для создания правила генерации оповещений.

3. Разверните шаблон из портал Azure, PowerShell или Azure CLI.

#### <a name="deploy-through-azure-portal"></a>Развертывание с помощью портал Azure

1. Скачайте и сохраните в локальной папке, шаблоне Azure Resource Manager и файле параметров, чтобы создать правило генерации оповещений с помощью следующих команд:

2. Чтобы развернуть настраиваемый шаблон на портале, выберите **создать ресурс** в [портал Azure](https://portal.azure.com).

3. Найдите **шаблон**и выберите **шаблоны развертывания**.

4. Нажмите кнопку **создания**.

5. Вы увидите несколько вариантов создания шаблона, выберите **создать собственный шаблон в редакторе**.

6. На **странице Изменение шаблона**выберите **загрузить файл** , а затем выберите файл шаблона.

7. На странице **изменение шаблона** нажмите кнопку **сохранить**.

8. На странице **Настраиваемое развертывание** укажите следующее, а затем после завершения выберите **купить** , чтобы развернуть шаблон и создать правило генерации оповещений.

    * Группа ресурсов
    * Расположение
    * Имя предупреждения
    * Идентификатор ресурса кластера

#### <a name="deploy-with-azure-powershell-or-cli"></a>Развертывание с помощью Azure PowerShell или CLI

1. Скачайте и сохраните в локальной папке, шаблоне Azure Resource Manager и файле параметров, чтобы создать правило генерации оповещений с помощью следующих команд:

2. Вы можете создать оповещение метрики с помощью шаблона и файла параметров, используя PowerShell или Azure CLI.

    Использование Azure PowerShell

    ```powershell
    Connect-AzAccount

    Select-AzSubscription -SubscriptionName <yourSubscriptionName>
    New-AzResourceGroupDeployment -Name CIMetricAlertDeployment -ResourceGroupName ResourceGroupofTargetResource `
    -TemplateFile templateFilename.json -TemplateParameterFile templateParameterFilename.parameters.json
    ```

    Использование Azure CLI

    ```azurecli
    az login

    az group deployment create \
    --name AlertDeployment \
    --resource-group ResourceGroupofTargetResource \
    --template-file templateFileName.json \
    --parameters @templateParameterFilename.parameters.json
    ```

    >[!NOTE]
    >Хотя оповещение метрики можно создать в группе ресурсов, отличной от целевого ресурса, рекомендуется использовать ту же группу ресурсов, что и целевой ресурс.

## <a name="edit-alert-rules"></a>Изменение правил генерации оповещений

Вы можете просматривать и управлять Azure Monitor для правил оповещений контейнеров, изменять их пороговое значение или настраивать [группу действий](../platform/action-groups.md) для кластера AKS. Хотя эти действия можно выполнять из портал Azure и Azure CLI, их также можно выполнять непосредственно из кластера AKS в Azure Monitor для контейнеров.

1. На панели команд выберите **Рекомендуемые оповещения**.

2. Чтобы изменить пороговое значение, на панели **Рекомендуемые предупреждения** выберите включенное оповещение. В **правиле изменения**выберите **критерии оповещения** , которые необходимо изменить.

    * Чтобы изменить пороговое значение правила оповещения, выберите **условие**.
    * Чтобы указать существующую или создать группу действий, выберите **Добавить** или **создать** в группе **действий** .

Чтобы просмотреть предупреждения, созданные для включенных правил, в области **Рекомендуемые оповещения** выберите **представление в оповещениях**. Вы будете перенаправлены в меню предупреждений для кластера AKS, где можно просмотреть все оповещения, созданные в данный момент для кластера.

## <a name="configure-alertable-metrics-in-configmaps"></a>Настройка метрик с оповещением в Конфигмапс

Выполните следующие действия, чтобы настроить файл конфигурации ConfigMap, чтобы переопределить пороговые значения использования ресурсов контейнера по умолчанию. Эти действия применимы только к следующим показателям, которые могут получать оповещения.

* *кпуексцеедедперцентаже*
* *меморирссексцеедедперцентаже*
* *мемориворкингсетексцеедедперцентаже*

1. Измените файл ConfigMap YAML в разделе `[alertable_metrics_configuration_settings.container_resource_utilization_thresholds]` .

2. Чтобы изменить пороговое значение *кпуексцеедедперцентаже* на 90% и начать сбор этой метрики при достижении и превышении порогового значения, настройте файл ConfigMap, используя следующий пример.

    ```
    container_cpu_threshold_percentage = 90.0
    # Threshold for container memoryRss, metric will be sent only when memory rss exceeds or becomes equal to the following percentage
    container_memory_rss_threshold_percentage = 95.0
    # Threshold for container memoryWorkingSet, metric will be sent only when memory working set exceeds or becomes equal to the following percentage
    container_memory_working_set_threshold_percentage = 95.0
    ```

3. Выполните следующую команду kubectl: `kubectl apply -f <configmap_yaml_file.yaml>` .

    Например, `kubectl apply -f container-azm-ms-agentconfig.yaml`.

До вступления в силу изменение конфигурации может занять несколько минут, и все omsagent Pod в кластере будут перезапущены. Перезагрузка является пошаговым перезапуском для всех модулей omsagent Pod, а не всех перезапусков одновременно. После завершения перезагрузки отображается сообщение, похожее на следующее и содержащее результат: `configmap "container-azm-ms-agentconfig" created` .

## <a name="next-steps"></a>Дальнейшие шаги

- Просмотрите [примеры запросов журналов](container-insights-log-search.md#search-logs-to-analyze-data) , чтобы просмотреть предварительно определенные запросы и примеры для проверки или настройки предупреждений, визуализации или анализа кластеров.

- Дополнительные сведения о Azure Monitor и мониторинге других аспектов кластера Kubernetes см. в разделе [Просмотр производительности кластера Kubernetes](container-insights-analyze.md).
