---
title: Несколько кластеров HDInsight & одной учетной записи Azure Data Lake Storage
description: Узнайте, как использовать несколько кластеров HDInsight с одной учетной записью Azure Data Lake Storage
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: how-to
ms.custom: hdinsightactive
ms.date: 12/18/2019
ms.openlocfilehash: 19c40f2a7609d556448641e78fdeffe83e8660b1
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2020
ms.locfileid: "86083956"
---
# <a name="use-multiple-hdinsight-clusters-with-an-azure-data-lake-storage-account"></a>Использование нескольких кластеров HDInsight с учетной записью Azure Data Lake Storage

Начиная с версии HDInsight 3.5, можно создавать кластеры HDInsight с учетными записями Azure Data Lake Storage в качестве файловой системы по умолчанию.
Data Lake Storage поддерживает неограниченное пространство для хранения данных, поэтому оно идеально подходит не только для хранения больших объемов данных, но и для размещения нескольких кластеров HDInsight, которые совместно используют одну и ту же учетную запись Data Lake Storage. Инструкции по созданию кластера HDInsight с Data Lake Storage в качестве хранилища см. в разделе [Краткое руководство по настройке кластеров в hdinsight](../storage/data-lake-storage/quickstart-create-connect-hdi-cluster.md).

Эта статья содержит рекомендации для администратора Data Lake Storage по настройке единственной и общей учетной записи Data Lake Storage, которая может использоваться в нескольких **активных** кластерах HDInsight. Эти рекомендации относятся к размещению нескольких защищенных и незащищенных кластеров Apache Hadoop в общей учетной записи Data Lake Storage.

## <a name="data-lake-storage-file-and-folder-level-acls"></a>Списки управления доступом на уровне файла и папки в Data Lake Storage

В оставшейся части этой статьи предполагается, что у вас хорошие знания списков управления доступом на уровне файла и папки в Azure Data Lake Storage, которые подробно описаны в разделе [Управление доступом в Azure Data Lake Storage](../data-lake-store/data-lake-store-access-control.md).

## <a name="data-lake-storage-setup-for-multiple-hdinsight-clusters"></a>Настройка Data Lake Storage для нескольких кластеров HDInsight

Позвольте нам использовать иерархию папок из двух уровней, чтобы объяснить рекомендации по использованию нескольких кластеров HDInsight с учетной записью Data Lake Storage. Предположим, что у вас есть учетная запись Data Lake Storage со структурой папок **/clusters/finance**. С такой структурой папок все кластеры, необходимые финансовой организации, могут использовать папку /clusters/finance в качестве расположения хранилища. Если другая организация (например, маркетинговая) захочет создать кластеры, использующие ту же учетную запись Data Lake Storage, для этой организации можно просто создать папку /clusters/marketing. Пока давайте использовать папку **/clusters/finance**.

Чтобы настроить такую структуру папок для кластеров HDInsight, администратору Data Lake Storage необходимо назначить соответствующие разрешения, как описано в следующей таблице. Разрешения, показанные в таблице, соответствуют спискам управления доступом (Access-ACL), а не спискам управления доступом по умолчанию (Default-ACL).

|Папка  |Разрешения  |владельца  |группы владельцев  | Именованный пользователь | Разрешения именованного пользователя | Именованная группа | Разрешения именованной группы |
|---------|---------|---------|---------|---------|---------|---------|---------|
|/ | rwxr-x--x  |admin |admin  |Субъект-служба |--x  |FINGRP   |r-x         |
|/clusters | rwxr-x--x |admin |admin |Субъект-служба |--x  |FINGRP |r-x         |
|/clusters/finance | rwxr-x--t |admin |FINGRP  |Субъект-служба |rwx  |-  |-     |

В этой таблице:

- **admin** — создатель и администратор учетной записи Data Lake Storage.
- **Субъект-служба** — субъект-служба Azure Active Directory (AAD), связанная с учетной записью.
- **FINGRP** — группа пользователей, созданная в AAD и содержащая пользователей из финансовой организации.

