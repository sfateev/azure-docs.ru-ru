---
title: Добавление аудитории предварительного просмотра для предложения SaaS в коммерческом магазине Майкрософт
description: Добавление аудитории предварительного просмотра для предложения SaaS в центре партнеров Майкрософт.
author: mingshen-ms
ms.author: mingshen
ms.reviewer: dannyevers
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: how-to
ms.date: 09/02/2020
ms.openlocfilehash: ed0c7ef98e70774350c9a3ff12b0cc3f4186e1bb
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2020
ms.locfileid: "89381191"
---
# <a name="how-to-add-a-preview-audience-for-your-saas-offer"></a>Как добавить аудиторию предварительного просмотра для предложения SaaS

По мере создания предложения SaaS (программное обеспечение как услуга) в центре партнеров необходимо определить аудиторию предварительной версии, которая сможет просмотреть список предлагаемых вам предложений. В этой статье объясняется, как настроить аудиторию предварительной версии.

> [!NOTE]
> Если вы решите обрабатывать транзакции независимо, этот параметр отображаться не будет. Вместо этого перейдите к [способу продвижения предложения SaaS](create-new-saas-offer-marketing.md).

## <a name="define-a-preview-audience"></a>Определение аудитории для предварительного просмотра

На вкладке **Предварительная версия аудитории** можно определить ограниченную аудиторию, которая может просмотреть предложение SaaS, прежде чем публиковать его в более широкой аудитории Marketplace. Вы можете отправлять приглашения в адреса электронной почты учетных записей Майкрософт (MSA) или Azure Active Directory (Azure AD). Добавьте не менее одного и 10 адресов электронной почты вручную или импортируйте до 20 с CSV-файлом. Список аудиторий предварительной версии можно обновить в любое время.

> [!NOTE]
> Аудитория предварительной версии отличается от частной аудитории. Аудитории предварительной версии предоставляется доступ к вашему предложению до его публикации в Интернет-магазинах. Вы также можете создать план и сделать его доступным только для частной аудитории. Частные планы описаны в разделах [Создание планов для предложения SaaS](create-new-saas-offer-plans.md).

### <a name="add-email-addresses-manually"></a>Добавить адреса электронной почты вручную

1. На странице **Предварительный просмотр аудитории** добавьте один адрес электронной почты Azure AD или MSA и необязательное описание в соответствующих полях.
1. Чтобы добавить другой адрес электронной почты, выберите ссылку **добавить другую электронную почту** .
1. Выберите **Сохранить черновик** , прежде чем перейти к следующей вкладке: **Техническая конфигурация**.
1. Продолжайте [добавлять технические сведения о предложении SaaS](create-new-saas-offer-technical.md).

### <a name="add-email-addresses-using-the-csv-file"></a>Добавление адресов электронной почты с помощью CSV-файла

1. На странице **Предварительный просмотр аудитории** выберите ссылку **экспортировать аудиторию (CSV)** .
1. Откройте. CSV-файл в приложении, например Microsoft Excel.
1. В. CSV-файл в столбце **идентификатор** введите адреса электронной почты, которые вы хотите добавить в аудиторию предварительного просмотра.
1. В столбце **Описание** при необходимости можно добавить описание для каждого адреса электронной почты.
1. В столбце **тип** добавьте **микседаадмсаид** в каждую строку с адресом электронной почты.
1. Сохраните файл как. CSV-файл.
1. На странице **Предварительный просмотр аудитории** выберите ссылку **Импорт аудитории (CSV)** .
1. В диалоговом окне **Подтверждение** выберите **Да**.
1. Выберите. CSV-файл и нажмите кнопку **Открыть**.
1. Выберите **Сохранить черновик** , прежде чем перейти к следующей вкладке: **Техническая конфигурация**.

## <a name="next-steps"></a>Дальнейшие шаги

- [Как добавить технические сведения о предложении SaaS](create-new-saas-offer-technical.md)
