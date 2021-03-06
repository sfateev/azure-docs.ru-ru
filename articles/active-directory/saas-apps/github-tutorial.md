---
title: Руководство по Интеграция Azure Active Directory с GitHub | Документация Майкрософт
description: Узнайте, как настроить единый вход между Azure Active Directory и GitHub.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 08/07/2020
ms.author: jeedes
ms.openlocfilehash: b26ee6d6e82903a3dad91ae931885f62daf5d15b
ms.sourcegitcommit: d2222681e14700bdd65baef97de223fa91c22c55
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/07/2020
ms.locfileid: "91821166"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-github"></a>Руководство по Интеграция единого входа Azure Active Directory с GitHub

В этом руководстве описано, как интегрировать GitHub с Azure Active Directory (Azure AD). Интеграция GitHub с Azure AD обеспечивает следующие возможности.

* С помощью Azure AD можно контролировать доступ к облачной организации GitHub Enterprise.
* Управляйте доступом к облачной организации GitHub Enterprise в одном централизованном расположении — на портале Azure.

Чтобы узнать больше об интеграции приложений SaaS с Azure AD, прочитайте статью [Что такое доступ к приложениям и единый вход с помощью Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

## <a name="prerequisites"></a>Предварительные требования

Чтобы настроить интеграцию Azure AD с GitHub, вам потребуется:

* Подписка Azure AD. (если у вас нет среды Azure AD, вы можете получить пробную версию на один месяц по [этой ссылке](https://azure.microsoft.com/pricing/free-trial/));
* Организация GitHub, созданная в облаке [GitHub Enterprise](https://help.github.com/articles/github-s-products/#github-enterprise), которой требуется [план выставления счетов GitHub Enterprise](https://help.github.com/articles/github-s-billing-plans/#billing-plans-for-organizations).

## <a name="scenario-description"></a>Описание сценария

В рамках этого руководства вы настроите и проверите единый вход Azure AD в тестовой среде.

* GitHub поддерживает единый вход инициированного **пакета обновления**.

* GitHub поддерживает [**автоматическую** подготовку пользователей](github-provisioning-tutorial.md) (приглашения организации).
* После настройки GitHub вы можете применить функцию управления сеансом, которая в режиме реального времени защищает конфиденциальные данные вашей организации от хищения и несанкционированного доступа. Управление сеансом является расширением функции условного доступа. [Узнайте, как применять управление сеансами с помощью Microsoft Cloud App Security](https://docs.microsoft.com/cloud-app-security/proxy-deployment-aad).

## <a name="adding-github-from-the-gallery"></a>Добавление GitHub из коллекции

Чтобы настроить интеграцию GitHub с Azure AD, необходимо добавить GitHub из коллекции в список управляемых приложений SaaS.

1. Войдите на [портал Azure](https://portal.azure.com) с помощью личной учетной записи Майкрософт либо рабочей или учебной учетной записи.
1. В области навигации слева выберите службу **Azure Active Directory**.
1. Перейдите в колонку **Корпоративные приложения** и выберите **Все приложения**.
1. Чтобы добавить новое приложение, выберите **Новое приложение**.
1. В разделе **Добавление из коллекции** в поле поиска введите **GitHub**.
1. Выберите **GitHub** в области результатов и добавьте это приложение. Подождите несколько секунд, пока приложение не будет добавлено в ваш клиент.

## <a name="configure-and-test-azure-ad-single-sign-on-for-github"></a>Настройка и проверка единого входа Azure AD для GitHub

Настройте и проверьте единый вход Azure AD в GitHub с использованием тестового пользователя **B.Simon**. Для обеспечения работы единого входа необходимо установить связь между пользователем Azure AD и соответствующим пользователем в GitHub.

Чтобы настроить и проверить единый вход Azure AD в GitHub, выполните действия в следующих стандартных блоках:

1. **[Настройка единого входа Azure AD](#configure-azure-ad-sso)** необходима, чтобы пользователи могли использовать эту функцию.
    * **[Создание тестового пользователя Azure AD](#create-an-azure-ad-test-user)** требуется для проверки работы единого входа Azure AD с помощью пользователя B.Simon.
    * **[Назначение тестового пользователя Azure AD](#assign-the-azure-ad-test-user)** необходимо, чтобы позволить пользователю B.Simon использовать единый вход Azure AD.
1. **[Настройка единого входа в GitHub](#configure-github-sso)** необходима, чтобы настроить параметры единого входа на стороне приложения.
    * **[Создание тестового пользователя GitHub](#create-github-test-user)** нужно для того, чтобы в GitHub также существовал пользователь B.Simon, связанный с представлением пользователя в Azure AD.
1. **[Проверка единого входа](#test-sso)** позволяет убедиться в правильности конфигурации.

## <a name="configure-azure-ad-sso"></a>Настройка единого входа Azure AD

Выполните следующие действия, чтобы включить единый вход Azure AD на портале Azure.

1. На [портале Azure](https://portal.azure.com/) на странице интеграции с приложением **GitHub** найдите раздел **Управление** и выберите **Единый вход**.
1. На странице **Выбрать метод единого входа** выберите **SAML**.
1. На странице **Настройка единого входа с помощью SAML** щелкните значок "Изменить" (значок пера), чтобы открыть диалоговое окно **Базовая конфигурация SAML** и изменить параметры.

   ![Изменение базовой конфигурации SAML](common/edit-urls.png)

1. На странице **Базовая конфигурация SAML** введите значения следующих полей.

   а. В текстовом поле **URL-адрес для входа** введите URL-адрес в следующем формате: `https://github.com/orgs/<Organization ID>/sso`.

    b. В текстовом поле **Идентификатор (сущности)** введите URL-адрес в следующем формате: `https://github.com/orgs/<Organization ID>`.

    c. В текстовом поле **URL-адрес ответа** введите URL-адрес в формате `https://github.com/orgs/<Organization ID>/saml/consume`.


    > [!NOTE]
    > Обратите внимание, что значения, указанные выше, используются в качестве примера. Необходимо обновить фактические значения URL-адреса для входа, идентификатора и URL-адреса ответа. Мы рекомендуем использовать уникальное значение строки идентификатора. Перейдите в раздел администрирования GitHub, чтобы получить эти значения.

5. Приложение GitHub ожидает утверждения SAML в определенном формате, поэтому следует добавить настраиваемые сопоставления атрибутов в вашу конфигурацию атрибутов токена SAML. На следующем снимке экрана показан список атрибутов по умолчанию, в котором значение **Уникальный идентификатор пользователя (ID)** сопоставляется с **user.userprincipalname**. Приложение GitHub ожидает сопоставления **уникального идентификатора пользователя (ID)** с **user.mail**, поэтому необходимо изменить сопоставление атрибутов, щелкнув значок **Изменить**.

    ![Снимок экрана, на котором показан раздел "Атрибуты пользователя" с выбранным значком "Изменить"](common/edit-attribute.png)

6. На странице **Настройка единого входа с помощью SAML** в разделе **Сертификат подписи SAML** щелкните **Загрузить**, чтобы загрузить требуемый **сертификат (Base64)** из предложенных вариантов, и сохраните его на компьютере.

    ![Ссылка для скачивания сертификата](common/certificatebase64.png)

7. Требуемый URL-адрес можно скопировать из раздела **Настройка GitHub**.

    ![Копирование URL-адресов настройки](common/copy-configuration-urls.png)

    а. URL-адрес входа.

    b. Идентификатор Azure AD.

    c. URL-адрес выхода.

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

В этом разделе описано, как разрешить пользователю B.Simon использовать единый вход Azure, предоставив этому пользователю доступ к GitHub.

1. На портале Azure выберите **Корпоративные приложения**, а затем — **Все приложения**.
1. В списке приложений выберите **GitHub**.
1. На странице "Обзор" приложения найдите раздел **Управление** и выберите **Пользователи и группы**.

   ![Ссылка "Пользователи и группы"](common/users-groups-blade.png)

1. Выберите **Добавить пользователя**, а в диалоговом окне **Добавление назначения** выберите **Пользователи и группы**.

    ![Ссылка "Добавить пользователя"](common/add-assign-user.png)

1. В диалоговом окне **Пользователи и группы** выберите **B.Simon** в списке пользователей, а затем в нижней части экрана нажмите кнопку **Выбрать**.
1. Если ожидается, что в утверждении SAML будет получено какое-либо значение роли, то в диалоговом окне **Выбор роли** нужно выбрать соответствующую роль для пользователя из списка и затем нажать кнопку **Выбрать**, расположенную в нижней части экрана.

    ![Роль пользователя](./media/github-tutorial/user-role.png)

    > [!NOTE]
    > Параметр **Выбор роли** станет недоступен, а выбранному пользователю будет назначена роль по умолчанию "Пользователь".

7. В диалоговом окне **Добавление назначения** нажмите кнопку **Назначить**.

## <a name="configure-github-sso"></a>Настройка единого входа для GitHub

1. В другом окне веб-браузера войдите на свой корпоративный сайт GitHub в качестве администратора.

2. Перейдите к разделу **Параметры** и щелкните **Безопасность**.

    ![Снимок экрана, на котором показано меню "Organization settings" (Параметры организации) на сайте GitHub с выбранным пунктом "Security" (Безопасность)](./media/github-tutorial/security.png)

3. Установите флажок **Enable SAML authentication** (Включить проверку подлинности SAML). После этого появятся поля конфигурации единого входа. Выполните следующие действия:

    ![Снимок экрана, на котором показан раздел "SAML single sign-on" (Единый вход SAML) с установленным флажком "Enable SAML authentication" (Включить проверку подлинности SAML) и выделенными текстовыми полями URL-адресов](./media/github-tutorial/saml-sso.png)

    а. Скопируйте значение **URL-адреса для единого входа** и вставьте его в текстовое поле **URL-адрес для входа** в разделе **Базовая конфигурация SAML** на портале Azure.
    
    b. Скопируйте значение **URL-адрес службы обработчика утверждений** и вставьте его в поле **URL-адрес ответа** в разделе **Базовая конфигурация SAML** на портале Azure.

4. Задайте значения в следующих полях:

    ![Снимок экрана, на котором показаны текстовые поля "Sign on URL" (URL-адрес входа), "Issuer" (Издатель) и "Public certificate" (Общедоступный сертификат)](./media/github-tutorial/configure.png)

    а. В текстовое поле **Sign on URL** (URL-адрес входа) вставьте значение **URL-адрес входа**, скопированное на портале Azure.

    b. В текстовое поле **Issuer** (Издатель) вставьте значение **Идентификатор Azure AD**, скопированное на портале Azure.

    c. Откройте в Блокноте сертификат, скачанный с портала Azure, и вставьте его содержимое в текстовое поле **Public Certificate** (Общедоступный сертификат).

    d. Щелкните значок **Изменить**, чтобы изменить **метод подписи** и **метод выборки** с **RSA-SHA1** и **SHA1** на **RSA-SHA256** и **SHA256**, как показано ниже.
    
    д) Замените URL-адрес по умолчанию на **URL-адрес службы обработчика утверждений (URL-адрес ответа)** , чтобы URL-адрес в GitHub совпадал с URL-адресом на странице регистрации приложения в Azure.

    ![Изображение](./media/github-tutorial/tutorial_github_sha.png)

5. Щелкните **Test SAML configuration** (Проверка конфигурации SAML), чтобы убедиться в отсутствии сбоев проверки при выполнении единого входа.

    ![Настройки](./media/github-tutorial/test.png)

6. Щелкните **Сохранить**.

> [!NOTE]
> Технология единого входа в GitHub позволяет выполнять аутентификацию определенных организаций и не заменяет метод аутентификации самого репозитория. Однако если сеанс github.com истекает, пользователи могут получить запрос на вход с использованием идентификатора и пароля GitHub во время процесса единого входа.

### <a name="create-github-test-user"></a>Создание тестового пользователя GitHub

В рамках этого раздела создается пользователь с именем Britta Simon в GitHub. GitHub поддерживает автоматическую подготовку пользователей, которая по умолчанию включена. Дополнительные сведения о настройке автоматической подготовки пользователей можно найти [здесь](github-provisioning-tutorial.md).

**Если необходимо создать пользователя вручную, выполните следующие действия:**

1. Выполните вход на свой корпоративный сайт Github в качестве администратора.

2. Выберите параметр **Пользователи**.

    ![Снимок экрана, на котором показан сайт GitHub с выбранным разделом "People" (Люди).](./media/github-tutorial/people.png "Люди")

3. Нажмите **Invite member** (Пригласить участника).

    ![Приглашение пользователей](./media/github-tutorial/invite-member.png "Пригласить пользователей")

4. На странице диалогового окна **Invite member** (Приглашение участников) сделайте следующее.

    а. В текстовом поле **Электронная почта** введите адрес электронной почты учетной записи Britta Simon.

    ![Приглашение пользователей](./media/github-tutorial/email-box.png "Приглашение пользователей")

    b. Щелкните **Send Invitation** (Отправить приглашение).

    ![Снимок экрана, на котором показана страница диалогового окна "Invite member" (Приглашение участников) с выбранными элементом "Member" (Участник) и кнопкой "Send Invitation" (Отправить приглашение)](./media/github-tutorial/send-invitation.png "Приглашение пользователей")

    > [!NOTE]
    > Владелец учетной записи Azure Active Directory получит по электронной почте сообщение со ссылкой для активации учетной записи.

## <a name="test-sso"></a>Проверка единого входа 

В этом разделе описано, как проверить конфигурацию единого входа Azure AD с помощью панели доступа.

Щелкнув плитку "GitHub" на панели доступа, вы автоматически войдете в приложение GitHub, для которого настроили единый вход. См. дополнительные сведения о [панели доступа](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)

## <a name="additional-resources"></a>Дополнительные ресурсы

- [Список учебников по интеграции приложений SaaS с Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Что такое доступ к приложениям и единый вход с помощью Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Что представляет собой условный доступ в Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Пробное использование GitHub с Azure AD](https://aad.portal.azure.com/)

- [Что такое управление сеансами в Microsoft Cloud App Security?](https://docs.microsoft.com/cloud-app-security/proxy-intro-aad)
