---
title: Резервное копирование и восстановление — база данных Azure для PostgreSQL — один сервер
description: Сведения об автоматическом резервном копировании и восстановлении сервера базы данных Azure для PostgreSQL на одном сервере.
author: sr-msft
ms.author: srranga
ms.service: postgresql
ms.topic: conceptual
ms.date: 02/25/2020
ms.openlocfilehash: 0c1b0b5ac0c5c71dc5c98cb91d86f879a82809bc
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2020
ms.locfileid: "91708460"
---
# <a name="backup-and-restore-in-azure-database-for-postgresql---single-server"></a>Резервное копирование и восстановление в базе данных Azure для PostgreSQL — один сервер

Служба "База данных Azure для PostgreSQL" автоматически создает резервные копии для сервера и сохраняет их в локальном избыточном или геоизбыточном хранилище, настроенном пользователем. Резервные копии можно использовать для восстановления сервера до точки во времени. Резервное копирование и восстановление данных являются важной частью любой стратегии непрерывности бизнес-процессов. Таким образом данные защищаются от случайного повреждения или удаления.

## <a name="backups"></a>Резервные копии

База данных Azure для PostgreSQL создает резервные копии файлов данных и журнала транзакций. В зависимости от поддерживаемого максимального размера хранилища мы будем использовать полные и разностные резервные копии (максимум 4 ТБ серверов хранения) или резервные копии моментальных снимков (до 16 ТБ серверов хранилища). При помощи этих резервных копий вы можете восстановить сервер до любой точки во времени в пределах заданного срока хранения резервных копий. По умолчанию срок хранения резервных копий составляет 7 дней. При необходимости вы увеличить его вплоть до 35 дней. Все резервные копии шифруются с помощью 256-битового шифрования AES.

Эти файлы резервных копий не могут быть экспортированы. Резервные копии можно использовать только для операций восстановления в базе данных Azure для PostgreSQL. Для копирования базы данных можно использовать [pg_dump](howto-migrate-using-dump-and-restore.md) .

### <a name="backup-frequency"></a>Частота резервного копирования

#### <a name="servers-with-up-to-4-tb-storage"></a>Серверы с хранилищем объемом до 4 ТБ

Для серверов, поддерживающих максимальный объем хранилища до 4 ТБ, полные резервные копии выполняются каждую неделю. Разностное резервное копирование выполняется дважды в день. Создание резервных копий журналов транзакций происходит каждые 5 минут.


#### <a name="servers-with-up-to-16-tb-storage"></a>Серверы с хранилищем объемом до 16 ТБ