Инструкции по созданию приложения AAD (при создании которого также создается субъект-служба) см. в разделе [Создание приложения AAD](../active-directory/develop/howto-create-service-principal-portal.md#register-an-application-with-azure-ad-and-create-a-service-principal). Инструкции по созданию группы пользователей в AAD см. в разделе [Управление группами в Azure Active Directory](../active-directory/fundamentals/active-directory-groups-create-azure-portal.md).

Необходимо учесть следующие основные моменты.

- **Перед** использованием учетной записи хранения для кластеров администратору Data Lake Storage необходимо создать и подготовить двухуровневую структуру папок (**/clusters/finance/**) с соответствующими разрешениями. Эта структура не создается автоматически при создании кластеров.
- В приведенном выше примере рекомендуется установить **FINGRP** в качестве группы-владельца **/clusters/finance** и установить для FINGRP разрешения **r-x** для всей структуры папок начиная с корневой папки. Это позволит участникам группы FINGRP перемещаться по всей структуре папок начиная с корневой папки.
- В том случае, когда различные субъекты-службы AAD могут создавать кластеры в каталоге **/clusters/finance**, бит sticky (установленный для папки **finance**) гарантирует, что папки, созданные одним субъектом-службой, не могут быть удалены другими субъектами-службами.
- После того как структура папок и разрешения установлены, процесс создания кластера HDInsight создает место хранения, определенное в кластере, в разделе **/Clusters/Finance/**. Например, в качестве расположения для кластера с именем fincluster01 может использоваться папка **/clusters/finance/fincluster01**. В таблице ниже показаны владельцы и разрешения для папок, созданных для кластера HDInsight.

    |Папка  |Разрешения  |владельца  |группы владельцев  | Именованный пользователь | Разрешения именованного пользователя | Именованная группа | Разрешения именованной группы |
    |---------|---------|---------|---------|---------|---------|---------|---------|
    |/clusters/finanace/ fincluster01 | rwxr-x---  |Субъект-служба |FINGRP  |- |-  |-   |-  |

## <a name="recommendations-for-job-input-and-output-data"></a>Рекомендации по входным и выходным данным задания

Входные и выходные данные задания рекомендуется размещать в папке за пределами папки **/clusters**. Это гарантирует, что даже если папка, относящаяся к кластеру, будет удалена для освобождения места в хранилище, входные и выходные данные задания будут по-прежнему доступны для будущего использования. В этом случае убедитесь, что иерархия папок для хранения входных и выходных данных задания обеспечивает соответствующий уровень доступа для субъекта-службы.

## <a name="limit-on-clusters-sharing-a-single-storage-account"></a>Ограничение на число кластеров, которые могут использовать общую учетную запись хранения

Ограничение на число кластеров, которые могут использовать общую учетную запись Data Lake Storage, зависит от рабочей нагрузки, выполняемой на этих кластерах. Слишком большое число кластеров или очень интенсивные рабочие нагрузки на кластерах, которые используют общую учетную запись хранения, могут привести к регулированию входящего/исходящего трафика для учетной записи хранения.

## <a name="support-for-default-acls"></a>Поддержка списков управления доступом по умолчанию

При создании субъекта-службы с доступом именованного пользователя (как показано в таблице выше) рекомендуется **не** добавлять именованного пользователя со списком управления доступом по умолчанию. При подготовке доступа именованного пользователя со списками управления доступом по умолчанию назначаются разрешения 770 для пользователя-владельца, группы-владельца и остальных пользователей. Хотя это значение по умолчанию 770 не принимает разрешений от владеющего пользователя (7) или группы-владельца (7), оно получает все разрешения для других (0). Это приводит к известной проблеме, которая подробно описана в разделе [Известные проблемы и их решения](#known-issues-and-workarounds).

## <a name="known-issues-and-workarounds"></a>Известные проблемы и их решения

В этом разделе перечислены известные проблемы, которые возникают при использовании HDInsight с Data Lake Storage, и их решения.

### <a name="publicly-visible-localized-apache-hadoop-yarn-resources"></a>Открытые локализованные ресурсы Apache Hadoop YARN

При создании новой учетной записи Azure Data Lake Storage корневая папка автоматически подготавливается с разрешениями 770 для списка управления доступом. В качестве пользователя — владельца корневой папки устанавливается пользователь, который создал учетную запись (администратор Data Lake Storage), а в качестве группы-владельца — основная группа пользователя, который создал учетную запись. Доступ для других пользователей не предоставляется.

Эти параметры влияют на один из вариантов использования HDInsight, который описан в разделе [YARN 247](https://hwxmonarch.atlassian.net/browse/YARN-247). Отправка задания может завершиться сбоем со следующим сообщением об ошибке:

```output
Resource XXXX is not publicly accessible and as such cannot be part of the public cache.
```

Как указано в статье YARN JIRA, ссылка на которую приведена выше, при локализации общедоступных ресурсов средство локализации проверяет, что все запрошенные ресурсы действительно являются общедоступными, анализируя их разрешения в удаленной файловой системе. Все LocalResource, которые не соответствуют этому условию, отклоняются для локализации. Проверка разрешений включает проверку доступа к файлу на чтение для остальных пользователей. При размещении кластеров HDInsight на Azure Data Lake этот сценарий не работает, так как Azure Data Lake запрещает доступ к "другим" на уровне корневой папки.

#### <a name="workaround"></a>Обходной путь

Установите права на чтение и выполнение для **остальных пользователей** по всей иерархии папок, например для папок **/**, **/clusters** и **/clusters/finance**, как показано в таблице выше.

## <a name="see-also"></a>См. также раздел

- [Краткое руководство по установке кластеров в HDInsight](../storage/data-lake-storage/quickstart-create-connect-hdi-cluster.md)
- [Использование Azure Data Lake Storage Gen2 с кластерами Azure HDInsight](hdinsight-hadoop-use-data-lake-storage-gen2.md)
