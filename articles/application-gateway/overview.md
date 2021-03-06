---
title: Что такое шлюз приложений Azure
description: Сведения об использовании шлюза приложений Azure для управления веб-трафиком для вашего приложения.
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.topic: overview
ms.custom: mvc
ms.date: 08/26/2020
ms.author: victorh
ms.openlocfilehash: 7ccc83a61ac4ffe6e1bb6767a9c611bd3fcc0edf
ms.sourcegitcommit: eb6bef1274b9e6390c7a77ff69bf6a3b94e827fc
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/05/2020
ms.locfileid: "88892785"
---
# <a name="what-is-azure-application-gateway"></a>Что такое шлюз приложений Azure?

Шлюз приложений Azure — это подсистема балансировки нагрузки веб-трафика, предназначенная для управления трафиком веб-приложений. Традиционные подсистемы балансировки нагрузки работают на транспортном уровне (уровень 4 OSI — протоколы TCP и UDP) и маршрутизируют трафик к IP-адресу и порту назначения в зависимости от исходного IP-адреса и порта.

Шлюза приложений умеет принимать решения о маршрутизации на основе дополнительных атрибутов HTTP-запроса, например пути URI или заголовков узла. Например, вы можете маршрутизировать трафик на основе входящего URL-адреса. Поэтому, если во входящем URL-адресе есть элемент `/images`, вы можете направлять трафик к определенному набору (пулу) серверов, настроенных для изображений. Если в URL-адресе есть элемент `/video`, трафик направляется к другому пулу, который оптимизирован для видео.

![imageURLroute](./media/application-gateway-url-route-overview/figure1-720.png)

Этот тип маршрутизации называется балансировкой нагрузки на уровне приложения (уровень 7 OSI). Шлюз приложений Azure может выполнять маршрутизацию на основе URL-адреса и многие другие действия.

>[!NOTE]
> Azure предоставляет набор полностью управляемых решений балансировки нагрузки для пользовательских сценариев. Если вам нужна высокая производительность, низкая задержка, балансировка нагрузки уровня 4, см. статью [Что такое Azure Load Balancer](../load-balancer/load-balancer-overview.md). Если вам нужна глобальная балансировка нагрузки DNS, ознакомьтесь со статьей [Что такое диспетчер трафика](../traffic-manager/traffic-manager-overview.md). В комплексных сценариях может быть целесообразно объединить эти решения.
>
> Сравнение параметров балансировки нагрузки Azure см. в статье [Overview of load-balancing options in Azure](https://docs.microsoft.com/azure/architecture/guide/technology-choices/load-balancing-overview) (Общие сведения о параметрах балансировки нагрузки в Azure).

## <a name="features"></a>Компоненты

Сведения о функциях Шлюза приложений см. в [этой статье](features.md).

## <a name="pricing-and-sla"></a>Цены и соглашение об уровне обслуживания

Сведения о ценах на Шлюз приложений см. на [этой странице](https://azure.microsoft.com/pricing/details/application-gateway/).

Соглашение об уровне обслуживания для Шлюза приложений представлено [здесь](https://azure.microsoft.com/support/legal/sla/application-gateway/v1_2/).

## <a name="whats-new"></a>Новые возможности

О новых возможностях Шлюза приложений Azure, можно узнать на странице [Обновления Azure](https://azure.microsoft.com/updates/?category=networking&query=Application%20Gateway).

## <a name="next-steps"></a>Дальнейшие шаги

В зависимости от требований и среды тестовый шлюз приложений можно создать с помощью портала Azure, Azure PowerShell или Azure CLI:

- [Краткое руководство. Направление веб-трафика с помощью Шлюза приложений Azure на портале Azure](quick-create-portal.md).
- [Краткое руководство. Направление веб-трафика с помощью шлюза приложений Azure в Azure PowerShell](quick-create-powershell.md)
- [Краткое руководство. Направление веб-трафика с помощью Шлюза приложений Azure и Azure CLI.](quick-create-cli.md)
