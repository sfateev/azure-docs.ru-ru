---
title: Многоклиентские серверные концы
titleSuffix: Azure Application Gateway
description: На этой странице предоставлен обзор поддержки мультитенантных серверных частей в шлюзе приложений.
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.topic: article
ms.date: 06/09/2020
ms.author: victorh
ms.openlocfilehash: 308098bd1ac49510afccf0a7964face726906332
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2020
ms.locfileid: "84628677"
---
# <a name="application-gateway-support-for-multi-tenant-back-ends-such-as-app-service"></a>Поддержка шлюза приложений для серверных интерфейсов с несколькими клиентами, например службы приложений

В архитектурных архитектурах с несколькими клиентами на веб-серверах на одном экземпляре веб-сервера выполняется несколько веб-сайтов. Имена узлов используются для различения различных размещенных приложений. По умолчанию шлюз приложений не изменяет заголовок узла входящих данных HTTP от клиента и без изменений отправляет его на серверную часть. Это хорошо подходит для членов серверного пула, таких как сетевые карты, масштабируемые наборы виртуальных машин, общедоступные IP-адреса, внутренние IP-адреса и полное доменное имя, так как они не используют конкретный заголовок узла или расширение SNI для разрешения в правильную конечную точку. Однако существует множество служб, таких как веб-приложения службы приложений Azure и управление API Azure, которые являются несколькими клиентами, и используют конкретный заголовок узла или расширение SNI для разрешения в правильную конечную точку. Как правило, DNS-имя приложения, которое, в свою очередь, является DNS-именем, связанным с шлюзом приложений, отличается от доменного имени внутренней службы. Таким образом, заголовок узла в исходном запросе, полученный шлюзом приложений, не совпадает с именем узла внутренней службы. По этой причине, если заголовок узла в запросе от шлюза приложений к серверной части не изменится на имя узла внутренней службы, серверные части с несколькими клиентами не смогут разрешить запрос в правильную конечную точку. 

В Шлюзе приложений есть функция, которая позволяет пользователям переопределять заголовок узла HTTP в запросе в зависимости от имени узла серверной части. Эта функция обеспечивает поддержку мультитенантных серверных частей, например в веб-приложениях Службы приложений Azure и службе "Управление API". Эта возможность доступна для SKU "Стандартный" версий 1 и 2, а также SKU для WAF. 

![Переопределение узла](./media/application-gateway-web-app-overview/host-override.png)

> [!NOTE]
> Это неприменимо к среде службы приложений Azure (ASE), так как ASE является выделенным ресурсом в отличие от службы приложений Azure, которая является ресурсом с несколькими клиентами.

## <a name="override-host-header-in-the-request"></a>Переопределить заголовок узла в запросе

