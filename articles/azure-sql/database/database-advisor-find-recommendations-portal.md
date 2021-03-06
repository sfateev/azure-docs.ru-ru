---
title: Применение рекомендаций по повышению производительности
description: Используйте портал Azure, чтобы найти рекомендации по производительности, которые могут оптимизировать производительность базы данных.
services: sql-database
ms.service: sql-database
ms.subservice: performance
ms.custom: sqldbrb=1
ms.devlang: ''
ms.topic: how-to
author: danimir
ms.author: danil
ms.reviewer: jrasnik, sstein
ms.date: 12/19/2018
ms.openlocfilehash: 0b7aab13871f1450a3c6907b30b446869b2fefa7
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2020
ms.locfileid: "91443894"
---
# <a name="find-and-apply-performance-recommendations"></a>Поиск и применение рекомендаций по производительности
[!INCLUDE[appliesto-sqldb](../includes/appliesto-sqldb.md)]

Вы можете использовать портал Azure для поиска рекомендаций по производительности, которые могут оптимизировать производительность базы данных в базе данных SQL Azure или устранить некоторые проблемы, обнаруженные в рабочей нагрузке. На странице **рекомендации по производительности** в портал Azure можно найти основные рекомендации на основе их потенциального воздействия.

## <a name="viewing-recommendations"></a>Просмотр рекомендаций

Для просмотра и применения рекомендаций по производительности необходимы соответствующие разрешения [управления доступом на основе ролей Azure (Azure RBAC)](../../role-based-access-control/overview.md) в Azure. Разрешения **Читатель** и **Участник баз данных SQL** необходимы для просмотра рекомендаций, а разрешения **Владелец**, **Участник баз данных SQL** необходимы для выполнения действий: создание, удаление индекса и отмена создания индекса.

Выполните следующие действия, чтобы найти рекомендации по производительности на портал Azure.

