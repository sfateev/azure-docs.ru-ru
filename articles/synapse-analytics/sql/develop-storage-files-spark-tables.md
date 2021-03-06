---
title: Синхронизация Apache Spark для определений внешних таблиц в SQL по запросу (предварительная версия)
description: Общие сведения о запросе таблиц Spark с помощью SQL по запросу (предварительная версия)
services: synapse-analytics
author: julieMSFT
ms.service: synapse-analytics
ms.topic: overview
ms.subservice: sql
ms.date: 04/15/2020
ms.author: jrasnick
ms.reviewer: jrasnick
ms.openlocfilehash: 3e9f688a31d2847505e974ab6a1557aa6a7b2047
ms.sourcegitcommit: eb6bef1274b9e6390c7a77ff69bf6a3b94e827fc
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/05/2020
ms.locfileid: "87046836"
---
# <a name="synchronize-apache-spark-for-azure-synapse-external-table-definitions-in-sql-on-demand-preview"></a>Синхронизация Apache Spark для определений внешних таблиц Azure Synapse в SQL по запросу (предварительная версия)

SQL по запросу (предварительная версия) может автоматически синхронизировать метаданные из Apache Spark для пулов Azure Synapse. Для каждой базы данных, имеющейся в пулах Spark (предварительная версия), будет создана база данных SQL по запросу. 

Для каждой внешней таблицы Spark, основанной на Parquet и размещенной в службе хранилища Azure, внешняя таблица создается в базе данных SQL по запросу. Таким образом, вы можете завершить работу пулов Spark и по-прежнему запрашивать внешние таблицы Spark из SQL по запросу.

Если таблица секционирована в Spark, файлы в хранилище упорядочиваются по папкам. SQL по запросу будет использовать для запроса метаданные секции и только целевые папки и файлы.

Синхронизация метаданных автоматически настраивается для каждого пула Spark, подготовленного в рабочей области Azure Synapse. Вы можете сразу же начать выполнение запросов ко внешним таблицам Spark.

Каждая внешняя таблица Spark на основе parquet, размещенная в службе хранилища Azure, представлена внешней таблицей в схеме dbo, которая соответствует базе данных SQL по запросу. 

Для запросов к внешним таблицам Spark выполните запрос, нацеленный на внешнюю таблицу [spark_table]. Перед выполнением примера убедитесь, что у вас есть правильный [доступ к учетной записи хранения](develop-storage-files-storage-access-control.md), в которой находятся файлы.

```sql
SELECT * FROM [db].dbo.[spark_table]
```

> [!NOTE]
> Команды добавления, удаления или изменения столбца внешней таблицы Spark не повлияют на внешнюю таблицу SQL по запросу.

## <a name="apache-spark-data-types-to-sql-data-types-mapping"></a>Сопоставление типов данных Apache Spark с типами данных SQL

| Тип данных Spark | Тип данных SQL               |
| --------------- | --------------------------- |
| ByteType        | smallint                    |
| ShortType       | smallint                    |
| IntegerType     | INT                         |
| LongType        | BIGINT                      |
| FloatType       | real                        |
| DoubleType      | FLOAT                       |
| DecimalType     | Decimal                     |
| TimestampType   | datetime2                   |
| DateType        | Дата                        |
| StringType      | varchar(max)*               |
| BinaryType      | varbinary                   |
| BooleanType     | bit                         |
| ArrayType       | varchar(max)* (в JSON)** |
| MapType         | varchar(max)* (в JSON)** |
| StructType      | varchar(max)* (в JSON)** |

\* Используемые параметры сортировки — Latin1_General_100_BIN2_UTF8.

** ArrayType, MapType и StructType представлены в виде JSON.



## <a name="next-steps"></a>Дальнейшие действия

Перейдите к статье [Управление доступом к хранилищу](develop-storage-files-storage-access-control.md), чтобы узнать больше о контроле доступа к хранилищу.
