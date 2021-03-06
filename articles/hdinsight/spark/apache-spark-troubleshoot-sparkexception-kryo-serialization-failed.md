---
title: Проблемы с JDBC/ODBC и платформой Apache Thrift в Azure HDInsight
description: Не удается загрузить большие наборы данных с помощью JDBC/ODBC и программной платформы Apache Thrift в Azure HDInsight
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: troubleshooting
ms.date: 05/14/2020
ms.openlocfilehash: a8dcd6ae844810213ed6706002cdb9a31de94f60
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2020
ms.locfileid: "83653587"
---
# <a name="unable-to-download-large-data-sets-using-jdbcodbc-and-apache-thrift-software-framework-in-hdinsight"></a>Не удается загрузить большие наборы данных с помощью JDBC/ODBC и программной платформы Apache Thrift в HDInsight

В этой статье описываются действия по устранению неполадок и возможные способы исправления проблем, возникающих при использовании компонентов Apache Spark в кластерах Azure HDInsight.

## <a name="issue"></a>Проблема

При попытке загрузить большие наборы данных с помощью JDBC/ODBC и программной платформы Apache Thrift в Azure HDInsight отображается следующее сообщение об ошибке:

```
org.apache.spark.SparkException: Kryo serialization failed:
Buffer overflow. Available: 0, required: 36518. To avoid this, increase spark.kryoserializer.buffer.max value.
```

## <a name="cause"></a>Причина

Это исключение вызвано тем, что процесс сериализации пытается использовать больше буферного пространства, чем разрешено. В Spark 2.0.0 класс `org.apache.spark.serializer.KryoSerializer` используется для сериализации объектов при доступе к данным через программную платформу Apache Thrift. Для данных, которые будут передаваться по сети или кэшироваться в сериализованной форме, используется другой класс.

## <a name="resolution"></a>Решение

Увеличьте значение буфера `Kryoserializer`. В конфигурации spark2 в разделе `Custom spark2-thrift-sparkconf` добавьте ключ `spark.kryoserializer.buffer.max` и задайте для него значение `2047`. Перезапустите все затронутые компоненты.

> [!IMPORTANT]
> Значение `spark.kryoserializer.buffer.max` должно быть меньше 2048. Дробные значения не поддерживаются.

## <a name="next-steps"></a>Дальнейшие действия

Если вы не видите своего варианта проблемы или вам не удается ее устранить, дополнительные сведения можно получить, посетив один из следующих каналов.

* Получите ответы специалистов Azure на [сайте поддержки сообщества пользователей Azure](https://azure.microsoft.com/support/community/).

* Подключитесь к [@AzureSupport](https://twitter.com/azuresupport) — официальной учетной записи Microsoft Azure. Она помогает оптимизировать работу пользователей благодаря возможности доступа к ресурсам сообщества Azure (ответы на вопросы, поддержка и консультации специалистов)

* Если вам нужна дополнительная помощь, отправьте запрос в службу поддержки на [портале Azure](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/). Выберите **Поддержка** в строке меню или откройте центр **Справка и поддержка**. Дополнительные сведения см. в статье [Создание запроса на поддержку Azure](https://docs.microsoft.com/azure/azure-portal/supportability/how-to-create-azure-support-request). Доступ к управлению подписками и поддержкой выставления счетов уже включен в вашу подписку Microsoft Azure, а техническая поддержка предоставляется в рамках одного из [планов Службы поддержки Azure](https://azure.microsoft.com/support/plans/).
