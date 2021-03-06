---
title: Добавление условного доступа к потоку пользователя в Azure AD B2C
description: Узнайте, как добавить условный доступ к потокам пользователей Azure AD B2C. Настройте параметры многофакторной проверки подлинности (MFA) и политик условного доступа в потоках пользователей, чтобы применять эти политики и устранять риски, связанные с рискованными входами.
services: active-directory
ms.service: active-directory
ms.subservice: conditional-access
ms.topic: overview
ms.date: 09/01/2020
ms.author: mimart
author: msmimart
manager: celested
ms.collection: M365-identity-device-management
ms.openlocfilehash: 60bfac3b80e772e7b359b1e926d5fb84e447a8fb
ms.sourcegitcommit: eb6bef1274b9e6390c7a77ff69bf6a3b94e827fc
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/05/2020
ms.locfileid: "89270797"
---
# <a name="add-conditional-access-to-user-flows-in-azure-active-directory-b2c"></a>Добавление условного доступа к потокам пользователей в Azure Active Directory B2C

[!INCLUDE [b2c-public-preview-feature](../../includes/active-directory-b2c-public-preview.md)]

В потоки пользователей Azure Active Directory B2C можно добавить функцию условного доступа для управления рискованными входами в приложения. Интеграция защиты идентификации и условного доступа в Azure AD B2C позволяет настроить политики, которые идентифицируют поведение, характерное для рискованных входов, и требуют дополнительных действий от пользователя или администратора. Следующие компоненты обеспечивают условный доступ в потоках пользователей Azure AD B2C:

- **Поток пользователя.** Создайте поток пользователя, который выполняет пользователь для входа и регистрации. В параметрах потока пользователя укажите, следует ли активировать политики условного доступа, когда этот поток выполняется для пользователя.
- **Приложение, направляющее пользователей в поток пользователя.** Настройте приложение так, чтобы оно направляло пользователей в правильный поток пользователя для регистрации и входа, указав в этом приложении конечную точку потока.
- **Политика условного доступа.** [Создайте политику условного доступа](conditional-access-identity-protection-setup.md) и укажите приложения, к которым должна применяться эта политика. Когда пользователь выполняет поток для входа или регистрации для приложения, политика условного доступа использует сигналы, поступающие от системы защиты идентификации, чтобы выявлять рискованные входы и при необходимости выполнять соответствующие действия по исправлению.

Условный доступ поддерживается только в последних версиях потоков пользователей. Вы можете добавлять политики условного доступа в новые потоки пользователей по мере их создания или существующие потоки пользователей, если текущая версия поддерживает условный доступ. При добавлении условного доступа в существующий поток пользователя вам нужно проверить следующие два параметра:

- **Многофакторная идентификация (MFA).** Теперь пользователи могут получать через SMS, голосовой звонок или электронную почту одноразовый пароль для многофакторной проверки подлинности. Параметры MFA не зависят от параметров условного доступа. Вы можете указать для MFA режим **Всегда включено**, чтобы многофакторная проверка подлинности применялась всегда независимо от конфигурации условного доступа. Также можно указать для MFA режим **Условный**, чтобы многофакторная проверка подлинности применялась, только когда этого требует активная политика условного доступа.

- **Условный доступ.** Этот параметр всегда должен иметь значение **Включено**. Обычно его **отключают** только на период устранения неполадок или миграции либо для устаревших реализаций.

См. сведения о [защите идентификации и условном доступе](conditional-access-identity-protection-overview.md) в Azure AD B2C, а также о [настройке этих возможностей](conditional-access-identity-protection-setup.md).

## <a name="add-conditional-access-to-a-new-user-flow"></a>Добавление условного доступа к новому потоку пользователя

