---
title: 'Преобразование данных: обработка & преобразование данных '
description: Узнайте, как преобразовать или обработать данные в фабрике данных Azure с помощью Hadoop, Машинного обучения Azure или Azure Data Lake Analytics.
services: data-factory
documentationcenter: ''
author: djpmsft
ms.author: daperlov
manager: jroth
ms.reviewer: maghan
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 01/10/2018
ms.openlocfilehash: eb320cb71de43c40522bf93213fd98247a0d5b59
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2020
ms.locfileid: "89436303"
---
# <a name="transform-data-in-azure-data-factory-version-1"></a>Преобразование данных в фабрике данных Azure версии 1
> [!div class="op_single_selector"]
> * [Hive](data-factory-hive-activity.md)  
> * [Pig](data-factory-pig-activity.md)  
> * [MapReduce](data-factory-map-reduce.md)  
> * [Потоковая передача Hadoop](data-factory-hadoop-streaming-activity.md)
> * [Машинное обучение](data-factory-azure-ml-batch-execution-activity.md) 
> * [Хранимая процедура](data-factory-stored-proc-activity.md)
> * [Аналитика озера данных U-SQL](data-factory-usql-activity.md)
> * [Пользовательские действия .NET](data-factory-use-custom-activities.md)

## <a name="overview"></a>Обзор
> [!NOTE]
> В этой статье рассматривается служба "Фабрика данных Azure" версии 1. Если вы используете текущую версию Фабрики данных, см. статью о [действиях по преобразованию данных в службе "Фабрика данных"](../transform-data.md).

В этой статье объясняются действия преобразования данных в фабрике данных Azure, с помощью которых можно обрабатывать необработанные данные и преобразовывать их в прогнозы и аналитику. Действие преобразования выполняется в вычислительной среде, например в кластере Azure HDInsight или пакетной службе Azure. Статья содержит ссылки на статьи с подробными сведениями о каждом действии преобразования.

Фабрика данных поддерживает указанные ниже действия преобразования, которые вы можете добавлять в [конвейеры](data-factory-create-pipelines.md) как по отдельности, так и в цепочке с другим действием.

> [!NOTE]
> Пошаговое руководство см. в статье [Учебник. Создание первого конвейера для обработки данных с помощью кластера Hadoop](data-factory-build-your-first-pipeline.md).  
> 
> 

## <a name="hdinsight-hive-activity"></a>Действие Hive HDInsight
Действие Hive HDInsight в конвейере фабрики данных выполняет запросы Hive к вашему собственному кластеру HDInsight или кластеру HDInsight по запросу под управлением Windows или Linux. Дополнительные сведения об этом действии см. в статье о [действиях Hive](data-factory-hive-activity.md) . 

## <a name="hdinsight-pig-activity"></a>Действие Pig HDInsight
Действие Pig HDInsight в конвейере фабрики данных выполняет запросы Pig к вашему собственному кластеру HDInsight или кластеру HDInsight по запросу под управлением Windows или Linux. Дополнительные сведения об этом действии см. в статье о [действии Pig](data-factory-pig-activity.md) . 

## <a name="hdinsight-mapreduce-activity"></a>Действие MapReduce HDInsight
Действие MapReduce HDInsight в конвейере фабрики данных выполняет программы MapReduce для вашего собственного кластера HDInsight или кластера HDInsight по запросу под управлением Windows или Linux. Дополнительные сведения об этом действии см. в статье о [действии MapReduce](data-factory-map-reduce.md) .

## <a name="hdinsight-streaming-activity"></a>Действие потоковой передачи HDInsight
Действие потоковой передачи HDInsight в конвейере фабрики данных выполняет программы потоковой передачи Hadoop для вашего собственного кластера HDInsight или кластера HDInsight по запросу под управлением Windows или Linux. Дополнительные сведения об этом действии см. в разделе [Потоковая активность Hadoop](data-factory-hadoop-streaming-activity.md).

## <a name="hdinsight-spark-activity"></a>Действие HDInsight Spark
Действие HDInsight Spark в конвейере фабрики данных выполняет программы Spark в вашем кластере HDInsight. Дополнительные сведения см. в разделе [Вызов программ Spark из фабрики данных](data-factory-spark.md). 

