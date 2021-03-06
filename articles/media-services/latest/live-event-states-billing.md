---
title: Состояния LiveEvent и выставление счетов в Службах мультимедиа Azure | Документация Майкрософт
description: В этой статье представлены общие сведения о состояниях компонента LiveEvent и выставлении счетов за его использование.
services: media-services
documentationcenter: ''
author: IngridAtMicrosoft
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: ne
ms.topic: conceptual
ms.date: 08/31/2020
ms.author: inhenkel
ms.openlocfilehash: 1b058eefe22238b60c3482c55b5ae340f4e597f0
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2020
ms.locfileid: "89296742"
---
# <a name="live-event-states-and-billing"></a>Состояния события потоковой трансляции и выставление счетов

[!INCLUDE [media services api v3 logo](./includes/v3-hr.md)]

В Службах мультимедиа Azure начисление платы за использование события потоковой трансляции начинается, как только оно переходит в состояние **Выполняется**. Плата будет взиматься, даже если нет видео, передаваемых через службу. Чтобы остановить начисление платы за использование события потоковой трансляции, его нужно остановить. Счет на запись в реальном времени выставляется так же, как в интерактивном событии.

Когда **лививентенкодингтипе** в [прямом эфире](/rest/api/media/liveevents) имеет значение Standard или Premium1080p, службы мультимедиа по-прежнему завершают любое активное событие, которое еще находится в **работающем** состоянии 12 часов после того, как входной канал будет потерян, а **выходные данные**не запущены. Но при этом вам будет начислена плата за все время, пока событие потоковой трансляции находилось в состоянии **Выполняется**.

> [!NOTE]
> Передаваемые Интерактивные события не завершаются автоматически и должны быть явно остановлены через API, чтобы избежать чрезмерного выставления счетов. 

## <a name="states"></a>Состояния

Событие потоковой трансляции может находиться в одном из указанных ниже состояний.

|Состояние|Описание|
|---|---|
|**Остановлена**| Это начальное состояние события прямой трансляции после создания (если для параметра автозапуска не задано значение true). В этом состоянии выставление счетов не выполняется. В этом состоянии можно изменять свойства события потоковой трансляции, но потоковая передача невозможна.|
|**Запуск**| Событие потоковой трансляции запускается, и выделяются ресурсы. В этом состоянии начисление оплаты не происходит. В этом состоянии обновление и потоковая передача запрещены. Если возникает ошибка, событие потоковой трансляции возвращается в состояние "Остановлено".|
|**Выполнение**| Ресурсы события потоковой трансляции выделены, URL-адреса приема и предварительного просмотра созданы, и компонент может получать потоки в реальном времени. На этом этапе начисление оплаты активно. Чтобы остановить начисление оплаты, нужно явно вызвать функцию Stop (Остановить) для ресурса события потоковой трансляции.|
|**Остановка**| Работа события потоковой трансляции приостанавливается, и выделенные ресурсы автоматически освобождаются. В этом переходном состоянии оплата не начисляется. В этом состоянии обновление и потоковая передача запрещены.|
|**Удаление**| Событие потоковой трансляции удаляется. В этом переходном состоянии оплата не начисляется. В этом состоянии обновление и потоковая передача запрещены.|

Вы можете включить динамическую транскрипцию при создании события Live. Если вы сделаете это, плата будет взиматься за Интерактивные записи, когда событие в реальном времени находится в состоянии **выполняется** . Обратите внимание, что плата будет взиматься, даже если нет звука, передаваемых через событие Live.

## <a name="next-steps"></a>Дальнейшие шаги

- [Общие сведения о потоковой трансляции](live-streaming-overview.md)
- [Руководство по потоковой трансляции](stream-live-tutorial-with-api.md)
