---
title: 'Центр безопасности Azure: часто задаваемые вопросы — общие вопросы'
description: Часто задаваемые вопросы о центре безопасности Azure — продукте, который помогает предотвращать, обнаруживать угрозы и реагировать на них
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.assetid: be2ab6d5-72a8-411f-878e-98dac21bc5cb
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/25/2020
ms.author: memildin
ms.openlocfilehash: 5695f9fa090419d803f4f3603b45b771321e5ce9
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2020
ms.locfileid: "91301454"
---
# <a name="faq---general-questions-about-azure-security-center"></a>Вопросы и ответы — Общие вопросы о центре безопасности Azure

## <a name="what-is-azure-security-center"></a>Что такое Центр безопасности Azure?
Центр безопасности Azure помогает предотвращать и обнаруживать угрозы, а также реагировать на них, обеспечивая более полную видимость и контроль над безопасностью ресурсов. Он включает встроенные функции мониторинга безопасности и управления политиками для подписок, помогает выявлять угрозы, которые в противном случае могли бы оказаться незамеченными, и взаимодействует с широким комплексом решений по обеспечению безопасности.

Центр безопасности использует агент Log Analytics для получения и хранения данных. Подробные сведения см. в статье [сбор данных в центре безопасности Azure](security-center-enable-data-collection.md).


## <a name="how-do-i-get-azure-security-center"></a>Как включить Центр безопасности Azure?
Центр безопасности Azure включается с помощью подписки Microsoft Azure и доступен на [портале Azure](https://azure.microsoft.com/features/azure-portal/). Чтобы получить доступ к нему, [Войдите на портал](https://portal.azure.com), нажмите кнопку **Обзор**и перейдите к **центру безопасности**.


## <a name="which-azure-resources-are-monitored-by-azure-security-center"></a>Какие ресурсы Azure отслеживаются центром безопасности Azure?
Центр безопасности Azure отслеживает следующие ресурсы Azure:

* Виртуальные машины (включая [облачные службы](../cloud-services/cloud-services-choose-me.md))
* Масштабируемые наборы виртуальных машин
* решения партнеров, интегрированные в подписку Azure, например брандмауэр веб-приложений на виртуальных машинах и в среде службы приложений.
* [Многие службы PaaS Azure перечислены в обзоре продукта.](features-paas.md)


## <a name="how-can-i-see-the-current-security-state-of-my-azure-resources"></a>Как увидеть текущее состояние безопасности ресурсов Azure?
На странице **Обзор центра безопасности** показана общая безопасность среды, разбитая на части вычислений, сети, & хранения данных и приложений. Каждый тип ресурса имеет индикатор, показывающий выявленные уязвимости системы безопасности. При выборе любой из плиток отображается список проблем безопасности, обнаруженных центром безопасности, а также перечень ресурсов в вашей подписке.



## <a name="what-is-a-security-policy"></a>Что такое политика безопасности?
Политика безопасности определяет набор элементов управления, которые рекомендуются для ресурсов в указанной подписке. В центре безопасности Azure вы можете настраивать политики для подписок Azure в соответствии с требованиями безопасности вашей компании, типом приложений и конфиденциальностью данных в каждой подписке.

Политики безопасности, включенные в центре безопасности Azure, позволяют получать рекомендации по обеспечению безопасности и осуществлять мониторинг. Дополнительные сведения о политиках безопасности см. в разделе [Наблюдение за работоспособностью системы безопасности в Центре безопасности Azure](security-center-monitoring.md).


## <a name="who-can-modify-a-security-policy"></a>Кто может изменять политику безопасности?
Чтобы изменить политику безопасности, необходимо быть **администратором безопасности** или **владельцем** этой подписки.

Дополнительные сведения о настройке политики безопасности см. в статье [Настройка политик безопасности в Центре безопасности Azure](tutorial-security-policy.md).


## <a name="what-is-a-security-recommendation"></a>Что такое рекомендация по безопасности?
Центр безопасности Azure анализирует состояние безопасности ресурсов Azure. При обнаружении потенциальной уязвимости системы безопасности создаются рекомендации. Рекомендации помогают выполнить процедуру настройки необходимого средства контроля. Примеры:

* Подготовка антивредоносного ПО помогает обнаруживать и устранять вредоносные программы.
* [Группы безопасности сети](../virtual-network/security-overview.md) и правила для управления трафиком виртуальных машин.
* Подготовка брандмауэра веб-приложения для защиты от атак на веб-приложения
* Развертывание отсутствующих обновлений системы
* Обращение к конфигурациям операционной системы, не соответствующих рекомендуемым базовым показателям

Здесь отображаются только рекомендации, включенные в политиках безопасности.


## <a name="what-triggers-a-security-alert"></a>Что инициирует оповещение системы безопасности?
Центр безопасности Azure автоматически собирает, анализирует и объединяет данные журнала из ресурсов Azure, сети и решений партнеров, таких как программы защиты от вредоносных программ и брандмауэры. При обнаружении угроз создается оповещение системы безопасности. Например, определяется следующее.

* Обращение взломанных виртуальных машин к опасному IP-адресу
* Обнаружение сложных вредоносных программ с использованием отчетов об ошибках Windows
* Грубые атаки на виртуальные машины
* Оповещения системы безопасности, отправленные встроенными партнерскими решениями обеспечения безопасности, такими как защита от вредоносных программ или брандмауэры веб-приложений


## <a name="whats-the-difference-between-threats-detected-and-alerted-on-by-microsoft-security-response-center-versus-azure-security-center"></a>Какова разница между угрозами, которые обнаруживаются и о которых сообщают Microsoft Security Response Center и Центр безопасности Azure?
Microsoft Security Response Center (MSRC) проводит выборочный мониторинг безопасности сети и инфраструктуры Azure и получает аналитические данные об угрозах и жалобы о нарушениях от третьих лиц. Когда MSRC узнает о том, что к данным клиента обращались незаконно или без соответствующих разрешений или что использование клиентом Azure не соответствует условиям допустимого использования, диспетчер инцидентов безопасности уведомляет об этом клиента. Уведомление обычно происходит путем отправки электронного сообщения контактным лицам по вопросам безопасности, указанным в центре безопасности Azure, или владельцу подписки Azure, если контактное лицо по вопросам безопасности не указано.

Центр безопасности — это служба Azure, которая постоянно следит за средой Azure клиента и использует средства аналитики для автоматического обнаружения широкого спектра потенциально вредоносных действий. Такие обнаружения регистрируются как предупреждения системы безопасности в панели мониторинга Центра безопасности.