В подмножестве [регионов Azure](https://docs.microsoft.com/azure/postgresql/concepts-pricing-tiers#storage)все новые подготовленные серверы могут поддерживать до 16 ТБ хранилища. Резервное копирование на этих больших серверах хранилища основано на моментальных снимках. Первая полная резервная копия моментального снимка планируется сразу же после создания сервера. Первая полная резервная копия моментальных снимков сохраняется как базовая резервная копия сервера. Последующие резервные копии моментальных снимков — это только разностные резервные копии. Разностные резервные копии моментальных снимков не создаются по фиксированному расписанию. В день выполняются три разностных резервных копирования моментальных снимков. Создание резервных копий журналов транзакций происходит каждые 5 минут. 

### <a name="backup-retention"></a>Хранение архивных копий

Резервные копии сохраняются на основе параметра срок хранения резервной копии на сервере. Можно выбрать срок хранения от 7 до 35 дней. Срок хранения по умолчанию составляет 7 дней. Период хранения можно задать во время создания сервера или позже, обновив конфигурацию резервного копирования с помощью [портал Azure](https://docs.microsoft.com/azure/postgresql/howto-restore-server-portal#set-backup-configuration) или [Azure CLI](https://docs.microsoft.com/azure/postgresql/howto-restore-server-cli#set-backup-configuration). 

Срок хранения резервных копий определяет, насколько раннюю точку во времени можно задать для восстановления, так как это зависит от доступных резервных копий. Срок хранения резервной копии можно также рассматривать как окно восстановления с точки зрения восстановления. Все резервные копии, необходимые для выполнения восстановления на определенный момент времени в течение срока хранения резервной копии, сохраняются в хранилище резервных копий. Например, если срок хранения резервной копии равен 7 дням, окно восстановления считается за последние семь дней. В этом сценарии сохраняются все резервные копии, необходимые для восстановления сервера за последние семь дней. В течение семи дней период хранения резервных копий:
- На серверах с хранилищем объемом до 4 ТБ будет храниться до 2 полных резервных копий базы данных, всех разностных резервных копий и резервных копий журналов транзакций, выполненных после самой ранней полной резервной копии базы данных.
-   На серверах с хранилищем объемом до 16 ТБ будет храниться полный моментальный снимок базы данных, все разностные моментальные снимки и резервные копии журналов транзакций за последние 8 дней.

### <a name="backup-redundancy-options"></a>Типы избыточности для резервного копирования

В службе "База данных Azure для PostgreSQL" вы можете выбрать локально избыточное или геоизбыточное хранилище резервных копий в ценовой категории "Общего назначения" или "С оптимизацией для операций в памяти". Резервные копии в геоизбыточном хранилище не только хранятся в регионе, где размещен сервер, но и реплицируются в [связанный центр обработки данных](https://docs.microsoft.com/azure/best-practices-availability-paired-regions). Благодаря этому сервер лучше защищен и его можно восстановить в другом регионе в случае аварии. На уровне "Базовый" предоставляется только локально избыточное хранилище резервных копий.

> [!IMPORTANT]
> Хранилище резервных копий (локально избыточное или геоизбыточное) можно настроить только на этапе создания сервера. После подготовки сервера невозможно изменить тип избыточности для хранилища резервных копий.

### <a name="backup-storage-cost"></a>Стоимость хранилищ резервных копий

В службе "База данных Azure для PostgreSQL" бесплатно предоставляется хранилище для резервных копий размером до 100 % объема подготовленного хранилища базы данных. За любое дополнительное используемое хранилище резервных копий начислено за ГБ в месяц. Например, если вы подготовили сервер с 250 ГБ хранилища, у вас будет 250 ГБ дополнительного хранилища, доступного для резервных копий сервера без дополнительной платы. Плата за использование хранилища для резервных копий превышает 250 ГБ в соответствии с [моделью ценообразования](https://azure.microsoft.com/pricing/details/postgresql/).

Для отслеживания хранилища резервных копий, используемого сервером, можно использовать [хранилище резервных копий, используемое](concepts-monitoring.md) в Azure Monitor, доступное в портал Azure. Используемая метрика хранилища резервных копий представляет собой сумму хранилища, потребляемую всеми полными резервными копиями базы данных, разностными резервными копиями и резервными копиями журналов на основе срока хранения резервной копии, заданного для сервера. Периодичность резервного копирования управляется службой и объясняется ранее. Высокая активность транзакций на сервере может привести к увеличению использования хранилища резервных копий, независимо от общего размера базы данных. Для геоизбыточного хранилища использование хранилища резервных копий в два раза больше, чем локально избыточное хранилище. 

Основным способом управления затратами на хранение резервных копий является установка соответствующего срока хранения резервных копий и выбор правильных параметров избыточности резервных копий в соответствии с необходимыми целями восстановления. Можно выбрать срок хранения в диапазоне от 7 до 35 дней. Общего назначения и оптимизированные для памяти серверы могут выбрать геоизбыточное хранилище для резервных копий.

## <a name="restore"></a>Восстановить

В службе "База данных Azure для PostgreSQL" при восстановлении создается новый сервер из резервных копий исходного сервера.

Есть два типа восстановления:

- **Восстановление до точки во времени** доступно с помощью параметра избыточность резервного копирования и создает новый сервер в том же регионе, что и исходный сервер.
- **Геовосстановление** доступно только в том случае, если сервер настроен для геоизбыточного хранилища и позволяет восстановить сервер в другой регион.

Предполагаемое время восстановления будет зависеть от нескольких факторов, включая размер базы данных, размер журнала транзакций, пропускную способность сети и общее количество баз данных, восстанавливаемых в том же регионе и в то же время. Обычно время восстановления составляет менее 12 часов.

> [!IMPORTANT]
> Удаленные серверы **нельзя** восстановить. Если вы удалите сервер, все связанные с ним базы данных также будут удалены без возможности восстановления. Чтобы защитить ресурсы сервера после развертывания от случайного удаления или внесения непредвиденных изменений, администраторы могут использовать [блокировки управления](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-lock-resources).

### <a name="point-in-time-restore"></a>Восстановление на момент времени

Независимо от типа избыточности для резервного копирования, вы можете выполнить восстановление до любой точки во времени в пределах срока хранения резервной копии. Новый сервер создается в том же регионе Azure, где расположен исходный сервер, и с такими же конфигурацией ценовой категории, поколением вычислительных ресурсов, числом виртуальных ядер, размером хранилища, сроком хранения резервных копий и типом избыточности резервного копирования.

Восстановление до точки во времени подходит для большинства сценариев. Например, если пользователь случайно удалил данные, важные таблицы или базы данных либо если приложение в результате ошибки случайно перезаписало правильные данные неправильными.

Возможно, вам придется подождать, пока создастся резервная копия журналов транзакций, прежде чем выполнить восстановление до точки во времени в пределах последних пяти минут.

### <a name="geo-restore"></a>Геовосстановление

Вы можете восстановить сервер в другом регионе Azure, где доступна служба, если вы настроили для сервера геоизбыточное хранилище. Серверы, поддерживающие до 4 ТБ хранилища, можно восстановить в геопарный регион или в любой регион, поддерживающий хранилище объемом до 16 ТБ. Для серверов, поддерживающих до 16 ТБ хранилища, георезервное копирование можно также восстановить в любом регионе, поддерживающем серверы размером 16 ТБ. Проверьте [ценовую категорию базы данных Azure для постжескл](concepts-pricing-tiers.md) в списке поддерживаемых регионов.

Геовосстановление используется по умолчанию, когда сервер недоступен из-за аварии в регионе, в котором он размещен. Если из-за масштабной аварии в регионе приложение базы данных станет недоступным, сервер можно будет восстановить из геоизбыточных резервных копий на сервер в любом другом регионе. Между созданием резервной копии и ее репликацией в другом регионе может пройти некоторое время. Эта задержка может длиться до часа, поэтому в случае аварии возможна потеря данных за час или менее.

Во время геовосстановления можно изменить такие конфигурации сервера, как поколение вычислительных ресурсов, виртуальное ядро, срок хранения резервных копий и варианты избыточности для резервного копирования. Изменить ценовую категорию ("Базовый", "Общего назначения" или "С оптимизацией для операций в памяти") и объем хранилища нельзя.

### <a name="perform-post-restore-tasks"></a>Задачи после восстановления

После восстановления с помощью любого из механизмов восстановления необходимо выполнить следующие задачи, прежде чем данные и приложения станут доступными:

- Если новый сервер заменит исходный, перенаправьте клиенты и клиентские приложения на новый сервер.
- Чтобы пользователи могли подключаться, убедитесь, что установлены соответствующие правила брандмауэра на уровне сервера и виртуальной сети. Эти правила не копируются с исходного сервера.
- Убедитесь, что заданы соответствующие данные для входа и разрешений уровня базы данных.
- Настройте оповещения соответствующим образом.

## <a name="next-steps"></a>Дальнейшие шаги

- Узнайте, как выполнить восстановление с помощью [портал Azure](howto-restore-server-portal.md).
- Узнайте, как выполнить восстановление с помощью [Azure CLI](howto-restore-server-cli.md).
- Дополнительные сведения о непрерывности бизнес-процессов см. в  [этой статье](concepts-business-continuity.md).
