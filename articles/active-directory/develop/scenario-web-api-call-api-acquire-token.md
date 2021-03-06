---
title: Получение маркера для веб-API, который вызывает веб-API | Службы
titleSuffix: Microsoft identity platform
description: Узнайте, как создать веб-API, который вызывает веб-API, требующие получения маркера для приложения.
services: active-directory
author: jmprieur
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 07/15/2020
ms.author: jmprieur
ms.custom: aaddev
ms.openlocfilehash: ab0b74ffbcd8167613c6a8470e2f9102566edc60
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2020
ms.locfileid: "91257237"
---
# <a name="a-web-api-that-calls-web-apis-acquire-a-token-for-the-app"></a>Веб-API, вызывающий веб-API: получение маркера для приложения

После построения объекта клиентского приложения используйте его для получения маркера, который можно использовать для вызова веб-API.

## <a name="code-in-the-controller"></a>Код в контроллере

# <a name="aspnet-core"></a>[ASP.NET Core](#tab/aspnetcore)

*Microsoft. Identity. Web* добавляет методы расширения, которые предоставляют удобные службы для вызова Microsoft Graph или нисходящего веб-API. Эти методы подробно описаны в [веб-API, который вызывает веб-API: вызов API](scenario-web-api-call-api-call-api.md). При использовании этих вспомогательных методов вам не нужно вручную получать маркер.

Однако если вы хотите вручную получить маркер, в следующем коде показан пример использования *Microsoft. Identity. Web* для этого в контроллере API. Он вызывает нисходящий API с именем *ToDoList*.
Чтобы получить маркер для вызова подчиненного API, необходимо внедрить `ITokenAcquisition` службу путем внедрения зависимостей в конструктор контроллера (или конструктор страницы при использовании блазор) и использовать его в действиях контроллера, получая маркер для пользователя ( `GetAccessTokenForUserAsync` ) или самого приложения ( `GetAccessTokenForAppAsync` ) в случае сценария управляющей программы.

```csharp
[Authorize]
public class MyApiController : Controller
{
    /// <summary>
    /// The web API will accept only tokens 1) for users, 2) that have the `access_as_user` scope for
    /// this API.
    /// </summary>
    static readonly string[] scopeRequiredByApi = new string[] { "access_as_user" };

     static readonly string[] scopesToAccessDownstreamApi = new string[] { "api://MyTodolistService/access_as_user" };

    private readonly ITokenAcquisition _tokenAcquisition;

    public MyApiController(ITokenAcquisition tokenAcquisition)
    {
        _tokenAcquisition = tokenAcquisition;
    }

    public IActionResult Index()
    {
        HttpContext.VerifyUserHasAnyAcceptedScope(scopeRequiredByApi);

        string accessToken = _tokenAcquisition.GetAccessTokenForUserAsync(scopesToAccessDownstreamApi);
        return await callTodoListService(accessToken);
    }
}
```

Дополнительные сведения о `callTodoListService` методе см.  [в веб-API, который вызывает веб-API: вызов API](scenario-web-api-call-api-call-api.md).

# <a name="java"></a>[Java](#tab/java)
Ниже приведен пример кода, который вызывается в действиях контроллеров API. Он вызывает нисходящий Microsoft Graph API.

```java
@RestController
public class ApiController {

    @Autowired
    MsalAuthHelper msalAuthHelper;

    @RequestMapping("/graphMeApi")
    public String graphMeApi() throws MalformedURLException {

        String oboAccessToken = msalAuthHelper.getOboToken("https://graph.microsoft.com/.default");

        return callMicrosoftGraphMeEndpoint(oboAccessToken);
    }

}
```

# <a name="python"></a>[Python](#tab/python)

Веб-API Python требует использования по промежуточного слоя для проверки токена носителя, полученного от клиента. Затем веб-API может получить маркер доступа для нисходящих API с помощью библиотеки MSAL Python, вызвав [`acquire_token_on_behalf_of`](https://msal-python.readthedocs.io/en/latest/?badge=latest#msal.ConfidentialClientApplication.acquire_token_on_behalf_of) метод. Пример, демонстрирующий этот поток с помощью MSAL Python, пока недоступен.

---

## <a name="next-steps"></a>Дальнейшие шаги

> [!div class="nextstepaction"]
> [Веб-API, вызывающий веб-API: вызов API](scenario-web-api-call-api-call-api.md)
