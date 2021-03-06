---
title: Выбор между подготовленной пропускной способностью и бессерверным Azure Cosmos DB
description: Узнайте, как выбрать между подготовленной пропускной способностью и бессерверной рабочей нагрузкой.
author: ThomasWeiss
ms.author: thweiss
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 08/19/2020
ms.openlocfilehash: 0adb346a693beaa905438cfdc1249c1646c28811
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2020
ms.locfileid: "88608864"
---
# <a name="how-to-choose-between-provisioned-throughput-and-serverless"></a>Выбор между подготовленной пропускной способностью и бессерверным

Azure Cosmos DB доступен в двух различных режимах емкости: [подготовленная пропускная способность](set-throughput.md) и [бессерверная](serverless.md). Вы можете выполнять одни и те же операции с базой данных в обоих режимах, но способ выставления счетов за эти операции коренным образом различается. В следующем видео объясняются основные различия между этими режимами и их соответствие различным типам рабочих нагрузок.

> [!VIDEO https://www.youtube.com/embed/CgYQo6uHyt0]

> [!NOTE]
> В настоящее время серверные компоненты поддерживаются только в API Azure Cosmos DB Core (SQL).

## <a name="detailed-comparison"></a>Подробное сравнение

| Критерии | Подготовленная пропускная способность | Бессерверные приложения |
| --- | --- | --- |
| Состояние | Общедоступная версия | В предварительной версии |
| Лучше всего подходит для | Критически важные рабочие нагрузки, для которых необходима прогнозируемая производительность | Малые и средние некритические рабочие нагрузки с интенсивным трафиком |
| Принцип работы | Для каждого контейнера вы подготавливаете некоторый объем пропускной способности, выраженный в [единицах запросов](request-units.md) в секунду. Каждую секунду это количество единиц запросов доступно для операций базы данных. Подготовленную пропускную способность можно обновить вручную или настроить автоматически с помощью [автомасштабирования](provision-throughput-autoscale.md). | Вы выполняете операции с базами данных в контейнерах без необходимости подготавливать какие бы то ни было ресурсы. |
| Ограничения на учетную запись | Максимальное число регионов Azure: без ограничений | Максимальное число регионов Azure: 1 |
| Ограничения на контейнер | Максимальная пропускная способность: без ограничений<br>Максимальный объем хранилища: не ограничено | Максимальная пропускная способность: 5 000 единиц запросов в секунду<br>Максимальный объем хранилища: 50 ГБ |
| Производительность | доступность на 99,99% до 99,999%, охваченная соглашением об уровне обслуживания<br>< 10 мс задержка для операций чтения и записи, охваченных соглашением об уровне обслуживания<br>Гарантия соглашения об уровне обслуживания на 99,99% гарантирует пропускную способность | доступность на 99,9% до 99,99%, охваченная соглашением об уровне обслуживания<br>< 10 мс задержка для операций чтения баллов и < 30 мс для операций записи, охваченных цели уровня обслуживания<br>95% нагрузки, охваченной целями уровня обслуживания |
| Модель выставления счетов | Выставление счетов выполняется почасово для подготовленного числа единиц запросов в секунду, независимо от того, сколько их было использовано. | Выставление счетов выполняется для каждого почасового периода в течение числа получателей, потребляемых операциями базы данных. |

> [!IMPORTANT]
> Некоторые бессерверные ограничения могут быть облегчило или удалены, когда бессерверная доступность становится общедоступной, и **ваши отзывы** помогут нам принять решение! Свяжитесь с нами и расскажите нам о бессерверных впечатлениях: [azurecosmosdbserverless@service.microsoft.com](mailto:azurecosmosdbserverless@service.microsoft.com) .

## <a name="burstability-and-expected-consumption"></a>Разбивка и ожидаемое потребление

В некоторых ситуациях может быть неясно, следует ли выбирать подготовленную пропускную способность или бессерверный режим для конкретной рабочей нагрузки. Чтобы упростить это решение, можно оценить следующее:

- **Требование к** нагрузке рабочей нагрузки — это максимальный объем, который может потребоваться использовать в одной секунде.
- Общее **ожидаемое потребление**, то есть общее количество, которое вы можете использовать в течение месяца (это можно оценить с помощью приведенной [здесь](plan-manage-costs.md#estimating-serverless-costs)таблицы).

Если для рабочей нагрузки требуется более 5 000 единиц запросов в секунду, необходимо выбрать подготовленную пропускную способность, так как бессерверные контейнеры не могут превысить это ограничение. В противном случае можно сравнить стоимость обоих режимов в зависимости от ожидаемого потребления.

**Пример 1**. Ожидается, что Рабочая нагрузка будет занимать не более 10 000 единиц запросов в секунду, а всего 20 000 000 в течение месяца.

- Только подготовленный режим пропускной способности может обеспечить пропускную способность 10 000 единиц запросов в секунду.

**Пример 2**. Ожидается, что Рабочая нагрузка будет занимать не более 500 единиц запросов в секунду, а всего 20 000 000 в течение месяца.

- В режиме подготовленной пропускной способности можно подготовить контейнер с 500 единиц запросов в секунду для месячной стоимости: $0,008 * 5 * 730 = **$29,20**
- В режиме "бессерверный" вы платите за использование сервера RUs: $0,25 * 20 = **$5,00**

**Пример 3**. Ожидается, что Рабочая нагрузка будет занимать не более 500 единиц запросов в секунду, а всего 250 000 000 в течение месяца.

- В режиме подготовленной пропускной способности можно подготовить контейнер с 500 единиц запросов в секунду для месячной стоимости: $0,008 * 5 * 730 = **$29,20**
- В режиме "бессерверный" вы платите за использование "RUs": $0,25 * 250 = **$62,50**

(эти примеры не являются учетом затрат на хранение, которые одинаковы в двух режимах).

> [!NOTE]
> Стоимость, показанная в предыдущем примере, используется только в демонстрационных целях. Последние сведения о ценах см. на [странице с ценами](https://azure.microsoft.com/pricing/details/cosmos-db/) .

## <a name="next-steps"></a>Дальнейшие действия

- Дополнительные сведения о [подготовке пропускной способности для Azure Cosmos DB](set-throughput.md)
- Узнайте больше о [Azure Cosmos DB бессерверных](serverless.md)
- Знакомство с концепцией [единиц запросов](request-units.md)
