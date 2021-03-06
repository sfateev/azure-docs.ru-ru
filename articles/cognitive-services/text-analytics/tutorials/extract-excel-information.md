---
title: Извлечение сведений в Excel с помощью службы "Анализ текста" и Power Automate
titleSuffix: Azure Cognitive Services
description: Узнайте, как извлечь текст Excel без написания кода с помощью Анализ текста и автоматизации Powering.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: article
ms.date: 02/27/2019
ms.author: aahi
ms.openlocfilehash: b67de07777fa3f4f2b6190d8b003eb0495e66d15
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2020
ms.locfileid: "91400491"
---
# <a name="extract-information-in-excel-using-text-analytics-and-power-automate"></a>Извлечение сведений в Excel с помощью службы "Анализ текста" и Power Automate 

В этом руководстве вы создадите поток Power автоматизировать для извлечения текста в электронной таблице Excel без написания кода. 

Этот поток получит электронную таблицу проблем, о которых было сообщено о сложном апартаменте, и классифицировать их в две категории: коммуникации и другие. Также извлекаются имена и номера телефонов клиентов, которые их отправили. Наконец, последовательность добавит эти сведения на лист Excel. 

В этом руководстве вы узнаете, как:

> [!div class="checklist"]
> * Создание последовательности с помощью Power автоматизиру
> * Отправка данных Excel из OneDrive для бизнеса
> * Извлеките текст из Excel и отправьте его в API анализа текста 
> * Используйте сведения из API для обновления листа Excel.

## <a name="prerequisites"></a>Предварительные требования

