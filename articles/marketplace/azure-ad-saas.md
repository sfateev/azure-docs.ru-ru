---
title: Azure Active Directory и предложения SaaS на рынке в коммерческом магазине
description: Узнайте, как Azure Active Directory работает с предложениями SaaS, предоставляемыми в коммерческом магазине Майкрософт.
author: mingshen-ms
ms.author: mingshen
ms.reviewer: dannyevers
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 09/04/2020
ms.openlocfilehash: 5a09105dac89f3dc241140f16f3d4be72cc97493
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2020
ms.locfileid: "89483632"
---
# <a name="azure-ad-and-transactable-saas-offers-in-the-commercial-marketplace"></a>Azure AD и предложения SaaS, предоставляемые в коммерческом магазине

Облачная служба управления удостоверениями и доступом Майкрософт, [Azure Active Directory](https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-whatis) (Azure AD), позволяет пользователям входить в систему и получать доступ к внутренним и внешним ресурсам. В коммерческом магазине Майкрософт Azure AD упрощает и обеспечивает более высокий уровень безопасности, включая издатели, покупатели и пользователей. С помощью Azure AD издатели могут автоматизировать подготовку пользователей к приложениям SaaS (программное обеспечение как услуга), а сами покупатели могут управлять этими подготовленными пользователями. 

