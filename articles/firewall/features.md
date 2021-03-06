---
title: Функции службы "Брандмауэр Azure"
description: Дополнительные сведения о функциях брандмауэра Azure
services: firewall
author: vhorne
ms.service: firewall
ms.topic: conceptual
ms.date: 10/08/2020
ms.author: victorh
ms.openlocfilehash: 7429be4430b2b520fb2a66b6b2c0dd138af8e501
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2020
ms.locfileid: "91850597"
---
# <a name="azure-firewall-features"></a>Функции службы "Брандмауэр Azure"

[Брандмауэр Azure](overview.md) — это управляемая облачная служба безопасности сети, которая защищает ресурсы виртуальной сети Azure.

![Общие сведения о брандмауэре](media/overview/firewall-threat.png)

Брандмауэр Azure включает следующие функции.

- [Встроенная Высокая доступность](#built-in-high-availability)
- [Зоны доступности](#availability-zones)
- [Неограниченная масштабируемость облака](#unrestricted-cloud-scalability)
- [Правила фильтрации FQDN для приложений](#application-fqdn-filtering-rules)
- [правила фильтрации трафика;](#network-traffic-filtering-rules)
- [Теги FQDN](#fqdn-tags)
- [Теги служб](#service-tags)
- [Анализ угроз](#threat-intelligence)
- [поддержку исходящих данных SNAT;](#outbound-snat-support)
- [Поддержка DNAT для входящего трафика](#inbound-dnat-support)
- [Несколько общедоступных IP-адресов](#multiple-public-ip-addresses)
- [Ведение журнала Azure Monitor](#azure-monitor-logging)
- [Принудительное туннелирование](#forced-tunneling)
- [Сертификаты](#certifications)

## <a name="built-in-high-availability"></a>высокую доступность;

Встроены средства обеспечения высокого уровня доступности, поэтому не требуются дополнительные подсистемы балансировки нагрузки и ничего не нужно настраивать.

## <a name="availability-zones"></a>зоны доступности;

Брандмауэр Azure можно настроить во время развертывания. Это позволит охватить несколько зон доступности, что повысит уровень доступности. С использованием зон доступности ваша доступность вырастет до 99,99 % Подробнее этот вопрос рассматривается в [Соглашении об уровне обслуживания](https://azure.microsoft.com/support/legal/sla/azure-firewall/v1_0/) для Брандмауэра Azure. Если выбраны две или более зон доступности, предоставляется Соглашение об уровне обслуживания на уровне 99,99 %.

Также Брандмауэр Azure можно связать с определенной ближайшей зоной, используя для этого стандарт обслуживания на уровне 99,95 %.

За брандмауэр, развернутый в зоне доступности, не взимается дополнительная плата. Тем не менее, существует дополнительная плата за входящую и исходящую передачу данных, связанную с зонами доступности. Дополнительные сведения см. на странице [Сведения о стоимости пропускной способности](https://azure.microsoft.com/pricing/details/bandwidth/).

Зоны доступности Брандмауэра Azure доступны в регионах, в которых поддерживаются зоны доступности. Дополнительные сведения см. в статье [Регионы с поддержкой Зон доступности в Azure](../availability-zones/az-region.md).

> [!NOTE]
> Зоны доступности можно настроить только во время развертывания. Существующий брандмауэр невозможно настроить на включения зон доступности.

Дополнительные сведения о Зонах доступности см. в статье [Что такое Зоны доступности в Azure](../availability-zones/az-overview.md).

## <a name="unrestricted-cloud-scalability"></a>неограниченную облачную масштабируемость;

Брандмауэр Azure может увеличивать масштаб настолько, насколько это необходимо для изменения потоков сетевого трафика, поэтому нет необходимости выделять бюджет для пикового трафика.

## <a name="application-fqdn-filtering-rules"></a>Правила фильтрации FQDN для приложений

Можно ограничить исходящий трафик HTTP/S или трафик Azure SQL указанным списком полных доменных имен (FQDN), включая подстановочные. Эта функция не требует завершения TSL-запросов.

## <a name="network-traffic-filtering-rules"></a>правила фильтрации трафика;

Можно централизованно создавать *разрешение* или *запрет* правил сетевой фильтрации по исходному и целевому IP-адресу, порту и протоколу. Брандмауэр Azure полностью отслеживает состояние, поэтому он может различать правомерные пакеты для разных типов подключений. Правила применяются и регистрируются в нескольких подписках и виртуальных сетях.

## <a name="fqdn-tags"></a>Теги FQDN

[Теги FQDN](fqdn-tags.md) упрощают настройку прохождения трафика известных служб Azure через брандмауэр. К примеру, вам нужно, чтобы брандмауэр пропускал трафик Центра обновления Windows. Вы создаете правило приложения и добавляете тег Центра обновления Windows. Теперь трафик из Центра обновления Windows сможет проходить через брандмауэр.

## <a name="service-tags"></a>Теги служб

[Тег службы](service-tags.md) представляет группу префиксов IP-адресов, чтобы упростить создание правила безопасности. Невозможно создать собственный тег службы или задать IP-адреса, которые будут входить в тег. Корпорация Майкрософт управляет префиксами адресов, входящих в тег службы, и автоматически обновляет этот тег при изменении адресов.

## <a name="threat-intelligence"></a>Анализ угроз

Фильтрация на основе [Threat Intelligence](threat-intel.md) может быть включена в брандмауэре с целю создания оповещений и запрета трафика, поступающего с известных вредоносных IP-адресов и доменов, а также передающегося на них. IP-адреса и домены также передаются из канала Microsoft Threat Intelligence.

## <a name="outbound-snat-support"></a>поддержку исходящих данных SNAT;

Все исходящие IP-адреса виртуального трафика преобразовываются к общедоступному IP-адресу брандмауэра Azure (преобразование исходных сетевых адресов (NAT)). Можно определить и разрешить трафик, исходящий из виртуальной сети, к удаленным интернет-адресатам. Брандмауэр Azure не использует SNAT, если IP-адрес назначения является IP-адресом частного диапазона согласно [IANA RFC 1918](https://tools.ietf.org/html/rfc1918). 

Если для частных сетей в организации используются общедоступные IP-адреса, брандмауэр Azure будет использовать SNAT трафика для одного из своих частных IP-адресов AzureFirewallSubnet. В настройках Брандмауэра Azure можно указать, что **не нужно** использовать SNAT для диапазона общедоступный IP-адресов. Дополнительные сведения см. в статье об [использовании SNAT для диапазонов частных IP-адресов в Брандмауэре Azure](snat-private-range.md).

## <a name="inbound-dnat-support"></a>Поддержка DNAT для входящего трафика

Входящий Интернет-трафик, поступающий на общедоступный IP-адрес брандмауэра, преобразуется (этот процесс называется преобразованием сетевых адресов назначения — DNAT) и фильтруется по частным IP-адресам в виртуальных сетях.

## <a name="multiple-public-ip-addresses"></a>Несколько общедоступных IP-адресов

С брандмауэром можно связать [несколько общедоступных IP-адресов](deploy-multi-public-ip-powershell.md) (до 250).

Это позволяет выполнить следующие сценарии:

- **DNAT**. Позволяет преобразовать несколько стандартных экземпляров порта для внутренних серверов. Например, если существует два общедоступных IP-адреса, то для обоих IP-адреса можно преобразовать TCP-порт 3389 (RDP).
- **SNAT**. Дополнительные порты доступны для исходящих SNAT подключений, что снижает вероятность нехватки SNAT портов. На данный момент Брандмауэр Azure случайно выбирает общедоступные IP-адреса источника, которые можно использовать для подключения. Если в сети установлена любая фильтрация, необходимо разрешить все публичные IP-адреса, которые связанные с брандмауэром. Чтобы упростить эту настройку, можно использовать [префикс общедоступного IP-адреса](../virtual-network/public-ip-address-prefix.md).

## <a name="azure-monitor-logging"></a>ведение журналов Azure Monitor;

Все события интегрируются с Azure Monitor, позволяя архивировать журналы в учетную запись хранения, передавать события в концентратор событий или отправлять их в журналы Azure Monitor. Примеры журналов Azure Monitor см. в разделе [журналы Azure Monitor для брандмауэра Azure](log-analytics-samples.md).

Дополнительные сведения см. в статье [Учебник. Мониторинг журналов и метрик Брандмауэра Azure](tutorial-diagnostics.md). 

Книга брандмауэра Azure предоставляет гибкий холст для анализа данных брандмауэра Azure. Его можно использовать для создания многофункциональных визуальных отчетов в портал Azure. Дополнительные сведения см. в статье [мониторинг журналов с помощью книги брандмауэра Azure](firewall-workbook.md).

## <a name="forced-tunneling"></a>Принудительное туннелирование

Вы можете настроить Брандмауэр Azure, чтобы направить весь интернет-трафик в назначенный узел следующего перехода, а не непосредственно в Интернет. Например, у вас может быть локальный пограничный брандмауэр или другой виртуальный сетевой модуль для обработки сетевого трафика перед его передачей в Интернет. Дополнительные сведения см. в статье [Azure Firewall forced tunneling](forced-tunneling.md) (Принудительное туннелирование в Брандмауэре Azure).

## <a name="certifications"></a>Сертификаты

Брандмауэр Azure соответствует требованиям стандартов Payment Card Industry Data Security Standard (PCI DSS), Международной организация по стандартизации (ISO) и ICSA Labs, а также требованиям к отчетам System and Organization Controls (SOC). Дополнительные сведения см. в статье о [сертификатах соответствия Брандмауэра Azure](compliance-certifications.md).

## <a name="next-steps"></a>Дальнейшие шаги

- [Логика обработки правил Брандмауэра Azure](rule-processing.md)