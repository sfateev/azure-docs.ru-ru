---
title: Вход без пароля с помощью Microsoft Authenticator приложения Azure Active Directory
description: Включение входа без пароля в Azure AD с помощью Microsoft Authenticator приложения (Предварительная версия)
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: how-to
ms.date: 09/29/2020
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: librown
ms.collection: M365-identity-device-management
ms.openlocfilehash: 053a489993c31344b96e83253c88eed93b27b145
ms.sourcegitcommit: d103a93e7ef2dde1298f04e307920378a87e982a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/13/2020
ms.locfileid: "91964831"
---
# <a name="enable-passwordless-sign-in-with-the-microsoft-authenticator-app-preview"></a>Включение входа без пароля с помощью приложения Microsoft Authenticator (Предварительная версия)

Приложение Microsoft Authenticator позволяет войти в любую учетную запись Azure AD, не используя пароль. Как и аналогичная технология [Windows Hello для бизнеса](/windows/security/identity-protection/hello-for-business/hello-identity-verification), Microsoft Authenticator использует аутентификацию на основе ключей, привязывает учетные данные пользователя к определенному устройству и применяет биометрические данные или ПИН-код. Этот метод проверки подлинности можно использовать на любой платформе устройства, включая мобильные приложения, и с любым приложением или веб-сайтом, интегрированным с библиотеками проверки подлинности Майкрософт.

![Пример входа в браузер с запросом пользователя на утверждение входа](./media/howto-authentication-passwordless-phone/phone-sign-in-microsoft-authenticator-app.png)

Вместо того чтобы выводить запрос на ввод пароля после ввода имени пользователя, пользователь, который включил вход с помощью телефона из приложения Microsoft Authenticator, увидит сообщение с запросом на касание номера в своем приложении. Чтобы завершить процесс входа в приложение, пользователь должен сопоставить это число, выбрать **утвердить**, а затем указать ПИН-код или биометрическую метрику.

## <a name="prerequisites"></a>Предварительные требования

Чтобы использовать вход без пароля для входа в приложение Microsoft Authenticator, должны быть выполнены следующие условия.

- Многофакторная идентификация Azure с Push-уведомлениями, разрешенными в качестве метода проверки.
- Последняя версия Microsoft Authenticator на устройствах под управлением iOS версии 8.0 или более поздней версии или Android 6.0 или более поздней версии.

> [!NOTE]
> Если вы включили предварительную версию входа без пароля для приложения Microsoft Authenticator с помощью Azure AD PowerShell, она была включена для всего каталога. Если вы включили использование этого нового метода, он заменяет политику PowerShell. Рекомендуется включить для всех пользователей в клиенте через меню " *методы проверки подлинности* ". в противном случае пользователи, не воявляющиеся в новую политику, больше не смогут выполнять вход без пароля.

## <a name="enable-passwordless-authentication-methods"></a>Включить методы проверки подлинности, не имеющие пароля

Чтобы использовать проверку подлинности без пароля в Azure AD, сначала включите Объединенный процесс регистрации, а затем включите для пользователей метод уменьшения пароля.

### <a name="enable-the-combined-registration-experience"></a>Включение объединенного процесса регистрации

Функции регистрации для методов проверки подлинности, не защищенных паролем, зависят от функции общей регистрации. Чтобы разрешить пользователям выполнять объединенную регистрацию, выполните действия, чтобы [включить объединенную регистрацию сведений о безопасности](howto-registration-mfa-sspr-combined.md).

### <a name="enable-passwordless-phone-sign-in-authentication-methods"></a>Включить методы проверки подлинности для входа с помощью телефона без пароля

Azure AD позволяет выбирать методы проверки подлинности, которые могут использоваться во время входа в систему. Затем пользователи регистрируются для использования методов, которые они хотят использовать.

Чтобы включить метод проверки подлинности для входа без пароля, выполните следующие действия.

