---
title: Инструкция CREATE EXTERNAL TABLE AS SELECT (CETAS) в Synapse SQL
description: Использование инструкции CREATE EXTERNAL TABLE AS SELECT (CETAS) с Synapse SQL
services: synapse-analytics
author: filippopovic
ms.service: synapse-analytics
ms.topic: overview
ms.subservice: sql
ms.date: 09/15/2020
ms.author: fipopovi
ms.reviewer: jrasnick
ms.openlocfilehash: 93f23cdcfb3fb7107e3b1838b48b3e58ccc2d028
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/25/2020
ms.locfileid: "91288772"
---
# <a name="cetas-with-synapse-sql"></a>CETAS в SQL Synapse

Вы можете использовать инструкцию CREATE EXTERNAL TABLE AS SELECT (CETAS) в пуле SQL или SQL по запросу (предварительная версия) для выполнения следующих задач:  

- Создание внешней таблицы
- Параллельный экспорт результатов инструкции Transact-SQL SELECT в:

  - Hadoop
  - Большой двоичный объект хранилища Azure
  - Azure Data Lake Storage 2-го поколения

## <a name="cetas-in-sql-pool"></a>CETAS в пуле SQL

Дополнительные сведения о пуле SQL, использовании CETAS и синтаксисе см. в статье [CREATE EXTERNAL TABLE AS SELECT](/sql/t-sql/statements/create-external-table-as-select-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest&preserve-view=true). Кроме того, инструкции по CTAS с использованием пула SQL см. в статье [CREATE TABLE AS SELECT](/sql/t-sql/statements/create-table-as-select-azure-sql-data-warehouse?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest&preserve-view=true).

## <a name="cetas-in-sql-on-demand"></a>CETAS в SQL по запросу

При использовании SQL по запросу CETAS применяется для создания внешней таблицы и экспорта результатов запроса в Azure Storage Blob или Azure Data Lake Storage 2-го поколения.

## <a name="syntax"></a>Синтаксис

```syntaxsql
CREATE EXTERNAL TABLE [ [database_name  . [ schema_name ] . ] | schema_name . ] table_name
    WITH (
        LOCATION = 'path_to_folder',  
        DATA_SOURCE = external_data_source_name,  
        FILE_FORMAT = external_file_format_name  
)
    AS <select_statement>  
[;]

<select_statement> ::=  
    [ WITH <common_table_expression> [ ,...n ] ]  
    SELECT <select_criteria>
```

## <a name="arguments"></a>Аргументы

*[ [ *database_name* . [ *schema_name* ] . ] | *schema_name* . ] *table_name**

Имя создаваемой таблицы, состоящее из одной, двух или трех частей. Для внешней таблицы SQL по запросу хранит только метаданные таблицы. Никакие данные не перемещаются и не хранятся в SQL по запросу.

LOCATION = *'path_to_folder'*

Указывает место записи результатов инструкции SELECT во внешнем источнике данных. Корневая папка — это расположение данных, указанное во внешнем источнике данных. Элемент LOCATION должен указывать на папку, и у него должна быть конечная косая черта (/). Пример: aggregated_data/ .

DATA_SOURCE = *external_data_source_name*

