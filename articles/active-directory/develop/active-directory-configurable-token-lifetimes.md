---
title: Настраиваемые времена жизни маркеров
titleSuffix: Microsoft identity platform
description: Узнайте, как задать время существования маркеров, выдаваемых платформой Microsoft Identity.
services: active-directory
author: rwike77
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.workload: identity
ms.topic: conceptual
ms.date: 09/29/2020
ms.author: ryanwi
ms.custom: aaddev, identityplatformtop40, content-perf, FY21Q1, contperfq1
ms.reviewer: hirsin, jlu, annaba
ms.openlocfilehash: 1410af4d3c1fb9974818e5c4ebc469eee03a314c
ms.sourcegitcommit: a2d8acc1b0bf4fba90bfed9241b299dc35753ee6
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/12/2020
ms.locfileid: "91948629"
---
# <a name="configurable-token-lifetimes-in-microsoft-identity-platform-preview"></a>Настраиваемое время существования маркеров в платформе Microsoft Identity (Предварительная версия)

Можно указать время существования маркера, выданного платформой идентификации Майкрософт. Время жизни маркеров можно настроить для всех приложений в организации, для многопользовательского приложения (приложения для нескольких организаций) или для определенного субъекта-службы в организации. Однако сейчас мы не поддерживаем настройку времени существования маркеров для [субъектов-служб удостоверений](../managed-identities-azure-resources/overview.md).

> [!IMPORTANT]
> После прослуха клиентов во время предварительной версии мы реализовали [возможности управления сеансом проверки подлинности](../conditional-access/howto-conditional-access-session-lifetime.md) в условном доступе Azure AD. С помощью этой новой функции можно настроить время существования маркера обновления, установив частоту входа. После 30 мая 2020 г. новый клиент не сможет использовать настраиваемую политику времени существования маркеров для настройки маркеров сеанса и обновления. Прекращение использования будет происходить в течение нескольких месяцев после этого. Это означает, что мы не будем учитывать существующие политики сеанса и обновления маркеров. Вы по-прежнему можете настроить время существования маркера доступа после его устаревания.

В Azure AD объект политики представлен набором правил, которые применяются к отдельным приложениям или ко всем приложениям в организации. Каждый тип политики имеет уникальную структуру и набор свойств, которые применяются к объектам, для которых назначена эта политика.

Для организации можно назначить политику по умолчанию. Она будет применяться ко всем приложениям в организации, если только не будет переопределена политикой с более высоким приоритетом. Политику можно также назначить и для отдельных приложений. Приоритетность политики определяется ее типом.

В качестве примера можно ознакомиться [с примерами настройки времени существования маркеров](configure-token-lifetimes.md).

