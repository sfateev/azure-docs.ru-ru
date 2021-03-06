---
title: Журналы — масштабирование (Цитус) — база данных Azure для PostgreSQL
description: Как получить доступ к журналам базы данных Azure для PostgreSQL-Scale (Цитус)
author: jonels-msft
ms.author: jonels
ms.service: postgresql
ms.subservice: hyperscale-citus
ms.topic: conceptual
ms.date: 7/13/2020
ms.openlocfilehash: 1dc7bc8e119de7c8fdcf09713286be2633457486
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2020
ms.locfileid: "90895861"
---
# <a name="logs-in-azure-database-for-postgresql---hyperscale-citus"></a>Журналы в базе данных Azure для PostgreSQL — масштабирование (Цитус)

Журналы PostgreSQL доступны на каждом узле группы серверов "горизонтальное масштабирование (Цитус)". Журналы можно поставлять на сервер хранилища или в службу аналитики. Журналы могут использоваться для обнаружения, устранения и исправления ошибок конфигурации и неоптимальной производительности.

## <a name="accessing-logs"></a>Доступ к журналам

Чтобы получить доступ к журналам PostgreSQL для координатора Цитус или рабочего узла, откройте узел в портал Azure:

:::image type="content" source="media/howto-hyperscale-logging/choose-node.png" alt-text="список узлов":::

Для выбранного узла откройте **параметры диагностики**и щелкните **+ Добавить параметр диагностики**.

:::image type="content" source="media/howto-hyperscale-logging/diagnostic-settings.png" alt-text="список узлов":::

Выберите имя для новых параметров диагностики и установите флажок **постгрескллогс** .  Выберите назначения, которые должны получить журналы.

:::image type="content" source="media/howto-hyperscale-logging/diagnostic-create-setting.png" alt-text="список узлов":::

## <a name="next-steps"></a>Дальнейшие шаги

- [Приступая к работе с запросами log Analytics](/azure/azure-monitor/log-query/get-started-portal)
- Сведения о [концентраторах событий Azure](/azure/event-hubs/event-hubs-about)
