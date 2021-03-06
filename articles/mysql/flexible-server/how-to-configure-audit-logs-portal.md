---
title: Настройка журналов аудита — портал Azure. база данных Azure для MySQL — гибкий сервер
description: В этой статье описывается, как настроить и получить доступ к журналам аудита в базе данных Azure для гибкого сервера MySQL из портал Azure.
author: ajlam
ms.author: andrela
ms.service: mysql
ms.topic: how-to
ms.date: 9/29/2020
ms.openlocfilehash: cadd384bc7d3222cbc5d1179271f7b4624c9224a
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2020
ms.locfileid: "91565893"
---
# <a name="configure-and-access-audit-logs-for-azure-database-for-mysql---flexible-server-using-the-azure-portal"></a>Настройка и доступ к журналам аудита для базы данных Azure для MySQL — гибкого сервера с помощью портал Azure

> [!IMPORTANT]
> Сейчас предоставляется общедоступная предварительная версия Гибкого сервера Базы данных Azure для MySQL.

Вы можете настроить для базы данных Azure для гибких [журналов аудита](concepts-audit-logs.md) сервера MySQL и параметров диагностики из портал Azure.

## <a name="prerequisites"></a>Предварительные требования
Для выполнения действий, описанных в этой статье, требуется [гибкий сервер](quickstart-create-server-portal.md).

## <a name="configure-audit-logging"></a>Настройка ведения журнала аудита

>[!IMPORTANT]
> Рекомендуется регистрировать только те типы событий и пользователей, которые необходимы для целей аудита, чтобы гарантировать, что производительность сервера не окажется сильной.

Включение и Настройка ведения журнала аудита.

1. Войдите на [портал Azure](https://portal.azure.com/).

1. Выберите гибкий сервер.

1. В разделе **Параметры** на боковой панели выберите **Параметры сервера**.
    :::image type="content" source="./media/how-to-configure-audit-logs-portal/server-parameters.png" alt-text="Параметры сервера":::

1. Обновите параметр **audit_log_enabled** в значение ON.
    :::image type="content" source="./media/how-to-configure-audit-logs-portal/audit-log-enabled.png" alt-text="Параметры сервера":::

1. Выберите [типы событий](concepts-audit-logs.md#configure-audit-logging) для записи, обновив параметр **audit_log_events** .
    :::image type="content" source="./media/how-to-configure-audit-logs-portal/audit-log-events.png" alt-text="Параметры сервера":::

1. Добавьте пользователей MySQL, которые будут включены или исключены из ведения журнала, обновив параметры **audit_log_exclude_users** и **audit_log_include_users** . Укажите пользователей, указав имя пользователя MySQL.
    :::image type="content" source="./media/how-to-configure-audit-logs-portal/audit-log-exclude-users.png" alt-text="Параметры сервера":::

1. После изменения параметров нажмите кнопку **Сохранить**. Также вы можете нажать кнопку **Отменить**, чтобы отменить изменения.
    :::image type="content" source="./media/how-to-configure-audit-logs-portal/save-parameters.png" alt-text="Параметры сервера":::

## <a name="set-up-diagnostics"></a>Настройка диагностики

Журналы аудита интегрируются с Azure Monitor параметрами диагностики, что позволяет передавать журналы в Azure Monitor журналы, концентраторы событий или службу хранилища Azure.

1. В разделе **мониторинг** на боковой панели выберите **параметры диагностики**.

1. Щелкните "+ добавить параметр диагностики"  :::image type="content" source="./media/how-to-configure-audit-logs-portal/add-diagnostic-setting.png" alt-text="Параметры сервера":::

1. Укажите имя параметра диагностики.

1. Укажите назначения для отправки журналов аудита (учетную запись хранения, концентратор событий и (или) рабочую область Log Analytics).

1. В качестве типа журнала выберите **мисклаудитлогс** .
    :::image type="content" source="./media/how-to-configure-audit-logs-portal/configure-diagnostic-setting.png" alt-text="Параметры сервера":::

1. После настройки приемников данных для передачи журналов аудита в можно нажать кнопку **сохранить**.
    :::image type="content" source="./media/how-to-configure-audit-logs-portal/save-diagnostic-setting.png" alt-text="Параметры сервера":::

1. Получите доступ к журналам аудита, просмотрев их в настроенных приемниках данных. Для отображения журналов может потребоваться до 10 минут.

Если вы передаете журналы аудита в журналы Azure Monitor (Log Analytics), ознакомьтесь с [примерами запросов](concepts-audit-logs.md#analyze-logs-in-azure-monitor-logs) , которые можно использовать для анализа.  

## <a name="next-steps"></a>Дальнейшие шаги

- Дополнительные сведения о [журналах аудита](concepts-audit-logs.md)
- Сведения о [журналах запросов с высокой скоростью](concepts-slow-query-logs.md)
<!-- - Learn how to configure audit logs in the [Azure CLI](howto-configure-audit-logs-cli.md)-->