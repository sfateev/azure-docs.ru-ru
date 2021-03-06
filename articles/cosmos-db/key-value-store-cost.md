---
title: Затраты единиц запроса на использование среды Azure Cosmos DB в качестве хранилища пар "ключ — значение"
description: Узнайте о затратах единиц запроса на использование среды Azure Cosmos DB для простых операций записи и чтения, если она используется как хранилище пар "ключ —значение".
author: SnehaGunda
ms.service: cosmos-db
ms.topic: how-to
ms.date: 08/23/2019
ms.author: sngun
ms.custom: seodec18
ms.openlocfilehash: 1cd6b4b52db224db5febcec1eff79b01379a5956
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2020
ms.locfileid: "85262826"
---
# <a name="azure-cosmos-db-as-a-key-value-store--cost-overview"></a>Azure Cosmos DB как хранилище значений ключей — общие сведения о стоимости

Azure Cosmos DB — это глобально распределенная многомодельная служба базы данных, предназначенная для быстрого создания высокодоступных крупномасштабных приложений. По умолчанию Azure Cosmos DB автоматически и эффективно индексирует все принимаемые данные. Это позволяет быстро и единообразно выполнять запросы [SQL](how-to-sql-query.md) (и [JavaScript](stored-procedures-triggers-udfs.md)) к данным. 

В этой статье рассматриваются затраты на использование среды Azure Cosmos DB для простых операций записи и чтения, если она используется как хранилище пар "ключ —значение". К операциям записи относятся операции вставки, замены, удаления и операции Upsert элементов данных. Помимо обеспечения соглашения об уровне обслуживания 99,999% для всех учетных записей с несколькими регионами, Azure Cosmos DB гарантирует <10 мс времени задержки для операций чтения и (индексированных) операций записи на 99-м процентиль. 

## <a name="why-we-use-request-units-rus"></a>Для чего используются единицы запроса (ЕЗ)

Производительность Azure Cosmos DB зависит от объема подготовленной пропускной способности, выраженной в [единицах запроса (единицы запросов](request-units.md) в секунду). Подготовка осуществляется на втором уровне детализации и приобретена в единицах запросов в секунду ([не следует путать с почасовой оплатой](https://azure.microsoft.com/pricing/details/cosmos-db/)). RUs следует рассматривать как логическую абстракцию (валюту), которая упрощает подготовку необходимой пропускной способности для приложения. Пользователям не нужно думать о различиях между пропускной способностью чтения и записи. Одновалютная модель ЕЗ повышает эффективность, позволяя разделять подготовленную емкость между операциями чтения и записи. Эта модель подготовленной емкости позволяет службе предоставлять **предсказуемую и постоянную пропускную способность, гарантированную низкую задержку и высокую доступность**. Наконец, хотя модель RU используется для отображения пропускной способности, каждая подготовленная ЕДИНИЦа времени также имеет определенный объем ресурсов (например, память, ядра, ЦП и операции ввода-вывода).

В качестве глобальной распределенной системы баз данных Cosmos DB — это единственная служба Azure, которая обеспечивает комплексное соглашение об уровне обслуживания, охватывающее задержки, пропускную способность, согласованность и высокий уровень доступности Пропускная способность, которую вы подготавливаете, применяется к каждому региону, связанному с вашей учетной записью Cosmos. Для операций чтения Cosmos DB предлагает на ваш выбор несколько четко определенных [уровней согласованности](consistency-levels.md). 

В следующей таблице показано число получателей, необходимое для выполнения операций чтения и записи на основе элемента данных размером 1 КБ и 100 KBs с отключенным автоматическим индексированием по умолчанию. 

|Размер элемента|1 операция чтения|1 операция записи|
|-------------|------|-------|
|1 КБ|1 ЕЗ|5 ЕЗ|
|100 КБ|10 ЕЗ|50 ЕЗ|

## <a name="cost-of-reads-and-writes"></a>Стоимость операций чтения и записи

Если вы подготавливаете 1 000 единиц запросов в секунду, это количество составляет 3 600 000 единиц запросов в час и будет стоить $0,08 в течение часа (в США и Европе). Для элемента данных размером в 1 КБ это означает, что вы можете использовать 3 600 000 операций чтения или 720 000 (3 600 000 единиц запросов в 5), используя подготовленную пропускную способность. После нормализации до миллиона операций чтения и записи стоимость составит $0,022/млн операций чтения ($0,08/3,6) и $0.111/миллионы операций записи ($0,08/0,72). Стоимость 1 млн операций становится минимальной, как показано в следующей таблице.

|Размер элемента|Стоимость 1 000 000 операций чтения|Стоимость 1 000 000 операций записи|
|-------------|-------|--------|
|1 КБ|0,022 долл. США|0,111 долл. США|
|100 КБ|0,222 долл. США|1,111 долл. США|


Большинство базовых хранилищ BLOB-объектов или объектов, обойдутся в 0,40 долл. США за 1 миллион транзакций чтения и 5 долл. США за 1 миллион транзакций записи. При оптимальном использовании Cosmos DB может быть до 98% дешевле, чем эти решения (для транзакций с 1 КБ).

## <a name="next-steps"></a>Дальнейшие действия

* Используйте [Калькулятор единиц](https://cosmos.azure.com/capacitycalculator/) для оценки пропускной способности рабочих нагрузок.

