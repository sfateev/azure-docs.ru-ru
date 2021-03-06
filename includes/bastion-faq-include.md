---
title: включить файл
description: включить файл
services: bastion
author: cherylmc
ms.service: bastion
ms.topic: include
ms.date: 05/04/2020
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: efb32631c5ee1eedece6d2a06b94702b602ed418
ms.sourcegitcommit: eb6bef1274b9e6390c7a77ff69bf6a3b94e827fc
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/05/2020
ms.locfileid: "86276126"
---
### <a name="which-regions-are-available"></a><a name="regions"></a>Какие регионы доступны?

[!INCLUDE [region](bastion-regions-include.md)]

### <a name="do-i-need-a-public-ip-on-my-virtual-machine"></a><a name="publicip"></a>Нужен ли общедоступный IP-адрес моей виртуальной машине?

При подключении к виртуальной машине с использованием Бастиона Azure общедоступный IP-адрес для этой виртуальной машины Azure НЕ нужен. Бастион устанавливает подключение к вашей виртуальной машине по протоколу удаленного рабочего стола или SSH, используя ее частный IP-адрес в вашей виртуальной сети.

### <a name="is-ipv6-supported"></a>Поддерживается ли IPv6?

Сейчас IPv6 не поддерживается. Служба "Бастион Azure" поддерживает только IPv4.

### <a name="do-i-need-an-rdp-or-ssh-client"></a><a name="rdpssh"></a>Нужен ли клиент RDP или SSH?

Для подключения по протоколу RDP или SSH к виртуальной машине Azure на портале Azure клиент RDP или SSH не нужен. Используйте [портал Azure](https://portal.azure.com), чтобы подключиться к своей виртуальной машине по протоколу RDP или SSH непосредственно в браузере.

### <a name="do-i-need-an-agent-running-in-the-azure-virtual-machine"></a><a name="agent"></a>Нужно ли запускать агент на виртуальной машине Azure?

В браузере или на виртуальной машине Azure не нужно устанавливать агент или другое программное обеспечение. Бастион работает без агента и не требует дополнительного программного обеспечения для RDP и SSH.

### <a name="how-many-concurrent-rdp-and-ssh-sessions-does-each-azure-bastion-support"></a><a name="limits"></a>Сколько параллельных сеансов RDP и SSH поддерживает каждый бастион Azure?

RDP и SSH — протоколы на основе использования. При высоком уровне использования сеансов узел-бастион поддерживает меньше сеансов. Ниже приведены показатели, характерные для стандартных ежедневных процессов.

[!INCLUDE [limits](bastion-limits.md)]

### <a name="what-features-are-supported-in-an-rdp-session"></a><a name="rdpfeaturesupport"></a>Какие функции поддерживаются в сеансе RDP?

Сейчас поддерживаются только операции копирования и вставки текста. Такие операции, как копирование файлов, не поддерживаются. Вы можете поделиться своими отзывами о новых возможностях на странице, посвященной [Бастиону Azure](https://feedback.azure.com/forums/217313-networking?category_id=367303).

### <a name="which-browsers-are-supported"></a><a name="browsers"></a>Какие браузеры поддерживаются?

Используйте браузер Microsoft Edge или Google Chrome в ОС Windows. Для Apple Mac используйте браузер Google Chrome. ОС Windows и Mac также поддерживают Microsoft Edge Chromium.

### <a name="where-does-azure-bastion-store-customer-data"></a><a name="data"></a>Где Бастион Azure хранит данные клиента?

Бастион Azure не перемещает и не хранит данные клиента за пределами региона, в котором он развернут.

### <a name="are-any-roles-required-to-access-a-virtual-machine"></a><a name="roles"></a>Нужны ли какие-то роли для доступа к виртуальной машине?

Для подключения нужны такие роли:

* роль читателя на виртуальной машине;
* роль читателя на сетевом адаптере с частным IP-адресом для виртуальной машины;
* роль читателя в ресурсе Бастиона Azure.

### <a name="what-is-the-pricing"></a><a name="pricingpage"></a>Каковы цены?

Дополнительные сведения см. на [странице с расценками](https://aka.ms/BastionHostPricing).

### <a name="does-azure-bastion-require-an-rds-cal-for-administrative-purposes-on-azure-hosted-vms"></a><a name="rdscal"></a>Требуется ли для службы "Бастион Azure" клиентская лицензия RDS для администрирования на размещенных в Azure виртуальных машинах?
Нет, для предоставления службе "Бастион Azure" доступа к виртуальным машинам Windows Server [клиентская лицензия RDS](https://www.microsoft.com/en-us/p/windows-server-remote-desktop-services-cal/dg7gmgf0dvsv?activetab=pivot:overviewtab) не нужна при использовании исключительно для администрирования.

### <a name="what-keyboard-layouts-are-supported-during-the-bastion-remote-session"></a><a name="keyboard"></a>Какие раскладки клавиатуры поддерживаются во время удаленного сеанса с использованием службы "Бастион"?

В настоящее время служба "Бастион" Azure поддерживает в виртуальной машине раскладку клавиатуры en-us-qwerty.  Сейчас проводятся работы по реализации поддержки других языковых стандартов.

### <a name="is-user-defined-routing-udr-supported-on-an-azure-bastion-subnet"></a><a name="udr"></a>Поддерживаются ли определяемые пользователем маршруты в подсети Бастиона Azure?

Нет. Определяемые пользователем маршруты не поддерживаются в подсети Бастиона Azure.
В сценариях, в которых Бастион Azure и Брандмауэр Azure или виртуальный сетевой модуль (NVA) находятся в одной виртуальной сети, не нужно принудительно перенаправлять трафик из подсети Бастиона Azure в Брандмауэр Azure, так как обмен данными между Бастионом Azure и виртуальными машинами выполняется по частному каналу. См. сведения руководство по [получению доступа к виртуальным машинам за Брандмауэром Azure с помощью Бастиона](https://azure.microsoft.com/blog/accessing-virtual-machines-behind-azure-firewall-with-azure-bastion/).

### <a name="why-do-i-get-your-session-has-expired-error-message-before-the-bastion-session-starts"></a><a name="session"></a>Почему я получаю сообщение об ошибке "ваш сеанс истек" до начала сеанса Бастиона?

Сеанс должен быть инициирован только на портале Azure. Войдите на портал Azure и заново начните сеанс. Если вы перешли по URL-адресу непосредственно из другого сеанса браузера или вкладки, эта ошибка закономерна. Она призвана обеспечивать безопасность сеанса и возможность его запуска только на портале Azure.

### <a name="how-do-i-handle-deployment-failures"></a><a name="udr"></a>Как устранить сбои развертывания?

Изучите все сообщения об ошибках и при необходимости [подайте запрос на поддержку на портале Azure](https://docs.microsoft.com/azure/azure-portal/supportability/how-to-create-azure-support-request). Сбои развертывания могут возникать из-за [лимитов, квот и ограничений подписки Azure](https://docs.microsoft.com/azure/azure-resource-manager/management/azure-subscription-service-limits). В частности, к сбою развертывания Бастиона Azure может привести ограничение количества общедоступных IP-адресов, которые разрешено использовать для подписки.
