---
title: Общие сведения о Работоспособность ресурсов шлюза приложений Azure
description: В этой статье представлен обзор функции работоспособности ресурсов для шлюза приложений Azure.
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.topic: article
ms.date: 7/9/2019
ms.author: victorh
ms.openlocfilehash: db29551a8150b70e797d45fe659482470c8aca2a
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2020
ms.locfileid: "67659506"
---
# <a name="azure-application-gateway-resource-health-overview"></a>Общие сведения о Работоспособность ресурсов шлюза приложений Azure

Служба [Работоспособность ресурсов Azure](../service-health/resource-health-overview.md) позволяет выполнять диагностику и получать поддержку, если неполадки со службой Azure влияют на ресурсы. Она предоставляет сведения о текущем состоянии работоспособности ресурсов и о состоянии работоспособности ресурсов за прошедший период, а также техническую поддержку для устранения проблем.

Для шлюза приложений Работоспособность ресурсов полагается на сигналы, порожденные шлюзом, чтобы оценить, является ли он работоспособным. Если шлюз неработоспособен, Работоспособность ресурсов анализирует дополнительные сведения для определения источника проблемы. Он также определяет действия, выполняемые корпорацией Майкрософт, или возможности, которые можно предпринять для решения проблемы.

Дополнительные сведения о том, как оценивается работоспособность, приведены в полном списке типов ресурсов и проверок работоспособности в [этой статье](../service-health/resource-health-checks-resource-types.md#microsoftnetworkapplicationgateways).


Состояние работоспособности шлюза приложений отображается как одно из следующих состояний:

## <a name="available"></a>Доступно

**Доступное** состояние означает, что служба не обнаружила событий, влияющих на работоспособность ресурса. Вы увидите **недавно устраненное** уведомление в случаях, когда шлюз был восстановлен после незапланированного простоя за последние 24 часа.

![Доступное состояние работоспособности](media/resource-health-overview/available-full.png)

## <a name="unavailable"></a>Недоступно

**Недоступное** состояние означает, что Служба обнаружила текущую платформу или событие, не связанное с платформой, которое влияет на работоспособность шлюза.

### <a name="platform-events"></a>События на платформе

События на платформе активируются несколькими компонентами инфраструктуры Azure. Они включают в себя как запланированные действия (например, плановое обслуживание), так и непредвиденные инциденты (например, внеплановая перезагрузка узла).

Служба работоспособности ресурсов предоставляет дополнительную информацию о событии и процессе восстановления. Она также позволяет обратиться в службу поддержки, даже если у вас нет действующего соглашения о поддержке с корпорацией Майкрософт.

![Состояние "недоступно"](media/resource-health-overview/unavailable.png)

## <a name="unknown"></a>Неизвестно

**Неизвестное** состояние работоспособности указывает, работоспособность ресурсов не получала сведения о шлюзе более 10 минут. Это состояние не является основным индикатором состояния шлюза. Но это важная точка данных в процессе устранения неполадок.

Если шлюз работает должным образом, состояние становится **доступным** через несколько минут.

При возникновении проблем **неизвестное** состояние работоспособности может потребовать, чтобы событие на платформе влияло на шлюз.

![Неизвестное состояние](media/resource-health-overview/unknown.png)

## <a name="degraded"></a>Деградация

Состояние работоспособности " **снижено** " указывает, что шлюз обнаружил снижение производительности, хотя он по-прежнему доступен для использования.

![Состояние "в состоянии"](media/resource-health-overview/degraded.png)

## <a name="next-steps"></a>Дальнейшие действия

Дополнительные сведения об устранении неполадок брандмауэра веб-приложения шлюза приложений (WAF) см. в разделе [Устранение неполадок брандмауэра веб-приложения (WAF) для шлюза приложений Azure](web-application-firewall-troubleshoot.md).