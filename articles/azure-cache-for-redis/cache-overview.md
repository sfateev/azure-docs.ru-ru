---
title: Что такое кэш Azure для Redis?
description: Узнайте о том, как использовать Кэш Azure для Redis для обеспечения кэширования содержимого, кэширования сеансов пользователей, организации очереди для заданий и сообщений, а также распределенных транзакций.
author: yegu-ms
ms.author: yegu
ms.service: cache
ms.topic: overview
ms.date: 05/12/2020
ms.openlocfilehash: 26f6c8e3aceddc6f766bb43a1e384d761dee32bf
ms.sourcegitcommit: eb6bef1274b9e6390c7a77ff69bf6a3b94e827fc
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/05/2020
ms.locfileid: "91631388"
---
# <a name="azure-cache-for-redis"></a>Кэш Redis для Azure
Кэш Azure для Redis обеспечивает хранение данных в памяти с помощью программного обеспечения с открытым исходным кодом [Redis](https://redis.io/). Redis повышает производительность и масштабируемость приложения, которое в значительной степени использует внутренние хранилища данных. Он может обрабатывать большие объемы запросов приложений, сохраняя часто используемые данные в памяти сервера, которые можно быстро записать и считать. Redis предоставляет критически важное решение для хранения данных с низкой задержкой и высокой пропускной способностью для современных приложений.

Кэш Azure для Redis предоставляет Redis как управляемую службу. Он предоставляет защищенные и выделенные экземпляры сервера Redis и полную совместимость с Redis API. Служба обслуживается корпорацией Майкрософт, размещается в Azure и предоставляется для любого приложения в среде Azure и за ее пределами.

Кэш Azure для Redis можно использовать как распределенный кэш данных или кэш содержимого, хранилище сеансов, брокер сообщений и т. д. Его можно развернуть как автономную службу базы данных Azure или вместе с другой службой, такой как Azure SQL или Cosmos DB.

## <a name="key-scenarios"></a>Основные сценарии
Кэш Azure для Redis повышает производительность приложений за счет поддержки стандартных шаблонов архитектуры приложений. Ниже перечислены некоторые наиболее распространенные шаблоны.

| Модель      | Описание                                        |
| ------------ | -------------------------------------------------- |
| [Кэш данных](cache-web-app-cache-aside-leaderboard.md) | Базы данных часто слишком велики для загрузки непосредственно в кэш. Обычно используется шаблон [кэш на стороне](https://docs.microsoft.com/azure/architecture/patterns/cache-aside) для загрузки данных только при необходимости. Когда система вносит изменения в данные, она может обновлять кэш, который затем распространяется на другие клиенты. Кроме того, система может установить истечение срока действия данных или использовать политику вытеснения, чтобы вызвать обновление данных в кэше.|
| [Кэш содержимого](cache-aspnet-output-cache-provider.md) | Многие веб-страницы создаются на основе шаблонов, использующих статическое содержимое, например верхние и нижние колонтитулы или баннеры. Эти статические элементы должны изменяться не часто. Использование кэша в памяти обеспечивает быстрый (по сравнению с серверными хранилищами данных) доступ к статическому содержимому. Данный шаблон сокращает время обработки и нагрузку на сервер, что позволяет повысить скорость реагирования веб-серверов. Это позволяет сократить количество серверов, необходимых для обработки нагрузок. Кэш Azure для Redis предоставляет поставщик кэша вывода Redis для поддержки этого шаблона с помощью ASP.NET.|
| [Хранилище сеансов](cache-aspnet-session-state-provider.md) | Этот шаблон обычно используется с корзинами покупок и другими данными из истории пользователя, которую веб-приложение может связать с файлами cookie пользователя. Хранение большого объема содержимого в файле cookie может отрицательно сказаться на производительности, так как размер этого файла растет и передается и проверяется с каждым запросом. Типичным решением является использование файла cookie в качестве ключа для запроса данных в базе данных. Использование кэша в памяти, например кэша Azure для Redis, для связывания информации с пользователем намного быстрее, чем взаимодействие с полной реляционной базой данных. |
| Очереди задач и сообщений | Приложения часто добавляют задачи в очередь, если для выполнения операций, связанных с запросом, требуется какое-то время. Более длительные операции помещаются в очередь для последовательной обработки (зачастую на другом сервере).  Этот метод отсрочки работы называется постановкой задач в очередь. Кэш Azure для Redis предоставляет распределенную очередь, обеспечивающую возможность использования этого шаблона в приложении.|
| Распределенные транзакции | Приложениям иногда требуется, чтобы ряд команд для серверного хранилища данных выполнялся как единая атомарная операция. Все команды должны успешно завершиться или все транзакции должны быть возвращены в исходное состояние. Кэш Azure для Redis поддерживает выполнение пакета команд в рамках одной [транзакции](https://redis.io/topics/transactions). |

## <a name="redis-versions"></a>Версии Redis

Кэш Azure для Redis поддерживает Redis версии 4.x и 6.0 (в виде предварительной версии). Мы решили пропустить Redis 5.0 и предоставить вам последнюю версию. Ранее Кэш Azure для Redis поддерживал только одну версию Redis. В будущем он будет обеспечивать поддержку обновления до новой основной версии и по меньшей мере одной предыдущей стабильной версии. Вы можете [выбрать версию](cache-how-to-version.md) для своего приложения.

> [!NOTE]
> Redis 6.0 в настоящее время предоставляется в предварительной версии. [Свяжитесь с нами](mailto:azurecache@microsoft.com), если хотите получить ее. Предварительная версия предоставляется без соглашения об уровне обслуживания и не рекомендуется для использования в рабочей среде. Дополнительные сведения см. в статье [Дополнительные условия использования предварительных выпусков Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).
>

## <a name="service-tiers"></a>Уровни службы
Кэш Azure для Redis доступен на следующих уровнях:

| Уровень | Описание |
|---|---|
| Basic | Кэш одного узла. Этот уровень поддерживает несколько вариантов объема памяти (от 250 МБ до 53 ГБ) и идеально подходит для разработки и тестирования, а также некритических рабочих нагрузок. Для уровня "Базовый" соглашение об уровне обслуживания не предусмотрено. |
| Standard | Управляемый Azure реплицируемый кэш в конфигурации с двумя узлами (основным и репликой) с [соглашением об уровне обслуживания](https://azure.microsoft.com/support/legal/sla/cache/v1_0/) с высоким уровнем доступности. |
| Premium | Уровень "Премиум" предназначен для предприятий. Кэш уровня "Премиум" поддерживает больше возможностей и имеет более высокую пропускную способность с более низкими задержками. Кэши уровня "Премиум" развертываются на более мощном оборудовании и обеспечивают лучшую производительность по сравнению с уровнями "Базовый" и "Стандартный". Это преимущество означает, что пропускная способность для кэша такого же размера будет выше для уровня "Премиум" по сравнению с уровнем "Стандартный". |

### <a name="feature-comparison"></a>Сравнение возможностей
Страница [Цены на кэш Azure для Redis](https://azure.microsoft.com/pricing/details/cache/) содержит подробное сравнение цен каждого уровня. В следующей таблице описаны некоторые функции, поддерживаемые уровнями:

| Описание функции | Premium | Standard | Basic |
| ------------------- | :-----: | :------: | :---: |
| [Соглашение об уровне обслуживания](https://azure.microsoft.com/support/legal/sla/cache/v1_0/) |✔|✔|-|
| [Сохраняемость данных Redis](cache-how-to-premium-persistence.md) |✔|-|-|
| [Кластер Redis](cache-how-to-premium-clustering.md) |✔|-|-|
| [Обеспечение безопасности с помощью правил брандмауэра](cache-configure.md#firewall) |✔|✔|✔|
| Шифрование при передаче |✔|✔|✔|
| [Усиленная безопасность и изоляция с помощью виртуальной сети](cache-how-to-premium-vnet.md) |✔|-|-|
| [Импорт и экспорт](cache-how-to-import-export-data.md) |✔|-|-|
| [Запланированные обновления](cache-administration.md#schedule-updates) |✔|✔|✔|
| [Георепликация](cache-how-to-geo-replication.md) |✔|-|-|
| [Перезагрузка](cache-administration.md#reboot) |✔|✔|✔|

### <a name="choosing-the-right-tier"></a>Выбор подходящего уровня
При выборе кэша Azure для уровня Redis необходимо учитывать следующее.

* **Память**. Уровни "Базовый" и "Стандартный" предлагают от 250 МБ до 53 ГБ. Уровень "Премиум" обеспечивает до 1,2 ТБ (в виде кластера) или 120 ГБ (без кластера). Дополнительные сведения см. на странице [Цены на Кэш Azure для Redis](https://azure.microsoft.com/pricing/details/cache/).
* **Производительность сети**. Если у вас есть рабочая нагрузка, которая требует высокой пропускной способности, то уровень "Премиум" предоставит большую пропускную способность по сравнению с уровнями "Стандартный" или "Базовый". Также в рамках каждого уровня пропускная способность для кэшей большего размера выше из-за виртуальной машины, на которой размещен кэш. Дополнительные сведения см. в разделе [Производительность кэша Azure для Redis](cache-planning-faq.md#azure-cache-for-redis-performance).
* **Пропускная способность**. Уровень "Премиум" предлагает максимальную доступную пропускную способность. При достижении сервером кэширования или клиентом пределов пропускной способности на стороне клиента могут возникнуть простои. Дополнительные сведения приведены в таблице ниже.
* **Высокий уровень доступности**. Кэш Azure для Redis гарантирует, что кэш уровня "Стандартный" или "Премиум" будет доступен в соответствии с [соглашением об уровне обслуживания](https://azure.microsoft.com/support/legal/sla/cache/v1_0/). Соглашение об уровне обслуживания распространяется только на подключения к конечным точкам кэша. Соглашение об уровне обслуживания не включает защиту от потери данных. Для повышения устойчивости к потере данных рекомендуется использовать функцию постоянного хранения данных Redis уровня Премиум.
* **Сохраняемость данных Redis**. Уровень Премиум позволяет сохранить данные кэша в учетной записи хранения Azure. В кэше уровня "Базовый" или "Стандартный" все данные хранятся только в памяти. Проблемы с базовой инфраструктурой могут привести к потере данных. Для повышения устойчивости к потере данных рекомендуется использовать функцию постоянного хранения данных Redis уровня Премиум. Кэш Azure для Redis предлагает варианты RDB и AOF (предварительная версия) для сохраняемости Redis. Дополнительные сведения см. в статье [How to configure persistence for a Premium Azure Cache for Redis](cache-how-to-premium-persistence.md) (Как настроить сохраняемость для кэша Azure для Redis категории "Премиум").
* **Кластер Redis**. Используется для создания кэша размером более 120 ГБ или сегментации данных по нескольким узлам Redis. Более того, можно воспользоваться кластеризацией Redis, которая доступна на уровне "Премиум". Каждый узел состоит из пары кэшей (основной кэш и реплика) для обеспечения высокой доступности. Дополнительные сведения см. в статье [How to configure clustering for a Premium Azure Cache for Redis](cache-how-to-premium-clustering.md) (Настройка кластеризации кэша Azure для Redis категории "Премиум").
* **Оптимизированная защита и сетевая изоляция**. Развертывание виртуальной сети Azure обеспечивает дополнительные возможности защиты и изоляции для кэша Azure для Redis. Такие развертывания позволяют определять подсети, настраивать политики контроля доступа, а также использовать другие функции ограничения доступа. Дополнительные сведения см. в статье [How to configure Virtual Network support for a Premium Azure Cache for Redis](cache-how-to-premium-vnet.md) (Настройка поддержки виртуальной сети для кэша Azure для Redis уровня "Премиум").
* **Настройка Redis**. На уровнях "Стандартный" и "Премиум" вы можете настроить уведомления пространства ключей в Redis.
* **Максимальное количество клиентских подключений**. Уровень "Премиум" обеспечивает максимальное число клиентов, которые могут подключаться к Redis, причем чем больше размер кэша, тем больше число подключений. Кластеризация не увеличивает число подключений, доступных для кластеризованного кэша. Дополнительные сведения см. на странице [Цены на Кэш Azure для Redis](https://azure.microsoft.com/pricing/details/cache/).
* **Выделенное ядро для сервера Redis**. На уровне "Премиум" для всех размеров кэша для Redis выделяется отдельное ядро. На уровнях "Базовый" и "Стандартный" отдельное ядро выделяется для размера C1 и выше.
* **Обработка с одним потоком**. По умолчанию Redis использует для обработки команд только один поток. Кэш Azure для Redis также использует дополнительные ядра для обработки операций ввода-вывода. Наличие большего количества ядер повышает пропускную способность, даже если это не приводит к линейному масштабированию. Кроме того, ВМ больших размеров, как правило, имеют более высокий предел пропускной способности, чем ВМ меньшего размера. Это поможет избежать насыщенности сети, что приведет к превышению времени ожидания в приложении.
* **Повышение производительности**. Кэши на уровне "Премиум" разворачиваются на оборудовании с более быстрыми процессорами и обеспечивают повышенную производительность по сравнению с уровнями "Базовый" и "Стандартный". Кэши уровня Премиум обладают более высокой пропускной способностью и меньшей задержкой. Дополнительные сведения см. разделе [Производительность кэша Azure для Redis](cache-planning-faq.md#azure-cache-for-redis-performance)

Созданный кэш можно перевести на более высокий уровень. Переход на более низкий уровень не поддерживается. Пошаговые инструкции по масштабированию см. в статье [How to Scale Azure Cache for Redis](cache-how-to-scale.md) (Масштабирование кэша Azure для Redis) и разделе, посвященному [автоматизации операции масштабирования](cache-how-to-scale.md#how-to-automate-a-scaling-operation).

## <a name="next-steps"></a>Дальнейшие действия
* [Краткое руководство по созданию приложения ASP.NET](cache-web-app-howto.md). Создание простого веб-приложения ASP.NET, в котором используется кэш Azure для Redis.
* [Краткое руководство по созданию приложения .NET](cache-dotnet-how-to-use-azure-redis-cache.md). Создание приложения .NET, в котором используется кэш Azure для Redis.
* [Краткое руководство по созданию приложения .NET Core](cache-dotnet-core-quickstart.md). Создание приложения .NET Core, в котором используется кэш Azure для Redis.
* [Краткое руководство по созданию приложения Node.js](cache-nodejs-get-started.md). Создание простого приложения Node.js, в котором используется кэш Azure для Redis.
* [Краткое руководство по созданию приложения Java](cache-java-get-started.md). Создание простого приложения Java, в котором используется кэш Azure для Redis.
* [Краткое руководство по созданию приложения Python](cache-python-get-started.md). Создание приложения Python, в котором используется кэш Azure для Redis.
