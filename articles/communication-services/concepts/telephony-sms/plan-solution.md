---
title: Планирование решения для телефонии и SMS в Службах коммуникации Azure
titleSuffix: An Azure Communication Services concept document
description: Узнайте, как спланировать эффективное использование телефонных номеров и телефонии.
author: prakulka
manager: nmurav
services: azure-communication-services
ms.author: prakulka
ms.date: 10/05/2020
ms.topic: overview
ms.custom: references_regions
ms.service: azure-communication-services
ms.openlocfilehash: 6a63df282cadf86668e69d2422a6c791e86010b6
ms.sourcegitcommit: d9ba60f15aa6eafc3c5ae8d592bacaf21d97a871
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/06/2020
ms.locfileid: "91767128"
---
# <a name="plan-your-telephony-and-sms-solution"></a>Планирование решения для телефонии и SMS

[!INCLUDE [Public Preview Notice](../../includes/public-preview-include.md)]


Службы коммуникации Azure позволяют использовать номера телефонов для голосовых звонков и отправки SMS-сообщений через телефонную сеть общего пользования (ТСОП). В этом документе мы рассмотрим типы телефонных номеров, планы и доступность по регионам для планирования решения по обеспечению телефонной связи и отправки SMS с помощью Служб коммуникации.

[!INCLUDE [Emergency Calling Notice](../../includes/emergency-calling-notice-include.md)]


## <a name="phone-number-types-in-azure-communication-services"></a>Типы телефонных номеров в Службах коммуникации Azure
 
Службы коммуникации предлагают два типа телефонных номеров: **местные** и **бесплатные**. 

### <a name="local-numbers"></a>Местные номера
Местные (географические) номера — это 10-значные телефонные номера, состоящие из местных кодов городов в США. Например, `+1 (206) XXX-XXXX` — это местный номер с кодом города `206`. Это телефонный код Сиэтла. Такие телефонные номера обычно используются физическими лицами и местными компаниями. Службы коммуникации Azure предлагают местные номера в США. Эти номера можно использовать для телефонных звонков, но они не подходят для отправки SMS. 

### <a name="toll-free-numbers"></a>Бесплатные номера
Бесплатные телефонные номера — это 10-значные телефонные номера с особыми телефонными кодами, на которые можно звонить бесплатно с любого номера телефона. Например, `+1 (800) XXX-XXXX` — это бесплатный номер в Северной Америке. Эти номера телефонов обычно используются для обслуживания клиентов. Службы коммуникации Azure предлагают бесплатные номера в США. Эти номера можно использовать для осуществления телефонных звонков и отправки SMS. Люди не могут использовать бесплатные номера сами. Их можно назначать только приложениям.

#### <a name="choosing-a-phone-number-type"></a>Выбор типа номера телефона

Если номер телефона будет использоваться приложением (например, для вызова или отправки сообщений от имени вашей службы), можете выбрать бесплатный или местный (географический) номер. Вы можете выбрать бесплатный номер, если приложение отправляет SMS-сообщения и (или) осуществляет вызовы.

Если номер телефона используется пользователем (например, пользователем вызывающего приложения), следует использовать местный (географический) номер телефона. 

В следующей таблице перечислены все возможные типы номеров телефонов. 

| Тип номера телефона | Пример                              | Доступность по странам    | Возможности использования телефонного номера |Типичный сценарий использования                                                                                                     |
| ----------------- | ------------------------------------ | ----------------------- | ------------------------|------------------------------------------------------------------------------------------------------------------- |
| Местный (географический)        | +1 (местный код города) XXX XX XX  | США                      | Вызов (исходящий) | Назначение телефонных номеров пользователям в приложениях  |
| Бесплатный номер         | +1 (*код*, бесплатный в пределах региона) XXX XX XX | США                      | Вызов (исходящий), SMS (входящие или исходящие)| Назначение телефонных номеров системам интерактивного речевого ответа (ботам) и приложениям для работы с SMS                                        |


## <a name="phone-number-plans-in-azure-communication-services"></a>Планы телефонных номеров в Службах коммуникации Azure 

Для большинства номеров телефонов мы поддерживаем персонализированную настройку набора планов. Некоторым разработчикам нужен только план для исходящих вызовов, а другие хотели бы получить план с возможностью осуществления вызовов и отправки SMS. Эти планы можно выбрать при оформлении аренды телефонных номеров в Службах коммуникации Azure.

Доступные планы зависят от страны, в которой вы работаете, варианта использования и выбранного типа номера телефона. Планы могут отличаться в зависимости от страны, так как это определяется нормативными требованиями. Службы коммуникации Azure предоставляют следующие планы:

- **Только отправка SMS**. Этот план позволяет отправить пользователям SMS-сообщения. Он удобен для таких сценариев, как уведомления и оповещения при двухфакторной проверке подлинности. 
- **Отправка и получение SMS**. Этот план позволяет отправлять сообщения пользователям и получать сообщения от них с помощью телефонных номеров. Он удобен для сценариев обслуживания клиентов.
- **Только исходящие вызовы**. Этот план позволяет звонить пользователям и настраивать идентификатор вызывающей стороны для исходящих вызовов, выполняемых службой. Он удобен в сценариях обслуживания клиентов и голосовых уведомлений.

## <a name="countryregion-availability"></a>Доступность в различных регионах и странах

В следующей таблице показано, где можно получить телефонные номера различных типов, а также возможности для входящих и исходящих вызовов и SMS, связанных с этими типами телефонных номеров.

|Тип номера| Место получения номера | Расположения вызываемого абонента                                        | Расположения вызывающего абонента                                    |Расположения получателя сообщений       | Расположения отправителя сообщений |
|-----------| ------------------ | ---------------------------------------------------  |-------------------------------------------------------|-----------------------|--------|
| Местный (географический)  | США                 | США, Канада, Соединенное Королевство, Германия, Франция, США, а также другие*| США, Канада, Соединенное Королевство, Германия, Франция, США, а также другие* |Недоступно| Недоступно |
| Бесплатный номер | США                 | США                                                   | США                                                    |США                | США |

* Дополнительные сведения о местах назначения и ценах для вызовов см. на [странице цен](../pricing.md).

## <a name="azure-subscriptions-eligibility"></a>Поддержка в подписках Azure

Чтобы получить номер телефона, требуется платная подписка Azure. Для пробных учетных записей их получить нельзя. 

## <a name="next-steps"></a>Дальнейшие действия

### <a name="quickstarts"></a>Краткие руководства

- [Получение номера телефона](../../quickstarts/telephony-sms/get-phone-number.md)
- [Осуществление вызовов](../../quickstarts/voice-video-calling/calling-client-samples.md)
- [отправка SMS;](../../quickstarts/telephony-sms/send.md)

### <a name="conceptual-documentation"></a>Основная документация

- [Основные понятия о голосовой связи и видеосвязи](../voice-video-calling/about-call-types.md)
- [Потоки вызовов](../call-flows.md)
- [Цены](../pricing.md)