1. Войдите на [портал Azure](https://portal.azure.com).
1. Выберите значок **Каталог и подписка** в верхней панели инструментов портала, а затем выберите каталог, содержащий клиент Azure AD B2C.
1. На портале Azure найдите и выберите **Azure AD B2C**.
1. В разделе **Политики** выберите **Потоки пользователей** и щелкните **Создать поток пользователя**.
1. На странице **Создание потока пользователя** выберите тип потока.
1. В разделе **Выбор версии**, выберите элемент **Рекомендуемая** и нажмите кнопку **Создать**. (См. сведения о [версиях потоков пользователей](user-flow-versions.md).)

    ![Страница создания потока пользователя на портале Azure с выделенными свойствами](./media/tutorial-create-user-flows/select-version.png)

1. Введите **имя** потока пользователя. Например, *signupsignin1*.
1. В разделе **Поставщики удостоверений** выберите поставщики удостоверений, которые вы хотите разрешить для этого потока пользователя.
2. В разделе **Многофакторная проверка подлинности** выберите нужный **метод MFA**, а затем в разделе **Принудительное применение MFA** выберите вариант **Условный (рекомендуется)** .
 
   ![Настройка многофакторной проверки подлинности](media/conditional-access-user-flow/configure-mfa.png)

1. В разделе **Условный доступ** установите флажок **Принудительное применение политик условного доступа**.

   ![Настройка параметров условного доступа](media/conditional-access-user-flow/configure-conditional-access.png)

1. В разделе **Атрибуты пользователя и утверждения** выберите утверждения и атрибуты, которые вы хотите собирать и отправлять во время регистрации пользователя. Например, щелкните **Показать еще**, а затем выберите атрибуты и утверждения **Страна или регион** и **Отображаемое имя**. Нажмите кнопку **ОК**.

    ![Страница выбора атрибутов и утверждений с тремя выбранными утверждениями](./media/conditional-access-user-flow/configure-user-attributes-claims.png)

1. Для добавления потока пользователя щелкните **Создать**. Префикс *B2C_1* добавляется к имени автоматически.

## <a name="add-conditional-access-to-an-existing-user-flow"></a>Добавление условного доступа в существующий поток пользователя

> [!NOTE]
> Версия существующего потока пользователя должна поддерживать условный доступ. Такие версии потоков пользователей помечены как **рекомендуемые**.

1. Войдите на [портал Azure](https://portal.azure.com).

1. Выберите значок **Каталог и подписка** в верхней панели инструментов портала, а затем выберите каталог, содержащий клиент Azure AD B2C.

1. На портале Azure найдите и выберите **Azure AD B2C**.

1. В разделе **Политики** выберите **Потоки пользователей**. Затем выберите поток пользователя.

1. Щелкните **Свойства** и убедитесь, что поток пользователя поддерживает условный доступ, выбрав в разделе **Свойства** параметр **Условный доступ**.
 
   ![Настройка MFA и условного доступа в свойствах](media/conditional-access-user-flow/add-conditional-access.png)

1. В разделе **Многофакторная проверка подлинности** выберите нужный **метод MFA**, а затем в разделе **Принудительное применение MFA** выберите вариант **Условный (рекомендуется)** .
 
1. В разделе **Условный доступ** установите флажок **Принудительное применение политик условного доступа**.

1. Щелкните **Сохранить**.

## <a name="test-the-user-flow"></a>Тестирование потока пользователя

Чтобы проверить условный доступ в потоке пользователя, [создайте политику условного доступа](conditional-access-identity-protection-setup.md) и включите условный доступ в потоке пользователя, как описано выше. 

### <a name="prerequisites"></a>Предварительные требования

- Для создания политик рискованного входа требуется Azure AD B2C Premium 2. Арендаторы уровня Premium P1 могут создавать политики на основе расположения, приложения или группы.
- В целях тестирования вы можете [зарегистрировать тестовое веб-приложение](tutorial-register-applications.md) `https://jwt.ms`, которое поддерживается корпорацией Майкрософт и отображает декодированное содержимое токена (содержимое токена не выходит за пределы браузера). 
- Чтобы имитировать рискованный вход, скачайте браузер TOR и попытайтесь войти на конечную точку вашего потока пользователя.
- Используя следующие параметры, [создайте политику условного доступа](conditional-access-identity-protection-setup.md).
   
   - В поле **Пользователи и группы** выберите любого пользователя для теста (но не указывайте вариант **Все пользователи**, чтобы случайно не заблокировать вход самому себе).
   - В области **Облачные приложения или действия** щелкните **Выбрать приложения** и выберите нужное приложение проверяющей стороны.
   - В области "Условия" выберите **Риск при входе** и укажите уровни риска **Высокий**, **Средний** и **Низкий**.
   - В поле **Предоставить** выберите **Запретить доступ**.

      ![Обнаружение рисков](media/conditional-access-identity-protection-setup/test-conditional-access-policy.png)

### <a name="run-the-user-flow"></a>Запуск потока пользователя

1. Выберите созданный поток пользователя, чтобы открыть его страницу обзора, а затем щелкните **Выполнить поток пользователя**. В поле **Приложение** выберите *webapp1*. В поле **URL-адрес ответа** должно содержаться значение `https://jwt.ms`.

   ![Страница выполнения потока пользователя на портале с выделенной кнопкой "Выполнить поток пользователя"](./media/tutorial-create-user-flows/signup-signin-run-now.PNG)

1. Скопируйте URL-адрес в разделе **Конечная точка выполнения потока пользователя**.

1. Чтобы имитировать рискованный вход, откройте [браузер Tor](https://www.torproject.org/download/) и перейдите на URL-адрес, скопированный на предыдущем шаге, чтобы войти в зарегистрированное приложение.

1. Введите требуемые сведения на странице входа и попробуйте выполнить вход. Токен должен вернуться по адресу `https://jwt.ms` и отобразиться пользователю. В декодированном токене jwt.ms вы увидите, что вход был заблокирован:

   ![Тестирование запрета входа](media/conditional-access-identity-protection-setup/test-blocked-sign-in.png)

## <a name="next-steps"></a>Дальнейшие действия

[Настройка пользовательского интерфейса в потоке пользователя Azure AD B2C](customize-ui-overview.md)
