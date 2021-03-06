---
title: Отладка & устранение неполадок конвейеров машинного обучения
titleSuffix: Azure Machine Learning
description: Отладка конвейеров Машинное обучение Azure в Python. Изучите распространенные ловушки для разработки конвейеров и советы по отладке сценариев до и во время удаленного выполнения. Узнайте, как использовать Visual Studio Code для интерактивной отладки конвейеров машинного обучения.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
author: lobrien
ms.author: laobri
ms.date: 08/28/2020
ms.topic: conceptual
ms.custom: troubleshooting, devx-track-python
ms.openlocfilehash: be68ad35deca754df70bb51e83929e73ff132ba6
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2020
ms.locfileid: "91315411"
---
# <a name="debug-and-troubleshoot-machine-learning-pipelines"></a>Отладка и устранение неполадок в конвейерах машинного обучения

Из этой статьи вы узнаете, как выполнять отладку и устранение неполадок в [конвейерах машинного обучения](concept-ml-pipelines.md) в [машинное обучение Azure SDK](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py&preserve-view=true) и [конструкторе машинное обучение Azure](https://docs.microsoft.com/azure/machine-learning/concept-designer). Сведения приведены в следующих руководствах:

## <a name="troubleshooting-tips"></a>Советы по устранению неполадок

В следующей таблице приведены распространенные проблемы во время разработки конвейера с потенциальными решениями.

| Проблема | Возможное решение |
|--|--|
| Не удалось передать данные в `PipelineData` Каталог | Убедитесь, что в скрипте создан каталог, который соответствует расположению выходных данных шага в конвейере. В большинстве случаев входной аргумент определяет выходной каталог, а затем создает каталог явным образом. Используйте `os.makedirs(args.output_dir, exist_ok=True)` для создания выходного каталога. См. [руководство](tutorial-pipeline-batch-scoring-classification.md#write-a-scoring-script) по примеру сценария оценки, в котором показан этот шаблон разработки. |
| Ошибки зависимостей | Если в удаленном конвейере отображаются ошибки зависимостей, которые не возникали при локальном тестировании, проверьте зависимости удаленной среды и версии, совпадающие с параметрами в тестовой среде. (См. раздел [Создание среды, кэширование и повторное использование](https://docs.microsoft.com/azure/machine-learning/concept-environments#environment-building-caching-and-reuse)|
| Неоднозначные ошибки с целевыми объектами вычислений | Попробуйте удалить и повторно создать целевые объекты вычислений. Повторное создание целевых объектов вычислений является быстрым и может решить некоторые временные проблемы. |
| Конвейер не использует повторно шаги | Повторное использование шага включено по умолчанию, но убедитесь, что вы не отключили его на этапе конвейера. Если повторное использование отключено, `allow_reuse` параметр на шаге будет установлен в значение `False` . |
| Конвейер перезапускается без необходимости | Чтобы обеспечить повторный запуск шагов только при изменении базовых данных или скриптов, следует разделить Каталоги исходного кода на каждый шаг. Если один и тот же исходный каталог используется для нескольких шагов, может возникнуть ненужное повторное использование. Используйте `source_directory` параметр для объекта шага конвейера, чтобы указать на свой изолированный каталог для этого шага, и убедитесь, что вы не используете один и тот же `source_directory` путь для нескольких шагов. |
| Шаг с замедлением при обучении эпохи обучения или другое поведение цикла | Попробуйте переключить записи файлов, включая ведение журнала, с `as_mount()` на `as_upload()` . Режим **подключения** использует удаленную виртуальную файловую систему и загружает весь файл каждый раз, когда он добавляется к. |

## <a name="debugging-techniques"></a>Методы отладки

Существует три основных метода отладки конвейеров: 

* Отладка отдельных шагов конвейера на локальном компьютере
* Использование ведения журналов и Application Insights для изоляции и диагностики источника проблемы
* Подключение удаленного отладчика к конвейеру, выполняющемуся в Azure

### <a name="debug-scripts-locally"></a>Локальная отладка скриптов

Одной из наиболее распространенных сбоев в конвейере является то, что сценарий домена не выполняется должным образом или содержит ошибки времени выполнения в удаленном контексте вычислений, которые трудно отладить.

Сами по себе конвейеры не могут выполняться локально, но выполнение сценариев на локальном компьютере позволяет выполнять отладку быстрее, поскольку не нужно ждать процесса сборки вычислений и среды. Для этого требуется выполнить некоторые действия по разработке:

* Если данные находятся в облачном хранилище данных, необходимо скачать данные и сделать их доступными для скрипта. Использование небольшого примера данных — хороший способ сократить время выполнения и получить отзыв о поведении сценария.
* При попытке имитировать промежуточный шаг конвейера может потребоваться вручную создать типы объектов, которые определенный сценарий ожидает на предыдущем шаге.
* Также потребуется определить собственную среду и реплицировать зависимости, определенные в удаленной среде вычислений.

После установки сценария для запуска в локальной среде гораздо проще выполнять задачи отладки, такие как:

* Присоединение пользовательской конфигурации отладки
* Приостановка выполнения и проверка состояния объекта
* Перехват типов или логических ошибок, которые не будут предоставляться до времени выполнения

> [!TIP] 
> Убедившись в том, что сценарий выполняется должным образом, на следующем шаге выполняется сценарий в одношаговом конвейере, прежде чем пытаться запустить его в конвейере с несколькими шагами.

## <a name="configure-write-to-and-review-pipeline-logs"></a>Настройка, запись и проверка журналов конвейера

Локальное тестирование скриптов — это отличный способ отладки основных фрагментов кода и сложной логики, прежде чем приступить к созданию конвейера, но в какой-то момент вам, скорее всего, придется отлаживать сценарии во время выполнения собственно конвейера, особенно при диагностике поведения, возникающего во время взаимодействия между этапами конвейера. Мы рекомендуем свободно использовать `print()` инструкции в сценариях шагов, чтобы видеть состояние объекта и ожидаемые значения во время удаленного выполнения, аналогично отладке кода JavaScript.

### <a name="logging-options-and-behavior"></a>Параметры ведения журнала и поведение

В таблице ниже приведены сведения о различных параметрах отладки для конвейеров. Он не является исчерпывающим списком, так как существуют другие варианты, кроме только Машинное обучение Azure, Python и Опенценсус, показанных здесь.

| Библиотека                    | Тип   | Пример                                                          | Назначение                                  | Ресурсы                                                                                                                                                                                                                                                                                                                    |
|----------------------------|--------|------------------------------------------------------------------|----------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| пакет SDK для Машинного обучения Azure; | Метрика | `run.log(name, val)`                                             | Пользовательский интерфейс портала Машинное обучение Azure             | [Как отвести эксперименты](how-to-track-experiments.md)<br>[класс azureml. Core. Run](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run%28class%29?view=azure-ml-py&preserve-view=true)                                                                                                                                                 |
| Печать и ведение журнала Python    | Журнал    | `print(val)`<br>`logging.info(message)`                          | Журналы драйверов, конструктор Машинное обучение Azure | [Как отвести эксперименты](how-to-track-experiments.md)<br><br>[Ведение журнала Python](https://docs.python.org/2/library/logging.html)                                                                                                                                                                       |
| Python для OpenCensus          | Журнал    | `logger.addHandler(AzureLogHandler())`<br>`logging.log(message)` | Application Insights трассировки                | [Отладка конвейеров в Application Insights](how-to-debug-pipelines-application-insights.md)<br><br>[Агенты OpenCensus Azure Monitor Exporter](https://github.com/census-instrumentation/opencensus-python/tree/master/contrib/opencensus-ext-azure)<br>[Cookbook ведения журнала Python](https://docs.python.org/3/howto/logging-cookbook.html) |

#### <a name="logging-options-example"></a>Пример параметров ведения журнала

```python
import logging

from azureml.core.run import Run
from opencensus.ext.azure.log_exporter import AzureLogHandler

run = Run.get_context()

# Azure ML Scalar value logging
run.log("scalar_value", 0.95)

# Python print statement
print("I am a python print statement, I will be sent to the driver logs.")

# Initialize python logger
logger = logging.getLogger(__name__)
logger.setLevel(args.log_level)

# Plain python logging statements
logger.debug("I am a plain debug statement, I will be sent to the driver logs.")
logger.info("I am a plain info statement, I will be sent to the driver logs.")

handler = AzureLogHandler(connection_string='<connection string>')
logger.addHandler(handler)

# Python logging with OpenCensus AzureLogHandler
logger.warning("I am an OpenCensus warning statement, find me in Application Insights!")
logger.error("I am an OpenCensus error statement with custom dimensions", {'step_id': run.id})
``` 

## <a name="azure-machine-learning-designer"></a>Конструктор Машинного обучения Azure

Для конвейеров, созданных в конструкторе, файл **70_driver_log** можно найти либо на странице Создание, либо на странице сведения о выполнении конвейера.

### <a name="enable-logging-for-real-time-endpoints"></a>Включение ведения журнала для конечных точек в реальном времени

Для устранения неполадок и отладки конечных точек в режиме реального времени в конструкторе необходимо включить ведение журнала Application Insights с помощью пакета SDK. Ведение журнала позволяет устранять неполадки и отлаживать проблемы развертывания и использования модели. Дополнительные сведения см. в разделе [ведение журнала для развернутых моделей](how-to-enable-logging.md#logging-for-deployed-models). 

### <a name="get-logs-from-the-authoring-page"></a>Получение журналов со страницы "Создание и Настройка"

При отправке выполнения конвейера и остаться на странице Создание и настройка можно найти файлы журналов, созданные для каждого модуля по мере завершения выполнения каждого модуля.

1. Выберите модуль, который завершил выполнение на холсте разработки.
1. На правой панели модуля перейдите на вкладку  **выходные данные + журналы** .
1. Разверните правую панель и выберите **70_driver_log.txt** , чтобы просмотреть файл в браузере. Кроме того, можно скачать журналы локально.

    ![Развернутая область вывода в конструкторе](./media/how-to-debug-pipelines/designer-logs.png)? View = Azure-ML-корректировка&сохранить-просмотреть = true)? View = Azure-ML-Корр&Preserve-View = true)

### <a name="get-logs-from-pipeline-runs"></a>Получение журналов из запусков конвейера

Файлы журналов для конкретных запусков можно также найти на странице сведений о запуске конвейера, которую можно найти в разделе **конвейеры** или **эксперименты** в студии.

1. Выберите Запуск конвейера, созданный в конструкторе.

    ![Страница выполнения конвейера](./media/how-to-debug-pipelines/designer-pipelines.png)

1. Выберите модуль в области просмотра.
1. На правой панели модуля перейдите на вкладку  **выходные данные + журналы** .
1. Разверните правую панель, чтобы просмотреть файл **70_driver_log.txt** в браузере, или выберите файл, чтобы скачать журналы локально.

> [!IMPORTANT]
> Чтобы обновить конвейер со страницы сведения о выполнении конвейера, необходимо **клонировать** выполнение конвейера в новый черновик конвейера. Запуск конвейера — это моментальный снимок конвейера. Он аналогичен файлу журнала и не может быть изменен. 

## <a name="application-insights"></a>Application Insights
Дополнительные сведения об использовании библиотеки Опенценсус Python таким образом см. в разделе [Отладка и устранение неполадок конвейеров машинного обучения в Application Insights](how-to-debug-pipelines-application-insights.md)

## <a name="interactive-debugging-with-visual-studio-code"></a>Интерактивная Отладка с помощью Visual Studio Code

В некоторых случаях может потребоваться интерактивно отлаживать код Python, используемый в конвейере машинного обучения. С помощью Visual Studio Code (VS Code) и дебугпи можно присоединяться к коду, как он выполняется в среде обучения. Дополнительные сведения см. в [разделе Интерактивная Отладка в VS Code Guide](how-to-debug-visual-studio-code.md#debug-and-troubleshoot-machine-learning-pipelines).

## <a name="next-steps"></a>Дальнейшие шаги

* Обратитесь к Справочнику по пакету SDK, чтобы получить справку по пакету [azureml-конвейеры-Core](https://docs.microsoft.com/python/api/azureml-pipeline-core/?view=azure-ml-py&preserve-view=true) и пакету [azureml-конвейеры-этапов](https://docs.microsoft.com/python/api/azureml-pipeline-steps/?view=azure-ml-py&preserve-view=true) .

* См. список [исключений и кодов ошибок конструктора](algorithm-module-reference/designer-error-codes.md).
