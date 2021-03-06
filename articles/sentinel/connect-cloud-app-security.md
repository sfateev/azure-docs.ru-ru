---
title: Подключение Cloud App Security данных к Azure Sentinel | Документация Майкрософт
description: Узнайте, как использовать соединитель Microsoft Cloud App Security (МКАС) для потоковой передачи предупреждений и Cloud Discovery журналов из МКАС в Azure Sentinel. 
services: sentinel
documentationcenter: na
author: yelevin
manager: rkarlin
editor: ''
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/24/2020
ms.author: yelevin
ms.openlocfilehash: 5c67f517ffd3704644fbe8041f3ac1e7b0d1f09c
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2020
ms.locfileid: "90904554"
---
# <a name="connect-data-from-microsoft-cloud-app-security"></a>Подключение данных из Microsoft Cloud App Security 

Соединитель [Microsoft Cloud App Security](https://docs.microsoft.com/cloud-app-security/what-is-cloud-app-security) (МКАС) позволяет передавать оповещения и [Cloud Discovery журналы](https://docs.microsoft.com/cloud-app-security/tutorial-shadow-it) из МКАС в метку Azure. Это позволит вам получить сведения об облачных приложениях, получить комплексную аналитику для определения и борьбы с киберугроз и контролировать, как передаются данные.

## <a name="prerequisites"></a>Предварительные требования

- Пользователь должен иметь разрешения на чтение и запись в рабочей области.
- Пользователь должен иметь разрешения глобального администратора или администратора безопасности в клиенте рабочей области.
- Чтобы выполнить потоковую передачу журналов Cloud Discovery в Azure Sentinel, [включите метку Azure в качестве SIEM в Microsoft Cloud App Security](https://aka.ms/AzureSentinelMCAS).

> [!IMPORTANT]
> Прием журналов Cloud Discovery в настоящее время находится в общедоступной предварительной версии.
> Эта функция предоставляется без соглашения об уровне обслуживания и не рекомендуется для рабочих нагрузок.
> Дополнительные сведения см. в статье [Дополнительные условия использования предварительных выпусков Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).
 
## <a name="connect-to-cloud-app-security"></a>Подключение к Cloud App Security

Если у вас уже есть Cloud App Security, убедитесь, что он [включен в сети](https://docs.microsoft.com/cloud-app-security/getting-started-with-cloud-app-security).
Если Cloud App Security развернут и принимает данные, данные предупреждений можно легко передать в Azure Sentinel.


1. В меню навигации меток Azure выберите **соединители данных**. В списке соединителей щелкните плитку **Microsoft Cloud App Security** , а затем нажмите кнопку **Открыть соединительную страницу** в правом нижнем углу.

1. Выберите журналы для потоковой передачи в Azure Sentinel; можно выбрать **оповещения** и **журналы Cloud Discovery** (Предварительная версия). 

1. Щелкните **Применить изменения**.

1. Вы можете выбрать, будут ли оповещения Azure Defender из центра безопасности Azure автоматически создавать инциденты в Azure Sentinel. В разделе **Создание инцидентов**выберите **включено** , чтобы включить правило аналитики по умолчанию, которое автоматически создает инциденты на основе оповещений. Затем можно изменить это правило в разделе **аналитика**на вкладке  **активные правила** .

1. Чтобы использовать соответствующую схему в Log Analytics для Cloud App Security предупреждений, введите `SecurityAlert` в окне запроса. Для схемы Cloud Discovery журналов введите `McasShadowItReporting` .

> [!NOTE]
> Cloud Discovery помогает обнаруживать и выявлять тенденции путем статистической обработки подключений пользователей базовых данных к облачным приложениям.
>
> Так как данные Cloud Discovery объединяются в один день, имейте в виду, что до 24 часов последние данные не будут отражены в Azure Sentinel. В случае, если исследование низкого уровня требует более немедленных данных, оно должно выполняться непосредственно в исходном устройстве или в службе, где хранятся необработанные данные.

## <a name="next-steps"></a>Дальнейшие шаги
В этом документе вы узнали, как подключить Microsoft Cloud App Security к Azure Sentinel. Ознакомьтесь с дополнительными сведениями об Azure Sentinel в соответствующих статьях.
- Узнайте, как [отслеживать свои данные и потенциальные угрозы](quickstart-get-visibility.md).
- Приступите к обнаружению угроз с помощью Azure Sentinel, используя [встроенные](tutorial-detect-threats.md) или [Настраиваемые](tutorial-detect-threats-custom.md) правила.