## <a name="machine-learning-activities"></a>Действия машинного обучения
Фабрика данных Azure позволяет легко создавать конвейеры, в которых для прогнозной аналитики используется опубликованная веб-служба Машинного обучения Azure. С помощью [действия выполнения пакета](data-factory-azure-ml-batch-execution-activity.md#invoking-a-web-service-using-batch-execution-activity) в конвейере фабрики данных Azure можно вызвать веб-службу машинное обучение, чтобы сделать прогнозы по данным в пакетной службе.

Со временем прогнозные модели в оценивающих экспериментах машинного обучения потребуют повторного обучения с помощью новых входных наборов данных. Когда повторное обучение будет завершено, вам потребуется обновить веб-службу оценки на основании обновленной модели машинного обучения. [Действие обновить ресурс](data-factory-azure-ml-batch-execution-activity.md#updating-models-using-update-resource-activity) можно использовать для обновления веб-службы с использованием новой обученной модели.  

Дополнительные сведения об этих действиях машинного обучения см. в разделе [Создание прогнозных конвейеров с помощью действий машинного обучения Azure](data-factory-azure-ml-batch-execution-activity.md). 

## <a name="stored-procedure-activity"></a>Действие хранимой процедуры
Вы можете использовать действие SQL Server хранимой процедуры в конвейере фабрики данных для вызова хранимой процедуры в одном из следующих хранилищ данных: база данных SQL Azure, Azure синапсе Analytics (ранее — хранилище данных SQL), SQL Server базу данных на предприятии или виртуальную машину Azure. Дополнительные сведения см. в статье о [действии хранимой процедуры](data-factory-stored-proc-activity.md) .  

## <a name="data-lake-analytics-u-sql-activity"></a>Действие U-SQL Data Lake Analytics
Действие U-SQL Data Lake Analytics запускает сценарий U-SQL для кластера Azure Data Lake Analytics. Дополнительные сведения см. в статье о [действиях U-SQL в аналитике данных](data-factory-usql-activity.md) . 

## <a name="net-custom-activity"></a>Настраиваемое действие .NET
Если вам нужно преобразовать данные способом, который не поддерживается фабрикой данных Azure, то можно создать настраиваемое действие с собственной логикой обработки данных и использовать это действие в конвейере. Можно настроить запуск настраиваемого действия .NET с помощью пакетной службы Azure или кластера HDInsight. Дополнительные сведения см. в разделе [Использование настраиваемых действий в конвейере фабрики данных Azure](data-factory-use-custom-activities.md). 

Можно создать настраиваемое действие для выполнения сценариев R в кластере HDInsight, где установлена среда R. Ознакомьтесь с примером в репозитории GitHub [Run R Script using Azure Data Factory](https://github.com/Azure/Azure-DataFactory/tree/master/SamplesV1/RunRScriptUsingADFSample) (Запуск сценария R с помощью фабрики данных Azure). 

## <a name="compute-environments"></a>Вычислительные среды
Вы создаете связанную службу для среды вычислений, а затем используете эту службу при определении действия преобразования. Фабрика данных поддерживает вычислительные среды двух типов. 

1. **По требованию**: в этом случае вычислительная среда полностью управляется фабрикой данных. Среда автоматически создается службой фабрики данных перед отправкой задания для обработки данных и удаляется после его выполнения. Вы можете настраивать и изменять для вычислительной среды "по требованию" детализированные параметры выполнения задания, управления кластером и действий начальной загрузки. 
2. **Собственная**— в этом случае вы регистрируете собственную вычислительную среду (например, кластер HDInsight) и используете ее в качестве связанной службы в фабрике данных. Вы будете управлять средой вычислений, а служба фабрики данных — использовать ее для выполнения действий. 

В статье [Связанные службы вычислений](data-factory-compute-linked-services.md) описывается, какие службы вычислений поддерживает фабрика данных. 

## <a name="summary"></a>Итоги
Фабрика данных Azure поддерживает приведенные ниже действия преобразования данных и вычислительных сред для них. Действия преобразования можно добавлять в конвейеры как по отдельности, так и в цепочке с другим действием.

| Действия по преобразованию данных | Вычислительная среда |
|:--- |:--- |
| [Hive](data-factory-hive-activity.md) |HDInsight [Hadoop] |
| [Pig](data-factory-pig-activity.md) |HDInsight [Hadoop] |
| [MapReduce](data-factory-map-reduce.md) |HDInsight [Hadoop] |
| [Потоковая передача Hadoop](data-factory-hadoop-streaming-activity.md) |HDInsight [Hadoop] |
| [Действия машинного обучения: выполнение пакета и обновление ресурса](data-factory-azure-ml-batch-execution-activity.md) |Azure |
| [Хранимая процедура](data-factory-stored-proc-activity.md) |Azure SQL, Azure Synapse Analytics или SQL Server |
| [Аналитика озера данных U-SQL](data-factory-usql-activity.md) |Аналитика озера данных Azure |
| [DotNet](data-factory-use-custom-activities.md) |HDInsight [Hadoop] или пакетная служба Azure |

