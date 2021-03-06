---
title: Руководство по интеграции единого входа Azure Active Directory с Field iD | Документация Майкрософт
description: Узнайте, как настроить единый вход между Azure Active Directory и Field iD.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 05/28/2020
ms.author: jeedes
ms.openlocfilehash: a9ace754a75d63bc24bea91dd6c88a3d004fd0eb
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2020
ms.locfileid: "88555101"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-field-id"></a>Руководство по интеграции единого входа Azure Active Directory с Field iD

В этом учебнике описано, как интегрировать Field iD с Azure Active Directory (Azure AD). Интеграция Field iD с Azure AD обеспечивает следующие возможности.

* С помощью Azure AD вы можете контролировать доступ к Field iD.
* Вы можете включить автоматический вход пользователей в Field iD с помощью учетных записей Azure AD.
* Централизованное управление учетными записями через портал Azure.

Дополнительные сведения об интеграции приложений SaaS с Azure AD см. в статье о [едином входе в приложения с помощью Azure Active Directory](https://docs.microsoft.com/azure/active-directory/manage-apps/what-is-single-sign-on).

## <a name="prerequisites"></a>Предварительные требования

Для начала работы необходимы перечисленные ниже компоненты и данные.

* Подписка Azure AD. Если у вас нет подписки, вы можете получить [бесплатную учетную запись](https://azure.microsoft.com/free/).
* Подписка Field iD с поддержкой единого входа.

## <a name="scenario-description"></a>Описание сценария

В рамках этого руководства вы настроите и проверите единый вход Azure AD в тестовой среде.

* Field iD поддерживает единый вход, инициируемый поставщиком удостоверений.
* После настройки Field iD вы сможете включить управление сеансом. Эта обеспечивает защиту от хищения конфиденциальных данных вашей организации и несанкционированного доступа к ним в реальном времени. Управление сеансом является расширением функции условного доступа. [Узнайте, как применять управление сеансами с помощью Microsoft Cloud App Security](https://docs.microsoft.com/cloud-app-security/proxy-deployment-any-app).

## <a name="add-field-id-from-the-gallery"></a>Добавление Field iD из коллекции

Чтобы настроить интеграцию Field iD в Azure AD, необходимо добавить Field iD из коллекции в список управляемых приложений SaaS.

1. Войдите на [портал Azure](https://portal.azure.com) с помощью личной учетной записи Майкрософт либо рабочей или учебной учетной записи.
1. В области навигации слева выберите **Azure Active Directory**.
1. Перейдите в колонку **Корпоративные приложения** и выберите **Все приложения**.
1. Чтобы добавить новое приложение, выберите **Новое приложение**.
1. В разделе **Добавление из коллекции** в поле поиска введите **Field iD**.
1. Выберите **Field iD** в области результатов и добавьте это приложение. Подождите несколько секунд, пока приложение не будет добавлено в ваш клиент.

## <a name="configure-and-test-azure-ad-single-sign-on-for-field-id"></a>Настройка и проверка единого входа Azure AD для Field iD

Настройте и проверьте единый вход с Azure AD в Field iD, используя тестового пользователя **B.Simon**. Для обеспечения работы единого входа необходимо установить связь между пользователем AAD и соответствующим пользователем в приложении Field iD.

Чтобы настроить и проверить единый вход Azure AD в Field iD, выполните следующие действия:

1. [Настройка единого входа Azure AD](#configure-azure-ad-sso) необходима, чтобы пользователи могли использовать эту функцию.
    1. [Создание тестового пользователя Azure AD](#create-an-azure-ad-test-user) требуется для проверки работы единого входа Azure AD от имени пользователя B.Simon.
    1. [Назначение тестового пользователя Azure AD](#assign-the-azure-ad-test-user) необходимо, чтобы разрешить B.Simon использовать единый вход Azure AD.
1. [Настройка единого входа в Field iD](#configure-field-id-sso) необходима, чтобы настроить параметры единого входа на стороне приложения.
    1. [Создание тестового пользователя Field iD](#create-a-field-id-test-user) требуется для того, чтобы в Field iD существовал пользователь B.Simon, связанный с одноименным пользователем в Azure AD.
1. [Проверка единого входа](#test-sso) позволяет убедиться в правильности конфигурации.

## <a name="configure-azure-ad-sso"></a>Настройка единого входа Azure AD

Выполните следующие действия, чтобы включить единый вход Azure AD на портале Azure.

1. На [портале Azure](https://portal.azure.com/) на странице интеграции с приложением **Field iD** найдите раздел **Управление**. Затем выберите **Единый вход**.
1. На странице **Выбрать метод единого входа** выберите **SAML**.
1. На странице **Настройка единого входа с помощью SAML** щелкните значок карандаша, чтобы открыть диалоговое окно **Базовая конфигурация SAML** и изменить эти параметры.

   ![Снимок экрана настройки единого входа на странице SAML с выделенным значком карандаша](common/edit-urls.png)

1. В разделе **Базовая конфигурация SAML** выполните приведенные ниже действия.

   а. В текстовом поле **Идентификатор** введите URL-адрес в следующем формате: `https://<tenantname>.fieldid.com/fieldid`.

   b. В текстовом поле **URL-адрес ответа** введите URL-адрес в следующем формате: `https://<tenantname>.fieldid.com/fieldid/saml/SSO/alias/<Tenant Name>`.

    > [!NOTE]
    > Эти значения приведены в качестве примера. Измените их на фактические значения идентификатора и URL-адреса ответа. Чтобы получить эти значения, обратитесь к [группе поддержки Field iD](mailto:support@ecompliance.com). Можно также посмотреть шаблоны в разделе **Базовая конфигурация SAML** на портале Azure.

1. На странице **Настройка единого входа с помощью SAML** в разделе **Сертификат подписи SAML** щелкните значок копирования, чтобы скопировать **URL-адрес метаданных федерации приложений**. Сохраните его на компьютере.

    ![Снимок экрана сертификата подписи SAML с выделенной кнопкой копирования](common/copy-metadataurl.png)

### <a name="create-an-azure-ad-test-user"></a>Создание тестового пользователя Azure AD

В этом разделе описано, как на портале Azure создать тестового пользователя с именем B.Simon.

1. Слева на портале Azure выберите **Azure Active Directory** > **Пользователи** > **Все пользователи**.
1. В верхней части экрана выберите **Новый пользователь**.
1. В разделе **Свойства пользователя** выполните следующие действия.
   1. Для параметра **Имя** введите `B.Simon`.  
   1. В поле **Имя пользователя** введите username@companydomain.extension (например, `B.Simon@contoso.com`).
   1. Установите флажок **Показать пароль** и запишите значение, которое отображается в поле **Пароль**.
   1. Нажмите кнопку **создания**.

### <a name="assign-the-azure-ad-test-user"></a>Назначение тестового пользователя Azure AD

В этом разделе описано, как включить единый вход в Azure для пользователя B.Simon, предоставив этому пользователю доступ к Field iD.

1. На портале Azure выберите **Корпоративные приложения** > **Все приложения**.
1. В списке приложений выберите **Field iD**.
1. На странице сводных сведений о приложении откройте раздел **Управление** и выберите **Пользователи и группы**.

   ![Снимок экрана: раздел "Управление" с выделенным пунктом "Пользователи и группы"](common/users-groups-blade.png)

1. Выберите **Добавить пользователя**, а в диалоговом окне **Добавление назначения** — **Пользователи и группы**.

    ![Снимок экрана: раздел Add user (Добавление пользователя)](common/add-assign-user.png)

1. В диалоговом окне **Пользователи и группы** выберите **B.Simon** в списке **Пользователи**, а затем в нижней части экрана нажмите кнопку **Выбрать**.
1. Если вы хотите получить значение роли в утверждении SAML, выберите в диалоговом окне **Выбор роли** подходящую роль для пользователя в предложенном списке. Затем нажмите кнопку **Выбрать** в нижней части экрана.
1. В диалоговом окне **Добавление назначения** выберите **Назначить**.

## <a name="configure-field-id-sso"></a>Настройка единого входа в Field iD

Чтобы настроить единый вход на стороне Field iD, отправьте **URL-адрес метаданных федерации приложения** [группе поддержки Field iD](mailto:support@ecompliance.com). Там обеспечат правильную настройку единого входа SAML с обеих сторон.

### <a name="create-a-field-id-test-user"></a>Создание тестового пользователя Field iD

В этом разделе описано, как создать пользователя Britta Simon в приложении Field iD. Обратитесь к [группе поддержки Field iD](mailto:support@ecompliance.com), чтобы добавить пользователей на платформу Field iD. Перед использованием единого входа необходимо создать и активировать пользователей.

## <a name="test-sso"></a>Проверка единого входа

В этом разделе описано, как проверить конфигурацию единого входа Azure AD с помощью Панели доступа.

Выбрав плитку Field iD на Панели доступа, вы автоматически войдете в приложение Field iD, для которого настроили единый вход. Дополнительные сведения см. в статье о [входе на портал "Мои приложения" и запуске приложений на нем](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Дополнительные ресурсы

- [Руководства по интеграции приложений SaaS с Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Что такое доступ к приложениям и единый вход с помощью Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/manage-apps/what-is-single-sign-on)

- [Что представляет собой условный доступ в Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Попробуйте использовать Field iD с Azure AD](https://aad.portal.azure.com/)

- [Что такое управление сеансами в Microsoft Cloud App Security?](https://docs.microsoft.com/cloud-app-security/proxy-intro-aad)

- [Защита приложений с помощью функции управления настройками условного доступа для приложений в Microsoft Cloud App Security](https://docs.microsoft.com/cloud-app-security/proxy-intro-aad)

