---
title: Просмотр счета в рамках Соглашения с партнером Майкрософт — Azure
description: Узнайте, как просматривать счет и использование ресурсов, а затем проверять расходы по счету Соглашения с партнером Майкрософт.
author: bandersmsft
ms.reviewer: judupont
tags: billing
ms.service: cost-management-billing
ms.subservice: billing
ms.topic: tutorial
ms.date: 08/20/2020
ms.author: banders
ms.openlocfilehash: 9e5ecc67fe86afa15c66d175f0705818154588bf
ms.sourcegitcommit: 56cbd6d97cb52e61ceb6d3894abe1977713354d9
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/20/2020
ms.locfileid: "88684360"
---
# <a name="tutorial-review-your-microsoft-partner-agreement-invoice"></a>Руководство по Просмотр счета в рамках Соглашения с партнером Майкрософт

 В учетной записи выставления счетов Соглашения с партнером Майкрософт в каждом профиле выставления счетов ежемесячно создается счет. В счет входят все расходы клиента за предыдущий месяц. Вы можете интерпретировать расходы, включенные в счет, проанализировав отдельные транзакции на портале Azure. Вы можете также просмотреть счета на портале Azure и сравнить расходы в файле сведений об использовании.

Дополнительные сведения см. в статье [Просмотр и скачивание счета Microsoft Azure](download-azure-invoice.md).

Это руководство относится только к партнерам Azure с Соглашением с партнером Майкрософт.

В этом руководстве описано следующее:

> [!div class="checklist"]
> * Проверка выставленных счетов за транзакции на портале Azure
> * Просмотр ожидаемых расходов для прогнозирования следующего счета
> * Анализ расходов на использование Azure

## <a name="prerequisites"></a>Предварительные требования

Необходимо иметь доступ к учетной записи выставления счетов для Соглашения с партнером Майкрософт.

Должно пройти не менее 30 дней с момента подписки на Azure. Azure выставляет счет только в конце периода выставления счетов.

## <a name="sign-in-to-azure"></a>Вход в Azure

