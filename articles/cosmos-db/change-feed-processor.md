---
title: Обработчик канала изменений в Azure Cosmos DB
description: Узнайте, как использовать обработчик канала изменений Azure Cosmos DB для чтения канала изменений, а также о компонентах обработчика канала изменений.
author: timsander1
ms.author: tisande
ms.service: cosmos-db
ms.devlang: dotnet
ms.topic: conceptual
ms.date: 05/13/2020
ms.reviewer: sngun
ms.custom: devx-track-csharp
ms.openlocfilehash: 3a802cc3d6178302445e0c31c52785d00207d0bd
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2020
ms.locfileid: "88998549"
---
# <a name="change-feed-processor-in-azure-cosmos-db"></a>Обработчик канала изменений в Azure Cosmos DB

Обработчик канала изменений входит в [Azure Cosmos DB SDK версии 3](https://github.com/Azure/azure-cosmos-dotnet-v3). Он упрощает процесс чтения канала изменений и эффективно распределяет обработку событий между несколькими потребителями.

Основным преимуществом библиотеки обработчика канала изменений является отказоустойчивое поведение, которое гарантирует доставку всех событий в канале изменений не менее одного раза.

## <a name="components-of-the-change-feed-processor"></a>Компоненты обработчика канала изменений

В реализации обработчика канала изменений существует четыре основных компонента:

1. **Отслеживаемый контейнер**. Контейнер с данными, из которых формируется канал изменений. Все операции вставки и обновлений в отслеживаемом контейнере отражаются в канале изменений контейнера.

1. **Контейнер аренд**. Контейнер аренды выступает в качестве хранилища состояний и координирует обработку канала изменений для нескольких рабочих ролей. Контейнер аренды может храниться в той же учетной записи, что и отслеживаемый контейнер, или в отдельной учетной записи.

1. **Узел.** Узел — это экземпляр приложения, который использует обработчик канала изменений для прослушивания изменений. Несколько экземпляров с одной и той же конфигурацией аренды могут выполняться параллельно, но все экземпляры должны иметь **разные имена**.

1. **Делегат.** Делегат — это код, определяющий, что разработчик должен сделать с каждым пакетом изменений, считанных обработчиком канала изменений. 

Чтобы лучше разобраться, как взаимодействуют эти четыре элемента обработчика для канала изменений, давайте рассмотрим пример, приведенный на схеме ниже. Отслеживаемый контейнер хранит документы и использует City в качестве ключа секции. Мы видим, что значения ключа секции распределяются по диапазонам, содержащим элементы. Существует два экземпляра узла, и обработчик канала изменений назначает каждому экземпляру разные диапазоны значений ключа секции для максимального распределения вычислительных ресурсов. Каждый диапазон считывается параллельно, и его ход выполнения отделен от других диапазонов в контейнере аренды.

:::image type="content" source="./media/change-feed-processor/changefeedprocessor.png" alt-text="Пример обработчика канала изменений" border="false":::

## <a name="implementing-the-change-feed-processor"></a>Реализация обработчика канала изменений

Точка входа всегда является отслеживаемым контейнером из экземпляра `Container`, который вызывается `GetChangeFeedProcessorBuilder`:

[!code-csharp[Main](~/samples-cosmosdb-dotnet-change-feed-processor/src/Program.cs?name=DefineProcessor)]

Первый параметр представляет собой уникальное имя, описывающее цель этого обработчика, а второе имя — это реализация делегата, который будет выполнять обработку изменений. 

Пример делегата:


[!code-csharp[Main](~/samples-cosmosdb-dotnet-change-feed-processor/src/Program.cs?name=Delegate)]

Наконец, определите имя для этого экземпляра обработчика с помощью `WithInstanceName` и контейнер для поддержания состояния аренды с помощью `WithLeaseContainer`.

Вызов `Build` предоставит экземпляр обработчика, который можно запустить, вызвав `StartAsync`.

## <a name="processing-life-cycle"></a>Жизненный цикл обработки

Обычный жизненный цикл экземпляра узла:

1. Чтение канала изменений.
1. Если изменения отсутствуют, перейдите в спящий режим на предопределенный период времени (настраиваемый с помощью `WithPollInterval` в построителе), а затем перейдите к № 1.
1. При наличии изменений отправьте их в **делегат**.
1. Когда делегат **успешно** завершит обработку изменений, добавьте в хранилище аренды последнюю обработанную точку во времени и перейдите к № 1.

## <a name="error-handling"></a>Обработка ошибок

Обработчик канала изменений устойчив к ошибкам пользовательского кода. Это означает, что если в реализации делегата имеется необработанное исключение (шаг 4), то поток, обрабатывающий определенный пакет изменений, будет остановлен и будет создан новый поток. Новый поток будет проверять, какой момент времени последним указан в хранилище аренды для этого диапазона значений ключей секций, и перезапускаться из этого момента, отправляя тот же пакет изменений делегату. Это поведение будет продолжаться до тех пор, пока делегат не сможет правильно обработать изменения. Поэтому обработчик канала изменений гарантированно отправляет изменения хотя бы один раз, так как, если код делегата создает исключение, он будет повторять попытку с этим пакетом.

Чтобы обработчик канала изменений не мог постоянно повторять попытки с одним и тем же пакетом изменений, следует добавить в код делегата логику для записи документов в очередь недоставленных сообщений при возникновении исключения. Такая схема гарантирует, что вы сможете отследить необработанные изменения, сохранив возможность продолжать обработку будущих изменений. Очередь недоставленных сообщений может просто быть другим контейнером Cosmos. Точное хранилище данных не имеет значения. В нем просто должны сохраняться необработанные изменения.

Кроме того, можно использовать [оценщик канала изменений](how-to-use-change-feed-estimator.md), чтобы отслеживать ход выполнения экземпляров обработчика канала изменений, считывающих канал изменений. Помимо наблюдения за тем, чтобы обработчик канала изменений не повторял попытки обработки одного и того же пакета изменений, вы сможете увидеть, работает ли обработчик канала изменений с задержкой, вызванной доступными ресурсами, такими как ЦП, память и пропускная способность сети.

## <a name="deployment-unit"></a>Единица развертывания

Одна единица развертывания обработчика канала изменений состоит из одного или нескольких экземпляров с одним и тем же `processorName` и конфигурацией контейнера аренды. Можно использовать множество единиц развертывания, каждая из которых имеет разные бизнес-процессы для изменений, и каждая единица развертывания может состоять из одного или нескольких экземпляров. 

Например, у вас может быть одна единица развертывания, которая активирует внешний API при каждом изменении в контейнере. Другая единица развертывания может перемещать данные в режиме реального времени каждый раз, когда происходит изменение. При возникновении изменений в отслеживаемом контейнере будут уведомлены все ваши единицы развертывания.

## <a name="dynamic-scaling"></a>Динамическое масштабирование

Как упоминалось ранее, в единице развертывания может быть один или несколько экземпляров. Чтобы воспользоваться преимуществами распределения вычислений в рамках единицы развертывания, необходимо выполнить следующие ключевые требования.

1. Все экземпляры должны иметь одинаковую конфигурацию контейнеров аренды.
1. Все экземпляры должны иметь один и тот же `processorName`.
1. Каждый экземпляр должен иметь другое имя (`WithInstanceName`).

Если эти три условия выполнены, обработчик веб-канала изменений будет использовать алгоритм равномерного распределения, распределит все аренды в контейнере аренды во всех работающих экземплярах этой единицы развертывания и параллелизует вычисления. Одна аренда может принадлежать только одному экземпляру в определенный момент времени, поэтому максимальное число экземпляров равно числу аренд.

Количество экземпляров может увеличиваться и уменьшаться, и обработчик канала изменений динамически корректирует нагрузку, перераспределяя ее соответствующим образом.

Более того, обработчик канала изменений можно динамически настроить на масштабирование контейнеров из-за увеличения пропускной способности или хранилища. Когда контейнер растет, обработчик канала изменений прозрачно обрабатывает эти сценарии путем динамического увеличения аренды и распределения новых аренд между существующими экземплярами.

## <a name="change-feed-and-provisioned-throughput"></a>Канал изменений и подготовленная пропускная способность

Вы оплачиваете единицы запросов, так как при перемещении данных в контейнеры Cosmos и из них всегда используются единицы запросов. Плата выставляется за число потребляемых ЕЗ контейнером аренды.

## <a name="where-to-host-the-change-feed-processor"></a>Место размещения обработчика веб-канала изменений

Обработчик веб-канала изменений может размещаться на любой платформе, поддерживающей длительные процессы или задачи:

* Непрерывно выполняющееся [веб-задание Azure](https://docs.microsoft.com/learn/modules/run-web-app-background-task-with-webjobs/).
* Процесс на [виртуальной машине Azure](https://docs.microsoft.com/azure/architecture/best-practices/background-jobs#azure-virtual-machines).
* Фоновое задание в [службе Azure Kubernetes](https://docs.microsoft.com/azure/architecture/best-practices/background-jobs#azure-kubernetes-service).
* [Размещенная служба ASP.NET](https://docs.microsoft.com/aspnet/core/fundamentals/host/hosted-services).

Хотя обработчик веб-каналов изменений может работать в средах с кратковременным временем существования, поскольку контейнер аренды сохраняет состояние, цикл запуска и окончания этих сред будет добавлять задержку к получению уведомлений (из-за издержек при запуске процессора при каждом запуске среды).

## <a name="additional-resources"></a>Дополнительные ресурсы

* [Azure Cosmos DB](sql-api-sdk-dotnet.md)
* [Полный пример приложения на GitHub](https://github.com/Azure-Samples/cosmos-dotnet-change-feed-processor)
* [Дополнительные примеры использования на GitHub](https://github.com/Azure/azure-cosmos-dotnet-v3/tree/master/Microsoft.Azure.Cosmos.Samples/Usage/ChangeFeed)
* [Cosmos DB семинар для обработчика веб-канала изменений](https://azurecosmosdb.github.io/labs/dotnet/labs/08-change_feed_with_azure_functions.html#consume-cosmos-db-change-feed-via-the-change-feed-processor)

## <a name="next-steps"></a>Дальнейшие действия

Вы можете продолжить знакомство с обработчиком канала изменений, перейдя к следующим статьям:

* [Работа с поддержкой веб-канала изменений в Azure Cosmos DB](change-feed.md)
* [Модель извлечения данных канала изменений](change-feed-pull-model.md)
* [Миграция из библиотеки обработчика канала изменений](how-to-migrate-from-change-feed-library.md)
* [Использование оценщика канала изменений](how-to-use-change-feed-estimator.md)
* [Настройка времени запуска обработчика канала изменений](how-to-configure-change-feed-start-time.md)
