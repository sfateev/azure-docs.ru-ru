---
title: Устранение неполадок с интерфейсом фабрики данных Azure
description: Узнайте, как устранять неполадки с помощью UX фабрики данных Azure.
services: data-factory
author: ceespino
ms.service: data-factory
ms.topic: troubleshooting
ms.date: 09/03/2020
ms.author: ceespino
ms.reviewer: daperlov
ms.openlocfilehash: 9f23155df6d9e63448b35974c331bf78c3e5f90c
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2020
ms.locfileid: "89426234"
---
# <a name="troubleshoot-azure-data-factory-ux-issues"></a>Устранение неполадок с UX в фабрике данных Azure

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

В этой статье рассматриваются распространенные методы устранения неполадок в службе "Фабрика данных Azure".

## <a name="adf-ux-not-loading"></a>UI ADF не загружается

> [!NOTE]
> Служба фабрики данных Azure официально поддерживает Microsoft ребра и Google Chrome. Использование других веб-браузеров может привести к непредвиденному или недокументированному поведению.

### <a name="third-party-cookies-blocked"></a>Сторонние файлы cookie заблокированы

Пользовательский интерфейс ADF использует файлы cookie браузера для сохранения пользовательского сеанса и обеспечения интерактивной разработки и мониторинга. Возможно, ваш браузер блокирует сторонние файлы cookie, так как вы используете сеанс режиме инкогнито или включили блокирование AD. Блокирование файлов cookie сторонних производителей может вызвать проблемы при загрузке портала, например перенаправлять на пустую страницу, https://adf.azure.com/accesstoken.html или получить предупреждающее сообщение о том, что сторонние файлы cookie заблокированы. Чтобы решить эту проблему, включите сторонние параметры файлов cookie в браузере, выполнив следующие действия.

### <a name="google-chrome"></a>Google Chrome

#### <a name="allow-all-cookies"></a>Разрешить все файлы cookie

1. Откройте **Chrome://Settings/cookies** в браузере.
1. Параметр « **Разрешить все файлы cookie** » 

    ![Разрешить все файлы cookie в Chrome](media/data-factory-ux-troubleshoot-guide/chrome-allow-all-cookies.png)
1. Обновите ADF UI и повторите попытку.

#### <a name="only-allow-adf-ux-to-use-cookies"></a>Разрешить использовать файлы cookie только в UI ADF
Если вы не хотите разрешать все файлы cookie, при необходимости можно просто разрешить использование ADF:
1. Посетите **Chrome://Settings/cookies**.
1. Выберите **Добавить** в разделе **сайты, которые всегда могут использовать файлы cookie** . 

    ![Добавление интерфейса ADF в разрешенные сайты в Chrome](media/data-factory-ux-troubleshoot-guide/chrome-only-adf-cookies-1.png)
1. Добавить сайт **ADF.Azure.com** , проверить **все файлы cookie** и сохранить. 

    ![Разрешить все файлы cookie с сайта UX ADF](media/data-factory-ux-troubleshoot-guide/chrome-only-adf-cookies-2.png)
1. Обновите ADF UI и повторите попытку.

### <a name="microsoft-edge"></a>Microsoft Edge

1. Откройте **Edge://Settings/Content/cookies** в браузере.
1. Убедитесь, что флажок **Разрешить сайтам сохранять и читать данные cookie** включен, а параметр **блокировать сторонние файлы cookie** отключен. 

    ![Разрешить все файлы cookie на границе](media/data-factory-ux-troubleshoot-guide/edge-allow-all-cookies.png)
1. Обновите ADF UI и повторите попытку.

#### <a name="only-allow-adf-ux-to-use-cookies"></a>Разрешить использовать файлы cookie только в UI ADF

Если вы не хотите разрешать все файлы cookie, при необходимости можно просто разрешить использование ADF:

1. Посетите **Edge://Settings/Content/cookies**.
1. В разделе **Разрешить** выберите **Добавить** и добавить сайт **ADF.Azure.com** . 

    ![Добавление пользовательского интерфейса ADF на разрешенные сайты в пограничных устройствах](media/data-factory-ux-troubleshoot-guide/edge-allow-adf-cookies.png)
1. Обновите ADF UI и повторите попытку.

## <a name="connection-failed-on-adf-ux"></a>Сбой подключения в UI ADF

Иногда при нажатии кнопки **проверить подключение**, **Предварительный просмотр**и т. д. на следующем снимке экрана появляется ошибка "ошибка подключения".

![Сбой подключения](media/data-factory-ux-troubleshoot-guide/connection-failed.png)

В этом случае можно сначала попробовать ту же операцию с режимом просмотра InPrivate в браузере.

Если он по-прежнему не работает, в браузере нажмите клавишу F12, чтобы открыть **средства для разработчиков**. Перейдите на вкладку **сеть** , установите флажок **отключить кэш**, повторите операцию, завершившейся сбоем, и найдите невыполненный запрос (красный цвет).

![Неудачный запрос](media/data-factory-ux-troubleshoot-guide/failed-request.png)