- Войдите на портал Azure по адресу [https://portal.azure.com](https://portal.azure.com).

## <a name="check-access-to-a-microsoft-customer-agreement"></a>Проверка доступа к Клиентскому соглашению Майкрософт

Проверьте тип соглашения, чтобы узнать, есть ли у вас доступ к учетной записи выставления счетов для Соглашения с партнером Майкрософт.

На портале Azure введите *cost management + billing* (Управление затратами и выставление счетов) в поле поиска, а затем выберите **Cost Management + Billing** (Управление затратами + выставление счетов).

![Снимок экрана, показывающий поиск на портале Azure](./media/review-partner-agreement-bill/billing-search-cost-management-billing.png)

Если у вас есть доступ только к одной области выставления счетов, выберите **Свойства** слева. Если тип учетной записи выставления счетов — **Соглашение с партнером Майкрософт**, то у вас есть доступ к учетной записи выставления счетов для Соглашения с партнером Майкрософт.

![Снимок экрана: страница свойств с Соглашением с партнером Майкрософт](./media/review-partner-agreement-bill/billing-account-properties-partner-agreement.png)

При наличии доступа к нескольким областям выставления счетов проверьте тип в столбце учетной записи выставления счетов. Если для всех областей в качестве типа учетных записей выставления счетов будет **Соглашение с партнером Майкрософт**, то у вас есть доступ к учетной записи выставления счетов для Соглашения с партнером Майкрософт.

![Снимок экрана: страница со списком учетных записей выставления счетов и Соглашением с партнером Майкрософт](./media/review-partner-agreement-bill/mpa-in-the-list.png)

## <a name="review-invoiced-transactions-in-the-azure-portal"></a>Проверка выставленных счетов за транзакции на портале Azure

В поле "Cost Management + Billing" (Управление затратами и выставление счетов) выберите **Все транзакции** в левой части страницы. В зависимости от уровня доступа вам, возможно, потребуется выбрать учетную запись выставления счетов, профиль выставления счетов или клиента, а затем **Все транзакции**.

На странице "Все транзакции" отображаются следующие сведения:

![Снимок экрана со списком транзакций, за которые выставлен счет](./media/review-partner-agreement-bill/all-transactions.png)

|Столбец  |Определение  |
|---------|---------|
|Дата     | Дата транзакции  |
|Идентификатор счета     | Идентификатор счета, за транзакцию по которому был выставлен счет. Если вы отправляете запрос на поддержку, предоставьте идентификатор службе поддержки Azure, чтобы ускорить обработку запроса на поддержку. |
|Тип транзакции     |  Тип транзакции, например покупка, отмена и расходы на использование  |
|Семейство продуктов     | Категория продукта, например вычислительные ресурсы для виртуальных машин или база данных для Базы данных SQL Azure.|
|Номер SDKU продукта     | Уникальный код, определяющий экземпляр продукта |
|Сумма     |  Сумма транзакции      |
|Профиль выставления счетов     | Эта транзакция отображается в этом профиле выставления счетов |

Выполните поиск по идентификатору счета, чтобы отфильтровать транзакции по счету.

## <a name="review-pending-charges-to-estimate-your-next-invoice"></a>Просмотр ожидаемых расходов для прогнозирования следующего счета

Расходы оцениваются и считаются ожидающими, пока не будет выставлен счет. Вы можете просмотреть ожидающие расходы для вашего профиля выставления счетов по Соглашению с партнером Майкрософт на портале Azure, чтобы оценить следующий счет. Ожидаемые расходы являются предполагаемыми и не включают налог. Фактические расходы в вашем следующем счете будут отличаться от ожидаемых.

### <a name="view-pending-transactions"></a>Просмотр ожидаемых транзакций

При определении ожидаемых расходов можно оценить расходы, проанализировав отдельные транзакции, которые добавлены к расходам. На этом этапе ожидаемые расходы на использование не отображаются на странице "Все транзакции". Ожидаемые расходы на использование можно просмотреть на странице подписок Azure.

В поле "Cost Management + Billing" (Управление затратами и выставление счетов) выберите профиль выставления счетов. В зависимости от уровня вашего доступа вам, возможно, потребуется выбрать учетную запись выставления счетов. В учетной записи выставления счетов щелкните **Профили выставления счетов** и выберите профиль выставления счетов.

Выберите **Все транзакции** в левой части страницы.

Выполните поиск по фразе *ожидаемые*. Используйте фильтр **Временной диапазон**, чтобы просмотреть ожидаемые расходы за текущий или прошлый месяцы.

<!-- ![Screenshot that shows the pending transactions list](./media/billing-mpa-understand-your-bill/mpa-billing-profile-pending-transactions.png) -->

### <a name="view-pending-charges-by-customer"></a>Просмотр ожидаемых расходов по клиенту

В поле "Cost Management + Billing" (Управление затратами и выставление счетов) выберите профиль выставления счетов. В зависимости от уровня вашего доступа вам, возможно, потребуется выбрать учетную запись выставления счетов. В учетной записи выставления счетов щелкните **Профили выставления счетов** и выберите профиль выставления счетов.

Выберите **Клиенты** в левой части страницы.

<!-- ![screenshot of billing profile customers list](./media/billing-mpa-understand-your-bill/mpa-billing-profile-customers.png) -->

На странице "Клиенты" отображаются текущие платежи и платежи за прошлый месяц для каждого клиента, связанного с профилем выставления счетов. Расходы за текущий месяц являются ожидаемыми и счета по ним выставляются при создании счета за месяц. Если счет за прошлый месяц еще не создан, расходы за прошлый месяц также являются ожидаемыми.

### <a name="view-pending-usage-charges"></a>Просмотр ожидаемых расходов на использование

В поле "Cost Management + Billing" (Управление затратами и выставление счетов) выберите профиль выставления счетов. В зависимости от уровня вашего доступа вам, возможно, потребуется выбрать учетную запись выставления счетов. В учетной записи выставления счетов щелкните **Профили выставления счетов** и выберите профиль выставления счетов.

Выберите **Подписки Azure** в левой части страницы. На странице подписок Azure для каждой подписки в профиле выставления счетов отображаются расходы за текущий и последний месяцы. Расходы за текущий месяц являются ожидаемыми и счета по ним выставляются при создании счета за месяц. Если счет за прошлый месяц еще не создан, расходы за прошлый месяц также являются ожидаемыми.

<!--     ![Screenshot that shows the Azure subscriptions list for MPA billing profile](./media/billing-mpa-understand-your-bill/mpa-billing-profile-subscriptions-list.png) -->

## <a name="analyze-your-azure-usage-charges"></a>Анализ расходов на использование Azure

Для анализа расходов на использование воспользуйтесь CSV-файлом с данными об использовании и расходах Azure. Вы можете отфильтровать файл с данными об использовании и расходах Azure, чтобы сверить расходы за использование каждого продукта, указанного в PDF-документе счета. Чтобы просмотреть подробные расходы на использование конкретного продукта, отфильтруйте столбец **product** в CSV-файле использования и расходов Azure, используя только имя этого продукта.

В CSV-файле использования и расходов Azure можно также отфильтровать столбец **customerName**, чтобы просмотреть расходы на ежедневное использование для каждого из клиентов. Если вы хотите просмотреть плату за ежедневное использование по подписке Azure, отфильтруйте столбец **subscriptionName**.

## <a name="pay-your-bill"></a>Оплата счета

Инструкции по оплате счета приведены в нижней части счета. Вы можете платить чеком или банковским переводом в течение 60 дней с даты выставления счета.

Если вы уже оплатили счет, то можете проверить состояние платежа на странице "Счета" на портале Azure.

## <a name="next-steps"></a>Дальнейшие действия

В этом руководстве вы узнали, как выполнять следующие задачи:

> [!div class="checklist"]
> * Проверка выставленных счетов за транзакции на портале Azure
> * Просмотр ожидаемых расходов для прогнозирования следующего счета
> * Анализ расходов на использование Azure

Сведения об использовании службы "Управление затратами Azure" для партнеров.

> [!div class="nextstepaction"]
> [Начало работы со службой "Управление затратами Azure" для партнеров](../costs/get-started-partners.md).
