---
title: Краткое руководство. Создание, развертывание и использование пользовательской модели в Custom Translator
titleSuffix: Azure Cognitive Services
description: В этом кратком руководстве вы выполните пошаговый процесс создания системы перевода с помощью Custom Translator.
author: swmachan
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.date: 12/09/2019
ms.author: swmachan
ms.topic: quickstart
ms.openlocfilehash: f24c9c372ff91db5836a62ac2d08b569434ff253
ms.sourcegitcommit: 6a4687b86b7aabaeb6aacdfa6c2a1229073254de
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/06/2020
ms.locfileid: "91761585"
---
# <a name="quickstart-build-deploy-and-use-a-custom-model-for-translation"></a>Краткое руководство. Создание, развертывание и использование пользовательской модели перевода

В этой статье приведена пошаговая инструкция по созданию системы перевода с помощью Custom Translator.

## <a name="prerequisites"></a>Предварительные требования

1. Чтобы войти на портал [Custom Translator](https://portal.customtranslator.azure.ai) и использовать его, вам потребуется [учетная запись Майкрософт](https://signup.live.com) или [Azure AD](https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-whatis) (размещенная в Azure учетная запись организации).

2. Подписка на API перевода текстов, которую можно получить на портале Azure. Вам потребуется связать ключ подписки API перевода текстов с рабочей областью в Custom Translator. [Сведения о регистрации для использования API перевода текстов](https://docs.microsoft.com/azure/cognitive-services/translator/translator-text-how-to-signup).

3. Если у вас есть все указанные выше компоненты, войдите на портал [Пользовательского переводчика](https://portal.customtranslator.azure.ai), чтобы создавать рабочие области и проекты, загружать файлы, а также создавать и развертывать модели.

>[!Note]
>Компонент "Пользовательский переводчик" не поддерживает создание рабочей области для ресурса API "Перевод текстов", созданного в [включенной виртуальной сети](https://docs.microsoft.com/azure/api-management/api-management-using-with-vnet).

## <a name="create-a-workspace"></a>Создание рабочей области

Если раньше вы не работали с этим продуктом, вам будет предложено принять условия предоставления услуг, создать рабочую область и связать рабочую область с подпиской API Перевода текстов (Майкрософт).

![Создание рабочей области](media/quickstart/terms-of-service.png)
![Создание рабочей области, изображение 1](media/quickstart/create-workspace-1.png)
![Создание рабочей области, изображение 2](media/quickstart/create-workspace-2.png)
![Создание рабочей области, изображение 3](media/quickstart/create-workspace-3.png)
![Создание рабочей области, изображение 4](media/quickstart/create-workspace-4.png)
![Создание рабочей области, изображение 5](media/quickstart/create-workspace-5.png)
![Создание рабочей области, изображение 6](media/quickstart/create-workspace-6.png)

В один из последующих визитов на портал Пользовательского переводчика перейдите на страницу параметров, где вы можете администрировать рабочую область, создать дополнительные рабочие области, связать ключ подписки API Перевода текстов (Майкрософт) с рабочими областями, добавить совладельцев, а также изменить ключ подписки.

## <a name="create-a-project"></a>Создание проекта

На целевой странице портала Custom Translator щелкните New Project (Новый проект). В диалоговом окне можно ввести имя нужного проекта, языковую пару и категорию, а также заполнить другие соответствующие поля. Сохраните проект. [Дополнительные сведения о создании проекта](how-to-create-project.md).

![Создание проекта](media/quickstart/ct-how-to-create-project.png)


## <a name="upload-documents"></a>Отправка документов

Затем отправьте наборы документов для [обучения](training-and-model.md#training-document-type-for-custom-translator), [настройки](training-and-model.md#tuning-document-type-for-custom-translator) и [тестирования](training-and-model.md#testing-dataset-for-custom-translator). Вы можете отправить [параллельные](what-are-parallel-documents.md) или комбинированные документы. Можно также отправить [словарь](what-is-dictionary.md).

Вы можете отправить документы со вкладки документов или со страницы конкретного проекта.

![Отправка документов](media/quickstart/ct-how-to-upload.png)

При отправке документов выберите тип документа (обучение, настройка или тестирование) и языковую пару. В случае отправки параллельных документов необходимо дополнительно указать имя документа. Дополнительные сведения см. в статье о [передаче документов](how-to-upload-document.md).

## <a name="create-a-model"></a>Создание модели

После передачи всех необходимых документов нужно переходить к созданию модели.

Выберите проект, который вы создали. Вы увидите все отправленные вами документы, языковая пара которых совпадает с настроенной для этого проекта. Выберите документы, которые необходимо включить в модель. Вы можете выбрать данные для [обучения](training-and-model.md#training-document-type-for-custom-translator), [настройки](training-and-model.md#tuning-document-type-for-custom-translator) или [тестирования](training-and-model.md#testing-dataset-for-custom-translator) либо выбрать только данные для обучения и разрешить Custom Translator автоматически создать для модели наборы для настройки и тестирования.

![Создание модели](media/quickstart/ct-how-to-train.png)

Когда вы выберете нужные документы, нажмите кнопку Create Model (Создать модель), чтобы создать модель и начать обучение. На вкладке Models (Модели) вы увидите состояние обучения и сведения обо всех обученных моделях.

[Дополнительные сведения об обучении модели](how-to-train-model.md).

## <a name="analyze-your-model"></a>Анализ модели

После успешного завершения обучения проверьте результаты. Оценка BLEU — это метрика, которая показывает качество вашего перевода. Вы также можете самостоятельно сравнить переводы, сделанные с помощью вашей пользовательской модели, с переводами, которые предоставлены в наборе тестирования, перейдя на вкладку Test (Тестирование) и выбрав System Results (Результаты системы). Проверка некоторых из этих переводов самостоятельно даст вам хорошее представление о качестве перевода, сделанного системой. Дополнительные сведения см. в статье [Просмотр результатов теста системы](how-to-view-system-test-results.md).

## <a name="deploy-a-trained-model"></a>Развертывание обученной модели

Когда вы будете готовы развернуть обученную модель, нажмите кнопку Deploy (Развернуть). У вас может быть одна развернутая модель на проект, и вы можете просмотреть состояние развертывания в столбце состояния. Дополнительные сведения см. в разделе [Развертывание модели](how-to-view-system-test-results.md#deploy-a-model)

![Развертывание обученной модели](media/quickstart/ct-how-to-deploy.png)

## <a name="swap-deployed-model"></a>Переключение развернутой модели

Чтобы переключить в проекте развернутую модель на другую, нажмите кнопку "Переключить", которая отображается рядом с требуемой моделью. При переключении развернутая модель будет по-прежнему доступна для обслуживания запросов на перевод. 

![Переключение развернутой модели](media/quickstart/ct-how-to-swap-model.png)

## <a name="use-a-deployed-model"></a>Использование развернутой модели

Доступ к развернутым моделям можно получить через [API перевода текстов Майкрософт версии 3, указав идентификатор категории](https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-translate?tabs=curl). Дополнительные сведения об API перевода текстов можно найти на [этой веб-странице](https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-reference).

## <a name="next-steps"></a>Дальнейшие действия

- Узнайте, как переходить между элементами в [рабочей области Custom Translator и управлять проектами](workspace-and-project.md).
