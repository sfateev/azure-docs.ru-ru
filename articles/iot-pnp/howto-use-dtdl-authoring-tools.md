---
title: Использование средства для создания и проверки моделей ДТДЛ | Документация Майкрософт
description: Установите редактор ДТДЛ для Visual Studio Code или Visual Studio 2019 и используйте его для создания моделей Plug and Play IoT.
author: dominicbetts
ms.author: dobett
ms.date: 09/14/2020
ms.topic: how-to
ms.service: iot-pnp
services: iot-pnp
ms.openlocfilehash: 59ea0258d6262f652e9ac563bbda5ec89c4c5819
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2020
ms.locfileid: "91580695"
---
# <a name="install-and-use-the-dtdl-authoring-tools"></a>Установка и использование средств разработки ДТДЛ

Модели [Digital двойников Definition Language (дтдл)](https://github.com/Azure/opendigitaltwins-dtdl/blob/master/DTDL/v2/dtdlv2.md) — это JSON-файлы. Вы можете использовать расширение для Visual Studio Code или Visual Studio 2019 для создания и проверки этих файлов модели.

## <a name="install-and-use-the-vs-code-extension"></a>Установка и использование расширения VS Code

Расширение ДТДЛ для VS Code добавляет следующие функции создания ДТДЛ:

- Проверка синтаксиса ДТДЛ v2.
- Технология IntelliSense, включая Автозаполнение, для упрощения синтаксиса языка.
- Возможность создания интерфейсов из палитры команд.

Чтобы установить расширение ДТДЛ, перейдите в [Редактор дтдл для Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.vscode-dtdl). Можно также выполнить поиск **дтдл** в представлении Extensions (расширения) в VS Code.

После установки расширения используйте его для создания файлов модели ДТДЛ в VS Code:

- Расширение обеспечивает проверку синтаксиса в файлах модели ДТДЛ, выделяя ошибки, как показано на следующем снимке экрана:

    :::image type="content" source="media/howto-use-dtdl-authoring-tools/model-validation.png" alt-text="Проверка модели в VS Code":::

- При редактировании моделей ДТДЛ используйте IntelliSense и Автозаполнение:

    :::image type="content" source="media/howto-use-dtdl-authoring-tools/model-intellisense.png" alt-text="Проверка модели в VS Code":::

- Создайте новый интерфейс ДТДЛ. Следующая команда создает JSON-файл с новым интерфейсом. Интерфейс включает примеры данных телеметрии, свойств и команд.

## <a name="install-and-use-the-visual-studio-extension"></a>Установка и использование расширения Visual Studio

Расширение ДТДЛ для Visual Studio 2019 добавляет следующие функции создания ДТДЛ:

- Проверка синтаксиса ДТДЛ v2.
- Технология IntelliSense, включая Автозаполнение, для упрощения синтаксиса языка.

Чтобы установить расширение ДТДЛ, перейдите на страницу [Поддержка языка дтдл для VS 2019](https://marketplace.visualstudio.com/items?itemName=vsc-iot.vs16dtdllanguagesupport). Вы также можете найти **дтдл** в окне **Управление расширениями** в Visual Studio.

После установки расширения используйте его для создания файлов модели ДТДЛ в Visual Studio:

- Расширение обеспечивает проверку синтаксиса в файлах модели ДТДЛ, выделяя ошибки, как показано на следующем снимке экрана:

    :::image type="content" source="media/howto-use-dtdl-authoring-tools/model-validation-2.png" alt-text="Проверка модели в VS Code":::

- При редактировании моделей ДТДЛ используйте IntelliSense и Автозаполнение:

    :::image type="content" source="media/howto-use-dtdl-authoring-tools/model-intellisense-2.png" alt-text="Проверка модели в VS Code":::

## <a name="next-steps"></a>Дальнейшие шаги

В этом пошаговом руководстве вы узнали, как использовать расширения ДТДЛ для Visual Studio Code и Visual Studio 2019 для создания и проверки файлов модели ДТДЛ. Предлагаемый следующий шаг — Узнайте, как использовать [Обозреватель Интернета вещей Azure с моделями и устройствами](./howto-use-iot-explorer.md).
