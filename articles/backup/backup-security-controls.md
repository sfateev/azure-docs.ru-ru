---
title: Средства управления безопасностью
description: Сведения об элементах управления безопасностью, используемых в службе Azure Backup. Эти элементы управления помогают службе предотвращать, обнаруживать и отвечать на уязвимости системы безопасности.
ms.topic: conceptual
ms.date: 09/23/2019
ms.openlocfilehash: 96899c0669f3063232c36ad3ae1fec76a90e0a5c
ms.sourcegitcommit: 30505c01d43ef71dac08138a960903c2b53f2499
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/15/2020
ms.locfileid: "92090868"
---
# <a name="security-controls-for-azure-backup"></a>Элементы управления безопасностью для Azure Backup

В этой статье описываются элементы управления безопасностью, встроенные в Azure Backup.

[!INCLUDE [Security controls Header](../../includes/security-controls-header.md)]

## <a name="network"></a>Сеть

| Управление безопасностью | Да/нет | Примечания | Документация
|---|---|--|--|
| Поддержка конечных точек службы| Нет |  |  |
| Поддержка внедрения виртуальной сети| Нет |  |  |
| Поддержка сетевой изоляции и брандмауэров| Да | |  |
| Поддержка принудительного туннелирования для виртуальных машин Azure | Да  |  |  |
| Поддержка принудительного туннелирования для приложений, выполняющихся на виртуальных машинах Azure| Нет  |  |  |

## <a name="monitoring--logging"></a>Мониторинг & ведения журнала

| Управление безопасностью | Да/нет | Примечания| Документация
|---|---|--|--|
| Поддержка мониторинга Azure (например, log Analytics, App Insights)| Да | Log Analytics поддерживается через журналы ресурсов. Дополнительные сведения см. в статье [мониторинг Azure Backup защищенных рабочих нагрузок с помощью log Analytics](backup-azure-diagnostics-mode-data-model.md). |  |
| Ведение журнала и аудит в плоскости управления и управления| Да | Все действия, активированные клиентом на портале Azure, регистрируются в журналах действий. |  |
| Ведение журнала и аудит в плоскости данных| Нет | К плоскости данных Azure Backup невозможно обратиться напрямую.  |  |

## <a name="identity"></a>Идентификация

| Управление безопасностью | Да/нет | Примечания| Документация
|---|---|--|--|
| Аутентификация| Да | Аутентификация выполняется с помощью Azure Active Directory. |  |
| Авторизация| Да | Используются созданные клиентом и встроенные роли Azure. Дополнительные сведения см. [в статье Использование управления доступом на основе ролей в Azure для управления Azure Backup точек восстановления](./backup-rbac-rs-vault.md). |  |

## <a name="data-protection"></a>Защита данных

| Управление безопасностью | Да/нет | Примечания | Документация
|---|---|--|--|
| Шифрование неактивных на стороне сервера: ключи, управляемые корпорацией Майкрософт | Да | Использование шифрования службы хранилища для учетных записей хранения. |  |
| Шифрование неактивных на стороне сервера: ключи, управляемые клиентом (BYOK) | Нет |  |  |
| Шифрование на уровне столбцов (службы данных Azure)| Нет |  |  |
| Шифрование при передаче (например, шифрование ExpressRoute, Шифрование виртуальной сети и шифрование VNet-VNet);| Нет | С использованием HTTPS. |  |
| Вызовы API в зашифрованном виде| Да |  |  |

## <a name="configuration-management"></a>Управление конфигурацией

| Управление безопасностью | Да/нет | Примечания| Документация
|---|---|--|--|
| Поддержка управления конфигурацией (управление версиями конфигураций и т. д.)| Да|  |  |

## <a name="next-steps"></a>Дальнейшие действия

- Дополнительные сведения о [встроенных средствах управления безопасностью в службах Azure](../security/fundamentals/security-controls.md).
