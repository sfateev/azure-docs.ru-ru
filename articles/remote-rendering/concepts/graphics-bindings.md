---
title: Графическая привязка
description: Настройка графических привязок и варианты использования.
author: florianborn71
manager: jlyons
services: azure-remote-rendering
titleSuffix: Azure Remote Rendering
ms.author: flborn
ms.date: 12/11/2019
ms.topic: conceptual
ms.service: azure-remote-rendering
ms.custom: devx-track-csharp
ms.openlocfilehash: 332213adf64e17c0935ddf612acac5bbca413a87
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2020
ms.locfileid: "91802299"
---
# <a name="graphics-binding"></a>Графическая привязка

Чтобы иметь возможность использовать Удаленную отрисовку Azure в пользовательском приложении, ее необходимо интегрировать в конвейер отрисовки приложения. За эту интеграцию отвечает графическая привязка.

После настройки графическая привязка обеспечивает доступ к различным функциям, влияющим на отрисованное изображение. Эти функции можно разделить на две категории: общие функции, которые всегда доступны, и специальные функции, которые соответствуют выбранному типу `Microsoft.Azure.RemoteRendering.GraphicsApiType`.

## <a name="graphics-binding-in-unity"></a>Графическая привязка в Unity

В Unity вся привязка обрабатывается структурой `RemoteUnityClientInit`, передаваемой в `RemoteManagerUnity.InitializeManager`. Чтобы задать графический режим, в поле `GraphicsApiType` необходимо задать выбранную привязку. Это поле заполняется автоматически, в зависимости от наличия XRDevice. Это поведение можно переопределить вручную, используя следующие варианты:

