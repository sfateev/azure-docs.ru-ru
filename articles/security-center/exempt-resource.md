---
title: Исключение ресурса из рекомендаций по безопасности центра безопасности Azure и оценки безопасности
description: Узнайте, как исключить ресурс из рекомендаций по безопасности и оценки безопасности
author: memildin
ms.author: memildin
ms.date: 9/22/2020
ms.topic: how-to
ms.service: security-center
manager: rkarlin
ms.openlocfilehash: 87c16207f312479dcfe083ad9494d75b3538e18c
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2020
ms.locfileid: "91532556"
---
# <a name="exempt-a-resource-from-recommendations-and-secure-score"></a>Исключение ресурса из рекомендаций и безопасной оценки

Основной приоритет каждой группы безопасности пытается убедиться в том, что аналитики могут сосредоточиться на задачах и инцидентах, которые важны для Организации. В центре безопасности имеется множество функций для настройки наиболее важных данных, и гарантируется, что ваш показатель безопасности является допустимым отражением решений по обеспечению безопасности вашей организации. Исключение ресурсов — одна из таких функций.

При исследовании рекомендации по безопасности в центре безопасности Azure одним из первых видов информации является список затронутых ресурсов.

Иногда ресурс будет указан в списке, который не должен включаться в список. Например, ресурс мог был исправлен процессом, который не отслеживается Центром безопасности. Или ваша организация решила принять этот риск для определенного ресурса. 

В таких случаях можно создать правило исключения и убедиться, что ресурс не перечислен с неработоспособными ресурсами в будущем и не повлияет на вашу оценку безопасности. 

Ресурс будет указан как неприменимо и причина будет отображаться как "исключено" с выбранным обоснованием.

## <a name="availability"></a>Доступность

|Аспект|Подробнее|
|----|:----|
|Состояние выпуска:|Предварительный просмотр|
|Цены|Это функция политики Azure уровня "Премиум", предлагаемая для клиентов защитника Azure без дополнительных затрат. В будущем может взиматься плата за использование других пользователей.|
|Требуемые роли и разрешения|**Владелец подписки** или **участник политики** для создания исключения<br>Чтобы создать правило, необходимо иметь разрешения на изменение политик в политике Azure.<br>Дополнительные сведения см. в статье [разрешения RBAC в политике Azure](../governance/policy/overview.md#azure-rbac-permissions-in-azure-policy).|
|Облако.|![Да](./media/icons/yes-icon.png) Коммерческие облака<br>![Нет](./media/icons/no-icon.png) Национальные и независимые (US Gov, China Gov, другие правительственные облака)|
|||


## <a name="create-an-exemption-rule"></a>Создание правила исключения

1. В списке неработоспособных ресурсов выберите меню с многоточием (...) для ресурса, который требуется исключить.

    :::image type="content" source="./media/exempt-resource/create-exemption.png" alt-text="Создание параметра исключения из контекстного меню":::

    Откроется область создание исключения.

    :::image type="content" source="./media/exempt-resource/exemption-rule-options.png" alt-text="Создание параметра исключения из контекстного меню":::

1. Введите критерии и выберите условия, по которым следует исключить этот ресурс:
    - **Устранение** . Эта проблема не относится к ресурсу, так как он обрабатывается другим средством или процессом, отличным от предлагаемого.
    - **Отказ** — принятие риска для этого ресурса
1. Щелкните **Сохранить**.
1. Через некоторое время (это может занять до 24 часов):
    - Ресурс не влияет на вашу оценку безопасности.
    - Ресурс указан на вкладке **неприменимо** на странице сведений об рекомендации.
    - В информационной панели в верхней части страницы сведений о рекомендациях отображается число исключенных ресурсов:
        
        :::image type="content" source="./media/exempt-resource/info-banner.png" alt-text="Создание параметра исключения из контекстного меню":::

1. Чтобы проверить исключенные ресурсы, откройте вкладку **неприменимо** .

    :::image type="content" source="./media/exempt-resource/modifying-exemption.png" alt-text="Создание параметра исключения из контекстного меню":::

    Причина каждого исключения включается в таблицу (1).

    Чтобы изменить или удалить исключение, выберите меню с многоточием (...), как показано (2).


## <a name="review-all-of-the-exemption-rules-on-your-subscription"></a>Проверьте все правила исключения в подписке.

Правила исключения используют политику Azure для создания исключения ресурса в назначении политики.

Вы можете использовать политику Azure для трассировки исключения на странице **исключение** :

:::image type="content" source="./media/exempt-resource/policy-page-exemption.png" alt-text="Создание параметра исключения из контекстного меню":::



## <a name="next-steps"></a>Дальнейшие шаги

В этой статье вы узнали, как исключить ресурс из рекомендации, чтобы он не влиял на оценку безопасности. Дополнительные сведения о безопасной оценке см. в следующих статьях:

- [Оценка безопасности в Центре безопасности Azure](secure-score-security-controls.md)