---
title: Руководство по использованию панели мониторинга размыкателя цепи в Azure Spring Cloud
description: Узнайте, как использовать панель мониторинга размыкателя цепи в Azure Spring Cloud.
author: MikeDodaro
ms.author: brendm
ms.service: spring-cloud
ms.topic: tutorial
ms.date: 04/06/2020
ms.custom: devx-track-java
ms.openlocfilehash: fa66f17c6f96ac7f70188c5a28c0b180ed2f03e0
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2020
ms.locfileid: "90906879"
---
# <a name="use-circuit-breaker-dashboard-with-azure-spring-cloud"></a>использованию панели мониторинга размыкателя цепи в Azure Spring Cloud

**Эта статья применима к:** ✔️ Java

::: zone pivot="programming-language-java"
Spring [Cloud Netflix Turbine](https://github.com/Netflix/Turbine) часто используется для объединения нескольких потоков метрик [Hystrix](https://github.com/Netflix/Hystrix), чтобы потоки можно было отслеживать в одном представлении с помощью панели мониторинга Hystrix. В этом учебнике показано, как использовать их в Azure Spring Cloud.
> [!NOTE]
> Netflix Hystrix широко используется во многих имеющихся приложениях Spring Cloud, но больше не находится в активной разработке. Если вы разрабатываете новый проект, используйте вместо размыкателя цепи Spring Cloud реализации, такие как [resilience4j](https://github.com/resilience4j/resilience4j). В отличие от Turbine, показанного в этом учебнике, новая платформа размыкателя цепи Spring Cloud объединяет все реализации своего конвейера данных метрик в Micrometer. Мы все еще работаем над поддержкой Micrometer в Azure Spring Cloud, поэтому он не рассматривается в этом учебнике.

## <a name="prepare-your-sample-applications"></a>Подготовка примеров приложений
Пример разветвлен из этого [репозитория](https://github.com/StackAbuse/spring-cloud/tree/master/spring-turbine).

Клонируйте пример репозитория в среду разработки:
```
git clone https://github.com/Azure-Samples/Azure-Spring-Cloud-Samples.git
cd Azure-Spring-Cloud-Samples/hystrix-turbine-sample
```

Создайте три приложения, которые будут использоваться в этом учебнике:
* user-service: простая служба REST, которая имеет одну конечную точку /personalized/{id}
* recommendation-service: простая служба REST с одной конечной точкой /recommendations, которая будет вызываться user-service.
* hystrix-turbine: служба панели мониторинга Hystrix для отображения потоков Hystrix и службы Turbine, объединяющей поток метрик Hystrix из других служб.
```
mvn clean package -D skipTests -f user-service/pom.xml
mvn clean package -D skipTests -f recommendation-service/pom.xml
mvn clean package -D skipTests -f hystrix-turbine/pom.xml
```
## <a name="provision-your-azure-spring-cloud-instance"></a>Подготовка экземпляра Azure Spring Cloud
Выполните процедуру из раздела [Подготовка экземпляра службы к работе в Azure CLI](https://docs.microsoft.com/azure/spring-cloud/spring-cloud-quickstart-launch-app-cli#provision-a-service-instance-on-the-azure-cli).

## <a name="deploy-your-applications-to-azure-spring-cloud"></a>Развертывание приложений в Azure Spring Cloud
Эти приложения не используют **сервер конфигурации**, таким образом нет необходимости настраивать **его** для Azure Spring Cloud.  Создайте и разверните приложения следующим образом:
```azurecli
az spring-cloud app create -n user-service --is-public
az spring-cloud app create -n recommendation-service
az spring-cloud app create -n hystrix-turbine --is-public

az spring-cloud app deploy -n user-service --jar-path user-service/target/user-service.jar
az spring-cloud app deploy -n recommendation-service --jar-path recommendation-service/target/recommendation-service.jar
az spring-cloud app deploy -n hystrix-turbine --jar-path hystrix-turbine/target/hystrix-turbine.jar
```
## <a name="verify-your-apps"></a>Проверка приложений
После запуска и обнаружения всех приложений получите доступ к `user-service` с помощью пути https://<username>-user-service.azuremicroservices.io/personalized/1 из браузера. Если приложение user-service может получить доступ к `recommendation-service`, вы должны получить следующие выходные данные. Обновите веб-страницу несколько раз, если она не работает.
```json
[{"name":"Product1","description":"Description1","detailsLink":"link1"},{"name":"Product2","description":"Description2","detailsLink":"link3"},{"name":"Product3","description":"Description3","detailsLink":"link3"}]
```
## <a name="access-your-hystrix-dashboard-and-metrics-stream"></a>Доступ к панели мониторинга Hystrix и потоку метрик
Выполните проверку, используя общедоступные конечные точки или частные конечные точки теста.

### <a name="using-public-endpoints"></a>Использование общедоступных конечных точек
Получите доступ к приложению hystrix-turbine с помощью пути `https://<SERVICE-NAME>-hystrix-turbine.azuremicroservices.io/hystrix` из браузера.  На следующем рисунке показана панель мониторинга Hystrix, работающая в этом приложении.

![Панель мониторинга Hystrix](media/spring-cloud-circuit-breaker/hystrix-dashboard.png)

Скопируйте URL-адрес потока Turbine `https://<SERVICE-NAME>-hystrix-turbine.azuremicroservices.io/turbine.stream?cluster=default` в текстовое поле и щелкните **Monitor Stream** (Мониторинг потока).  Отобразится панель мониторинга. Если в средстве просмотра ничего не отображается, нажмите конечные точки `user-service` для создания потоков.

![Поток Hystrix](media/spring-cloud-circuit-breaker/hystrix-stream.png) Теперь вы можете поэкспериментировать с панелью мониторинга размыкателя цепи.
> [!NOTE] 
> В рабочей среде панель мониторинга Hystrix и поток метрик не должны быть доступны в Интернете.

### <a name="using-private-test-endpoints"></a>Использование частных конечных точек теста
Потоки метрик Hystrix также доступны из `test-endpoint`. В качестве серверной службы мы не назначали общедоступную конечную точку для `recommendation-service`, но мы можем отобразить ее метрики с помощью test-endpoint в `https://primary:<KEY>@<SERVICE-NAME>.test.azuremicroservices.io/recommendation-service/default/actuator/hystrix.stream`.

![Поток test-endpoint Hystrix](media/spring-cloud-circuit-breaker/hystrix-test-endpoint-stream.png)

В качестве веб-приложения должна работать панель мониторинга Hystrix в `test-endpoint`. Если она не работает должным образом, это может происходить по двум причинам: во-первых, при использовании `test-endpoint` изменен базовый URL-адрес из `/ to /<APP-NAME>/<DEPLOYMENT-NAME>` или, во-вторых, для статического ресурса веб-приложение использует абсолютный путь. Чтобы заставить ее работать в `test-endpoint`, возможно, потребуется вручную отредактировать <base> в файлах переднего плана.

## <a name="next-steps"></a>Дальнейшие действия
* [Подготовка экземпляра службы к работе в Azure CLI](https://docs.microsoft.com/azure/spring-cloud/spring-cloud-quickstart-launch-app-cli#provision-a-service-instance-on-the-azure-cli)
* [Подготовка приложения Java Spring для развертывания в Azure Spring Cloud](https://docs.microsoft.com/azure/spring-cloud/spring-cloud-tutorial-prepare-app-deployment)
::: zone-end
