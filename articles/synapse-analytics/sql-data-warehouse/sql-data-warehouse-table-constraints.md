---
title: Первичные, внешние и уникальные ключи
description: Поддержка табличных ограничений в пуле SQL синапсе в Azure синапсе Analytics
services: synapse-analytics
author: XiaoyuMSFT
manager: craigg
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: sql-dw
ms.date: 09/05/2019
ms.author: xiaoyul
ms.reviewer: nibruno; jrasnick
ms.custom: seo-lt-2019, azure-synapse
ms.openlocfilehash: 562e2cce317d8774ecf72971d53be4f66f9c3da4
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2020
ms.locfileid: "85212774"
---
# <a name="primary-key-foreign-key-and-unique-key-in-synapse-sql-pool"></a>Первичный ключ, внешний ключ и уникальный ключ в пуле SQL синапсе

Сведения об ограничениях таблиц в пуле синапсе SQL, включая первичный ключ, внешний ключ и уникальный ключ.

## <a name="table-constraints"></a>Ограничения таблиц

Пул SQL синапсе поддерживает следующие ограничения таблицы: 
- ПЕРВИЧный ключ поддерживается только в том случае, если используются некластеризованные и непринудительные.    
- Ограничение UNIQUE поддерживается только при использовании не применяется.

Для синтаксиса установите флажок [ALTER TABLE](https://docs.microsoft.com/sql/t-sql/statements/alter-table-transact-sql) и [CREATE TABLE](https://docs.microsoft.com/sql/t-sql/statements/create-table-azure-sql-data-warehouse). 

Ограничение внешнего ключа не поддерживается в пуле SQL синапсе.  


## <a name="remarks"></a>Комментарии

Наличие первичного ключа и (или) уникального ключа позволяет синапсе подсистеме пула SQL создать оптимальный план выполнения для запроса.  Все значения в первичном ключевом столбце или столбце уникального ограничения должны быть уникальными.

После создания таблицы с ограничением PRIMARY KEY или UNIQUE в пуле синапсе SQL пользователи должны убедиться, что все значения в этих столбцах уникальны.  Нарушение этого запроса может привести к возврату неточного результата.  В этом примере показано, как запрос может вернуть неточный результат, если столбец первичного ключа или уникального ограничения содержит повторяющиеся значения.  

```sql
 -- Create table t1
CREATE TABLE t1 (a1 INT NOT NULL, b1 INT) WITH (DISTRIBUTION = ROUND_ROBIN)

-- Insert values to table t1 with duplicate values in column a1.
INSERT INTO t1 VALUES (1, 100)
INSERT INTO t1 VALUES (1, 1000)
INSERT INTO t1 VALUES (2, 200)
INSERT INTO t1 VALUES (3, 300)
INSERT INTO t1 VALUES (4, 400)

-- Run this query.  No primary key or unique constraint.  4 rows returned. Correct result.
SELECT a1, COUNT(*) AS total FROM t1 GROUP BY a1

/*
a1          total
----------- -----------
1           2
2           1
3           1
4           1

(4 rows affected)
*/

-- Add unique constraint
ALTER TABLE t1 ADD CONSTRAINT unique_t1_a1 unique (a1) NOT ENFORCED

-- Re-run this query.  5 rows returned.  Incorrect result.
SELECT a1, count(*) AS total FROM t1 GROUP BY a1

/*
a1          total
----------- -----------
2           1
4           1
1           1
3           1
1           1

(5 rows affected)
*/

-- Drop unique constraint.
ALTER TABLE t1 DROP CONSTRAINT unique_t1_a1

-- Add primary key constraint
ALTER TABLE t1 add CONSTRAINT PK_t1_a1 PRIMARY KEY NONCLUSTERED (a1) NOT ENFORCED

-- Re-run this query.  5 rows returned.  Incorrect result.
SELECT a1, COUNT(*) AS total FROM t1 GROUP BY a1

/*
a1          total
----------- -----------
2           1
4           1
1           1
3           1
1           1

(5 rows affected)
*/

-- Manually fix the duplicate values in a1
UPDATE t1 SET a1 = 0 WHERE b1 = 1000

-- Verify no duplicate values in column a1 
SELECT * FROM t1

/*
a1          b1
----------- -----------
2           200
3           300
4           400
0           1000
1           100

(5 rows affected)
*/

-- Add unique constraint
ALTER TABLE t1 add CONSTRAINT unique_t1_a1 UNIQUE (a1) NOT ENFORCED  

-- Re-run this query.  5 rows returned.  Correct result.
SELECT a1, COUNT(*) as total FROM t1 GROUP BY a1

/*
a1          total
----------- -----------
2           1
3           1
4           1
0           1
1           1

(5 rows affected)
*/

-- Drop unique constraint.
ALTER TABLE t1 DROP CONSTRAINT unique_t1_a1

-- Add primary key contraint
ALTER TABLE t1 ADD CONSTRAINT PK_t1_a1 PRIMARY KEY NONCLUSTERED (a1) NOT ENFORCED

-- Re-run this query.  5 rows returned.  Correct result.
SELECT a1, COUNT(*) AS total FROM t1 GROUP BY a1

/*
a1          total
----------- -----------
2           1
3           1
4           1
0           1
1           1

(5 rows affected)
*/

```

## <a name="examples"></a>Примеры

Создайте таблицу пула SQL синапсе с первичным ключом: 

```sql 
CREATE TABLE mytable (c1 INT PRIMARY KEY NONCLUSTERED NOT ENFORCED, c2 INT);
```
Создайте таблицу пула SQL синапсе с ограничением UNIQUE:

```sql
CREATE TABLE t6 (c1 INT UNIQUE NOT ENFORCED, c2 INT);
```

## <a name="next-steps"></a>Дальнейшие действия

Следующим шагом после создания таблиц для пула SQL синапсе является загрузка данных в таблицу. Руководство по загрузке см. [в разделе Загрузка данных в пул SQL синапсе](load-data-wideworldimportersdw.md).