1. Войдите на [портал Azure](https://portal.azure.com/).
2. Перейдите в раздел **все службы**  >  **базы данных SQL**и выберите свою базу данных.
3. Перейдите на страницу **Рекомендации по производительности**, чтобы просмотреть доступные рекомендации для выбранной базы данных.

Рекомендации по производительности показаны в таблице, как на приведенном ниже рисунке.

![На снимке экрана показаны рекомендации по производительности в таблице с описанием действия и рекомендации.](./media/database-advisor-find-recommendations-portal/recommendations.png)

Рекомендации сортируются по их возможному влиянию на производительность и делятся на следующие категории:

| Влияние | Описание |
|:--- |:--- |
| Высокий |Рекомендации с высоким уровнем влияния должны обеспечить наиболее значимое изменение производительности. |
| Средний |Рекомендации со средним уровнем влияния должны улучшить производительность, но не существенным образом. |
| Низкий |Рекомендации с низким уровнем влияния должны улучшить производительность, но улучшения не будут значительными. |

> [!NOTE]
> Базе данных SQL Azure требуется отслеживать действия по крайней в течение дня, чтобы определить некоторые рекомендации. База данных SQL Azure может обеспечить более эффективную оптимизацию при стабильном потоке запросов, чем на основе случайных всплесков активности. Если доступных рекомендаций сейчас нет, на странице **Рекомендации по производительности** будет указана причина.

Здесь же можно увидеть состояние последних операций. Выберите рекомендацию или состояние, чтобы просмотреть дополнительные сведения.

Ниже приведен пример рекомендации по созданию индекса в портал Azure.

![Создание индекса](./media/database-advisor-find-recommendations-portal/sql-database-performance-recommendation.png)

## <a name="applying-recommendations"></a>Применение рекомендаций

База данных SQL Azure предоставляет полный контроль над применением рекомендаций с помощью следующих трех параметров:

* применять отдельные рекомендаций по одной;
* включить автоматическую настройку для автоматического применения рекомендаций;
* запустить рекомендованный сценарий T-SQL для выполнения рекомендации в базе данных вручную.

Выберите рекомендацию, чтобы просмотреть ее описание, и щелкните **Показать скрипт**, чтобы узнать, как будет создана рекомендация.

База данных остается в сети, пока применяется рекомендация. Применение рекомендации по производительности или автоматическая настройка никогда не отключает базу данных от сети.

### <a name="apply-an-individual-recommendation"></a>Применение отдельной рекомендации

Рекомендации можно просматривать и применять по одной.

1. На странице **Рекомендации** выберите рекомендацию.
2. На странице **сведения** нажмите кнопку **Применить** .

   ![Применить рекомендацию](./media/database-advisor-find-recommendations-portal/apply.png)

Выбранные рекомендации применяются к базе данных.

### <a name="removing-recommendations-from-the-list"></a>Удаление рекомендаций из списка

Если какие-либо рекомендации в списке нужно удалить, отклоните соответствующую рекомендацию, выполнив описанные ниже действия.

1. Выберите рекомендацию из списка **Рекомендации**, чтобы открыть сведения о ней.
2. Щелкните **Отклонить** на странице **Сведения**.

При необходимости отклоненные рекомендации можно вернуть в список **Рекомендации** , выполнив следующие действия.

1. На странице **Рекомендации** щелкните **Представление отклонено**.
2. Выберите отклоненный элемент из списка, чтобы просмотреть сведения о нем.
3. При необходимости щелкните **Отменить отклонение**, чтобы вернуть индекс обратно в основной список рекомендаций в колонке **Рекомендации**.

> [!NOTE]
> Обратите внимание, что если в службе "База данных SQL" включена [автоматическая настройка](automatic-tuning-overview.md) и вручную отменены рекомендации из списка, такие рекомендации впредь не будут применяться автоматически. Отмена рекомендаций — это удобный способ для пользователей, который позволяет включить автоматическую настройку, если определенная рекомендация не должна применяться.
> Чтобы восстановить настройки, верните отмененную рекомендацию в список, выбрав параметр "Отменить отклонение".

### <a name="enable-automatic-tuning"></a>Включение автоматической настройки

Можно настроить автоматическую реализацию рекомендаций для базы данных. В этом случае появляющиеся рекомендации применяются автоматически. Как и для всех рекомендаций, управляемых службой, если выполнение рекомендации ведет к ухудшению производительности, она отменяется.

1. На странице **Рекомендации** щелкните **Автоматизация**.

   ![Настройки помощника](./media/database-advisor-find-recommendations-portal/settings.png)
2. Выберите действия для автоматизации.

   ![Снимок экрана, на котором показано, где можно выбрать действия для автоматизации.](./media/database-advisor-find-recommendations-portal/server.png)

> [!NOTE]
> Обратите внимание, что функция **DROP_INDEX** в настоящее время несовместима с приложениями, которые используют переключения секций и подсказки индекса.

После выбора соответствующей конфигурации щелкните "Применить".

### <a name="manually-apply-recommendations-through-t-sql"></a>Применение рекомендаций вручную с помощью T-SQL

Выберите рекомендацию и щелкните **Показать скрипт**. Выполните этот сценарий для базы данных, чтобы применить рекомендацию вручную.

*Для индексов, созданных вручную, не выполняется мониторинг и проверка влияния на производительность службы,* поэтому после создания индекса убедитесь в том, что он действительно повысил производительность, и при необходимости измените или удалите этот индекс. Дополнительные сведения о создании индексов см. в статье [CREATE INDEX (Transact-SQL)](https://docs.microsoft.com/sql/t-sql/statements/create-index-transact-sql). Кроме того, примененные вручную рекомендации останутся активными и будут отображаться в списке рекомендаций в течение 24-48 часов. перед тем, как система автоматически отбудет их снять. Если вы хотите удалить рекомендацию раньше, вы можете вручную отменить ее.

### <a name="canceling-recommendations"></a>Отмена рекомендаций

Рекомендации с состоянием **Ожидание**, **Проверка** или **Успешно** можно отменить. Рекомендация с состоянием **Выполняется** не может быть отменена.

1. Выберите рекомендацию в области **Журнал настройки**, чтобы открыть страницу **сведений о рекомендации**.
2. Щелкните **Отменить** , чтобы прервать процесс применения рекомендации.

## <a name="monitoring-operations"></a>Мониторинг операций

Применение рекомендаций может происходить не мгновенно. Портал предоставляет сведения о состоянии рекомендации. Ниже перечислены состояния, в которых может находиться индекс.

| Состояние | Описание |
|:--- |:--- |
| Pending |Команда на применение рекомендации получена и запланирована для выполнения. |
| Выполняется |Идет применение рекомендации. |
| Validating |Рекомендация успешно применена, и служба он оценивает полученные преимущества. |
| Успех |Рекомендация успешно применена, и преимущества были оценены. |
| Ошибка |В процессе применения рекомендации возникла ошибка. Это может быть временной проблемой, либо это может быть результатом изменения схемы, в результате чего скрипт больше не работает. |
| Возврат |Рекомендация была применена, но не дала существенного улучшения и была автоматически отменена. |
| Отмена |Рекомендация была отменена. |

Щелкните внутрипроцессную рекомендацию в списке, чтобы увидеть дополнительные сведения.

![Снимок экрана, на котором показан список рекомендаций в процессе.](./media/database-advisor-find-recommendations-portal/operations.png)

### <a name="reverting-a-recommendation"></a>Отмена рекомендации

Если для применения рекомендации использовалась страница рекомендаций по производительности (то есть вы не запускали сценарий T-SQL вручную), эта рекомендация будет автоматически отменена, если выяснится, что она понизила производительность. Если по какой-то причине рекомендацию необходимо отменить, выполните указанные ниже действия.

1. Выберите успешно примененную рекомендацию в области **Журнал настройки** .
2. Щелкните **Отменить** на странице **сведений о рекомендации**.

![Рекомендованные индексы](./media/database-advisor-find-recommendations-portal/details.png)

## <a name="monitoring-performance-impact-of-index-recommendations"></a>Мониторинг влияния рекомендаций по индексам на производительность

После успешного применения рекомендаций (сейчас доступны только рекомендации по операциям с индексами и параметризации запросов) щелкните **Подробные сведения о запросе** на странице сведений о рекомендации, чтобы отобразить [подробные сведения о производительности запросов](query-performance-insight-use.md) и просмотреть влияние на производительность ваших основных запросов.

![Мониторинг влияния на производительность](./media/database-advisor-find-recommendations-portal/query-insights.png)

## <a name="summary"></a>Итоги

База данных SQL Azure предоставляет рекомендации по повышению производительности базы данных. Благодаря сценариям T-SQL вы получаете помощь в оптимизации базы данных и, таким образом, можете значительно повысить производительность запросов.

## <a name="next-steps"></a>Дальнейшие шаги

Отслеживайте рекомендации и продолжайте применять их для повышения производительности. Рабочие нагрузки базы данных являются динамическими и меняются непрерывно. База данных SQL продолжает отслеживать работу и давать рекомендации, которые могут повысить производительность вашей базы данных.

* Прочитайте раздел [Автоматическая настройка](automatic-tuning-overview.md), чтобы узнать больше об автоматической настройке в Базе данных SQL Azure.
* Общие сведения о рекомендациях по производительности базы данных SQL Azure см. в статье [Помощник по работе с базами данных SQL](database-advisor-implement-performance-recommendations.md).
* См. статью [Анализ производительности запросов базы данных Azure SQL](query-performance-insight-use.md), чтобы узнать о том, как просмотреть влияние на производительность основных запросов.

## <a name="additional-resources"></a>Дополнительные ресурсы

* [Хранилище запросов](https://msdn.microsoft.com/library/dn817826.aspx)
* [CREATE INDEX](https://msdn.microsoft.com/library/ms188783.aspx)
* [Управление доступом Azure на основе ролей (Azure RBAC)](../../role-based-access-control/overview.md)
