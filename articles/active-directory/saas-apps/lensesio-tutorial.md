---
title: Руководство по интеграции единого входа Azure Active Directory с Lenses.io | Документация Майкрософт
description: Из этого руководства вы узнаете, как настроить единый вход Azure Active Directory в Lenses.io.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 07/02/2020
ms.author: jeedes
ms.openlocfilehash: 48a1e50d451abb429e9bc33308909b368283644f
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2020
ms.locfileid: "88661458"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-the-lensesio-dataops-portal"></a>Руководство по интеграции единого входа Azure Active Directory с порталом Lenses.io DataOps

В этом руководстве описано, как интегрировать портал [Lenses.io](https://lenses.io/) DataOps с Azure Active Directory (Azure AD). Интеграция Lenses.io с Azure AD обеспечивает следующие возможности.

* Контроль доступа к порталу Lenses.io с помощью Azure AD.
* Включение автоматического входа пользователей в Lenses с учетными записями Azure AD.
* Централизованное управление учетными записями через портал Azure.

Дополнительные сведения об интеграции приложений SaaS с Azure AD см. в статье о [едином входе в приложения с помощью Azure AD](https://docs.microsoft.com/azure/active-directory/manage-apps/what-is-single-sign-on).

## <a name="prerequisites"></a>Предварительные требования

Чтобы приступить к работе, потребуется следующее.

* Подписка Azure AD. Если у вас нет подписки, вы можете получить [бесплатную учетную запись](https://azure.microsoft.com/free/).
* Экземпляр портала Lenses. Вы можете выбрать один из нескольких [вариантов развертывания](https://lenses.io/product/deployment/).
* [Лицензия](https://lenses.io/product/pricing/) Lenses.io с поддержкой единого входа (SSO).

## <a name="scenario-description"></a>Описание сценария

В рамках этого учебника вы настроите и проверите единый вход Azure AD в тестовой среде.

* Lenses.io поддерживает единый вход, инициированный поставщиком услуг.

* После настройки Lenses.io вы сможете включить управление сеансом. Этот механизм защищает от утечки данных и несанкционированного доступа к конфиденциальным данным вашей организации в режиме реального времени. Управление сеансом является расширением функции условного доступа. [Узнайте, как применять управление сеансами с помощью Microsoft Cloud App Security](https://docs.microsoft.com/cloud-app-security/proxy-deployment-any-app).

## <a name="add-lensesio-from-the-gallery"></a>Добавление Lenses.io из коллекции

Чтобы настроить интеграцию Lenses.io с Azure AD, добавьте Lenses.io в список управляемых приложений SaaS:

1. Войдите на [портал Azure](https://portal.azure.com) с помощью личной учетной записи Майкрософт либо рабочей или учебной учетной записи.
1. В области слева выберите службу **Azure Active Directory**.
1. Перейдите в колонку **Корпоративные приложения** и выберите **Все приложения**.
1. Выберите **Новое приложение**.
1. В разделе **Добавление из коллекции** в поле поиска введите **Lenses.io**.
1. Выберите **Lenses.io** в области результатов и добавьте это приложение. Подождите несколько секунд, пока приложение не будет добавлено в ваш клиент.

## <a name="configure-and-test-azure-ad-sso-for-lensesio"></a>Настройка и проверка единого входа Azure AD для Lenses.io

Создайте тестового пользователя *B. Simon*, чтобы настроить и проверить единый вход Azure AD на портал Lenses.io. Для обеспечения работы единого входа необходимо установить связь между пользователем Azure AD и соответствующим пользователем в Lenses.io.

Выполните следующие шаги:

1. [Настройка единого входа Azure AD](#configure-azure-ad-sso) необходима, чтобы пользователи могли использовать эту функцию.
    1. [Создание тестового пользователя и группы Azure AD](#create-an-azure-ad-test-user-and-group) позволяет проверить работу единого входа Azure AD для пользователя B. Simon.
    1. [Назначение тестового пользователя Azure AD](#assign-the-azure-ad-test-user) необходимо, чтобы пользователь B. Simon мог использовать единый вход Azure AD.
1. [Настройка единого входа в Lenses.io](#configure-lensesio-sso) необходима, чтобы настроить параметры единого входа на стороне приложения.
    1. [Создание разрешений для тестовой группы Lenses.io](#create-lensesio-test-group-permissions) требуется, чтобы указать ресурсы, доступные пользователю B. Simon в Lenses.io (авторизация).
1. [Проверка единого входа](#test-sso) позволяет убедиться в правильности конфигурации.

## <a name="configure-azure-ad-sso"></a>Настройка единого входа Azure AD

Выполните следующие действия, чтобы включить единый вход Azure AD на портале Azure:

1. На [портале Azure](https://portal.azure.com/) на странице интеграции с приложением **Lenses.io** найдите раздел **Управление**, а затем щелкните **Единый вход**.
1. На странице **Выбрать метод единого входа** выберите **SAML**.
1. На странице **Настройка единого входа с помощью SAML** щелкните значок правки или пера, чтобы открыть диалоговое окно **Базовая конфигурация SAML** и изменить эти параметры.

   ![Снимок экрана со значком для изменения базовой конфигурации SAML.](common/edit-urls.png)

1. На странице **Базовая конфигурация SAML** введите значения в следующие поля для ввода текста:

    а. **URL-адрес для входа**. Введите URL-адрес в следующем формате: `https://<CUSTOMER_LENSES_BASE_URL>`. Например, `https://lenses.my.company.com`.

    b. **Идентификатор (сущности)** . Введите URL-адрес в следующем формате: `https://<CUSTOMER_LENSES_BASE_URL>`. Например, `https://lenses.my.company.com`.

    c. **URL-адрес ответа**. Введите URL-адрес в следующем формате: `https://<CUSTOMER_LENSES_BASE_URL>/api/v2/auth/saml/callback?client_name=SAML2Client`. Например, `https://lenses.my.company.com/api/v2/auth/saml/callback?client_name=SAML2Client`.

    > [!NOTE]
    > Эти значения приведены для примера. Замените их фактическими значениями URL-адреса для входа, URL-адреса ответа и идентификатора на основе базового URL-адреса вашего экземпляра портала Lenses. Дополнительные сведения можно найти в [документации по работе единого входа в Lenses.io](https://docs.lenses.io/install_setup/configuration/security.html#single-sign-on-sso-saml-2-0).

1. На странице **Настройка единого входа с помощью SAML** перейдите в раздел **Сертификат подписи SAML**. Найдите **XML метаданных федерации** и выберите **Скачать**, чтобы скачать и сохранить сертификат на локальном компьютере.

    ![Снимок экрана ссылки для скачивания сертификата.](common/metadataxml.png)

1. В разделе **Настройка Lenses.io** примените скачанный XML-файл, чтобы настроить в Lenses единый вход через Azure.

### <a name="create-an-azure-ad-test-user-and-group"></a>Создание тестового пользователя и группы Azure AD

На портале Azure вы создадите тестового пользователя с именем B. Simon. Затем вы создадите тестовую группу, которая управляет доступом пользователя B. Simon в Lenses.

Сведения о том, как Lenses использует сопоставление членства в группах для авторизации, вы можете найти в [документации по работе единого входа в Lenses](https://docs.lenses.io/install_setup/configuration/security.html#id3).

**Чтобы создать тестового пользователя, сделайте следующее.**

1. На портале Azure в области слева последовательно выберите **Azure Active Directory** > **Пользователи** > **Все пользователи**.
1. В верхней части экрана щелкните **Новый пользователь**.
1. В разделе **Свойства пользователя** выполните следующие действия.
   1. В поле **Имя** введите **B.Simon**.  
   1. В поле **Имя пользователя** введите username@companydomain.extension. Например, B.Simon@contoso.com.
   1. Установите флажок **Показать пароль**. Запишите пароль, который отображается в поле **Пароль**.
   1. Нажмите кнопку **создания**.

**Чтобы создать группу, сделайте следующее.**

1. Выберите **Azure Active Directory**, а затем щелкните **Группы**.
1. В верхней части экрана щелкните **Создать группу**.
1. В разделе **Свойства группы** сделайте следующее.
   1. В поле **Тип группы** выберите **Безопасность**.
   1. В поле **Имя группы** введите **LensesUsers**.
   1. Нажмите кнопку **создания**.
1. Выберите группу **LensesUsers** и скопируйте значение параметра **Идентификатор объекта** (например, f8b5c1ec-45de-4abd-af5c-e874091fb5f7). Вы будете использовать этот идентификатор в Lenses для сопоставления пользователей этой группы с [нужными разрешениями](https://docs.lenses.io/install_setup/configuration/security.html#id3).  

**Чтобы включить тестового пользователя в группу, сделайте следующее.**

1. Выберите **Azure Active Directory**, а затем щелкните **Пользователи**.
1. Выберите тестового пользователя **B. Simon**.
1. Выберите **Группы**.
1. В верхней части экрана выберите **Добавить членства**.
1. Найдите и выберите **LensesUsers**.
1. Нажмите кнопку **Выбрать**.

### <a name="assign-the-azure-ad-test-user"></a>Назначение тестового пользователя Azure AD

В этом разделе описано, как включить единый вход Azure для пользователя B.Simon, предоставив этому пользователю доступ к Lenses.io.

1. На портале Azure выберите **Корпоративные приложения**, а затем — **Все приложения**.
1. В списке приложений выберите **Lenses.io**.
1. В разделе **Управление** на странице сводных данных о приложении выберите **Пользователи и группы**:

   ![Снимок экрана со ссылкой "Пользователи и группы".](common/users-groups-blade.png)

1. Выберите **Добавить пользователя**.

   ![Снимок экрана со ссылкой "Добавить пользователя".](common/add-assign-user.png)

1. В диалоговом окне **Добавление назначения** выберите **Пользователи и группы**.
1. В диалоговом окне **Пользователи и группы** выберите **B. Simon** из списка пользователей. Затем в нижней части экрана нажмите кнопку **Выбрать**.
1. Если вы хотите получить значение роли в утверждении SAML, выберите в диалоговом окне **Выбор ролей** подходящую роль для пользователя в предложенном списке. Затем в нижней части экрана нажмите кнопку **Выбрать**.
1. В диалоговом окне **Добавление назначения** нажмите кнопку **Назначить**.

## <a name="configure-lensesio-sso"></a>Настройка единого входа в приложении Lenses.io

Чтобы настроить единый вход на портале **Lenses.io**, установите скачанный **XML метаданных федерации** в экземпляр Lenses и [настройте единый вход в Lenses](https://docs.lenses.io/install_setup/configuration/security.html#configure-lenses).

### <a name="create-lensesio-test-group-permissions"></a>Создание разрешений для тестовой группы Lenses.io

1. Чтобы создать группу в Lenses, используйте значение параметра **Идентификатор объекта** группы **LensesUsers**. Это идентификатор, который вы скопировали в разделе [создания пользователя](#create-an-azure-ad-test-user-and-group).
1. Назначьте необходимые разрешения для пользователя B. Simon.

Подробные сведения см. в разделе по [сопоставлению групп между Azure и Lenses](https://docs.lenses.io/install_setup/configuration/security.html#azure-groups).

## <a name="test-sso"></a>Проверка единого входа

В этом разделе описано, как проверить конфигурацию единого входа Azure AD с помощью Панели доступа.

Щелкнув плитку Lenses.io на Панели доступа, вы автоматически войдете на портал Lenses.io. Дополнительные сведения см. в статье [Общие сведения о панели доступа](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Дополнительные ресурсы

- [Настройка единого входа в экземпляре Lenses.io](https://docs.lenses.io/install_setup/configuration/security.html#single-sign-on-sso-saml-2-0)

- [Учебники по интеграции приложений SaaS с Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Что собой представляет единый вход?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Условный доступ в Azure AD](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Пробное использование Lenses.io с Azure AD](https://aad.portal.azure.com/)

- [Что такое управление сеансами в Microsoft Cloud App Security?](https://docs.microsoft.com/cloud-app-security/proxy-intro-aad)

- [Как защитить Lenses.io с помощью функции управления настройками условного доступа](https://docs.microsoft.com/cloud-app-security/proxy-intro-aad)
