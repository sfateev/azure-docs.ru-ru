---
title: Конфликт портов при запуске служб в Azure HDInsight
description: Действия по устранению неполадок и возможные способы устранения проблем с конфликтом портов при взаимодействии с кластерами Azure HDInsight.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: troubleshooting
ms.date: 01/23/2020
ms.openlocfilehash: 0d39f31e9d52359d0c91317280a7f8db06c1c1b0
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2020
ms.locfileid: "90530970"
---
# <a name="scenario-port-conflict-when-starting-services-in-azure-hdinsight"></a>Сценарий: конфликт портов при запуске служб в Azure HDInsight

В этой статье описываются действия по устранению неполадок и возможные способы решения проблем при взаимодействии с кластерами Azure HDInsight.

## <a name="issue"></a>Проблема

Не удается запустить службу.

## <a name="cause"></a>Причина

Существует конфликт портов.

## <a name="resolution"></a>Решение

### <a name="method-1"></a>Метод 1

Используйте приведенные ниже команды для получения или уничтожения всех запущенных процессов, которые зависят от проблемы с портом.

```bash
netstat -lntp | grep <port>
ps -ef | grep <service>
kill -9 <service>
```

Затем запустите службу.

### <a name="method-2"></a>Метод 2

Перезагрузите узел.

## <a name="next-steps"></a>Дальнейшие действия

Если вы не видите своего варианта проблемы или вам не удается ее устранить, дополнительные сведения можно получить, посетив один из следующих каналов.

* Получите ответы специалистов Azure на [сайте поддержки сообщества пользователей Azure](https://azure.microsoft.com/support/community/).

* Подпишитесь на [@AzureSupport](https://twitter.com/azuresupport) — официальный канал Microsoft Azure для работы с клиентами. Вступайте в сообщество Azure для получения нужных ресурсов: ответов, поддержки и советов экспертов.

* Если вам нужна дополнительная помощь, отправьте запрос в службу поддержки на [портале Azure](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/). Выберите **Поддержка** в строке меню или откройте центр **Справка и поддержка**. Дополнительные сведения см. в статье [Создание запроса на поддержку Azure](https://docs.microsoft.com/azure/azure-supportability/how-to-create-azure-support-request). Доступ к управлению подписками и поддержкой выставления счетов уже включен в вашу подписку Microsoft Azure, а техническая поддержка предоставляется в рамках одного из [планов Службы поддержки Azure](https://azure.microsoft.com/support/plans/).
