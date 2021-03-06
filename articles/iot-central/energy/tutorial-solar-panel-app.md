---
title: Руководство по созданию приложения для мониторинга солнечной панели с помощью IoT Central
description: Руководство по Узнайте, как создать приложение солнечных панелей с помощью шаблонов приложений Azure IoT Central.
author: op-ravi
ms.author: omravi
ms.date: 11/12/2019
ms.topic: tutorial
ms.service: iot-central
services: iot-central
manager: abjork
ms.openlocfilehash: c0f4c4deaa57b1414a3ef55226e4c451b53ba72c
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2020
ms.locfileid: "90971316"
---
# <a name="tutorial-create-and-walk-through-the-solar-panel-monitoring-app-template"></a>Руководство по Создание и пошаговое руководство по шаблону приложения мониторинга солнечных панелей 



В этом учебнике описывается процесс создания приложения мониторинга солнечных панелей, которое включает в себя пример модели устройства с смоделированными данными. В этом учебнике вы узнаете следующее:


> [!div class="checklist"]
> * Бесплатное создание приложения солнечной панели
> * Пошаговое руководство по приложению
> * Очистка ресурсов


Если у вас нет подписки, [создайте бесплатную пробную учетную запись](https://azure.microsoft.com/free).

## <a name="prerequisites"></a>Предварительные требования
- None
- Подписка Azure рекомендуется, но не требуется для пробного использования


## <a name="create-a-solar-panel-monitoring-app"></a>Создание приложения для мониторинга солнечной панели 

Это приложение можно создать в три простых шага:

1. Откройте домашнюю страницу [Azure IoT Central](https://apps.azureiotcentral.com) и щелкните **Сборка**, чтобы создать новое приложение. 

2. Выберите вкладку **Энергия** и щелкните **Создать приложение** в плитке приложения **Мониторинг солнечной панели**. 

    > [!div class="mx-imgBorder"]
    > ![Создание приложения](media/tutorial-iot-central-solar-panel/solar-panel-build.png)
  
3. **Создание приложения** приведет к открытию формы**Новое приложение**. Заполните запрошенные сведения, как показано на рисунке ниже.
    * **Имя приложения**: Выберите имя для приложения IoT Central. 
    * **URL-адрес**. Выберите URL-адрес IoT Central, платформа будет проверять его уникальность.
    * **Бесплатная пробная версия на 7 дней**: Если у вас уже есть подписка Azure, рекомендуется использовать параметр по умолчанию. Если у вас нет подписки Azure, начните с бесплатной пробной версии.
    * **Billing Info** (Данные для выставления счетов): Само приложение является бесплатным. Сведения о каталоге, подписке Azure и регионе необходимы для подготовки ресурсов для приложения.
    * Нажмите кнопку **Создать** в нижней части страницы, и приложение будет создано в течение минуты.
        ![Форма нового приложения](media/tutorial-iot-central-solar-panel/solar-panel-create-app.png)
        
        ![Данные для выставления счетов в форме нового приложения](media/tutorial-iot-central-solar-panel/solar-panel-create-app-billinginfo.png)


### <a name="verify-the-application-and-simulated-data"></a>Проверка приложения и имитации данных

Созданное приложение солнечной панели можно изменить в любое время. Убедитесь, что приложение развернуто и работает должным образом, прежде чем изменять его.

Чтобы проверить создание и моделирование данных приложения, перейдите на **Панель мониторинга**. Если вы видите плитки с данными — развертывание приложения прошло успешно. Моделирование данных может занять некоторое время, поэтому подождите 1-2 минуты. 

## <a name="application-walk-through"></a>Пошаговое руководство по приложению
После успешного развертывания шаблона приложения он поставляется с примерами интеллектуального измерительного устройства, модели устройства и панели мониторинга.

Adatum — это вымышленная компания, которая отслеживает солнечные панели и управляет ими. На панели мониторинга солнечных панелей можно увидеть свойства солнечных панелей, данные и примеры команд. Он позволяет операторам и командам поддержки заранее выполнять следующие действия перед их включением в службу поддержки.
* Ознакомление с последними сведениями о панели и расположении установки на карте.
* Заблаговременная проверка состояния панели и подключения.
* Обзор тенденций в производстве энергии и изменении температуры для выявления любых аномальных явлений.
* Мониторинг общего создания энергии для планирования и выставления счетов.
* Командные и управляющие операции, такие как активация панели и обновление прошивки. В шаблоне кнопки команд показывают возможные функции и не отправляют реальные команды.

> [!div class="mx-imgBorder"]
> ![Панель мониторинга солнечной панели](media/tutorial-iot-central-solar-panel/solar-panel-dashboard.png)

### <a name="devices"></a>Устройства
Приложение поставляется с примером устройства солнечной панели. Чтобы просмотреть сведения об устройстве, щелкните вкладку **Устройства**.

> [!div class="mx-imgBorder"]
> ![Устройства солнечной панели](media/tutorial-iot-central-solar-panel/solar-panel-device.png)


Щелкните ссылку примера устройства **SP0123456789**, чтобы просмотреть сведения об устройстве. Вы можете обновить доступные для записи свойства устройства на странице **Свойства обновления** и визуализировать обновленные значения на панели мониторинга. 

> [!div class="mx-imgBorder"]
> ![Свойства солнечной панели](media/tutorial-iot-central-solar-panel/solar-panel-device-properties.png)


### <a name="device-template"></a>Шаблон устройства
Щелкните вкладку **Шаблоны устройства**, чтобы просмотреть модель устройства солнечной панели. Модель имеет предварительно определенный интерфейс для данных, свойств, команд и представлений.

> [!div class="mx-imgBorder"]
> ![Шаблон устройств солнечной панели](media/tutorial-iot-central-solar-panel/solar-panel-device-templates.png)


## <a name="clean-up-resources"></a>Очистка ресурсов
Если вы решили не продолжать работу с этим приложением, удалите приложение, выполнив следующие действия.

1. В левой области откройте вкладку администрирования.
2. Выберите параметры приложения и нажмите кнопку "Удалить" в нижней части страницы. 

    > [!div class="mx-imgBorder"]
    > ![Удаление приложения](media/tutorial-iot-central-solar-panel/solar-panel-delete-app.png)

## <a name="next-steps"></a>Дальнейшие действия
* Дополнительные сведения об архитектуре приложений солнечной панели 
> [!div class="nextstepaction"]
> см. в статье [концепции](https://docs.microsoft.com/azure/iot-central/energy/concept-iot-central-solar-panel-app).
* Бесплатное создание шаблонов приложение солнечной панели, см. в разделе [Приложения солнечной панели](https://apps.azureiotcentral.com/build/new/solar-panel-monitoring)
* Дополнительные сведения об IoT Central, см. в разделе [Общие сведения об IoT Central](https://docs.microsoft.com/azure/iot-central/).