---
title: Проверка подлинности и авторизация API — Аналитика временных рядов Azure | Документация Майкрософт
description: В этой статье описывается настройка аутентификации и авторизации для пользовательского приложения, которое вызывает API "Аналитика временных рядов Azure".
ms.service: time-series-insights
services: time-series-insights
author: deepakpalled
ms.author: shresha
manager: dpalled
ms.reviewer: v-mamcge, jasonh, kfile
ms.devlang: csharp
ms.workload: big-data
ms.topic: conceptual
ms.date: 10/02/2020
ms.custom: seodec18, has-adal-ref
ms.openlocfilehash: 7408e3fb279536f61dd2e5cf1858476da57219d4
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2020
ms.locfileid: "91665822"
---
# <a name="authentication-and-authorization-for-azure-time-series-insights-api"></a>Проверка подлинности и авторизация для API Azure Time Series Insights

В этом документе описывается порядок регистрации приложения в Azure Active Directory с помощью новой колонки Azure Active Directory. Приложения, зарегистрированные в Azure Active Directory позволяют пользователям проходить проверку подлинности и иметь право использовать API анализа временных рядов Azure, связанный с средой "аналитика временных рядов Azure".

## <a name="service-principal"></a>Субъект-служба

В следующих разделах описывается настройка приложения для доступа к API службы "аналитика временных рядов Azure" от имени приложения. Затем приложение может запрашивать или публиковать эталонные данные в среде службы "аналитика временных рядов Azure" с помощью собственных учетных данных приложения с помощью Azure Active Directory.

## <a name="summary-and-best-practices"></a>Сводка и рекомендации

Процесс регистрации приложения Azure Active Directory состоит из трех основных этапов.

