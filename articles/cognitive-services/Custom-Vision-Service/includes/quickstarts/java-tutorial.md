---
author: areddish
ms.custom: devx-track-java
ms.author: areddish
ms.service: cognitive-services
ms.date: 09/15/2020
ms.openlocfilehash: f4b64c25bff93683c79aa1aa3eb556988c798cb0
ms.sourcegitcommit: 80b9c8ef63cc75b226db5513ad81368b8ab28a28
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/16/2020
ms.locfileid: "90604994"
---
Это руководство содержит инструкции и пример кода, которые помогут вам приступить к работе с клиентской библиотекой службы "Пользовательское визуальное распознавание" для Java и создать модель классификации изображений. Здесь объясняется, как создать проект, добавить теги, обучить проект и использовать URL-адрес конечной точки прогнозирования проекта для программного тестирования. Этот пример можно использовать как шаблон при создании своего приложения для распознавания изображений.

> [!NOTE]
> Если вы хотите создать и обучить модель классификации _без_ написания кода, см. руководство по [работе со средствами на основе браузера](../../getting-started-build-a-classifier.md).

## <a name="prerequisites"></a>Предварительные требования

- Любая среда разработки Java.
- [JDK 7 или 8](https://aka.ms/azure-jdks) установлены.
- Установленный [Maven](https://maven.apache.org/).
- [!INCLUDE [create-resources](../../includes/create-resources.md)]

## <a name="get-the-custom-vision-client-library"></a>Получение клиентской библиотеки службы "Пользовательское визуальное распознавание"

Чтобы написать приложение для анализа изображений с помощью службы "Пользовательское визуальное распознавание" для Java, вам потребуются пакеты Maven для этой службы. Эти пакеты включены в скачиваемый пример проекта, но будут доступны по отдельности здесь.

Клиентскую библиотеку Пользовательского визуального распознавания можно найти в репозитории Maven:

- [Пакет SDK для обучения](https://mvnrepository.com/artifact/com.microsoft.azure.cognitiveservices/azure-cognitiveservices-customvision-training)
- [Пакет SDK для прогнозирования](https://mvnrepository.com/artifact/com.microsoft.azure.cognitiveservices/azure-cognitiveservices-customvision-prediction)

Клонируйте или скачайте проект с [примерами пакета SDK для Java в Cognitive Services](https://github.com/Azure-Samples/cognitive-services-java-sdk-samples/tree/master). Перейдите в папку **Vision/CustomVision/**.

Этот проект Java создает проект классификации изображений Пользовательской службы визуального распознавания с именем __Пример проекта Java__, доступ к которому можно получить через [веб-сайт Пользовательской службы визуального распознавания](https://customvision.ai/). Затем приложение передает изображения для обучения и тестирует классификатор. В этом проекте классификатор предназначен, чтобы определить, является ли дерево __болиголовом__ или __японской вишней__.

[!INCLUDE [get-keys](../../includes/get-keys.md)]

Эта программа настроена для создания ссылки на данные ключа в качестве переменных среды. Перейдите в папку **Vision/CustomVision** и введите следующие команды PowerShell, чтобы задать переменные среды. 

> [!NOTE]
> Если вы используете операционную систему, отличную от Windows, см. инструкции по [настройке переменных среды](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account?tabs=multiservice%2Cwindows#configure-an-environment-variable-for-authentication).

```powershell
$env:AZURE_CUSTOMVISION_TRAINING_API_KEY ="<your training api key>"
$env:AZURE_CUSTOMVISION_PREDICTION_API_KEY ="<your prediction api key>"
```

## <a name="examine-the-code"></a>Анализ кода

Загрузите проект `Vision/CustomVision` в Java IDE и откройте файл _CustomVisionSamples.java_. Найдите метод **runSample** и закомментируйте вызов метода **ObjectDetection_Sample** &mdash; будет выполнен сценарий обнаружения объекта, который не рассматривается в этом руководстве. Метод **ImageClassification_Sample** реализует основные функции этого примера. Перейдите в его определение и проверьте код.

## <a name="create-a-custom-vision-project"></a>Создание проекта в службе "Пользовательское визуальное распознавание"

Этот первый фрагмент кода создает проект классификации изображений. Созданный проект будет отображаться на [веб-сайте Пользовательской службы визуального распознавания](https://customvision.ai/), который вы посещали ранее. Ознакомьтесь с перегрузками метода [CreateProject](https://docs.microsoft.com/java/api/com.microsoft.azure.cognitiveservices.vision.customvision.training.trainings.createproject?view=azure-java-stable#com_microsoft_azure_cognitiveservices_vision_customvision_training_Trainings_createProject_String_CreateProjectOptionalParameter_), чтобы указать другие параметры при создании проекта (см. пояснения в руководстве по [созданию классификатора с помощью веб-портала](../../getting-started-build-a-classifier.md)).

[!code-java[](~/cognitive-services-java-sdk-samples/Vision/CustomVision/src/main/java/com/microsoft/azure/cognitiveservices/vision/customvision/samples/CustomVisionSamples.java?name=snippet_create)]

## <a name="create-tags-in-the-project"></a>Создание тегов в проекте

[!code-java[](~/cognitive-services-java-sdk-samples/Vision/CustomVision/src/main/java/com/microsoft/azure/cognitiveservices/vision/customvision/samples/CustomVisionSamples.java?name=snippet_tags)]

## <a name="upload-and-tag-images"></a>Отправка и снабжение тегами изображений

Примеры изображений включены в папку проекта **src/main/resources**. Они считываются оттуда и передаются в службу с соответствующими тегами.

[!code-java[](~/cognitive-services-java-sdk-samples/Vision/CustomVision/src/main/java/com/microsoft/azure/cognitiveservices/vision/customvision/samples/CustomVisionSamples.java?name=snippet_upload)]

Предыдущий фрагмент кода применяет две вспомогательные функции, которые извлекают изображения как потоки данных и загружают их в службу (можно передать до 64 изображений в одном пакете).

[!code-java[](~/cognitive-services-java-sdk-samples/Vision/CustomVision/src/main/java/com/microsoft/azure/cognitiveservices/vision/customvision/samples/CustomVisionSamples.java?name=snippet_helpers)]

## <a name="train-and-publish-the-project"></a>Обучение и публикация проекта

Этот код создает первую итерацию модели прогнозирования и публикует итерацию в конечной точке прогнозирования. Имя, присвоенное опубликованной итерации, можно использовать для отправки запросов на прогнозирование. Итерация недоступна в конечной точке прогнозирования, пока она не будет опубликована.

[!code-java[](~/cognitive-services-java-sdk-samples/Vision/CustomVision/src/main/java/com/microsoft/azure/cognitiveservices/vision/customvision/samples/CustomVisionSamples.java?name=snippet_train)]

## <a name="use-the-prediction-endpoint"></a>Использование конечной точки прогнозирования

Конечная точка прогнозирования, представленная объектом `predictor`, представляет собой ссылку, которую можно использовать для отправки изображения в текущую модель и получения прогноза классификации. В этом примере `predictor` определяется в другом месте с помощью переменной среды ключа прогнозирования.

[!code-java[](~/cognitive-services-java-sdk-samples/Vision/CustomVision/src/main/java/com/microsoft/azure/cognitiveservices/vision/customvision/samples/CustomVisionSamples.java?name=snippet_predict)]

## <a name="run-the-application"></a>Выполнение приложения

Чтобы скомпилировать и запустить решение с помощью Maven, перейдите в каталог проекта (**Vision/CustomVision**) в командной строке и выполните следующую команду.

```bash
mvn compile exec:java
```

Выходные данные консоли приложения должны выглядеть приблизительно так:

```console
Creating project...
Adding images...
Adding image: hemlock_1.jpg
Adding image: hemlock_2.jpg
Adding image: hemlock_3.jpg
Adding image: hemlock_4.jpg
Adding image: hemlock_5.jpg
Adding image: hemlock_6.jpg
Adding image: hemlock_7.jpg
Adding image: hemlock_8.jpg
Adding image: hemlock_9.jpg
Adding image: hemlock_10.jpg
Adding image: japanese_cherry_1.jpg
Adding image: japanese_cherry_2.jpg
Adding image: japanese_cherry_3.jpg
Adding image: japanese_cherry_4.jpg
Adding image: japanese_cherry_5.jpg
Adding image: japanese_cherry_6.jpg
Adding image: japanese_cherry_7.jpg
Adding image: japanese_cherry_8.jpg
Adding image: japanese_cherry_9.jpg
Adding image: japanese_cherry_10.jpg
Training...
Training status: Training
Training status: Training
Training status: Completed
Done!
        Hemlock: 93.53%
        Japanese Cherry: 0.01%
```

Затем можно проверить правильность прогноза относительно тестового изображения (последние несколько строк выходных данных).

[!INCLUDE [clean-ic-project](../../includes/clean-ic-project.md)]

## <a name="next-steps"></a>Дальнейшие действия

Теперь вы узнали, как выполнять в коде каждый шаг процесса обнаружения объектов. В этом примере выполняется одна итерация обучения, но часто нужно несколько раз обучать и тестировать модель, чтобы сделать ее более точной.

> [!div class="nextstepaction"]
> [Тестирование и переобучение модели с помощью Пользовательской службы визуального распознавания](../../test-your-model.md)

* Что собой представляет Пользовательское визуальное распознавание
* * [Справочная документация по пакету SDK](https://docs.microsoft.com/java/api/overview/azure/cognitiveservices/client/customvision?view=azure-java-stable)