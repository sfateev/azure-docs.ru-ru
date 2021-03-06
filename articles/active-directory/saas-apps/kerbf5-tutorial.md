---
title: Руководство по Интеграция единого входа Azure AD с F5 | Документация Майкрософт
description: Узнайте, как настроить единый вход между Azure Active Directory и F5.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 11/19/2019
ms.author: jeedes
ms.openlocfilehash: 9db53e36dee318d39d34d26a548d1d32cbbec3b2
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/25/2020
ms.locfileid: "91266077"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-f5"></a>Руководство по Интеграция единого входа Azure Active Directory с F5

В этом учебнике описано, как интегрировать F5 с Azure Active Directory (Azure AD). Интеграция F5 с Azure AD обеспечивает следующие возможности:

* Контроль доступа к F5 с помощью Azure AD.
* Автоматический вход пользователей в F5 с помощью учетных записей Azure AD.
* Централизованное управление учетными записями через портал Azure.

Чтобы узнать больше об интеграции приложений SaaS с Azure AD, прочитайте статью [Что такое доступ к приложениям и единый вход с помощью Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

## <a name="prerequisites"></a>Предварительные требования

Чтобы приступить к работе, потребуется следующее.

* Подписка Azure AD. Если у вас нет подписки, вы можете получить [бесплатную учетную запись](https://azure.microsoft.com/free/).

* Подписка F5 с поддержкой единого входа.

* Для развертывания совместного решения требуется следующая лицензия:
    * Пакет F5 BIG-IP&reg; Best (или)

    * отдельная лицензия F5 BIG-IP Access Policy Manager&trade; (APM).

    * Дополнительная лицензия F5 BIG-IP Access Policy Manager&trade; (APM) на имеющийся BIG-IP F5 BIG-IP&reg; Local Traffic Manager&trade; (LTM).

    * В дополнение к указанной выше лицензии система F5 также может быть лицензирована с помощью:

        * подписки на фильтрацию URL-адресов для использования базы данных категории URL-адресов;

        * подписки на F5 IP Intelligence для обнаружения и блокирования известных злоумышленников и вредоносного трафика;

        * аппаратного сетевого модуля безопасности (HSM) для защиты цифровых ключей и управления ими с целью обеспечения надежной проверки подлинности.

* Система F5 BIG-IP подготавливается с модулями APM (LTM является необязательным).

* Хотя это необязательно, настоятельно рекомендуется развернуть системы F5 в [группе устройств для синхронизации и отработки отказа](https://techdocs.f5.com/content/techdocs/en-us/bigip-14-1-0/big-ip-device-service-clustering-administration-14-1-0.html), которая включает в себя активную резервную пару с плавающим IP-адресом для обеспечения высокой доступности. Дальнейшая избыточность интерфейса реализуется с помощью протокола LACP. LACP управляет подключенными физическими интерфейсами как одним виртуальным интерфейсом (статистической группой) и обнаруживает все сбои интерфейса в группе.

* Для приложений Kerberos требуется локальная учетная запись службы AD для ограниченного делегирования.  Сведения о создании учетной записи делегирования AD см. в [документации по использованию F5](https://support.f5.com/csp/article/K43063049).

## <a name="access-guided-configuration"></a>Интерактивная конфигурация доступа

* Интерактивная конфигурация доступа поддерживается в F5 ТМОС версии 13.1.0.8 и выше. Если ваша система BIG-IP работает под управлением 13.1.0.8, обратитесь к разделу **Расширенная конфигурация**.

* Интерактивная конфигурация доступа обеспечивает совершенно новое и упрощенное взаимодействие с пользователем. Эта архитектура на основе рабочих процессов предоставляет интуитивно понятные и повторные действия по настройке, предназначенные для выбранной топологии.

* Прежде чем перейти к настройке, обновите интерактивную конфигурацию, загрузив последний пакет вариантов использования с сайта [downloads.f5.com](https://login.f5.com/resource/login.jsp?ctx=719748). Чтобы выполнить обновление, выполните описанную ниже процедуру.

    >[!NOTE]
    >Ниже приведены снимки экрана для последней выпущенной версии (BIG-IP 15.0 с AGC версии 5.0). Приведенные ниже действия по настройке допустимы для этого варианта использования начиная с версии 13.1.0.8 и до последней версии BIG-IP.

1. В пользовательском веб-интерфейсе F5 BIG-IP щелкните **Access >> Guided Configuration** (Доступ >> Интерактивная конфигурация).

2. На странице **Guided Configuration** (Интерактивная конфигурация) щелкните **Upgrade Guided Configuration** (Обновить интерактивную конфигурацию) в верхнем левом углу.

    ![Настройка F5 (Kerberos)](./media/kerbf5-tutorial/configure14.png) 

3. Во всплывающем окне Upgrade Guided Configuration (Обновить интерактивную конфигурацию) щелкните **Choose File** (Выбрать файл), чтобы отправить скачанный пакет вариантов использования, и нажмите кнопку **Upload and Install** (Отправить и установить).

    ![Настройка F5 (Kerberos)](./media/kerbf5-tutorial/configure15.png) 

4. После завершения обновления нажмите кнопку **Continue** (Продолжить).

    ![Настройка F5 (Kerberos)](./media/kerbf5-tutorial/configure16.png)

## <a name="scenario-description"></a>Описание сценария

В рамках этого руководства вы настроите и проверите единый вход Azure AD в тестовой среде.

* F5 поддерживает вход, инициированный **поставщиком услуг или поставщиком удостоверений**.
* Единый вход в F5 можно настроить тремя способами.

- [Настройка единого входа в F5 для приложения Kerberos](#configure-f5-single-sign-on-for-kerberos-application)

- [настройка единого входа в F5 для приложения на основе заголовка](headerf5-tutorial.md);

- [Настройка единого входа в F5 для расширенного приложения Kerberos](advance-kerbf5-tutorial.md)

### <a name="key-authentication-scenarios"></a>Сценарии проверки подлинности с использованием ключа

Помимо встроенной поддержки интеграции с Azure Active Directory для современных протоколов проверки подлинности, таких как Open ID Connect, SAML и WS-Fed, F5 обеспечивает для приложений с устаревшей проверкой подлинности безопасный внутренний и внешний доступ с помощью Azure AD, что позволяет использовать для них современные сценарии (например, доступ без пароля). К таким приложениям относятся:

* приложения с проверкой подлинности на основе заголовков;

* приложения с проверкой подлинности Kerberos;

* приложения с анонимной проверкой подлинности или без встроенной проверки подлинности;

* приложения с проверкой подлинности NTLM (защита с двойным подтверждением для пользователя);

* приложение на основе форм (защита с двойным подтверждением для пользователя).

## <a name="adding-f5-from-the-gallery"></a>Добавление F5 из коллекции

Чтобы настроить интеграцию F5 с Azure AD, необходимо добавить F5 из коллекции в список управляемых приложений SaaS.

1. Войдите на [портал Azure](https://portal.azure.com) с помощью личной учетной записи Майкрософт либо рабочей или учебной учетной записи.
1. В области навигации слева выберите службу **Azure Active Directory**.
1. Перейдите в колонку **Корпоративные приложения** и выберите **Все приложения**.
1. Чтобы добавить новое приложение, выберите **Новое приложение**.
1. В разделе **Добавление из коллекции** в поле поиска введите **F5**.
1. Выберите **F5** на панели результатов и добавьте это приложение. Подождите несколько секунд, пока приложение не будет добавлено в ваш клиент.

## <a name="configure-and-test-azure-ad-single-sign-on-for-f5"></a>Настройка и проверка единого входа Azure AD для F5

Настройте и проверьте единый вход с Azure AD в F5, используя тестового пользователя **B.Simon**. Для обеспечения работы единого входа необходимо установить связь между пользователем Azure AD и соответствующим пользователем в F5.

Чтобы настроить и проверить единый вход Azure AD в F5, выполните действия в следующих стандартных блоках:

1. **[Настройка единого входа Azure AD](#configure-azure-ad-sso)** необходима, чтобы пользователи могли использовать эту функцию.
    1. **[Создание тестового пользователя Azure AD](#create-an-azure-ad-test-user)** требуется для проверки работы единого входа Azure AD с помощью пользователя B.Simon.
    1. **[Назначение тестового пользователя Azure AD](#assign-the-azure-ad-test-user)** необходимо, чтобы позволить пользователю B.Simon использовать единый вход Azure AD.
1. **[Настройка единого входа в F5](#configure-f5-sso)** необходима, чтобы настроить параметры единого входа на стороне приложения.
    1. **[Создание тестового пользователя приложения F5](#create-f5-test-user)** требуется для того, чтобы в F5 существовал пользователь B.Simon, связанный с одноименным пользователем в Azure AD.
1. **[Проверка единого входа](#test-sso)** позволяет убедиться в правильности конфигурации.

## <a name="configure-azure-ad-sso"></a>Настройка единого входа Azure AD

Выполните следующие действия, чтобы включить единый вход Azure AD на портале Azure.

1. На [портале Azure](https://portal.azure.com/) на странице интеграции с приложением **F5** найдите раздел **Управление** и выберите **Единый вход**.
1. На странице **Выбрать метод единого входа** выберите **SAML**.
1. На странице **Настройка единого входа с помощью SAML** щелкните значок "Изменить" (значок пера), чтобы открыть диалоговое окно **Базовая конфигурация SAML** и изменить параметры.

   ![Изменение базовой конфигурации SAML](common/edit-urls.png)

1. Если вы хотите настроить приложение в режиме, инициируемом **поставщиком удостоверений**, в разделе **Базовая конфигурация SAML** введите значения следующих полей.

    а. В текстовом поле **Идентификатор** введите URL-адрес в формате `https://<YourCustomFQDN>.f5.com/`.

    b. В текстовом поле **URL-адрес ответа** введите URL-адрес в формате `https://<YourCustomFQDN>.f5.com/`.

1. Чтобы настроить приложение для работы в режиме, инициируемом **поставщиком услуг**, щелкните **Задать дополнительные URL-адреса** и выполните следующие действия.

    В текстовом поле **URL-адрес входа** введите URL-адрес в формате `https://<YourCustomFQDN>.f5.com/`.

    > [!NOTE]
    > Эти значения приведены для примера. Замените их фактическими значениями идентификатора, URL-адреса ответа и URL-адреса входа. Чтобы получить эти значения, обратитесь в [службу поддержки клиентов F5](https://support.f5.com/csp/knowledge-center/software/BIG-IP?module=BIG-IP%20APM45). Можно также посмотреть шаблоны в разделе **Базовая конфигурация SAML** на портале Azure.

1. На странице **Настройка единого входа с помощью SAML** в разделе **Сертификат подписи SAML** найдите элемент **XML метаданных федерации** и **Сертификат (Base64)** , а затем нажмите кнопку **Скачать**, чтобы скачать сертификат и сохранить его на компьютере.

    ![Ссылка для скачивания сертификата](common/metadataxml.png)

1. Требуемые URL-адреса можно скопировать из раздела **Настройка F5**.

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

В этом разделе описано, как включить единый вход в Azure для пользователя B.Simon, предоставив этому пользователю доступ к F5.

1. На портале Azure выберите **Корпоративные приложения**, а затем — **Все приложения**.
1. В списке приложений выберите **F5**.
1. На странице "Обзор" приложения найдите раздел **Управление** и выберите **Пользователи и группы**.

   ![Ссылка "Пользователи и группы"](common/users-groups-blade.png)

1. Выберите **Добавить пользователя**, а в диалоговом окне **Добавление назначения** выберите **Пользователи и группы**.

    ![Ссылка "Добавить пользователя"](common/add-assign-user.png)

1. В диалоговом окне **Пользователи и группы** выберите **B.Simon** в списке пользователей, а затем в нижней части экрана нажмите кнопку **Выбрать**.
1. Если ожидается, что в утверждении SAML будет получено какое-либо значение роли, то в диалоговом окне **Выбор роли** нужно выбрать соответствующую роль для пользователя из списка и затем нажать кнопку **Выбрать**, расположенную в нижней части экрана.
1. В диалоговом окне **Добавление назначения** нажмите кнопку **Назначить**.
1. Щелкните **Условный доступ**.
1. Выберите **Создать политику**.
1. Теперь приложение F5 отображается как ресурс для политики условного доступа. Вы можете применить любой условный доступ, включая многофакторную проверку подлинности, управление доступом на основе устройств или политику защиты идентификации.

## <a name="configure-f5-sso"></a>Настройка единого входа в F5

- [настройка единого входа в F5 для приложения на основе заголовка](headerf5-tutorial.md);

- [Настройка единого входа в F5 для расширенного приложения Kerberos](advance-kerbf5-tutorial.md)

### <a name="configure-f5-single-sign-on-for-kerberos-application"></a>Настройка единого входа в F5 для приложения Kerberos

### <a name="guided-configuration"></a>Интерактивная конфигурация

1. Откройте новое окно веб-браузера, зайдите на сайт F5 (Kerberos) с правами администратора и сделайте следующее:

1. Понадобится импортировать в F5 сертификат метаданных, который будет использоваться позже в процессе настройки.

1. Выберите **System > Certificate Management > Traffic Certificate Management > SSL Certificate List** (Система > Управление сертификатами > Управление сертификатами трафика > Список SSL-сертификатов). В правом верхнем углу нажмите кнопку **Import** (Импортировать). Укажите **имя сертификата** (понадобится позднее в конфигурации). В поле **Certificate Source** (Источник сертификата) выберите Upload File (Отправить файл) и укажите сертификат, скачанный из Azure при настройке единого входа SAML. Щелкните **Импорт**.

    ![Настройка F5 (Kerberos)](./media/kerbf5-tutorial/configure01.png) 

1. Кроме того, **для имени узла приложения потребуется SSL-сертификат. Выберите System > Certificate Management > Traffic Certificate Management > SSL Certificate List** (Система > Управление сертификатами > Управление сертификатами трафика > Список SSL-сертификатов). В правом верхнем углу нажмите кнопку **Import** (Импортировать). Выберите в качестве **типа импорта** **PKCS 12(IIS)** . Укажите **имя ключа** (понадобится позднее в конфигурации) и PFX-файл. Введите **пароль** для PFX-файла. Щелкните **Импорт**.

    >[!NOTE]
    >В примере имя нашего приложения — `Kerbapp.superdemo.live`. Мы используем групповой сертификат с именем ключа `WildCard-SuperDemo.live`.

    ![Настройка F5 (Kerberos)](./media/kerbf5-tutorial/configure02.png) 
 
1. Мы используем интерактивный интерфейс для настройки федерации Azure AD и доступа к приложениям. Перейдите в раздел F5 BIG-IP **Main** (Основные) и выберите **Access > Guided Configuration > Federation > SAML Service Provider** (Доступ > Интерактивная конфигурация > Федерация > Поставщик службы SAML). Нажмите кнопку **Next** (Далее) **дважды**, чтобы начать настройку.

    ![Настройка F5 (Kerberos)](./media/kerbf5-tutorial/configure03.png) 

    ![Настройка F5 (Kerberos)](./media/kerbf5-tutorial/configure04.png)

1. Укажите **имя конфигурации**. Укажите **идентификатор сущности** (тот же, который вы настроили в конфигурации приложения Azure AD). Укажите **имя узла**. Добавьте **описание** для справки. Примите остальные записи по умолчанию, а затем нажмите кнопку **Save & Next** (Сохранить и продолжить).

    ![Настройка F5 (Kerberos)](./media/kerbf5-tutorial/configure05.png) 

1. В этом примере мы создаем виртуальный сервер с адресом 192.168.30.200 и портом 443. Укажите IP-адрес виртуального сервера в поле **Destination Address** (Адрес назначения). В разделе **Client SSL Profile** (Профиль SSL клиента) выберите Create new (Создать). Укажите ранее переданный сертификат приложения (в этом примере — групповой сертификат) и связанный с ним ключ, а затем нажмите кнопку **Save & Next** (Сохранить и продолжить).

    >[!NOTE]
    >В этом примере наш внутренний сервер работает на порту 80, а мы хотим опубликовать его с помощью порта 443.

    ![Настройка F5 (Kerberos)](./media/kerbf5-tutorial/configure06.png)

1. В разделе **Select method to configure your IdP connector** (Выбрать метод настройки соединителя IdP) укажите метаданные, щелкните Choose File (Выбрать файл) и отправьте XML-файл метаданных, скачанный ранее из Azure AD. Укажите уникальное **имя** для соединителя поставщика удостоверений SAML. Выберите **Metadata Signing Certificate** (Сертификат для подписи метаданных) который был отправлен ранее. Нажмите кнопку **Save & Next** (Сохранить и продолжить).

    ![Настройка F5 (Kerberos)](./media/kerbf5-tutorial/configure07.png)  

1. В разделе **Select a Pool** (Выбор пула) выберите **Create New** (Создать) (можно также выбрать пул, который уже существует). Оставьте для другого параметра значение по умолчанию.    В разделе серверов пула введите IP-адрес в поле **IP Address/Node Name** (IP-адрес или имя узла). Укажите **порт**. Нажмите кнопку **Save & Next** (Сохранить и продолжить).
 
    ![Настройка F5 (Kerberos)](./media/kerbf5-tutorial/configure08.png)

1. На экране единого входа выберите **Enable Single Sign-On** (Включить единый вход). В разделе **Selected Single Sign-On Type** (Выбранный тип единого входа) выберите **Kerberos**. Замените **session.saml.last.Identity** на **session.saml.last.attr.name.Identity** в разделе **Username Source** (Источник имени пользователя) (эта переменная задается с помощью сопоставления утверждений в Azure AD). Выберите **Show Advanced Setting** (Показать дополнительный параметр). В разделе **Kerberos Realm** (Область Kerberos) введите имя домена. В разделе **Account Name/ Account Password** (Имя учетной записи или пароль учетной записи) укажите учетную запись и пароль для делегирования APM. Укажите IP-адрес контроллера домена в поле **KDC**. Нажмите кнопку **Save & Next** (Сохранить и продолжить).

    ![Настройка F5 (Kerberos)](./media/kerbf5-tutorial/configure09.png)   

1. В этом руководстве мы пропустим проверку конечных точек.  Дополнительные сведения см. в документации по F5.  На экране выберите **Save & Next** (Сохранить и продолжить).

    ![Настройка F5 (Kerberos)](./media/kerbf5-tutorial/configure10.png) 

1. Примите значения по умолчанию и нажмите кнопку **Save & Next** (Сохранить и продолжить). Дополнительные сведения о параметрах управления сеансами SAML см. в документации по F5.


    ![Настройка F5 (Kerberos)](./media/kerbf5-tutorial/configure11.png) 
 
1. Проверьте экран сводки и нажмите кнопку **Deploy** (Развернуть), чтобы настроить BIG-IP.
 
    ![Настройка F5 (Kerberos)](./media/kerbf5-tutorial/configure12.png)

1. После развертывания приложения щелкните **Finish** (Готово).

    ![Настройка F5 (Kerberos)](./media/kerbf5-tutorial/configure13.png)

## <a name="advanced-configuration"></a>Расширенная настройка

>[!NOTE]
>Для получения справки щелкните [здесь](https://techdocs.f5.com/kb/en-us/products/big-ip_apm/manuals/product/apm-authentication-single-sign-on-11-5-0/2.html).

### <a name="configuring-an-active-directory-aaa-server"></a>Настройка сервера Active Directory AAA

Вы настраиваете сервер Active Directory AAA в Access Policy Manager (APM), чтобы указать контроллеры домена и учетные данные, которые APM будет использовать для проверки подлинности пользователей.

1. На главной вкладке щелкните **Access Policy > AAA Servers > Active Directory** (Политика доступа > Серверы AAA > Active Directory). Откроется экран со списком серверов Active Directory.

2. Нажмите кнопку **Создать**. Откроется экран свойств нового сервера.

3. В поле **Name** (Имя) введите уникальное имя сервера проверки подлинности.

4. В поле **Domain Name** (Доменное имя) введите имя домена Windows.

5. Для параметра**Server Connection** (Подключение к серверу) выберите один из следующих вариантов:

   * выберите **Use Pool** (Использовать пул), чтобы настроить высокий уровень доступности для сервера AAA;

   * выберите **Direct** (Напрямую), чтобы настроить сервер AAA для работы в автономном режиме.

6. Если вы выбрали **Direct** (Напрямую), введите имя в поле **Domain Controller** (Контроллер домена).

7. Если вы выбрали **Use Pool** (Использовать пул), настройте пул.

   * Введите имя в поле **Domain Controller Pool Name** (Имя пула контроллера домена).

   * Укажите **контроллеры домена** в пуле, введя IP-адрес и имя узла для каждого из них, а затем нажмите кнопку **Add** (Добавить).

   * Чтобы отслеживать работоспособность сервера AAA, можно выбрать монитор работоспособности. В этом случае подходит только монитор — **gateway_icmp**. Его можно выбрать в списке **Server Pool Monitor** (Монитор пула серверов).

8. В поле **Admin Name** (Имя администратора) введите имя с учетом регистра для администратора с правами администратора Active Directory. APM использует сведения, указанные в полях **Admin Name** (Имя администратора) и **Admin Password** (Пароль администратора), для запросов AD. Если Active Directory настроен для анонимных запросов, указывать имя администратора не нужно. В противном случае APM требуется учетная запись с достаточными привилегиями для привязки к серверу Active Directory, получения сведений о группе пользователей и политик паролей Active Directory для поддержки функций, связанных с паролями. (APM должен получить политики паролей, например, если в действии запроса AD выбран параметр напоминать пользователям об истечении срока действия пароля заранее.) Если в этой конфигурации не указать сведения об учетной записи администратора, APM будет получать информацию с помощью учетной записи пользователя. Это работает, если учетная запись пользователя имеет достаточно прав.

9. В поле **Admin Password** (Пароль администратора) введите пароль администратора, связанный с доменным именем.

10. В поле **Verify Admin Password** (Проверка пароля администратора) введите пароль администратора, связанный с параметром **Domain Name** (Доменное имя).

11. В поле **Group Cache Lifetime** (Время существования кэша группы) введите число дней. Время существования по умолчанию составляет 30 дней.

12. В поле **Password Security Object Cache Lifetime** (Время существования кэша объектов безопасности паролей) введите число дней. Время существования по умолчанию составляет 30 дней.

13. В списке **Kerberos Preauthentication Encryption Type** (Тип шифрования предварительной проверки подлинности Kerberos) выберите тип шифрования. Значение по умолчанию — **None**. Если указать тип шифрования, система BIG-IP добавляет данные предварительной проверки подлинности Kerberos в первый пакет запроса на обслуживание проверки подлинности (AS-REQ).

14. В поле **Timeout** (Время ожидания) введите интервал времени ожидания (в секундах) для сервера AAA. (Это необязательный параметр.)

15. Нажмите кнопку **Finished** (Готово). Новый сервер отобразится в списке. Он будет добавлен в список серверов Active Directory.

    ![Настройка F5 (Kerberos)](./media/kerbf5-tutorial/configure17.png)

### <a name="saml-configuration"></a>Настройка SAML

1. Понадобится импортировать в F5 сертификат метаданных, который будет использоваться позже в процессе настройки. Выберите **System > Certificate Management > Traffic Certificate Management > SSL Certificate List** (Система > Управление сертификатами > Управление сертификатами трафика > Список SSL-сертификатов). В правом верхнем углу нажмите кнопку **Import** (Импортировать).

    ![Настройка F5 (Kerberos)](./media/kerbf5-tutorial/configure18.png)

2. Чтобы настроить поставщик удостоверений SAML, выберите **Access > Federation > SAML: Service Provider > External Idp Connectors** (Доступ > Федерация > SAML: поставщик служб > Внешние соединители поставщика удостоверений) и щелкните **Create > From Metadata** (Создать > Из метаданных).

    ![Настройка F5 (Kerberos)](./media/kerbf5-tutorial/configure19.png)

    ![Настройка F5 (Kerberos)](./media/kerbf5-tutorial/configure20.png)

    ![Настройка F5 (Kerberos)](./media/kerbf5-tutorial/configure21.png)

    ![Настройка F5 (Kerberos)](./media/kerbf5-tutorial/configure22.png)

    ![Настройка F5 (Kerberos)](./media/kerbf5-tutorial/configure23.png)

    ![Настройка F5 (Kerberos)](./media/kerbf5-tutorial/configure24.png)

1. Чтобы настроить поставщик служб (SP) SAML, выберите **Access > Federation > SAML Service Provider > Local SP Services** (Доступ > Федерация > Поставщик служб SAML > Локальные службы SP) и щелкните **Create** (Создать). Введите следующие сведения и нажмите кнопку **ОК**.

    * Имя типа: KerbApp200SAML.
    * Идентификатор сущности*: https://kerb-app.com.cutestat.com.
    * Параметры имени поставщика служб.
    * Схема: HTTPS.
    * Узел: kerbapp200.superdemo.live.
    * Описание: kerbapp200.superdemo.live.

     ![Настройка F5 (Kerberos)](./media/kerbf5-tutorial/configure25.png)

     b. Выберите конфигурацию поставщика служб, KerbApp200SAML, и щелкните **Bind/UnBind IdP Connectors** (Привязать/отменить привязку соединителей IdP).

     ![Настройка F5 (Kerberos)](./media/kerbf5-tutorial/configure26.png)

     ![Настройка F5 (Kerberos)](./media/kerbf5-tutorial/configure27.png)

     c. Щелкните **Add New Row** (Добавить новую строку) и выберите **External IdP connector** (Внешний соединитель IdP), созданный на предыдущем шаге, щелкните **Update** (Обновить), а затем нажмите кнопку **ОК**.

     ![Настройка F5 (Kerberos)](./media/kerbf5-tutorial/configure28.png)

1. Для настройки единого входа Kerberos выберите **Access > Single Sign-on > Kerberos** (Доступ > Единый вход > Kerberos), введите сведения и щелкните **Finished** (Завершено).

    >[!Note]
    > Вам потребуется создать и указать учетную запись делегирования Kerberos. См. раздел KCD (см. приложение для ссылок на переменные).

    * **Источник имени пользователя**: session.saml.last.attr.name.http:\//schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname

    * **Источник области пользователя**: session.logon.last.domain.

        ![Настройка F5 (Kerberos)](./media/kerbf5-tutorial/configure29.png)

1. Чтобы настроить профиль доступа, выберите **Access > Profile/Policies > Access Profile (per session policies)** (Доступ > Профиль/политики > Профиль доступа (политики для каждого сеанса)), щелкните **Create** (Создать), введите следующие сведения и щелкните **Finished** (Завершено).

    * Имя: KerbApp200.
    * Тип профиля: All
    * Область профиля: Профиль
    * Языки: Английский

        ![Настройка F5 (Kerberos)](./media/kerbf5-tutorial/configure30.png)

1. Щелкните имя, KerbApp200, введите следующие сведения и нажмите кнопку **Update** (Обновить).

    * Файл cookie домена: superdemo.live.
    * Настройка единого входа: KerAppSSO_sso.

        ![Настройка F5 (Kerberos)](./media/kerbf5-tutorial/configure31.png)

1. Щелкните **Access Policy** (Политика доступа), а затем выберите **Edit Access Policy** (Изменить политику доступа) для профиля KerbApp200.

    ![Настройка F5 (Kerberos)](./media/kerbf5-tutorial/configure32.png)

    ![Настройка F5 (Kerberos)](./media/kerbf5-tutorial/configure33.png)

    ![Настройка F5 (Kerberos)](./media/kerbf5-tutorial/configure34.png)

    * **session.logon.last.usernameUPN   expr {[mcget {session.saml.last.identity}]}**

    * **session.ad.lastactualdomain  TEXT superdemo.live**

        ![Настройка F5 (Kerberos)](./media/kerbf5-tutorial/configure35.png)

    * **(userPrincipalName=%{session.logon.last.usernameUPN})**

        ![Настройка F5 (Kerberos)](./media/kerbf5-tutorial/configure36.png)

        ![Настройка F5 (Kerberos)](./media/kerbf5-tutorial/configure37.png)

    * **session.logon.last.username  expr { "[mcget {session.ad.last.attr.sAMAccountName}]" }**

        ![Настройка F5 (Kerberos)](./media/kerbf5-tutorial/configure38.png)

    * **mcget {session.logon.last.username}**
    * **mcget {session.logon.last.password}**

1. Для добавления нового узла выберите **Local Traffic > Nodes > Node List (Локальный трафик > Узлы > Список узлов), щелкните Create (Создать)** , введите следующие сведения, а затем щелкните **Finished** (Завершено).

    * Имя: KerbApp200.
    * Описание. KerbApp200.
    * Адрес: 192.168.20.200.

        ![Настройка F5 (Kerberos)](./media/kerbf5-tutorial/configure39.png)

1. Для создания пула выберите **Local Traffic > Pools > Pool List (Локальный трафик > Пулы > Список пулов), щелкните Create (Создать)** , введите следующие сведения и щелкните **Finished** (Завершено).

    * Имя: KerbApp200-Pool.
    * Описание. KerbApp200-Pool.
    * Мониторы работоспособности: http.
    * Адрес: 192.168.20.200.
    * Порт службы: 81

        ![Настройка F5 (Kerberos)](./media/kerbf5-tutorial/configure40.png)

1. Для создания виртуального сервера выберите **Local Traffic > Virtual Servers > Virtual Server List > + (Локальный трафик > Виртуальные серверы > Список виртуальных серверов > +)** , введите следующие сведения и щелкните **Finished** (Завершено).

    * Имя: KerbApp200.
    * Адрес назначения или маска: узел 192.168.30.200.
    * Порт службы: порт 443 HTTPS.
    * Профиль доступа: KerbApp200.
    * Укажите профиль доступа, созданный на предыдущем шаге

        ![Настройка F5 (Kerberos)](./media/kerbf5-tutorial/configure41.png)

        ![Настройка F5 (Kerberos)](./media/kerbf5-tutorial/configure42.png)

### <a name="setting-up-kerberos-delegation"></a>Настройка делегирования Kerberos 

>[!NOTE]
>Для получения справки щелкните [здесь](https://www.f5.com/pdf/deployment-guides/kerberos-constrained-delegation-dg.pdf).

*  **Шаг 1**. Создание учетной записи делегирования

    **Пример**.
    * Доменное имя: **superdemo.live**.

    * Имя учетной записи SAM: **big-ipuser**.

    * New-ADUser -Name "APM Delegation Account" -UserPrincipalName host/big-ipuser.superdemo.live@superdemo.live -SamAccountName "big-ipuser" -PasswordNeverExpires $true -Enabled $true -AccountPassword (Read-Host -AsSecureString "Password!1234")

* **Шаг 2**. Установка имени субъекта-службы (для учетной записи делегирования APM)

    **Пример**.
    * setspn –A **host/big-ipuser.superdemo.live** big-ipuser

* **Шаг 3**. Делегирование имени субъекта-службы (для учетной записи службы приложений). Настройте соответствующее делегирование для учетной записи делегирования F5.
    В приведенном ниже примере для учетной записи делегирования APM настраивается ограниченное делегирование Kerberos для приложения FRP-App1.superdemo. live.

    ![Настройка F5 (Kerberos)](./media/kerbf5-tutorial/configure43.png)

* Укажите сведения, указанные в приведенном выше справочном [документе](https://techdocs.f5.com/kb/en-us/products/big-ip_apm/manuals/product/apm-authentication-single-sign-on-11-5-0/2.html).

### <a name="create-f5-test-user"></a>Создание тестового пользователя в F5

В этом разделе вы узнаете, как создать пользователя B.Simon в приложении F5. Обратитесь к  [группе поддержки клиентов F5](https://support.f5.com/csp/knowledge-center/software/BIG-IP?module=BIG-IP%20APM45), чтобы добавить пользователей на платформу F5. Перед использованием единого входа необходимо создать и активировать пользователей. 

## <a name="test-sso"></a>Проверка единого входа 

В этом разделе описано, как проверить конфигурацию единого входа Azure AD с помощью панели доступа.

Щелкнув плитку F5 на Панели доступа, вы автоматически войдете в F5, для которого настроили единый вход. См. дополнительные сведения о [панели доступа](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)

## <a name="additional-resources"></a>Дополнительные ресурсы

- [Список учебников по интеграции приложений SaaS с Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Что такое доступ к приложениям и единый вход с помощью Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Что представляет собой условный доступ в Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Попробуйте использовать F5 с Azure AD](https://aad.portal.azure.com/)

- [настройка единого входа в F5 для приложения на основе заголовка](headerf5-tutorial.md);

- [Настройка единого входа в F5 для расширенного приложения Kerberos](advance-kerbf5-tutorial.md)

