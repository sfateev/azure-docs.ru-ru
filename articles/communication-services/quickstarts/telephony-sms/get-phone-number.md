---
title: Краткое руководство. Получение номера телефона из Служб коммуникации Azure
description: Узнайте, как приобрести номер телефона в Службах коммуникации Azure с помощью портала Azure.
author: prakulka
manager: nmurav
services: azure-communication-services
ms.author: prakulka
ms.date: 10/05/2020
ms.topic: quickstart
ms.service: azure-communication-services
ms.custom: references_regions
ms.openlocfilehash: e06c3720e180c1dc4fa2f227fd86d15cbbb0ff33
ms.sourcegitcommit: 6a4687b86b7aabaeb6aacdfa6c2a1229073254de
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/06/2020
ms.locfileid: "91756914"
---
# <a name="quickstart-get-a-phone-number-using-the-azure-portal"></a>Краткое руководство. Получение номера телефона с помощью портала Azure

[!INCLUDE [Public Preview Notice](../../includes/public-preview-include.md)]

Начните работу со Службами коммуникации Azure, приобретя номер телефона с помощью портала Azure.

## <a name="prerequisites"></a>Предварительные требования

- Учетная запись Azure с активной подпиской. [Создайте учетную запись](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) бесплатно.
- [Активный ресурс Служб коммуникации.](../create-communication-resource.md)

## <a name="get-a-phone-number"></a>Получение номера телефона

