---
title: Создание субъекта-службы Azure
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 09/01/2020
ms.author: pafarley
ms.openlocfilehash: 5cff103d17138608f4932e36444847437f6da236
ms.sourcegitcommit: eb6bef1274b9e6390c7a77ff69bf6a3b94e827fc
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/05/2020
ms.locfileid: "89321653"
---
## <a name="create-an-azure-service-principal"></a>Создание субъекта-службы Azure

Чтобы приложение взаимодействовало с учетной записью Azure, необходима субъект-служба Azure для управления разрешениями. Выполните инструкции по [созданию субъекта-службы Azure](https://docs.microsoft.com/powershell/azure/create-azure-service-principal-azureps?view=azps-4.4.0&viewFallbackFrom=azps-3.3.0).

При создании субъекта-службы вы увидите, что у него есть значение секрета, идентификатор и идентификатор приложения. Сохраните идентификатор приложения и секрет во временное расположение для последующих шагов.