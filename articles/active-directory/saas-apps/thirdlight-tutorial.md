---
title: Руководство по Интеграция Azure Active Directory с ThirdLight | Документация Майкрософт
description: Из этого руководства вы узнаете, как настроить единый вход между Azure Active Directory и ThirdLight.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 03/25/2019
ms.author: jeedes
ms.openlocfilehash: 3193df0848c0d67935faa0020465e79bd50e2151
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2020
ms.locfileid: "88544984"
---
# <a name="tutorial-azure-active-directory-integration-with-thirdlight"></a>Руководство по Интеграция Azure Active Directory с ThirdLight

В этом руководстве описано, как интегрировать ThirdLight с Azure Active Directory (Azure AD). Такая интеграция обеспечивает следующие преимущества.

* С помощью Azure AD вы можете контролировать доступ к ThirdLight.
* Вы можете включить автоматический вход пользователей в ThirdLight (единый вход) с использованием учетных записей Azure AD.
* Вы можете управлять учетными записями централизованно — через портал Azure.

Дополнительные сведения об интеграции приложений SaaS с Azure AD см. в статье [Single sign-on to applications in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Единый вход в приложения в Azure Active Directory).

Если у вас еще нет подписки Azure, [создайте бесплатную учетную запись Azure](https://azure.microsoft.com/free/), прежде чем начинать работу.

## <a name="prerequisites"></a>Предварительные требования

Чтобы настроить интеграцию Azure AD с ThirdLight, вам потребуется:

* Подписка Azure AD. (если у вас нет среды Azure AD, вы можете получить [бесплатную учетную запись](https://azure.microsoft.com/free/));
* подписка ThirdLight с поддержкой единого входа.

## <a name="scenario-description"></a>Описание сценария

В рамках этого руководства вы настроите и проверите единый вход Azure Active Directory в тестовой среде.

* ThirdLight поддерживает единый вход, инициированный поставщиком службы.

## <a name="add-thirdlight-from-the-gallery"></a>Добавление ThirdLight из коллекции

Чтобы настроить интеграцию ThirdLight с Azure AD, необходимо добавить ThirdLight из коллекции в список управляемых приложений SaaS.

1. На [портале Azure](https://portal.azure.com) в области слева щелкните **Azure Active Directory**.

    ![Выберите Azure Active Directory.](common/select-azuread.png)

2. Выберите **Корпоративные приложения** > **Все приложения**.

    ![Колонка "Корпоративные приложения"](common/enterprise-applications.png)

3. Чтобы добавить новое приложение, выберите **Новое приложение** в верхней части окна.

    ![Выбор элемента "Новое приложение"](common/add-new-app.png)

4. В поле поиска введите **ThirdLight**. В результатах поиска выберите **ThirdLight**, а затем щелкните **Добавить**.

     ![Результаты поиска](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Настройка и проверка единого входа в Azure AD

В этом разделе описана настройка и проверка единого входа Azure AD в ThirdLight с использованием тестового пользователя Britta Simon.
Для единого входа необходимо установить связь между пользователем в Azure AD и соответствующим пользователем в ThirdLight.

Чтобы настроить и проверить единый вход Azure AD в ThirdLight, вам потребуется выполнить следующие действия.

1. **[Настройка единого входа Azure AD](#configure-azure-ad-single-sign-on)** необходима, чтобы пользователи могли использовать эту функцию.
2. **[Настройка единого входа в ThirdLight](#configure-thirdlight-single-sign-on)** на стороне приложения.
3. **[Создание тестового пользователя Azure AD](#create-an-azure-ad-test-user)** требуется для проверки работы единого входа Azure AD.
4. **[Назначение тестового пользователя Azure AD](#assign-the-azure-ad-test-user)** необходимо, чтобы разрешить ему использовать единый вход Azure AD.
5. **[Создание тестового пользователя ThirdLight](#create-a-thirdlight-test-user)** , связанного с одноименным пользователем в Azure AD.
6. **[Проверка единого входа](#test-single-sign-on)** необходима, чтобы убедиться, что конфигурация работает правильно.

### <a name="configure-azure-ad-single-sign-on"></a>Настройка единого входа Azure AD

В этом разделе описано включение единого входа Azure AD на портале Azure.

Чтобы настроить единый вход Azure AD в ThirdLight, выполните следующие действия:

1. На [портале Azure](https://portal.azure.com/) на странице интеграции с приложением ThirdLight выберите **Единый вход**.

    ![Выбор параметра "Единый вход"](common/select-sso.png)

2. В диалоговом окне **Выбрать метод единого входа** выберите режим **SAML/WS-Fed**, чтобы включить единый вход.

    ![Выбор метода единого входа](common/select-saml-option.png)

3. На странице **Настройка единого входа с помощью SAML** щелкните **Изменить**, чтобы открыть диалоговое окно **Базовая конфигурация SAML**.

    ![Значок "Изменить"](common/edit-urls.png)

4. В диалоговом окне **Базовая конфигурация SAML** выполните следующие действия.

    ![Диалоговое окно "Базовая конфигурация SAML"](common/sp-identifier.png)

    1. В поле **URL-адрес для входа** введите URL-адрес в следующем формате:
    
          `https://<subdomain>.thirdlight.com/`

    1. В поле **Идентификатор (сущности)** введите URL-адрес в следующем формате:

       `https://<subdomain>.thirdlight.com/saml/sp`

       > [!NOTE]
       > Эти значения представляют собой заполнители. Необходимо указать фактические URL-адрес для входа и идентификатор. Чтобы получить эти значения, обратитесь в [службу поддержки ThirdLight](https://www.thirdlight.com/support). Можно также ознакомиться с шаблонами в диалоговом окне **Базовая конфигурация SAML** на портале Azure.

5. На странице **Настройка единого входа с помощью SAML** в разделе **Сертификат подписи SAML** щелкните ссылку **Скачать** рядом с пунктом **XML метаданных федерации** и сохраните файл на компьютере.

    ![Ссылка для скачивания сертификата](common/metadataxml.png)

6. Требуемые URL-адреса можно скопировать из раздела **Настройка ThirdLight**.

    ![Копирование URL-адресов настройки](common/copy-configuration-urls.png)

    1. **URL-адрес входа**.

    1. **Идентификатор Azure AD**

    1. **URL-адрес выхода**.

### <a name="configure-thirdlight-single-sign-on"></a>Настройка единого входа в ThirdLight

1. В новом окне веб-браузера войдите на свой корпоративный сайт ThirdLight в качестве администратора.

1. Последовательно выберите **Configuration** > **System Administration** > **SAML2** (Конфигурация > Системное администрирование > SAML2).

    ![Системное администрирование](./media/thirdlight-tutorial/ic805843.png "Администрирование системы")

1. В разделе конфигурации SAML2 выполните следующие действия:
  
    ![Раздел конфигурации SAML2](./media/thirdlight-tutorial/ic805844.png "Раздел конфигурации SAML2")

    1. Выберите параметр **Разрешить единый вход SAML2**.

    1. В разделе **Source for IdP Metadata** (Источник для метаданных поставщика удостоверений) выберите **Load IdP Metadata from XML** (Загрузить метаданные поставщика удостоверений из XML).

    1. Откройте файл метаданных, скачанный с портала Azure при работе с предыдущим разделом. Скопируйте содержимое файла и вставьте его в поле **IdP Metadata XML** (XML-файл с метаданными поставщика удостоверений).

    1. Щелкните **Save SAML2 settings** (Сохранить параметры SAML2).

### <a name="create-an-azure-ad-test-user"></a>Создание тестового пользователя Azure AD

В этом разделе описано, как создать на портале Azure тестового пользователя с именем Britta Simon.

1. На портале Azure в области слева выберите **Azure Active Directory**, **Пользователи**, а затем — **Все пользователи**.

    ![Выбор "Все пользователи"](common/users.png)

2. В верхней части окна выберите **Новый пользователь**.

    ![Выбор "Новый пользователь"](common/new-user.png)

3. В диалоговом окне **Пользователь** сделайте следующее:

    ![Диалоговое окно "Пользователь"](common/user-properties.png)

    1. В поле **Имя** введите **BrittaSimon**.
  
    1. В поле **Имя пользователя** введите **BrittaSimon@\<yourcompanydomain>.\<extension>** . (Например, BrittaSimon@contoso.com).

    1. Установите флажок **Показать пароль** и запишите значение, которое отображается в поле **Пароль**.

    1. Нажмите кнопку **создания**.

### <a name="assign-the-azure-ad-test-user"></a>Назначение тестового пользователя Azure AD

В этом разделе описано, как разрешить пользователю Britta Simon использовать единый вход Azure, предоставив этому пользователю доступ к ThirdLight.

1. На портале Azure выберите **Корпоративные приложения**, **Все приложения**, а затем — **ThirdLight**.

    ![Колонка "Корпоративные приложения"](common/enterprise-applications.png)

2. В списке приложений выберите **ThirdLight**.

    ![Список приложений](common/all-applications.png)

3. В области слева выберите **Пользователи и группы**.

    ![Выбор параметра "Пользователи и группы"](common/users-groups-blade.png)

4. Выберите **Добавить пользователя**, а в диалоговом окне **Добавление назначения** — **Пользователи и группы**.

    ![Выбор элемента "Добавить пользователя"](common/add-assign-user.png)

5. В диалоговом окне **Пользователи и группы** выберите **Britta Simon** из списка пользователей, а затем в нижней части окна нажмите кнопку **Выбрать**.

6. Если ожидается, что в утверждении SAML будет получено значение роли, то в диалоговом окне **Выбор ролей** нужно выбрать соответствующую роль для пользователя из списка. В нижней части окна нажмите кнопку **Выбрать**.

7. В диалоговом окне **Добавление назначения** выберите **Назначить**.

### <a name="create-a-thirdlight-test-user"></a>Создание тестового пользователя ThirdLight

Чтобы разрешить пользователям Azure AD входить в ThirdLight, их нужно добавить в ThirdLight. Необходимо добавить их вручную.

Чтобы создать учетную запись пользователя, выполните следующие действия.

1. Войдите на корпоративный сайт ThirdLight в качестве администратора.

1. Перейдите на вкладку **Users** (Пользователи).

1. Выберите пункт **Пользователи и группы**.

1. Выберите **Add New User** (Добавить нового пользователя).

1. Введите имя пользователя, имя или описание, адрес электронной почты для действующей учетной записи Azure AD, которую вы хотите подготовить. Выберите предустановку или группу новых участников.

1. Нажмите кнопку **создания**.

> [!NOTE]
> Вы можете использовать любые другие средства или API, предоставленные ThirdLight, для подготовки учетных записей пользователей Azure AD.

### <a name="test-single-sign-on"></a>Проверка единого входа

Теперь необходимо проверить конфигурацию единого входа Azure AD с помощью Панели доступа.

Выбрав плитку ThirdLight на Панели доступа, вы автоматически войдете в экземпляр ThirdLight, для которого настроили единый вход. Дополнительные сведения о Панели доступа см. в статье [Access and use apps on the My Apps portal](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) (Доступ к приложениям и их использование на портале "Мои приложения").

## <a name="additional-resources"></a>Дополнительные ресурсы

- [Руководства по интеграции приложений SaaS с Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Что такое доступ к приложениям и единый вход с помощью Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Что представляет собой условный доступ в Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)
