---
title: Реализация динамического стиля для схем помещений Azure Maps Creator
description: Узнайте, как реализовать динамический стиль для схем помещений Creator
author: anastasia-ms
ms.author: v-stharr
ms.date: 05/18/2020
ms.topic: how-to
ms.service: azure-maps
services: azure-maps
manager: philmea
ms.openlocfilehash: a96a62d7bb93f0ede6b16008dc844ad7f1a8c8d2
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2020
ms.locfileid: "86517303"
---
# <a name="implement-dynamic-styling-for-creator-indoor-maps"></a>Реализация динамического стиля для схем помещений Creator

[Служба состояния признаков](https://docs.microsoft.com/rest/api/maps/featurestate) Azure Maps Creator позволяет применять стили на основе динамических свойств признаков схем помещений.  Например, можно отобразить конференц-залы в определенном цвете в зависимости от состояния заполнения. В этой статье мы покажем, как динамически преобразовывать для просмотра признаки схемы с помощью [службы состояния признаков](https://docs.microsoft.com/rest/api/maps/featurestate) и [веб-модуля схем помещений](how-to-use-indoor-module.md).

## <a name="prerequisites"></a>Предварительные требования

1. [Создайте учетную запись службы Azure Maps.](quick-demo-map-app.md#create-an-azure-maps-account)
2. [Получите первичный ключ подписки](quick-demo-map-app.md#get-the-primary-key-for-your-account), который иногда называется первичным ключом или ключом подписки.
3. [Создайте ресурс Creator](how-to-manage-creator.md).
4. Скачайте [пример пакета рисунков](https://github.com/Azure-Samples/am-creator-indoor-data-examples).
5. [Создайте схему помещения](tutorial-creator-indoor-maps.md), чтобы получить `tilesetId` и `statesetId`.
6. Выполните сборку веб-приложения по инструкциям, описанным в статье [Использование модуля схем помещений](how-to-use-indoor-module.md).

В этом руководстве используется приложение [Postman](https://www.postman.com/), но вы можете выбрать другую среду разработки API.

## <a name="implement-dynamic-styling"></a>Реализация динамического стиля

После выполнения предварительных требований у вас должно быть простое веб-приложение, настроенное с использованием ключа подписки, `tilesetId` и `statesetId`.

### <a name="select-features"></a>Выбор функций

Для реализации динамического стиля, на признак, такой как конференц-зал или переговорная, нужно ссылаться по его идентификатору (`id`). Используя значение `id` признака, можно обновить его динамическое свойство или *состояние*. Для просмотра признаков, определенных в наборе данных, можно использовать один из следующих методов.

* API WFS (Web Feature Service). Наборы данных можно запрашивать с помощью API WFS. WFS соответствует признакам API Открытого геопространственного консорциума. API WFS удобно использовать для запросов к признакам в наборе данных. Например, WFS можно использовать для поиска всех конференц-залов среднего размера на конкретном этаже здания.

* Реализуйте настроенный код, позволяющий выбирать признаки на схеме с помощью веб-приложения. В этой статье мы будем использовать этот вариант.  

Следующий скрипт реализует событие щелчка мыши. Код извлекает значение `id` признака на основе точки, в которой находился указатель при щелчке. В приложении можно вставить код под блоком кода Indoor Manager. Запустите приложение и проверьте консоль, чтобы получить значение `id` признака в точке, в которой находился указатель при щелчке.

```javascript
/* Upon a mouse click, log the feature properties to the browser's console. */
map.events.add("click", function(e){

    var features = map.layers.getRenderedShapes(e.position, "indoor");

    var result = features.reduce(function (ids, feature) {
        if (feature.layer.id == "indoor_unit_office") {
            console.log(feature);
        }
    }, []);
});
```

По инструкциям из руководства [Создание схемы помещения](tutorial-creator-indoor-maps.md) вы настроили набор состояний функции, чтобы принимать обновления состояния для `occupancy`.

В следующем разделе для офиса `UNIT26` будет задано *состояние* заполнения `true`. А для офиса `UNIT27` будет задано состояние `false`.

### <a name="set-occupancy-status"></a>Указание состояния заполнения

 Теперь мы обновим состояние двух офисов (`UNIT26` и `UNIT27`):

1. В приложении Postman выберите **New** (Создать). В окне **Create New** (Создание) выберите **Request** (Запрос). Введите **имя запроса** и выберите коллекцию. Щелкните **Сохранить**.

2. Чтобы обновить состояние, используйте [API обновления состояний признаков](https://docs.microsoft.com/rest/api/maps/featurestate/updatestatespreview). Передайте идентификатор набора состояний и `UNIT26` для одного из двух офисов. Добавьте ключ подписки на Azure Maps. Ниже приведен URL-адрес запроса **POST** для обновления состояния:

    ```http
    https://atlas.microsoft.com/featureState/state?api-version=1.0&statesetID={statesetId}&featureID=UNIT26&subscription-key={Azure-Maps-Primary-Subscription-key}
    ```

3. В **заголовках** запроса **POST** установите для параметра `Content-Type` значение `application/json`. В **тексте** запроса **POST** напишите следующий код JSON с обновлениями признаков. Обновление будет сохранено только в том случае, если отметка времени публикации будет позже метки времени, используемой в предыдущих запросах на обновление состояния признака для того же значения `ID` признака. Передайте значение occupied для параметра `keyName`, чтобы обновить его.

    ```json
    {
        "states": [
            {
                "keyName": "occupied",
                "value": true,
                "eventTimestamp": "2019-11-14T17:10:20"
            }
        ]
    }
    ```

4. Повторите шаги 2 и 3 для `UNIT27`, используя следующий код JSON.

    ``` json
    {
        "states": [
            {
                "keyName": "occupied",
                "value": false,
                "eventTimestamp": "2019-11-14T17:10:20"
            }
        ]
    }
    ```

### <a name="visualize-dynamic-styles-on-a-map"></a>Визуализация динамических стилей на схеме

Веб-приложение, которое вы ранее открыли в браузере, теперь должно отображать обновленное состояние для признаков схемы. Объект `UNIT27`(151) должен отображаться зеленым цветом, а `UNIT26`(157) — красным.

![Свободное помещение выделено зеленым, а занятое помещение — красным](./media/indoor-map-dynamic-styling/room-state.png)

## <a name="next-steps"></a>Дальнейшие действия

Дополнительные сведения см. в следующих материалах:

> [!div class="nextstepaction"]
> [Creator для создания схем помещений](creator-indoor-maps.md)

См. ссылки на API, упомянутые в этой статье:

> [!div class="nextstepaction"]
> [Data — Upload Preview](creator-indoor-maps.md#upload-a-drawing-package) (Данные — предварительная версия отправки)

> [!div class="nextstepaction"]
> [Преобразование данных](creator-indoor-maps.md#convert-a-drawing-package)

> [!div class="nextstepaction"]
> [Набор данных](creator-indoor-maps.md#datasets)

> [!div class="nextstepaction"]
> [Набор фрагментов](creator-indoor-maps.md#tilesets)

> [!div class="nextstepaction"]
> [Набор состояний функции](creator-indoor-maps.md#feature-statesets)

> [!div class="nextstepaction"]
> [Служба WFS](creator-indoor-maps.md#web-feature-service-api)

