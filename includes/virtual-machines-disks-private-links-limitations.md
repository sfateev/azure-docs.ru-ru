---
title: Включить имя файла
description: включить файл
services: virtual-machines
author: roygara
ms.service: virtual-machines
ms.topic: include
ms.date: 03/05/2020
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: 293f0f459e1f1e464fdec16b76eaf08336c92e93
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/25/2020
ms.locfileid: "91376554"
---
- Объект доступа к диску можно связать только с одной виртуальной сетью.
- Для этого виртуальная сеть должна находиться в той же подписке, что и объект доступа к диску.
- Одновременно можно импортировать или экспортировать до 10 дисков или моментальных снимков с использованием одного объекта доступа к диску.
- Вы не можете запросить утверждение вручную для установки связи между виртуальной сетью и объектом доступа к диску.
- Пошаговые моментальные снимки нельзя экспортировать, если они связаны с объектом доступа к диску.
- Вы не можете использовать AzCopy для скачивания VHD диска и (или) моментального снимка, которые защищены приватными каналами к учетной записи хранения. Но вы можете использовать AzCopy, чтобы скачать VHD на свои виртуальные машины.
