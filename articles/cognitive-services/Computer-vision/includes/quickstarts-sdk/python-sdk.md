---
title: Краткое руководство. Клиентская библиотека службы "Компьютерное зрение" для Python
description: В этом кратком руководстве описано начало работы с клиентской библиотекой API "Компьютерное зрение" для Python.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: include
ms.date: 12/05/2019
ms.author: pafarley
ms.openlocfilehash: 744ef58b54dc3718c4693ce2aad69024e2d6e2fe
ms.sourcegitcommit: eb6bef1274b9e6390c7a77ff69bf6a3b94e827fc
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/05/2020
ms.locfileid: "89321894"
---
<a name="HOLTop"></a>

[Справочная документация](https://docs.microsoft.com/python/api/azure-cognitiveservices-vision-computervision/azure.cognitiveservices.vision.computervision) | [Исходный код библиотеки](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/cognitiveservices/azure-cognitiveservices-vision-computervision) | [Пакет (PiPy)](https://pypi.org/project/azure-cognitiveservices-vision-computervision/) | [Примеры](https://azure.microsoft.com/resources/samples/?service=cognitive-services&term=vision&sort=0)

## <a name="prerequisites"></a>Предварительные требования

* подписка Azure — [создайте бесплатную учетную запись](https://azure.microsoft.com/free/cognitive-services/).
* [Python 3.x](https://www.python.org/)
* Получив подписку Azure, <a href="https://portal.azure.com/#create/Microsoft.CognitiveServicesComputerVision"  title="создайте ресурс Компьютерного зрения"  target="_blank">create a Computer Vision resource <span class="docon docon-navigate-external x-hidden-focus"></span></a> на портале Azure, чтобы получить ключ и конечную точку. После развертывания щелкните **Перейти к ресурсам**.
    * Для подключения приложения к API Компьютерного зрения потребуется ключ и конечная точка из созданного ресурса. Ключ и конечная точка будут вставлены в приведенный ниже код в кратком руководстве.
    * Используйте бесплатную ценовую категорию (`F0`), чтобы опробовать службу, а затем выполните обновление до платного уровня для рабочей среды.
* [Создайте переменные среды](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account#configure-an-environment-variable-for-authentication) для ключа и URL-адреса конечной точки с именами `COMPUTER_VISION_SUBSCRIPTION_KEY` и `COMPUTER_VISION_ENDPOINT` соответственно.

> [!NOTE]
> Вы можете скачать [полный исходный код для примеров](https://github.com/Azure-Samples/cognitive-services-quickstart-code/blob/master/python/ComputerVision/ComputerVisionQuickstart.py), приведенных ниже и примеров всех функций, доступных в классе [ComputerVisionClient](https://docs.microsoft.com/python/api/azure-cognitiveservices-vision-computervision/azure.cognitiveservices.vision.computervision.computervisionclient?view=azure-python).

## <a name="setting-up"></a>Настройка
 
### <a name="create-a-new-python-application"></a>Создание приложения Python

Создайте скрипт Python с произвольным именем, например&mdash;*quickstart-file.py*. Откройте его в редакторе или интегрированной среде разработки и импортируйте следующие библиотеки.

[!code-python[](~/cognitive-services-quickstart-code/python/ComputerVision/ComputerVisionQuickstart.py?name=snippet_imports)]

Затем создайте переменные для конечной точки и ключа ресурса Azure.

[!code-python[](~/cognitive-services-quickstart-code/python/ComputerVision/ComputerVisionQuickstart.py?name=snippet_vars)]

> [!NOTE]
> Если вы создали переменную среды после запуска приложения, для доступа к переменной следует закрыть и повторно открыть редактор, интегрированную среду разработки или оболочку, где эта переменная была запущена.

### <a name="install-the-client-library"></a>Установка клиентской библиотеки

Клиентскую библиотеку можно установить с помощью следующей команды:

```console
pip install --upgrade azure-cognitiveservices-vision-computervision
```

## <a name="object-model"></a>Объектная модель

Следующие классы и интерфейсы обрабатывают некоторые основные функции пакета SDK Python для Компьютерного зрения.

|Имя|Описание|
|---|---|
|[ComputerVisionClientOperationsMixin](https://docs.microsoft.com/python/api/azure-cognitiveservices-vision-computervision/azure.cognitiveservices.vision.computervision.operations.computervisionclientoperationsmixin?view=azure-python)| Этот класс напрямую обрабатывает все операции с изображениями, такие как анализ изображений, обнаружение текста и создание эскизов.|
| [ComputerVisionClient](https://docs.microsoft.com/python/api/azure-cognitiveservices-vision-computervision/azure.cognitiveservices.vision.computervision.computervisionclient?view=azure-python) | Этот класс требуется для всех функций Компьютерного зрения. Вы создаете его экземпляр с информацией о подписке и используете его для создания экземпляров других классов. Он реализует **ComputerVisionClientOperationsMixin**.|
|[VisualFeatureTypes](https://docs.microsoft.com/python/api/azure-cognitiveservices-vision-computervision/azure.cognitiveservices.vision.computervision.models.visualfeaturetypes?view=azure-python)| Это перечисление определяет различные типы анализа изображений, которые можно выполнить в стандартной операции анализа. Набор значений **VisualFeatureTypes** указывается в зависимости от потребностей. |

## <a name="code-examples"></a>Примеры кода

Эти фрагменты кода показывают, как выполнить следующие действия с помощью клиентской библиотеки службы "Компьютерное зрение" для Python:

* [аутентификация клиента](#authenticate-the-client);
* [Анализ изображения](#analyze-an-image)
* [Чтение печатного и рукописного текста](#read-printed-and-handwritten-text).

## <a name="authenticate-the-client"></a>Аутентификация клиента

> [!NOTE]
> В этом кратком руководстве предполагается, что вы уже [создали переменную среды](../../../cognitive-services-apis-create-account.md#configure-an-environment-variable-for-authentication) для ключа Компьютерного зрения с именем `COMPUTER_VISION_SUBSCRIPTION_KEY`.

Создайте экземпляр клиента с конечной точкой и ключом. Создайте объект [CognitiveServicesCredentials](https://docs.microsoft.com/python/api/msrest/msrest.authentication.cognitiveservicescredentials?view=azure-python) с помощью своего ключа и используйте его с вашей конечной точкой для создания объекта [ComputerVisionClient](https://docs.microsoft.com/python/api/azure-cognitiveservices-vision-computervision/azure.cognitiveservices.vision.computervision.computervisionclient?view=azure-python).

[!code-python[](~/cognitive-services-quickstart-code/python/ComputerVision/ComputerVisionQuickstart.py?name=snippet_client)]

## <a name="analyze-an-image"></a>Анализ изображения

Сохраните ссылку на URL-адрес образа, который необходимо проанализировать.

[!code-python[](~/cognitive-services-quickstart-code/python/ComputerVision/ComputerVisionQuickstart.py?name=snippet_remoteimage)]

### <a name="get-image-description"></a>Получение описания изображения

Следующий код получает список созданных заголовков для изображения. Дополнительные сведения см. в статье [Описание изображений на понятном для пользователя языке](../../concept-describing-images.md).

[!code-python[](~/cognitive-services-quickstart-code/python/ComputerVision/ComputerVisionQuickstart.py?name=snippet_describe)]

### <a name="get-image-category"></a>Получение категории изображения

Следующий код получает обнаруженную категорию изображения. Дополнительные сведения см. в статье [Categorize images by subject matter](../../concept-categorizing-images.md) (Классификация изображений по темам).

[!code-python[](~/cognitive-services-quickstart-code/python/ComputerVision/ComputerVisionQuickstart.py?name=snippet_categorize)]

### <a name="get-image-tags"></a>Получение тегов изображения

Следующий код получает набор обнаруженных тегов изображения. Дополнительные сведения см. в статье о [тегах содержимого](../../concept-tagging-images.md).

[!code-python[](~/cognitive-services-quickstart-code/python/ComputerVision/ComputerVisionQuickstart.py?name=snippet_tags)]

### <a name="detect-objects"></a>Обнаружение объектов

Следующий код обнаруживает общие объекты в образе и выводит их на консоль. Дополнительные сведения см. в статье [Обнаружение объектов](../../concept-object-detection.md).

[!code-python[](~/cognitive-services-quickstart-code/python/ComputerVision/ComputerVisionQuickstart.py?name=snippet_objects)]

### <a name="detect-brands"></a>Обнаружение торговых марок

Следующий код обнаруживает фирменные торговые марки и логотипы в образе и выводит их на консоль. Дополнительные сведения см. в статье [Обнаружение торговых марок](../../concept-brand-detection.md).

[!code-python[](~/cognitive-services-quickstart-code/python/ComputerVision/ComputerVisionQuickstart.py?name=snippet_brands)]

### <a name="detect-faces"></a>Распознавание лиц

Следующий код возвращает обнаруженные лица на изображении с их координатами прямоугольника и атрибутами выбора лиц. Дополнительные сведения см. в статье [Определение лиц с помощью компьютерного зрения](../../concept-detecting-faces.md).

[!code-python[](~/cognitive-services-quickstart-code/python/ComputerVision/ComputerVisionQuickstart.py?name=snippet_faces)]

### <a name="detect-adult-racy-or-gory-content"></a>Обнаружение содержимого для взрослых, непристойного или жуткого содержимого

Следующий код выводит сведения об обнаружении содержимого для взрослых на изображении. Дополнительные сведения см. в статье [Detect adult content](../../concept-detecting-adult-content.md) (Обнаружение содержимого для взрослых).

[!code-python[](~/cognitive-services-quickstart-code/python/ComputerVision/ComputerVisionQuickstart.py?name=snippet_adult)]

### <a name="get-image-color-scheme"></a>Получение цветовой схемы изображения

Следующий код выводит обнаруженные атрибуты цвета на изображении, например доминирующие и акцентные цвета. Дополнительные сведения см. в статье [Обнаружение цветовых схем на изображениях](../../concept-detecting-color-schemes.md).

[!code-python[](~/cognitive-services-quickstart-code/python/ComputerVision/ComputerVisionQuickstart.py?name=snippet_color)]

### <a name="get-domain-specific-content"></a>Получение содержимого, связанного с определенной предметной областью

Компьютерное зрение может использовать специализированную модель для дальнейшего анализа изображений. Дополнительные сведения см. в статье [Обнаружение содержимого, связанного с определенными предметными областями](../../concept-detecting-domain-content.md). 

Следующий код анализирует данные об обнаруженных известных лицах на изображении.

[!code-python[](~/cognitive-services-quickstart-code/python/ComputerVision/ComputerVisionQuickstart.py?name=snippet_celebs)]

Следующий код анализирует данные об обнаруженных достопримечательностях на изображении.

[!code-python[](~/cognitive-services-quickstart-code/python/ComputerVision/ComputerVisionQuickstart.py?name=snippet_landmarks)]

### <a name="get-the-image-type"></a>Получение сведений о типе изображения

Следующий код выводит информацию о типе изображения&mdash;, независимо от того, является ли это картинкой или рисунком.

[!code-python[](~/cognitive-services-quickstart-code/python/ComputerVision/ComputerVisionQuickstart.py?name=snippet_type)]

## <a name="read-printed-and-handwritten-text"></a>Чтение печатного и рукописного текста

Компьютерное зрение может считывать видимый текст в образе и преобразовывать его в поток символов. Это необходимо выполнять двумя частями.

### <a name="call-the-read-api"></a>Вызов API чтения

Сначала выполните следующий код, чтобы вызвать метод **read** для предоставленного изображения. Он возвращает идентификатор операции и запускает асинхронный процесс чтения содержимого образа.

[!code-python[](~/cognitive-services-quickstart-code/python/ComputerVision/ComputerVisionQuickstart.py?name=snippet_read_call)]

### <a name="get-read-results"></a>Получение результатов чтения

Затем получите идентификатор операции, возвращенный из вызова **read**, и получите от службы результаты операции по этому идентификатору. Следующий код проверяет операцию с интервалами в одну секунду, пока не будут возвращены результаты. После этого извлеченные текстовые данные выводятся на консоль.

[!code-python[](~/cognitive-services-quickstart-code/python/ComputerVision/ComputerVisionQuickstart.py?name=snippet_read_response)]

## <a name="run-the-application"></a>Выполнение приложения

Запустите приложение, выполнив команду `python` для файла quickstart.

```console
python quickstart-file.py
```

## <a name="clean-up-resources"></a>Очистка ресурсов

Если вы хотите очистить и удалить подписку Cognitive Services, вы можете удалить ресурс или группу ресурсов. При этом удаляются все ресурсы, связанные с ней.

* [Портал](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account#clean-up-resources)
* [Azure CLI](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account-cli#clean-up-resources)


## <a name="next-steps"></a>Дальнейшие действия

Изучив это краткое руководство, вы узнали, как с помощью библиотеки службы "Компьютерное зрение" для Python выполнять базовые задачи. Далее ознакомьтесь со справочной документацией, чтобы узнать больше о библиотеке.


> [!div class="nextstepaction"]
>[Справочник по API службы "Компьютерное зрение" (Python)](https://docs.microsoft.com/python/api/azure-cognitiveservices-vision-computervision/azure.cognitiveservices.vision.computervision)

* [Что собой представляет Компьютерное зрение](../../overview.md)
* Исходный код для этого шаблона можно найти на портале [GitHub](https://github.com/Azure-Samples/cognitive-services-quickstart-code/blob/master/python/ComputerVision/ComputerVisionQuickstart.py).