Затем найдите **имя узла** (в данном случае — **dpnortheurope.svc.datafactory.Azure.com**) в **URL-адресе запроса** невыполненного запроса.

Введите **имя узла** непосредственно в адресной строке браузера. Если в браузере отображается 404, это обычно означает, что клиент работает нормально, и проблема возникает на стороне службы ADF. Отправьте запрос в службу поддержки с **идентификатором действия** из сообщения об ошибке ADF UX.

![Ресурс не найден](media/data-factory-ux-troubleshoot-guide/status-code-404.png)

Если это не так или вы видите подобную ошибку ниже в браузере, это обычно означает, что у вас есть проблема на стороне клиента. Выполните действия по устранению неполадок.

![Ошибка на стороне клиента](media/data-factory-ux-troubleshoot-guide/client-side-error.png)

Откройте **командную строку** и введите команду **nslookup dpnortheurope.svc.datafactory.Azure.com**. Обычно ответ должен выглядеть следующим образом:

![Ответ команды 1](media/data-factory-ux-troubleshoot-guide/command-response-1.png)

Если вы видите нормальный ответ DNS, обратитесь за помощью в службу поддержки местного ИТ, чтобы проверить, заблокировано ли HTTPS-соединение с этим именем узла. Если проблему не удалось устранить, отправьте запрос в службу поддержки с **идентификатором действия** из сообщения об ошибке ADF UX.

Если вы видите что-то еще, это обычно означает, что при разрешении имени DNS возникла проблема с сервером DNS. Обычно изменение поставщика услуг Интернета (поставщик услуг Интернета) или DNS (например, в Google DNS 8.8.8.8) может стать возможным решением проблемы. Если проблема будет повторяться, можно еще раз попробовать команду **nslookup datafactory.Azure.com** и **nslookup Azure.com** , чтобы узнать, на каком уровне не удалось разрешить DNS-разрешение, и отправить все сведения в локальную службу ИТ-поддержки или поставщик услуг Интернета для устранения неполадок. Если они считают, что эта ошибка все еще находится на стороне Майкрософт, отправьте запрос в службу поддержки с **идентификатором действия** из сообщения об ошибке ADF UX.

![Ответ команды 2](media/data-factory-ux-troubleshoot-guide/command-response-2.png)

## <a name="change-linked-service-type-in-datasets"></a>Изменение типа связанной службы в наборах данных

Набор данных формата файла можно использовать со всеми соединителями на основе файлов, например, можно настроить набор данных Parquet в большом двоичном объекте Azure или Azure Data Lake Storage 2-го поколения. Примечание. каждый соединитель поддерживает разные наборы параметров, связанных с хранилищем данных, и с разными моделями приложений. 

В пользовательском интерфейсе для создания ADF при использовании набора данных формата файла в действии, включая операции копирования, уточняющего запроса, метаданных, удаления и в наборе данных, который необходимо указать на связанную службу другого типа из текущего (например, переключиться с файловой системы на ADLS 2-го поколения), вы увидите следующее предупреждающее сообщение. Чтобы убедиться в том, что после вашего согласия конвейеры и действия, ссылающиеся на этот набор данных, будут изменены для использования нового типа, а все существующие параметры хранилища данных, несовместимые с новым типом, будут удалены, так как они больше не применяются.

Чтобы узнать больше о поддерживаемых параметрах хранилища данных для каждого соединителя, перейдите к соответствующей статье соединителя — > свойства действия копирования, чтобы просмотреть подробный список свойств. Ознакомьтесь с [Amazon S3](connector-amazon-simple-storage-service.md), [BLOB-объектом Azure](connector-azure-blob-storage.md), [Azure Data Lake Storage 1-го поколения](connector-azure-data-lake-store.md), [Azure Data Lake Storage 2-го поколения](connector-azure-data-lake-storage.md), [хранилищем файлов Azure](connector-azure-file-storage.md), [файловой системой](connector-file-system.md), [FTP](connector-ftp.md), [Google Cloud Storage](connector-google-cloud-storage.md), [HDFS](connector-hdfs.md), [http](connector-http.md)и [SFTP](connector-sftp.md).

![Предупреждение](media/data-factory-ux-troubleshoot-guide/warning-message.png)

## <a name="next-steps"></a>Дальнейшие действия

Для получения дополнительных сведений об устранении неполадок воспользуйтесь следующими ресурсами:

* [Блог о Фабрике данных](https://azure.microsoft.com/blog/tag/azure-data-factory/)
* [Запросы на добавление функции в Фабрику данных](https://feedback.azure.com/forums/270578-data-factory)
* [Форум Stack Overflow по Фабрике данных](https://stackoverflow.com/questions/tagged/azure-data-factory)
* [Сведения о Фабрике данных в Twitter](https://twitter.com/hashtag/DataFactory)
* [Видео по Azure](https://azure.microsoft.com/resources/videos/index/)
* [Раздел вопросов и ответов на сайте Майкрософт](https://docs.microsoft.com/answers/topics/azure-data-factory.html)
