---
title: Руководство по Интеграция единого входа Azure Active Directory с SumoLogic | Документация Майкрософт
description: Узнайте, как настроить единый вход Azure Active Directory в SumoLogic.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 01/03/2020
ms.author: jeedes
ms.openlocfilehash: a14235cb4fc20147accc8eb8a0e421cbc4df90f8
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2020
ms.locfileid: "88533672"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-sumologic"></a>Руководство по Интеграция единого входа Azure Active Directory с SumoLogic

В этом учебнике описано, как интегрировать SumoLogic с Azure Active Directory (Azure AD). Интеграция SumoLogic с Azure AD обеспечивает следующие возможности:

* С помощью Azure AD вы можете контролировать доступ к SumoLogic.
* Вы можете включить автоматический вход пользователей в SumoLogic с помощью учетных записей Azure AD.
* Централизованное управление учетными записями через портал Azure.

Чтобы узнать больше об интеграции приложений SaaS с Azure AD, прочитайте статью [Что такое доступ к приложениям и единый вход с помощью Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

## <a name="prerequisites"></a>Предварительные требования

Чтобы приступить к работе, потребуется следующее.

* Подписка Azure AD. Если у вас нет подписки, вы можете получить [бесплатную учетную запись](https://azure.microsoft.com/free/).
* Подписка SumoLogic с поддержкой единого входа.

## <a name="scenario-description"></a>Описание сценария

В рамках этого руководства вы настроите и проверите единый вход Azure AD в тестовой среде.

* SumoLogic поддерживает единый вход, инициированный **поставщиком удостоверений**.

## <a name="adding-sumologic-from-the-gallery"></a>Добавление SumoLogic из коллекции

Чтобы настроить интеграцию SumoLogic с Azure AD, необходимо добавить SumoLogic из коллекции в список управляемых приложений SaaS.

1. Войдите на [портал Azure](https://portal.azure.com) с помощью личной учетной записи Майкрософт либо рабочей или учебной учетной записи.
1. В области навигации слева выберите службу **Azure Active Directory**.
1. Перейдите в колонку **Корпоративные приложения** и выберите **Все приложения**.
1. Чтобы добавить новое приложение, выберите **Новое приложение**.
1. В разделе **Добавление из коллекции** в поле поиска введите **SumoLogic**.
1. Выберите **SumoLogic** в области результатов и добавьте это приложение. Подождите несколько секунд, пока приложение не будет добавлено в ваш клиент.

## <a name="configure-and-test-azure-ad-single-sign-on-for-sumologic"></a>Настройка и проверка единого входа Azure AD для SumoLogic

Настройте и проверьте единый вход Azure AD в SumoLogic с помощью тестового пользователя **B. Simon**. Для обеспечения работы единого входа необходимо установить связь между пользователем Azure AD и соответствующим пользователем в SumoLogic.

Чтобы настроить и проверить единый вход Azure AD в SumoLogic, выполните действия в следующих стандартных блоках.

1. **[Настройка единого входа Azure AD](#configure-azure-ad-sso)** необходима, чтобы пользователи могли использовать эту функцию.
    * **[Создание тестового пользователя Azure AD](#create-an-azure-ad-test-user)** требуется для проверки работы единого входа Azure AD с помощью пользователя B.Simon.
    * **[Назначение тестового пользователя Azure AD](#assign-the-azure-ad-test-user)** необходимо, чтобы позволить пользователю B.Simon использовать единый вход Azure AD.
1. **[Настройка единого входа в SumoLogic](#configure-sumologic-sso)** необходима, чтобы настроить параметры единого входа на стороне приложения.
    * **[Создание тестового пользователя SumoLogic](#create-sumologic-test-user)** требуется для того, чтобы в SumoLogic существовал пользователь B. Simon, связанный с одноименным пользователем в Azure AD.
1. **[Проверка единого входа](#test-sso)** позволяет убедиться в правильности конфигурации.

## <a name="configure-azure-ad-sso"></a>Настройка единого входа Azure AD

Выполните следующие действия, чтобы включить единый вход Azure AD на портале Azure.

1. На [портале Azure](https://portal.azure.com/) на странице интеграции с приложением **SumoLogic** найдите раздел **Управление** и выберите **Единый вход**.
1. На странице **Выбрать метод единого входа** выберите **SAML**.
1. На странице **Настройка единого входа с помощью SAML** щелкните значок "Изменить" (значок пера), чтобы открыть диалоговое окно **Базовая конфигурация SAML** и изменить параметры.

   ![Изменение базовой конфигурации SAML](common/edit-urls.png)

1. На странице **Настройка единого входа с помощью SAML** введите значения для следующих полей:

    а. В текстовом поле **Идентификатор** введите URL-адрес в таком формате:

    - `https://service.sumologic.com`
    - `https://<tenantname>.us2.sumologic.com`
    - `https://<tenantname>.us4.sumologic.com`
    - `https://<tenantname>.eu.sumologic.com`
    - `https://<tenantname>.jp.sumologic.com`
    - `https://<tenantname>.de.sumologic.com`
    - `https://<tenantname>.ca.sumologic.com`

    b. В текстовом поле **URL-адрес ответа** введите URL-адрес в таком формате:

    - `https://service.sumologic.com/sumo/saml/consume/<tenantname>`
    - `https://service.us2.sumologic.com/sumo/saml/consume/<tenantname>`
    - `https://service.us4.sumologic.com/sumo/saml/consume/<tenantname>`
    - `https://service.eu.sumologic.com/sumo/saml/consume/<tenantname>`
    - `https://service.jp.sumologic.com/sumo/saml/consume/<tenantname>`
    - `https://service.de.sumologic.com/sumo/saml/consume/<tenantname>`
    - `https://service.ca.sumologic.com/sumo/saml/consume/<tenantname>`
    - `https://service.au.sumologic.com/sumo/saml/consume/<tenantname>`

    > [!NOTE]
    > Эти значения приведены для примера. Измените их на фактические значения идентификатора и URL-адреса ответа. Чтобы получить эти значения, обратитесь в [службу поддержки клиентов SumoLogic](https://www.sumologic.com/contact-us/). Можно также посмотреть шаблоны в разделе **Базовая конфигурация SAML** на портале Azure.

1. Приложение SumoLogic ожидает проверочные утверждения SAML в определенном формате, который требует добавить настраиваемые сопоставления атрибутов в вашу конфигурацию атрибутов токена SAML. На следующем снимке экрана показан список атрибутов по умолчанию.

    ![Изображение](common/default-attributes.png)

1. В дополнение к описанному выше приложение SumoLogic ожидает в ответе SAML несколько дополнительных атрибутов, которые показаны ниже. Эти атрибуты также заранее заполнены, но вы можете изменить их в соответствии со своими требованиями.

    |  Имя | Исходный атрибут |
    | ---------------| --------------- |
    | FirstName | user.givenname |
    | LastName | user.surname |
    | Роли | user.assignedroles |

    > [!NOTE]
    > Перейдите по [этой ссылке](https://docs.microsoft.com/azure/active-directory/develop/active-directory-enterprise-app-role-management), чтобы прочитать о настройке **роли** в Azure Active Directory.

1. На странице **Настройка единого входа с помощью SAML** в разделе **Сертификат подписи SAML** найдите пункт **Сертификат (Base64)** и щелкните **Скачать**, чтобы скачать сертификат. Сохраните этот сертификат на компьютере.

    ![Ссылка для скачивания сертификата](common/certificatebase64.png)

1. В разделе **Настройка SumoLogic** скопируйте URL-адрес, соответствующий вашим требованиям.

    ![Копирование URL-адресов настройки](common/copy-configuration-urls.png)

### <a name="create-an-azure-ad-test-user"></a>Создание тестового пользователя Azure AD

В этом разделе описано, как на портале Azure создать тестового пользователя с именем B.Simon.

1. На портале Azure в области слева выберите **Azure Active Directory**, **Пользователи**, а затем — **Все пользователи**.
1. В верхней части экрана выберите **Новый пользователь**.
1. В разделе **Свойства пользователя** выполните следующие действия.
   1. В поле **Имя** введите `B.Simon`.  
   1. В поле **Имя пользователя** введите username@companydomain.extension. Например, `B.Simon@contoso.com`.
   1. Установите флажок **Показать пароль** и запишите значение, которое отображается в поле **Пароль**.
   1. Нажмите кнопку **Создать**.

### <a name="assign-the-azure-ad-test-user"></a>Назначение тестового пользователя Azure AD

В этом разделе описано, как включить единый вход в Azure для пользователя B. Simon, предоставив этому пользователю доступ к SumoLogic.

1. На портале Azure выберите **Корпоративные приложения**, а затем — **Все приложения**.
1. В списке приложений выберите **SumoLogic**.
1. На странице "Обзор" приложения найдите раздел **Управление** и выберите **Пользователи и группы**.

   ![Ссылка "Пользователи и группы"](common/users-groups-blade.png)

1. Выберите **Добавить пользователя**, а в диалоговом окне **Добавление назначения** выберите **Пользователи и группы**.

    ![Ссылка "Добавить пользователя"](common/add-assign-user.png)

1. В диалоговом окне **Пользователи и группы** выберите **B.Simon** в списке пользователей, а затем в нижней части экрана нажмите кнопку **Выбрать**.
1. Если ожидается, что в утверждении SAML будет получено какое-либо значение роли, то в диалоговом окне **Выбор роли** нужно выбрать соответствующую роль для пользователя из списка и затем нажать кнопку **Выбрать**, расположенную в нижней части экрана.
1. В диалоговом окне **Добавление назначения** нажмите кнопку **Назначить**.

## <a name="configure-sumologic-sso"></a>Настройка единого входа в SumoLogic

1. В другом окне веб-браузера войдите на корпоративный веб-сайт SumoLogic в качестве администратора.

1. Выберите **Manage (Управление) \> Security (Безопасность)** .

    ![Управление](./media/sumologic-tutorial/ic778556.png "Управление")

1. Нажмите кнопку **SAML**.

    ![Глобальные параметры безопасности](./media/sumologic-tutorial/ic778557.png "Глобальные параметры безопасности")

1. В списке **Select a configuration or create a new one** (Выберите настройку или создайте новую) выберите **Azure AD**, а затем щелкните **Configure** (Настройка).

    ![Настройка SAML 2.0](./media/sumologic-tutorial/ic778558.png "Настройка SAML 2.0")

1. В диалоговом окне **Настройка SAML 2.0** сделайте следующее:

    ![Настройка SAML 2.0](./media/sumologic-tutorial/ic778559.png "Настройка SAML 2.0")

    а. В текстовом поле **Configuration Name** (Имя конфигурации) введите **Azure AD**.

    b. Выберите **Режим отладки**.

    c. В текстовое поле **Issuer** (Издатель) вставьте значение **идентификатора Azure AD**, скопированное на портале Azure.

    d. В текстовое поле **Authn Request URL** (URL-адрес для запроса аутентификации) вставьте **URL-адрес входа**, скопированный на портале Azure.

    д) Откройте сертификат в кодировке Base-64 в Блокноте, скопируйте его содержимое в буфер обмена и вставьте весь сертификат в текстовое поле **Сертификат X.509** .

    е) В поле **Email Attribute** (Атрибут электронной почты) задайте значение **Use SAML subject** (Использовать субъект SAML).  

    ж. Выберите пункт **Конфигурация входа, инициируемая поставщиком услуг**.

    h. В текстовом поле **Login Path** (Путь входа) введите **Azure** и нажмите кнопку **Save** (Сохранить).

### <a name="create-sumologic-test-user"></a>Создание тестового пользователя SumoLogic

Чтобы пользователи Azure AD могли входить в SumoLogic, их необходимо подготовить для SumoLogic. В случае SumoLogic подготовка пользователей осуществляется вручную.

**Чтобы подготовить учетную запись пользователя, сделайте следующее:**

1. Войдите в клиент **SumoLogic**.

1. Выберите **Manage \> Users** (Управление > Пользователи).

    ![Пользователи](./media/sumologic-tutorial/ic778561.png "Пользователи")

1. Нажмите кнопку **Добавить**.

    ![Пользователи](./media/sumologic-tutorial/ic778562.png "Пользователи")

1. В диалоговом окне **Новый пользователь** сделайте следующее:

    ![Новый пользователь](./media/sumologic-tutorial/ic778563.png "Новый пользователь")

    а. Введите сведения об учетной записи Azure AD, которую необходимо подготовить, в текстовые поля **First Name** (Имя), **Last Name** (Фамилия) и **Email** (Адрес электронной почты).
  
    b. Выберите роль.
  
    c. Для параметра **Status** (Состояние) выберите значение **Active** (Активно).
  
    d. Выберите команду **Сохранить**.

> [!NOTE]
> Вы можете использовать любые другие средства создания учетной записи пользователя SumoLogic или API-интерфейсы, предоставляемые SumoLogic, для подготовки учетных записей пользователя AAD.

## <a name="test-sso"></a>Проверка единого входа

В этом разделе описано, как проверить конфигурацию единого входа Azure AD с помощью панели доступа.

Щелкнув плитку SumoLogic на Панели доступа, вы автоматически войдете в приложение SumoLogic, для которого настроили единый вход. См. дополнительные сведения о [панели доступа](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)

## <a name="additional-resources"></a>Дополнительные ресурсы

- [Список учебников по интеграции приложений SaaS с Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Что такое доступ к приложениям и единый вход с помощью Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Что представляет собой условный доступ в Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Попробуйте использовать SumoLogic с Azure AD](https://aad.portal.azure.com/)