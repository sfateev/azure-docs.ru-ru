---
title: Исключение хранилища после сброса подключения в Azure HDInsight
description: Исключение хранилища после сброса подключения в Azure HDInsight
ms.service: hdinsight
ms.topic: troubleshooting
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.date: 08/08/2019
ms.openlocfilehash: a7af6407191577112f936bfb9048985e85c868ea
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2020
ms.locfileid: "75887229"
---
# <a name="scenario-storage-exception-after-connection-reset-in-azure-hdinsight"></a>Сценарий: исключение хранилища после сброса подключения в Azure HDInsight

В этой статье описываются действия по устранению неполадок и возможные способы решения проблем при взаимодействии с кластерами Azure HDInsight.

## <a name="issue"></a>Проблема

Не удалось создать новую таблицу Apache HBase.

## <a name="cause"></a>Причина

В процессе усечения таблицы возникла ошибка подключения к хранилищу. Запись таблицы была удалена в таблице метаданных HBase. Был удален только один файл большого двоичного объекта.

Хотя в хранилище отсутствует большой двоичный объект папки `/hbase/data/default/ThatTable` . Драйвер WASB обнаружил наличие приведенного выше файла большого двоичного объекта и не сможет создавать BLOB-объекты, вызванные `/hbase/data/default/ThatTable` , так как предполагается, что родительские папки существовали, поэтому создание таблицы завершится ошибкой.

## <a name="resolution"></a>Решение

1. В пользовательском интерфейсе Apache Ambari перезапустите активный HMaster. Это позволит одному из двух резервных HMaster становится активным, а новый активный HMaster будет перезагружать сведения о таблице метаданных. Поэтому таблица не будет отображаться `already-deleted` в пользовательском интерфейсе HMaster.

1. Потерянный файл большого двоичного объекта можно найти в средствах пользовательского интерфейса, таких как Cloud Explorer или выполнение команды `hdfs dfs -ls /xxxxxx/yyyyy` , например. Выполните команду, `hdfs dfs -rmr /xxxxx/yyyy` чтобы удалить этот большой двоичный объект. Например, `hdfs dfs -rmr /hbase/data/default/ThatTable/ThatFile`.

Теперь можно создать новую таблицу с тем же именем в HBase.

## <a name="next-steps"></a>Дальнейшие действия

Если вы не видите своего варианта проблемы или вам не удается ее устранить, дополнительные сведения можно получить, посетив один из следующих каналов.

* Получите ответы специалистов Azure на [сайте поддержки сообщества пользователей Azure](https://azure.microsoft.com/support/community/).

* Подпишитесь на [@AzureSupport](https://twitter.com/azuresupport) — официальный канал Microsoft Azure для работы с клиентами. Вступайте в сообщество Azure для получения нужных ресурсов: ответов, поддержки и советов экспертов.

* Если вам нужна дополнительная помощь, отправьте запрос в службу поддержки на [портале Azure](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/). Выберите **Поддержка** в строке меню или откройте центр **Справка и поддержка**. Дополнительные сведения см. в статье [Создание запроса на поддержку Azure](https://docs.microsoft.com/azure/azure-portal/supportability/how-to-create-azure-support-request). Доступ к управлению подписками и поддержкой выставления счетов уже включен в вашу подписку Microsoft Azure, а техническая поддержка предоставляется в рамках одного из [планов Службы поддержки Azure](https://azure.microsoft.com/support/plans/).