Кроме того, [единый вход Azure AD](https://docs.microsoft.com/azure/active-directory/manage-apps/what-is-single-sign-on) (SSO) обеспечивает безопасность и удобство при входе пользователей в приложения в Azure AD. Более быстрое сотрудничество и оптимизированные возможности также вдохновить покупателю и уверенность пользователей с самого первого взаимодействия с приложением SaaS издателя. Это дает положительный впечатление, что создает видимость и способствует повторному бизнесу.

Следуя указаниям в этой статье, вы сможете сертифицировать предложение SaaS на коммерческом рынке. Дополнительные сведения о сертификации см. в подробных [политиках сертификации коммерческой Marketplace](https://aka.ms/commercial-marketplace-certification-policies#100-general), включая те, которые [относятся к SaaS](https://aka.ms/commercial-marketplace-certification-policies#1000-software-as-a-service-saas).

## <a name="before-you-begin"></a>Перед началом

При [создании предложения SaaS](./partner-center-portal/create-new-saas-offer.md) в центре партнеров вы выбираете набор конкретных параметров перечня, которые будут отображаться в списке предложений. Ваш выбор определяет, как ваше предложение будет передаваться в коммерческом магазине. Предложения, продаваемые через корпорацию Майкрософт, называются предложениями для Transact-t Мы выставляем клиенту счета от вашего имени для всех предложений, которые можно было бы выставлять. Если вы решили продать корпорации Майкрософт и хотите, чтобы транзакции были размещены от вашего имени (параметр **Да** ), вы решили создать предложение, которое можно использовать для инструкции, и эта статья предназначена для вас. Рекомендуется полностью прочитать его.

Если вы решили перечислить предложение только через коммерческий магазин и обрабатывать транзакции независимо (вариант **нет** ), у вас есть три способа получить доступ к вашему предложению: получите его бесплатно, Бесплатная пробная версия и свяжитесь со мной. Если выбрать вариант **получить сейчас (бесплатно)** или **бесплатную пробную версию**, эта статья не предназначена для вас. Дополнительные сведения см. [в статье Создание целевой страницы для бесплатного или пробного предложения SaaS в коммерческом магазине](./azure-ad-free-or-trial-landing-page.md) . Если вы выберете " **связаться со мной**", нет никаких обязанностей прямых издателей. Продолжайте создавать предложение в центре партнеров.

## <a name="how-azure-ad-works-with-the-commercial-marketplace-for-saas-offers"></a>Как Azure AD работает с коммерческим магазином для предложений SaaS

Azure AD обеспечивает эффективное приобретение, выполнение и управление решениями для коммерческих рынков. На рис. 1 показано, как издатель, покупатель и пользователь взаимодействуют для приобретения и активации подписки. Здесь также показано, как клиенты используют и управляют приложениями SaaS, которые они получают из коммерческого рынка. Для целей этого примера покупатель является пользователем приложения SaaS, который инициирует покупку из коммерческого рынка.

Как показано на рис. 1, когда покупатель выбирает ваше предложение, он запускает цепочку рабочих процессов, которая включает в себя покупку, подписку и управление пользователями. В рамках этой цепочки вы, как издатель несете ответственность за определенные требования, корпорация Майкрософт предоставляет поддержку в ключевых точках.

***Рис. 1. Использование Azure AD для предложений SaaS в коммерческом магазине***

:::image type="content" source="./media/azure-ad-saas/azure-ad-saas-flow.png" alt-text="Демонстрирует этапы управления покупками, управления подписками и необязательными процессами управления пользователями.":::

В следующих разделах приводятся сведения о требованиях для каждого шага процесса.

## <a name="process-steps-for-purchase-management"></a>Этапы процесса управления покупкой

На этом рисунке показаны четыре шага процесса управления покупками.

:::image type="content" source="./media/azure-ad-saas/azure-ad-saas-flow-1-4.png" alt-text="Демонстрирует этапы управления покупками, управления подписками и необязательными процессами управления пользователями.":::

Эта таблица содержит сведения о шагах процесса управления покупкой.

| Шаг процесса | Действие издателя | Рекомендуется или требуется для издателей |
| ------------ | ------------- | ------------- |
| 1. покупатель входит в коммерческий магазин с идентификатором Azure ID и выбирает предложение SaaS. | Действие издателя не требуется. | Неприменимо |
| 2. После приобретения покупатель выбирает **настройку учетной записи** в Azure Marketplace или **настраивается сейчас** в AppSource, который направляет покупателю на целевую страницу издателя для этого предложения. Покупатель должен иметь возможность войти в приложение SaaS издателя с помощью единого входа Azure AD и должно запросить только минимальное согласие, не требующее утверждения администратором Azure AD. | Создайте [целевую страницу](azure-ad-transactable-saas-landing-page.md) для предложения, чтобы она получала пользователя с удостоверением Azure AD или учетная запись Майкрософт (MSA) и поддерживала все необходимые дополнительные возможности подготовки или настройки. | Обязательно |
| 3. издатель запрашивает сведения о приобретении от API выполнения SaaS. | Используя [маркер доступа](./partner-center-portal/pc-saas-registration.md) , созданный на основе идентификатора приложения целевой страницы, [вызовите эту конечную точку](./partner-center-portal/pc-saas-fulfillment-api-v2.md#resolve-a-purchased-subscription) , чтобы получить сведения о приобретении. | Обязательно |
| 4. с помощью Azure AD и Microsoft Graph API Издатель собирает сведения о компании и пользователях, необходимые для предоставления покупателю в приложении SaaS издателя.  | Разработайте токен пользователя Azure AD, чтобы найти имя и адрес электронной почты, либо [вызовите API Microsoft Graph](https://docs.microsoft.com/graph/use-the-api) и используйте делегированные разрешения для [получения сведений](https://docs.microsoft.com/graph/api/user-get) о пользователе, вошедшем в систему. | Обязательно |
||||

## <a name="process-steps-for-subscription-management"></a>Этапы процесса для управления подписками

На этом рисунке показаны два этапа процесса управления подписками.

:::image type="content" source="./media/azure-ad-saas/azure-ad-saas-flow-5-6.png" alt-text="Демонстрирует этапы управления покупками, управления подписками и необязательными процессами управления пользователями.":::

В этой таблице приводятся сведения о шагах процесса управления подписками.

| Шаг процесса | Действие издателя | Рекомендуется или требуется для издателей |
| ------------ | ------------- | ------------- |
| 5. издатель управляет подпиской на приложение SaaS через API выполнения SaaS. | Обработка изменений подписки и других задач управления с помощью [API-интерфейсов выполнения SaaS](./partner-center-portal/pc-saas-fulfillment-api-v2.md).<br><br>Для этого шага требуется маркер доступа, как описано в разделе Обработка шага 3. | Обязательно |
| 6. при использовании тарифных цен издатель создает события использования в API службы измерения. | Если приложение SaaS включает выставление счетов на основе использования, уведомления об использовании можно использовать через [API-интерфейсы службы измерения Marketplace](./partner-center-portal/marketplace-metering-service-apis.md).<br><br>Для этого шага требуется маркер доступа, как описано в шаге 3. | Требуется для измерения |
||||

## <a name="process-steps-for-user-management"></a>Этапы процесса управления пользователями

На этом рисунке показаны три этапа процесса управления пользователями.

:::image type="content" source="./media/azure-ad-saas/azure-ad-saas-flow-7-9.png" alt-text="Демонстрирует этапы управления покупками, управления подписками и необязательными процессами управления пользователями.":::

Шаги с 7 по 9 являются необязательными этапами процесса управления пользователями. Они предоставляют дополнительные преимущества для издателей, которые поддерживают единый вход Azure AD (SSO). В этой таблице приводятся сведения о шагах процесса управления пользователями.

| Шаг процесса | Действие издателя | Рекомендуется или требуется для издателей |
| ------------ | ------------- | ------------- |
| 7. Администраторы Azure AD в компании-покупателе могут дополнительно управлять доступом для пользователей и групп через Azure AD. | Для этого не требуется никаких действий издателя, если для пользователя установлен единый вход Azure AD (шаг 9). | Неприменимо |
| 8. Служба подготовки Azure AD обменивается изменениями между Azure AD и приложением SaaS издателя. | [Реализация конечной точки scim](https://docs.microsoft.com/azure/active-directory/app-provisioning/use-scim-to-provision-users-and-groups) для получения обновлений из Azure AD по мере добавления и удаления пользователей. | Рекомендуется |
| 9. После разрешения и подготовки приложения пользователи из компании-покупателя могут использовать единый вход Azure AD для входа в приложение SaaS издателя. | [Используйте единый вход Azure AD](https://docs.microsoft.com/azure/active-directory/manage-apps/what-is-single-sign-on) , чтобы позволить пользователям входить один раз с одной учетной записью в приложение SaaS издателя. | Рекомендуется |
||||

## <a name="next-steps"></a>Дальнейшие шаги

- [Создание целевой страницы для вашего предложения SaaS в коммерческом магазине](azure-ad-transactable-saas-landing-page.md)
- [Создание целевой страницы для бесплатного или пробного предложения SaaS в коммерческом магазине](azure-ad-free-or-trial-landing-page.md)
- [Создание предложения SaaS в коммерческом магазине](create-new-saas-offer.md)
