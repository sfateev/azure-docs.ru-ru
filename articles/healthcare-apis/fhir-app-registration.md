---
title: Регистрация приложений Azure Active Directory для Azure API для FHIR
description: В этом учебнике содержатся сведения о типах приложений, которые необходимо зарегистрировать для Azure API для FHIR и FHIR Server для Azure.
services: healthcare-apis
ms.service: healthcare-apis
ms.subservice: fhir
ms.topic: overview
ms.reviewer: dseven
ms.author: matjazl
author: matjazl
ms.date: 10/13/2019
ms.openlocfilehash: 22f31cf3911b5ea24e8798fb226e389071fadd0b
ms.sourcegitcommit: eb6bef1274b9e6390c7a77ff69bf6a3b94e827fc
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/05/2020
ms.locfileid: "87848984"
---
# <a name="register-the-azure-active-directory-apps-for-azure-api-for-fhir"></a>Регистрация приложений Azure Active Directory для Azure API для FHIR

При настройке Azure API для FHIR или FHIR Server для Azure (OSS) можно выбрать один из нескольких вариантов конфигурации. Для открытого кода необходимо создать собственную регистрацию приложения-ресурса. Для Azure API для FHIR это приложение ресурсов создается автоматически.

## <a name="application-registrations"></a>Регистрация приложений

Чтобы приложение взаимодействовало с Azure AD, его необходимо зарегистрировать. В контексте сервера FHIR существует два вида регистрации приложений:

1. Регистрация приложений-ресурсов.
1. Регистрация клиентских приложений.

**Приложения-ресурсы** — это представления API или ресурса в Azure AD, защищенные с помощью Azure AD, в частности Azure API для FHIR. Приложение-ресурс для Azure API для FHIR будет создано автоматически при подготовке службы, но при использовании сервера с открытым исходным кодом необходимо [зарегистрировать приложение-ресурс](register-resource-azure-ad-client-app.md) в Azure AD. Это приложение-ресурс будет иметь URI идентификатора. Рекомендуется, чтобы этот URI совпадал с URI сервера FHIR. Этот URI следует использовать в качестве `Audience` для сервера FHIR. Клиентское приложение может запросить доступ к этому серверу FHIR при запросе маркера.

*Клиентские приложения* — это регистрации клиентов, которые будут запрашивать маркеры. Часто в OAuth 2.0 мы различаем, по крайней мере, три разных типа приложений:

1. **Конфиденциальные клиенты**, также известные как веб-приложения в Azure AD. Конфиденциальные клиенты — это приложения, использующие [поток кода авторизации](https://docs.microsoft.com/azure/active-directory/develop/v1-protocols-oauth-code) для получения маркера от имени пользователя, выполнившего вход, для предоставления допустимых учетных данных. Они называются конфиденциальными клиентами, так как могут хранить секрет и предоставлять этот секрет в Azure AD при обмене кодом проверки подлинности для маркера. Так как конфиденциальные клиенты могут пройти проверку подлинности с помощью секрета клиента, они являются более надежными в сравнении с общедоступными клиентами и могут иметь более длительный срок существования маркеров. Кроме того, им можно предоставлять маркер обновления. Ознакомьтесь с подробными сведениями о [регистрации конфиденциального клиента](register-confidential-azure-ad-client-app.md). Обратите внимание, что важно зарегистрировать URL-адрес ответа, по которому клиент будет получать код авторизации.
1. **Общедоступные клиенты**. Это клиенты, которые не могут хранить секреты. Обычно это приложение для мобильных устройств или одностраничное приложение JavaScript, в котором секрет клиента может быть обнаружен пользователем. Общедоступные клиенты также используют поток кода авторизации, но им не разрешено предоставлять секрет при получении маркера. Кроме того, их маркеры имеют более краткий срок жизни, а также им нельзя предоставлять маркер обновления. Ознакомьтесь с подробными сведениями о [регистрации общедоступного клиента](register-public-azure-ad-client-app.md).
1. Клиенты службы. Эти клиенты получают маркеры от своего имени (не от имени пользователя) с помощью [потока учетных данных клиента](https://docs.microsoft.com/azure/active-directory/develop/v1-oauth2-client-creds-grant-flow). Обычно они представляют собой приложения, которые обращаются к серверу FHIR неинтерактивным образом. Примером может быть процесс приема. При использовании клиента службы не нужно запускать процесс получения маркера с вызовом конечной точки `/authorize`. Клиент службы может перейти непосредственно к конечной точке `/token` и предоставить идентификатора и секрет клиента для получения маркера. Ознакомьтесь с подробными сведениями о [регистрации клиента службы](register-service-azure-ad-client-app.md).

## <a name="next-steps"></a>Дальнейшие действия

В этом обзоре вы ознакомились с типами регистраций приложений, которые могут понадобиться для работы с API FHIR.

В соответствии с настройками ознакомьтесь с инструкциями по регистрации приложений.

* [Регистрация приложения-ресурса](register-resource-azure-ad-client-app.md)
* [Регистрация конфиденциального клиентского приложения](register-confidential-azure-ad-client-app.md)
* [Регистрация общедоступного клиентского приложения](register-public-azure-ad-client-app.md)
* [Регистрация приложения-службы](register-service-azure-ad-client-app.md)

После регистрации приложений можно развернуть Azure API для FHIR.

>[!div class="nextstepaction"]
>[Развертывание Azure API для FHIR](fhir-paas-powershell-quickstart.md)