- Учетная запись Microsoft Azure. [Создайте бесплатную учетную запись](https://azure.microsoft.com/free/cognitive-services/) или [войдите](https://portal.azure.com/) в существующую.
- Ресурс Анализ текста. Если у вас ее нет, можно [создать ее на портал Azure](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesTextAnalytics) и использовать уровень Free для работы с этим руководством.
- [Ключ и конечная точка](../../cognitive-services-apis-create-account.md#get-the-keys-for-your-resource) , созданные при регистрации.
- Электронная таблица, содержащая проблемы с клиентом. Пример данных предоставлен на сайте GitHub
- Microsoft 365 с OneDrive для бизнеса.

## <a name="add-the-excel-file-to-onedrive-for-business"></a>Добавление файла Excel в OneDrive для бизнеса

Скачайте пример файла Excel с сайта [GitHub](https://github.com/Azure-Samples/cognitive-services-sample-data-files/blob/master/TextAnalytics/sample-data/ReportedIssues.xlsx). Этот файл должен храниться в вашей учетной записи OneDrive для бизнеса.

> [!div class="mx-imgBorder"] 
> :::image type="content" source="../media/tutorials/excel/example-data.png" alt-text="Примеры из файла Excel.":::

Проблемы выводятся в необработанном тексте. Для извлечения имени пользователя и номера телефона будет использоваться распознавание именованных сущностей API анализа текста. Затем последовательность будет искать слово «коммуникации» в описании для классификации проблем. 

## <a name="create-a-new-power-automate-workflow"></a>Создание нового рабочего процесса Power автоматизиру

Перейдите на [сайт Power автоматизиру](https://preview.flow.microsoft.com/)и войдите в систему. Затем щелкните **создать** и **распланировать поток**.

> [!div class="mx-imgBorder"] 
> :::image type="content" source="../media/tutorials/excel/flow-creation.png" alt-text="Примеры из файла Excel.":::


На странице **Создание запланированного потока** выполните инициализацию последовательности со следующими полями:

|Поле |Значение  |
|---------|---------|
|**Имя потока**     | **Запланированная проверка** или другое имя.         |
|**Запуск**     |  Введите текущие дату и время.       |
|**Повторять каждые**     | **1 час**        |

## <a name="add-variables-to-the-flow"></a>Добавление переменных в последовательность

> [!NOTE]
> Если вы хотите просмотреть образ завершенного потока, его можно скачать с сайта [GitHub](https://github.com/Azure-Samples/cognitive-services-sample-data-files/tree/master/TextAnalytics/flow-diagrams). 

Создайте переменные, представляющие сведения, которые будут добавлены в файл Excel. Щелкните **новый шаг** и выполните поиск по запросу **инициализировать переменную**. Сделайте это четыре раза, чтобы создать четыре переменные.

> [!div class="mx-imgBorder"] 
> :::image type="content" source="../media/tutorials/excel/initialize-variables.png" alt-text="Примеры из файла Excel.":::

Добавьте следующие сведения к созданным переменным. Они представляют столбцы файла Excel. Если какие бы то ни было переменные свернуты, можно щелкнуть их, чтобы развернуть.

| Действие |Имя   | Тип | Значение |
|---------|---------|---|---|
| Инициализация переменной | var_person | Строковый тип | Модель Person |
| Инициализация переменной 2 | var_phone | Строковый тип | Phone_Number |
| Инициализация переменной 3 | var_plumbing | Строковый тип | коммуникаций |
| Инициализация переменной 4 | var_other | Строковый тип | иное | 

> [!div class="mx-imgBorder"] 
> :::image type="content" source="../media/tutorials/excel/flow-variables.png" alt-text="Примеры из файла Excel.":::

## <a name="read-the-excel-file"></a>Чтение файла Excel

Нажмите кнопку **создать шаг** и введите **Excel**, а затем в списке действий выберите **список строк, представленных в таблице** .

> [!div class="mx-imgBorder"] 
> :::image type="content" source="../media/tutorials/excel/list-excel-rows.png" alt-text="Примеры из файла Excel.":::

Добавьте файл Excel в последовательность, заполнив поля в этом действии. Для работы с этим руководством требуется, чтобы файл был отправлен в OneDrive для бизнеса.

> [!div class="mx-imgBorder"] 
> :::image type="content" source="../media/tutorials/excel/list-excel-rows-options.png" alt-text="Примеры из файла Excel.":::

Щелкните **новый шаг** и добавьте действие **Применить к каждому** действию.

> [!div class="mx-imgBorder"] 
> :::image type="content" source="../media/tutorials/excel/add-apply-action.png" alt-text="Примеры из файла Excel.":::

Щелкните **Выбрать выходные данные на предыдущем шаге**. В появившемся поле динамического содержимого выберите **значение**.

> [!div class="mx-imgBorder"] 
> :::image type="content" source="../media/tutorials/excel/select-output.png" alt-text="Примеры из файла Excel.":::

## <a name="send-a-request-to-the-text-analytics-api"></a>Отправка запроса API анализа текста

Если вы этого еще не сделали, необходимо создать [анализ текста ресурс](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesTextAnalytics) в портал Azure.

### <a name="create-a-text-analytics-connection"></a>Создание подключения Анализ текста

В поле **Применить к каждому**нажмите кнопку **Добавить действие**. Перейдите на страницу **ключа и конечной точки** анализ текста ресурса в портал Azure и получите ключ и конечную точку для ресурса анализ текста.

В последовательности введите следующие сведения, чтобы создать новое подключение Анализ текста.

> [!NOTE]
> Если вы уже создали подключение Анализ текста и хотите изменить сведения о подключении, щелкните многоточие в правом верхнем углу и выберите **+ Добавить новое подключение**.

| Поле           | Значение                                                                                                             |
|-----------------|-------------------------------------------------------------------------------------------------------------------|
| Имя подключения | Имя для подключения к ресурсу Анализ текста. Например, `TAforPowerAutomate`. |
| Ключ учетной записи     | Ключ для ресурса Анализ текста.                                                                                   |
| URL-адрес сайта        | Конечная точка для ресурса Анализ текста.                                                       |

> [!div class="mx-imgBorder"] 
> :::image type="content" source="../media/tutorials/excel/add-credentials.png" alt-text="Примеры из файла Excel.":::

## <a name="extract-the-excel-content"></a>Извлечение содержимого Excel 

После создания подключения выполните поиск по фразе **анализ текста** и выберите **сущности**. Это приведет к извлечению сведений из столбца Описание проблемы.

> [!div class="mx-imgBorder"] 
> :::image type="content" source="../media/tutorials/excel/extract-info.png" alt-text="Примеры из файла Excel.":::

Щелкните **текстовое** поле и выберите **Описание** из появившихся окон динамического содержимого. Введите `en` для языка. (Если язык не отображается, щелкните Показать дополнительные параметры.)

> [!div class="mx-imgBorder"] 
> :::image type="content" source="../media/tutorials/excel/description-from-dynamic-content.png" alt-text="Примеры из файла Excel.":::


## <a name="extract-the-person-name"></a>Извлечение имени пользователя

Далее мы найдут тип сущности Person в выходных данных Анализ текста. В области **Применить к каждому**нажмите кнопку **Добавить действие**и создайте другое действие **Применить к каждому** действию. Щелкните внутри текстового поля и выберите **сущности** в появившемся окне динамического содержимого.

> [!div class="mx-imgBorder"] 
> :::image type="content" source="../media/tutorials/excel/add-apply-action-2.png" alt-text="Примеры из файла Excel.":::

В созданном действии **Применить к каждому из двух** действий щелкните **Добавить действие**и добавьте элемент управления **условие** .

> [!div class="mx-imgBorder"] 
> :::image type="content" source="../media/tutorials/excel/create-condition.png" alt-text="Примеры из файла Excel.":::

В окне Условие щелкните первое текстовое поле. В окне динамического содержимого найдите **тип сущности** и выберите его.

> [!div class="mx-imgBorder"] 
> :::image type="content" source="../media/tutorials/excel/choose-entities-value.png" alt-text="Примеры из файла Excel.":::

Убедитесь, что для второго поля установлено значение **равно**. Затем выберите третье поле и выполните поиск `var_person` в окне динамического содержимого. 

> [!div class="mx-imgBorder"] 
> :::image type="content" source="../media/tutorials/excel/choose-variable-value.png" alt-text="Примеры из файла Excel.":::

В условии **Если да** , введите в Excel, а затем выберите **обновить строку**.

> [!div class="mx-imgBorder"] 
> :::image type="content" source="../media/tutorials/excel/yes-column-action.png" alt-text="Примеры из файла Excel.":::

Введите данные Excel и обновите **Ключевые столбцы**, **Ключевые значения** и поля **персоннаме** . Это приведет к добавлению имени, обнаруженному API, на лист Excel. 

> [!div class="mx-imgBorder"] 
> :::image type="content" source="../media/tutorials/excel/yes-column-action-options.png" alt-text="Примеры из файла Excel.":::

## <a name="get-the-phone-number"></a>Получение номера телефона

**Чтобы выполнить действие "применить к каждому из 2** ", щелкните имя. Затем добавьте еще один **атрибут Apply для каждого** действия, как и раньше. Он будет называться **Apply for each 3**. Выберите текстовое поле и добавьте **сущности** в качестве выходных данных для этого действия. 

> [!div class="mx-imgBorder"] 
> :::image type="content" source="../media/tutorials/excel/add-apply-action-3.png" alt-text="Примеры из файла Excel.":::

В области **Применить к каждому 3**добавьте элемент управления **условие** . Оно будет иметь имя **Condition 2**. В первом текстовом поле Найдите и добавьте **тип сущности** из окна динамического содержимого. Убедитесь, что в центральном поле установлено значение **равно**. Затем в правом текстовом поле введите `var_phone` . 

> [!div class="mx-imgBorder"] 
> :::image type="content" source="../media/tutorials/excel/condition-2-options.png" alt-text="Примеры из файла Excel.":::

В условии **Если да** , добавьте действие **обновить строку** . Затем введите сведения, как мы делали выше, для столбца номера телефона листа Excel. Это приведет к добавлению номера телефона, обнаруженного API, на лист Excel. 

> [!div class="mx-imgBorder"] 
> :::image type="content" source="../media/tutorials/excel/condition-2-yes-column.png" alt-text="Примеры из файла Excel.":::


## <a name="get-the-plumbing-issues"></a>Получите проблемы с коммуникации

**Чтобы применить к каждому из 3** , щелкните имя. Затем создайте другое действие **Применить к каждому** в родительском действии. Выберите текстовое поле и добавьте **сущности** в качестве выходных данных для этого действия из окна динамического содержимого. 

> [!div class="mx-imgBorder"] 
> :::image type="content" source="../media/tutorials/excel/add-apply-action-4.png" alt-text="Примеры из файла Excel.":::


Затем последовательность проверит, содержит ли описание проблемы из строки таблицы Excel слово «коммуникации». Если да, в столбце Иссуетипе будет добавлено «водо». В противном случае мы будем вводить «other».

Внутри действия **Применить к каждому 4** добавьте элемент управления **условие** . Оно будет иметь имя **Condition 3**. В первом текстовом поле Найдите и добавьте **Описание** из файла Excel с помощью окна динамическое содержимое. Убедитесь, что в центральном поле указано слово **содержит**. Затем в правом текстовом поле Найдите и выберите `var_plumbing` . 

> [!div class="mx-imgBorder"] 
> :::image type="content" source="../media/tutorials/excel/condition-3-options.png" alt-text="Примеры из файла Excel.":::


В условии **Если да** нажмите кнопку **Добавить действие**и выберите **обновить строку**. Затем введите такие сведения, как и ранее. В столбце Иссуетипе выберите `var_plumbing` . При этом к строке будет применена метка "коммуникации".

В поле **Если нет** условия нажмите кнопку **Добавить действие**и выберите **обновить строку**. Затем введите такие сведения, как и ранее. В столбце Иссуетипе выберите `var_other` . При этом к строке будет применена метка "другой".

> [!div class="mx-imgBorder"] 
> :::image type="content" source="../media/tutorials/excel/plumbing-issue-condition.png" alt-text="Примеры из файла Excel.":::

## <a name="test-the-workflow"></a>Проверка рабочего процесса

В правом верхнем углу экрана щелкните **сохранить**, а затем — **тест**. Выберите  **действие триггера**. Щелкните **сохранить & тест**, **запустить поток**, а затем **Готово**.

Файл Excel будет обновлен в вашей учетной записи OneDrive. Он будет выглядеть, как показано ниже.

> [!div class="mx-imgBorder"] 
> :::image type="content" source="../media/tutorials/excel/updated-excel-sheet.png" alt-text="Примеры из файла Excel.":::

## <a name="next-steps"></a>Дальнейшие шаги

> [!div class="nextstepaction"]
> [Знакомство с другими решениями](../text-analytics-user-scenarios.md)
