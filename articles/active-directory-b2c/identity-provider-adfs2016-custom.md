---
title: Добавление ADFS в качестве поставщика удостоверений SAML с помощью настраиваемых политик
titleSuffix: Azure AD B2C
description: Настройка ADFS 2016 с помощью протокола SAML и пользовательских политик в Azure Active Directory B2C
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: how-to
ms.date: 02/27/2020
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: 8f6131232d9f6f98095d073672e201cfb591b540
ms.sourcegitcommit: 1b47921ae4298e7992c856b82cb8263470e9e6f9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/14/2020
ms.locfileid: "92054786"
---
# <a name="add-adfs-as-a-saml-identity-provider-using-custom-policies-in-azure-active-directory-b2c"></a>Добавление ADFS в качестве поставщика удостоверений SAML с помощью пользовательских политик в Azure Active Directory B2C

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

В этой статье показано, как включить вход для учетной записи пользователя ADFS с помощью [пользовательских политик](custom-policy-overview.md) в Azure Active Directory B2C (Azure AD B2C). Вход в систему включается путем добавления [технического профиля поставщика удостоверений SAML](saml-identity-provider-technical-profile.md) в пользовательскую политику.

## <a name="prerequisites"></a>Предварительные требования

- Выполните шаги, описанные в статье [Начало работы с настраиваемыми политиками в Azure Active Directory B2C](custom-policy-get-started.md).
- Убедитесь, что у вас есть доступ к PFX-файлу сертификата с закрытым ключом. Можно создать собственный подписанный сертификат и передать его в Azure AD B2C. Azure AD B2C использует этот сертификат для подписи запроса SAML, отправляемого поставщику удостоверений SAML.
- Чтобы служба Azure принимала пароль PFX-файла, пароль должен быть зашифрован с помощью параметра TripleDES – SHA1 в служебной программе экспорта хранилища сертификатов Windows, а не AES256-SHA256.

## <a name="create-a-policy-key"></a>Создание ключа политики

Вам необходимо сохранить сертификат в клиенте Azure AD B2C.

