---
title: Учебник. Интеграция Azure Active Directory с Rally Software | Документация Майкрософт
description: Узнайте, как настроить единый вход между Azure Active Directory и Rally Software.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 03/27/2019
ms.author: jeedes
ms.openlocfilehash: 542d330138885a997a0e4a335272492116686a65
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2020
ms.locfileid: "88548909"
---
# <a name="tutorial-azure-active-directory-integration-with-rally-software"></a>Учебник. Интеграция Azure Active Directory с Rally Software

В этом учебнике описано, как интегрировать Rally Software с Azure Active Directory (Azure AD).
Интеграция Rally Software с Azure AD обеспечивает следующие преимущества:

* С помощью Azure AD вы можете контролировать доступ к Rally Software.
* Вы можете включить автоматический вход пользователей в Rally Software (единый вход) с использованием учетных записей Azure AD.
* Вы можете управлять учетными записями централизованно на портале Azure.

Дополнительные сведения об интеграции приложений SaaS с Azure AD см. в статье [Единый вход в приложениях в Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Если у вас еще нет подписки Azure, [создайте бесплатную учетную запись Azure](https://azure.microsoft.com/free/), прежде чем начинать работу.

## <a name="prerequisites"></a>предварительные требования

Чтобы настроить интеграцию Azure AD с Rally Software, вам потребуется следующее:

* подписка Azure AD; (если у вас нет среды Azure AD, вы можете получить пробную версию на один месяц по [этой ссылке](https://azure.microsoft.com/pricing/free-trial/));
* подписка Rally Software с поддержкой единого входа.

## <a name="scenario-description"></a>Описание сценария

В рамках этого руководства вы настроите и проверите единый вход Azure AD в тестовой среде.

* Rally Software поддерживает единый вход, инициированный **поставщиком услуг**.

## <a name="adding-rally-software-from-the-gallery"></a>Добавление Rally Software из коллекции

Чтобы настроить интеграцию Rally Software с Azure AD, вам потребуется добавить Rally Software из коллекции в список управляемых приложений SaaS.

**Чтобы добавить Rally Software из коллекции, выполните следующие действия:**

1. На **[портале Azure](https://portal.azure.com)** в области навигации слева щелкните значок **Azure Active Directory**.

    ![Кнопка Azure Active Directory](common/select-azuread.png)

2. Перейдите в колонку **Корпоративные приложения** и выберите **Все приложения**.

    ![Колонка "Корпоративные приложения"](common/enterprise-applications.png)

3. Чтобы добавить новое приложение, в верхней части диалогового окна нажмите кнопку **Создать приложение**.

    ![Кнопка "Создать приложение"](common/add-new-app.png)

4. В поле поиска введите **Rally Software**, выберите **Rally Software** на панели результатов и нажмите кнопку **Добавить**, чтобы добавить это приложение.

     ![Rally Software в списке результатов](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Настройка и проверка единого входа в Azure AD

В этом разделе описана настройка и проверка единого входа Azure AD в Rally Software для тестового пользователя **Britta Simon**.
Чтобы обеспечить единый вход, необходимо установить связь между пользователем Azure AD и соответствующим пользователем в Rally Software.

Чтобы настроить и проверить единый вход в Azure AD в Rally Software, вам потребуется выполнить действия в следующих стандартных блоках:

1. **[Настройка единого входа Azure AD](#configure-azure-ad-single-sign-on)** необходима, чтобы пользователи могли использовать эту функцию.
2. **[Настройка единого входа в Rally Software](#configure-rally-software-single-sign-on)** требуется, чтобы определить параметры единого входа на стороне приложения.
3. **[Создание тестового пользователя Azure AD](#create-an-azure-ad-test-user)** требуется для проверки работы единого входа Azure AD от имени пользователя Britta Simon.
4. **[Назначение тестового пользователя Azure AD](#assign-the-azure-ad-test-user)** необходимо, чтобы разрешить пользователю Britta Simon использовать единый вход Azure AD.
5. **[Создание тестового пользователя Rally Software](#create-rally-software-test-user)** требуется для создания в Rally Software пользователя Britta Simon, связанного с одноименным пользователем в Azure AD.
6. **[Проверка единого входа](#test-single-sign-on)** необходима, чтобы проверить работу конфигурации.

### <a name="configure-azure-ad-single-sign-on"></a>Настройка единого входа Azure AD

В этом разделе описано включение единого входа Azure AD на портале Azure.

Чтобы настроить единый вход в Azure AD для Rally Software, выполните следующие действия:

1. На [портале Azure](https://portal.azure.com/) на странице интеграции с приложением **Rally Software** выберите **Единый вход**.

    ![Ссылка "Настройка единого входа"](common/select-sso.png)

2. В диалоговом окне **Выбрать метод единого входа** выберите режим **SAML/WS-Fed**, чтобы включить единый вход.

    ![Режим выбора единого входа](common/select-saml-option.png)

3. На странице **Настройка единого входа с помощью SAML** щелкните **Изменить**, чтобы открыть диалоговое окно **Базовая конфигурация SAML**.

    ![Правка базовой конфигурации SAML](common/edit-urls.png)

4. В разделе **Базовая конфигурация SAML** выполните приведенные ниже действия.

    ![Сведения о домене и URL-адресах единого входа приложения Rally Software](common/sp-identifier.png)

    а. В текстовом поле **URL-адрес для входа** введите URL-адрес в следующем формате: `https://<tenant-name>.rally.com`.

    b. В текстовом поле **Идентификатор (сущности)** введите URL-адрес в следующем формате: `https://<tenant-name>.rally.com`.

    > [!NOTE]
    > Эти значения приведены для примера. Необходимо обновить эти значения действующим URL-адресом для входа и идентификатором. Чтобы получить эти значения, обратитесь в [службу поддержки клиентов Rally Software](https://help.rallydev.com/). Можно также посмотреть шаблоны в разделе **Базовая конфигурация SAML** на портале Azure.

5. На странице **Настройка единого входа с помощью SAML** в разделе **Сертификат подписи SAML** щелкните **Скачать**, чтобы скачать нужный вам **XML метаданных федерации**, и сохраните его на компьютере.

    ![Ссылка для скачивания сертификата](common/metadataxml.png)

6. Требуемые URL-адреса можно скопировать из раздела **Настройка Rally Software**.

    ![Копирование URL-адресов настройки](common/copy-configuration-urls.png)

    а. URL-адрес входа.

    b. Идентификатор Azure AD

    c. URL-адрес выхода.

### <a name="configure-rally-software-single-sign-on"></a>Настройка единого входа в Rally Software

1. Выполните вход в клиент **Rally Software**.

2. На панели инструментов вверху щелкните **Setup** (Настройка), затем выберите **Subscription** (Подписка).
   
    ![подписка](./media/rally-software-tutorial/ic769531.png "Subscription")

3. Нажмите кнопку **Действие**. В правой верхней части панели инструментов выберите **Изменение подписки**.

4. На странице диалогового окна **Subscription** (Подписка) выполните указанные ниже действия, после чего нажмите кнопку **Save & Close** (Сохранить и закрыть).
   
    ![Аутентификация](./media/rally-software-tutorial/ic769542.png "Аутентификация")
   
    а. Из раскрывающегося списка "Аутентификация" выберите пункт **Rally or SSO authentication** (Аутентификация Rally или путем единого входа).

    b. В текстовое поле **Identity provider URL** (URL-адрес поставщика удостоверений) вставьте значение **идентификатора Azure AD**, скопированное с портала Azure. 

    c. В текстовое поле **SSO Logout** (Выход из службы единого входа) вставьте значение **URL-адреса выхода**, скопированное на портале Azure.

### <a name="create-an-azure-ad-test-user"></a>Создание тестового пользователя Azure AD 

Цель этого раздела — создать на портале Azure тестового пользователя с именем Britta Simon.

1. На портале Azure в области слева выберите **Azure Active Directory**, **Пользователи**, а затем — **Все пользователи**.

    ![Ссылки "Пользователи и группы" и "Все пользователи"](common/users.png)

2. В верхней части экрана выберите **Новый пользователь**.

    ![Кнопка "Новый пользователь"](common/new-user.png)

3. В разделе свойств пользователя сделайте следующее:

    ![Диалоговое окно "Пользователь"](common/user-properties.png)

    а. В поле **Имя** введите **BrittaSimon**.
  
    b. В поле **Имя пользователя** введите brittasimon@yourcompanydomain.extension. Например BrittaSimon@contoso.com.

    c. Установите флажок **Показать пароль** и запишите значение, которое отображается в поле "Пароль".

    d. Нажмите кнопку **Создать**.

### <a name="assign-the-azure-ad-test-user"></a>Назначение тестового пользователя Azure AD

В этом разделе описано, как включить единый вход Azure для пользователя Britta Simon и предоставить этому пользователю доступ к Rally Software.

1. На портале Azure последовательно выберите **Корпоративные приложения**, **Все приложения**, а затем — **Rally Software**.

    ![Колонка "Корпоративные приложения"](common/enterprise-applications.png)

2. В списке приложений выберите **Rally Software**.

    ![Ссылка на Rally Software в списке "Приложения"](common/all-applications.png)

3. В меню слева выберите **Пользователи и группы**.

    ![Ссылка "Пользователи и группы"](common/users-groups-blade.png)

4. Нажмите кнопку **Добавить пользователя**, а затем в диалоговом окне **Добавление назначения** выберите **Пользователи и группы**.

    ![Область "Добавление назначения"](common/add-assign-user.png)

5. В диалоговом окне **Пользователи и группы** из списка пользователей выберите **Britta Simon**, а затем в верхней части экрана нажмите кнопку **Выбрать**.

6. Если ожидается, что в утверждении SAML будет получено какое-либо значение роли, то в диалоговом окне **Выбор ролей** нужно выбрать соответствующую роль для пользователя из списка и затем нажать кнопку **Выбрать**, расположенную в нижней части экрана.

7. В диалоговом окне **Добавление назначения** нажмите кнопку **Назначить**.

### <a name="create-rally-software-test-user"></a>Создание тестового пользователя Rally Software

Чтобы пользователи Azure AD могли войти систему, их необходимо подготовить для приложения Rally Software, используя их имена пользователей Azure Active Directory.

**Чтобы настроить подготовку учетных записей пользователей, выполните следующие действия.**

1. Выполните вход в клиент Rally Software.

2. Щелкните **Setup \> USERS** ("Настройка" > "ПОЛЬЗОВАТЕЛИ"), затем выберите **+ Add New** (+ Добавить).
   
    ![Пользователи](./media/rally-software-tutorial/ic781039.png "Пользователи")

3. Введите имя в текстовом поле "Новый пользователь" и щелкните **Добавить со сведениями**.

4. В разделе **Создание пользователя** выполните следующие действия.
   
    ![Создание пользователя](./media/rally-software-tutorial/ic781040.png "Создать пользователя")

    а. В текстовое поле **Имя пользователя** введите имя пользователя, например **Brittsimon**.
   
    b. В текстовое поле **Адрес электронной почты** введите адрес электронной почты пользователя, например brittasimon@contoso.com.

    c. В текстовое поле **First Name** (Имя) введите имя пользователя, например **Britta**.

    d. В текстовое поле **Last Name** (Фамилия) введите фамилию пользователя, например **Simon**.

    д) Щелкните **Save & Close** (Сохранить и закрыть).

   >[!NOTE]
   >Вы можете использовать любые другие инструменты создания учетных записей пользователя Rally Software или API, предоставляемые Rally Software для подготовки учетных записей пользователя Azure AD.

### <a name="test-single-sign-on"></a>Проверка единого входа 

В этом разделе описано, как проверить конфигурацию единого входа Azure AD с помощью панели доступа.

Щелкнув плитку Rally Software на Панели доступа, вы автоматически войдете в приложение Rally Software, для которого настроили единый вход. См. дополнительные сведения о [панели доступа](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)

## <a name="additional-resources"></a>Дополнительные ресурсы

- [Список учебников по интеграции приложений SaaS с Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Что такое доступ к приложениям и единый вход с помощью Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Что представляет собой условный доступ в Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

