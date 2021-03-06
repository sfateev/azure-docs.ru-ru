---
title: Сравнение вариантов хранения для использования с кластерами Azure HDInsight
description: Предоставляет обзор типов хранилища и того, как они работают с Azure HDInsight.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.custom: seoapr2020
ms.date: 04/21/2020
ms.openlocfilehash: a866a225da87c22a3a276a5d59b8e86f1f955cae
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2020
ms.locfileid: "91856200"
---
# <a name="compare-storage-options-for-use-with-azure-hdinsight-clusters"></a>Сравнение вариантов хранения для использования с кластерами Azure HDInsight

При создании кластеров HDInsight можно выбрать одну из нескольких разных служб хранилища Azure:

* [Хранилище BLOB-объектов Azure с HDInsight](./overview-azure-storage.md)
* [Azure Data Lake Storage 2-го поколения с помощью HDInsight](./overview-data-lake-storage-gen2.md)
* [Azure Data Lake Storage 1-го поколения с помощью HDInsight](./overview-data-lake-storage-gen1.md)

В этой статье предоставлен обзор типов хранилища и их уникальных функций.

## <a name="storage-types-and-features"></a>Типы и функции хранения

В следующей таблице перечислены службы хранилища Azure, которые поддерживаются в различных версиях HDInsight.

| Служба хранилища | Тип учетной записи | Тип пространства имен | Поддерживаемые службы | Поддерживаемые уровни производительности | Поддерживаемые уровни доступа | Версия HDInsight | Тип кластера |
|---|---|---|---|---|---|---|---|
|Azure Data Lake Storage 2-го поколения| Общего назначения версии 2 | Иерархическая (файловая система) | BLOB-объект | Standard | Горячий, холодный или архивный | 3.6+ | Все, кроме Spark 2,1 и 2,2|
|Хранилище Azure| Общего назначения версии 2 | Объект | BLOB-объект | Standard | Горячий, холодный или архивный | 3.6+ | All |
|Хранилище Azure| Общего назначения версии 1 | Объект | BLOB-объект | Standard | Недоступно | Все | All |
|Хранилище Azure| Хранилище BLOB-объектов * * | Объект | Блочный BLOB-объект | Стандартный | Горячий, холодный или архивный | All | All |
|Хранилище Azure Data Lake Storage 1-го поколения| Недоступно | Иерархическая (файловая система) | Недоступно | Недоступно | Недоступно | только 3,6 | Все, кроме HBase |

* * Для кластеров HDInsight только вторичные учетные записи хранения могут иметь тип Блобстораже, а страничный BLOB-объект — не поддерживаемый вариант хранения.

Дополнительные сведения о типах учетных записей хранения Azure см. в статье [Обзор учетной записи хранения Azure](../storage/common/storage-account-overview.md) .

Дополнительные сведения о уровнях доступа к хранилищу Azure см. в статье [хранилище BLOB-объектов Azure: Premium (Предварительная версия), горячий, стильный и архивный уровни хранилища.](../storage/blobs/storage-blob-storage-tiers.md)

Вы можете создавать кластеры с помощью сочетаний служб для основного и дополнительного хранилища. В следующей таблице перечислены конфигурации хранилища кластера, которые в настоящее время поддерживаются в HDInsight.

| Версия HDInsight | Основное хранилище | Дополнительное хранилище | Поддерживается |
|---|---|---|---|
| 3,6 & 4,0 | Общего назначения v1, общего назначения v2 | Общего назначения v1, общего назначения v2, Блобстораже (блочные BLOB-объекты) | Да |
| 3,6 & 4,0 | Общего назначения v1, общего назначения v2 | Data Lake Storage 2-го поколения | Нет |
| 3,6 & 4,0 | Data Lake Storage 2-го поколения * | Data Lake Storage 2-го поколения | Да |
| 3,6 & 4,0 | Data Lake Storage 2-го поколения * | Общего назначения v1, общего назначения v2, Блобстораже (блочные BLOB-объекты) | Да |
| 3,6 & 4,0 | Data Lake Storage 2-го поколения | Azure Data Lake Storage 1-го поколения | Нет |
| 3,6 | Azure Data Lake Storage 1-го поколения | Azure Data Lake Storage 1-го поколения | Да |
| 3,6 | Azure Data Lake Storage 1-го поколения | Общего назначения v1, общего назначения v2, Блобстораже (блочные BLOB-объекты) | Да |
| 3,6 | Azure Data Lake Storage 1-го поколения | Data Lake Storage 2-го поколения | Нет |
| 4,0 | Azure Data Lake Storage 1-го поколения | Любой | Нет |
| 4,0 | Общего назначения v1, общего назначения v2 | Azure Data Lake Storage 1-го поколения | Нет |

* = Это может быть один или несколько Data Lake Storage 2-го поколения, если все они настроены для использования одного управляемого удостоверения для доступа к кластеру.

> [!NOTE]
> Data Lake Storage 2-го поколения основное хранилище не поддерживается для кластеров Spark 2,1 и 2,2.

## <a name="data-replication"></a>Репликация данных

Azure HDInsight не хранит данные клиента. Основным средством хранения для кластера являются связанные учетные записи хранения. Вы можете подключить кластер к существующей учетной записи хранения или создать новую учетную запись хранения в процессе создания кластера. Если создана новая учетная запись, она будет создана как учетная запись локально избыточного хранилища (LRS) и будет соответствовать требованиям местонахождение данных в регионе, включая те, которые указаны в [центре управления безопасностью](https://azuredatacentermap.azurewebsites.net).

Вы можете проверить, правильно ли настроен HDInsight для хранения данных в одном регионе, убедившись, что учетная запись хранения, связанная с HDInsight, является LRS или другим вариантом хранилища, указанным в [центре управления безопасностью](https://azuredatacentermap.azurewebsites.net).
 
## <a name="next-steps"></a>Дальнейшие шаги

* [Общие сведения о службе хранилища Azure в HDInsight](./overview-azure-storage.md)
* [Общие сведения о Azure Data Lake Storage 1-го поколения в HDInsight](./overview-data-lake-storage-gen1.md)
* [Общие сведения о Azure Data Lake Storage 2-го поколения в HDInsight](./overview-data-lake-storage-gen2.md)
* [Общие сведения об Azure Data Lake Storage 2-го поколения](../storage/blobs/data-lake-storage-introduction.md)
* [Введение в хранилище Azure](../storage/common/storage-introduction.md)
