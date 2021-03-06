---
title: включить файл
description: включить файл
services: cognitive-services
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.date: 05/05/2020
ms.topic: include
ms.openlocfilehash: fda9df6c7e9651bbd3b0b70ad9d47f23c0c19d01
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2020
ms.locfileid: "91541443"
---
## <a name="sign-in-to-luis-portal"></a>Вход на портал LUIS

Новому пользователю LUIS следует выполнить следующие действия:

1. Войдите на [портал LUIS](https://www.luis.ai), выберите свою страну или регион и примите условия использования. Если же вы видите **Мои приложения**, это значит, что ресурс LUIS уже существует и вы можете сразу перейти к созданию приложения. Сведения о поддерживаемых регионах см. в статье о [Регионы создания и публикации и связанные ключи](https://docs.microsoft.com/azure/cognitive-services/luis/luis-reference-regions).

1. Последовательно выберите **Create Azure resource** (Создать ресурс Azure) и **Create an authoring resource to migrate your apps to** (Создать ресурс для разработки, в который будут перенесены ваши приложения).

    ![Выбор типа ресурса для разработки в службе "Распознавание речи"](../media/luis-how-to-azure-subscription/sign-in-create-resource.png)

1. Укажите сведения о ресурсе.

    ![На снимке экрана показана область "Создание нового ресурса для разработки".](../media/migrate-authoring-key/choose-authoring-resource-form.png)

    При **создании ресурса для разработки** укажите следующие сведения:

    * **Имя ресурса.** Выбранное пользователем имя, используемое в качестве части URL-адреса для запросов конечной точки разработки и прогнозирования.
    * **Клиент.** Клиент, с которым связана подписка Azure.
    * **Имя подписки.** Подписка, для которой будет выставлен счет за ресурс.
    * **Группа ресурсов.** Выбранное или созданное пользователем имя группы ресурсов. Группы ресурсов позволяют группировать ресурсы Azure для осуществления доступа и управления.
    * **Расположение.** Выбор расположения зависит от выбранной **группы ресурсов**.
    * **Ценовая категория.** Ценовая категория определяет максимальное количество транзакций в секунду и месяц.

1. Отобразится сводка по создаваемому ресурсу. Выберите **Далее**.

    ![На снимке экрана показана страница приветствия с параметром "Связывание учетной записи Azure".](../media/sign-in/sign-in-confirm-key-selection.png)

1. Подтвердите введенные данные, выбрав **Продолжить**.

    ![На снимке экрана показана страница приветствия после связывания учетной записи Azure.](../media/sign-in/sign-in-confirm-continue.png)
