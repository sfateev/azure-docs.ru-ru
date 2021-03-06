---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 09/23/2019
ms.author: glenga
ms.openlocfilehash: 8530f4469a0c25f3c32e652e2b0752c51c28ff3f
ms.sourcegitcommit: eb6bef1274b9e6390c7a77ff69bf6a3b94e827fc
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/05/2020
ms.locfileid: "78191079"
---
Атрибуты привязки определяются непосредственно в файле function.json. В зависимости от типа привязки могут потребоваться дополнительные свойства. [Параметр выходных данных очереди](../articles/azure-functions/functions-bindings-storage-queue-output.md#configuration) описывает поля, требуемые для привязки очереди службы хранилища Azure. Расширение позволяет легко добавлять привязки в файл function.json. 

Чтобы создать привязку, щелкните правой кнопкой мыши (CTRL+щелчок в macOS) файл `function.json` в папке HttpTrigger и выберите **Add binding...** (Добавить привязку...). Выполните инструкции, указанные на экране, чтобы определить следующие свойства для новой привязки:

| prompt | Значение | Описание |
| -------- | ----- | ----------- |
| **Select binding direction** (Выберите направление привязки) | `out` | Привязка является выходной привязкой. |
| **Select binding with direction...** (Выберите привязку с направлением...) | `Azure Queue Storage` | Привязка является привязкой очереди службы хранилища Azure. |
| **The name used to identify this binding in your code** (Имя, используемое для идентификации этой привязки в коде) | `msg` | Имя, которое используется для идентификации параметров привязки, указанных в коде. |
| **The queue to which the message will be sent** (Очередь, в которую будет отправляться сообщение) | `outqueue` | Имя очереди, в которой записывается привязка. Если *queueName* не существует, то при первом использовании этот параметр будет создан привязкой. |
| **Select setting from "local.setting.json"** (Выберите параметр из файла local.setting.json) | `AzureWebJobsStorage` | Имя параметра приложения, который содержит строку подключения к учетной записи хранения. Параметр `AzureWebJobsStorage` содержит строку подключения учетной записи хранения, созданной в приложении-функции. |

Привязка добавляется к массиву `bindings` в function.json, который теперь должен выглядеть примерно так:

:::code language="son" source="~/functions-docs-javascript/functions-add-output-binding-storage-queue-cli/HttpExample/function.json" range="18-24":::