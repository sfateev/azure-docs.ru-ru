---
title: Управление местом в файле базы данных SQL Azure
description: Здесь описывается управление файловым пространством отдельной базы данных или базы данных в пуле Базы данных SQL Azure и приведены примеры кода для определения необходимости сжатия отдельной базы данных или базы данных в составе пула и выполнения операции сжатия.
services: sql-database
ms.service: sql-database
ms.subservice: operations
ms.custom: sqldbrb=1
ms.devlang: ''
ms.topic: conceptual
author: oslake
ms.author: moslake
ms.reviewer: jrasnick, sstein
ms.date: 03/12/2019
ms.openlocfilehash: 2e751a77d40403c7bdd4644e8e6fb03ff89063e8
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2020
ms.locfileid: "91335077"
---
# <a name="manage-file-space-for-databases-in-azure-sql-database"></a>Управление пространством для баз данных в базе данных SQL Azure
[!INCLUDE[appliesto-sqldb](../includes/appliesto-sqldb.md)]

В этой статье описываются различные типы дискового пространства для баз данных в базе данных SQL Azure, а также действия, которые могут быть выполнены, если выделенное место на диске должно быть явно управляемым.

> [!NOTE]
> Эта статья не относится к управляемому экземпляру базы данных SQL Azure.

## <a name="overview"></a>Обзор

В базе данных SQL Azure существуют шаблоны рабочей нагрузки, в которых распределение базовых файлов данных для баз данных может стать больше, чем количество используемых страниц данных. Это может произойти, когда используемое пространство увеличивается, а данные затем удаляются. Это объясняется тем, что выделенное файловое пространство автоматически не освобождается.

Мониторинг использования файлового пространства и сжатие файлов базы данных может потребоваться в следующих сценариях:

- предоставление места для роста данных в эластичном пуле, когда файловое пространство, выделенное для баз данных, достигает максимального размера пула;
- уменьшение максимального размера отдельной базы данных или эластичного пула;
- изменение уровня служб или производительности отдельной базы данных или эластичного пула с меньшим максимальным размером.

### <a name="monitoring-file-space-usage"></a>Мониторинг использования файлового пространства

Большинство метрик дискового пространства, отображаемых на портале Azure, и следующие API-интерфейсы измеряют только размер используемых страниц данных:

- API метрик на основе Azure Resource Manager, включая PowerShell [get-metrics](https://docs.microsoft.com/powershell/module/az.monitor/get-azmetric).
- T-SQL: [sys.dm_db_resource_stats](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-db-resource-stats-azure-sql-database).

Тем не менее следующие API-интерфейсы также измеряют размер пространства, выделяемого для баз данных и эластичных пулов:

- T-SQL: [sys.resource_stats](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-resource-stats-azure-sql-database).
- T-SQL: [sys.elastic_pool_resource_stats](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-elastic-pool-resource-stats-azure-sql-database).

### <a name="shrinking-data-files"></a>Сжатие файлов данных

База данных SQL Azure не сжимает файлы данных автоматически для освобождения неиспользуемого выделенного пространства из-за потенциального влияния на производительность базы данных.  Тем не менее клиенты могут сжимать файлы данных через самостоятельный выбор, выполнив действия, описанные в статье об [освобождении неиспользуемого выделенного пространства](#reclaim-unused-allocated-space).

> [!NOTE]
> В отличие от файлов данных, база данных SQL Azure автоматически сжимает файлы журналов, так как эта операция не влияет на производительность базы данных.

## <a name="understanding-types-of-storage-space-for-a-database"></a>Основные сведения о типах дискового пространства для базы данных

Для управления файловым пространством базы данных важно разобраться со следующими объемами дискового пространства.

|Объем пространства базы данных|Определение|Комментарии|
|---|---|---|
|**Используемое пространство данных**|Объем пространства, используемого для хранения данных базы данных, в страницах по 8 КБ.|Как правило, используемое пространство увеличивается (уменьшается) при операциях вставки (удаления). В некоторых случаях используемое пространство остается неизменным при операциях вставки или удаления в зависимости от объема и шаблона данных, участвующих в операции и фрагментации. Например, удаление одной строки на каждой странице данных не обязательно приведет к уменьшению используемого пространства.|
|**Выделенное пространство данных**|Объем форматированного файлового пространства, который стал доступным для хранения данных базы данных.|Объем выделенного пространства увеличивается автоматически, но никогда не уменьшается после удалений. Такое поведение гарантирует, что будущие вставки выполняются быстрее, так как пространство не нуждается в переформатировании.|
|**Выделенное, но неиспользуемое пространство данных**|Разница между объемом выделенного и используемого пространства данных.|Это количество представляет максимальный объем свободного пространства, которое можно освободить путем сжатия файлов данных базы данных.|
|**Максимальный размер данных**|Максимальный объем пространства, который можно использовать для хранения данных базы данных.|Объем выделенного пространства данных не может превышать максимальный размер данных.|

На следующей схеме показана связь между разными типами дискового пространства для базы данных.

![типы дискового пространства и их связи](./media/file-space-manage/storage-types.png)

## <a name="query-a-single-database-for-storage-space-information"></a>Запрос отдельной базы данных для получения сведений о дисковом пространстве

Следующие запросы можно использовать для определения объема дискового пространства для отдельной базы данных.  

### <a name="database-data-space-used"></a>Место, занятое данными базы данных

Измените следующий запрос, чтобы получить объем места, занятого данными базы данных.  Единицы результатов запроса указываются в МБ.

```sql
-- Connect to master
-- Database data space used in MB
SELECT TOP 1 storage_in_megabytes AS DatabaseDataSpaceUsedInMB
FROM sys.resource_stats
WHERE database_name = 'db1'
ORDER BY end_time DESC
```

### <a name="database-data-space-allocated-and-unused-allocated-space"></a>Выделенное пространство данных базы данных и неиспользуемое выделенное пространство

Используйте следующий запрос, чтобы получить объем выделенного пространства данных базы данных и неиспользуемого выделенного пространства.  Единицы результатов запроса указываются в МБ.

```sql
-- Connect to database
-- Database data space allocated in MB and database data space allocated unused in MB
SELECT SUM(size/128.0) AS DatabaseDataSpaceAllocatedInMB,
SUM(size/128.0 - CAST(FILEPROPERTY(name, 'SpaceUsed') AS int)/128.0) AS DatabaseDataSpaceAllocatedUnusedInMB
FROM sys.database_files
GROUP BY type_desc
HAVING type_desc = 'ROWS'
```

### <a name="database-data-max-size"></a>Максимальный размер данных базы данных

Измените следующий запрос, чтобы получить максимальный размер данных базы данных.  Единицы результатов запроса указываются в байтах.

```sql
-- Connect to database
-- Database data max size in bytes
SELECT DATABASEPROPERTYEX('db1', 'MaxSizeInBytes') AS DatabaseDataMaxSizeInBytes
```

## <a name="understanding-types-of-storage-space-for-an-elastic-pool"></a>Основные сведения о типах дискового пространства для эластичного пула

Для управления файловым пространством эластичного пула важно разобраться со следующими объемами дискового пространства.

|Объем пространства эластичных пулов|Определение|Комментарии|
|---|---|---|
|**Используемое пространство данных**|Общий объем пространства данных, используемого всеми базами данных в эластичном пуле.||
|**Выделенное пространство данных**|Общий объем пространства данных, выделенного всеми базами данных в эластичном пуле.||
|**Выделенное, но неиспользуемое пространство данных**|Разница между объемом выделенного пространства данных и пространства данных, используемого всеми базами данных в эластичном пуле.|Это количество представляет максимальный объем выделенного для эластичного пула пространства, которое можно освободить путем сжатия файлов данных базы данных.|
|**Максимальный размер данных**|Максимальный объем пространства данных, который эластичный пул может использовать для всех своих баз данных.|Выделенное для эластичного пула пространство не должно превышать максимальный размер эластичного пула.  В этом случае выделенное неиспользуемое пространство можно освободить путем сжатия файлов данных базы данных.|

## <a name="query-an-elastic-pool-for-storage-space-information"></a>Запрос эластичного пула для получения сведений о дисковом пространстве

Следующие запросы можно использовать для определения объема дискового пространства для эластичного пула.  

### <a name="elastic-pool-data-space-used"></a>Используемое пространство данных эластичного пула

Измените следующий запрос, чтобы получить объем пространства, занятого данными эластичного пула.  Единицы результатов запроса указываются в МБ.

```sql
-- Connect to master
-- Elastic pool data space used in MB  
SELECT TOP 1 avg_storage_percent / 100.0 * elastic_pool_storage_limit_mb AS ElasticPoolDataSpaceUsedInMB
FROM sys.elastic_pool_resource_stats
WHERE elastic_pool_name = 'ep1'
ORDER BY end_time DESC
```

### <a name="elastic-pool-data-space-allocated-and-unused-allocated-space"></a>Выделенное пространство данных эластичного пула и неиспользуемое выделенное пространство

Измените следующие примеры, чтобы получить таблицу с перечнем выделенного и неиспользуемого пространства для каждой базы данных в эластичном пуле. Таблица сортирует базы данных по объему неиспользованного выделенного пространства от наибольших к наименьшим.  Единицы результатов запроса указываются в МБ.  

Результаты запроса для определения выделенного пространства для каждой базы данных в пуле могут суммироваться для представления общего пространства, выделенного для эластичного пула. Выделенное пространство эластичного пула не должно превышать максимальный размер эластичного пула.  

> [!IMPORTANT]
> Модуль PowerShell Azure Resource Manager по-прежнему поддерживается базой данных SQL Azure, но вся будущая разработка сосредоточена на модуле Az.Sql. Исправления ошибок для модуля AzureRM будут продолжать выпускаться как минимум до декабря 2020 г. Аргументы команд в модулях Az и AzureRm практически идентичны. Дополнительные сведения о совместимости см. в статье [Знакомство с новым модулем Az для Azure PowerShell](/powershell/azure/new-azureps-module-az).

Сценарию PowerShell требуется модуль SQL Server PowerShell. Дополнительные сведения см. в разделе [Установка модуля SQL Server PowerShell](https://docs.microsoft.com/sql/powershell/download-sql-server-ps-module).

```powershell
$resourceGroupName = "<resourceGroupName>"
$serverName = "<serverName>"
$poolName = "<poolName>"
$userName = "<userName>"
$password = "<password>"

# get list of databases in elastic pool
$databasesInPool = Get-AzSqlElasticPoolDatabase -ResourceGroupName $resourceGroupName `
    -ServerName $serverName -ElasticPoolName $poolName
$databaseStorageMetrics = @()

# for each database in the elastic pool, get space allocated in MB and space allocated unused in MB
foreach ($database in $databasesInPool) {
    $sqlCommand = "SELECT DB_NAME() as DatabaseName, `
    SUM(size/128.0) AS DatabaseDataSpaceAllocatedInMB, `
    SUM(size/128.0 - CAST(FILEPROPERTY(name, 'SpaceUsed') AS int)/128.0) AS DatabaseDataSpaceAllocatedUnusedInMB `
    FROM sys.database_files `
    GROUP BY type_desc `
    HAVING type_desc = 'ROWS'"
    $serverFqdn = "tcp:" + $serverName + ".database.windows.net,1433"
    $databaseStorageMetrics = $databaseStorageMetrics + 
        (Invoke-Sqlcmd -ServerInstance $serverFqdn -Database $database.DatabaseName `
            -Username $userName -Password $password -Query $sqlCommand)
}

# display databases in descending order of space allocated unused
Write-Output "`n" "ElasticPoolName: $poolName"
Write-Output $databaseStorageMetrics | Sort -Property DatabaseDataSpaceAllocatedUnusedInMB -Descending | Format-Table
```

В качестве примера выходных данных скрипта приведен следующий снимок экрана:

![пример выделенного и неиспользуемого пространства эластичного пула](./media/file-space-manage/elastic-pool-allocated-unused.png)

### <a name="elastic-pool-data-max-size"></a>Максимальный размер данных эластичного пула

Измените следующий запрос T-SQL, чтобы получить максимальный размер данных эластичного пула.  Единицы результатов запроса указываются в МБ.

```sql
-- Connect to master
-- Elastic pools max size in MB
SELECT TOP 1 elastic_pool_storage_limit_mb AS ElasticPoolMaxSizeInMB
FROM sys.elastic_pool_resource_stats
WHERE elastic_pool_name = 'ep1'
ORDER BY end_time DESC
```

## <a name="reclaim-unused-allocated-space"></a>Освобождение неиспользуемого выделенного пространства

> [!NOTE]
> Эта команда может влиять на производительность базы данных во время выполнения, поэтому по возможности ее следует выполнять в периоды низкого уровня использования.

### <a name="dbcc-shrink"></a>Сжатие DBCC

Определив, в каких базах данных необходимо освободить неиспользуемое выделенное пространство, измените имя базы данных в следующей команде, чтобы сжать файлы для каждой базы данных.

```sql
-- Shrink database data space allocated.
DBCC SHRINKDATABASE (N'db1')
```

Эта команда может влиять на производительность базы данных во время выполнения, поэтому по возможности ее следует выполнять в периоды низкого уровня использования.  

Дополнительные сведения об этой команде см. в статье [DBCC SHRINKDATABASE (Transact-SQL)](https://docs.microsoft.com/sql/t-sql/database-console-commands/dbcc-shrinkdatabase-transact-sql).

### <a name="auto-shrink"></a>Автоматическое сжатие

Кроме того, для базы данных можно включить автоматическое сжатие.  Автоматическое сжатие упрощает управления файлами и оказывает меньшее влияние на производительность базы данных, чем `SHRINKDATABASE` или `SHRINKFILE`.  Автоматическое сжатие может быть особенно полезным для управления эластичными пулами с множеством баз данных.  Однако автоматическое сжатие может быть менее эффективным при восстановлении файлового пространства, чем операции `SHRINKDATABASE` и `SHRINKFILE`.
Чтобы включить автоматическое сжатие, измените имя базы данных в следующей команде.

```sql
-- Enable auto-shrink for the database.
ALTER DATABASE [db1] SET AUTO_SHRINK ON
```

Дополнительные сведения об этой команде см. в статье [Параметры ALTER DATABASE SET (Transact-SQL)](https://docs.microsoft.com/sql/t-sql/statements/alter-database-transact-sql-set-options?view=azuresqldb-current).

### <a name="rebuild-indexes"></a>Перестроение индексов

После сжатия файлов базы данных индексы могут стать фрагментированными, а эффективность оптимизации их производительности может ухудшиться. Если происходит замедление, попробуйте перестроить индексы базы данных. Дополнительные сведения о фрагментации и перестроении индексов см. в статье [Реорганизация и перестроение индексов](https://docs.microsoft.com/sql/relational-databases/indexes/reorganize-and-rebuild-indexes).

## <a name="next-steps"></a>Дальнейшие шаги

- Сведения о максимальных размерах базы данных см. в статьях:
  - [Ограничения ресурсов для отдельной базы данных в Базе данных SQL Azure при использовании модели приобретения на основе виртуальных ядер](resource-limits-vcore-single-databases.md)
  - [Ограничения ресурсов для одиночных баз данных в модели приобретения на основе единиц DTU](resource-limits-dtu-single-databases.md)
  - [Ограничения для эластичных пулов в службе "База данных SQL Azure" в рамках модели приобретения на основе виртуальных ядер](resource-limits-vcore-elastic-pools.md)
  - [Ограничения ресурсов для эластичных пулов в модели приобретения на основе единиц DTU](resource-limits-dtu-elastic-pools.md)
- Дополнительные сведения о команде `SHRINKDATABASE` см. в статье [DBCC SHRINKDATABASE (Transact-SQL)](/sql/t-sql/database-console-commands/dbcc-shrinkdatabase-transact-sql).
- Дополнительные сведения о фрагментации и перестроении индексов см. в статье [Реорганизация и перестроение индексов](/sql/relational-databases/indexes/reorganize-and-rebuild-indexes).