Задает имя объекта внешнего источника данных, которое содержит расположение, где будут храниться внешние данные. Для создания внешнего источника данных используйте инструкцию [CREATE EXTERNAL DATA SOURCE (Transact-SQL)](develop-tables-external-tables.md#create-external-data-source).

FILE_FORMAT = *external_file_format_name*

Задает имя объекта формата внешнего файла, который хранит формат внешнего файла данных. Чтобы создать формат внешнего файла, используйте [CREATE EXTERNAL FILE FORMAT (Transact-SQL)](develop-tables-external-tables.md#create-external-file-format). В настоящее время поддерживаются только форматы внешний файлов с FORMAT_TYPE=PARQUET и FORMAT_TYPE=DELIMITEDTEXT.

WITH  *<common_table_expression>*

Задается временно именованный результирующий набор, называемый обобщенным табличным выражением (ОТВ). Дополнительные сведения см. в статье [WITH common_table_expression (Transact-SQL)](/sql/t-sql/queries/with-common-table-expression-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest&preserve-view=true).

SELECT <select_criteria>

Заполняет новую таблицу результатами выполнения инструкции SELECT. *select_criteria* являются основной частью инструкции SELECT и определяют, какие данные нужно скопировать в новую таблицу. Сведения об инструкциях SELECT см. в статье [SELECT (Transact-SQL)](/sql/t-sql/queries/select-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest&preserve-view=true).

> [!NOTE]
> Предложение ORDER BY в части SELECT не поддерживается для CETAS.

## <a name="permissions"></a>Разрешения

Для работы CETAS необходимо иметь разрешения на вывод содержимого папки и запись в папку LOCATION.

## <a name="examples"></a>Примеры

В этих примерах CETAS используется для сохранения данных о всем населении, агрегированных по годам и штатам, в папку aggregated_data, расположенную в источнике данных population_ds.

Этот пример основан на учетных данных, источнике данных и формате внешнего файла, которые созданы ранее. Дополнительные сведения см. в статье о [внешних таблицах](develop-tables-external-tables.md). Чтобы сохранить результаты запроса в другую папку в том же источнике данных, измените аргумент LOCATION. 

Чтобы сохранить результаты в другую учетную запись хранения, создайте и используйте другой источник данных для аргумента DATA_SOURCE.

> [!NOTE]
> В приведенных ниже примерах используется открытая учетная запись хранения Azure Open Data. Она доступна только в режиме для чтения. Для выполнения этих запросов необходимо указать источник данных, для которого у вас есть разрешения на запись.

```sql
-- use CETAS to export select statement with OPENROWSET result to  storage
CREATE EXTERNAL TABLE population_by_year_state
WITH (
    LOCATION = 'aggregated_data/',
    DATA_SOURCE = population_ds,  
    FILE_FORMAT = census_file_format
)  
AS
SELECT decennialTime, stateName, SUM(population) AS population
FROM
    OPENROWSET(BULK 'https://azureopendatastorage.blob.core.windows.net/censusdatacontainer/release/us_population_county/year=*/*.parquet',
    FORMAT='PARQUET') AS [r]
GROUP BY decennialTime, stateName
GO

-- you can query the newly created external table
SELECT * FROM population_by_year_state
```

В следующем примере внешняя таблица используется в качестве источника для CETAS. Этот пример основан на учетных данных, источнике данных, формате внешнего файла и внешней таблице, которые созданы ранее. Дополнительные сведения см. в статье о [внешних таблицах](develop-tables-external-tables.md).

```sql
-- use CETAS with select from external table
CREATE EXTERNAL TABLE population_by_year_state
WITH (
    LOCATION = 'aggregated_data/',
    DATA_SOURCE = population_ds,  
    FILE_FORMAT = census_file_format
)  
AS
SELECT decennialTime, stateName, SUM(population) AS population
FROM census_external_table
GROUP BY decennialTime, stateName
GO

-- you can query the newly created external table
SELECT * FROM population_by_year_state
```

## <a name="supported-data-types"></a>Поддерживаемые типы данных

CETAS можно использовать для хранения результирующих наборов со следующими типами данных SQL:

- binary
- varbinary
- char
- varchar
- Дата
- time
- datetime2
- Decimal
- NUMERIC
- FLOAT
- real
- BIGINT
- INT
- smallint
- tinyint
- bit

> [!NOTE]
> LOB нельзя использовать с CETAS.

В части SELECT в CETAS нельзя использовать следующие типы данных:

- nchar
- nvarchar
- DATETIME
- smalldatetime
- datetimeoffset
- money
- smallmoney
- UNIQUEIDENTIFIER

## <a name="next-steps"></a>Дальнейшие действия

Попробуйте выполнить запросы к [внешним таблицам Apache Spark для Azure Synapse](develop-storage-files-spark-tables.md).