Чтобы начать подготовку номеров, перейдите к ресурсу Служб коммуникации на [портале Azure](https://portal.azure.com).

:::image type="content" source="../media/manage-phone-azure-portal-start.png" alt-text="Снимок экрана: главная страница ресурса Служб коммуникации.":::

### <a name="getting-new-phone-numbers"></a>Получение новых телефонных номеров

Перейдите в колонку "Номера телефонов" через меню ресурса.

:::image type="content" source="../media/manage-phone-azure-portal-phone-page.png" alt-text="Снимок экрана: главная страница ресурса Служб коммуникации.":::

Нажмите кнопку `Get`, чтобы запустить мастер. Мастер в колонке `Phone numbers` задаст вам ряд вопросов, которые помогут вам выбрать подходящий номер телефона для вашего сценария. 

Сначала необходимо выбрать страну или регион (`Country/region`), где вы хотите подготовить номер телефона. Затем нужно выбрать план телефонии (`use case`), который лучше подходит для ваших потребностей. 

:::image type="content" source="../media/manage-phone-azure-portal-get-numbers.png" alt-text="Снимок экрана: главная страница ресурса Служб коммуникации.":::

### <a name="select-a-phone-plan"></a>Выбор плана телефонии

Выбор плана телефонии включает два этапа: 

1. выбор [типа номера](../../concepts/telephony-sms/plan-solution.md#phone-number-types-in-azure-communication-services);
2. выбор самого [плана](../../concepts/telephony-sms/plan-solution.md#phone-number-plans-in-azure-communication-services).

Сейчас мы предлагаем два типа номеров: `Geographic` и `Toll-free`. Когда вы выберете тип номера, вам будет предложено несколько планов.

В нашем примере мы выбрали тип номера `Toll-free` и планы `Outbound calling` и `Inbound and Outbound SMS`.

:::image type="content" source="../media/manage-phone-azure-portal-select-plans.png" alt-text="Снимок экрана: главная страница ресурса Служб коммуникации.":::

Нажмите кнопку `Next: Numbers` в нижней части страницы, чтобы настроить номера телефонов, которые вы хотите подготовить.

### <a name="customizing-phone-numbers"></a>Настройка номеров телефонов

На странице `Numbers` можно настроить номера телефонов, которые вы хотите подготовить.

:::image type="content" source="../media/manage-phone-azure-portal-select-numbers-start.png" alt-text="Снимок экрана: главная страница ресурса Служб коммуникации.":::

> [!NOTE]
> В этом кратком руководстве показан поток настройки для номеров с типом `Toll-free`. Если вы выбрали номер с типом `Geographic`, процесс может немного отличаться, но результат будет таким же.

Выберите `Area code` в списке доступных кодов областей и введите количество номеров для подготовки, а затем нажмите `Search`, чтобы найти номера, соответствующие вашим требованиям. Отобразятся номера телефонов, соответствующие вашим потребностям, а также сведения о ежемесячной плате.

:::image type="content" source="../media/manage-phone-azure-portal-found-numbers.png" alt-text="Снимок экрана: главная страница ресурса Служб коммуникации.":::

> [!NOTE]
> Доступность зависит от типа номера, расположения и плана, которые вы выбрали.
> Эти номера резервируются на небольшой период времени, пока не истечет срок действия транзакции. После этого вам нужно будет снова выбрать номера.

Чтобы просмотреть сводку по покупке и разместить заказ, нажмите кнопку `Next: Summary` в нижней части страницы.

### <a name="place-order"></a>Размещение заказа

На странице сводки отобразятся следующие сведения: тип номера, возможности, номера телефонов и общая ежемесячная плата за подготовку этих номеров телефонов.

> [!NOTE]
> Это **ежемесячная абонентская плата** за аренду выбранного номера телефона. В этом представлении не отображаются **суммы для оплаты по мере использования**, то есть расходы на осуществление или прием вызовов. Полностью цены перечислены [здесь](../../concepts/pricing.md). Эти расходы зависят от типа номера и регионов, в которые осуществляются вызовы. Например, стоимость минуты вызова с регионального номера в Сиэтле на региональный номер в Нью-Йорке может отличаться от цены за вызов с того же номера на номер в Соединенном Королевстве.

Наконец, подтвердите выбор, щелкнув `Place order` в нижней части страницы.

:::image type="content" source="../media/manage-phone-azure-portal-get-numbers-summary.png" alt-text="Снимок экрана: главная страница ресурса Служб коммуникации.":::

## <a name="find-your-phone-numbers-on-the-azure-portal"></a>Поиск номеров телефонов на портале Azure

Перейдите к ресурсу Служб коммуникации Azure на [портале Azure](https://portal.azure.com).

:::image type="content" source="../media/manage-phone-azure-portal-start.png" alt-text="Снимок экрана: главная страница ресурса Служб коммуникации.":::

Выберите в меню колонку "Номера телефонов", чтобы управлять номерами телефонов.

:::image type="content" source="../media/manage-phone-azure-portal-phones.png" alt-text="Снимок экрана: главная страница ресурса Служб коммуникации.":::

> [!NOTE]
> Может пройти несколько минут, пока выбранные для подготовки номера телефонов отобразятся на этой странице.

### <a name="customizing-phone-number-plans"></a>Настройка планов номеров телефонов
На странице `Numbers` можно выбрать номер телефона, щелкнув номер, для которого вы хотите настроить план.

:::image type="content" source="../media/manage-phone-azure-portal-capability-update.png" alt-text="Снимок экрана: главная страница ресурса Служб коммуникации.":::

Выберите возможности из списка доступных возможностей для вызовов и отправки SMS, а затем нажмите `Confirm`, чтобы применить выбор.

## <a name="troubleshooting"></a>Диагностика

Распространенные вопросы и проблемы.

- Покупка телефонных номеров сейчас поддерживается только в США. Регион определяется по адресу выставления счетов в той подписке, с которой связан ресурс. Сейчас вы не можете переместить ресурс в другую подписку.

- При удалении номер телефона не будет освобожден или доступен для повторного приобретения до окончания периода выставления счетов.

- При удалении ресурса Служб коммуникации связанные с ним номера телефонов немедленно автоматически освобождаются.

## <a name="next-steps"></a>Дальнейшие действия

Из этого руководства вы узнали, как выполнить следующие действия:

> [!div class="checklist"]
> * приобрести номер телефона;
> * управлять номером телефона;
> * освободить номер телефона.

> [!div class="nextstepaction"]
> [Отправка SMS](../telephony-sms/send.md)
> [Начало работы с функцией вызова](../voice-video-calling/getting-started-with-calling.md)
