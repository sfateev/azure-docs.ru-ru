---
title: Анализ видеотрансляции с помощью собственной модели HTTP — Azure
description: В этом кратком руководстве показано, как с помощью компьютерного зрения выполнить анализ потока видеотрансляции IP-камеры (имитированной), используя собственную модель HTTP.
ms.topic: quickstart
ms.date: 04/27/2020
zone_pivot_groups: ams-lva-edge-programming-languages
ms.openlocfilehash: 07661fb1e9496aacff6f108a840e0c357f068d41
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/25/2020
ms.locfileid: "91336468"
---
# <a name="quickstart-analyze-live-video-by-using-your-own-http-model"></a>Краткое руководство. Анализ видеотрансляции с помощью собственной модели HTTP

В этом кратком руководстве показано, как использовать Аналитику видеотрансляций в службе IoT Edge для анализа потока видеотрансляции из (имитированной) IP-камеры. Вы узнаете, как применить модель компьютерного зрения для обнаружения объектов. Подмножество кадров в потоке видео в реальном времени отправляется в службу вывода. Результаты отправляются в центр IoT Edge. 

В этом кратком руководстве используется имитация видеотрансляции, а в качестве устройства IoT Edge используется виртуальная машина Azure. Оно основывается на образце кода, написанном на C# для краткого руководства [Создание событий при обнаружении движения](detect-motion-emit-events-quickstart.md). 

::: zone pivot="programming-language-csharp"
[!INCLUDE [header](includes/analyze-live-video-your-http-model-quickstart/csharp/header.md)]
::: zone-end

::: zone pivot="programming-language-python"
[!INCLUDE [header](includes/analyze-live-video-your-http-model-quickstart/python/header.md)]
::: zone-end

## <a name="prerequisites"></a>Предварительные требования

::: zone pivot="programming-language-csharp"
[!INCLUDE [prerequisites](includes/analyze-live-video-your-http-model-quickstart/csharp/prerequisites.md)]
::: zone-end

::: zone pivot="programming-language-python"
[!INCLUDE [prerequisites](includes/analyze-live-video-your-http-model-quickstart/python/prerequisites.md)]
::: zone-end

## <a name="review-the-sample-video"></a>Просмотр примера видео

::: zone pivot="programming-language-csharp"
[!INCLUDE [review-sample-video](includes/analyze-live-video-your-http-model-quickstart/csharp/review-sample-video.md)]
::: zone-end

::: zone pivot="programming-language-python"
[!INCLUDE [review-sample-video](includes/analyze-live-video-your-http-model-quickstart/python/review-sample-video.md)]
::: zone-end

## <a name="overview"></a>Обзор

::: zone pivot="programming-language-csharp"
[!INCLUDE [overview](includes/analyze-live-video-your-http-model-quickstart/csharp/overview.md)]
::: zone-end

::: zone pivot="programming-language-python"
[!INCLUDE [overview](includes/analyze-live-video-your-http-model-quickstart/python/overview.md)]
::: zone-end

## <a name="create-and-deploy-the-media-graph"></a>Создание и развертывание графа мультимедиа

::: zone pivot="programming-language-csharp"
[!INCLUDE [create-deploy-media-graph](includes/analyze-live-video-your-http-model-quickstart/csharp/create-deploy-media-graph.md)]
::: zone-end

::: zone pivot="programming-language-python"
[!INCLUDE [create-deploy-media-graph](includes/analyze-live-video-your-http-model-quickstart/python/create-deploy-media-graph.md)]
::: zone-end

## <a name="interpret-results"></a>Интерпретация результатов

::: zone pivot="programming-language-csharp"
[!INCLUDE [interpret-results](includes/analyze-live-video-your-http-model-quickstart/csharp/interpret-results.md)]
::: zone-end

::: zone pivot="programming-language-python"
[!INCLUDE [interpret-results](includes/analyze-live-video-your-http-model-quickstart/python/interpret-results.md)]
::: zone-end

## <a name="clean-up-resources"></a>Очистка ресурсов

Если вы планируете работать с другими краткими руководствами, сохраните созданные ресурсы. В противном случае на портале Azure перейдите к группам ресурсов, выберите группу ресурсов, с которой вы работали в этом кратком руководстве, и удалите все ресурсы.

## <a name="next-steps"></a>Дальнейшие действия

* Попробуйте [защищенную версию модели YOLOv3](https://github.com/Azure/live-video-analytics/blob/master/utilities/video-analysis/tls-yolov3-onnx/readme.md) и разверните ее на устройстве IoT Edge. 

Ознакомьтесь с дополнительными задачами для опытных пользователей.

* Вместо симулятора RTSP используйте [IP-камеру](https://en.wikipedia.org/wiki/IP_camera) с поддержкой RTSP. IP-камеры, поддерживающие протокол RTSP, можно найти на странице [продуктов, соответствующих ONVIF](https://www.onvif.org/conformant-products/). Ищите устройства, которые соответствуют профилям G, S или T.
* Используйте устройство AMD64 или x64 на базе Linux вместо виртуальной машины Linux в Azure. Устройство должно находиться в той же сети, что и IP-камера. Вы можете выполнить инструкции из статьи [Install Azure IoT Edge runtime on Linux](../../iot-edge/how-to-install-iot-edge-linux.md) (Установка среды выполнения Azure IoT Edge в системах Linux на основе Debian). Чтобы зарегистрировать устройство в Центре Интернета вещей Azure, следуйте инструкциям краткого руководства [Развертывание первого модуля IoT Edge на виртуальном устройстве Linux](../../iot-edge/quickstart-linux.md).
