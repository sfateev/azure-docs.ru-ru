---
title: Подключение SSL/TLS — база данных Azure для MySQL
description: Сведения о настройке базы данных Azure для MySQL и связанных приложений для правильного использования SSL-соединений.
author: kummanish
ms.author: manishku
ms.service: mysql
ms.topic: conceptual
ms.date: 07/09/2020
ms.openlocfilehash: 2969c963b491e4b08a0959d548e43ba11276d28a
ms.sourcegitcommit: 7dacbf3b9ae0652931762bd5c8192a1a3989e701
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/16/2020
ms.locfileid: "92126555"
---
# <a name="ssltls-connectivity-in-azure-database-for-mysql"></a>Подключение SSL/TLS в базе данных Azure для MySQL

База данных Azure для MySQL поддерживает подключение сервера базы данных к клиентским приложениям с помощью SSL (Secure Sockets Layer). Применение SSL-соединений между сервером базы данных и клиентскими приложениями обеспечивает защиту от атак "злоумышленник в середине" за счет шифрования потока данных между сервером и приложением.

> [!NOTE]
> Обновление `require_secure_transport` значения параметра сервера не влияет на поведение службы MySQL. Используйте функции принудительного применения SSL и TLS, описанные в этой статье, для защиты подключений к базе данных.

>[!NOTE]
> Основываясь на отзывах клиентов, мы расширили устаревший корневой сертификат для нашего существующего корневого ЦС Baltimore до 15 февраля 2021 (02/15/2021).

> [!IMPORTANT] 
> Срок действия корневого сертификата SSL истекает 15 февраля 2021 (02/15/2021). Обновите приложение, чтобы использовать [новый сертификат](https://cacerts.digicert.com/DigiCertGlobalRootG2.crt.pem). Дополнительные сведения см. в разделе [запланированные обновления сертификатов](concepts-certificate-rotation.md) .

## <a name="ssl-default-settings"></a>Параметры SSL по умолчанию

По умолчанию в службе базы данных должно быть настроено обязательное использование SSL-соединений при подключении к MySQL.  Мы рекомендуем не отключать параметр SSL без необходимости.

При подготовке нового сервера базы данных Azure для MySQL с помощью портала Azure и интерфейса командной строки применение SSL-соединений включается по умолчанию. 

Строки подключения для различных языков программирования отображаются на портале Azure. Они включают необходимые параметры SSL для подключения к базе данных. На портале Azure выберите свой сервер. В разделе **Параметры** выберите **Строки подключения**. Значение параметра SSL зависит от соединителя. Например, может использоваться "ssl=true", "sslmode=require", "sslmode=required" или другой вариант.

В некоторых случаях для безопасного подключения приложениям требуется локальный файл сертификата, созданный из файла сертификата доверенного центра сертификации (ЦС). Сейчас клиенты могут **использовать только** предопределенный сертификат для подключения к серверу базы данных Azure для MySQL, расположенному по адресу https://www.digicert.com/CACerts/BaltimoreCyberTrustRoot.crt.pem . 

Аналогично, следующие ссылки указывают на сертификаты для серверов в облаках независимых: [Azure для государственных организаций](https://www.digicert.com/CACerts/BaltimoreCyberTrustRoot.crt.pem), [Azure для Китая](https://dl.cacerts.digicert.com/DigiCertGlobalRootCA.crt.pem)и [Azure для Германии](https://www.d-trust.net/cgi-bin/D-TRUST_Root_Class_3_CA_2_2009.crt).

Чтобы узнать, как включить или отключить SSL-соединение при разработке приложения, ознакомьтесь со [статьей, посвященной настройке SSL](howto-configure-ssl.md).

## <a name="tls-enforcement-in-azure-database-for-mysql"></a>Применение TLS в базе данных Azure для MySQL

База данных Azure для MySQL поддерживает шифрование для клиентов, подключающихся к серверу базы данных с помощью протокола TLS. TLS — это стандартный промышленный протокол, обеспечивающий безопасность сетевых подключений между сервером базы данных и клиентскими приложениями, что позволяет соблюдать требования соответствия.

### <a name="tls-settings"></a>Параметры протокола TLS

База данных Azure для MySQL обеспечивает возможность принудительного применения версии TLS для клиентских подключений. Чтобы применить версию TLS, используйте параметр **минимальной версии TLS** . Для этого параметра можно использовать следующие значения:

|  Минимальная Настройка TLS             | Поддерживаемая версия клиента TLS                |
|:---------------------------------|-------------------------------------:|
| Тлсенфорцементдисаблед (по умолчанию) | TLS не требуется                      |
| TLS1_0                           | TLS 1,0, TLS 1,1, TLS 1,2 и более поздние версии           |
| TLS1_1                           | TLS 1,1, TLS 1,2 и более поздние версии                   |
| TLS1_2                           | TLS версии 1,2 и выше                     |


Например, если установить для параметра минимальной версии протокола TLS значение TLS 1,0, то сервер разрешит подключения клиентов, использующих TLS 1,0, 1,1 и 1.2. Кроме того, если задать для этого параметра значение 1,2, то разрешаются подключения только от клиентов, использующих TLS 1.2 +, а все соединения с TLS 1,0 и TLS 1,1 будут отклонены.

> [!Note] 
> По умолчанию база данных Azure для MySQL не применяет минимальную версию TLS (параметр `TLSEnforcementDisabled` ).
>
> После применения минимальной версии TLS вы не сможете позже отключить минимальную защиту версий.

Сведения о настройке параметра TLS для базы данных Azure для MySQL см. в разделе [Настройка параметра TLS](howto-tls-configurations.md).

## <a name="cipher-support-by-azure-database-for-mysql-single-server"></a>Поддержка шифра для одного сервера базы данных Azure для MySQL

При обмене данными по протоколу SSL или TLS комплекты шифров проходят проверку и поддерживают только масти шифров, которые могут обмениваться данными с Serer базы данных. Проверка комплекта шифров контролируется на [уровне шлюза](concepts-connectivity-architecture.md#connectivity-architecture) , а не на самом узле. Если комплекты шифров не соответствуют одному из перечисленных ниже пакетов, входящие клиентские соединения будут отклонены.

### <a name="cipher-suite-supported"></a>Поддерживаемый набор шифров

*   TLS_ECDHE_RSA_WITH_AES_256_GCM_SHA384
*   TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256
*   TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA384
*   TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA256

## <a name="next-steps"></a>Дальнейшие шаги

- [Библиотеки подключений для базы данных Azure для MySQL](concepts-connection-libraries.md)
- Узнайте, как [настроить SSL](howto-configure-ssl.md)
- Узнайте, как [настроить TLS](howto-tls-configurations.md)