1. Войдите в [портал Azure](https://portal.azure.com) с помощью учетной записи *глобального администратора* .
1. Найдите и выберите *Azure Active Directory*, а затем перейдите к методам проверки подлинности **Безопасность**  >  **Authentication methods**  >  **Политика метода проверки подлинности (Предварительная версия)** .
1. В разделе **безпарольный вход**с помощью телефона выберите следующие параметры.
   1. **Включить** -да или нет
   1. **Target** — все пользователи или выбранные пользователи
1. Чтобы применить новую политику, нажмите кнопку **сохранить**.

## <a name="user-registration-and-management-of-microsoft-authenticator-app"></a>Регистрация пользователей и управление Microsoft Authenticator приложением

С помощью метода проверки подлинности без пароля, который можно использовать в Azure AD, пользователи должны зарегистрироваться для использования метода проверки подлинности без пароля, выполнив следующие действия.

1. Перейдите по ссылке [https://aka.ms/mysecurityinfo](https://aka.ms/mysecurityinfo).
1. Войдите, а затем добавьте приложение Authenticator, выбрав **Добавить метод > средство проверки подлинности**, а затем **Добавить**.
1. Следуйте инструкциям по установке и настройке приложения Microsoft Authenticator на устройстве.
1. Выберите **Готово** , чтобы завершить настройку средства проверки подлинности.
1. В **Microsoft Authenticator** приложении выберите параметр **включить вход** с помощью телефона в раскрывающемся меню для зарегистрированной учетной записи.
1. Следуйте инструкциям в приложении, чтобы завершить регистрацию учетной записи для входа на телефон без пароля.

Организации могут направлять пользователей для [входа с помощью телефона, а не пароля](../user-help/user-help-auth-app-sign-in.md) для дальнейшей помощи в настройке Microsoft Authenticator приложения и включении входа с помощью телефона.

> [!NOTE]
> Пользователи, которым не разрешено использовать вход с помощью телефона, больше не могут включить его в приложении Microsoft Authenticator.  

## <a name="sign-in-with-passwordless-credential"></a>Вход с использованием учетных данных без пароля

Пользователь может начать использовать безпарольный вход после того, как администратор включил свой клиент, и пользователь обновил свое приложение Microsoft Authenticator, чтобы включить вход с помощью телефона.

Чтобы начать использовать вход с помощью телефона, после ввода имени пользователя на странице входа и нажатия кнопки **Далее**пользователям может потребоваться выбрать **другие способы входа**, а затем **утвердить запрос в Microsoft Authenticator приложении**. После этого пользователи получают номер, и в Microsoft Authenticator приложении появится запрос на выбор подходящего номера для проверки подлинности вместо использования пароля. После того как пользователи применяют вход без пароля, им предлагается снова использовать его, пока они не будут использовать команду выбрать другой метод.

![Пример входа в браузер с помощью приложения Microsoft Authenticator](./media/howto-authentication-passwordless-phone/web-sign-in-microsoft-authenticator-app.png)

## <a name="known-issues"></a>Известные проблемы

В текущем интерфейсе предварительной версии существуют следующие известные проблемы.

### <a name="not-seeing-option-for-passwordless-phone-sign-in"></a>Не отображается параметр для входа на телефон без пароля

Если у пользователя есть ожидающая проверка входа без пароля, а попытки входа еще не выполняются, пользователь может увидеть только параметр ввода пароля. Откройте Microsoft Authenticator и ответьте на любые запросы уведомлений, чтобы продолжить использовать вход с использованием телефона без пароля.

### <a name="federated-accounts"></a>Федеративные учетные записи

Если пользователь включил учетные данные без пароля, то имена входа Azure AD будут остановлены с помощью login_hint для ускорения пользователя до федеративного расположения входа. Эта логика запрещает пользователям гибридного клиента направлять AD FS для проверки входа, не выполняя дополнительные действия по нажатию кнопки "использовать пароль".

### <a name="azure-mfa-server"></a>Сервер Azure MFA

Конечные пользователи, которым разрешено использование MFA через локальный сервер Azure MFA Организации, могут по-прежнему создавать и использовать один и тот же учетные данные для входа в систему без пароля. Если пользователь введет одну учетную запись в несколько (5 и более) установок Microsoft Authenticator, это может привести к ошибке.  

### <a name="device-registration"></a>Регистрация устройства

Одним из предварительных условий для создания новых надежных учетных данных является то, что устройство, на котором установлено Microsoft Authenticator приложение, также должно быть зарегистрировано в клиенте Azure AD для отдельного пользователя. Из-за текущих ограничений на регистрацию устройств устройство может быть зарегистрировано только в одном клиенте. Это означает, что в приложении Microsoft Authenticator можно настроить только одну рабочую или учебную учетную запись для входа с телефона.

> [!NOTE]
> Регистрация устройств не аналогична управлению устройствами или MDM. Он связывает только идентификатор устройства и идентификатор пользователя вместе в каталоге Azure AD.  

## <a name="next-steps"></a>Дальнейшие действия

Дополнительные сведения о проверке подлинности Azure AD и методах, не имеющих пароля, см. в следующих статьях:

* [Узнайте, как работает проверка подлинности с паролем](concept-authentication-passwordless.md)
* [Сведения о регистрации устройств](../devices/overview.md#getting-devices-in-azure-ad)
* [Сведения о многофакторной идентификации Azure](../authentication/howto-mfa-getstarted.md)
