---
title: Руководство по настройке Azure Active Directory B2C с помощью Вхоиам
titleSuffix: Azure AD B2C
description: В этом руководстве описано, как интегрировать проверку подлинности Azure AD B2C с Вхоиам для проверки пользователей.
services: active-directory-b2c
author: gargi-sinha
manager: martinco
ms.service: active-directory
ms.workload: identity
ms.topic: how-to
ms.date: 08/20/2020
ms.author: gasinh
ms.subservice: B2C
ms.openlocfilehash: 718ccbaa57ffe9f4ebaf4e8df448b602ba8cc3fa
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2020
ms.locfileid: "89293157"
---
# <a name="tutorial-for-configuring-whoiam-with-azure-active-directory-b2c"></a>Руководство по настройке Вхоиам с помощью Azure Active Directory B2C

В этом примере учебника мы представили рекомендации по настройке [вхоиамной](https://www.whoiam.ai/brims/) системы управления удостоверениями (бримс) в вашей среде и интеграции ее с Active Directory B2C (Azure AD B2C).

БРИМС — это набор приложений и служб, развернутых в вашей среде. Он обеспечивает проверку подлинности пользователя, SMS и электронной почты для пользовательской базы данных. БРИМС работает в сочетании с существующим решением для управления удостоверениями и доступом и не зависит от платформы.

## <a name="prerequisites"></a>Предварительные требования

Чтобы приступить к работе, вам потребуется:

- Подписка Azure AD. Если у вас нет подписки, вы можете получить [бесплатную учетную запись](https://azure.microsoft.com/free/).

- [Azure AD B2C клиент](https://docs.microsoft.com/azure/active-directory-b2c/tutorial-create-tenant) , связанный с подпиской Azure.

- [Пробная учетная запись](https://www.whoiam.ai/contact-us/)вхоиам.

## <a name="scenario-description"></a>Описание сценария

Интеграция Вхоиам включает следующие компоненты:

- Клиент Azure AD B2C. Это сервер авторизации, проверяющий учетные данные пользователя на основе пользовательских политик, определенных в ней. Он также известен как поставщик удостоверений.

- Портал администрирования для управления клиентами и их конфигурациями.

- Служба API, которая предоставляет различные возможности через конечные точки.  

- Azure Cosmos DB, которая выступает в качестве серверной части для портала администрирования БРИМС и службы API.

Реализация показана на следующей схеме архитектуры.

![Схема архитектуры интеграции Azure AD B2C с Вхоиам.](media/partner-whoiam/whoiam-architecture-diagram.png)

|Шаг | Описание |
|:-----| :-----------|
| 1. | Пользователь поступает на страницу для запуска запроса на регистрацию или вход в приложение, которое использует Azure AD B2C в качестве поставщика удостоверений.
| 2. | В рамках проверки подлинности пользователь запрашивает подтверждение принадлежности своей электронной почты или телефона или использует свой рабочий процесс в качестве биометрической проверки.  
| 3. | Azure AD B2C вызывает службу API БРИМС и передает адрес электронной почты пользователя, номер телефона и запись голоса.
| 4. | БРИМС использует предопределенные конфигурации, такие как полностью настраиваемые шаблоны электронной почты и SMS, для взаимодействия с пользователем на соответствующем языке способом, согласованным с стилем приложения.
| 5. | После завершения проверки удостоверения пользователя БРИМС возвращает маркер для Azure AD B2C, чтобы указать результат проверки. Azure AD B2C предоставит пользователю доступ к приложению или попытается выполнить проверку подлинности.  

## <a name="sign-up-with-whoiam"></a>Регистрация с помощью Вхоиам

1. Обратитесь в [вхоиам](https://www.whoiam.ai/contact-us/) и создайте учетную запись бримс.

2. Используйте рекомендации по регистрации, которые были доступны для вас, и настройте следующие службы Azure:

    - [Azure Key Vault](https://azure.microsoft.com/services/key-vault/): используется для безопасного хранения паролей, таких как пароли службы электронной почты.

    - [Служба приложений Azure](https://azure.microsoft.com/services/app-service/). используется для размещения API бримс и служб портала администрирования.

    - [Azure Active Directory](https://azure.microsoft.com/services/active-directory/): используется для проверки подлинности пользователей с правами администратора на портале администрирования.

    - [Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/): используется для хранения и извлечения параметров.

    - [Application Insights](https://docs.microsoft.com/azure/azure-monitor/app/app-insights-overview#:~:text=Application%20Insights%2C%20a%20feature%20of%20Azure%20Monitor%2C%20is,professionals.%20Use%20it%20to%20monitor%20your%20live%20applications) (необязательно): используется для входа как в API, так и на портал администрирования.

3. Развертывание API БРИМС и портала администрирования БРИМС в среде Azure.

4. Azure AD B2C примеры настраиваемой политики доступны в документации по регистрации БРИМС. Следуйте указаниям в документации, чтобы настроить приложение и использовать платформу БРИМС для проверки удостоверения пользователя.  

Дополнительные сведения о БРИМС Вхоиам см. в документации по [продукту](https://www.whoiam.ai/brims/).

## <a name="test-the-user-flow"></a>Тестирование потока пользователя

1. Откройте клиент Azure AD B2C. В разделе **Политики** выберите **Identity Experience Framework**.

2. Выберите ранее созданный **сигнупсигнин**.

3. Выберите **выполнить поток пользователя** , а затем:

   a. Для **приложения**выберите зарегистрированное приложение (пример — JWT).

   b. В поле **URL-адрес ответа**выберите **URL-адрес перенаправления**.

   c. Выберите **Выполнить поток пользователя**.

4. Пройдите процесс регистрации и создайте учетную запись.

5. Служба БРИМС будет вызываться во время последовательности после создания пользовательского атрибута. Если поток неполон, убедитесь, что пользователь не сохранен в каталоге.

## <a name="next-steps"></a>Дальнейшие шаги

Дополнительные сведения см. в следующих статьях:

- [Пользовательские политики в Azure AD B2C](https://docs.microsoft.com/azure/active-directory-b2c/custom-policy-overview)

- [Приступая к работе с пользовательскими политиками в Azure AD B2C](https://docs.microsoft.com/azure/active-directory-b2c/custom-policy-get-started?tabs=applications)
