---
title: Варианты передачи небольших наборов данных Azure при низкой или средней пропускной способности сети | Документация Майкрософт
description: Узнайте, как выбрать решение Azure для передачи данных, когда в среде низкая или средняя пропускная способность сети и планируется передача небольших наборов данных.
services: storage
author: alkohli
ms.service: storage
ms.subservice: blobs
ms.topic: conceptual
ms.date: 12/05/2018
ms.author: alkohli
ms.openlocfilehash: 4f21e7f64338b7d50ca401081bf73ca0c1a1c88f
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2020
ms.locfileid: "85504310"
---
# <a name="data-transfer-for-small-datasets-with-low-to-moderate-network-bandwidth"></a>Передача небольших наборов данных при низкой или средней пропускной способности сети
 
В этой статье представлен обзор решений для передачи данных, когда в среде низкая или средняя пропускная способность сети и планируется передача небольших наборов данных. Также здесь описаны рекомендуемые варианты передачи данных и приведена матрица ключевых функций для указанного сценария.

Чтобы узнать больше обо всех доступных параметрах передачи данных, ознакомьтесь со статьей [Choose an Azure solution for data transfer](storage-choose-data-transfer-solution.md) (Выбор решения Azure для передачи данных).

## <a name="scenario-description"></a>Описание сценария

Небольшими считаются наборы данных, размеры которых измеряются в гигабайтах или нескольких терабайтах. Низкая или средняя пропускная способность сети подразумевает скорость от 45 Мбит/с (подключение T3 в центре обработки данных) до 1 Гбит/с.

- Если вы перемещаете только несколько файлов и вам не нужно автоматизировать передачу данных, рассмотрите использование средств с графическим интерфейсом.
- Если вы знакомы с системным администрированием, рассмотрите использование командной строки, программных средств или средств для работы со сценариями.

## <a name="recommended-options"></a>Рекомендуемые варианты

Ниже приведены варианты, рекомендуемые в этом сценарии.

- **Средства с графическим интерфейсом**, такие как Обозреватель службы хранилища и служба хранилища на портале Azure. Они предоставляют простой способ просмотра данных и быстрой передачи нескольких файлов.

    - **Обозреватель службы хранилища Azure**. Это кроссплатформенное средство позволяет управлять содержимым учетных записей хранения Azure. С его помощью можно передавать и скачивать большие двоичные объекты, файлы, очереди, таблицы и сущности Azure Cosmos DB, а также управлять ими. Используйте его с хранилищем BLOB-объектов для управления большими двоичными объектами и папками, а также для передачи и скачивания больших двоичных объектов между локальной файловой системой и хранилищем BLOB-объектов или учетными записями хранения.
    - **Портал Azure**. Служба хранилища на портале Azure предоставляет веб-интерфейс для просмотра файлов и их передачи по одному. Это хороший вариант, если вам не нужно устанавливать какие-либо средства или применять команды для быстрого просмотра или передачи нескольких файлов.

- **Программные средства и средства для работы со сценариями**, такие как AzCopy, PowerShell, Azure CLI и интерфейсы REST API службы хранилища Azure.

    - **AzCopy.** Используйте эту программу командной строки, чтобы копировать данные в хранилище BLOB-объектов Azure, хранилище файлов и таблиц и из них с оптимальной производительностью. AzCopy поддерживает параллелизм и возможность возобновить операции копирования в случае сбоя.
    - **Azure PowerShell**. Пользователи, которые знакомы с системным администрированием, используют для передачи данных модуль службы хранилища в Azure PowerShell.
    - **Azure CLI**. Используйте это кроссплатформенное средство, чтобы управлять службами Azure и передавать данные в службу хранилища Azure.
    - **Интерфейсы REST API и пакеты SDK службы хранилища Azure**. При создании приложения можно разработать его с интерфейсами REST API и пакетами SDK службы хранилища Azure, а также использовать клиентские библиотеки Azure, которые доступны на нескольких языках.


## <a name="comparison-of-key-capabilities"></a>Сравнение ключевых возможностей

В следующей таблице перечислены различия в ключевых возможностях.

| Компонент | Обозреватель службы хранилища Azure | Портал Azure | AzCopy<br>Azure PowerShell<br>Azure CLI | Интерфейсы REST API и пакеты SDK службы хранилища Azure |
|---------|------------------------|--------------|-----------------------------------------|---------------------------------|
| Доступность | Скачивание и установка <br>Автономное средство | Веб-средства просмотра на портале Azure | Программа командной строки |Программируемые интерфейсы в .NET, Java, Python, JavaScript, C++, Go, Ruby и PHP |
| Графический интерфейс | Да | Да | нет | нет |
| Поддерживаемые платформы | Windows, Mac, Linux | Через Интернет |Windows, Mac, Linux |Все платформы |
| Разрешенные операции в хранилище BLOB-объектов<br>для больших двоичных объектов и папок | Передать<br>Скачать<br>Управление | Передать<br>Скачать<br>Управление |Передать<br>Скачать<br>Управление | Да, поддерживается настройка |
| Разрешенное хранилище Azure Data Lake 1-го поколения<br>операции для файлов и папок | Передать<br>Скачать<br>Управление | нет |Передать<br>Скачать<br>Управление                   | нет |
| Разрешенные операции в хранилище файлов<br>для файлов и каталогов | Передать<br>Скачать<br>Управление | Передать<br>Скачать<br>Управление   |Передать<br>Скачать<br>Управление | Да, поддерживается настройка |
| Разрешенные операции в табличном хранилище<br>для таблиц |Управление | нет |Поддержка таблиц в AzCopy версии 7 |Да, поддерживается настройка|
| Разрешенное хранилище очередей | Управление | нет  |нет | Да, поддерживается настройка|


## <a name="next-steps"></a>Дальнейшие действия

- Узнайте, как [переносить данные с помощью Обозревателя службы хранилища Azure](https://docs.microsoft.com/azure/machine-learning/team-data-science-process/move-data-to-azure-blob-using-azure-storage-explorer).
- [Передача данных с помощью AzCopy версии 10 (предварительная версия)](https://docs.microsoft.com/azure/storage/common/storage-use-azcopy-v10)