* **HoloLens 2**: всегда используется графическая привязка [Windows Mixed Reality](#windows-mixed-reality).
* **Двухмерное классическое приложение UWP**: всегда используется [моделирование](#simulation).
* **Unity Editor**: всегда используется [моделирование](#simulation), если не подключена гарнитура виртуальной реальности WMR. В этом случае служба ARR будет отключена, чтобы сделать возможной отладку частей приложения, не связанных с ARR. Ознакомьтесь также с [голографическим удаленным взаимодействием](../how-tos/unity/holographic-remoting.md).

Еще одной важной частью для Unity является только доступ к [базовой привязке](#access), все остальные разделы, приведенные ниже, можно пропустить.

## <a name="graphics-binding-setup-in-custom-applications"></a>Настройка графической привязки в пользовательских приложениях

Чтобы выбрать графическую привязку, выполните следующие два действия. Во-первых, при инициализации программы графическая привязка должна быть инициализирована статически.

```cs
RemoteRenderingInitialization managerInit = new RemoteRenderingInitialization;
managerInit.graphicsApi = GraphicsApiType.WmrD3D11;
managerInit.connectionType = ConnectionType.General;
managerInit.right = ///...
RemoteManagerStatic.StartupRemoteRendering(managerInit);
```

```cpp
RemoteRenderingInitialization managerInit;
managerInit.graphicsApi = GraphicsApiType::WmrD3D11;
managerInit.connectionType = ConnectionType::General;
managerInit.right = ///...
StartupRemoteRendering(managerInit); // static function in namespace Microsoft::Azure::RemoteRendering
```

Приведенный выше вызов необходим для инициализации Удаленной отрисовки Azure в голографических API. Эта функция должна вызываться перед вызовом какого-либо голографического API и обращением к другим API Удаленной отрисовки. Аналогичным образом соответствующая функция деинициализации `RemoteManagerStatic.ShutdownRemoteRendering();` должна вызываться тогда, когда голографические API уже не вызываются.

## <a name="span-idaccessaccessing-graphics-binding"></a><span id="access">Обращение к графической привязке

После настройки клиента к базовой графической привязке можно обратиться с помощью метода получения `AzureSession.GraphicsBinding`. В качестве примера можно получить статистику последнего кадра, как показано ниже.

```cs
AzureSession currentSession = ...;
if (currentSession.GraphicsBinding)
{
    FrameStatistics frameStatistics;
    if (currentSession.GraphicsBinding.GetLastFrameStatistics(out frameStatistics) == Result.Success)
    {
        ...
    }
}
```

```cpp
ApiHandle<AzureSession> currentSession = ...;
if (ApiHandle<GraphicsBinding> binding = currentSession->GetGraphicsBinding())
{
    FrameStatistics frameStatistics;
    if (*binding->GetLastFrameStatistics(&frameStatistics) == Result::Success)
    {
        ...
    }
}
```

## <a name="graphic-apis"></a>Графические API

В настоящее время можно использовать два графических API, `WmrD3D11` и `SimD3D11`. Третий API, `Headless`, существует, но еще не поддерживается на стороне клиента.

### <a name="windows-mixed-reality"></a>Windows Mixed Reality

`GraphicsApiType.WmrD3D11` — привязка по умолчанию для запуска в HoloLens 2. Создается привязка `GraphicsBindingWmrD3d11`. В этом режиме Удаленная отрисовка Azure подключается непосредственно к голографическим API.

Для доступа к производным графическим привязкам требуется привести базовый `GraphicsBinding`.
Для использования привязки WMR необходимо выполнить две задачи.

#### <a name="inform-remote-rendering-of-the-used-coordinate-system"></a>Указать систему координат для Удаленной отрисовки

```cs
AzureSession currentSession = ...;
IntPtr ptr = ...; // native pointer to ISpatialCoordinateSystem
GraphicsBindingWmrD3d11 wmrBinding = (currentSession.GraphicsBinding as GraphicsBindingWmrD3d11);
if (wmrBinding.UpdateUserCoordinateSystem(ptr) == Result.Success)
{
    ...
}
```

```cpp
ApiHandle<AzureSession> currentSession = ...;
void* ptr = ...; // native pointer to ISpatialCoordinateSystem
ApiHandle<GraphicsBindingWmrD3d11> wmrBinding = currentSession->GetGraphicsBinding().as<GraphicsBindingWmrD3d11>();
if (*wmrBinding->UpdateUserCoordinateSystem(ptr) == Result::Success)
{
    //...
}
```

В примере выше `ptr` должен быть указателем на собственный объект `ABI::Windows::Perception::Spatial::ISpatialCoordinateSystem`, определяющий систему координат в абсолютном пространстве, в которой выражаются координаты в API.

#### <a name="render-remote-image"></a>Отобразить удаленное изображение

В начале каждого кадра Удаленная рамка должна быть подготовлена к просмотру в обратном буфере. Это делается путем вызова метода `BlitRemoteFrame` , который будет заполнять сведения о цвете и глубине для обоих глаз в привязанном к текущей цели отрисовки. Поэтому важно сделать это после привязки полного заднего буфера в качестве целевого объекта отрисовки.

> [!WARNING]
> После того, как удаленный образ был Блит в буфер обмена, локальное содержимое должно быть визуализировано с помощью однопроходного стерео-метода отрисовки, например с помощью **SV_RenderTargetArrayIndex**. Использование других методов рендеринга стерео, таких как отрисовка каждого взгляда в отдельном проходе, может привести к ухудшению производительности или графическим артефактам, поэтому их следует избегать.

```cs
AzureSession currentSession = ...;
GraphicsBindingWmrD3d11 wmrBinding = (currentSession.GraphicsBinding as GraphicsBindingWmrD3d11);
wmrBinding.BlitRemoteFrame();
```

```cpp
ApiHandle<AzureSession> currentSession = ...;
ApiHandle<GraphicsBindingWmrD3d11> wmrBinding = currentSession->GetGraphicsBinding().as<GraphicsBindingWmrD3d11>();
wmrBinding->BlitRemoteFrame();
```

### <a name="simulation"></a>Моделирование

`GraphicsApiType.SimD3D11` — привязка моделирования, и если она выбрана, создается графическая привязка `GraphicsBindingSimD3d11`. Этот интерфейс используется для моделирования движения головы, например в классическом приложении. Он отрисовывает монокулярное изображение.

Чтобы реализовать привязку моделирования, важно понимать разницу между локальной камерой и удаленной рамкой, как описано на странице [Камера](../overview/features/camera.md) .

Требуются две камеры:

* **Локальная Камера**: Эта камера представляет текущую точку камеры, управляемую логикой приложения.
* **Камера прокси-сервера**. Эта камера соответствует текущему *удаленному кадру* , отправленному сервером. Так как между клиентом, запрашивающим кадр, и его поступлением есть задержка времени, *Удаленный кадр* всегда является немедленным за перемещение локальной камеры.

Основной подход заключается в том, что удаленный образ и локальное содержимое подготавливаются к просмотру на экране с помощью камеры прокси-сервера. После этого образ прокси переносится в локальное пространство камеры, что более подробно объясняется в разделе [репроектная стадия](../overview/features/late-stage-reprojection.md).

Его настройка является более сложной и выполняется следующим образом.

#### <a name="create-proxy-render-target"></a>Создание промежуточного целевого объекта отрисовки

Удаленное и локальное содержимое должно быть отрисовано на целевом объекте отрисовки со смещенными значениями цвета и глубины, называемом промежуточным, с использованием данных промежуточной камеры, предоставленных функцией `GraphicsBindingSimD3d11.Update`.

Прокси-сервер должен соответствовать разрешению заднего буфера и должен иметь тип int *DXGI_FORMAT_R8G8B8A8_UNORM* или *DXGI_FORMAT_B8G8R8A8_UNORM* . Когда сеанс готов, перед подключением к нему необходимо вызвать `GraphicsBindingSimD3d11.InitSimulation`.

```cs
AzureSession currentSession = ...;
IntPtr d3dDevice = ...; // native pointer to ID3D11Device
IntPtr color = ...; // native pointer to ID3D11Texture2D
IntPtr depth = ...; // native pointer to ID3D11Texture2D
float refreshRate = 60.0f; // Monitor refresh rate up to 60hz.
bool flipBlitRemoteFrameTextureVertically = false;
bool flipReprojectTextureVertically = false;
GraphicsBindingSimD3d11 simBinding = (currentSession.GraphicsBinding as GraphicsBindingSimD3d11);
simBinding.InitSimulation(d3dDevice, depth, color, refreshRate, flipBlitRemoteFrameTextureVertically, flipReprojectTextureVertically);
```

```cpp
ApiHandle<AzureSession> currentSession = ...;
void* d3dDevice = ...; // native pointer to ID3D11Device
void* color = ...; // native pointer to ID3D11Texture2D
void* depth = ...; // native pointer to ID3D11Texture2D
float refreshRate = 60.0f; // Monitor refresh rate up to 60hz.
bool flipBlitRemoteFrameTextureVertically = false;
bool flipReprojectTextureVertically = false;
ApiHandle<GraphicsBindingSimD3d11> simBinding = currentSession->GetGraphicsBinding().as<GraphicsBindingSimD3d11>();
simBinding->InitSimulation(d3dDevice, depth, color, refreshRate, flipBlitRemoteFrameTextureVertically, flipReprojectTextureVertically);
```

Функции инициализации нужно предоставить указатели на собственное устройство D3D, а также на цвет и глубину текстуры промежуточного целевого объекта отрисовки. После инициализации `AzureSession.ConnectToRuntime` и `DisconnectFromRuntime` можно вызывать несколько раз, но при переключении на другой сеанс необходимо сначала вызвать `GraphicsBindingSimD3d11.DeinitSimulation` в старом сеансе, прежде чем можно будет вызвать `GraphicsBindingSimD3d11.InitSimulation` в другом сеансе.

#### <a name="render-loop-update"></a>Обновление цикла отрисовки

Обновление цикла отрисовки состоит из нескольких этапов.

1. Перед отрисовкой каждого кадра вызывается `GraphicsBindingSimD3d11.Update` с текущим преобразованием камеры, которое передается на сервер для отрисовки. В то же время возвращенное промежуточное преобразование необходимо применить к промежуточной камере, чтобы выполнить отрисовку на промежуточном целевом объекте отрисовки.
Если возвращенное обновление промежуточного преобразования `SimulationUpdate.frameId` имеет значение NULL, удаленные данные пока отсутствуют. В этом случае вместо отрисовки на промежуточном целевом объекте отрисовки любое локальное содержимое должно быть преобразовано для просмотра в заднем буфере с использованием текущих данных камеры, а следующие два шага должны быть пропущены.
1. Теперь приложение должно привязать промежуточный целевой объект отрисовки и вызвать `GraphicsBindingSimD3d11.BlitRemoteFrameToProxy`. Это позволит применить данные удаленного цвета и глубины к промежуточному целевому объекту. Теперь любое локальное содержимое можно будет отобразить на промежуточном целевом объекте, используя преобразование промежуточной камеры.
1. Затем задний буфер нужно будет привязать в качестве целевого объекта отрисовки и вызвать `GraphicsBindingSimD3d11.ReprojectProxy` для представления заднего буфера.

```cs
AzureSession currentSession = ...;
GraphicsBindingSimD3d11 simBinding = (currentSession.GraphicsBinding as GraphicsBindingSimD3d11);
SimulationUpdate update = new SimulationUpdate();
// Fill out camera data with current camera data
...
SimulationUpdate proxyUpdate = new SimulationUpdate();
simBinding.Update(update, out proxyUpdate);
// Is the frame data valid?
if (proxyUpdate.frameId != 0)
{
    // Bind proxy render target
    simBinding.BlitRemoteFrameToProxy();
    // Use proxy camera data to render local content
    ...
    // Bind back buffer
    simBinding.ReprojectProxy();
}
else
{
    // Bind back buffer
    // Use current camera data to render local content
    ...
}
```

```cpp
ApiHandle<AzureSession> currentSession;
ApiHandle<GraphicsBindingSimD3d11> simBinding = currentSession->GetGraphicsBinding().as<GraphicsBindingSimD3d11>();

SimulationUpdate update;
// Fill out camera data with current camera data
...
SimulationUpdate proxyUpdate;
simBinding->Update(update, &proxyUpdate);
// Is the frame data valid?
if (proxyUpdate.frameId != 0)
{
    // Bind proxy render target
    simBinding->BlitRemoteFrameToProxy();
    // Use proxy camera data to render local content
    ...
    // Bind back buffer
    simBinding->ReprojectProxy();
}
else
{
    // Bind back buffer
    // Use current camera data to render local content
    ...
}
```

## <a name="api-documentation"></a>Документирование API

* [C# Ремотеманажерстатик. Стартупремотерендеринг ()](https://docs.microsoft.com/dotnet/api/microsoft.azure.remoterendering.remotemanagerstatic.startupremoterendering)
* [Класс C# Графиксбиндинг](https://docs.microsoft.com/dotnet/api/microsoft.azure.remoterendering.graphicsbinding)
* [Класс C# GraphicsBindingWmrD3d11](https://docs.microsoft.com/dotnet/api/microsoft.azure.remoterendering.graphicsbindingwmrd3d11)
* [Класс C# GraphicsBindingSimD3d11](https://docs.microsoft.com/dotnet/api/microsoft.azure.remoterendering.graphicsbindingsimd3d11)
* [Структура C++ Ремотерендерингинитиализатион](https://docs.microsoft.com/cpp/api/remote-rendering/remoterenderinginitialization)
* [Класс C++ Графиксбиндинг](https://docs.microsoft.com/cpp/api/remote-rendering/graphicsbinding)
* [Класс C++ GraphicsBindingWmrD3d11](https://docs.microsoft.com/cpp/api/remote-rendering/graphicsbindingwmrd3d11)
* [Класс C++ GraphicsBindingSimD3d11](https://docs.microsoft.com/cpp/api/remote-rendering/graphicsbindingsimd3d11)

## <a name="next-steps"></a>Дальнейшие шаги

* [Камера](../overview/features/camera.md)
* [Перепроецирование на позднем этапе](../overview/features/late-stage-reprojection.md)
* [Руководство. Просмотр модели удаленной отрисовки](../tutorials/unity/view-remote-models/view-remote-models.md)
