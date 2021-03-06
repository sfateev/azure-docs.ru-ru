---
title: Производительность Apache Spark — кэш ввода-вывода Azure HDInsight (Предварительная версия)
description: Узнайте о службе IO Cache для Azure HDInsight и ее использовании для повышения производительности Apache Spark.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: how-to
ms.date: 12/23/2019
ms.openlocfilehash: 3e724e6336163a092c9b4385324b1aa037295bb6
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2020
ms.locfileid: "86081763"
---
# <a name="improve-performance-of-apache-spark-workloads-using-azure-hdinsight-io-cache"></a>Повышение производительности Apache Spark рабочих нагрузок с помощью кэша ввода-вывода Azure HDInsight

IO Cache — это служба кэширования данных для Azure HDInsight, который повышает производительность заданий Apache Spark. IO Cache также поддерживает рабочие нагрузки [Apache Tez](https://tez.apache.org/) и [Apache Hive](https://hive.apache.org/), которые могут выполняться в кластерах [Apache Spark](https://spark.apache.org/). IO Cache использует компонент кэширования с открытым исходным кодом RubiX. Локальный дисковый кэш RubiX предназначен для использования с модулями аналитики больших данных, которые получают данные из систем облачного хранения. RubiX выделяется в ряду систем кэширования, поскольку использует для хранения данных не оперативную память, а твердотельные накопители (SSD). Служба IO Cache запускает серверы метаданных RubiX на каждом рабочем узле кластера и управляет ими. Она также настраивает прозрачное использование кэша RubiX для всех служб кластера.

Большинство твердотельных накопителей обеспечивают пропускную способность более 1 ГБ в секунду. Такая пропускная способность в сочетании с файловым кэшем в памяти, который поддерживает операционная система, обеспечивает возможность загружать модули обработки для вычисления больших данных, например Apache Spark. Оперативная память остается доступной Apache Spark для обработки задач с высокой нагрузкой на память, например процессов изменения порядка элементов. Эксклюзивное использование оперативной памяти позволяет Apache Spark добиться оптимального использования ресурсов.  

> [!Note]  
> В настоящее время IO Cache использует RubiX в качестве компонента кэширования, но в будущих версиях службы это может измениться. Используйте интерфейсы IO Cache, не создавая никаких прямых зависимостей от реализации RubiX.
>В настоящее время кэш операций ввода-вывода поддерживается только в хранилище больших двоичных объектов Azure.

## <a name="benefits-of-azure-hdinsight-io-cache"></a>Преимущества IO Cache для Azure HDInsight

Использование IO Cache повышает производительность заданий, которые считывают данные из хранилища BLOB-объектов Azure.

Вам не придется вносить никаких изменений в задания Spark, чтобы заметить повышение производительности от использования IO Cache. При отключенной службе IO Cache этот код Spark считывает данные напрямую из удаленного хранилища BLOB-объектов Azure: `spark.read.load('wasbs:///myfolder/data.parquet').count()`. При включенной службе IO Cache эта же строка кода считывает данные через IO Cache. В дальнейшем данные уже считываются локально с твердотельного накопителя. Рабочие узлы в кластере HDInsight оснащаются локально подключенными выделенными твердотельными накопителями. IO Cache для HDInsight использует эти локальные твердотельные накопители для кэширования, обеспечивая минимально возможный уровень задержки и повышая пропускную способность.

## <a name="getting-started"></a>Начало работы

IO Cache для Azure HDInsight по умолчанию отключен в предварительной версии. Служба IO Cache доступна в кластерах Azure HDInsight 3.6 и более поздних версий с Apache Spark 2.3.  Чтобы активировать кэш ввода-вывода в HDInsight 4,0, выполните следующие действия.

1. В веб-браузере перейдите на страницу `https://CLUSTERNAME.azurehdinsight.net`, где `CLUSTERNAME` — это имя вашего кластера.

1. Выберите службу **IO Cache** в левой части страницы.

1. Выберите **действия** (**действия службы** в HDi 3,6) и **активируйте**.

    ![Включение службы кэша ввода-вывода в Ambari](./media/apache-spark-improve-performance-iocache/ambariui-enable-iocache.png "Включение службы кэша ввода-вывода в Ambari")

1. Подтвердите перезапуск всех затрагиваемых служб в кластере.

> [!NOTE]  
> Несмотря на значение индикатора выполнения, фактически IO Cache активируется только после перезапуска всех затрагиваемых служб.

## <a name="troubleshooting"></a>Устранение неполадок
  
После включения IO Cache при выполнении заданий Spark могут возникать ошибки отсутствия места на диске. Эти ошибки связаны с тем, что Spark также использует место на локальном диске для хранения данных во время операций перетасовки. Может случиться так, что после включения IO Cache на твердотельном накопителе закончится свободное место для Spark. По умолчанию IO Cache использует половину от общего объема твердотельного накопителя. В Ambari можно настроить параметры использования места на диске для IO Cache. Если вы столкнетесь с ошибками отсутствия места на диске, сократите объем памяти на твердотельном накопителе, используемый для IO Cache, и перезапустите службу. Чтобы изменить настройки пространства для IO Cache, выполните следующие действия:

1. В Apache Ambari выберите службу **HDFS** слева.

1. Выберите вкладки **Configs** (Конфигурации) и **Advanced** (Дополнительно).

    ![Изменить расширенную конфигурацию HDFS](./media/apache-spark-improve-performance-iocache/ambariui-hdfs-service-configs-advanced.png "Изменить расширенную конфигурацию HDFS")

1. Прокрутите вниз и разверните область **Custom core-site** (Пользовательский основной сайт).

1. Найдите свойство **hadoop.cache.data.fullness.percentage**.

1. Измените значение в этом поле.

    ![Изменение полных значений в кэше ввода-вывода](./media/apache-spark-improve-performance-iocache/ambariui-cache-data-fullness-percentage-property.png "Изменение полных значений в кэше ввода-вывода")

1. Выберите **Сохранить** в правом верхнем углу.

1. Выберите **перезапустить**  >  **все затронутые**.

    ![Apache Ambari перезапускает все затронутые](./media/apache-spark-improve-performance-iocache/ambariui-restart-all-affected.png "Перезапустить все затронутые")

1. Выберите пункт **подтвердить перезагрузку все**.

Если это не сработает, отключите кэш ввода-вывода.

## <a name="next-steps"></a>Next Steps

Дополнительная информация об IO Cache, в том числе о сравнительных тестах производительности, приведена в [этой записи блога](https://azure.microsoft.com/blog/apache-spark-speedup-with-hdinsight-io-cache/)
