---
title: Создание предложения консультационных услуг на коммерческой платформе Майкрософт
description: Узнайте, как опубликовать предложение консультационных услуг в Microsoft AppSource или Azure Marketplace с помощью Центра партнеров.
author: Microsoft-BradleyWright
ms.author: brwrigh
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: how-to
ms.date: 06/17/2020
ms.openlocfilehash: 464e75e55bc67ce619134be01ba00f2606a271a4
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2020
ms.locfileid: "91709076"
---
# <a name="create-a-consulting-service-offer"></a>Создание предложения консультационных услуг

В этой статье описано, как публиковать предложение консультационных услуг в [Microsoft AppSource](https://appsource.microsoft.com/) или [Azure Marketplace](https://azuremarketplace.microsoft.com/). Вы сможете разместить предложение консультационных услуг на основе Microsoft [Dynamics 365](https://dynamics.microsoft.com/) и Power Platform в AppSource, а также размещение предложений услуг на основе Microsoft Azure в Azure Marketplace. Сначала [создайте учетную запись на коммерческой платформе в Центре партнеров](create-account.md), если вы еще этого не сделали. Убедитесь, что ваша учетная запись зарегистрирована в программе коммерческой платформы.

Перед созданием предложения ознакомьтесь со статьей [Предварительные требования к консультационным услугам](consulting-service-prerequisites.md).

## <a name="publishing-benefits"></a>Преимущества публикации

Преимущества публикации на коммерческой платформе:

- Продвижение вашей компании за счет использования бренда Майкрософт.
- Возможно достижение более 100 000 000 Microsoft 365 и Dynamics 365 пользователей в AppSource и более 200 000 организаций через Azure Marketplace.
- Получение высококачественных потенциальных клиентов на этих коммерческих платформах.
- Продвижение ваших услуг с привлечением специалистов по продажам по телефону и реализации Майкрософт.

## <a name="create-a-new-offer"></a>Создание нового предложения

1. Войдите в [Центр партнеров](https://partner.microsoft.com/dashboard/home).
2. В меню слева выберите **Коммерческий Marketplace** > **Обзор**.
3. На странице "Обзор" выберите **+ Новое предложение** > **Консультационная служба**.

    ![Показано меню навигации слева.](./media/new-offer-consulting-service.png)

>[!NOTE]
>После публикации предложения изменения, внесенные в него в центре партнеров, отображаются только в Интернет-магазинах после повторной публикации предложения. Если вы внесли изменения, обязательно выполните повторную публикацию.

## <a name="new-offer"></a>Новое предложение

Укажите **идентификатор предложения**. У каждого предложения в вашей учетной записи должен быть уникальный идентификатор.

- Этот идентификатор отображается клиентам в строке веб-адреса предложения в Marketplace.
- Вы можете использовать только строчные буквы и цифры. Идентификатор может содержать дефисы и символы подчеркивания, но не пробелы, и может включать не больше 50 символов. Например, если вы введете **test-offer-1**, веб-адрес предложения будет иметь следующий вид: `https://azuremarketplace.microsoft.com/marketplace/../test-offer-1`.
- Идентификатор предложения нельзя изменить после выбора команды **Создать**.

Заполните поле **Псевдоним предложения**. Это имя, которое используется для предложения в Центре партнеров.

- Это имя не используется в Marketplace и отличается от имени предложения и других значений, отображаемых для клиентов.
- Псевдоним предложения нельзя изменить после выбора команды **Создать**.

Выберите **Создать**, чтобы создать предложение и продолжить.

## <a name="offer-setup"></a>Настройка предложения

### <a name="connect-lead-management"></a>Подключение к системе управления потенциальными клиентами

При публикации предложения на Marketplace с помощью Центра партнеров его _нужно_ подключить к системе управления отношениями с клиентами (CRM) или к системе автоматизации маркетинга. Это позволит вам немедленно получать контактные данные всех, кто заинтересуется или воспользуется вашим продуктом.

1. Выберите **Подключить**, чтобы указать, куда будут отправляться данные о потенциальных клиентах. Центр партнеров поддерживает следующие системы:

    - [Dynamics 365](commercial-marketplace-lead-management-instructions-dynamics.md) для плана Customer Engagement
    - [Marketo](commercial-marketplace-lead-management-instructions-marketo.md)
    - [Salesforce](commercial-marketplace-lead-management-instructions-salesforce.md)

    > [!NOTE]
    > Если ваша система CRM не указана выше, используйте [таблицу Azure](commercial-marketplace-lead-management-instructions-azure-table.md) или [конечные точки HTTPS](commercial-marketplace-lead-management-instructions-https.md) для хранения данных о потенциальных клиентах, а затем экспортируйте их в систему CRM.

2. При публикации предложения в Центре партнеров подключите свое предложение к месту назначения данных о потенциальных клиентах.
3. Убедитесь, что подключение к месту назначения данных о потенциальных клиентах настроено правильно. После публикации в Центре партнеров мы проверим подключение и отправим вам данные тестового потенциального клиента. На этапе предварительной версии предложения перед его публикацией вы также можете проверить подключение для передачи данных о потенциальных клиентах. Для этого попробуйте приобрести предложение самостоятельно в предварительной версии среды.
4. Чтобы не упустить потенциальных клиентов, убедитесь, что подключение к месту назначения данных о них обновляется.

Ниже приведены дополнительные ресурсы по управлению потенциальными клиентами.

- [Потенциальные клиенты из предложения на коммерческой платформе](commercial-marketplace-get-customer-leads.md)
- [Часто задаваемые вопросы об управлении потенциальными клиентами](../lead-management-faq.md#common-questions-about-lead-management)
- [Устранение ошибок конфигурации интересов](../lead-management-faq.md#publishing-config-errors)
- [Общие сведения об управлении потенциальными клиентами](https://assetsprod.microsoft.com/mpn/cloud-marketplace-lead-management.pdf) (PDF-документ; убедитесь, что блокирование всплывающих окон отключено)

Чтобы продолжить, выберите **Сохранить черновик**.

### <a name="properties"></a>Свойства

На этой странице можно задать основной продукт, на который рассчитана ваша консультационная услуга, задать тип консультационной услуги и выбрать применимые продукты.

1. Выберите **Основной продукт** из раскрывающегося списка.
2. Выберите **Тип консультационных услуг** из раскрывающегося списка:

    - **Оценка**. Анализ среды клиента для определения применимости решения и предоставления оценки затрат и времени.
    - **Совещание**. Представление решения или консультационной услуги для привлечения клиента с использованием схем, демонстрационных материалов и историй клиентов.
    - **Реализация**. Полная установка, предоставляющая клиенту полностью рабочее решение. Ограничьтесь решениями, которые можно реализовать не более чем за две недели.
    - **Подтверждение концепции**. Ограниченная область реализации, позволяющая определить применимость решения к требованиям клиента.
    - **Семинар**. Интерактивная вовлеченность на территории клиента. Сюда может входить обучение, брифинги, оценки и демонстрации, основанные на данных или рабочей среде клиента.

3. Если вы выбрали **Azure** в качестве основного продукта, выберите до трех **областей решения**. Так клиенты быстрее найдут ваше предложение в Azure Marketplace. Если вы выбрали другой продукт, пропустите этот шаг.

    - Аналитика
    - Модернизации приложения
    - Архив
    - ИИ и машинное обучение
    - Резервное копирование
    - Данные большого размера
    - Платформа данных
    - Управление центром обработки данных
    - DevOps
    - Аварийное восстановление
    - Идентификация
    - Интернет вещей
    - Миграция
    - Сеть
    - Безопасность
    - Хранилище

1. Если вы выбрали **Azure** в качестве основного продукта, то можете выбрать до шести **отраслей**. Так клиенты быстрее найдут ваше предложение в Azure Marketplace. Полный список отраслей в [предложении](../gtm-offer-listing-best-practices.md)содержит рекомендации. Если вы не выбрали Azure, пропустите этот шаг.
1. Если вы выбрали *другой* основной продукт, задайте не более трех **применимых продуктов**. Так клиенты быстрее найдут ваше предложение в AppSource. Дополнительные сведения см. в документе [с рекомендациями по размещению консультационных услуг в Microsoft AppSource](https://go.microsoft.com/fwlink/?LinkId=828734&amp;clcid=0x409) (в формате PDF).
1. Если вы выбрали *основной продукт, отличный от* Azure, можно выбрать до двух **отраслей** и два **вертикальных** варианта для каждой отрасли. Так клиенты быстрее найдут ваше предложение в AppSource. Полный список отраслей и по вертикали см. в списке [предлагаемых рекомендаций](../gtm-offer-listing-best-practices.md).
1. Добавьте до трех **компетенций** своей компании для отображения вместе с предложением консультационных услуг. Необходимо указать по меньшей мере одну компетенцию (за исключением Azure Expert MSP и Azure Networking MSP).

Чтобы продолжить, выберите **Сохранить черновик**.

## <a name="offer-listing"></a>Размещение предложения

Здесь необходимо задать сведения о предложении, которые будут отображаться в Marketplace, в том числе название предложения, описание, изображения и т. п. При настройке предложения обязательно руководствуйтесь политиками, изложенными на [странице политик сертификации для коммерческой платформы](https://docs.microsoft.com/legal/marketplace/certification-policies#800-consulting-services).

> [!NOTE]
> Сведения о предложении не обязательно должны быть изложены на английском языке, если описание предложения начинается с фразы &quot;Это приложение доступно только на [язык, отличный от английского]&quot;. Кроме того, можно предоставить полезную ссылку на касающееся предложения содержимое на языке, отличном от того, который используется в его описании.

Ниже приведен пример того, как в Azure Marketplace отображаются сведения о предложении (любые указанные цены приведены только в качестве примера и не должны отражать фактические затраты):

:::image type="content" source="media/example-consulting-service-offer.png" alt-text="Показывает, как это предложение отображается в Azure Marketplace.":::

#### <a name="call-out-descriptions"></a>Описания вызова

1. Эмблема
2. Цена
3. Области решения
4. Отрасли
5. Название предложения
6. Итоги
7. Описание
8. Снимки экрана и видео

<br>Ниже приведен пример того, как отображаются сведения о предложении в Microsoft AppSource (любые указанные цены приведены только для примера и не предназначены для отражения фактических затрат):

:::image type="content" source="media/example-consulting-service-offer-appsource.png" alt-text="Показывает, как это предложение отображается в Azure Marketplace.":::

#### <a name="call-out-descriptions"></a>Описания вызова

1. Эмблема
2. Цена
3. Продукты
4. Отрасли
5. Название предложения
6. Итоги
7. Описание
8. Снимки экрана и видео
9. Документы

### <a name="name"></a>Имя

Введенное здесь имя отображается в качестве заголовка вашего предложения. Это поле предварительно заполняется именем, указанным в поле **Псевдоним предложения** при создании предложения. Впоследствии его можно изменить.

Имя:

- может быть товарным знаком (вы можете указать символы товарного знака и авторского права);
- не может содержать более 50 символов;
- не может содержать эмодзи.

### <a name="search-results-summary"></a>Описание для отображения в результатах поиска

Введите краткое описание предложения. Оно может содержать до 100 символов и использоваться в результатах поиска Marketplace.

### <a name="description"></a>Описание

[!INCLUDE [Long description-1](./includes/long-description-1.md)]

[!INCLUDE [Long description-2](./includes/long-description-2.md)]

[!INCLUDE [Long description-3](./includes/long-description-3.md)]

### <a name="keywords"></a>Keywords

Введите до трех ключевых слов поиска, относящихся к основному продукту и консультационной услуге. Так ваше предложение будет проще найти.

### <a name="duration"></a>Duration

Задайте ожидаемую длительность вовлеченности клиента.

### <a name="contact-information"></a>контактная информация.

Вам необходимо указать имя, адрес электронной почты и номер телефона для **основного** и **дополнительного контакта**. Эти сведения не отображаются для клиентов. Они доступны корпорации Майкрософт и могут предоставляться партнерам по программе "Поставщик облачных решений" (CSP).

### <a name="supporting-documents"></a>Вспомогательные документы

Добавьте до трех (но не менее одного) сопровождающих PDF-документов для вашего предложения.

### <a name="marketplace-images"></a>Образы Marketplace

Предоставьте логотипы и изображения для использования с вашим предложением. Все изображения должны быть в формате PNG. Размытые изображения не принимаются.

[!INCLUDE [logo tips](../includes/graphics-suggestions.md)]

>[!Note]
>Если при отправке файлов возникнет проблема, убедитесь, что ваша локальная сеть не блокирует службу https://upload.xboxlive.com, которую использует Центр партнеров.

#### <a name="store-logos"></a>Логотипы магазина

Укажите PNG-файл для логотипа **крупного** размера. Центр партнеров будет использовать его для создания **небольшого** логотипа. При необходимости можно заменить его другим изображением позже.

- **Крупный** (от 216 x 216 до 350 x 350 ПКС, требуется)
- **Малый** (48 x 48 ПКС, необязательно)

Эти логотипы используются в разных местах списка.

[!INCLUDE [Logo tips](../includes/graphics-suggestions.md)]

#### <a name="screenshots-optional"></a>Снимки экрана (необязательно)

Добавьте до пяти снимков экрана, демонстрирующих работу вашего предложения. Каждый из них должен быть в формате PNG с разрешением 1280 x 720 пикселей.

#### <a name="videos-optional"></a>Видео (необязательно)

Добавьте до четырех видео, демонстрирующих работу вашего предложения. Введите для них имя, URL-адрес и добавьте эскиз в формате PNG размером 1280 x 720 пикселей.

Чтобы продолжить, выберите **Сохранить черновик**.

## <a name="pricing-and-availability"></a>Цены и доступность

Здесь вы определите такие элементы, как цена, рынок и закрытый ключ.

1. **Рынок** — рынок, на котором будет доступно ваше предложение. Для каждого предложения можно выбрать только один рынок.
    1. Для рынков США или Канады выберите **Изменить штаты** (или **провинции**), чтобы задать регионы, в которых будет доступно предложение.
2. **Предварительный просмотр аудитории** — настройте **ключ скрытия**, используемый для ограничения аудитории вашего предложения.
3. **Цены**. Укажите, является ли ваше предложение **бесплатным** или **платным**.

    > [!NOTE]
    > Мы предоставляем платформу только для размещения информации о предложениях консультационных услуг. Все транзакции осуществляются напрямую, вне коммерческой платформы.

4. Для платного предложения укажите **цену и валюту**, а также то, является ли цена **фиксированной** или **приблизительной**. Если вы задали приблизительную цену, укажите в описании, какие факторы влияют на определение фактической цены.
5. Чтобы продолжить, выберите **Сохранить черновик**.

## <a name="review-and-publish"></a>Проверка и публикация

После того как все необходимые разделы предложения будут настроены, вы сможете отправить его для проверки и публикации.

1. Когда вы будете готовы опубликовать предложение консультационной службы, выберите **проверить и опубликовать**.
2. Проверьте сведения на странице отправки окончательных сведений.
3. Если вы считаете, что какие-либо данные требуют дополнительных пояснений, напишите короткое примечание для команды сертификации.
4. Когда будете готовы, нажмите кнопку **Отправить**.
5. На странице **Обзор предложения** можно будет видеть, на каком этапе публикации находится ваше предложение.

Дополнительные сведения о том, сколько времени требуется на прохождение каждого этапа публикации, см. в статье [Проверка состояния публикации предложения на коммерческой платформе](publishing-status.md).

## <a name="update-your-existing-consulting-service-offers"></a>Обновление существующих предложений консультационных услуг

- [Обновление имеющегося предложения на коммерческой платформе](update-existing-offer.md)
