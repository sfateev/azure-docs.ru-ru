---
title: Общие сведения об Azure ExpressRoute. Соединение через частное подключение
description: В этом техническом обзоре возможностей ExpressRoute показано, как работает подключение ExpressRoute при расширении локальной сети в Azure через частное подключение.
services: expressroute
author: duongau
ms.service: expressroute
ms.topic: overview
ms.date: 10/05/2020
ms.author: duau
ms.openlocfilehash: ee690a73907eca3bcd577cf2d983c8abc5409925
ms.sourcegitcommit: a07a01afc9bffa0582519b57aa4967d27adcf91a
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/05/2020
ms.locfileid: "91743069"
---
# <a name="what-is-azure-expressroute"></a>Что такое Azure ExpressRoute?
ExpressRoute позволяет переносить локальные сети в Microsoft Cloud по частному подключению, обеспечиваемому поставщиком услуг подключения. ExpressRoute позволяет устанавливать подключения к облачным службам Майкрософт, таким как Microsoft Azure и Microsoft 365.

Это может быть подключение типа "любой к любому" (IP VPN), подключение Ethernet типа "точка-точка" или виртуальное кросс-подключение через поставщика услуг подключения на совместно используемом сервере. Подключения ExpressRoute не проходят через общедоступный Интернет. Это обеспечивает повышенный уровень безопасности, надежности и быстродействия подключений ExpressRoute и сопоставимый уровень задержек по сравнению с типовыми подключениями через Интернет. Сведения о том, как подключить сеть к облаку Майкрософт с помощью ExpressRoute, см. в статье [Модели подключения ExpressRoute](expressroute-connectivity-models.md).

![Варианты подключения ExpressRoute](./media/expressroute-introduction/expressroute-connection-overview.png)

## <a name="key-benefits"></a>Основные преимущества

