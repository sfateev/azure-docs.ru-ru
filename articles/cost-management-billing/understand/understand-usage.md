---
title: Расшифровка подробных данных об использовании и расходах
description: Узнайте, как правильно расшифровать подробные сведения в файлах с данными об использовании и расходах. Просмотрите список терминов и описаний, используемых в файле.
author: bandersmsft
ms.reviewer: micflan
tags: billing
ms.service: cost-management-billing
ms.subservice: billing
ms.topic: conceptual
ms.date: 08/20/2020
ms.author: banders
ms.openlocfilehash: d113ad7d3de3478fbbdcce32363e048b7a8a75ce
ms.sourcegitcommit: 56cbd6d97cb52e61ceb6d3894abe1977713354d9
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/20/2020
ms.locfileid: "88681742"
---
# <a name="understand-the-terms-in-your-azure-usage-and-charges-file"></a>Общие сведения об условиях в файле сведений об использовании и расходах Azure

Файл подробных сведений об использовании и расходах содержит сведения о ежедневном тарифицированном использовании на основе согласованных тарифов, покупок (например, резервирования, плата за Marketplace) и возвратов денежных средств за указанный период.
В плату не включены кредиты, налоги, другие расходы или скидки.
В следующей таблице описано, какие расходы предусмотрены в каждом типе учетной записи.

Тип учетной записи | Использование Azure | Использование Marketplace | Покупки | Возвраты платежей
--- | --- | --- | --- | ---
Соглашение Enterprise (EA) | Да | Да | Да | Нет
Клиентское соглашение Майкрософт (MCA) | Да | Да | Да | Да
Оплата по мере использования | Да | Да | нет | Нет

Дополнительные сведения о заказах Marketplace (также называются внешними службами) см. в статье [Understand your Azure external services charges](understand-azure-marketplace-charges.md) (Общие сведения о расходах на внешние службы Azure).

Дополнительные сведения о скачивании инструкций см. в статье [Download or view your Azure billing invoice and daily usage data](../manage/download-azure-invoice-daily-usage-date.md) (Скачивание или просмотр счета на оплату и данных о ежедневном использовании в Azure).
Вы можете открыть CSV-файл со сведениями об использовании и расходах в Microsoft Excel или другом приложении электронных таблиц.

## <a name="list-of-terms-and-descriptions"></a>Список терминов и описаний

В следующей таблице описаны важные термины, используемые в последней версии файла со сведениями об использовании и расходах Azure.
Список содержит учетные записи с оплатой по мере использования (PAYG), Соглашения Enterprise (EA) и клиентского соглашения Майкрософт (MCA).

