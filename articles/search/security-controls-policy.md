---
title: Элементы управления соответствием Политики Azure для Когнитивного поиска Azure
description: Содержит список элементов управления соответствием Политики Azure, доступных для Когнитивного поиска Azure. Эти встроенные определения политик предоставляют популярные подходы к управлению соответствием ресурсов Azure.
ms.date: 10/07/2020
ms.topic: sample
author: HeidiSteen
ms.author: heidist
ms.service: search
ms.custom: subject-policy-compliancecontrols
ms.openlocfilehash: c2423874ccc5a1169b46200f0549b5d280db0870
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2020
ms.locfileid: "91825461"
---
# <a name="azure-policy-regulatory-compliance-controls-for-azure-cognitive-search"></a>Элементы управления соответствием Политики Azure для Когнитивного поиска Azure

Если вы используете [Политику Azure](../governance/policy/overview.md) для применения рекомендаций в [тестировании безопасности Azure](../security/benchmarks/introduction.md), скорее всего, вы уже знаете, что вы можете создавать политики для выявления и исправления служб с нарушениями соответствия. Такие политики могут быть пользовательскими или могут основываться на встроенных определениях, которые предоставляют критерии соответствия и подходящие решения для реализации проверенных рекомендаций.

Для Когнитивного поиска Azure в настоящее время предоставляется одно встроенное определение (приведено ниже), которое можно использовать при назначении политики. Такое встроенное определение предназначено для ведения журналов и мониторинга. Используя встроенное определение в [созданной вами политике](../governance/policy/assign-policy-portal.md), система проверит службы поиска, для которых не включено [диагностическое ведение журналов](search-monitor-logs.md), и при необходимости включит его.

Статья [Соответствие нормативным требованиям в Политике Azure](../governance/policy/concepts/regulatory-compliance.md) содержит созданные и управляемые корпорацией Майкрософт определения инициатив, называемые _встроенными_, для **доменов соответствия** и **элементов управления безопасностью**, которые связаны с различными стандартами соответствия. На этой странице перечислены **домены соответствия** и **элементы управления безопасностью** для Когнитивного поиска Azure. Вы можете назначить эти встроенные элементы для индивидуального **управления безопасностью**, чтобы обеспечить соответствие ресурсов Azure какому-либо определенному стандарту.

[!INCLUDE [azure-policy-compliancecontrols-introwarning](../../includes/policy/standards/intro-warning.md)]

[!INCLUDE [azure-policy-compliancecontrols-search](../../includes/policy/standards/byrp/microsoft.search.md)]

## <a name="next-steps"></a>Дальнейшие действия

- Узнайте больше о [соответствии требованиям Политики Azure](../governance/policy/concepts/regulatory-compliance.md).
- Ознакомьтесь со встроенными инициативами в [репозитории GitHub для Политики Azure](https://github.com/Azure/azure-policy).
