---
title: Настройка регистрации и входа с помощью учетной записи GitHub
titleSuffix: Azure AD B2C
description: Вы можете организовать в приложениях регистрацию и вход для клиентов с учетными записями GitHub, используя Azure Active Directory B2C.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: how-to
ms.date: 08/08/2019
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: ba2441ae48c99d63ae637d2b80069058a04c5ef9
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2020
ms.locfileid: "85388193"
---
# <a name="set-up-sign-up-and-sign-in-with-a-github-account-using-azure-active-directory-b2c"></a>Настройка регистрации и входа с учетной записью GitHub через Azure Active Directory B2C

[!INCLUDE [active-directory-b2c-public-preview](../../includes/active-directory-b2c-public-preview.md)]

## <a name="create-a-github-oauth-application"></a>Создание приложения OAuth GitHub

Чтобы использовать учетную запись GitHub в качестве [поставщика удостоверений](authorization-code-flow.md) в Azure Active Directory B2C (Azure AD B2C), необходимо создать в своем клиенте приложение, которое его представляет. Если у вас еще нет учетной записи GitHub, вы можете зарегистрироваться по адресу [https://www.github.com/](https://www.github.com/) .

1. Войдите на сайт [разработчика GitHub](https://github.com/settings/developers), используя учетные данные GitHub.
1. Выберите **Приложения OAuth**, а затем — **New OAuth App** (Создать приложение OAuth).
1. Заполните поля **Application name** (Имя приложения) и **Homepage URL** (URL-адрес домашней страницы).
1. Введите значение `https://your-tenant-name.b2clogin.com/your-tenant-name.onmicrosoft.com/oauth2/authresp` в поле **Authorization callback URL** (URL-адрес обратного вызова авторизации). Замените `your-tenant-name` именем вашего клиента Azure AD B2C. При вводе имени вашего клиента используйте только строчные буквы, даже если в Azure AD B2C имя клиента определено с помощью прописных букв.
1. Нажмите кнопку **Зарегистрировать приложение**.
1. Скопируйте значения **идентификатора клиента** и **секрета клиента**. Оба этих значения потребуются при добавлении поставщика удостоверений для вашего клиента.

## <a name="configure-a-github-account-as-an-identity-provider"></a>Настройка учетной записи GitHub в качестве поставщика удостоверений

1. Войдите на [портал Azure](https://portal.azure.com/) с правами глобального администратора клиента Azure AD B2C.
1. Убедитесь, что используете каталог с клиентом Azure AD B2C, выбрав фильтр **Каталог и подписка** в меню вверху и каталог с вашим клиентом.
1. Выберите **Все службы** в левом верхнем углу окна портала Azure, найдите службу **Azure AD B2C** и выберите ее.
1. Выберите **поставщики удостоверений**, а затем — **GitHub (Предварительная версия)**.
1. Введите **Имя**. Например, *GitHub*.
1. В поле **идентификатор клиента**введите идентификатор клиента приложения GitHub, созданного ранее.
1. В качестве **секрета клиента**введите записанный секрет клиента.
1. Щелкните **Сохранить**.
