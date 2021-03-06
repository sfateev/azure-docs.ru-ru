---
title: Руководство по интеграции единого входа Azure Active Directory с SolarWinds Orion | Документация Майкрософт
description: Сведения о том, как настроить единый вход Azure Active Directory в SolarWinds Orion.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 07/24/2020
ms.author: jeedes
ms.openlocfilehash: 723546d6a997d895c1a1df39b558aabb797d0a11
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2020
ms.locfileid: "88545103"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-solarwinds-orion"></a>Руководство по интеграции единого входа Azure Active Directory с SolarWinds Orion

В этом руководстве описано, как интегрировать SolarWinds Orion с Azure Active Directory (Azure AD). Интеграция SolarWinds Orion с Azure AD обеспечивает следующие возможности:

* Контроль доступа к SolarWinds Orion с помощью Azure AD.
* Включение автоматического входа пользователей в SolarWinds Orion с использованием учетных записей Azure AD.
* Централизованное управление учетными записями через портал Azure.

Чтобы узнать больше об интеграции приложений SaaS с Azure AD, прочитайте статью [Что такое доступ к приложениям и единый вход с помощью Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/manage-apps/what-is-single-sign-on)

## <a name="prerequisites"></a>Предварительные требования

Чтобы приступить к работе, потребуется следующее.

