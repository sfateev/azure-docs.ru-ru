---
title: На узле кластера заканчивается свободное место на диске в Azure HDInsight
description: Устранение неполадок, связанных с Apache Hadoop места на диске узла кластера в Azure HDInsight.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: troubleshooting
ms.date: 04/30/2020
ms.openlocfilehash: ead79ca0a37a270f03a305064c80426553db59ca
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2020
ms.locfileid: "82628543"
---
# <a name="scenario-cluster-node-runs-out-of-disk-space-in-azure-hdinsight"></a>Сценарий: на узле кластера заканчивается свободное место на диске в Azure HDInsight

В этой статье описываются действия по устранению неполадок и возможные способы решения проблем при взаимодействии с кластерами Azure HDInsight.

## <a name="issue"></a>Проблема

Задание может завершиться ошибкой с сообщением об ошибке следующего вида: `/usr/hdp/2.6.3.2-14/hadoop/libexec/hadoop-config.sh: fork: No space left on device.`

Или вы можете получить предупреждение Apache Ambari, похожее на: `local-dirs usable space is below configured utilization percentage` .

## <a name="cause"></a>Причина

Кэш приложения Apache Yarn мог потреблять все доступное место на диске. Скорее всего, приложение Spark работает неэффективно.

## <a name="resolution"></a>Решение

1. Используйте пользовательский интерфейс Ambari, чтобы определить, на каком узле заканчивается свободное место.

1. Определите, какая папка в узле беспокоят влияет на большую часть дискового пространства. Сначала подключитесь к узлу по протоколу SSH, а затем выполните команду `df` , чтобы вывести сведения об использовании диска для всех подключений. Обычно это `/mnt` временный диск, используемый OSS. Можно ввести в папку, а затем ввести, `sudo du -hs` чтобы отобразить обобщенные размеры файлов в папке. Если вы видите папку, аналогичную `/mnt/resource/hadoop/yarn/local/usercache/livy/appcache/application_1537280705629_0007` , это означает, что приложение все еще работает. Это могло произойти из-за RDD сохраняемости или промежуточного случайного файла.

1. Чтобы устранить эту ошибку, завершите работу приложения, которое освободит место на диске, используемое этим приложением.

1. Если эта ошибка часто возникает на рабочих узлах, можно настроить параметры локального кэша YARN в кластере.

    Откройте пользовательский интерфейс Ambari, перейдите в раздел YARN--> configs (> Advanced).  
    Добавьте следующие 2 свойства в раздел Custom yarn-site.xml и сохраните:

    ```
    yarn.nodemanager.localizer.cache.target-size-mb=2048
    yarn.nodemanager.localizer.cache.cleanup.interval-ms=300000
    ```

1. Если описанная выше проблема не устраняет проблему постоянно, оптимизируйте приложение.

## <a name="next-steps"></a>Дальнейшие действия

Если вы не видите своего варианта проблемы или вам не удается ее устранить, дополнительные сведения можно получить, посетив один из следующих каналов.

* Получите ответы специалистов Azure на [сайте поддержки сообщества пользователей Azure](https://azure.microsoft.com/support/community/).

* Подключитесь к [@AzureSupport](https://twitter.com/azuresupport) — официальной учетной записи Microsoft Azure. Она помогает оптимизировать работу пользователей благодаря возможности доступа к ресурсам сообщества Azure (ответы на вопросы, поддержка и консультации специалистов).

* Если вам нужна дополнительная помощь, отправьте запрос в службу поддержки на [портале Azure](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/). Выберите **Поддержка** в строке меню или откройте центр **Справка и поддержка**. Дополнительные сведения см. в статье [Создание запроса на поддержку Azure](https://docs.microsoft.com/azure/azure-portal/supportability/how-to-create-azure-support-request). Доступ к управлению подписками и поддержкой выставления счетов уже включен в вашу подписку Microsoft Azure, а техническая поддержка предоставляется в рамках одного из [планов Службы поддержки Azure](https://azure.microsoft.com/support/plans/).
