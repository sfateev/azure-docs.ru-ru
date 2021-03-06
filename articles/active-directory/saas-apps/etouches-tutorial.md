---
title: Руководство по интеграции единого входа Azure Active Directory с Aventri | Документация Майкрософт
description: Узнайте, как настроить единый вход между Azure Active Directory и Aventri.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 03/10/2020
ms.author: jeedes
ms.openlocfilehash: 68534a9456cdaccfff32275b37eff39f67b7988e
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2020
ms.locfileid: "88555371"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-aventri"></a>Руководство по интеграции единого входа Azure Active Directory с Aventri

В этом руководстве вы узнаете, как интегрировать Aventri с Azure Active Directory (Azure AD). Интеграция Aventri с Azure AD обеспечивает следующие возможности.

* С помощью Azure AD вы можете контролировать доступ к Aventri.
* Включение автоматического входа пользователей в Aventri с помощью учетных записей Azure AD.
* Централизованное управление учетными записями через портал Azure.

Чтобы узнать больше об интеграции приложений SaaS с Azure AD, прочитайте статью [Что такое доступ к приложениям и единый вход с помощью Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/manage-apps/what-is-single-sign-on)

## <a name="prerequisites"></a>Предварительные требования

Чтобы приступить к работе, потребуется следующее.

* Подписка Azure AD. Если у вас нет подписки, вы можете получить [бесплатную учетную запись](https://azure.microsoft.com/free/).
* Подписка Aventri с поддержкой единого входа.

## <a name="scenario-description"></a>Описание сценария

В рамках этого руководства вы настроите и проверите единый вход Azure AD в тестовой среде.

* Aventri поддерживает единый вход, инициируемый **поставщиком услуг**.
* После настройки Aventri можно применять элемент управления сеансами, который защищает от хищения и несанкционированного доступа к конфиденциальным данным вашей организации в режиме реального времени. Управление сеансом является расширением функции условного доступа. [Узнайте, как применять управление сеансами с помощью Microsoft Cloud App Security](https://docs.microsoft.com/cloud-app-security/proxy-deployment-aad).


## <a name="adding-aventri-from-the-gallery"></a>Добавление Aventri из коллекции

Чтобы настроить интеграцию Aventri с Azure AD, необходимо добавить Aventri из коллекции в список управляемых приложений SaaS.

1. Войдите на [портал Azure](https://portal.azure.com) с помощью личной учетной записи Майкрософт либо рабочей или учебной учетной записи.
1. В области навигации слева выберите службу **Azure Active Directory**.
1. Перейдите в колонку **Корпоративные приложения** и выберите **Все приложения**.
1. Чтобы добавить новое приложение, выберите **Новое приложение**.
1. В разделе **Добавление из коллекции** в поле поиска введите **Aventri**.
1. Выберите **Aventri** в области результатов и добавьте это приложение. Подождите несколько секунд, пока приложение не будет добавлено в ваш клиент.


## <a name="configure-and-test-azure-ad-single-sign-on-for-aventri"></a>Настройка и проверка единого входа Azure AD для Aventri

Настройте и проверьте единый вход Azure AD в Aventri с помощью тестового пользователя **B.Simon**. Для обеспечения работы единого входа необходимо установить связь между пользователем Azure AD и соответствующим пользователем в Aventri.

Чтобы настроить и проверить единый вход Azure AD в Aventri, выполните действия в следующих стандартных блоках:

1. **[Настройка единого входа Azure AD](#configure-azure-ad-sso)** необходима, чтобы пользователи могли использовать эту функцию.
    1. **[Создание тестового пользователя Azure AD](#create-an-azure-ad-test-user)** требуется для проверки работы единого входа Azure AD с помощью пользователя B.Simon.
    1. **[Назначение тестового пользователя Azure AD](#assign-the-azure-ad-test-user)** необходимо, чтобы позволить пользователю B.Simon использовать единый вход Azure AD.
1. **[Настройка единого входа в Aventri](#configure-aventri-sso)** необходима, чтобы настроить параметры единого входа на стороне приложения.
    1. **[Создание тестового пользователя Aventri](#create-aventri-test-user)** требуется для создания в Aventri пользователя B.Simon, связанного с представлением этого же пользователя в Azure AD.
1. **[Проверка единого входа](#test-sso)** позволяет убедиться в правильности конфигурации.

## <a name="configure-azure-ad-sso"></a>Настройка единого входа Azure AD

Выполните следующие действия, чтобы включить единый вход Azure AD на портале Azure.

1. На [портале Azure](https://portal.azure.com/) на странице интеграции с приложением **Aventri** найдите раздел **Управление** и выберите **Единый вход**.
1. На странице **Выбрать метод единого входа** выберите **SAML**.
1. На странице **Настройка единого входа с помощью SAML** щелкните значок "Изменить" (значок пера), чтобы открыть диалоговое окно **Базовая конфигурация SAML** и изменить параметры.

   ![Изменение базовой конфигурации SAML](common/edit-urls.png)

1. На странице **Базовая конфигурация SAML** введите значения следующих полей.

    а. В текстовом поле **URL-адрес для входа** введите URL-адрес в следующем формате: `https://na-admin.eventscloud.com/saml/accounts/acs/<ACCOUNTID>`.

    b. В текстовом поле **Идентификатор (сущности)** введите URL-адрес в следующем формате: `https://na-admin.eventscloud.com/saml/accounts/sso/<ACCOUNTID>`.

    > [!NOTE] 
    > Эти значения приведены для примера. Их необходимо заменить на фактический URL-адрес входа и идентификатор, как описано далее в этом руководстве.

1. Приложение Aventri ожидает проверочные утверждения SAML в определенном формате, который требует добавить настраиваемые сопоставления атрибутов в конфигурацию атрибутов токена SAML. На следующем снимке экрана показан список атрибутов по умолчанию.

    ![Изображение](common/edit-attribute.png)

1. В дополнение к описанному выше приложение Aventri ожидает несколько дополнительных атрибутов в ответе SAML, как показано ниже. Эти атрибуты также заранее заполнены, но вы можете изменить их в соответствии со своими требованиями.

    | Имя | Исходный атрибут|
    | ------------------- | -------------------- |
    | Email | user.mail | 

1. На странице **Настройка единого входа с помощью SAML** в разделе **Сертификат подписи SAML** найдите элемент **XML метаданных федерации** и выберите **Скачать**, чтобы скачать сертификат и сохранить его на компьютере.

    ![Ссылка для скачивания сертификата](common/metadataxml.png)

1. Требуемые URL-адреса можно скопировать из раздела **Настройка Aventri**.

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

В этом разделе описано, как включить единый вход в Azure для пользователя B.Simon, предоставив этому пользователю доступ к Aventri.

1. На портале Azure выберите **Корпоративные приложения**, а затем — **Все приложения**.
1. В списке приложений выберите **Aventri**.
1. На странице "Обзор" приложения найдите раздел **Управление** и выберите **Пользователи и группы**.

   ![Ссылка "Пользователи и группы"](common/users-groups-blade.png)

1. Выберите **Добавить пользователя**, а в диалоговом окне **Добавление назначения** выберите **Пользователи и группы**.

    ![Ссылка "Добавить пользователя"](common/add-assign-user.png)

1. В диалоговом окне **Пользователи и группы** выберите **B.Simon** в списке пользователей, а затем в нижней части экрана нажмите кнопку **Выбрать**.
1. Если ожидается, что в утверждении SAML будет получено какое-либо значение роли, то в диалоговом окне **Выбор роли** нужно выбрать соответствующую роль для пользователя из списка и затем нажать кнопку **Выбрать**, расположенную в нижней части экрана.
1. В диалоговом окне **Добавление назначения** нажмите кнопку **Назначить**.

## <a name="configure-aventri-sso"></a>Настройка единого входа Aventri

1. Чтобы настроить единый вход для приложения Aventri, выполните следующие действия. 

    ![Конфигурация Aventri](./media/etouches-tutorial/aventri-tutorial-06.png) 

    а. Войдите в приложение **Aventri** с правами администратора.
   
    b. Перейдите к настройке **SAML**.

    c. Перейдите в раздел **General Settings** (Общие параметры), откройте в Блокноте скачанный с портала Azure сертификат, скопируйте его содержимое и вставьте в текстовое поле IDP metadata (Метаданные поставщика удостоверений). 

    d. Нажмите кнопку **Save & Stay** (Сохранить и остаться).
  
    д) В разделе метаданных SAML нажмите кнопку **Update Metadata** (Обновить метаданные). 

    е) Это действие открывает страницу единого входа. Если единый вход работает, можно настроить имя пользователя.

    ж. В поле Username (Имя пользователя) выберите значение **emailaddress**, как показано на изображении ниже. 

    h. Скопируйте значение **SP entity ID** (Идентификатор сущности поставщика услуг) и вставьте его в текстовое поле **Идентификатор** в разделе **Базовая конфигурация SAML** на портале Azure.

    i. Скопируйте значение **SSO URL / ACS** (URL-адрес и ASC для единого входа) и вставьте его в текстовое поле **URL-адрес единого входа** в разделе **Базовая конфигурация SAML** на портале Azure.

### <a name="create-aventri-test-user"></a>Создание тестового пользователя Aventri

В этом разделе вы узнаете, как создать пользователя B.Simon в приложении Aventri. Обратитесь к [группе поддержки клиентов Aventri](mailto:support@aventri.com), чтобы добавить пользователей на платформу Aventri.

## <a name="test-sso"></a>Проверка единого входа 

В этом разделе описано, как проверить конфигурацию единого входа Azure AD с помощью панели доступа.

Щелкнув плитку Aventri на панели доступа, вы автоматически войдете в приложение Aventri, для которого настроили единый вход. См. дополнительные сведения о [панели доступа](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)

## <a name="additional-resources"></a>Дополнительные ресурсы

- [Список учебников по интеграции приложений SaaS с Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Что такое доступ к приложениям и единый вход с помощью Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/manage-apps/what-is-single-sign-on)

- [Что представляет собой условный доступ в Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Пробное использование Aventri с Azure AD](https://aad.portal.azure.com/)

- [Что такое управление сеансами в Microsoft Cloud App Security?](https://docs.microsoft.com/cloud-app-security/proxy-intro-aad)