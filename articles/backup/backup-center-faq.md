---
title: Центр архивации — часто задаваемые вопросы
description: В этой статье содержатся ответы на часто задаваемые вопросы о центре архивации
ms.topic: conceptual
ms.date: 09/08/2020
ms.openlocfilehash: 7e227eb6a14d06791e52ec33e090afdcb01bab61
ms.sourcegitcommit: 30505c01d43ef71dac08138a960903c2b53f2499
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/15/2020
ms.locfileid: "92094047"
---
# <a name="backup-center---frequently-asked-questions"></a>Центр архивации — часто задаваемые вопросы

## <a name="management"></a>Управление

### <a name="can-backup-center-be-used-across-tenants"></a>Можно ли использовать Центр архивации в клиентах?

Да, если вы используете [Azure лигхсаусе](https://docs.microsoft.com/azure/lighthouse/overview) и имеете делегированный доступ к подпискам в разных клиентах, вы можете использовать центр резервного копирования как единую панель прозрачности для управления резервными копиями в клиентах.

### <a name="can-backup-center-be-used-to-manage-both-recovery-services-vaults-and-backup-vaults"></a>Можно ли использовать Центр архивации для управления хранилищами служб восстановления и резервными хранилищами?

Да, Центр архивации может управлять [хранилищами служб восстановления](https://docs.microsoft.com/azure/backup/backup-azure-recovery-services-vault-overview) и [резервными хранилищами](backup-vault-overview.md).

### <a name="is-there-a-delay-before-data-surfaces-in-backup-center"></a>Существует ли задержка перед поверхностью данных в центре архивации?

Центр архивации предназначен для предоставления информации в режиме реального времени. Между моментом, когда сущность отображается в отдельном окне хранилища, может появиться несколько секунд, а в центре архивации появится время, в котором одна и та же сущность будет отображаться.

## <a name="configuration"></a>Параметр Configuration

### <a name="do-i-need-to-configure-anything-to-see-data-in-backup-center"></a>Нужно ли настраивать содержимое для просмотра данных в центре архивации?

Нет. Центр архивации готов к использованию. Однако для просмотра [отчетов о резервном копировании](https://docs.microsoft.com/azure/backup/configure-reports) в центре архивации необходимо настроить отчеты для своих хранилищ.

### <a name="do-i-need-to-have-any-special-permissions-to-use-backup-center"></a>Требуются ли специальные разрешения для использования центра архивации?

Центр архивации не нуждается в новых разрешениях. Пока у вас есть правильный уровень доступа к Azure RBAC для управляемых ресурсов, можно использовать Центр архивации для этих ресурсов. Например, чтобы просмотреть сведения о резервных копиях, вам потребуется доступ для **чтения** к вашим хранилищам. Чтобы настроить резервное копирование и выполнить другие действия, связанные с резервным копированием, потребуется **участник резервного копирования** или роли **оператора резервного копирования** . Дополнительные сведения о [ролях Azure для Azure Backup](https://docs.microsoft.com/azure/backup/backup-rbac-rs-vault).

## <a name="pricing"></a>Цены

### <a name="do-i-need-to-pay-anything-extra-to-use-backup-explorer"></a>Нужно ли платить что-нибудь еще для использования обозревателя резервного копирования?

В настоящее время нет дополнительных затрат (помимо затрат на резервное копирование) для использования центра архивации. Однако при использовании [отчетов по резервному копированию](https://docs.microsoft.com/azure/backup/configure-reports) в центре архивации взимается плата [за использование](https://azure.microsoft.com/pricing/details/monitor/) журналов Azure Monitor для создания отчетов по резервному копированию.

## <a name="next-steps"></a>Дальнейшие действия

См. другие статьи с вопросами и ответами:

* [Распространенные вопросы о хранилищах служб восстановления](https://docs.microsoft.com/azure/backup/backup-azure-backup-faq)
* [Распространенные вопросы о резервном копировании виртуальных машин Azure](https://docs.microsoft.com/azure/backup/backup-azure-vm-backup-faq)
