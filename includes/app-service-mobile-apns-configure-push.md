---
author: conceptdev
ms.author: crdun
ms.service: app-service-mobile
ms.topic: include
ms.date: 08/23/2018
ms.openlocfilehash: 852a3b00e8513f9ce68c5aae54c974505d0c9af6
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/28/2020
ms.locfileid: "67185785"
---
1. На компьютере Mac запустите программу **Keychain Access**. На панели навигации слева в разделе **Category** (Категория) откройте **My Certificates** (Мои сертификаты). Найдите и откройте скачанный на предыдущем этапе SSL-сертификат. Выберите только сертификат (без закрытого ключа). Затем [экспортируйте его](https://support.apple.com/kb/PH20122?locale=en_US).
2. На [портале Azure](https://portal.azure.com/) последовательно выберите **Browse All (Просмотреть все)** > **Службы приложений**. Выберите серверную часть для функции "Мобильные приложения". 
3. В разделе **Параметры** выберите **Push-уведомления службы приложений**. Затем выберите имя концентратора уведомлений. 
4. Перейти к **Apple Push Уведомления Услуги** > **Загрузка Сертификат**. Передайте P12-файл, выбрав правильный **режим** (в зависимости от того, откуда получен SSL-сертификат клиента: из рабочей среды или песочницы). Сохраните внесенные изменения.

Теперь ваша служба настроена для работы с push-уведомлениями в iOS.

[1]: ./media/app-service-mobile-apns-configure-push/mobile-push-notification-hub.png
