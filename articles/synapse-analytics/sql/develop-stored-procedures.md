---
title: Использование хранимых процедур
description: Советы по реализации хранимых процедур в синапсе SQL для разработки решений.
services: synapse-analytics
author: XiaoyuMSFT
manager: craigg
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: sql
ms.date: 09/23/2020
ms.author: xiaoyul
ms.reviewer: igorstan
ms.openlocfilehash: f2046614f3665a699d02c76210676fb32f99fc73
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2020
ms.locfileid: "91288925"
---
# <a name="use-stored-procedures-in-synapse-sql"></a>Использование хранимых процедур в синапсе SQL

Советы по реализации хранимых процедур в пуле синапсе SQL (хранилище данных) для разработки решений.

## <a name="what-to-expect"></a>Чего следует ожидать

Синапсе SQL поддерживает многие функции T-SQL, используемые в SQL Server. Что более важно, в нем есть специальные функции горизонтального масштабирования, с помощью которых можно максимально увеличить производительность решения.

Для поддержания масштабируемости и производительности пула SQL существуют некоторые функции и функции, которые имеют различия в поведении и другие, которые не поддерживаются.

## <a name="stored-procedures-in-synapse-sql"></a>Хранимые процедуры в синапсе SQL

Хранимые процедуры — это отличный способ инкапсуляции кода SQL и сохранения его близкого к данным в хранилище данных. Хранимые процедуры помогают разработчикам разделения свои решения путем инкапсуляции кода в управляемые единицы, облегчая более многократное использование кода. Каждая хранимая процедура может также принимать параметры, что делает их еще более гибкими. В следующем примере можно увидеть процедуры, которые удаляют внешние объекты, если они существуют в базе данных:

```sql
CREATE PROCEDURE drop_external_table_if_exists @name SYSNAME
AS BEGIN
    IF (0 <> (SELECT COUNT(*) FROM sys.external_tables WHERE name = @name))
    BEGIN
        DECLARE @drop_stmt NVARCHAR(200) = N'DROP EXTERNAL TABLE ' + @name; 
        EXEC sp_executesql @tsql = @drop_stmt;
    END
END
GO
CREATE PROCEDURE drop_external_file_format_if_exists @name SYSNAME
AS BEGIN
    IF (0 <> (SELECT COUNT(*) FROM sys.external_file_formats WHERE name = @name))
    BEGIN
        DECLARE @drop_stmt NVARCHAR(200) = N'DROP EXTERNAL FILE FORMAT ' + @name; 
        EXEC sp_executesql @tsql = @drop_stmt;
    END
END
GO
CREATE PROCEDURE drop_external_data_source_if_exists @name SYSNAME
AS BEGIN
    IF (0 <> (SELECT COUNT(*) FROM sys.external_data_sources WHERE name = @name))
    BEGIN
        DECLARE @drop_stmt NVARCHAR(200) = N'DROP EXTERNAL DATA SOURCE ' + @name; 
        EXEC sp_executesql @tsql = @drop_stmt;
    END
END
```

Эти процедуры можно выполнить с помощью `EXEC` инструкции, где можно указать имя и параметры процедуры:

```sql
EXEC drop_external_table_if_exists 'mytest';
EXEC drop_external_file_format_if_exists 'mytest';
EXEC drop_external_data_source_if_exists 'mytest';
```

Синапсе SQL предоставляет упрощенную и упрощенную реализацию хранимых процедур. Главное отличие от SQL Server — хранимая процедура не является предварительно скомпилированным кодом. В хранилищах данных время компиляции мало по сравнению со временем, которое требуется для выполнения запросов к большим объемам данных. Более важно убедиться, что код хранимой процедуры правильно оптимизирован для больших запросов. Цель — сэкономить часы, минуты и секунды, но не миллисекунды. Поэтому удобнее считать хранимые процедуры контейнерами для логики SQL.

Когда синапсе SQL выполняет хранимую процедуру, инструкции SQL анализируются, транслируются и оптимизируются во время выполнения. Во время этого процесса каждая инструкция преобразуется в распределенные запросы. Код SQL, который выполняется с данными, отличается от отправляемого запроса.

## <a name="encapsulate-validation-rules"></a>Инкапсуляция правил проверки

Хранимые процедуры позволяют нахождение логики проверки в одном модуле, хранящемся в базе данных SQL. В следующем примере можно увидеть, как проверить значения параметров и изменить их значения по умолчанию.

