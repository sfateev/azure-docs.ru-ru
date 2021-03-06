---
title: Что такое Azure Synapse Analytics?
description: Сведения об Azure Synapse Analytics
services: synapse-analytics
author: saveenr
ms.service: synapse-analytics
ms.topic: overview
ms.subservice: overview
ms.date: 09/12/2020
ms.author: saveenr
ms.reviewer: jrasnick
ms.openlocfilehash: c4338152579170bf809577262992f0db9a1a95ff
ms.sourcegitcommit: 03662d76a816e98cfc85462cbe9705f6890ed638
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/15/2020
ms.locfileid: "90524952"
---
# <a name="what-is-azure-synapse-analytics-workspaces-preview"></a>Что такое Azure Synapse Analytics (предварительная версия рабочих областей)?

[!INCLUDE [preview](includes/note-preview.md)]

Для аналитики корпоративного класса необходима обработка больших объемов данных любого типа, будь то необработанные, уточненные или тщательно проверенные данные. Это необходимо компаниям для объединения больших данных и технологий их хранения, таких как Spark и SQL, в многофункциональные конвейеры данных, работающие с данными в реляционных хранилищах и озерах данных. Подобные решения трудно создавать, защищать и поддерживать. Сложность приводит к тому, что аналитические данные, необходимые предприятиям, поступают с задержкой.

**Azure Synapse** — это интегрированная служба аналитики, которая ускоряет извлечение аналитических сведений в разных хранилищах данных и системах анализа больших данных. По сути, служба сочетает в себе лучшие **технологии SQL**, используемые в корпоративных хранилищах данных, **технологии Spark**, используемые при работе с большими данными, а также **конвейеры** для интеграции данных и их извлечения, преобразования и загрузки. В Synapse есть **веб-студия**, которая предоставляет единое пространство для управления, мониторинга, написания кода и обеспечения безопасности. Synapse отличается глубокой интеграцией с другими службами Azure, такими как **PowerBI**, **CosmosDB** и **AzureML**.

## <a name="key-features--benefits"></a>Ключевые возможности и преимущества

### <a name="industry-leading-sql"></a>Ведущая в отрасли система SQL

* **Synapse SQL** — это система распределенных запросов, которая позволяет предприятиям реализовать сценарии хранения и виртуализации данных с помощью стандартных возможностей T-SQL, знакомых специалистам по работе с данными. Кроме того, решение расширяет возможности SQL для использования в сценариях потоковой передачи данных и машинного обучения.

* Synapse SQL предлагает как **бессерверные** модели, так и модели **выделенных ресурсов**, предоставляя вам возможность выбрать варианты использования и выставления счетов в соответствии с потребностями. Для прогнозируемой производительности и затрат можно создавать выделенные пулы SQL, чтобы резервировать вычислительные мощности для данных, хранящихся в таблицах SQL. Для незапланированных или пакетных рабочих нагрузок используйте бессерверную конечную точку SQL, которая всегда доступна.
* Используйте встроенные возможности **потоковой передачи** для передачи данных из облачных источников данных в таблицы SQL.
* Объедините возможности искусственного интеллекта с SQL с помощью моделей **машинного обучения** для оценки данных с использованием [функции PREDICT T-SQL](https://docs.microsoft.com/sql/t-sql/queries/predict-transact-sql?view=azure-sqldw-latest).

### <a name="industry-standard-apache-spark"></a>Подсистема Apache Spark, являющаяся отраслевым стандартом

Решение **Apache Spark для Azure Synapse** плотно интегрировано с Apache Spark — самой популярной подсистемой обработки больших данных с открытым кодом. Она обеспечивает подготовку, инжиниринг, извлечение, преобразование и загрузку данных, а также машинное обучение.

* Модели машинного обучения с алгоритмами SparkML и интеграцией Azure ML для Apache Spark 2.4 со встроенной поддержкой Linux Foundation Delta Lake.
* Упрощенная модель ресурсов, которая освобождает вас от необходимости заниматься управлением кластерами.
* Быстрый запуск подсистемы Spark и быстрое автомасштабирование.
* Встроенная поддержка .NET для Spark, позволяющая использовать опыт работы с языком C# и существующий код .NET в приложении Spark.

### <a name="interop-of-sql-and-apache-spark-on-your-data-lake"></a>Межпрограммное взаимодействие SQL и Apache Spark в Data Lake

Azure Synapse устраняет традиционные технологические барьеры, препятствующие совместному использованию SQL и Spark. Вы можете легко сочетать их в соответствии со своими потребностями и опытом.

* Общая система метаданных, совместимая с Hive, позволяет легко использовать в Spark или Hive таблицы, определенные для файлов в озере данных.
* SQL и Spark могут напрямую изучать и анализировать файлы Parquet, CSV, TSV и JSON, хранящиеся в озере данных.
* Быстро масштабируемая загрузка и выгрузка данных, передаваемых между базами данных SQL и Spark.

### <a name="built-in-data-integration-via-pipelines"></a>Встроенная интеграция данных за счет конвейеров

Azure Synapse поставляется с тем же механизмом интеграции данных и процессом взаимодействия с пользователем, что и Фабрика данных Azure. Это позволяет создавать многофункциональные конвейеры для извлечения, преобразования и загрузки данных в масштабе в самой этой службе.

* Прием данных из более чем 90 источников.
* Извлечение, преобразование и загрузка без кода с помощью действий потока данных.
* Управление Записными книжками, заданиями Spark, хранимыми процедурами, скриптами SQL и т. д.

### <a name="unified-management-monitoring-and-security"></a>Унифицированное управление, мониторинг и обеспечение безопасности

Azure Synapse предоставляет предприятиям единый способ управления ресурсами аналитики, мониторинга использования и активности, а также обеспечения безопасности:

* Назначение пользователям ролей для упрощения доступа к ресурсам аналитики.
* Детальное управление доступом к данным и коду.
* Единая панель мониторинга, предназначенная для мониторинга ресурсов, использования и пользователей в SQL и Spark.

### <a name="synapse-studio"></a>Synapse Studio

**Synapse Studio** — это веб-интерфейс, объединяющий все функции для инженеров по работе с данными. С его помощью можно выполнить все задачи, необходимые для создания полного решения:

* Создание комплексного решения для анализа в одном расположении, включающее прием, просмотр, подготовку, оркестрацию и визуализацию данных.
* Самые высокие в отрасли показатели продуктивности для специалистов по работе с данными при написании кода SQL или Spark: создание, отладка и оптимизация производительности.
* Интеграция с корпоративными процессами CI/CD.

## <a name="next-steps"></a>Дальнейшие действия

* [Начало работы с Azure Synapse Analytics](get-started.md)
* [Создание рабочей области](quickstart-create-workspace.md)
* [Использование службы SQL по запросу](quickstart-sql-on-demand.md)
