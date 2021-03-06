---
title: Руководство по интеграции единого входа Azure Active Directory с приложением WhosOnLocation | Документация Майкрософт
description: Узнайте, как настроить единый вход Azure Active Directory в WhosOnLocation.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 06/30/2020
ms.author: jeedes
ms.openlocfilehash: ba9fe6b727ec3f2f5ec8133901fb1aea287fbcd8
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2020
ms.locfileid: "88523125"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-whosonlocation"></a>Руководство по интеграции единого входа Azure Active Directory с WhosOnLocation

В этом руководстве описано, как интегрировать WhosOnLocation с Azure Active Directory (Azure AD). Интеграция WhosOnLocation с Azure AD обеспечивает следующие возможности.

* С помощью Azure AD вы можете контролировать доступ к WhosOnLocation.
* Вы можете включить автоматический вход пользователей в WhosOnLocation с учетными записями Azure AD.
* Централизованное управление учетными записями через портал Azure.

Чтобы узнать больше об интеграции приложений SaaS с Azure AD, прочитайте статью [Что такое доступ к приложениям и единый вход с помощью Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/manage-apps/what-is-single-sign-on)

## <a name="prerequisites"></a>Предварительные требования

Чтобы приступить к работе, потребуется следующее.

* Подписка Azure AD. Если у вас нет подписки, вы можете получить [бесплатную учетную запись](https://azure.microsoft.com/free/).
* Подписка WhosOnLocation с поддержкой единого входа.

## <a name="scenario-description"></a>Описание сценария

В рамках этого руководства вы настроите и проверите единый вход Azure AD в тестовой среде.

* WhosOnLocation поддерживает единый вход, инициируемый **поставщиком услуг**.

* После настройки WhosOnLocation вы сможете применить функцию управления сеансом, которая в реальном времени защищает от хищения конфиденциальных данных вашей организации и несанкционированного доступа к ним. Управление сеансом является расширением функции условного доступа. [Узнайте, как применять управление сеансами с помощью Microsoft Cloud App Security](https://docs.microsoft.com/cloud-app-security/proxy-deployment-any-app).

## <a name="adding-whosonlocation-from-the-gallery"></a>Добавление WhosOnLocation из коллекции

Чтобы настроить интеграцию WhosOnLocation с Azure AD, вам потребуется добавить WhosOnLocation из коллекции в список управляемых приложений SaaS.

1. Войдите на [портал Azure](https://portal.azure.com) с помощью личной учетной записи Майкрософт либо рабочей или учебной учетной записи.
1. В области навигации слева выберите службу **Azure Active Directory**.
1. Перейдите в колонку **Корпоративные приложения** и выберите **Все приложения**.
1. Чтобы добавить новое приложение, выберите **Новое приложение**.
1. В разделе **Добавление из коллекции** в поле поиска введите **WhosOnLocation**.
1. Выберите **WhosOnLocation** в области результатов и добавьте это приложение. Подождите несколько секунд, пока приложение не будет добавлено в ваш клиент.


## <a name="configure-and-test-azure-ad-sso-for-whosonlocation"></a>Настройка и проверка единого входа Azure AD для WhosOnLocation

Настройте и проверьте единый вход Azure AD в WhosOnLocation с помощью тестового пользователя **B.Simon**. Для обеспечения работы единого входа необходимо установить связь между пользователем AAD и соответствующим пользователем в приложении WhosOnLocation.

Чтобы настроить и проверить единый вход Azure AD в WhosOnLocation, выполните действия в следующих стандартных блоках.

1. **[Настройка единого входа Azure AD](#configure-azure-ad-sso)** необходима, чтобы пользователи могли использовать эту функцию.
    1. **[Создание тестового пользователя Azure AD](#create-an-azure-ad-test-user)** требуется для проверки работы единого входа Azure AD с помощью пользователя B.Simon.
    1. **[Назначение тестового пользователя Azure AD](#assign-the-azure-ad-test-user)** необходимо, чтобы позволить пользователю B.Simon использовать единый вход Azure AD.
1. **[Настройка единого входа в WhosOnLocation](#configure-whosonlocation-sso)** необходима, чтобы настроить параметры единого входа на стороне приложения.
    1. **[Создание тестового пользователя WhosOnLocation](#create-whosonlocation-test-user)** требуется для того, чтобы в приложении WhosOnLocation существовал пользователь B.Simon, связанный с одноименным пользователем в Azure AD.
1. **[Проверка единого входа](#test-sso)** позволяет убедиться в правильности конфигурации.

## <a name="configure-azure-ad-sso"></a>Настройка единого входа Azure AD

Выполните следующие действия, чтобы включить единый вход Azure AD на портале Azure.

1. На [портале Azure](https://portal.azure.com/) на странице интеграции с приложением **WhosOnLocation** найдите раздел **Управление** и выберите **Единый вход**.
1. На странице **Выбрать метод единого входа** выберите **SAML**.
1. На странице **Настройка единого входа с помощью SAML** щелкните значок "Изменить" (значок пера), чтобы открыть диалоговое окно **Базовая конфигурация SAML** и изменить параметры.

   ![Изменение базовой конфигурации SAML](common/edit-urls.png)

1. На странице **Базовая конфигурация SAML** введите значения следующих полей.

    а. В текстовом поле **URL-адрес для входа** введите URL-адрес в следующем формате: `https://login.whosonlocation.com/saml/login/<CUSTOM_ID>`.

    b. В текстовом поле **Идентификатор (сущности)** введите URL-адрес в следующем формате: `https://login.whosonlocation.com/saml/metadata/<CUSTOM_ID>`.

    c. В текстовом поле **URL-адрес ответа** введите URL-адрес в формате `https://login.whosonlocation.com/saml/acs/<CUSTOM_ID>`.

    > [!NOTE]
    > Эти значения приведены для примера. Вместо них необходимо указать фактические значения URL-адреса входа, URL-адреса ответа и идентификатора. Чтобы получить их, обратитесь в [службу поддержки WhosOnLocation](mailto:support@whosonlocation.com). Можно также посмотреть шаблоны в разделе **Базовая конфигурация SAML** на портале Azure.

1. На странице **Настройка единого входа с помощью SAML** в разделе **Сертификат подписи SAML** найдите пункт **Сертификат (Base64)** и щелкните **Скачать**, чтобы скачать сертификат. Сохраните этот сертификат на компьютере.

    ![Ссылка для скачивания сертификата](common/certificatebase64.png)

1. Скопируйте требуемый URL-адрес из раздела **Настройка WhosOnLocation**.

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

В этом разделе описано, как включить единый вход в Azure для пользователя B.Simon, предоставив этому пользователю доступ к WhosOnLocation.

1. На портале Azure выберите **Корпоративные приложения**, а затем — **Все приложения**.
1. В списке приложений выберите **WhosOnLocation**.
1. На странице "Обзор" приложения найдите раздел **Управление** и выберите **Пользователи и группы**.

   ![Ссылка "Пользователи и группы"](common/users-groups-blade.png)

1. Выберите **Добавить пользователя**, а в диалоговом окне **Добавление назначения** выберите **Пользователи и группы**.

    ![Ссылка "Добавить пользователя"](common/add-assign-user.png)

1. В диалоговом окне **Пользователи и группы** выберите **B.Simon** в списке пользователей, а затем в нижней части экрана нажмите кнопку **Выбрать**.
1. Если ожидается, что в утверждении SAML будет получено какое-либо значение роли, то в диалоговом окне **Выбор роли** нужно выбрать соответствующую роль для пользователя из списка и затем нажать кнопку **Выбрать**, расположенную в нижней части экрана.
1. В диалоговом окне **Добавление назначения** нажмите кнопку **Назначить**.

## <a name="configure-whosonlocation-sso"></a>Настройка единого входа в WhosOnLocation

1. В другом окне браузера войдите на корпоративный веб-сайт WhosOnLocation с правами администратора.

2. Щелкните **Tools** -> **Account** (Средства — Учетные записи).

    ![Конфигурация WhosOnLocation](./media/WhosOnLocation-tutorial/config1.png)

3. На панели навигации слева выберите **Employee Access** (Доступ для сотрудников).

    ![Конфигурация WhosOnLocation](./media/WhosOnLocation-tutorial/config2.png)

4. На следующей странице выполните приведенные ниже шаги.

    ![Конфигурация WhosOnLocation](./media/WhosOnLocation-tutorial/config3.png)

    а. Для параметра **Single sign-on with SAML** (Единый вход с использованием SAML) выберите **Yes** (Да).

    b. В текстовое поле **Issuer URL** (URL-адрес издателя) вставьте значение **Идентификатор сущности**, скопированное на портале Azure.

    c. В текстовое поле **SSO Endpoint** (Конечная точка единого входа) вставьте значение **URL-адрес входа**, скопированное на портале Azure.

    d. Откройте в блокноте скачанный с портала Azure файл **сертификата (Base64)** и вставьте его содержимое в текстовое поле **Certificate** (Сертификат).

    д) Щелкните **Save SAML Configuration** (Сохранить конфигурацию SAML).

### <a name="create-whosonlocation-test-user"></a>Создание тестового пользователя WhosOnLocation

В этом разделе вы создадите в приложении WhosOnLocation пользователя с именем B. Simon. Для создания пользователей на платформе WhosOnLocation обратитесь к  [группе поддержки WhosOnLocation](mailto:support@whosonlocation.com). Перед использованием единого входа необходимо создать и активировать пользователей.

## <a name="test-sso"></a>Проверка единого входа 

В этом разделе описано, как проверить конфигурацию единого входа Azure AD с помощью панели доступа.

Щелкнув плитку WhosOnLocation на панели доступа, вы автоматически войдете в приложение WhosOnLocation, для которого настроили единый вход. См. дополнительные сведения о [панели доступа](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)

## <a name="additional-resources"></a>Дополнительные ресурсы

- [Список учебников по интеграции приложений SaaS с Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Что такое доступ к приложениям и единый вход с помощью Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Что представляет собой условный доступ в Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Попробуйте работу в WhosOnLocation с Azure AD](https://aad.portal.azure.com/)

- [Что такое управление сеансами в Microsoft Cloud App Security?](https://docs.microsoft.com/cloud-app-security/proxy-intro-aad)

- [Защита приложений с помощью функции управления настройками условного доступа для приложений в Microsoft Cloud App Security](https://docs.microsoft.com/cloud-app-security/proxy-intro-aad)