> [!NOTE]
> Настраиваемая политика времени существования маркера применяется только к мобильным и настольным клиентам, которые обращаются к SharePoint Online и OneDrive для бизнеса и не применяются к сеансам веб-браузера.
> Чтобы управлять временем существования сеансов веб-браузера для SharePoint Online и OneDrive для бизнеса, используйте функцию [время существования сеанса условного доступа](../conditional-access/howto-conditional-access-session-lifetime.md) . Ознакомьтесь с [блогом о SharePoint Online](https://techcommunity.microsoft.com/t5/SharePoint-Blog/Introducing-Idle-Session-Timeout-in-SharePoint-and-OneDrive/ba-p/119208), чтобы получить дополнительные сведения о настройке времени ожидания бездействующих сеансов.

## <a name="token-types"></a>Типы маркеров

Можно задать политики времени жизни маркеров для маркеров обновления, маркеров доступа, токенов SAML, токенов сеансов и маркеров идентификации.

### <a name="access-tokens"></a>Маркеры доступа

Чтобы получить доступ к защищенному ресурсу, клиенты используют маркеры доступа. Маркер доступа можно использовать только для конкретного сочетания пользователя, клиентского приложения и ресурса. Маркеры доступа не могут быть отозваны. Они действуют до истечения срока своего действия. Вредоносный субъект, получивший маркер доступа, сможет использовать его на протяжении всего времени жизни. Настройка времени жизни маркера доступа — это компромисс между производительностью системы и временем, в течение которого клиентское приложение сохраняет доступ после отключения учетной записи пользователя. Чтобы повысить производительность системы, нужно минимизировать частоту обращений клиентского приложения за обновленным маркером доступа.  Значение по умолчанию — 1 час. По прошествии этого времени клиент должен использовать маркер обновления, чтобы (обычно это происходит автоматически) получить новый маркер обновления и маркер доступа. 

### <a name="saml-tokens"></a>Токены SAML

Токены SAML используются многими веб-приложениями SAAS и получаются с помощью конечной точки протокола SAML2 Azure Active Directory. Они также используются приложениями, использующими WS-Federation. Время существования маркера по умолчанию составляет 1 час. С точки зрения приложения срок действия маркера определяется значением Нотонорафтер `<conditions …>` элемента в токене. По завершении срока действия маркера клиент должен инициировать новый запрос проверки подлинности, который, как правило, будет удовлетворен без интерактивного входа в результате маркера сеанса единого входа (SSO).

Значение Нотонорафтер можно изменить с помощью `AccessTokenLifetime` параметра в `TokenLifetimePolicy` . Для него будет задано время существования, настроенное в политике, если таковое имеется, а также коэффициент наклона часов, равный пяти минутам.

`<SubjectConfirmationData>`Конфигурация времени существования маркера не зависит от нотонорафтер подтверждения субъекта, указанного в элементе. 

### <a name="refresh-tokens"></a>Маркеры обновления

Когда клиентское приложение обращается за маркером доступа для доступа к защищенному ресурсу, клиент также получает маркер обновления. Маркер обновления используется для получения новой пары маркеров доступа и обновления, когда истечет срок действия маркера доступа. Маркер обновления привязан одновременно и к пользователю, и клиентскому приложению. Маркер обновления можно [отменить в любой момент](access-tokens.md#token-revocation), и при каждом использовании проверяется его действительность.  Маркеры обновления не отзывают, если они используются, чтобы получить новые маркеры доступа. Тем не менее мы советуем удалить старый маркер при получении нового. 

Важно различать конфиденциальные и общедоступные клиентские приложения. Это влияет на продолжительность использования маркеров обновления. Дополнительные сведения о различных типах клиентов см. в разделе [RFC 6749](https://tools.ietf.org/html/rfc6749#section-2.1).

#### <a name="token-lifetimes-with-confidential-client-refresh-tokens"></a>Время жизни маркера для маркеров обновления конфиденциального клиента
Конфиденциальные клиенты — это такие клиентские приложения, которые могут безопасно хранить клиентский пароль (секрет), с помощью которого можно подтвердить, что запросы поступают от защищенного клиентского приложения, а не от вредоносного субъекта. Например, веб-приложение является конфиденциальным клиентом, так как оно может хранить секрет клиента на веб-сервере, и потому он остается невидимым. Так как такие потоки достаточно безопасные, для них по умолчанию устанавливается время жизни выпускаемых маркеров обновления `until-revoked`, и этот срок нельзя изменить с помощью политики или отменить при необязательном сбросе пароля.

#### <a name="token-lifetimes-with-public-client-refresh-tokens"></a>Время жизни маркеров для маркеров обновления общедоступного клиента

Общедоступные клиенты не могут обеспечить безопасное хранение клиентского пароля (секрета). Например, приложение iOS или Android не может замаскировать секрет от владельца ресурса, и поэтому считается общедоступным клиентом. Для ресурсов можно определить политики, которые запрещают маркерам обновления из общедоступных клиентских приложений, которые старше указанного периода, получать новую пару маркеров доступа и обновления. Для этого используйте [свойство "максимальное значение неактивного времени" для маркера обновления](#refresh-token-max-inactive-time) ( `MaxInactiveTime` ). Кроме того, с помощью политик можно задать период времени, по истечении которого маркеры обновления больше не принимаются. Для этого используйте свойство " [максимальный возраст маркера обновления](#single-factor-session-token-max-age) " или " [максимальный возраст токена многофакторного сеанса](#multi-factor-refresh-token-max-age) ". Настроив время жизни маркера обновления, вы можете управлять частотой, с которой пользователь должен повторно вводить учетные данные при использовании общедоступного клиентского приложения.

> [!NOTE]
> Свойство максимального возраста — это период времени, в течение которого можно использовать один маркер. 

### <a name="id-tokens"></a>Маркеры идентификации
Маркеры идентификации передаются веб-сайтам и собственным клиентам. Они содержат сведения о профиле пользователя. Маркер идентификации привязан одновременно и к пользователю, и клиентскому приложению. Маркеры идентификации считаются действительными до истечения срока действия. Как правило, в веб-приложениях длительность пользовательского сеанса согласовывается со временем жизни маркера идентификации, выданного этому пользователю. Вы можете настроить время существования маркера идентификации, чтобы управлять тем, как часто веб-приложение истекает сеанс приложения, и как часто требуется повторная проверка подлинности пользователя с платформой Microsoft Identity (автоматическая или интерактивная).

### <a name="single-sign-on-session-tokens"></a>Маркеры сеанса единого входа
Когда пользователь проходит проверку подлинности с помощью платформы идентификации Майкрософт, сеанс единого входа (SSO) устанавливается с браузером пользователя и платформой Microsoft Identity. Этот сеанс представлен маркером сеанса единого входа — в виде файла cookie. Маркер сеанса единого входа не привязан к конкретному ресурсу или клиентскому приложению. Маркеры сеанса единого входа могут быть отозваны, а при каждом использовании проверяется их действительность.

Платформа Microsoft Identity использует два вида маркеров сеанса единого входа: постоянные и непостоянные. Постоянные маркеры сеанса хранятся в постоянных файлах cookie, а временные — хранятся в виде файлов cookie сеанса. (Файлы cookie сеанса уничтожаются при закрытии браузера.) Обычно сохраняется непостоянный токен сеанса. Но когда пользователь при аутентификации устанавливает флажок **Оставаться в системе**, сохраняется постоянный маркер сеанса.

Срок действия временных маркеров сеанса составляет 24 часа, Срок действия постоянных маркеров составляет 90 дней. Когда в течение срока действия используется маркер сеанса единого входа, срок действия увеличивается еще на 24 часа или 90 дней в зависимости от типа токена. Если маркер сеанса единого входа не используется в течение срока его действия, он считается истекшим и не будет приниматься.

С помощью политик можно задать период времени после выдачи первого маркера сеанса, по прошествии которого такой маркер не будет приниматься. (Для этого используйте свойство максимальный возраст токена сеанса.) Вы можете настроить время существования маркера сеанса, чтобы управлять временем и частотой, с которой пользователь должен повторно вводить учетные данные, а не выполнять автоматическую проверку подлинности при использовании веб-приложения.

### <a name="token-lifetime-policy-properties"></a>Свойства политики времени жизни маркера
Политика времени жизни маркера — это объект политики, который содержит правила времени жизни маркера. Свойства этой политики и определяют срок действия соответствующих маркеров. Если политика не задана, система использует стандартное значение для времени жизни.

### <a name="configurable-token-lifetime-properties"></a>Свойства для настройки времени жизни маркера
| Свойство | Строка свойства политики | Область применения | По умолчанию | Минимальные | Максимум |
| --- | --- | --- | --- | --- | --- |
| Время жизни маркера доступа |Акцесстокенлифетиме<sup>2</sup> |Маркеры доступа, маркеры безопасности, маркеры SAML2 |1 час |10 минут. |1 день |
| Максимальное время неактивности для маркеров обновления |MaxInactiveTime |Маркеры обновления |90 дней |10 минут. |90 дней |
| Максимальный возраст однофакторного маркера обновления |MaxAgeSingleFactor |Маркеры обновления (для всех пользователей) |Пока не будет отозван |10 минут. |Пока не будет отозван<sup>1</sup> |
| Максимальный возраст многофакторного маркера обновления |MaxAgeMultiFactor |Маркеры обновления (для всех пользователей) | 180 дней |10 минут. |180 дней <sup>1</sup> |
| Максимальный возраст однофакторного маркера сеанса |MaxAgeSessionSingleFactor |Маркеры сеанса (постоянные и временные) |Пока не будет отозван |10 минут. |Пока не будет отозван<sup>1</sup> |
| Максимальный возраст многофакторного маркера сеанса |MaxAgeSessionMultiFactor |Маркеры сеанса (постоянные и временные) | 180 дней |10 минут. | 180 дней <sup>1</sup> |

* <sup>1</sup>Максимальная длительность, которую можно явно задать для этих атрибутов, — 365 дней.
* <sup>2</sup> Чтобы обеспечить работу веб-клиента Microsoft Teams, рекомендуется Акцесстокенлифетиме более 15 минут для Microsoft Teams.

### <a name="exceptions"></a>Исключения
| Свойство | Область применения | По умолчанию |
| --- | --- | --- |
| Обновление максимального возраста маркеров (выданные для федеративных пользователей с недостаточной информацией об отзыве <sup>1</sup>) |Маркеры обновления (выданные для федеративных пользователей с недостаточной информацией об отзыве <sup>1</sup>) |12 часов |
| Максимальное время неактивности для маркера обновления (выданного для конфиденциальных клиентов) |Маркеры обновления (выданные для конфиденциальных клиентов) |90 дней |
| Максимальный возраст маркеров обновления (выданных для конфиденциальных клиентов) |Маркеры обновления (выданные для конфиденциальных клиентов) |Пока не будет отозван |

* <sup>1</sup> Федеративные пользователи, имеющие недостаточную информацию об отзыве, включают всех пользователей, не имеющих синхронизированного атрибута "LastPasswordChangeTimestamp". Этим пользователям предоставляется этот короткий максимальный возраст, так как Azure Active Directory не удается проверить, когда следует отозвать маркеры, привязанные к старым учетным данным (например, измененный пароль), и необходимо чаще выполнять возврат, чтобы убедиться, что пользователь и связанные с ним маркеры все еще находятся в хорошем расположении. Чтобы улучшить этот процесс, администраторы клиента должны убедиться, что они синхронизируют атрибут "LastPasswordChangeTimestamp" (этот параметр можно задать для объекта пользователя с помощью PowerShell или AADSync).

### <a name="policy-evaluation-and-prioritization"></a>Оценка политики и ее приоритеты
Политики времени жизни маркера можно создать и назначить для конкретного приложения, организации и субъектов-служб. К одному приложению могут применяться сразу несколько политик. Политики определяют время жизни маркеров в соответствии со следующими правилами:

* если политика явно назначена субъекту-службе, применяется именно она;
* если нет политики, явно назначенной субъекту-службе, применяется политика, явно назначенная родительской организации субъекта-службы;
* если нет политики, явно назначенной субъекту-службе или организации, применяется политика, назначенная приложению;
* Если ни одна политика не назначена субъекту-службе, организации или объекту приложения, применяются значения по умолчанию. (См. таблицу в разделе [Свойства для настройки времени жизни маркера](#configurable-token-lifetime-properties).)

Дополнительные сведения о связях между объектами приложения и объектами субъекта-службы см. в статье [Объекты приложения и субъекта-службы в Azure Active Directory](app-objects-and-service-principals.md).

Срок действия маркера оценивается при его использовании. Используется политика с самым высоким приоритетом, установленная для оцениваемого приложения.

Все интервалы времени указаны в формате объекта C# [TimeSpan](/dotnet/api/system.timespan): Д.ЧЧ:ММ:СС.  Поэтому 80 дней и 30 минут указываются как `80.00:30:00`.  Начальное значение Д можно опустить, если оно равно нулю, поэтому 90 минут будет указываться как `00:90:00`.  

> [!NOTE]
> Ниже приведен пример сценария.
>
> Пользователь хочет получить доступ к двум веб-приложениям, условно именуемым веб-приложение A и веб-приложение Б.
> 
> Факторы:
> * Оба веб-приложения находятся в одной родительской организации.
> * Для родительской организации в качестве политики по умолчанию установлена политика времени жизни маркеров 1, в соответствии с которой максимальный возраст маркера сеанса составляет 8 часов.
> * Веб-приложение A — это обычное веб-приложение без настроенных политик.
> * Веб-приложение Б используется для процессов с высоким уровнем конфиденциальности, и его субъект-служба связана с политикой времени жизни маркеров 2, в которой максимальный возраст маркера сеанса составляет 30 минут.
>
> В 12:00 PM пользователь запускает новый сеанс браузера и пытается получить доступ к веб-приложению а. Пользователь перенаправляется на платформу удостоверений Майкрософт и ему предлагается выполнить вход. При этом в браузере создается файл cookie с маркером сеанса. После этого пользователь перенаправляется в веб-приложение A с маркером идентификатора, который предоставляет права доступа к приложению.
>
> В 12:15 PM пользователь пытается получить доступ к веб-приложению б. Браузер перенаправляется на платформу Microsoft Identity Platform, которая обнаруживает файл cookie сеанса. Субъект-служба веб-приложения Б связан с политикой времени жизни маркеров 2, но также входит в родительскую организацию, для которой по умолчанию используется политика 1. Здесь применяется политика времени жизни маркеров 2, так как политики для субъекта-службы имеют более высокий приоритет, чем политики по умолчанию для организации. Исходный маркер сеанса был выпущен в течение последних 30 минут, поэтому считается допустимым. Пользователь перенаправляется в веб-приложение Б с маркером идентификатора, предоставляющим права доступа.
>
> В 1:00 PM пользователь пытается получить доступ к веб-приложению а. Пользователь перенаправляется на платформу Microsoft Identity. Веб-приложение A не имеет связанных политик, но входит в организацию, для которой в качестве политики по умолчанию установлена политика времени жизни маркеров 1, поэтому применяется именно эта политика. Система обнаруживает файл cookie сеанса, выпущенный в пределах последних 8 часов. Пользователь автоматически перенаправляется обратно в веб-приложение A с новым маркером идентификатора. Проверка подлинности в таком случае не требуется.
>
> Сразу же после этого пользователь пытается получить доступ к веб-приложению б. Пользователь перенаправляется на платформу Microsoft Identity. Как и прошлый раз, применяется политика времени жизни маркеров 2. Так как маркер был выдан более 30 минут назад, пользователю предлагается повторно ввести учетные данные для входа, чтобы получить новые маркер сеанса и маркер идентификатора. Пользователь получает доступ к веб-приложению Б.
>
>

## <a name="configurable-policy-property-details"></a>Подробные сведения о настройке свойств политики
### <a name="access-token-lifetime"></a>Время жизни маркера доступа
**Строка:** AccessTokenLifetime

**Влияет на:** Маркеры доступа, маркеры идентификации, токены SAML

**Описание:** эта политика определяет, как долго продолжают действовать маркеры доступа и маркеры идентификации для определенного ресурса. Чем меньше значение свойства времени жизни маркера доступа, тем ниже риск использования маркера доступа или маркера идентификатора вредоносным субъектом в течение длительного времени. (Эти токены не могут быть отозваны.) Компромисс заключается в том, что производительность отрицательно сказывается на производительности, поскольку маркеры должны быть заменены чаще.

Пример см. в разделе [Создание политики для входа в веб-](configure-token-lifetimes.md#create-a-policy-for-web-sign-in)приложение.

### <a name="refresh-token-max-inactive-time"></a>Максимальное время неактивности для маркеров обновления
**Строка:** MaxInactiveTime

**Область применения:** маркеры обновления

**Описание:** эта политика определяет, насколько старым должен быть маркер обновления, чтобы клиент больше не мог использовать его для получения новой пары маркеров доступа и обновления при попытке доступа к определенному ресурсу. При использовании маркера обновления обычно возвращается новый маркер обновления. Следовательно, политика запретит доступ после того, как клиент в течение указанного времени будет пытаться получить доступ к каким-либо ресурсам, используя текущий маркер обновления.

Эта политика вынуждает пользователей, не выполнявших активных действий в клиентских приложениях, повторно проходить проверку подлинности для получения нового маркера обновления.

Свойство максимального времени неактивности маркера обновления должно быть меньше, чем свойства максимального возраста однофакторного маркера или многофакторного маркера обновления.

Пример см. в разделе [Создание политики для собственного приложения, вызывающего веб-API](configure-token-lifetimes.md#create-a-policy-for-a-native-app-that-calls-a-web-api).

### <a name="single-factor-refresh-token-max-age"></a>Максимальный возраст однофакторного маркера обновления
**Строка:** MaxAgeSingleFactor

**Область применения:** маркеры обновления

**Описание:** эта политика определяет, как долго пользователь может применять маркер обновления для получения новых пар маркеров доступа и обновления после последней успешной проверки подлинности с использованием однофакторных методов. Когда пользователь выполнит проверку подлинности и получит новый маркер обновления, он сможет и дальше использовать маркер обновления в течение указанного периода времени. (Это справедливо, если текущий маркер обновления не отозван и не используется дольше, чем время неактивности.) На этом этапе пользователь вынужден выполнить повторную проверку подлинности, чтобы получить новый маркер обновления.

Сокращение максимального возраста вынуждает пользователей чаще выполнять проверку подлинности. Так как однофакторная идентификация подлинности считается менее безопасной, чем многофакторная, мы советуем указать значение этого свойства, не превышающее максимальный возраст многофакторного маркера обновления.

Пример см. в разделе [Создание политики для собственного приложения, вызывающего веб-API](configure-token-lifetimes.md#create-a-policy-for-a-native-app-that-calls-a-web-api).

### <a name="multi-factor-refresh-token-max-age"></a>Максимальный возраст многофакторного маркера обновления
**Строка:** MaxAgeMultiFactor

**Область применения:** маркеры обновления

**Описание:** эта политика определяет, как долго пользователь может применять маркер обновления для получения новых пар маркеров доступа и обновления после последней успешной проверки подлинности с использованием многофакторных методов. Когда пользователь выполнит проверку подлинности и получит новый маркер обновления, он сможет и дальше использовать маркер обновления в течение указанного периода времени. (Это справедливо, если текущий маркер обновления не отозван и не используется дольше времени неактивности.) На этом этапе пользователи вынуждены повторно пройти проверку подлинности для получения нового маркера обновления.

Сокращение максимального возраста вынуждает пользователей чаще выполнять проверку подлинности. Так как однофакторная идентификация подлинности считается менее безопасной, чем многофакторная, мы советуем указать значение этого свойства, не превышающее максимальный возраст однофакторного маркера обновления.

Пример см. в разделе [Создание политики для собственного приложения, вызывающего веб-API](configure-token-lifetimes.md#create-a-policy-for-a-native-app-that-calls-a-web-api).

### <a name="single-factor-session-token-max-age"></a>Максимальный возраст однофакторного маркера сеанса
**Строка:** MaxAgeSessionSingleFactor

**Область применения:** маркеры сеанса (постоянные и временные)

**Описание:** эта политика определяет, как долго пользователь может применять маркер сеанса для получения новых маркеров сеанса и идентификатора после последней успешной проверки подлинности с использованием однофакторных методов. Когда пользователь выполнит проверку подлинности и получит новый маркер сеанса, он сможет и дальше использовать поток маркеров сеанса в течение указанного периода времени. (Это справедливо, если текущий маркер сеанса не отозван и срок его действия не истек.) По истечении указанного периода времени пользователю принудительно выполняется повторная проверка подлинности для получения нового маркера сеанса.

Сокращение максимального возраста вынуждает пользователей чаще выполнять проверку подлинности. Так как однофакторная идентификация подлинности считается менее безопасной, чем многофакторная, мы советуем указать значение этого свойства, не превышающее максимальный возраст многофакторного маркера сеанса.

Пример см. в разделе [Создание политики для входа в веб-](configure-token-lifetimes.md#create-a-policy-for-web-sign-in)приложение.

### <a name="multi-factor-session-token-max-age"></a>Максимальный возраст многофакторного маркера сеанса
**Строка:** MaxAgeSessionMultiFactor

**Область применения:** маркеры сеанса (постоянные и временные)

**Описание:** эта политика определяет, как долго пользователь может применять маркер сеанса для получения новых маркеров сеанса и идентификатора после последней успешной проверки подлинности с использованием многофакторных методов. Когда пользователь выполнит проверку подлинности и получит новый маркер сеанса, он сможет и дальше использовать поток маркеров сеанса в течение указанного периода времени. (Это справедливо, если текущий маркер сеанса не отозван и срок его действия не истек.) По истечении указанного периода времени пользователю принудительно выполняется повторная проверка подлинности для получения нового маркера сеанса.

Сокращение максимального возраста вынуждает пользователей чаще выполнять проверку подлинности. Так как однофакторная идентификация подлинности считается менее безопасной, чем многофакторная, мы рекомендуем указать значение этого свойства, не превышающее максимальный возраст однофакторного маркера сеанса.

## <a name="cmdlet-reference"></a>Справка по командлетам

Это командлеты в [модуле предварительной версии Azure Active Directory PowerShell для Graph](/powershell/module/azuread/?view=azureadps-2.0-preview#service-principals&preserve-view=true&preserve-view=true).

### <a name="manage-policies"></a>Управление политиками

Управлять политиками можно с помощью следующих командлетов.

| Командлет | Описание | 
| --- | --- |
| [New-AzureADPolicy](/powershell/module/azuread/new-azureadpolicy?view=azureadps-2.0-preview&preserve-view=true) | Создает новую политику. |
| [Get-AzureADPolicy](/powershell/module/azuread/get-azureadpolicy?view=azureadps-2.0-preview&preserve-view=true) | Возвращает полный список политик Azure AD или одну указанную политику. |
| [Get-AzureADPolicyAppliedObject](/powershell/module/azuread/get-azureadpolicyappliedobject?view=azureadps-2.0-preview&preserve-view=true) | Возвращает список приложений и субъектов-служб, связанных с политикой. |
| [Set-AzureADPolicy](/powershell/module/azuread/set-azureadpolicy?view=azureadps-2.0-preview&preserve-view=true) | Обновляет существующую политику. |
| [Remove-AzureADPolicy](/powershell/module/azuread/remove-azureadpolicy?view=azureadps-2.0-preview&preserve-view=true) | Удаляет указанную политику. |

### <a name="application-policies"></a>Политики приложений
Управлять политиками приложений можно с помощью следующих командлетов.</br></br>

| Командлет | Описание | 
| --- | --- |
| [Add-AzureADApplicationPolicy](/powershell/module/azuread/add-azureadapplicationpolicy?view=azureadps-2.0-preview&preserve-view=true) | Связывает указанную политику с приложением. |
| [Get-AzureADApplicationPolicy](/powershell/module/azuread/get-azureadapplicationpolicy?view=azureadps-2.0-preview&preserve-view=true) | Возвращает политику, назначенную для приложения. |
| [Remove-AzureADApplicationPolicy](/powershell/module/azuread/remove-azureadapplicationpolicy?view=azureadps-2.0-preview&preserve-view=true) | Удаляет политику из приложения. |

### <a name="service-principal-policies"></a>Политики субъекта-службы
Управлять политиками субъектов-служб можно с помощью следующих командлетов.

| Командлет | Описание | 
| --- | --- |
| [Add-AzureADServicePrincipalPolicy](/powershell/module/azuread/add-azureadserviceprincipalpolicy?view=azureadps-2.0-preview&preserve-view=true) | Связывает указанную политику с субъектом-службой. |
| [Get-AzureADServicePrincipalPolicy](/powershell/module/azuread/get-azureadserviceprincipalpolicy?view=azureadps-2.0-preview&preserve-view=true) | Возвращает все политики, связанные с указанным субъектом-службой.|
| [Remove-AzureADServicePrincipalPolicy](/powershell/module/azuread/remove-azureadserviceprincipalpolicy?view=azureadps-2.0-preview&preserve-view=true) | Удаляет политику из указанного субъекта-службы.|

## <a name="license-requirements"></a>Требования лицензий

Для использования этой функции требуется лицензия Azure AD Premium P1. Чтобы найти подходящую лицензию для ваших требований, см. статью [Сравнение общедоступных функций выпусков Free и Premium](https://azure.microsoft.com/pricing/details/active-directory/).

Клиенты с [лицензиями Microsoft 365 бизнес](/office365/servicedescriptions/microsoft-365-service-descriptions/microsoft-365-business-service-description) также имеют доступ к функциям условного доступа.

## <a name="next-steps"></a>Дальнейшие действия

Дополнительные сведения см. в статье [примеры настройки времени существования маркеров](configure-token-lifetimes.md).