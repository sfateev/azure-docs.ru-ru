---
title: Отмена связи рабочей области с учетной записью службы автоматизации для Отслеживания изменений и инвентаризации
description: В этой статье объясняется, как отменить связь рабочей области Log Analytics с учетной записью службы автоматизации для Отслеживания изменений и инвентаризации.
services: automation
ms.date: 4/11/2019
ms.topic: conceptual
ms.custom: mvc
ms.openlocfilehash: 2be702ec6e820fe71dd8d2da7aa4cf831b52402e
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2020
ms.locfileid: "83828259"
---
# <a name="unlink-workspace-from-automation-account"></a>Отмена связи рабочей области с учетной записью службы автоматизации

Вы можете отказаться от интеграции учетной записи службы автоматизации с рабочей областью Log Analytics при включении операций функции [Отслеживание изменений и инвентаризация](change-tracking.md). Из этой статьи вы узнаете, как удалить связь между рабочей областью и учетной записью службы автоматизации.

1. Войдите в Azure по адресу https://portal.azure.com.

2. Удалите функцию "Управление обновлениями" для виртуальных машин. Подробные сведения см. в статье [Исключение виртуальных машин из Отслеживания изменений и инвентаризации](automation-remove-vms-from-change-tracking.md).

3. Если функция "Отслеживание изменений и инвентаризация" включает более ранние версии мониторинга SQL Azure, то при ее настройке могли быть созданы ресурсы службы автоматизации, которые следует удалить. Найдите эти ресурсы и удалите их.

4. Откройте страницу учетной записи службы автоматизации и выберите **Связанная рабочая область** в разделе **Связанные ресурсы**.

5. На странице "Отмена связи с рабочей областью" щелкните **Отменить связь с рабочей областью** и следуйте указаниям.

   ![Страница "Отмена связи с рабочей областью"](media/automation-unlink-workspace-change-tracking/automation-unlink-workspace-blade.png).

6. Пока служба автоматизации Azure пытается удалить связь с рабочей областью Log Analytics, вы можете отслеживать ход выполнения этой задачи в меню **Уведомления**.

Кроме того, связь рабочей области Log Analytics с учетной записью службы автоматизации можно удалить прямо из рабочей области:

1. Выберите **Учетная запись службы автоматизации** в разделе **Связанные ресурсы** для рабочей области. 
2. На странице учетной записи службы автоматизации выберите **Удалить связь с учетной записью**.

## <a name="next-steps"></a>Дальнейшие действия

* Дополнительные сведения см. в статье [Управление решением для Отслеживания изменений и инвентаризации](change-tracking-file-contents.md).
* Сведения об устранении проблем общего характера см. в статье [Устранение неполадок с Отслеживанием изменений и инвентаризацией](troubleshoot/change-tracking.md).
