---
title: Создание входных данных задания служб мультимедиа Azure из локального файла | Документация Майкрософт
description: В этой статье показано, как создать входные данные задания служб мультимедиа Azure из локального файла.
services: media-services
documentationcenter: ''
author: IngridAtMicrosoft
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: how-to
ms.date: 08/31/2020
ms.author: inhenkel
ms.openlocfilehash: 02aaae89f7544392765b46361b1617e0fd7dae8c
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2020
ms.locfileid: "89296813"
---
# <a name="create-a-job-input-from-a-local-file"></a>Создание входных данных задания из локального файла

[!INCLUDE [media services api v3 logo](./includes/v3-hr.md)]

При отправке заданий из Служб мультимедиа версии 3 необходимо указать расположение входного видео. Это видео может храниться как ресурс Служб мультимедиа. В этом случае необходимо создать входной ресурс на основе файла, который хранится локально в хранилище BLOB-объектов Azure. В этой статье показано, как создать входные данные задания из локального файла. Полный пример см. в этом [репозитории GitHub](https://github.com/Azure-Samples/media-services-v3-dotnet-tutorials/blob/master/AMSV3Tutorials/UploadEncodeAndStreamFiles/Program.cs).

## <a name="prerequisites"></a>Предварительные требования 

* [Создание учетной записи Служб мультимедиа](./create-account-howto.md).

## <a name="net-sample"></a>Пример кода .NET

С помощью приведенного ниже кода можно создать входной ресурс и использовать его в качестве входных данных задания. Функция CreateInputAsset выполняет следующие задачи:

* создает ресурс;
* Получает доступный для записи [URL-адрес SAS](../../storage/common/storage-sas-overview.md) [контейнера ресурса в хранилище](../../storage/blobs/storage-quickstart-blobs-dotnet.md#upload-blobs-to-a-container) .
* отправляет файл в контейнер в хранилище через URL-адрес SAS.

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/UploadEncodeAndStreamFiles/Program.cs#CreateInputAsset)]

Следующий фрагмент кода создает выходной ресурс, если он еще не существует:

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/UploadEncodeAndStreamFiles/Program.cs#CreateOutputAsset)]

Следующий фрагмент кода отправляет задание кодирования:

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/UploadEncodeAndStreamFiles/Program.cs#SubmitJob)]

## <a name="job-error-codes"></a>Коды ошибок задания

См. статью о [кодах ошибок](/rest/api/media/jobs/get#joberrorcode).

## <a name="next-steps"></a>Дальнейшие шаги

[Создайте входные данные задания на основе URL-адреса HTTPS](job-input-from-http-how-to.md).
