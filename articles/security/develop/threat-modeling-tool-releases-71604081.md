---
title: Microsoft Threat Modeling Tool выпуск 4/9/2019
titleSuffix: Azure
description: Документирование заметок о выпуске для 7.1.60408.1 выпуска средства моделирования угроз.
author: jegeib
ms.author: jegeib
ms.service: security
ms.subservice: security-develop
ms.topic: article
ms.date: 04/03/2019
ms.openlocfilehash: 8d24eb868a13e7b2d9ff3d2b625302c1c1fcb908
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2020
ms.locfileid: "91318013"
---
# <a name="threat-modeling-tool-update-release-71604081---492019"></a>Threat Modeling Tool обновления выпуска 7.1.60408.1 — 4/9/2019

Версия 7.1.60408.1 Microsoft Threat Modeling Tool (ТМТ) была выпущена 9 2019 апреля и содержит следующие изменения:

- Новые наборы элементов для Azure Key Vault и диспетчера трафика Azure
- Номер версии ТМТ теперь отображается на начальном экране
- Ссылки на поддержку обновлены
- Исправления ошибок

## <a name="feature-changes"></a>Изменения функций

### <a name="new-stencils-for-azure-key-vault-and-azure-traffic-manager"></a>Новые наборы элементов для Azure Key Vault и диспетчера трафика Azure

![Набор элементов Azure Key Vault](./media/threat-modeling-tool-releases-71604081/tmt_keyvault_trafficmanager.PNG)

В набор трафаретов Azure добавлены новые наборы элементов и угрозы для Azure Key Vault и диспетчера трафика Azure. При открытии моделей, основанных на наборе элементов Azure, пользователям будет предложено обновить шаблон, связанный с моделью. Обновление модели на основе набора элементов Azure также может инициироваться вручную с помощью команды "применить шаблон" в меню "файл" и повторного применения последнего файла облачных служб Azure. tb7.

### <a name="tmt-version-number-is-now-shown-on-the-home-screen"></a>Номер версии ТМТ теперь отображается на начальном экране

Клиентская версия Threat Modeling Tool теперь отображается на начальном экране приложения для простоты доступа.

![Набор элементов Azure Key Vault](./media/threat-modeling-tool-releases-71604081/tmt_version.PNG)

### <a name="support-links-have-been-updated"></a>Ссылки на поддержку обновлены

Все ссылки на поддержку в средстве были обновлены для направления пользователей в, [tmtextsupport@microsoft.com](mailto:tmtextsupport@microsoft.com) а не на форуме MSDN.

## <a name="system-requirements"></a>Требования к системе

- Поддерживаемые операционные системы
  - [Юбилейное обновление Microsoft Windows 10](https://blogs.windows.com/windowsexperience/2016/08/02/how-to-get-the-windows-10-anniversary-update/#HTkoK5Zdv0g2F2Zq.97) или более поздняя версия
- Требуемая версия .NET
  - [.NET 4.7.1](https://go.microsoft.com/fwlink/?LinkId=863262) или более поздней версии
- Дополнительные требования
  - Для получения обновлений средства и шаблонов требуется подключение к Интернету.

## <a name="documentation-and-feedback"></a>Документация и отзывы

- Документация по Threat Modeling Tool находится на сайте [docs.microsoft.com](threat-modeling-tool.md) и содержит информацию [об использовании средства](threat-modeling-tool-getting-started.md).

## <a name="next-steps"></a>Дальнейшие шаги

Скачайте последнюю версию [Microsoft Threat Modeling Tool](https://aka.ms/threatmodelingtool).
