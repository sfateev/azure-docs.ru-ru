---
title: Руководство по интеграции единого входа Azure Active Directory с Euromonitor Passport | Документация Майкрософт
description: Узнайте, как настроить единый вход между Azure Active Directory и Euromonitor Passport.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 10/31/2019
ms.author: jeedes
ms.openlocfilehash: a7b5a1433e2d94d68c0512aa7c09909cb4e7fee5
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2020
ms.locfileid: "88555313"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-euromonitor-passport"></a>Руководство по интеграции единого входа Azure Active Directory с Euromonitor Passport

Из этого руководства вы узнаете, как интегрировать Euromonitor Passport с Azure Active Directory (Azure AD). После интеграции Euromonitor Passport с Azure AD вы сможете выполнять приведенные ниже действия.

* Контролировать доступ к Euromonitor Passport с помощью Azure AD.
* Включить автоматический вход пользователей в Euromonitor Passport с помощью учетной записи Azure AD.
* Централизованное управление учетными записями через портал Azure.

Чтобы узнать больше об интеграции приложений SaaS с Azure AD, прочитайте статью [Что такое доступ к приложениям и единый вход с помощью Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

## <a name="prerequisites"></a>Предварительные требования

Чтобы приступить к работе, потребуется следующее.

* Подписка Azure AD. Если у вас нет подписки, вы можете получить [бесплатную учетную запись](https://azure.microsoft.com/free/).
* Подписка Euromonitor Passport с поддержкой единого входа.

## <a name="scenario-description"></a>Описание сценария

В рамках этого руководства вы настроите и проверите единый вход Azure AD в тестовой среде.

* Euromonitor Passport поддерживает единый вход, инициируемый **поставщиком услуг и поставщиком удостоверений**.

> [!NOTE]
> Идентификатор этого приложения — фиксированное строковое значение, поэтому в одном клиенте можно настроить только один экземпляр.

## <a name="adding-euromonitor-passport-from-the-gallery"></a>Добавление Euromonitor Passport из коллекции

Чтобы настроить интеграцию Euromonitor Passport с Azure AD, необходимо добавить Euromonitor Passport из коллекции в список управляемых приложений SaaS.

1. Войдите на [портал Azure](https://portal.azure.com) с помощью личной учетной записи Майкрософт либо рабочей или учебной учетной записи.
1. В области навигации слева выберите службу **Azure Active Directory**.
1. Перейдите в колонку **Корпоративные приложения** и выберите **Все приложения**.
1. Чтобы добавить новое приложение, выберите **Новое приложение**.
1. В разделе **Добавление из коллекции** в поле поиска введите **Euromonitor Passport**.
1. Выберите **Euromonitor Passport** в области результатов и добавьте это приложение. Подождите несколько секунд, пока приложение не будет добавлено в ваш клиент.

## <a name="configure-and-test-azure-ad-single-sign-on-for-euromonitor-passport"></a>Настройка и проверка единого входа Azure AD для Euromonitor Passport

Настройте и проверьте единый вход Azure AD в Euromonitor Passport с помощью тестового пользователя **B.Simon**. Для обеспечения работы единого входа необходимо установить связь между пользователем Azure AD и соответствующим пользователем в Euromonitor Passport.

Чтобы настроить и проверить единый вход Azure AD в Euromonitor Passport, выполните действия в следующих стандартных блоках.

1. **[Настройка единого входа Azure AD](#configure-azure-ad-sso)** необходима, чтобы пользователи могли использовать эту функцию.
    * **[Создание тестового пользователя Azure AD](#create-an-azure-ad-test-user)** требуется для проверки работы единого входа Azure AD с помощью пользователя B.Simon.
    * **[Назначение тестового пользователя Azure AD](#assign-the-azure-ad-test-user)** необходимо, чтобы позволить пользователю B.Simon использовать единый вход Azure AD.
1. **[Настройку единого входа в Euromonitor Passport](#configure-euromonitor-passport-sso)** необходимо выполнить, чтобы настроить параметры единого входа на стороне приложения.
    * **[Создание тестового пользователя Euromonitor Passport](#create-euromonitor-passport-test-user)** требуется для того, чтобы в Euromonitor Passport существовал пользователь B.Simon, связанный с одноименным пользователем в Azure AD.
1. **[Проверка единого входа](#test-sso)** позволяет убедиться в правильности конфигурации.

## <a name="configure-azure-ad-sso"></a>Настройка единого входа Azure AD

Выполните следующие действия, чтобы включить единый вход Azure AD на портале Azure.

1. На [портале Azure](https://portal.azure.com/) на странице интеграции с приложением **Euromonitor Passport** найдите раздел **Управление** и выберите **Единый вход**.
1. На странице **Выбрать метод единого входа** выберите **SAML**.
1. На странице **Настройка единого входа с помощью SAML** щелкните значок "Изменить" (значок пера), чтобы открыть диалоговое окно **Базовая конфигурация SAML** и изменить параметры.

   ![Изменение базовой конфигурации SAML](common/edit-urls.png)

1. В разделе **Базовая конфигурация SAML** не нужно выполнять никаких действий, так как приложение уже предварительно интегрировано с Azure.

1. Если вы хотите настроить приложение в режиме, инициированном **поставщиком услуг**, вам нужно получить URL-адрес входа в [службе поддержки Euromonitor Passport](mailto:passport.support@euromonitor.com). Получив этот URL-адрес, щелкните **Задать дополнительные URL-адреса** и выполните следующие действия:

    Вставьте полученное значение URL-адреса входа, полученного от службы поддержки Euromonitor Passport, в текстовое поле URL-адреса входа.

1. Выберите команду **Сохранить**.

1. Для приложения Euromonitor Passport проверочные утверждения SAML должны иметь определенный формат. Для этого необходимо добавить настраиваемые сопоставления атрибутов в вашу конфигурацию атрибутов маркера SAML. На следующем снимке экрана показан список атрибутов по умолчанию.

    ![Изображение](common/default-attributes.png)

1. В дополнение к описанному выше, приложение Euromonitor Passport ожидает несколько дополнительных атрибутов в ответе SAML, как показано ниже. Эти атрибуты также заранее заполнены, но вы можете изменить их в соответствии со своими требованиями.

    | Имя | Исходный атрибут|
    | ---------------| --------- |
    | Значение идентификатора имени | user.userprincipalname |

    > [!NOTE]
    > Администраторы клиента могут добавить или изменить атрибуты в зависимости от поставленной задачи.

1. На странице **Настройка единого входа с помощью SAML** в разделе **Сертификат подписи SAML** нажмите кнопку "Копировать", чтобы скопировать **URL-адрес метаданных федерации приложений** и сохранить его на компьютере.

    ![Ссылка для скачивания сертификата](common/copy-metadataurl.png)

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

В рамках этого раздела вы предоставите пользователю B.Simon доступ к Euromonitor Passport для использования единого входа Azure.

1. На портале Azure выберите **Корпоративные приложения**, а затем — **Все приложения**.
1. В списке приложений выберите **Euromonitor Passport**.
1. На странице "Обзор" приложения найдите раздел **Управление** и выберите **Пользователи и группы**.

   ![Ссылка "Пользователи и группы"](common/users-groups-blade.png)

1. Выберите **Добавить пользователя**, а в диалоговом окне **Добавление назначения** выберите **Пользователи и группы**.

    ![Ссылка "Добавить пользователя"](common/add-assign-user.png)

1. В диалоговом окне **Пользователи и группы** выберите **B.Simon** в списке пользователей, а затем в нижней части экрана нажмите кнопку **Выбрать**.
1. Если ожидается, что в утверждении SAML будет получено какое-либо значение роли, то в диалоговом окне **Выбор роли** нужно выбрать соответствующую роль для пользователя из списка и затем нажать кнопку **Выбрать**, расположенную в нижней части экрана.
1. В диалоговом окне **Добавление назначения** нажмите кнопку **Назначить**.

## <a name="configure-euromonitor-passport-sso"></a>Настройка единого входа в приложении Euromonitor Passport

Чтобы настроить единый вход на стороне **Euromonitor Passport**, необходимо отправить созданный **URL-адрес метаданных федерации приложений** в [службу технической поддержки Euromonitor Passport](mailto:passport.support@euromonitor.com). Специалисты службы поддержки настроят подключение единого входа SAML на обеих сторонах.

### <a name="create-euromonitor-passport-test-user"></a>Создание тестового пользователя в приложении Euromonitor Passport

В рамках этого раздела вы создадите пользователя B.Simon в приложении Euromonitor Passport. Обратитесь в  [службу технической поддержки Euromonitor Passport](mailto:passport.support@euromonitor.com), чтобы добавить пользователей на платформу Euromonitor Passport. Перед использованием единого входа необходимо создать и активировать пользователей.

## <a name="test-sso"></a>Проверка единого входа 

В этом разделе описано, как проверить конфигурацию единого входа Azure AD с помощью панели доступа.

Щелкнув элемент Euromonitor Passport на панели доступа, вы автоматически войдете в приложение Euromonitor Passport, для которого настроили единый вход. См. дополнительные сведения о [панели доступа](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)

## <a name="additional-resources"></a>Дополнительные ресурсы

- [Список учебников по интеграции приложений SaaS с Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Что такое доступ к приложениям и единый вход с помощью Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Что представляет собой условный доступ в Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Пробное использование Euromonitor Passport с Azure AD](https://aad.portal.azure.com/)