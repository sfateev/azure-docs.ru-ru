---
author: areddish
ms.author: areddish
ms.service: cognitive-services
ms.date: 09/15/2020
ms.custom: devx-track-js
ms.openlocfilehash: 90927109a78d387ed3a535128e98ae7910c222dc
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/25/2020
ms.locfileid: "91321069"
---
Это руководство содержит инструкции и пример кода, которые помогут вам начать работу с клиентской библиотекой Пользовательского визуального распознавания для Node.js и создать модель классификации изображений. Здесь объясняется, как создать проект, добавить теги, обучить проект и использовать URL-адрес конечной точки прогнозирования проекта для тестирования программными средствами. Этот пример можно использовать как шаблон при создании собственного приложения для распознавания изображений.

> [!NOTE]
> Если вы хотите создать и обучить модель классификации _без_ написания кода, см. руководство по [работе со средствами на основе браузера](../../getting-started-build-a-classifier.md).

## <a name="prerequisites"></a>Предварительные требования

- Установленный компонент [Node.js 8](https://www.nodejs.org/en/download/) или более поздней версии.
- Установленный компонент [npm](https://www.npmjs.com/).
- [!INCLUDE [create-resources](../../includes/create-resources.md)]

## <a name="install-the-custom-vision-client-library"></a>Установка клиентской библиотеки Пользовательского визуального распознавания

Чтобы написать приложение для анализа изображений с помощью Пользовательского визуального распознавания для Node.js, вам потребуются пакеты NPM этой службы. Чтобы установить их, выполните в PowerShell следующую команду:

```shell
npm install @azure/cognitiveservices-customvision-training
npm install @azure/cognitiveservices-customvision-prediction
```

[!INCLUDE [get-keys](../../includes/get-keys.md)]

[!INCLUDE [node-get-images](../../includes/node-get-images.md)]

## <a name="add-the-code"></a>Добавление кода

Создайте файл с именем *sample.js* в необходимом каталоге проекта.

## <a name="create-the-custom-vision-project"></a>Создание проекта Пользовательского визуального распознавания

Добавьте в скрипт следующий код, чтобы создать проект Пользовательской службы визуального распознавания. Вставьте ключи подписки в соответствующие определения и задайте путь к папке образа в качестве значения пути sampleDataRoot. Убедитесь, что значение endPoint соответствует конечным точкам обучения и прогнозирования, созданным в [Customvision.ai](https://www.customvision.ai/). Обратите внимание, что разница между созданием проекта поиска объектов и классификации изображений состоит в домене, установленном в вызове **createProject**.

```javascript
const util = require('util');
const fs = require('fs');
const TrainingApi = require("@azure/cognitiveservices-customvision-training");
const PredictionApi = require("@azure/cognitiveservices-customvision-prediction");
const msRest = require("@azure/ms-rest-js");

const setTimeoutPromise = util.promisify(setTimeout);

const trainingKey = "<your training key>";
const predictionKey = "<your prediction key>";
const predictionResourceId = "<your prediction resource id>";
const sampleDataRoot = "<path to image files>";

const endPoint = "https://<my-resource-name>.cognitiveservices.azure.com/"

const publishIterationName = "classifyModel";

const credentials = new msRest.ApiKeyCredentials({ inHeader: { "Training-key": trainingKey } });
const trainer = new TrainingApi.TrainingAPIClient(credentials, endPoint);

(async () => {
    console.log("Creating project...");
    const sampleProject = await trainer.createProject("Sample Project");
```

## <a name="create-tags-in-the-project"></a>Создание тегов в проекте

Чтобы создать теги классификации в проекте, добавьте в конец *sample.go* следующий код:

```javascript
    const hemlockTag = await trainer.createTag(sampleProject.id, "Hemlock");
    const cherryTag = await trainer.createTag(sampleProject.id, "Japanese Cherry");
```

## <a name="upload-and-tag-images"></a>Отправка и снабжение тегами изображений

Чтобы добавить примеры изображений в проект, вставьте следующий код после создания тегов. Этот код загружает каждое изображение с соответствующим тегом. В одном пакете можно передать до 64 изображений.

> [!NOTE]
> Вам также понадобится изменить параметр *sampleDataRoot* на путь к изображениям в зависимости от того, куда вы ранее скачали проект с образцами пакета SDK Cognitive Services для Node.js.

```javascript
    console.log("Adding images...");
    let fileUploadPromises = [];
    
    const hemlockDir = `${sampleDataRoot}/Hemlock`;
    const hemlockFiles = fs.readdirSync(hemlockDir);
    hemlockFiles.forEach(file => {
        fileUploadPromises.push(trainer.createImagesFromData(sampleProject.id, fs.readFileSync(`${hemlockDir}/${file}`), { tagIds: [hemlockTag.id] }));
    });
    
    const cherryDir = `${sampleDataRoot}/Japanese Cherry`;
    const japaneseCherryFiles = fs.readdirSync(cherryDir);
    japaneseCherryFiles.forEach(file => {
        fileUploadPromises.push(trainer.createImagesFromData(sampleProject.id, fs.readFileSync(`${cherryDir}/${file}`), { tagIds: [cherryTag.id] }));
    });
    
    await Promise.all(fileUploadPromises);
```

## <a name="train-and-publish-the-classifier"></a>Обучение и публикация классификатора

Этот код создает первую итерацию модели прогнозирования и публикует итерацию в конечной точке прогнозирования. Имя, присвоенное опубликованной итерации, можно использовать для отправки запросов на прогнозирование. Итерация недоступна в конечной точке прогнозирования, пока она не будет опубликована.

```javascript
    console.log("Training...");
    let trainingIteration = await trainer.trainProject(sampleProject.id);
    
    // Wait for training to complete
    console.log("Training started...");
    while (trainingIteration.status == "Training") {
        console.log("Training status: " + trainingIteration.status);
        await setTimeoutPromise(1000, null);
        trainingIteration = await trainer.getIteration(sampleProject.id, trainingIteration.id)
    }
    console.log("Training status: " + trainingIteration.status);
    
    // Publish the iteration to the end point
    await trainer.publishIteration(sampleProject.id, trainingIteration.id, publishIterationName, predictionResourceId);
```

## <a name="use-the-prediction-endpoint"></a>Использование конечной точки прогнозирования

Чтобы отправить изображение в конечную точку прогнозирования и извлечь прогнозирование, добавьте в конец файла следующий код:

```javascript
    const predictor_credentials = new msRest.ApiKeyCredentials({ inHeader: { "Prediction-key": predictionKey } });
    const predictor = new PredictionApi.PredictionAPIClient(predictor_credentials, endPoint);
    const testFile = fs.readFileSync(`${sampleDataRoot}/Test/test_image.jpg`);

    const results = await predictor.classifyImage(sampleProject.id, publishIterationName, testFile);

    // Step 6. Show results
    console.log("Results:");
    results.predictions.forEach(predictedResult => {
        console.log(`\t ${predictedResult.tagName}: ${(predictedResult.probability * 100.0).toFixed(2)}%`);
    });
})()
```

## <a name="run-the-application"></a>Выполнение приложения

Выполните *sample.js*.

```shell
node sample.js
```

Выходные данные приложения должны выглядеть приблизительно так:

```console
Creating project...
Adding images...
Training...
Training started...
Training status: Training
Training status: Training
Training status: Training
Training status: Completed
Results:
         Hemlock: 94.97%
         Japanese Cherry: 0.01%
```

Вы можете убедиться, что тестовое изображение (в **<base_image_url>/Images/Test/** ) помечено соответствующим образом. Вы также можете вернуться к [веб-сайту Пользовательской службы визуального распознавания](https://customvision.ai) и просмотреть текущее состояние созданного проекта.

[!INCLUDE [clean-ic-project](../../includes/clean-ic-project.md)]

## <a name="next-steps"></a>Дальнейшие действия

Теперь вы узнали, как выполнять в коде каждый шаг процесса обнаружения объектов. В этом примере выполняется одна итерация обучения, но часто нужно несколько раз обучать и тестировать модель, чтобы сделать ее более точной.

> [!div class="nextstepaction"]
> [Тестирование и переобучение модели с помощью Пользовательской службы визуального распознавания](../../test-your-model.md)

* [Что собой представляет Пользовательское визуальное распознавание](../../overview.md)
* [Справочная документация по пакету SDK (обучение)](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-customvision-training/?view=azure-node-latest)
* [Справочная документация по пакету SDK (прогнозирование)](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-customvision-prediction/?view=azure-node-latest)