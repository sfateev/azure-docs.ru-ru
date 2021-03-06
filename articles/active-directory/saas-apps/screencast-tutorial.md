---
title: Руководство по интеграции единого входа Azure Active Directory с Screencast-O-Matic | Документация Майкрософт
description: Узнайте, как настроить единый вход между Azure Active Directory и Screencast-O-Matic.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 11/15/2019
ms.author: jeedes
ms.openlocfilehash: e56684c30d243e0b7848355234fef896ccd220ce
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2020
ms.locfileid: "88543301"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-screencast-o-matic"></a>Руководство по интеграции единого входа Azure Active Directory с Screencast-O-Matic

В этом руководстве вы узнаете, как интегрировать Screencast-O-Matic с Azure Active Directory (Azure AD). Интеграция Screencast-O-Matic с Azure AD обеспечивает некоторые возможности.

* Контролируйте доступ к Screencast-O-Matic с помощью Azure AD.
* Включайте автоматический вход пользователей в Screencast-O-Matic с помощью учетных записей Azure AD.
* Централизованное управление учетными записями через портал Azure.

Чтобы узнать больше об интеграции приложений SaaS с Azure AD, прочитайте статью [Что такое доступ к приложениям и единый вход с помощью Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

## <a name="prerequisites"></a>Предварительные требования

Чтобы приступить к работе, потребуется следующее.

* Подписка Azure AD. Если у вас нет подписки, вы можете получить [бесплатную учетную запись](https://azure.microsoft.com/free/).
* подписка на Screencast-O-Matic с поддержкой единого входа (SSO).

## <a name="scenario-description"></a>Описание сценария

В рамках этого руководства вы настроите и проверите единый вход Azure AD в тестовой среде.

* Screencast-O-Matic поддерживает единый вход, инициированный **пакетом обновления**.
* Screencast-O-Matic поддерживает **JIT**-подготовку пользователей.

## <a name="adding-screencast-o-matic-from-the-gallery"></a>Добавление Screencast-O-Matic из коллекции

Чтобы настроить интеграцию Screencast-O-Matic с Azure AD, необходимо добавить Screencast-O-Matic из коллекции в список управляемых приложений SaaS.

1. Войдите на [портал Azure](https://portal.azure.com) с помощью личной учетной записи Майкрософт либо рабочей или учебной учетной записи.
1. В области навигации слева выберите службу **Azure Active Directory**.
1. Перейдите в колонку **Корпоративные приложения** и выберите **Все приложения**.
1. Чтобы добавить новое приложение, выберите **Новое приложение**.
1. В разделе **Добавление из коллекции** в поле поиска введите **Screencast-O-Matic**.
1. Выберите **Screencast-O-Matic** в области результатов и добавьте это приложение. Подождите несколько секунд, пока приложение не будет добавлено в ваш клиент.

## <a name="configure-and-test-azure-ad-single-sign-on-for-screencast-o-matic"></a>Настройка и проверка единого входа в Azure AD для Screencast-O-Matic

Настройте и проверьте единый вход Azure AD в Screencast-O-Matic с помощью тестового пользователя **B.Simon**. Для обеспечения работы единого входа необходимо установить связь между пользователем Azure AD и соответствующим пользователем в Screencast-O-Matic.

Чтобы настроить и проверить единый вход Azure AD в Screencast-O-Matic, выполните действия в следующих стандартных блоках.

1. **[Настройка единого входа Azure AD](#configure-azure-ad-sso)** необходима, чтобы пользователи могли использовать эту функцию.
    * **[Создание тестового пользователя Azure AD](#create-an-azure-ad-test-user)** требуется для проверки работы единого входа Azure AD с помощью пользователя B.Simon.
    * **[Назначение тестового пользователя Azure AD](#assign-the-azure-ad-test-user)** необходимо, чтобы позволить пользователю B.Simon использовать единый вход Azure AD.
1. **[Настройка единого входа в Screencast-O-Matic](#configure-screencast-o-matic-sso)** необходима, чтобы настроить параметры единого входа на стороне приложения.
    * **[Создание тестового пользователя Screencast-O-Matic](#create-screencast-o-matic-test-user)** требуется для того, чтобы в Screencast-O-Matic существовал пользователь B.Simon, связанный с представлением этого же пользователя в Azure AD.
1. **[Проверка единого входа](#test-sso)** позволяет убедиться в правильности конфигурации.

## <a name="configure-azure-ad-sso"></a>Настройка единого входа Azure AD

Выполните следующие действия, чтобы включить единый вход Azure AD на портале Azure.

1. На [портале Azure](https://portal.azure.com/) на странице интеграции с приложением **Screencast-O-Matic** найдите раздел **Управление** и выберите **Единый вход**.
1. На странице **Выбрать метод единого входа** выберите **SAML**.
1. На странице **Настройка единого входа с помощью SAML** щелкните значок "Изменить" (значок пера), чтобы открыть диалоговое окно **Базовая конфигурация SAML** и изменить параметры.

   ![Изменение базовой конфигурации SAML](common/edit-urls.png)

1. На странице **Базовая конфигурация SAML** введите значения следующих полей.

    В текстовом поле **URL-адрес входа** введите URL-адрес в формате `https://screencast-o-matic.com/<InstanceName>`.

    > [!NOTE]
    > Это значение приведено для примера. Вместо него необходимо указать фактический URL-адрес входа. Чтобы получить это значение, обратитесь в [службу поддержки клиентов Screencast-O-Matic](mailto:support@screencast-o-matic.com). Можно также посмотреть шаблоны в разделе **Базовая конфигурация SAML** на портале Azure.

1. На странице **Настройка единого входа с помощью SAML** в разделе **Сертификат подписи SAML** найдите элемент **XML метаданных федерации** и выберите **Скачать**, чтобы скачать сертификат и сохранить его на компьютере.

    ![Ссылка для скачивания сертификата](common/metadataxml.png)

1. В разделе **Настройка Screencast-O-Matic** скопируйте нужные URL-адреса.

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

В этом разделе описано, как включить единый вход в Azure для пользователя B.Simon, предоставив этому пользователю доступ к Screencast-O-Matic.

1. На портале Azure выберите **Корпоративные приложения**, а затем — **Все приложения**.
1. Из списка приложений выберите **Screencast-O-Matic**.
1. На странице "Обзор" приложения найдите раздел **Управление** и выберите **Пользователи и группы**.

   ![Ссылка "Пользователи и группы"](common/users-groups-blade.png)

1. Выберите **Добавить пользователя**, а в диалоговом окне **Добавление назначения** выберите **Пользователи и группы**.

    ![Ссылка "Добавить пользователя"](common/add-assign-user.png)

1. В диалоговом окне **Пользователи и группы** выберите **B.Simon** в списке пользователей, а затем в нижней части экрана нажмите кнопку **Выбрать**.
1. Если ожидается, что в утверждении SAML будет получено какое-либо значение роли, то в диалоговом окне **Выбор роли** нужно выбрать соответствующую роль для пользователя из списка и затем нажать кнопку **Выбрать**, расположенную в нижней части экрана.
1. В диалоговом окне **Добавление назначения** нажмите кнопку **Назначить**.

## <a name="configure-screencast-o-matic-sso"></a>Настройка единого входа Screencast-O-Matic

1. Чтобы автоматизировать настройку в Screencast-O-Matic, установите **расширение браузера для защищенного входа на страницу "Мои приложения"** , щелкнув **Установить расширение**.

    ![Расширение "Мои приложения"](common/install-myappssecure-extension.png)

1. Чтобы перейти к приложению Screencast-O-Matic после добавления расширения в браузер, щелкните **Настройка Screencast-O-Matic**. После этого укажите учетные данные администратора для входа в Screencast-O-Matic. Расширение браузера автоматически настроит приложение и автоматизирует шаги 3–11.

    ![Настройка конфигурации](common/setup-sso.png)

1. Если вы хотите настроить Screencast-O-Matic вручную, откройте новое окно веб-браузера, войдите на свой корпоративный сайт Screencast-O-Matic в качестве администратора и выполните следующие действия:

1. Выберите пункт **Подписка**.

    ![Подписка](./media/screencast-tutorial/tutorial_screencast_sub.png)

1. В разделе **Страница доступа** нажмите кнопку **Настроить**.

    ![Доступ](./media/screencast-tutorial/tutorial_screencast_setup.png)

1. На экране **Страница настройки доступа** выполните следующие действия.

1. В разделе **URL-адрес** в указанном текстовом поле введите имя экземпляра.

    ![Доступ](./media/screencast-tutorial/tutorial_screencast_access.png)

1. В разделе **SAML User Restriction (optional)** (Ограничение пользователя SAML (необязательно)) установите флажок **Require Domain User** (Требовать пользователя домена).

1. В разделе **Upload IDP Metadata XML File** (Отправка XML-файла метаданных поставщика удостоверений) щелкните **Выбрать файл**, чтобы отправить файл метаданных, скачанный на портале Azure.

1. Нажмите кнопку **ОК**.

    ![Доступ](./media/screencast-tutorial/tutorial_screencast_save.png)

### <a name="create-screencast-o-matic-test-user"></a>Создание тестового пользователя Screencast-O-Matic

В этом разделе вы создадите тестового пользователя с именем Britta Simon в Screencast-O-Matic. Приложение Screencast-O-Matic поддерживает JIT-подготовку пользователей, которая включена по умолчанию. В этом разделе никакие действия с вашей стороны не требуются. Если пользователь еще не существует в Screencast-O-Matic, то он создается после проверки подлинности. Чтобы создать учетную запись пользователя вручную, обратитесь в [службу поддержки Screencast-O-Matic](mailto:support@screencast-o-matic.com).

## <a name="test-sso"></a>Проверка единого входа

В этом разделе описано, как проверить конфигурацию единого входа Azure AD с помощью панели доступа.

Щелкнув плитку Screencast-O-Matic на Панели доступа, вы автоматически войдете в приложение Screencast-O-Matic, для которого настроили единый вход. См. дополнительные сведения о [панели доступа](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)

## <a name="additional-resources"></a>Дополнительные ресурсы

- [Список учебников по интеграции приложений SaaS с Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Что такое доступ к приложениям и единый вход с помощью Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Что представляет собой условный доступ в Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Пробное использование Screencast-O-Matic с Azure AD](https://aad.portal.azure.com/)