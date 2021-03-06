---
title: Аналитика Marketplace — Microsoft для коммерческого рынка, Microsoft AppSource и Azure Marketplace
description: Получите доступ к сводной веб-аналитике Marketplace, которая позволяет измерять вовлеченность клиентов в Microsoft AppSource и Azure Marketplace.
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: article
ms.date: 07/22/2019
author: mingshen-ms
ms.author: mingshen
ms.openlocfilehash: 1a645a333db9b24005639f4adbb2913a2b887b66
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2020
ms.locfileid: "89055674"
---
# <a name="marketplace-insights-dashboard-in-partner-center"></a>Панель мониторинга "Аналитика Marketplace" в Центре партнеров

Эта статья содержит сведения о панели мониторинга "Аналитика Marketplace" в Центре партнеров. На этой панели мониторинга отображается сводка веб-аналитики Marketplace, которая позволяет издателям измерять взаимодействия клиентов с соответствующими страницами сведений о продуктах, указанными в Интернет-магазинах коммерческого рынка: Microsoft AppSource и Azure Marketplace.

## <a name="marketplace-insights-dashboard"></a>Панель мониторинга аналитики Marketplace

Чтобы получить доступ к панели мониторинга **Аналитика Marketplace** в Центре партнеров, откройте вкладку **[Анализ](https://partner.microsoft.com/dashboard/commercial-marketplace/analytics/summary)** в коммерческой платформе.

Можно просматривать следующие графические представления:  

- [Сводка по аналитике Marketplace](#marketplace-insights-summary)
- [Посещения страниц по географическому расположению](#page-visits-by-geography)  
- [Тренд посещений страниц по отношению к уникальным посетителям](#page-visits-versus-unique-visitors-trend)
- [Вызов действия (ПНА) и уникальных посетителей с помощью CTAs](#call-to-action-versus-unique-visitors-with-ctas)
- [Посещения страниц и призыв к действию по предложениям](#page-visits-and-calls-to-action-by-offers)
- [Тренд процентного показателя призыва к действию](#call-to-action-percentage-trend)
- [Посещения страниц и призыв к действию по реферальным доменам](#page-visits-and-calls-to-action-by-referral-domains)
- [Подробная таблица аналитики Marketplace](#marketplace-insights-details-table)

Максимальная задержка между пользователями, посещаемыми в Azure Marketplace или AppSource, и отчетами в центре партнеров составляет 48 часов.

>[!NOTE]
> Подробное описание терминологии аналитики см. в [часто задаваемых вопросах и терминологии по аналитике коммерческой платформы](./faq-terminology.md).

### <a name="insights-dashboard-layout"></a>Макет панели мониторинга аналитики

Просмотр метрик коммерческого рынка различными способами.

- Вкладки Интернет-магазина
- Фильтры страниц
- Фильтры по дате

**Вкладки Интернет-магазина**. метрики предложений можно просмотреть отдельно через вкладки AppSource & Azure Marketplace. Выберите предложения из раскрывающегося списка предложений справа, чтобы просмотреть метрики для выбранных предложений. По умолчанию выбраны все предложения.

![Раскрывающийся список панели мониторинга аналитики Центра партнеров](./media/insights-offer-dropdown.png)

**Фильтры на странице аналитики**. Эти фильтры применяются на уровне предложения (страницы сведений о продукте). Можно выбрать несколько фильтров по критериям, которые вы хотите просмотреть. Этот фильтр применяется ко всему разделу "Аналитика Marketplace", включая диаграммы и подробные сведения.

![Фильтры панели мониторинга аналитики Центра партнеров](./media/insights-page-filter.png)

- Отображаются только те имена предложений, у которых есть посещения страниц в выбранном диапазоне дат.  
- По умолчанию для каждого из параметров фильтра выбрано значение "Все".
- Примененные фильтры показывают количество выбранных вариантов. Примененные фильтры не отображаются при выбранном значении по умолчанию "Все".

![Примененные фильтры аналитики Центра партнеров](./media/insights-page-filter-two.png)

**Фильтры аналитики по дате**. Этот фильтр применяется ко всему разделу "Аналитика Marketplace". Фильтры могут включать заранее определенные или пользовательские диапазоны дат.

![Фильтры по дате аналитики Центра партнеров](./media/insights-date-range.png)

## <a name="marketplace-insights-summary"></a>Сводка по аналитике Marketplace

В разделе "Сводка" в Marketplace Insights отображается количество **посещений страниц**, **вызовов действий**и **уникальных посетителей** для выбранного диапазона дат.

>[!NOTE]
>Информационная панель Marketplace Insights предоставляет данные навигации, которые не должны быть связаны с интересами, созданными в конечной точке назначения интереса.

### <a name="page-visits"></a>Посещения страниц

Это число представляет количество различных пользовательских сеансов на странице предложения (страница сведений о продукте) для выбранного диапазона дат. Красный или зеленый индикаторы информируют о процентном росте посещений. Диаграмма трендов представляет число посещений страниц по месяцам.

### <a name="unique-visitors"></a>Уникальные посетители

Это число представляет количество различных посетителей в течение выбранного диапазона дат для предложений, выбранных в фильтре страницы. Посетитель, который посещает одну или несколько страниц сведений о продукте, считается одним уникальным посетителем.

### <a name="call-to-action"></a>Призыв к действию

Это число представляет количество нажатий кнопки **призыва к действию** на странице предложения (странице сведений о продукте). **Вызовы действия** подсчитываются, когда пользователь выбирает кнопки **получить сейчас**, **Бесплатная пробная версия**, **связаться со мной**или **тестовый диск** .

![Сводка по призывам к действию в аналитике Центре партнеров](./media/insights-summary.png)

## <a name="page-visits-by-geography"></a>Посещения страниц по географическому расположению

На тепловой карте ниже показано **количество посещений страниц**, **призывов к действию** и **уникальных посетителей в соответствии со страной клиента**. Большие количества посещений страницы представлены на карте темными цветами, а меньшие — светлыми.

![Географическое распределение в аналитике Центра партнеров](./media/insights-geography.png)

Тепловая карта включает следующие возможности.

- Тепловая карта имеет дополнительную сетку для просмотра сведений о **посещениях страниц**, **призывах к действию** и **уникальных посетителях** в определенном расположении. При необходимости можно приблизить определенное расположение.  
- **Распределение по странам или регионам** — это показатели для всех стран или регионов, из которых были зарегистрированы посещения страниц клиентами в выбранном диапазоне дат.
- Можно выполнить поиск и выбрать страну в сетке, чтобы приблизить расположение на карте. Нажмите кнопку **Домой** на карте, чтобы вернуть исходный вид.

## <a name="page-visits-versus-unique-visitors-trend"></a>Тренд посещений страниц по отношению к уникальным посетителям

Столбцы ниже представляют число ежемесячных посещений страниц, отображаемых по оси Y (ось в левой части диаграммы). Линия тренда представляет собой месячную тенденцию уникальных посетителей, которая отображается на вспомогательной оси Y (оси в правой части диаграммы) для ваших предложений, опубликованных в Интернет-магазинах: Azure Marketplace и AppSource.

![Тренд посещений страниц по отношению к уникальным посетителям в аналитике Центра партнеров](./media/insights-page-vists-unique-visitors.png)

## <a name="call-to-action-versus-unique-visitors-with-ctas"></a>Призыв к действию по отношению к уникальным посетителям, пришедшим по призыву

Столбцы с накоплением представляют ежемесячные призывы к действию (CTA), разбитые по типам CTA (**Получить сейчас**, **Обратный звонок**и **Попробовать бесплатно**) и отображаются по оси Y (ось в левой части страницы). Линия тренда представляет помесячный тренд уникальных посетителей по CTA и отображается на вспомогательной оси Y (ось в правой части диаграммы) для предложений, опубликованных в Azure Marketplace и AppSource.

![Призыв к действию по отношению к уникальным посетителям, пришедшим по призыву в аналитике Центра партнеров](./media/insights-call-to-action-unique-visitors.png)

## <a name="page-visits-and-calls-to-action-by-offers"></a>Посещения страниц и призыв к действию по предложениям

Внешняя круговая диаграмма представляет разбиение **посещений страниц** на основе предложений, опубликованных в магазине и выбранных в фильтре. Внутренняя диаграмма представляет разбиение **призывов к действию** по тем же предложениям.

![Посещения страниц и призыв к действию по предложениям в аналитике Центра партнеров](./media/insights-page-visits-and-cta-by-offer.png)

## <a name="call-to-action-percentage-trend"></a>Тренд процентного показателя призыва к действию

**Тренд процентного показателя призыва к действию** представляет CTA в процентах для предложений, опубликованных в магазине. CTA % = (количество CTA/посещения страницы) * 100.

![Тренд процентного показателя призыва к действию в аналитике Центре партнеров](./media/insights-call-to-action-percentage-trend.png)

## <a name="page-visits-and-calls-to-action-by-referral-domains"></a>Посещения страниц и призыв к действию по реферальным доменам

На приведенном ниже графике представлены 50 основных реферальных доменов. При выборе конкретного реферального домена на диаграмме справа отображается помесячный тренд посещений страницы и призывов к действию.

![Посещения страниц и призыв к действию по реферальным доменам и кампаниям в аналитике Центра партнеров](./media/insights-page-visits-call-to-actions.png)

## <a name="marketplace-insights-details-table"></a>Подробная таблица аналитики Marketplace

В этой таблице представлен список посещений страниц и призывов к действию выбранных предложений, отсортированных по дате.

![Подробная таблица аналитики Центра партнеров](./media/insights-details-page.png)

- Данные можно извлечь в CSV-файл, если число записей меньше 1000.
- Если число записей превышает 1000, экспортированные данные будут асинхронно помещены на страницу загрузки в течение следующих 30 дней.
- Отфильтруйте данные по именам предложений и названиям кампаний, чтобы отобразить нужные данные.

## <a name="next-steps"></a>Дальнейшие действия

- Общие сведения об аналитических отчетах, доступных в коммерческой платформе Центра партнеров, см. в статье [Аналитика коммерческой платформы в Центре партнеров](./analytics.md).
- Сведения о диаграммах, трендах и значениях статистических данных, суммирующих действия платформы для вашего предложения, см. в статье [Сводная панель мониторинга в аналитике коммерческой платформы](./summary-dashboard.md).
- Сведения о заказах в графическом и загружаемом формате см. в статье [Панель мониторинга заказов в аналитике коммерческой платформы](./orders-dashboard.md).
- Сведения об использовании предложений и метриках для выставления счетов для виртуальных машин см. в статье [Панель мониторинга использования в аналитике коммерческой платформы](./usage-dashboard.md).
- Подробные сведения о клиентах, в том числе тренды роста, см. в статье [Панель мониторинга клиентов в аналитике коммерческой платформы](./customer-dashboard.md).
- Список запросов на загрузку за последние 30 дней см. в статье [Панель мониторинга загрузок в аналитике коммерческой платформы](./downloads-dashboard.md).
- Сведения о просмотре объединенного представления отзывов клиентов для предложений в Azure Marketplace и AppSource см. в статье [Оценки и отзывы в аналитике коммерческой платформы](./ratings-reviews.md).
- Вопросы и ответы об аналитике коммерческой платформы и подробный словарь терминов данных см. в статье [Часто задаваемые вопросы и терминология по аналитике коммерческой платформы](./faq-terminology.md).