Возможность указывать переопределение узла определяется в [параметрах HTTP](https://docs.microsoft.com/azure/application-gateway/configuration-overview#http-settings) и может применяться к любому внутреннему пулу во время создания правила. Поддерживаются следующие два способа переопределения заголовка узла и расширения SNI для серверной части с несколькими клиентами.

- Возможность задать для имени узла фиксированное значение, явно указанное в параметрах HTTP. Эта возможность гарантирует, что заголовок узла переопределит это значение для всего трафика во внутреннем пуле, где применяются определенные параметры HTTP. При использовании сквозного протокола TLS это переопределенное имя узла используется в расширении SNI. Эта возможность позволяет использовать сценарии, в которых ферма серверных пулов ожидает заголовок узла, который отличается от входящего заголовка узла клиента.

- Возможность получать имя узла из IP-адреса или полного доменного имени участников серверного пула. Параметры HTTP также позволяют динамически выбирать имя узла из полного доменного имени участника серверного пула, если оно настроено с возможностью получения имени узла из отдельного элемента пула серверной части. При использовании сквозного TLS-адреса это имя узла берется из полного доменного имени и используется в расширении SNI. Эта возможность позволяет использовать сценарии, в которых серверный пул имеет несколько мультитенатных служб PaaS, таких как Веб-приложения Azure, а заголовок узла в запросе к каждому участнику содержит имя узла, извлеченное из полного доменного имени. Для реализации этого сценария мы используем параметр в параметрах HTTP под названием [выбрать имя узла из серверного адреса](https://docs.microsoft.com/azure/application-gateway/configuration-overview#pick-host-name-from-back-end-address) , который динамически переопределит заголовок узла в исходном запросе на тот, который упоминается во внутреннем пуле.  Например, если полное доменное имя серверного пула содержит "contoso11.azurewebsites.net" и "contoso22.azurewebsites.net", заголовок узла исходного запроса, который является contoso.com, будет переопределен в contoso11.azurewebsites.net или contoso22.azurewebsites.net при отправке запроса соответствующему внутреннему серверу. 

  ![сценарий веб-приложения](./media/application-gateway-web-app-overview/scenario.png)

Благодаря этой возможности клиенты настраивают параметры HTTP и пользовательские пробы для соответствующей конфигурации. Этот параметр затем привязывается к прослушивателю и серверному пулу с помощью правила.

## <a name="special-considerations"></a>Специальные рекомендации

### <a name="tls-termination-and-end-to-end-tls-with-multi-tenant-services"></a>Завершение TLS и комплексное TLS-обслуживание с несколькими клиентами

С помощью служб с несколькими клиентами поддерживается как завершение TLS, так и сквозное шифрование TLS. Для завершения TLS в шлюзе приложений сертификат TLS по крайней мере должен быть добавлен в прослушиватель шлюза приложений. Однако в случае сквозного TLS Доверенные службы Azure, такие как веб-приложения службы приложений Azure, не нуждаются в разрешении серверных интерфейсов в шлюзе приложений. Поэтому нет необходимости добавлять сертификаты проверки подлинности. 

![сквозной TLS](./media/application-gateway-web-app-overview/end-to-end-ssl.png)

Обратите внимание, что на приведенном выше рисунке не требуется добавлять сертификаты проверки подлинности, если служба приложений выбрана в качестве серверной части.

### <a name="health-probe"></a>Проверка работоспособности

Переопределение заголовка узла в **параметрах HTTP** влияет только на запрос и его маршрутизацию. Он не влияет на поведение пробы работоспособности. Для комплексной работы функции и пробы, и параметры HTTP необходимо изменить в соответствии с правильной конфигурацией. Кроме предоставления возможности указывать заголовок узла в конфигурации зонда, пользовательские зонды также поддерживают возможность получения заголовка узла из текущих настроенных параметров HTTP. Эту конфигурацию можно указать с помощью параметра `PickHostNameFromBackendHttpSettings` в конфигурации пробы.

### <a name="redirection-to-app-services-url-scenario"></a>Перенаправление в сценарий URL-адреса службы приложений

Возможны ситуации, когда имя узла в ответе службы приложений может направить браузер конечных пользователей на имя узла *. azurewebsites.net вместо домена, связанного с шлюзом приложений. Эта проблема может возникать в следующих случаях.

- У вас есть перенаправление, настроенное в службе приложений. Перенаправление может быть простым добавлением завершающей косой черты в запрос.
- У вас есть аутентификация Azure AD, которая вызывает перенаправление.

Сведения об устранении таких ситуаций см. в [статье Устранение неполадок перенаправления в URL-адрес службы приложений](https://docs.microsoft.com/azure/application-gateway/troubleshoot-app-service-redirection-app-service-url).

## <a name="next-steps"></a>Дальнейшие действия

Узнайте, как настроить шлюз приложений с многопользовательским приложением, например веб-приложением службы приложений Azure, в качестве участника пула серверной части, посетив [страницу Настройка веб-приложений службы приложений с помощью шлюза приложений](https://docs.microsoft.com/azure/application-gateway/configure-web-app-portal) .
