---
title: Библиотеки и пакеты SDK для Plug and Play IoT
description: Сведения о библиотеках устройств и служб, доступных для разработки решений IoT Plug and Play.
author: rido-min
ms.author: rmpablos
ms.date: 07/22/2020
ms.topic: reference
ms.service: iot-pnp
services: iot-pnp
ms.custom: mvc
ms.openlocfilehash: 6ea9440c153e26e36aa17b55c4cb712dd08d4508
ms.sourcegitcommit: 2e72661f4853cd42bb4f0b2ded4271b22dc10a52
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/14/2020
ms.locfileid: "92042683"
---
# <a name="microsoft-sdks-for-iot-plug-and-play"></a>Пакеты SDK для Microsoft Plug and Play IoT

Библиотеки Plug and Play IoT и пакеты SDK позволяют разработчикам создавать решения IoT с помощью различных языков программирования на различных платформах. В следующей таблице приведены ссылки на примеры и краткие руководства, которые помогут вам приступить к работе.

## <a name="device-sdks"></a>Пакеты SDK для устройств

| Язык | Пакет | Репозиторий кода | примеры | Краткое руководство | Справочник |
|---|---|---|---|---|---|
| C-устройство | [vcpkg 1.3.9](https://github.com/Azure/azure-iot-sdk-c/blob/master/doc/setting_up_vcpkg.md) | [GitHub](https://github.com/Azure/azure-iot-sdk-c) | [Примеры](https://github.com/Azure/azure-iot-sdk-c/tree/master/iothub_client/samples/pnp) | [Подключение к Центру Интернета вещей](quickstart-connect-device-c.md) | [Ссылки](/azure/iot-hub/iot-c-sdk-ref/) |
| .NET — устройство | [1.31.0 NuGet](https://www.nuget.org/packages/Microsoft.Azure.Devices.Client) | [GitHub](https://github.com/Azure/azure-iot-sdk-csharp/tree/master/) | [Примеры](https://github.com/Azure-Samples/azure-iot-samples-csharp/tree/master/iot-hub/Samples/device/PnpDeviceSamples) | [Подключение к Центру Интернета вещей](quickstart-connect-device-csharp.md) | [Ссылки](/dotnet/api/microsoft.azure.devices.client?preserve-view=true&view=azure-dotnet) |
| Java — устройство | [Maven 1.26.0](https://mvnrepository.com/artifact/com.microsoft.azure.sdk.iot/iot-device-client) | [GitHub](https://github.com/Azure/azure-iot-sdk-java/tree/master/) | [Примеры](https://github.com/Azure/azure-iot-sdk-java/tree/master/device/iot-device-samples/pnp-device-sample) | [Подключение к Центру Интернета вещей](quickstart-connect-device-java.md) | [Ссылки](/java/api/com.microsoft.azure.sdk.iot.device?preserve-view=true&view=azure-java-stable) |
| Python — устройство | [PIP 2.3.0](https://pypi.org/project/azure-iot-device/) | [GitHub](https://github.com/Azure/azure-iot-sdk-python/tree/master/) | [Примеры](https://github.com/Azure/azure-iot-sdk-python/tree/master/azure-iot-device/samples/pnp) | [Подключение к Центру Интернета вещей](quickstart-connect-device-python.md) | [Ссылки](/python/api/azure-iot-device/azure.iot.device?preserve-view=true&view=azure-python) |
| Узел-устройство | [NPM 1.17.2](https://www.npmjs.com/package/azure-iot-device)  | [GitHub](https://github.com/Azure/azure-iot-sdk-node/tree/master/) | [Примеры](https://github.com/Azure/azure-iot-sdk-node/tree/master/device/samples/pnp) | [Подключение к Центру Интернета вещей](quickstart-connect-device-node.md) | [Ссылки](/javascript/api/azure-iot-device/?preserve-view=true&view=azure-node-latest) |
| Встроенное C-устройство | Недоступно | [GitHub](https://github.com/Azure/azure-sdk-for-c/)| [Примеры](howto-use-embedded-c.md#samples) | [Как использовать внедренный C](howto-use-embedded-c.md) | Н/Д

## <a name="service-sdks"></a>Пакеты SDK службы

| Платформа  | Пакет | Репозиторий кода | примеры | Краткое руководство | Справочник |
|---|---|---|---|---|---|
| .NET — служба центра Интернета вещей | [1.27.1 NuGet](https://www.nuget.org/packages/Microsoft.Azure.Devices ) | [GitHub](https://github.com/Azure/azure-iot-sdk-csharp) | [Примеры](https://github.com/Azure-Samples/azure-iot-samples-csharp/tree/master/iot-hub/Samples/service/PnpServiceSamples) | Н/Д | [Ссылки](/dotnet/api/microsoft.azure.devices?preserve-view=true&view=azure-dotnet) |
| Java — служба центра Интернета вещей | [Maven 1.26.0](https://mvnrepository.com/artifact/com.microsoft.azure.sdk.iot/iot-service-client/1.26.0) | [GitHub](https://github.com/Azure/azure-iot-sdk-java) | [Примеры](https://github.com/Azure/azure-iot-sdk-java/tree/master/service/iot-service-samples/pnp-service-sample) | Н/Д | [Ссылки](/java/api/com.microsoft.azure.sdk.iot.service?preserve-view=true&view=azure-java-stable) |
| Узел — Служба центра Интернета вещей | [NPM 1.13.0](https://www.npmjs.com/package/azure-iothub) | [GitHub](https://github.com/Azure/azure-iot-sdk-node) | [Примеры](https://github.com/Azure/azure-iot-sdk-node/tree/master/service/samples) | Н/Д | [Ссылки](/javascript/api/azure-iothub/?preserve-view=true&view=azure-node-latest) |
| Python — Служба Digital двойников | [PIP 2.2.3](https://pypi.org/project/azure-iot-hub) | [GitHub](https://github.com/Azure/azure-iot-sdk-python) | [Примеры](https://github.com/Azure/azure-iot-sdk-python/tree/master/azure-iot-hub/samples) | [Взаимодействие с API Digital двойников для центра Интернета вещей](quickstart-service-python.md) | Н/Д |
| Узел — Служба Digital двойников | [NPM 1.13.0](https://www.npmjs.com/package/azure-iot-digitaltwins-service) | [GitHub](https://github.com/Azure/azure-iot-sdk-node) | [Примеры](https://github.com/Azure/azure-iot-sdk-node/tree/master/service/samples/javascript) | [Взаимодействие с API Digital двойников для центра Интернета вещей](quickstart-service-node.md) | Недоступно |

## <a name="next-steps"></a>Дальнейшие действия

Чтобы испытать пакеты SDK и библиотеки, ознакомьтесь с  [руководством для разработчиков](concepts-developer-guide-device-csharp.md) и [краткими](quickstart-connect-device-c.md) руководствами и [краткими](quickstart-service-node.md)руководствами по службам.