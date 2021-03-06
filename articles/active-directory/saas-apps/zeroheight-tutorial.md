---
title: Руководство по интеграции единого входа Azure Active Directory с zeroheight | Документация Майкрософт
description: Узнайте, как настроить единый вход между Azure Active Directory и zeroheight.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 09/09/2020
ms.author: jeedes
ms.openlocfilehash: bcfd9e1b132ef47c83d028acf5e2bcb3fc637ef5
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2020
ms.locfileid: "91369388"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-zeroheight"></a>Руководство по Интеграции единого входа Azure Active Directory с zeroheight

В этом руководстве описано, как интегрировать zeroheight с Azure Active Directory (Azure AD). Интеграция zeroheight с Azure AD обеспечивает следующие возможности:

* Контроль доступа к zeroheight с помощью Azure AD.
* Автоматический вход пользователей в zeroheight с помощью учетных записей Azure AD.
* Централизованное управление учетными записями через портал Azure.

## <a name="prerequisites"></a>Предварительные требования

Чтобы приступить к работе, потребуется следующее.

* Подписка Azure AD. Если у вас нет подписки, вы можете получить [бесплатную учетную запись](https://azure.microsoft.com/free/).
* Подписка zeroheight с поддержкой единого входа.

## <a name="scenario-description"></a>Описание сценария

В рамках этого руководства вы настроите и проверите единый вход Azure AD в тестовой среде.

* Приложение zeroheight поддерживает единый вход, инициируемый **поставщиком услуг**.

## <a name="adding-zeroheight-from-the-gallery"></a>Добавление zeroheight из коллекции

Чтобы настроить интеграцию zeroheight с Azure AD, необходимо добавить zeroheight из коллекции в список управляемых приложений SaaS.

1. Войдите на портал Azure с помощью личной учетной записи Майкрософт либо рабочей или учебной учетной записи.
1. В области навигации слева выберите службу **Azure Active Directory**.
1. Перейдите в колонку **Корпоративные приложения** и выберите **Все приложения**.
1. Чтобы добавить новое приложение, выберите **Новое приложение**.
1. В разделе **Добавление из коллекции** в поле поиска введите **zeroheight**.
1. Выберите **zeroheight** в области результатов и добавьте это приложение. Подождите несколько секунд, пока приложение не будет добавлено в ваш клиент.


## <a name="configure-and-test-azure-ad-sso-for-zeroheight"></a>Настройка и проверка единого входа Azure AD для zeroheight

Настройте и проверьте единый вход Azure AD в приложение zeroheight с помощью тестового пользователя **B. Simon**. Чтобы обеспечить работу единого входа, необходимо установить связь между пользователем Azure AD и соответствующим пользователем в zeroheight.

Чтобы настроить и проверить единый вход Azure AD в zeroheight, выполните действия в следующих стандартных блоках.

1. **[Настройка единого входа Azure AD](#configure-azure-ad-sso)** необходима, чтобы пользователи могли использовать эту функцию.
    1. **[Создание тестового пользователя Azure AD](#create-an-azure-ad-test-user)** требуется для проверки работы единого входа Azure AD с помощью пользователя B.Simon.
    1. **[Назначение тестового пользователя Azure AD](#assign-the-azure-ad-test-user)** необходимо, чтобы позволить пользователю B.Simon использовать единый вход Azure AD.
1. **[Настройка единого входа в zeroheight](#configure-zeroheight-sso)** необходима, чтобы настроить параметры единого входа на стороне приложения.
    1. **[Создание тестового пользователя zeroheight](#create-zeroheight-test-user)** требуется для того, чтобы в zeroheight существовал пользователь B. Simon, связанный с одноименным пользователем в Azure AD.
1. **[Проверка единого входа](#test-sso)** позволяет убедиться в правильности конфигурации.

## <a name="configure-azure-ad-sso"></a>Настройка единого входа Azure AD

Выполните следующие действия, чтобы включить единый вход Azure AD на портале Azure.

1. На портале Azure на странице интеграции с приложением **zeroheight** найдите раздел **Управление** и выберите **Единый вход**.
1. На странице **Выбрать метод единого входа** выберите **SAML**.
1. На странице **Настройка единого входа с помощью SAML** щелкните значок "Изменить" (значок пера), чтобы открыть диалоговое окно **Базовая конфигурация SAML** и изменить параметры.

   ![Изменение базовой конфигурации SAML](common/edit-urls.png)

1. На странице **Базовая конфигурация SAML** введите значения следующих полей.

    а. В текстовом поле **URL-адрес для входа** введите URL-адрес: `https://zeroheight.com/sso`

    b. В текстовом поле **Идентификатор (сущности)** введите URL-адрес в следующем формате: `zeroheight:<CUSTOM_ID>`.

    c. В текстовом поле **URL-адрес ответа** введите URL-адрес в формате `https://zeroheight.com/sso/acs/<CUSTOM_ID>`.

    > [!NOTE]
    > Эти значения приведены для примера. Измените их на фактические значения идентификатора и URL-адреса ответа. Чтобы получить их, обратитесь в [службу поддержки клиентов zeroheight](mailto:support@zeroheight.com). Можно также посмотреть шаблоны в разделе **Базовая конфигурация SAML** на портале Azure.

1. Приложение zeroheight ожидает утверждения SAML в определенном формате, который предполагает, что вы должны добавить сопоставления настраиваемых атрибутов в конфигурацию атрибутов токена SAML. На следующем снимке экрана показан список атрибутов по умолчанию.

    ![Изображение](common/default-attributes.png)

1. Кроме того, приложение zeroheight ожидает несколько дополнительных атрибутов в ответе SAML, как показано ниже. Эти атрибуты также заранее заполнены, но вы можете изменить их в соответствии со своими требованиями.
    
    | Имя |  Исходный атрибут|
    | ---------- | --------- |
    | email | user.mail |

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

В этом разделе описано, как включить единый вход Azure для пользователя B. Simon, предоставив этому пользователю доступ к zeroheight.

1. На портале Azure выберите **Корпоративные приложения**, а затем — **Все приложения**.
1. Из списка приложений выберите **zeroheight**.
1. На странице "Обзор" приложения найдите раздел **Управление** и выберите **Пользователи и группы**.
1. Выберите **Добавить пользователя**, а в диалоговом окне **Добавление назначения** выберите **Пользователи и группы**.
1. В диалоговом окне **Пользователи и группы** выберите **B.Simon** в списке пользователей, а затем в нижней части экрана нажмите кнопку **Выбрать**.
1. Если пользователям необходимо назначить роль, вы можете выбрать ее из раскрывающегося списка **Выберите роль**. Если для этого приложения не настроена ни одна роль, будет выбрана роль "Доступ по умолчанию".
1. В диалоговом окне **Добавление назначения** нажмите кнопку **Назначить**.

## <a name="configure-zeroheight-sso"></a>Настройка единого входа в zeroheight

Чтобы настроить единый вход на стороне **zeroheight**, необходимо отправить **URL-адрес метаданных федерации приложения** [группе поддержки zeroheight](mailto:support@zeroheight.com). Специалисты службы поддержки настроят подключение единого входа SAML на обеих сторонах.

### <a name="create-zeroheight-test-user"></a>Создание тестового пользователя zeroheight

В этом разделе описано, как создать пользователя Britta Simon в приложении zeroheight. Чтобы добавить пользователей на платформу zeroheight, обратитесь к  [группе поддержки zeroheight](mailto:support@zeroheight.com). Перед использованием единого входа необходимо создать и активировать пользователей.

## <a name="test-sso"></a>Проверка единого входа 

В этом разделе описано, как проверить конфигурацию единого входа Azure AD с помощью указанных ниже способов. 

1. Выберите **Тестировать приложение** на портале Azure. Вы будете перенаправлены по URL-адресу для входа в zeroheight, где можно будет инициировать поток входа. 

2. Перейдите по URL-адресу для входа в zeroheight и инициируйте поток входа.

3. Вы можете использовать Панель доступа (Майкрософт). Щелкнув плитку "zeroheight" на Панели доступа, вы перейдете по URL-адресу для входа в zeroheight. См. дополнительные сведения о [панели доступа](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)

## <a name="next-steps"></a>Next Steps

После настройки zeroheight вы можете применить функцию управления сеансом, чтобы в реальном времени защищать конфиденциальные данные своей организации от кражи и несанкционированного доступа. Управление сеансом является расширением функции условного доступа. [Узнайте, как применять управление сеансами с помощью Microsoft Cloud App Security](https://docs.microsoft.com/cloud-app-security/proxy-deployment-any-app).