* Подключение третьего уровня между локальной сетью и облаком Майкрософт через поставщика услуг подключения. Это может быть подключение типа "любой к любому" (IP VPN), подключение Ethernet типа "точка-точка" или виртуальное кросс-подключение через Ethernet Exchange.
* Подключение к облачным службам Майкрософт во всех регионах геополитической области.
* Глобальное подключение к службам Майкрософт во всех регионах с помощью надстройки ExpressRoute Premium.
* Динамическая маршрутизация между вашей сетью и средой Майкрософт по протоколу BGP.
* Встроенная избыточность в каждом расположении пиринга для более высокой надежности.
* [Соглашения об уровне обслуживания](https://azure.microsoft.com/support/legal/sla/), обеспечивающие бесперебойное подключение.
* Поддержка QoS для Skype для бизнеса.

Дополнительные сведения см. в статье [Вопросы и ответы по ExpressRoute](expressroute-faqs.md).

## <a name="features"></a>Компоненты

### <a name="layer-3-connectivity"></a>Подключение третьего уровня
Корпорация Майкрософт использует стандартный отраслевой протокол динамической маршрутизации BGP. Для обмена маршрутами между локальной сетью, экземплярами в Azure и общедоступными адресами Майкрософт. Мы устанавливаем несколько сеансов связи с вашей сетью по протоколу BGP для различных профилей трафика. Дополнительные сведения см. в статье [Канал ExpressRoute и домены маршрутизации](expressroute-circuit-peerings.md).

### <a name="redundancy"></a>Избыточность
Каждый канал ExpressRoute состоит из двух подключений к двум концевым маршрутизаторам Microsoft Enterprise (MSEE) в [расположении ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-locations#expressroute-locations) — на стороне поставщика услуг подключения и в вашей сети. Для Майкрософт требуется двойное BGP-подключение — со стороны поставщика услуг подключения и границы вашей сети — по одному для каждого MSEE. При желании вы можете не развертывать избыточные устройства или каналы Ethernet со своей стороны. Тем не менее поставщики услуг подключения используют избыточные устройства для того, чтобы убедиться, что подключения передаются в корпорацию Майкрософт избыточным образом. [Соглашение об уровне обслуживания](https://azure.microsoft.com/support/legal/sla/) будет действительно только при избыточной конфигурации подключения третьего уровня.

### <a name="connectivity-to-microsoft-cloud-services"></a>Подключение к облачным службам Майкрософт
Подключения ExpressRoute обеспечивают доступ к следующим службам:
* Службы Microsoft Azure
* Службы Microsoft 365

> [!NOTE]
> [!INCLUDE [expressroute-office365-include](../../includes/expressroute-office365-include.md)]
> 

Подробный список служб, поддерживаемых подключениями ExpressRoute, см. на странице с [вопросами и ответами по ExpressRoute](expressroute-faqs.md).

### <a name="connectivity-to-all-regions-within-a-geopolitical-region"></a>Подключение ко всем регионам геополитической области
Вы можете подключиться к среде Майкрософт в одном из [расположений пиринга](expressroute-locations.md) и получить доступ к регионам геополитической области.

Например, при подключении к Майкрософт в Амстердам с помощью ExpressRoute. У вас будет доступ ко всем облачным службам Майкрософт, размещенным в Северной и Западной Европе. Общие сведения о геополитических регионах, связанных регионах облачных служб Майкрософт и расположениях соответствующих пирингов ExpressRoute см. в статье [Партнеры и одноранговые расположения ExpressRoute](expressroute-locations.md).

### <a name="global-connectivity-with-expressroute-premium"></a>Глобальные подключения с использованием ExpressRoute Premium
Включив [ExpressRoute Premium](expressroute-faqs.md), вы сможете устанавливать подключения с другими геополитическими регионами. Например, подключившись через ExpressRoute к Майкрософт в Амстердаме, вы получите доступ ко всем облачным службам Майкрософт, размещенным по всем регионам мира. Доступ к службам, развернутым в Южной Америке или Австралии, можно получить точно так же, как к службам в Северной и Западной Европе. Национальные облака исключаются.

### <a name="local-connectivity-with-expressroute-local"></a>Локальные подключения с использованием ExpressRoute Local
Вы можете эффективно передавать данные, включив [локальный номер SKU](expressroute-faqs.md). С помощью локального номера SKU можно перенести данные в расположение ExpressRoute рядом с нужным регионом Azure. Для этого предложения плата за передачу данных включается в затраты на порт ExpressRoute. 

### <a name="across-on-premises-connectivity-with-expressroute-global-reach"></a>Локальные подключения с помощью ExpressRoute Global Reach
Вы можете включить службу ExpressRoute Global Reach для обмена данными между локальными сайтами, подключившись к своим цепям ExpressRoute. Например, если у вас есть закрытый центр обработки данных в Калифорнии, подключенный к каналу ExpressRoute в Кремниевой долине, а другой частный центр обработки данных в Техасе подключен к каналу ExpressRoute в Далласе. С помощью ExpressRoute Global Reach можно подключать частные центры обработки данных через эти два канала ExpressRoute. Трафик между центрами будет проходить через сеть Майкрософт.

См. дополнительные сведения об [ExpressRoute Global Reach](expressroute-global-reach.md).
### <a name="rich-connectivity-partner-ecosystem"></a>Широкая экосистема партнеров по подключению
ExpressRoute обладает постоянно растущей экосистемой поставщиков услуг подключения и партнеров-системных интеграторов. Последние данные см. в статье [Партнеры и одноранговые расположения ExpressRoute](expressroute-locations.md).

### <a name="connectivity-to-national-clouds"></a>Подключение к национальным облакам
Корпорация Майкрософт управляет изолированными облачными средами для особых геополитических регионов и клиентских сегментов. Список национальных облаков и поставщиков услуг см. на странице [Партнеры и одноранговые расположения ExpressRoute](expressroute-locations.md).

### <a name="expressroute-direct"></a>ExpressRoute Direct
ExpressRoute Direct дает пользователям возможность подключения непосредственно к глобальной сети корпорации Майкрософт в стратегически распределенных по всему миру расположениях пиринга. ExpressRoute Direct обеспечивает двойное подключение со скоростью 100 Гбит/с, которое поддерживает схему "активный — активный" при масштабировании.

К основным особенностям ExpressRoute Direct можно отнести следующее:

* возможность приема больших объемов данных в службах, таких как служба хранилища и Cosmos DB;
* физическая изоляция для отраслей, которые регулируются и требуют специализированной и изолированной связи, например банковское дело, правительство и розничная торговля;
* четкий контроль распределения схем на основе бизнес-единиц.

См. дополнительные сведения об [ExpressRoute Direct](https://go.microsoft.com/fwlink/?linkid=2022973).

### <a name="bandwidth-options"></a>Поддерживаемая пропускная способность
Каналы ExpressRoute можно приобрести для различной пропускной способности. Поддерживаемые значения пропускной способности перечислены ниже. Обратитесь к поставщику услуг подключения и узнайте, какую пропускную способность они предлагают.

* 50 Мбит/с
* 100 Мбит/с
* 200 Мбит/с
* 500 Мбит/с
* 1 Гбит/с
* 2 Гбит/с
* 5 Гбит/с
* 10 Гбит/с

### <a name="dynamic-scaling-of-bandwidth"></a>Динамическое масштабирование пропускной способности
Вы можете увеличить пропускную способность канала ExpressRoute (действуя по принципу "наибольших усилий"), не разорвав подключения. Дополнительные сведения см. в разделе [Изменение канала ExpressRoute](expressroute-howto-circuit-portal-resource-manager.md#modify).

### <a name="flexible-billing-models"></a>Гибкие модели тарификации
Вы можете выбрать оптимальную модель тарификации. Доступные модели тарификации перечислены ниже. Дополнительные сведения см. в статье [Вопросы и ответы по ExpressRoute](expressroute-faqs.md).

* **Безлимитный тарифный план**. Счета выставляются по ежемесячному тарифу, а передача любых входящих и исходящих данных предоставляется бесплатно.
* **Тарифный план с оплатой за трафик**. Счета выставляются по ежемесячному тарифу, а передача любых входящих данных предоставляется бесплатно. Передача исходящих данных оплачивается за каждый гигабайт исходящих данных. Скорость передачи данных зависит от региона.
* **Надстройка ExpressRoute Premium**. ExpressRoute Premium — это надстройка для канала ExpressRoute. Надстройка ExpressRoute Premium обеспечивает следующие возможности: 
  * повышенные квоты маршрутов для частного и общедоступного пиринга Azure с 4000 до 10 000;
  * глобальные подключения для служб. Канал ExpressRoute, созданный в любом регионе (за исключением национальных облаков), будет иметь доступ к ресурсам в любом другом регионе мира. Например, доступ к виртуальной сети, созданной в Западной Европе, может осуществляться через канал ExpressRoute, подготовленный в Кремниевой долине.
  * Увеличение числа связей виртуальных сетей на канал ExpressRoute с 10 до более широкого предела в зависимости от пропускной способности канала.

## <a name="faq"></a>ВОПРОСЫ И ОТВЕТЫ
Ответы на часто задаваемые вопросы об ExpressRoute см. в [этой статье](expressroute-faqs.md).

## <a name="whats-new"></a><a name="new"></a>Новые возможности

Подпишитесь на RSS-канал и просматривайте последние обновления для ExpressRoute на странице [Обновления Azure](https://azure.microsoft.com/updates/?category=networking&query=ExpressRoute).

## <a name="next-steps"></a>Дальнейшие действия
* Убедитесь, что выполнены все необходимые условия. Ознакомьтесь с разделом [Предварительные требования и контрольный список для ExpressRoute](expressroute-prerequisites.md).
* Узнайте о [моделях подключения ExpressRoute](expressroute-connectivity-models.md).
* Найти поставщика услуг. См. статью [Партнеры и одноранговые расположения ExpressRoute](expressroute-locations.md).