Термин | Тип учетной записи | Описание
--- | --- | ---
AccountName | EA, PAYG | Отображаемое имя учетной записи регистрации EA или учетной записи выставления счетов PAYG.
AccountOwnerId<sup>1</sup> | EA, PAYG | Уникальный идентификатор учетной записи регистрации EA или учетной записи выставления счетов PAYG.
AdditionalInfo | All | Метаданные определенных служб. Например, тип образа для виртуальной машины.
BillingAccountId<sup>1</sup> | All | Уникальный идентификатор для корневой учетной записи выставления счетов.
BillingAccountName | All | Имя учетной записи выставления счетов.
BillingCurrency | All | Валюта, связанная с учетной записью выставления счетов.
BillingPeriod | EA, PAYG | Период выставления счетов взимаемой платы.
BillingPeriodEndDate | All | Дата окончания периода выставления счетов.
BillingPeriodStartDate | All | Дата начала периода выставления счетов.
BillingProfileId<sup>1</sup> | All | Уникальный идентификатор регистрации EA, подписки PAYG, профиля выставления счетов MCA или консолидированной учетной записи AWS.
BillingProfileName | All | Имя регистрации EA, подписки PAYG, профиля выставления счетов MCA или консолидированной учетной записи AWS.
ChargeType | All | Указывает, что представляет собой взимаемая плата: использование (**Использование**), покупку (**Покупка**) или возврат денежных средств (**Возврат**).
ConsumedService | All | Имя службы, с которой связаны расходы.
CostCenter<sup>1</sup> | EA, MCA | Место возникновения затрат, определенное в подписке для отслеживания затрат (доступно только в открытых периодах выставления счетов для учетных записей MCA)
Стоимость | EA, PAYG | См. CostInBillingCurrency.
CostInBillingCurrency | MCA | Взимаемая плата в валюте выставления счетов до кредитов или налогов.
CostInPricingCurrency | MCA | Взимаемая плата в валюте цены до кредитов или налогов.
Валюта | EA, PAYG | См. BillingCurrency.
Date<sup>1</sup> | All | Дата использования или покупки для взимаемой платы.
EffectivePrice | All | Смешанная цена за единицу за период. В смешанных ценах усреднены любые колебания цены за единицу, например при распределении по уровням, что приводит к снижению цены при увеличении количества с течением времени.
ExchangeRateDate | MCA | Дата установления валютного курса.
ExchangeRatePricingToBilling | MCA | Валютный курс, используемый для преобразования стоимости в валюте цены в валюту выставления счетов.
Частота | All | Указывает, ожидается ли повторная оплата. Плата может взиматься один раз (**OneTime**), повторяться ежемесячно или ежегодно (**Recurring**) или основываться на использовании (**UsageBased**).
InvoiceId | PAYG, MCA | Уникальный идентификатор документа, указанный в PDF-файле счета.
InvoiceSection | MCA | См. InvoiceSectionName.
InvoiceSectionId<sup>1</sup> | EA, MCA | Уникальный идентификатор для отдела EA или раздела счета MCA.
InvoiceSectionName | EA, MCA | Имя отдела EA или раздела счета MCA.
IsAzureCreditEligible | All | Указывает, можно ли оплачивать расходы с помощью денег на счете в Azure (значения: True, False).
Расположение | MCA | Расположение центра обработки данных, где выполняется ресурс.
MeterCategory | All | Имя категории классификации для единицы измерения. Например, *Облачные службы* и *Сеть*.
MeterId<sup>1</sup> | All | Уникальный идентификатор единицы измерения.
MeterName | All | Имя единицы измерения.
MeterRegion | All | Имя расположения центра обработки данных для служб, цены на которые определяются на основе расположения. См. Location.
MeterSubCategory | All | Имя категории подклассификации единицы измерения.
OfferId<sup>1</sup> | All | Название приобретенного предложения.
PayGPrice | All | Розничная цена на ресурс.
PartNumber<sup>1</sup> | EA, PAYG | Идентификатор, используемый для получения цен в конкретной единице измерения.
PlanName | EA, PAYG | Имя плана Marketplace.
PreviousInvoiceId | MCA | Ссылка на исходный счет, если этот элемент строки является возвратом денежных средств.
PricingCurrency | MCA | Валюта, используемая при оценке на основе согласованных цен.
PricingModel | All | Идентификатор, указывающий на способ тарификации за единицу измерения. (Значения: On Demand, Reservation, Spot)
Продукт | All | Имя продукта.
ProductId<sup>1</sup> | MCA | Уникальный идентификатор продукта.
ProductOrderId | All | Уникальный идентификатор заказа продукта.
ProductOrderName | All | Уникальное имя заказа продукта.
PublisherName | All | Издатель для служб Marketplace.
PublisherType | All | Тип издателя (значения: **Azure**, **AWS**, **Marketplace**).
Количество | All | Количество приобретенных или израсходованных единиц.
Идентификатор резервирования | EA, MCA | Уникальный идентификатор для приобретенного экземпляра резервирования.
ReservationName | EA, MCA | Имя приобретенного экземпляра резервирования.
ResourceGroup | All | Имя [группы ресурсов](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview), в которой находится ресурс. Не все расходы связаны с ресурсами, развернутыми в группах ресурсов. Расходы, для которых нет группы ресурсов, будут отображаться со значением NULL, "Пусто", **Другие** или **Неприменимо**.
ResourceId<sup>1</sup> | All | Уникальный идентификатор ресурса [Azure Resource Manager](https://docs.microsoft.com/rest/api/resources/resources).
ResourceLocation | All | Расположение центра обработки данных, где выполняется ресурс. См. Location.
ResourceName | EA, PAYG | Имя ресурса. Не все расходы связаны с развернутыми ресурсами. Расходы, для которых не указан тип ресурса, будут отображаться со значением NULL, "Пусто", **Другие** или **Неприменимо**.
ResourceType | MCA | Тип экземпляра ресурса. Не все расходы связаны с развернутыми ресурсами. Расходы, для которых не указан тип ресурса, будут отображаться со значением NULL, "Пусто", **Другие** или **Неприменимо**.
ServiceFamily | MCA | Семейство служб, к которому принадлежит служба.
ServiceInfo1 | All | Метаданные определенных служб.
ServiceInfo2 | All | Устаревшее поле с необязательными метаданными службы.
ServicePeriodEndDate | MCA | Дата окончания периода оценки, в которой определены и заблокированы цены для использованной или приобретенной службы.
ServicePeriodStartDate | MCA | Дата начала периода оценки, в которой определены и заблокированы цены для использованной или приобретенной службы.
SubscriptionId<sup>1</sup> | All | Уникальный идентификатор подписки Azure.
Параметр SubscriptionName | All | Имя подписки Azure.
Tags<sup>1</sup> | All | Теги, назначенные ресурсу. Сюда не входят теги группы ресурсов. Могут использоваться для группировки или распределения затрат на внутренний возвратный платеж. Дополнительные сведения см. в статье [Использование тегов для организации ресурсов в Azure](https://azure.microsoft.com/updates/organize-your-azure-resources-with-tags/).
Термин | All | Отображает срок действия предложения. Пример: В случае с зарезервированными экземплярами для срока действия отображается значение в 12 месяцев. Для однократных или повторяющихся покупок срок действия составляет 1 месяц (SaaS, поддержка Marketplace). Это не применяется к использованию Azure.
UnitOfMeasure | All | Единица измерения выставления счетов для службы. Например, счета за службы вычислений выставляются на почасовой основе.
UnitPrice | EA, PAYG | Цена за единицу для взимаемой платы.

_<sup>**1**</sup> Поля, используемые для создания уникального идентификатора для отдельной записи о затратах._

Обратите внимание, что в разных типах учетных записей некоторые поля могут отличаться по регистру и интервалам.
В старых версиях файлов с оплатой по мере использования есть отдельные разделы для оператора и сведений о ежедневном использовании.

### <a name="list-of-terms-from-older-apis"></a>Список терминов из старых API-интерфейсов
В следующей таблице термины, используемые в старых API-интерфейсах, сопоставляются с новыми терминами. Эти описания приведены в таблице выше.

Старый термин | Новый термин
--- | ---
ConsumedQuantity | Количество
includedQuantity | Недоступно
InstanceId | ResourceId
Тариф | EffectivePrice
Единицы | UnitOfMeasure
UsageDate | Дата
UsageEnd | Дата
UsageStart | Дата


## <a name="ensure-charges-are-correct"></a>Убедитесь, что взимаемая плата — правильная

Узнайте больше о получении подробных данных об использовании и расходах, а также о том, как выставляются счета [с оплатой по мере использования](review-individual-bill.md) или в рамках [клиентского соглашения Майкрософт](review-customer-agreement-bill.md).

## <a name="need-help-contact-us"></a>Требуется помощь? Свяжитесь с нами.

Если у вас есть вопросы или вам нужна помощь, [создайте запрос в службу поддержки](https://go.microsoft.com/fwlink/?linkid=2083458).

## <a name="next-steps"></a>Дальнейшие действия

- [Просмотр и скачивание счета Microsoft Azure](download-azure-invoice.md)
- [Скачивание или просмотр счета на оплату и данных о ежедневном использовании в Azure](download-azure-daily-usage.md)