1. [Регистрация приложения](#azure-active-directory-app-registration) в Azure Active Directory.
1. Авторизация приложения для [доступа к данным в среде службы "аналитика временных рядов Azure"](#granting-data-access).
1. Использование **идентификатора приложения** и **секрета клиента** для получения маркера от `https://api.timeseries.azure.com/` в [клиентском приложении](#client-app-initialization). Затем маркер можно использовать для вызова API службы "аналитика временных рядов Azure".

В **шаге 3** разделение учетных данных приложения и пользователя позволяет выполнить следующее:

* Назначить удостоверению приложения разрешения, которые отличаются от ваших разрешений. Как правило, приложение получает именно те разрешения, которые требуются для его работы. Например, можно разрешить приложению считывать данные только из определенной среды службы "аналитика временных рядов Azure".
* Изолировать безопасность приложения от создания учетных данных для проверки подлинности пользователя, используя **секрет клиента** или сертификат безопасности. В результате учетные данные приложения не зависят от учетных данных конкретного пользователя. При изменении роли пользователя приложению не обязательно потребуются новые учетные данные или дальнейшая настройка. Если пользователь изменит пароль, для доступа к приложению не требуются новые учетные данные или ключи.
* Запустить автоматический сценарий, используя **секрет клиента** или сертификат безопасности, а не учетные данные конкретного пользователя (требование их наличия).
* Используйте сертификат безопасности, а не пароль для защиты доступа к API Аналитики временных рядов Azure.

> [!IMPORTANT]
> Следуйте принципу **разделения проблем** (описанных выше в этом сценарии) при настройке политики безопасности Аналитики временных рядов Azure

> [!NOTE]

> * В статье рассматривается однотенантное приложение — решение, используемое в пределах одной организации.
> * Обычно однотенантная архитектура используется для создания бизнес-приложений в рамках организации.

## <a name="detailed-setup"></a>Подробная настройка

### <a name="azure-active-directory-app-registration"></a>Регистрация приложения в Azure Active Directory

[!INCLUDE [Azure Active Directory app registration](../../includes/time-series-insights-aad-registration.md)]

### <a name="granting-data-access"></a>Предоставление доступа к данным

1. Для среды "аналитика временных рядов Azure" выберите **политики доступа к данным** и нажмите кнопку **Добавить**.

   [![Добавление новой политики доступа к данным в среду "аналитика временных рядов Azure"](media/authentication-and-authorization/time-series-insights-data-access-policies-add.png)](media/authentication-and-authorization/time-series-insights-data-access-policies-add.png#lightbox)

1. В диалоговом окне **Выбор пользователя** вставьте **имя приложения** или **идентификатор приложения**, скопированные в разделе регистрации приложения Azure Active Directory.

   [![Поиск приложения в диалоговом окне "Выбор пользователя"](media/authentication-and-authorization/time-series-insights-data-access-policies-select-user.png)](media/authentication-and-authorization/time-series-insights-data-access-policies-select-user.png#lightbox)

1. Выберите роль. Выберите **Читатель**, чтобы предоставить разрешение на запрос данных, или **Участник**, чтобы предоставить разрешение на запрос данных и изменение эталонных данных. Щелкните **ОК**.

   [![Выбор роли "Читатель" или "Участник" в диалоговом окне "Выбор роли"](media/authentication-and-authorization/time-series-insights-data-access-policies-select-role.png)](media/authentication-and-authorization/time-series-insights-data-access-policies-select-role.png#lightbox)

1. Сохраните политику, нажав кнопку **ОК**.

   > [!TIP]
   > Сведения о дополнительных параметрах доступа к данным см. в разделе, посвященном [предоставлению доступа к данным](./time-series-insights-data-access.md).

### <a name="client-app-initialization"></a>Инициализация клиентского приложения

* Разработчики могут использовать [библиотеку проверки подлинности Майкрософт (MSAL) для проверки подлинности с помощью службы "аналитика временных рядов Azure".

* Проверка подлинности с помощью MSAL:

   1. Используйте **идентификатор приложения** и **секрет клиента** (ключ приложения) из раздела регистрации приложения Azure Active Directory, чтобы получить маркер от имени приложения.

   1. В C# маркер безопасности от имени приложения можно получить с помощью следующего кода. Полный пример запроса данных из среды Gen1 см. в статье [запрос данных с помощью C#](time-series-insights-query-data-csharp.md).

        Чтобы получить доступ к коду C#, см. репозиторий [Azure Time Series Insights](https://github.com/Azure-Samples/Azure-Time-Series-Insights/blob/master/gen1-sample/csharp-tsi-gen1-sample/Program.cs)].

   1. После этого маркер можно передать в заголовок, `Authorization` когда приложение вызовет API службы "аналитика временных рядов Azure".

> [!IMPORTANT]
> Если вы используете [библиотеку Azure Active Directory для проверки подлинности (ADAL)](https://docs.microsoft.com/azure/active-directory/azuread-dev/active-directory-authentication-libraries) , см. статью о [миграции на MSAL](https://docs.microsoft.com/azure/active-directory/develop/msal-net-migration).

## <a name="common-headers-and-parameters"></a>Общие заголовки и параметры

В этом разделе описаны распространенные заголовки HTTP-запросов и параметры, используемые для выполнения запросов к API Gen1 и Gen2 службы "аналитика временных рядов Azure". Особые требования к API подробно описаны в [справочной документации Azure Time Series insights REST API](https://docs.microsoft.com/rest/api/time-series-insights/).

> [!TIP]
> Дополнительные сведения об использовании интерфейсов REST API, а также о HTTP-запросах и обработке HTTP-ответов см. в [справочнике по Azure REST API](https://docs.microsoft.com/rest/api/azure/)

### <a name="authentication"></a>Аутентификация

Для выполнения запросов с проверкой подлинности к [API-интерфейсам службы "аналитика временных рядов Azure](https://docs.microsoft.com/rest/api/time-series-insights/)" допустимый токен носителя OAuth 2,0 должен быть передан в [заголовок авторизации](/rest/api/apimanagement/2019-12-01/authorizationserver/createorupdate) с помощью произвольного клиента (POST, JavaScript, C#).

> [!TIP]
> Ознакомьтесь с [примером визуализации клиентского пакета SDK](https://tsiclientsample.azurewebsites.net/) для службы "аналитика временных рядов Azure", чтобы узнать, как выполнять аутентификацию с помощью API-интерфейсов службы "аналитика временных рядов Azure" программным способом с помощью [клиентского пакета SDK для JavaScript](https://github.com/microsoft/tsiclient/blob/master/docs/API.md) и диаграмм и графиков.

### <a name="http-headers"></a>HTTP-заголовки

Обязательные заголовки запроса описаны ниже.

| Обязательный заголовок запроса | Описание |
| --- | --- |
| Авторизация | Для аутентификации с помощью службы "аналитика временных рядов Azure" в заголовке **авторизации** должен быть передан допустимый токен носителя OAuth 2,0. |

> [!IMPORTANT]
> Маркер должен быть выдан ресурсу `https://api.timeseries.azure.com/` (известному как "аудитория" маркера).

> * Результирующий **AuthURL** [Postman](https://www.getpostman.com/) будет следующим: `https://login.microsoftonline.com/microsoft.onmicrosoft.com/oauth2/authorize?scope=https://api.timeseries.azure.com//.default`
> * `https://api.timeseries.azure.com/` является действительным адресом, а `https://api.timeseries.azure.com` — нет.

Дополнительные заголовки запроса описаны ниже.

| Дополнительный заголовок запроса. | Описание |
| --- | --- |
| Content-type | Поддерживается только `application/json`. |
| x-ms-client-request-id | Идентификатор запроса клиента Служба записывает это значение. Позволяет службе выполнять трассировку операций между службами. |
| x-ms-client-session-id | Идентификатор сеанса клиента. Служба записывает это значение. Позволяет службе выполнять трассировку группы связанных операций между службами. |
| x-ms-client-application-name | Имя приложения, создавшего этот запрос. Служба записывает это значение. |

Необязательные, но рекомендуемые заголовки ответа описаны ниже.

| Заголовок ответа | Описание |
| --- | --- |
| Content-type | Поддерживается только `application/json`. |
| x-ms-request-id | Идентификатор запроса, сформированный сервером. Можно использовать для обращения в корпорацию Майкрософт, чтобы исследовать запрос. |
| x-ms-property-not-found-behavior | Необязательный заголовок ответа API. Возможные значения: `ThrowError` (по умолчанию) или `UseNull`. |

### <a name="http-parameters"></a>Параметры HTTP

> [!TIP]
> Дополнительные сведения об обязательных и необязательных запросах см. в [справочной документации](https://docs.microsoft.com/rest/api/time-series-insights/).

Обязательные параметры строки запроса URL-адреса зависят от версии API.

| Release | Возможные значения версии API |
| --- |  --- |
| Поколение 1 | `api-version=2016-12-12`|
| Поколение 2 | `api-version=2020-07-31` и `api-version=2018-11-01-preview`|

> [!IMPORTANT]
>
> В `api-version=2018-11-01-preview` ближайшее время версия будет устаревшей. Мы рекомендуем пользователям переключаться на более новую версию.

Необязательные параметры строки запроса URL-адреса включают установку времени ожидания для времени выполнения HTTP-запроса.

| Необязательный параметр запроса | Описание | Версия |
| --- |  --- | --- |
| `timeout=<timeout>` | Время ожидания на стороне сервера для выполнения HTTP-запроса. Применяется только к API [получения событий среды](https://docs.microsoft.com/rest/api/time-series-insights/dataaccess(preview)/query/getavailability) и [получения агрегатов среды](https://docs.microsoft.com/rest/api/time-series-insights/gen1-query-api#get-environment-aggregates-api). Значение времени ожидания должно быть в формате длительности ISO 8601 (например, `"PT20S"`) и должно находиться в диапазоне `1-30 s`. Значение по умолчанию: `30 s`. | Поколение 1 |
| `storeType=<storeType>` | Для сред Gen2 с включенным горячим хранилищем запрос можно выполнить либо в, `WarmStore` либо в `ColdStore` . Этот параметр в запросе определяет, в каком хранилище должен выполняться запрос. Если этот параметр не определен, запрос будет выполнен в холодном хранилище. Чтобы запросить теплое хранилище, параметру **storeType** следует присвоить значение `WarmStore`. Если этот параметр не определен, запрос будет выполнен для холодного хранилища. | Поколение 2 |

## <a name="next-steps"></a>Дальнейшие шаги

* Пример кода, который вызывает API Gen1 Azure Time Series Insights, считывает [данные Gen1 запросов с помощью C#](./time-series-insights-query-data-csharp.md).

* Пример кода, который вызывает примеры кода API Gen2 для службы "аналитика временных рядов Azure", — чтение [данных запроса Gen2 с помощью C#](./time-series-insights-update-query-data-csharp.md).

* Справочные сведения об API см. в [справочной документации по API запроса](https://docs.microsoft.com/rest/api/time-series-insights/gen1-query-api).

* Узнайте подробнее о [создании субъекта-службы](../active-directory/develop/howto-create-service-principal-portal.md).
