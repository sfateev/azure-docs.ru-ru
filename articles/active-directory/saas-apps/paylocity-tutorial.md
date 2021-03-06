---
title: Руководство по интеграции единого входа Azure Active Directory с Paylocity | Документация Майкрософт
description: Узнайте, как настроить единый вход между Azure Active Directory и Paylocity.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 01/21/2020
ms.author: jeedes
ms.openlocfilehash: 53a224f53f0270166ce75e718e670780730a10c8
ms.sourcegitcommit: 023d10b4127f50f301995d44f2b4499cbcffb8fc
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/18/2020
ms.locfileid: "88543658"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-paylocity"></a>Руководство по Интеграция единого входа Azure Active Directory с Paylocity

В этом руководстве описано, как интегрировать Paylocity с Azure Active Directory (Azure AD). Интеграция Paylocity с Azure AD обеспечивает следующие возможности.

* Контроль доступа к Paylocity с помощью Azure AD.
* Вы можете включить автоматический вход пользователей в Paylocity с помощью учетных записей Azure AD.
* Централизованное управление учетными записями через портал Azure.

Чтобы узнать больше об интеграции приложений SaaS с Azure AD, прочитайте статью [Что такое доступ к приложениям и единый вход с помощью Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

## <a name="prerequisites"></a>Предварительные требования

Чтобы приступить к работе, потребуется следующее.

* Подписка Azure AD. Если у вас нет подписки, вы можете получить [бесплатную учетную запись](https://azure.microsoft.com/free/).
* подписка Paylocity с поддержкой единого входа (SSO).

## <a name="scenario-description"></a>Описание сценария

В рамках этого руководства вы настроите и проверите единый вход Azure AD в тестовой среде.

* Paylocity поддерживает единый вход, инициируемый **поставщиком услуг и поставщиком удостоверений**

* После настройки Paylocity можете применить функцию управления сеансом, которая защищает от хищения и несанкционированного доступа к конфиденциальным данным вашей организации в реальном времени. Элементы управления сеансом являются расширением функции условного доступа. [Узнайте, как применять управление сеансами с помощью Microsoft Cloud App Security](https://docs.microsoft.com/cloud-app-security/proxy-deployment-aad).

## <a name="adding-paylocity-from-the-gallery"></a>Добавление Paylocity из коллекции

Чтобы настроить интеграцию Paylocity с Azure AD, необходимо добавить Paylocity из коллекции в список управляемых приложений SaaS.

1. Войдите на [портал Azure](https://portal.azure.com) с помощью личной учетной записи Майкрософт либо рабочей или учебной учетной записи.
1. В области навигации слева выберите службу **Azure Active Directory**.
1. Перейдите в колонку **Корпоративные приложения** и выберите **Все приложения**.
1. Чтобы добавить новое приложение, выберите **Новое приложение**.
1. В разделе **Добавление из коллекции** в поле поиска введите **Paylocity**.
1. Выберите **Paylocity** в области результатов и добавьте это приложение. Подождите несколько секунд, пока приложение не будет добавлено в ваш клиент.

## <a name="configure-and-test-azure-ad-single-sign-on-for-paylocity"></a>Настройка и проверка единого входа Azure AD для Paylocity

Настройте и проверьте единый вход Azure AD в Paylocity с помощью тестового пользователя **B.Simon**. Для обеспечения работы единого входа необходимо установить связь между пользователем Azure AD и соответствующим пользователем в Paylocity.

Чтобы настроить и проверить единый вход Azure AD в Paylocity, выполните действия в следующих стандартных блоках.

1. **[Настройка единого входа Azure AD](#configure-azure-ad-sso)** необходима, чтобы пользователи могли использовать эту функцию.
    * **[Создание тестового пользователя Azure AD](#create-an-azure-ad-test-user)** требуется для проверки работы единого входа Azure AD с помощью пользователя B.Simon.
    * **[Назначение тестового пользователя Azure AD](#assign-the-azure-ad-test-user)** необходимо, чтобы позволить пользователю B.Simon использовать единый вход Azure AD.
1. **[Настройка единого входа в Paylocity](#configure-paylocity-sso)** необходима, чтобы настроить параметры единого входа на стороне приложения.
    * **[Создание тестового пользователя Paylocity](#create-paylocity-test-user)** требуется для того, чтобы в Paylocity существовал пользователь B.Simon, связанный с одноименным пользователем в Azure AD.
1. **[Проверка единого входа](#test-sso)** позволяет убедиться в правильности конфигурации.

## <a name="configure-azure-ad-sso"></a>Настройка единого входа Azure AD

Выполните следующие действия, чтобы включить единый вход Azure AD на портале Azure.

1. На [портале Azure](https://portal.azure.com/) на странице интеграции с приложением **Paylocity** найдите раздел **Управление** и щелкните **Единый вход**.
1. На странице **Выбрать метод единого входа** выберите **SAML**.
1. На странице **Настройка единого входа с помощью SAML** щелкните значок "Изменить" (значок пера), чтобы открыть диалоговое окно **Базовая конфигурация SAML** и изменить параметры.

   ![Изменение базовой конфигурации SAML](common/edit-urls.png)

1. В разделе **Базовая конфигурация SAML** не нужно выполнять никаких действий, так как приложение уже предварительно интегрировано с Azure.

1. Чтобы настроить приложение для работы в режиме, инициируемом **поставщиком услуг**, щелкните **Задать дополнительные URL-адреса** и выполните следующие действия.

    В текстовом поле **URL-адрес входа** введите URL-адрес: `https://access.paylocity.com/`.

1. Выберите команду **Сохранить**.

1. Приложение Paylocity ожидает проверочные утверждения SAML в определенном формате, который требует добавить настраиваемые сопоставления атрибутов в конфигурацию атрибутов токена SAML. На следующем снимке экрана показан список атрибутов по умолчанию.

    ![Изображение](common/default-attributes.png)

1. В дополнение к описанному выше приложение Paylocity ожидает несколько дополнительных атрибутов в ответе SAML, как показано ниже. Эти атрибуты также заполнены предварительно, однако их следует обновить с помощью реальных значений.

    | Имя |  Исходный атрибут|
    | ---------------| --------------- |
    | PartnerId | `P8000010` |
    | PaylocityUser | `user.mail`|
    | PaylocityEntity | < `PaylocityEntity` > |

    > [!NOTE]
    > PaylocityEntity — идентификатор компании Paylocity.

1. На странице **Настройка единого входа с помощью SAML** в разделе **Сертификат подписи SAML** найдите элемент **XML метаданных федерации** и выберите **Скачать**, чтобы скачать сертификат и сохранить его на компьютере.

    ![Ссылка для скачивания сертификата](common/metadataxml.png)

1. На странице **Настройка единого входа с помощью SAML** в разделе **Сертификат подписи SAML** щелкните **Изменить значок**.

    ![Изменение сертификата для подписи SAML](./media/paylocity-tutorial/edit-samlassertion.png)

1. Для параметра **Вариант подписывания** выберите значение **Ответ знака SAML и утверждение** и нажмите кнопку **Сохранить**.

    ![Изменение сертификата для подписи SAML](./media/paylocity-tutorial/saml-assertion.png)

1. Требуемые URL-адреса вы можете скопировать из раздела **Настройка Paylocity**.

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

В этом разделе описано, как включить единый вход в Azure для пользователя B.Simon, предоставив этому пользователю доступ к Paylocity.

1. На портале Azure выберите **Корпоративные приложения**, а затем — **Все приложения**.
1. В списке приложений выберите **Paylocity**.
1. На странице "Обзор" приложения найдите раздел **Управление** и выберите **Пользователи и группы**.

   ![Ссылка "Пользователи и группы"](common/users-groups-blade.png)

1. Выберите **Добавить пользователя**, а в диалоговом окне **Добавление назначения** выберите **Пользователи и группы**.

    ![Ссылка "Добавить пользователя"](common/add-assign-user.png)

1. В диалоговом окне **Пользователи и группы** выберите **B.Simon** в списке пользователей, а затем в нижней части экрана нажмите кнопку **Выбрать**.
1. Если ожидается, что в утверждении SAML будет получено какое-либо значение роли, то в диалоговом окне **Выбор роли** нужно выбрать соответствующую роль для пользователя из списка и затем нажать кнопку **Выбрать**, расположенную в нижней части экрана.
1. В диалоговом окне **Добавление назначения** нажмите кнопку **Назначить**.

## <a name="configure-paylocity-sso"></a>Настройка единого входа для Paylocity

Чтобы настроить единый вход на стороне **Paylocity**, нужно отправить скачанный файл **XML метаданных федерации** и соответствующие URL-адреса, скопированные на портале Azure, в [службу поддержки Paylocity](mailto:service@paylocity.com). Специалисты службы поддержки настроят подключение единого входа SAML на обеих сторонах.

### <a name="create-paylocity-test-user"></a>Создание тестового пользователя в Paylocity

В этом разделе вы узнаете, как создать пользователя B.Simon в приложении Paylocity. Обратитесь в  [службу поддержки Paylocity](mailto:service@paylocity.com), чтобы добавить пользователей на платформу Paylocity. Перед использованием единого входа необходимо создать и активировать пользователей.

## <a name="test-sso"></a>Проверка единого входа

В этом разделе описано, как проверить конфигурацию единого входа Azure AD с помощью панели доступа.

Щелкнув плитку Paylocity на панели доступа, вы автоматически войдете в приложение Paylocity, для которого настроили единый вход. См. дополнительные сведения о [панели доступа](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)

## <a name="additional-resources"></a>Дополнительные ресурсы

- [Список учебников по интеграции приложений SaaS с Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Что такое доступ к приложениям и единый вход с помощью Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Что представляет собой условный доступ в Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Пробное использование Paylocity с помощью Azure AD](https://aad.portal.azure.com/)

* [Что такое управление сеансами в Microsoft Cloud App Security?](https://docs.microsoft.com/cloud-app-security/proxy-intro-aad)

* [Защита приложений с помощью функции управления настройками условного доступа для приложений в Microsoft Cloud App Security](https://docs.microsoft.com/cloud-app-security/proxy-intro-aad)