* Подписка Azure AD. Если у вас нет подписки, вы можете получить [бесплатную учетную запись](https://azure.microsoft.com/free/).
* Подписка SolarWinds Orion с поддержкой единого входа.

## <a name="scenario-description"></a>Описание сценария

В рамках этого руководства вы настроите и проверите единый вход Azure AD в тестовой среде.

* SolarWinds Orion поддерживает единый вход, инициированный **поставщиком услуг и поставщиком удостоверений**.
* После настройки SolarWinds Orion вы можете применить функцию управления сеансом, которая в реальном времени защищает конфиденциальные данные вашей организации от кражи и несанкционированного доступа. Управление сеансом является расширением функции условного доступа. [Узнайте, как применять управление сеансами с помощью Microsoft Cloud App Security](https://docs.microsoft.com/cloud-app-security/proxy-deployment-any-app).

## <a name="adding-solarwinds-orion-from-the-gallery"></a>Добавление приложения SolarWinds Orion из коллекции

Чтобы настроить интеграцию SolarWinds Orion с Azure AD, вам потребуется добавить приложение SolarWinds Orion из коллекции в список управляемых приложений SaaS.

1. Войдите на [портал Azure](https://portal.azure.com) с помощью личной учетной записи Майкрософт либо рабочей или учебной учетной записи.
1. В области навигации слева выберите службу **Azure Active Directory**.
1. Перейдите в колонку **Корпоративные приложения** и выберите **Все приложения**.
1. Чтобы добавить новое приложение, выберите **Новое приложение**.
1. В разделе **Добавление из коллекции** в поле поиска введите **SolarWinds Orion**.
1. Выберите **SolarWinds Orion** в области результатов и добавьте это приложение. Подождите несколько секунд, пока приложение не будет добавлено в ваш клиент.


## <a name="configure-and-test-azure-ad-sso-for-solarwinds-orion"></a>Настройка и проверка единого входа Azure AD для SolarWinds Orion

Настройте и проверьте единый вход Azure AD в SolarWinds Orion с использованием данных тестового пользователя **B.Simon**. Для обеспечения работы единого входа необходимо установить связь между пользователем Azure AD и соответствующим пользователем в SolarWinds Orion.

Чтобы настроить и проверить единый вход Azure AD в SolarWinds Orion, выполните действия в следующих стандартных блоках:

1. **[Настройка единого входа Azure AD](#configure-azure-ad-sso)** необходима, чтобы пользователи могли использовать эту функцию.
    1. **[Создание тестового пользователя Azure AD](#create-an-azure-ad-test-user)** требуется для проверки работы единого входа Azure AD с помощью пользователя B.Simon.
    1. **[Назначение тестового пользователя Azure AD](#assign-the-azure-ad-test-user)** необходимо, чтобы позволить пользователю B.Simon использовать единый вход Azure AD.
1. **[Настройка единого входа в SolarWinds Orion](#configure-solarwinds-orion-sso)** необходима, чтобы настроить параметры единого входа на стороне приложения.
    1. **[Создание тестового пользователя SolarWinds Orion](#create-solarwinds-orion-test-user)** требуется для того, чтобы в SolarWinds Orion существовал пользователь B.Simon, связанный с представлением этого пользователя в Azure AD.
1. **[Проверка единого входа](#test-sso)** позволяет убедиться в правильности конфигурации.

## <a name="configure-azure-ad-sso"></a>Настройка единого входа Azure AD

Выполните следующие действия, чтобы включить единый вход Azure AD на портале Azure.

1. На [портале Azure](https://portal.azure.com/) на странице интеграции с приложением **SolarWinds Orion** найдите раздел **Управление** и выберите **Единый вход**.
1. На странице **Выбрать метод единого входа** выберите **SAML**.
1. На странице **Настройка единого входа с помощью SAML** щелкните значок "Изменить" (значок пера), чтобы открыть диалоговое окно **Базовая конфигурация SAML** и изменить параметры.

   ![Изменение базовой конфигурации SAML](common/edit-urls.png)

1. Если вы хотите настроить приложение в режиме, инициируемом **поставщиком удостоверений**, в разделе **Базовая конфигурация SAML** введите значения следующих полей.

    а. В текстовом поле **Идентификатор** введите URL-адрес в формате `https://<ORION-HOSTNAME-OR-EXTERNAL-URL>`.

    b. В текстовом поле **URL-адрес ответа** введите URL-адрес в формате `https://<ORION-HOSTNAME-OR-EXTERNAL-URL>/Orion/SAMLLogin.aspx`.

1. Чтобы настроить приложение для работы в режиме, инициируемом **поставщиком услуг**, щелкните **Задать дополнительные URL-адреса** и выполните следующие действия.

    В текстовом поле **URL-адрес входа** введите URL-адрес в формате `https://<ORION-HOSTNAME-OR-EXTERNAL-URL>/Orion/Login.aspx`.

    > [!NOTE]
    > Эти значения приведены для примера. Замените их фактическими значениями идентификатора, URL-адреса ответа и URL-адреса входа. Чтобы получить эти значения, обратитесь в [службу поддержки SolarWinds Orion](mailto:technicalsupport@solarwinds.com). Можно также посмотреть шаблоны в разделе **Базовая конфигурация SAML** на портале Azure.

1. Приложение SolarWinds Orion ожидает проверочные утверждения SAML в определенном формате, который требует добавить настраиваемые сопоставления атрибутов в вашу конфигурацию атрибутов токена SAML. На следующем снимке экрана показан список атрибутов по умолчанию.

    ![Изображение](common/default-attributes.png)

1. Кроме того, приложение SolarWinds Orion ожидает несколько дополнительных атрибутов в ответе SAML, как показано ниже. Эти атрибуты также заранее заполнены, но вы можете изменить их в соответствии со своими требованиями.
    
    | Имя |  Исходный атрибут|
    | ----------- | --------- |
    | FirstName | user.givenname |
    | LastName | user.surname |
    | Email |user.mail |

1. На странице **Настройка единого входа с помощью SAML** в разделе **Сертификат подписи SAML** найдите пункт **Сертификат (Base64)** и щелкните **Скачать**, чтобы скачать сертификат. Сохраните этот сертификат на компьютере.

    ![Ссылка для скачивания сертификата](common/certificatebase64.png)

1. Требуемые URL-адреса можно скопировать в разделе **Настройка SolarWinds Orion**.

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

В этом разделе описано, как включить единый вход в Azure для пользователя B.Simon, предоставив этому пользователю доступ к SolarWinds Orion.

1. На портале Azure выберите **Корпоративные приложения**, а затем — **Все приложения**.
1. В списке приложений выберите **SolarWinds Orion**.
1. На странице "Обзор" приложения найдите раздел **Управление** и выберите **Пользователи и группы**.

   ![Ссылка "Пользователи и группы"](common/users-groups-blade.png)

1. Выберите **Добавить пользователя**, а в диалоговом окне **Добавление назначения** выберите **Пользователи и группы**.

    ![Ссылка "Добавить пользователя"](common/add-assign-user.png)

1. В диалоговом окне **Пользователи и группы** выберите **B.Simon** в списке пользователей, а затем в нижней части экрана нажмите кнопку **Выбрать**.
1. Если ожидается, что в утверждении SAML будет получено какое-либо значение роли, то в диалоговом окне **Выбор роли** нужно выбрать соответствующую роль для пользователя из списка и затем нажать кнопку **Выбрать**, расположенную в нижней части экрана.
1. В диалоговом окне **Добавление назначения** нажмите кнопку **Назначить**.

## <a name="configure-solarwinds-orion-sso"></a>Настройка единого входа для приложения SolarWinds Orion

1. Войдите в приложение SolarWinds Orion и выберите **Settings** -> **All Settings** (Параметры -> Все параметры).

    ![ Настройка SolarWinds Orion ](./media/solarwinds-orion-tutorial/settings.png)

1. В разделе **User Accounts** (Учетные записи пользователей) выберите **SAML Configuration** (Конфигурация SAML).

    ![ Настройка SolarWinds Orion ](./media/solarwinds-orion-tutorial/configure-user-accounts.png)

1. Щелкните **Add Identity Provider** (Добавить поставщика удостоверений).

    ![ Настройка SolarWinds Orion ](./media/solarwinds-orion-tutorial/configure-add-identity-provider.png)

1. Выполните следующие действия на странице **Add Identity Provider** (Добавление поставщика удостоверений):

    ![ Настройка SolarWinds Orion ](./media/solarwinds-orion-tutorial/configure-solarwinds.png)

    а. Перейдите на вкладку **Configure** (Настройка).

    b. В текстовом поле **Identity Provider Name** (Имя поставщика удостоверений) введите любое действительное имя, например `My SSO service`.

    c. В текстовое поле **SSO Target URL** (Целевой URL-адрес единого входа) вставьте значение **URL-адрес входа**, скопированное на портале Azure.

    d.  В текстовое поле **Issuer URL** (URL-адрес издателя) вставьте значение **Идентификатор Azure AD**, скопированное на портале Azure.

    д) Откройте в блокноте файл **сертификата (Base64)** , скачанный с портала Azure, и вставьте его содержимое в текстовое поле **X.509 Signing Certificate** (Сертификат X.509 для подписи).

    е) Щелкните **Save**(Сохранить).

### <a name="create-solarwinds-orion-test-user"></a>Создание тестового пользователя SolarWinds Orion

1. Войдите на веб-сайт SolarWinds Orion и выберите **Settings** -> **All Settings** (Параметры -> Все параметры).

    ![ Создание тестового пользователя SolarWinds Orion ](./media/solarwinds-orion-tutorial/settings.png)

1. В разделе **User Accounts** (Учетные записи пользователей) выберите **Manage Accounts** (Управление учетными записями).

    ![ Создание тестового пользователя SolarWinds Orion ](./media/solarwinds-orion-tutorial/user-accounts.png)

1. На вкладке **Individual Accounts** (Отдельные учетные записи) щелкните **Add New Account** (Добавить новую учетную запись).

    ![ Создание тестового пользователя SolarWinds Orion ](./media/solarwinds-orion-tutorial/create-user.png)

1. Выберите тип учетной записи, которую вы хотите создать. Это могут быть отдельные пользователи или группы SAML.

    ![ Создание тестового пользователя SolarWinds Orion ](./media/solarwinds-orion-tutorial/create-user-new-account.png)

1.  В текстовом поле **Name ID** (Идентификатор имени) введите имя, которое должно точно совпадать с именем пользователя или группы в Azure AD.

1.  Щелкните **Next** (Далее) и сохраните данные на странице.

    ![ Создание тестового пользователя SolarWinds Orion ](./media/solarwinds-orion-tutorial/create-user-name-id.png)

## <a name="test-sso"></a>Проверка единого входа 

В этом разделе описано, как проверить конфигурацию единого входа Azure AD с помощью панели доступа.

Щелкнув плитку SolarWinds Orion на Панели доступа, вы автоматически войдете в приложение SolarWinds Orion, для которого настроили единый вход. См. дополнительные сведения о [панели доступа](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)

## <a name="additional-resources"></a>Дополнительные ресурсы

- [Список учебников по интеграции приложений SaaS с Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Что такое доступ к приложениям и единый вход с помощью Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Что представляет собой условный доступ в Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Пробное использование SolarWinds Orion с Azure AD](https://aad.portal.azure.com/)

- [Что такое управление сеансами в Microsoft Cloud App Security?](https://docs.microsoft.com/cloud-app-security/proxy-intro-aad)

- [Как защитить SolarWinds Orion с помощью функции управления настройками условного доступа](https://docs.microsoft.com/cloud-app-security/proxy-intro-aad)