```sql
CREATE PROCEDURE count_objects_by_date_created 
                            @start_date DATETIME2,
                            @end_date DATETIME2
AS BEGIN 

    IF( @start_date >= GETUTCDATE() )
    BEGIN
        THROW 51000, 'Invalid argument @start_date. Value should be in past.', 1;  
    END

    IF( @end_date IS NULL )
    BEGIN
        SET @end_date = GETUTCDATE();
    END

    IF( @start_date >= @end_date )
    BEGIN
        THROW 51000, 'Invalid argument @end_date. Value should be greater than @start_date.', 2;  
    END

    SELECT
         year = YEAR(create_date),
         month = MONTH(create_date),
         objects_created = COUNT(*)
    FROM
        sys.objects
    WHERE
        create_date BETWEEN @start_date AND @end_date
    GROUP BY
        YEAR(create_date), MONTH(create_date);
END
```

Логика в процедуре SQL проверит входные параметры при вызове процедуры.

```sql

EXEC count_objects_by_date_created '2020-08-01', '2020-09-01'

EXEC count_objects_by_date_created '2020-08-01', NULL

EXEC count_objects_by_date_created '2020-09-01', '2020-08-01'
-- Error
-- Invalid argument @end_date. Value should be greater than @start_date.

EXEC count_objects_by_date_created '2120-09-01', NULL
-- Error
-- Invalid argument @start_date. Value should be in past.
```

## <a name="nesting-stored-procedures"></a>Вложенные хранимые процедуры

Когда хранимые процедуры вызывают другие хранимые процедуры или выполняют динамический код SQL, то внутреннюю хранимую процедуру или код вызова называют вложенными.
Пример вложенной процедуры показан в следующем коде:

```sql
CREATE PROCEDURE clean_up @name SYSNAME
AS BEGIN
    EXEC drop_external_table_if_exists @name;
    EXEC drop_external_file_format_if_exists @name;
    EXEC drop_external_data_source_if_exists @name;
END
```

Эта процедура принимает параметр, представляющий некоторое имя, а затем вызывает другие процедуры для удаления объектов с таким именем.
Пул SQL синапсе поддерживает не более восьми уровней вложенности. Эта возможность немного отличается от SQL Server. Уровень вложенности в SQL Server — 32.

Вызов хранимой процедуры верхнего уровня считается 1 уровнем вложенности.

```sql
EXEC clean_up 'mytest'
```

Если хранимая процедура также выполняет еще один вызов EXEC, уровень вложенности увеличится до 2.

```sql
CREATE PROCEDURE clean_up @name SYSNAME
AS
    EXEC drop_external_table_if_exists @name  -- This call is nest level 2
GO
EXEC clean_up 'mytest'  -- This call is nest level 1
```

Если вторая процедура затем выполняет какой-либо динамический SQL-запрос, уровень вложенности увеличится до 3.

```sql
CREATE PROCEDURE drop_external_table_if_exists @name SYSNAME
AS BEGIN
    /* See full code in the previous example */
    EXEC sp_executesql @tsql = @drop_stmt;  -- This call is nest level 3
END
GO
CREATE PROCEDURE clean_up @name SYSNAME
AS
    EXEC drop_external_table_if_exists @name  -- This call is nest level 2
GO
EXEC clean_up 'mytest'  -- This call is nest level 1
```

> [!NOTE]
> Синапсе SQL в настоящее время не [поддерживает @NESTLEVEL @](/sql/t-sql/functions/nestlevel-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest&preserve-view=true). Необходимо отслеживать уровень вложенности. Маловероятно, что вы превысите восемь уровней вложенности, но если вы это сделаете, вам нужно переписать свой код, чтобы он соответствовал уровням вложенности в этих пределах.

## <a name="insertexecute"></a>INSERT..EXECUTE

Синапсе SQL не позволяет использовать результирующий набор хранимой процедуры с инструкцией INSERT. Существует альтернативный подход, который можно использовать. Например, см. статью о [временных таблицах](develop-tables-temporary.md).

## <a name="limitations"></a>Ограничения

Существуют некоторые аспекты хранимых процедур Transact-SQL, которые не реализованы в синапсе SQL, например:

* временные хранимые процедуры;
* нумерованные хранимые процедуры;
* расширенные хранимые процедуры;
* хранимые процедуры CLR;
* возможность шифрования;
* возможность репликации;
* параметры с табличным значением;
* параметры только для чтения;
* параметры по умолчанию (в подготовленном пуле)
* контекст выполнения;
* Оператор return

## <a name="next-steps"></a>Дальнейшие действия

Дополнительные советы по разработке приведены в [обзоре разработки](develop-overview.md).