1. Войдите на [портал Azure](https://portal.azure.com/).
2. Убедитесь, что вы используете каталог, содержащий клиент Azure AD B2C. Выберите фильтр **Каталог и подписка** в верхнем меню и выберите каталог, который содержит ваш клиент.
3. Выберите **Все службы** в левом верхнем углу окна портала Azure, а затем найдите и выберите **Azure AD B2C**.
4. На странице "Обзор" выберите **Identity Experience Framework**.
5. Выберите **Ключи политики**, а затем щелкните **Добавить**.
6. Для пункта **Параметры** выберите `Upload`.
7. Введите **имя** ключа политики. Например, `SamlCert`. Префикс `B2C_1A_` будет автоматически добавлен к имени ключа.
8. Найдите и выберите PFX-файл сертификата с закрытым ключом.
9. Нажмите кнопку **Создать**.

## <a name="add-a-claims-provider"></a>Добавление поставщика утверждений

Если необходимо разрешить пользователям входить в систему с помощью учетной записи ADFS, нужно определить учетную запись в качестве поставщика утверждений, с которым Azure AD B2C может взаимодействовать через конечную точку. Конечная точка предоставляет набор утверждений, используемых Azure AD B2C, чтобы проверить, была ли выполнена проверка подлинности определенного пользователя.

Чтобы определить учетную запись ADFS в качестве поставщика утверждений, добавьте ее в элемент **ClaimsProviders** в файле расширения политики. Дополнительные сведения см. в разделе об [определении технического профиля поставщика удостоверений SAML](saml-identity-provider-technical-profile.md).

1. Откройте файл *TrustFrameworkExtensions.xml*.
1. Найдите элемент **ClaimsProviders**. Если он не существует, добавьте его в корневой элемент.
1. Добавьте новый элемент **ClaimsProvider** следующим образом.

    ```xml
    <ClaimsProvider>
      <Domain>contoso.com</Domain>
      <DisplayName>Contoso ADFS</DisplayName>
      <TechnicalProfiles>
        <TechnicalProfile Id="Contoso-SAML2">
          <DisplayName>Contoso ADFS</DisplayName>
          <Description>Login with your ADFS account</Description>
          <Protocol Name="SAML2"/>
          <Metadata>
            <Item Key="WantsEncryptedAssertions">false</Item>
            <Item Key="PartnerEntity">https://your-ADFS-domain/federationmetadata/2007-06/federationmetadata.xml</Item>
            <Item Key="XmlSignatureAlgorithm">Sha256</Item>
          </Metadata>
          <CryptographicKeys>
            <Key Id="SamlMessageSigning" StorageReferenceId="B2C_1A_ADFSSamlCert"/>
          </CryptographicKeys>
          <OutputClaims>
            <OutputClaim ClaimTypeReferenceId="issuerUserId" PartnerClaimType="userPrincipalName" />
            <OutputClaim ClaimTypeReferenceId="givenName" PartnerClaimType="given_name"/>
            <OutputClaim ClaimTypeReferenceId="surname" PartnerClaimType="family_name"/>
            <OutputClaim ClaimTypeReferenceId="email" PartnerClaimType="email"/>
            <OutputClaim ClaimTypeReferenceId="displayName" PartnerClaimType="name"/>
            <OutputClaim ClaimTypeReferenceId="identityProvider" DefaultValue="contoso.com" />
            <OutputClaim ClaimTypeReferenceId="authenticationSource" DefaultValue="socialIdpAuthentication"/>
          </OutputClaims>
          <OutputClaimsTransformations>
            <OutputClaimsTransformation ReferenceId="CreateRandomUPNUserName"/>
            <OutputClaimsTransformation ReferenceId="CreateUserPrincipalName"/>
            <OutputClaimsTransformation ReferenceId="CreateAlternativeSecurityId"/>
            <OutputClaimsTransformation ReferenceId="CreateSubjectClaimFromAlternativeSecurityId"/>
          </OutputClaimsTransformations>
          <UseTechnicalProfileForSessionManagement ReferenceId="SM-Saml-idp"/>
        </TechnicalProfile>
      </TechnicalProfiles>
    </ClaimsProvider>
    ```

1. Замените `your-ADFS-domain` на имя своего домена ADFS и замените значение исходящего утверждения **identityProvider** на свой DNS (произвольное значение, указывающее домен).

1. Откройте раздел `<ClaimsProviders>` и добавьте следующий фрагмент кода XML. Если политика уже содержит технический профиль `SM-Saml-idp`, перейдите к следующему шагу. Дополнительные сведения см. в разделе об [управлении сеансом единого входа](custom-policy-reference-sso.md).

    ```xml
    <ClaimsProvider>
      <DisplayName>Session Management</DisplayName>
      <TechnicalProfiles>
        <TechnicalProfile Id="SM-Saml-idp">
          <DisplayName>Session Management Provider</DisplayName>
          <Protocol Name="Proprietary" Handler="Web.TPEngine.SSO.SamlSSOSessionProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
          <Metadata>
            <Item Key="IncludeSessionIndex">false</Item>
            <Item Key="RegisterServiceProviders">false</Item>
          </Metadata>
        </TechnicalProfile>
      </TechnicalProfiles>
    </ClaimsProvider>
    ```

1. Сохраните файл.

### <a name="upload-the-extension-file-for-verification"></a>Отправка файла расширения для проверки

К этому моменту политика настроена, так что Azure AD B2C знает, как взаимодействовать с учетной записью ADFS. Попробуйте отправить файл расширения политики, чтобы убедиться, что все в порядке.

1. На странице **Пользовательские политики** в клиенте Azure AD B2C выберите **Отправить политику**.
2. Включите функцию **Перезаписать политику, если она уже существует**, а затем найдите и выберите файл *TrustFrameworkExtensions.xml*.
3. Щелкните **Отправить**.

> [!NOTE]
> Расширение Visual Studio Code B2C использует "СоЦиалидпусерид". Для ADFS также требуется политика социальных сетей.
>

## <a name="register-the-claims-provider"></a>Регистрация поставщика утверждений

На этом этапе поставщик удостоверений уже настроен, но еще недоступен ни на одном экране регистрации или входа. Чтобы сделать его доступным, необходимо создать дубликат существующего шаблона пути взаимодействия пользователя, а затем изменить его таким образом, чтобы он также содержал поставщик удостоверений ADFS.

1. Откройте файл *TrustFrameworkBase.xml* из начального пакета.
2. Найдите и скопируйте все содержимое элемента **UserJourney**, в котором присутствует запись `Id="SignUpOrSignIn"`.
3. Откройте файл *TrustFrameworkExtensions.xml* и найдите элемент **UserJourneys**. Если элемент не существует, добавьте его.
4. Вставьте все скопированное содержимое элемента **UserJourney** в качестве дочернего элемента в элемент **UserJourneys**.
5. Переименуйте идентификатор пути взаимодействия пользователя. Например, `SignUpSignInADFS`.

### <a name="display-the-button"></a>Отображение кнопки

Элемент **ClaimsProviderSelection** является аналогом кнопки поставщика удостоверений на экране регистрации или входа. Если вы добавите для учетной записи ADFS элемент **ClaimsProviderSelection**, при переходе пользователя на страницу отобразится новая кнопка.

1. Найдите элемент **OrchestrationStep**, содержащий `Order="1"` в созданном пути взаимодействия пользователя.
2. Добавьте следующий элемент под элементом **ClaimsProviderSelections**. Установите для параметра **TargetClaimsExchangeId** соответствующее значение, например `ContosoExchange`:

    ```xml
    <ClaimsProviderSelection TargetClaimsExchangeId="ContosoExchange" />
    ```

### <a name="link-the-button-to-an-action"></a>Связывание кнопки с действием

Теперь, когда у вас есть кнопка, вам необходимо связать ее с действием. В этом случае действие — это возможность взаимодействия Azure AD B2C с учетной записью ADFS для получения токена.

1. Найдите элемент **OrchestrationStep**, содержащий `Order="2"` в пути пользователя.
2. Добавьте следующий элемент **ClaimsExchange**, убедившись, что для идентификатора используется то же значение, которое использовалось для **TargetClaimsExchangeId**.

    ```xml
    <ClaimsExchange Id="ContosoExchange" TechnicalProfileReferenceId="Contoso-SAML2" />
    ```

    Обновите значение **TechnicalProfileReferenceId**, присвоив ему значение идентификатора ранее созданного технического профиля. Например, `Contoso-SAML2`.

3. Сохраните файл *TrustFrameworkExtensions.xml* и повторно отправьте его для проверки.


## <a name="configure-an-adfs-relying-party-trust"></a>Настройка отношения доверия с проверяющей стороной ADFS

Чтобы использовать ADFS в качестве поставщика удостоверений в Azure AD B2C, необходимо сначала создать отношение доверия с проверяющей стороной ADFS с использованием метаданных SAML в Azure AD B2C. В указанном ниже примере показан URL-адрес метаданных SAML, связанных с техническим профилем Azure AD B2C.

```
https://your-tenant-name.b2clogin.com/your-tenant-name.onmicrosoft.com/your-policy/samlp/metadata?idptp=your-technical-profile
```

Измените следующие значения:

- **ваш клиент** с именем клиента, например Your-tenant.onmicrosoft.com.
- **your-policy** — именем собственной политики. Например, B2C_1A_signup_signin_adfs.
- **ваш-технический профиль** с именем технического профиля поставщика удостоверений SAML. Например, Contoso-SAML2.

Откройте браузер и перейдите по URL-адресу. Убедитесь, что ввели правильный URL-адрес и что имеете доступ к XML-файлу метаданных. Чтобы добавить новое отношение доверия с проверяющей стороной с помощью оснастки управления AD FS и вручную настроить параметры, выполните указанную ниже процедуру на сервере федерации. Для выполнения этой процедуры требуется как минимум членство в группах **Администраторы** или эквивалент на локальном компьютере.

1. В диспетчере сервера выберите **Средства**, а затем — **ADFS Management** (Управление ADFS).
2. Выберите **Добавить отношение доверия с проверяющей стороной**.
3. На странице **приветствия** выберите **Поддерживающие утверждения** и нажмите кнопку **Запустить**.
4. На странице **выбора источника данных** выберите параметр **импорта данных о проверяющей стороне, опубликованных в Интернете или локальной сети**, укажите URL-адрес метаданных Azure AD B2C и нажмите кнопку **Далее**.
5. На странице **Указание отображаемого имени** введите **отображаемое имя**. В поле **Примечания** введите описание для этого отношения доверия с проверяющей стороной и нажмите кнопку **Далее**.
6. На странице **Выбрать политику управления доступом** выберите политику и нажмите кнопку **Далее**.
7. На странице **Готовность для добавления отношения доверия** проверьте параметры, а затем нажмите кнопку **Далее**, чтобы сохранить сведения об отношениях доверия с проверяющей стороной.
8. На странице **Готово** щелкните **Закрыть**. При этом автоматически откроется диалоговое окно **Edit Claim Rules** (Изменение правил утверждений).
9. Выберите **Добавить правило**.
10. В разделе **Claim rule template** (Шаблон правила утверждения) выберите **Send LDAP attributes as claims** (Отправка атрибутов LDAP как утверждений).
11. Укажите **имя правила утверждения**. В поле **хранилище атрибутов**выберите **выбрать Active Directory**, добавьте следующие утверждения, а затем нажмите кнопку **Готово** и **ОК**.

    | Атрибут LDAP | Тип исходящего утверждения |
    | -------------- | ------------------- |
    | User-Principal-Name | userPrincipalName |
    | Surname | family_name |
    | Given-Name | given_name |
    | E-Mail-Address | email |
    | Display-Name | name |

    Обратите внимание, что эти имена не отображаются в раскрывающемся списке Тип исходящего утверждения. Их необходимо ввести вручную в. (Раскрывающийся список фактически доступен для редактирования).

12.  В зависимости от типа сертификата может потребоваться задать хэш-алгоритм. В окне свойств отношения доверия с проверяющей стороной (демоверсия B2C) перейдите на вкладку **Дополнительно**, измените **алгоритм SHA** на `SHA-256` и нажмите кнопку **ОК**.
13. В диспетчере сервера выберите **Средства**, а затем — **ADFS Management** (Управление ADFS).
14. Выберите созданное отношение доверия проверяющей стороны, щелкните **Update from Federation Metadata** (Обновить с метаданных федерации), а затем выберите **Обновить**.

## <a name="create-an-azure-ad-b2c-application"></a>Создание приложения Azure AD B2C

Взаимодействие с Azure AD B2C осуществляется с помощью приложения, зарегистрированного в клиенте B2C. В этом разделе перечислены необязательные действия, которые можно выполнить, чтобы создать тестовое приложение, если вы его еще не создали.

[!INCLUDE [active-directory-b2c-appreg-idp](../../includes/active-directory-b2c-appreg-idp.md)]

### <a name="update-and-test-the-relying-party-file"></a>Обновление и тестирование файла проверяющей стороны

Обновите файл проверяющей стороны, который активирует созданный путь взаимодействия пользователя.

1. Создайте копию *SignUpOrSignIn.xml* в рабочем каталоге и переименуйте ее. Например, переименуйте ее в *SignUpSignInADFS.xml*.
2. Откройте новый файл и обновите значение атрибута **PolicyId** для **TrustFrameworkPolicy**, указав уникальное значение. Например, `SignUpSignInADFS`.
3. Обновите значение **PublicPolicyUri**, указав URI для политики. Например: `http://contoso.com/B2C_1A_signup_signin_adfs`.
4. Обновите значение атрибута **ReferenceId** для элемента **DefaultUserJourney**, чтобы он соответствовал идентификатору созданного вами нового пути взаимодействия пользователя (SignUpSignInADFS).
5. Сохраните изменения, отправьте файл, а затем выберите в списке новую политику.
6. Убедитесь, что созданное приложение Azure AD B2C выбрано в поле **Выберите приложение**, а затем протестируйте его, щелкнув **Запустить сейчас**.

