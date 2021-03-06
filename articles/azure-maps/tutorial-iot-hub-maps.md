---
title: Руководство по Реализация пространственной аналитики Интернета вещей | Microsoft Azure Maps
description: Руководство по интеграции Центра Интернета вещей с API-интерфейсами службы Azure Maps
author: anastasia-ms
ms.author: v-stharr
ms.date: 09/01/2020
ms.topic: tutorial
ms.service: azure-maps
services: azure-maps
manager: philmea
ms.custom: mvc
ms.openlocfilehash: 3eb405783b16d1bb7de27f6638dba394457601c8
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2020
ms.locfileid: "91321838"
---
# <a name="tutorial-implement-iot-spatial-analytics-by-using-azure-maps"></a>Руководство по Реализация пространственной аналитики Интернета вещей с помощью Azure Maps

В сценарии IoT зачастую фиксируются и отслеживаются соответствующие события, происходящие в пространстве и времени. Примерами могут служить управление автопарком, отслеживание активов, приложения мобильности и "умного" города. В этом учебнике вы изучите решение, которое отслеживает передвижение арендованных автомобилей, используя API-интерфейсы Azure Maps.

Изучив данный учебник, вы научитесь:

> [!div class="checklist"]
> * Создавать учетную запись хранения Azure для записи данных отслеживания автомобилей.
> * Передавать данные о геозоне в службу данных Azure Maps с помощью API отправки данных.
> * Создавать центр в Центре Интернета вещей Azure и регистрировать устройства.
> * Создавать функции в Функциях Azure, которая реализует бизнес-логику на основе пространственной аналитики Azure Maps.
> * Подписываться на события телеметрии устройств Интернета вещей из функции Azure через службу "Сетка событий Azure".
> * Фильтровать события телеметрии с помощью маршрутизации сообщений, предоставляемой Центром Интернета вещей.

## <a name="prerequisites"></a>Предварительные требования

1. Войдите на [портал Azure](https://portal.azure.com).

2. [Создайте учетную запись службы Azure Maps.](quick-demo-map-app.md#create-an-azure-maps-account)

3. [Получите первичный ключ подписки](quick-demo-map-app.md#get-the-primary-key-for-your-account), который иногда называется первичным ключом или ключом подписки. Дополнительные сведения см. в статье [Управление аутентификацией в Azure Maps](how-to-manage-authentication.md).

4. [Создайте группу ресурсов.](https://docs.microsoft.com/azure/azure-resource-manager/management/manage-resource-groups-portal#create-resource-groups) В этом учебнике мы назовем нашу группу ресурсов *ContosoRental*, но можно выбрать любое имя.

5. Скачайте проект C# [rentalCarSimulation](https://github.com/Azure-Samples/iothub-to-azure-maps-geofencing/tree/master/src/rentalCarSimulation).

В этом учебнике используется приложение [Postman](https://www.postman.com/), но вы можете выбрать другую среду разработки API.

## <a name="use-case-rental-car-tracking"></a>Вариант использования: отслеживание арендованных автомобилей

Допустим, компания по аренде автомобилей хочет регистрировать информацию о местоположении, пройденном расстоянии и рабочем состоянии своих транспортных средств. Кроме того, она хочет сохранять эти сведения каждый раз, когда автомобиль покидает правильный авторизованный географический регион.

Арендованные автомобили оснащаются устройствами Интернета вещей, которые регулярно отправляют данные телеметрии в Центр Интернета вещей. Данные телеметрии содержат текущее расположение и сведения о том, работает ли двигатель автомобиля. Схема расположения устройств соответствует схеме [IoT Plug and Play для геопространственных данных.](https://github.com/Azure/opendigitaltwins-dtdl/blob/master/DTDL/v1-preview/schemas/geospatial.md) Схема телеметрии устройства для арендованного автомобиля выглядит примерно так как следующий код JSON:

```JSON
{
    "data": {
        "properties": {
            "Engine": "ON"
        },
        "systemProperties": {
            "iothub-content-type": "application/json",
            "iothub-content-encoding": "utf-8",
            "iothub-connection-device-id": "ContosoRentalDevice",
            "iothub-connection-auth-method": "{\"scope\":\"device\",\"type\":\"sas\",\"issuer\":\"iothub\",\"acceptingIpFilterRule\":null}",
            "iothub-connection-auth-generation-id": "636959817064335548",
            "iothub-enqueuedtime": "2019-06-18T00:17:20.608Z",
            "iothub-message-source": "Telemetry"
        },
        "body": {
            "location": {
                "type": "Point",
                "coordinates": [ -77.025988698005662, 38.9015330523316 ]
            }
        }
    }
}
```

В этом учебнике рассматривается отслеживание только одного автомобиля. После настройки служб Azure необходимо скачать проект C# [rentalCarSimulation](https://github.com/Azure-Samples/iothub-to-azure-maps-geofencing/tree/master/src/rentalCarSimulation), чтобы запустить симулятор автомобиля. Весь процесс, от события к исполнению функций, приведен в следующих шагах:

1. Устройство, которое находится в автомобиле, отправляет данные телеметрии в Центр Интернета вещей.

2. Если двигатель автомобиля работает, центр публикует данные телеметрии в Сетке событий.

3. Функция Azure срабатывает из-за подписки на события телеметрии устройства.

4. Функция регистрирует координаты расположения устройства автомобиля, время события и идентификатор устройства. Затем она использует [API определения пространственной геозоны](https://docs.microsoft.com/rest/api/maps/spatial/getgeofence), чтобы определить, выехал ли автомобиль за пределы геозоны. Если он находится за пределами геозоны, функция сохраняет данные о местоположении, полученные от события, в наш контейнер больших двоичных объектов. Кроме того, эта функция запрашивает [обратное преобразование найденных адресов](https://docs.microsoft.com/rest/api/maps/search/getsearchaddressreverse), чтобы перевести координаты местоположения в адрес улицы и сохранить его с остальными данными о расположении устройства.

На следующей схеме представлено обобщенное описание всей системы.

   :::image type="content" source="./media/tutorial-iot-hub-maps/system-diagram.png" border="false" alt-text="Схема обзора системы.":::

На следующем рисунке представлена область геозоны, выделенная синим цветом. Маршрут арендованного автомобиля обозначен зеленой линией.

   :::image type="content" source="./media/tutorial-iot-hub-maps/geofence-route.png" border="false" alt-text="Схема обзора системы.":::

## <a name="create-an-azure-storage-account"></a>Создание учетной записи хранения Azure

Чтобы сохранить данные отслеживания нарушений автомобиля, создайте [учетную запись хранения общего назначения версии 2](https://docs.microsoft.com/azure/storage/common/storage-account-overview#general-purpose-v2-accounts) в группе ресурсов. Если вы еще не создали группу ресурсов, следуйте указаниям в разделе [Создание группы ресурсов](https://docs.microsoft.com/azure/azure-resource-manager/management/manage-resource-groups-portal#create-resource-groups). Для выполнения инструкций в этом учебнике назовите группу ресурсов *ContosoRental*.

Чтобы создать учетную запись хранения, следуйте инструкциям в статье [Создание учетной записи хранения Azure](https://docs.microsoft.com/azure/storage/common/storage-account-create?tabs=azure-portal). Для прохождения этого учебника используйте имя учетной записи хранения *contosorentalstorage*, но вообще можно назвать ее как угодно.

После успешного создания учетной записи хранения необходимо создать контейнер для хранения данных журнала.

1. Перейдите к только что созданной учетной записи хранения. В разделе **Основные компоненты** щелкните ссылку **Контейнеры**.

    :::image type="content" source="./media/tutorial-iot-hub-maps/containers.png" alt-text="Схема обзора системы.":::

2. В верхнем левом углу щелкните **+ Контейнер**. В правой части браузера отобразится панель. Присвойте контейнеру имя *contoso-rental-logs* и выберите **Создать**.

     :::image type="content" source="./media/tutorial-iot-hub-maps/container-new.png" alt-text="Схема обзора системы." и добавление подписки на службу "Сетка событий".

    :::image type="content" source="./media/tutorial-iot-hub-maps/access-keys.png" alt-text="Схема обзора системы.":::

## <a name="upload-a-geofence"></a>Отправка геозоны

Далее можно использовать [приложение Postman](https://www.getpostman.com), чтобы [отправить геозону](https://docs.microsoft.com/azure/azure-maps/geofence-geojson) в Azure Maps. Геозоны определяют авторизованное географическое расположение для арендованного автомобиля. Геозоны будут использоваться в функции Azure, чтобы определить, переместился ли автомобиль за пределы области геозоны.

Выполните следующие действия, чтобы отправить геозоны с помощью API отправки данных Azure Maps. 

1. Откройте приложение POST и выберите **New** (Создать). В окне **Create New** (Создание) выберите **Collection** (Коллекция). Присвойте имя коллекции и нажмите кнопку **Create** (Создать).

2. Чтобы создать запрос, нажмите кнопку **Создать** еще раз. В окне **Create New** (Создание) выберите **Request** (Запрос) и введите имя запроса. Выберите коллекцию, созданную на предыдущем шаге, а затем нажмите кнопку **Сохранить**.

3. Выберите HTTP-метод **POST** на вкладке построителя и введите следующий URL-адрес, чтобы отправить данные о геозоне в API отправки данных. Обязательно замените `{subscription-key}` ключом первичной подписки.

    ```HTTP
    https://atlas.microsoft.com/mapData/upload?subscription-key={subscription-key}&api-version=1.0&dataFormat=geojson
    ```

    В URL-адрес значение `geojson` для параметра `dataFormat` в пути URL-адреса определяет формат отправляемых данных.

4. Выберите **Body** > **raw** (Текст > необработанный) для формата входных данных и щелкните **JSON** в раскрывающемся списке. [Откройте файл данных JSON](https://raw.githubusercontent.com/Azure-Samples/iothub-to-azure-maps-geofencing/master/src/Data/geofence.json?token=AKD25BYJYKDJBJ55PT62N4C5LRNN4) и скопируйте JSON в раздел текста. Нажмите кнопку **Отправить**.

5. Нажмите **Send** (Отправить) и дождитесь обработки запроса. После выполнения запроса перейдите на вкладку **Заголовки** ответа. Скопируйте значение ключа **Расположение**, который является `status URL`.

    ```http
    https://atlas.microsoft.com/mapData/operations/<operationId>?api-version=1.0
    ```

6. Чтобы проверить состояние вызова API, создайте HTTP-запрос **GET** по `status URL`. Вам потребуется добавить первичный ключ подписки к URL-адресу для проверки подлинности. Запрос **GET** должен быть похож на следующий URL-адрес.

   ```HTTP
   https://atlas.microsoft.com/mapData/<operationId>/status?api-version=1.0&subscription-key={subscription-key}

7. When the **GET** HTTP request completes successfully, it returns a `resourceLocation`. The `resourceLocation` contains the unique `udid` for the uploaded content. Copy this `udid` for later use in this tutorial.

      ```json
      {
          "status": "Succeeded",
          "resourceLocation": "https://atlas.microsoft.com/mapData/metadata/{udid}?api-version=1.0"
      }
      ```

## <a name="create-an-iot-hub"></a>Создание центра Интернета вещей

Центр Интернета вещей обеспечивает безопасный и надежный двунаправленный обмен данными между приложением Интернета вещей и управляемыми ими устройствами. Для прохождения данного учебника необходимо получить информацию с устройства в автомобиле, чтобы определить его расположение. В этом разделе описано, как создать центр Интернета вещей в группе ресурсов *ContosoRental*. Этот центр будет отвечать за публикацию событий телеметрии устройства.

> [!NOTE]
> Функция публикации событий телеметрии устройства в службу "Сетка событий" доступна в режиме общедоступной предварительной версии. Она доступна во всех регионах, кроме следующих: восточная часть США, западная часть США, Западная Европа, Azure для государственных организаций, Azure для Китая (21Vianet) и Azure для Германии.

Чтобы создать центр Интернета вещей в группе ресурсов *ContosoRental*, выполните действия, описанные в разделе [Создание центра Интернета вещей](https://docs.microsoft.com/azure/iot-hub/quickstart-send-telemetry-dotnet#create-an-iot-hub).

## <a name="register-a-device-in-your-iot-hub"></a>Регистрация устройства в центре Интернета вещей

Устройства не могут подключиться к центру Интернета вещей, если они не зарегистрированы в реестре удостоверений центра. В данном случае вы создадите одно устройство с именем *InVehicleDevice*. Чтобы создать и зарегистрировать устройство в центре Интернета вещей, выполните действия, описанные в разделе [Регистрация нового устройства в центре Интернета вещей](https://docs.microsoft.com/azure/iot-hub/iot-hub-create-through-portal#register-a-new-device-in-the-iot-hub). Не забудьте скопировать основную строку подключения устройства. Он понадобится вам позднее.

## <a name="create-a-function-and-add-an-event-grid-subscription"></a>Создание функции и добавление подписки на службу "Сетка событий"

Функции Azure — это бессерверная служба вычислений, которая позволяет запускать небольшие фрагменты кода (функции) без необходимости явно подготавливать вычислительную инфраструктуру или управлять ею. Дополнительные сведения см. в статье о службе [Функции Azure](https://docs.microsoft.com/azure/azure-functions/functions-overview).

Функция активируется определенным событием. В данном случае вы создадите функцию, активируемую триггером Сетки событий. Создайте связь между триггером и функцией, создав подписку на события для событий телеметрии устройства Центра Интернета вещей. При возникновении события телеметрии устройства наша функция будет вызываться как конечная точка и будет получать соответствующие данные для устройства, которое ранее было зарегистрировано в Центре Интернета вещей.

[Код скрипта на C#, который будет содержать наша функция, размещен здесь](https://github.com/Azure-Samples/iothub-to-azure-maps-geofencing/blob/master/src/Azure%20Function/run.csx).

Теперь настройте функцию Azure.

1. На панели мониторинга портала Azure щелкните **Создать ресурс**. В поле поиска введите **Приложение-функция**. Выберите **Приложение-функция** > **Создать**.

1. На странице создания **приложения-функции** присвойте имя приложению-функции. В разделе **Группа ресурсов** в раскрывающемся списке выберите **ContosoRental**. Выберите **.NET Core** в качестве значения для параметра **Стек среды выполнения**. В нижней части страницы нажмите кнопку **Далее: Размещение >** .

    :::image type="content" source="./media/tutorial-iot-hub-maps/rental-app.png" alt-text="Схема обзора системы.":::

1. **Учетная запись хранилища** — выберите учетную запись, которую вы создали на [этапе создания учетной записи](#create-an-azure-storage-account). Выберите **Review + create** (Просмотреть и создать).

1. Просмотрите данные о приложении-функции и щелкните **Создать**.

1. После создания приложения необходимо добавить в него функцию. Перейдите к приложению-функции. Перейдите на панель **Функции**. В верхней части страницы выберите **+ Добавить**. Отобразится панель шаблона функции. Прокрутите панель вниз и выберите **триггер Сетки событий Azure**.

     >[!IMPORTANT]
    > Шаблоны **Триггер концентратора событий Azure** и **триггер Сетки событий Azure** имеют похожие имена. Убедитесь, что выбран шаблон **триггер для Сетки событий Azure**.

    :::image type="content" source="./media/tutorial-iot-hub-maps/function-create.png" alt-text="Схема обзора системы.":::

1. Присвойте функции имя. В этом учебнике мы будем использовать имя *GetGeoFunction*, но в общем можно использовать любое имя. Выберите **Создать функцию**.

1. В меню слева выберите область **Code + Test** (Код и тестирование). Скопируйте и вставьте [скрипт C#](https://github.com/Azure-Samples/iothub-to-azure-maps-geofencing/blob/master/src/Azure%20Function/run.csx) в окно кода.

     :::image type="content" source="./media/tutorial-iot-hub-maps/function-code.png" alt-text="Схема обзора системы.":::

1. В коде C# замените следующие параметры:
    * Замените значение **SUBSCRIPTION_KEY** первичным ключом подписки для учетной записи Azure Maps.
    * Замените значение **UDID** идентификатором `udid` отправленной ранее геозоны в разделе [Отправка геозоны](#upload-a-geofence).
    * Функция `CreateBlobAsync` в скрипте создает большой двоичный объект для каждого события в учетной записи хранения данных. Замените заполнители **ACCESS_KEY**, **ACCOUNT_NAME** и **STORAGE_CONTAINER_NAME** значениями ключа доступа к учетной записи хранения, имени учетной записи и контейнера хранилища данных. Эти значения были созданы при создании учетной записи хранения в [соответствующем](#create-an-azure-storage-account) разделе.

1. В меню слева выберите область **Интеграция**. Выберите на схеме **Триггер сетки событий**. Введите имя триггера, например *eventGridEvent*, и щелкните **Создание подписки для Сетки событий**.

     :::image type="content" source="./media/tutorial-iot-hub-maps/function-integration.png" alt-text="Схема обзора системы.":::

1. Заполните сведения о подписке. Назовите подписку на событие. Для пункта **Схема событий** выберите **Схема сетки событий**. Для пункта **Topic Types** (Типы разделов) выберите **Azure IoT Hub Accounts** (Учетные записи Центра Интернета вещей Azure). Выберите **Группы ресурсов**, а затем созданную для этого учебника группу ресурсов. Для пункта **Ресурс** выберите центр Интернета вещей, созданный при прохождении раздела "Создание центра Интернета вещей Azure". Для пункта **Filter to Event Types** (Фильтр по типам событий) выберите **Device Telemetry** (Телеметрия устройства).

   Завершив выбор параметров, вы увидите, что значение для параметра **Тип раздела** изменится на **Центр Интернета вещей**. Для пункта **System Topic Name** (Имя системного раздела) можно использовать то же имя, что и для ресурса. Наконец, в разделе **Сведения о конечной точке** нажмите **Выбрать конечную точку**. Примите все параметры и нажмите **Подтвердить выбор**.

    :::image type="content" source="./media/tutorial-iot-hub-maps/function-create-event-subscription.png" alt-text="Схема обзора системы.":::

1. Проверьте настройки. Убедитесь, что конечная точка указывает функцию, созданную в начале этого раздела. Нажмите кнопку **создания**.

    :::image type="content" source="./media/tutorial-iot-hub-maps/function-create-event-subscription-confirm.png" alt-text="Схема обзора системы.":::

1. Теперь вы снова находитесь на панели **Изменить триггер**. Щелкните **Сохранить**.

## <a name="filter-events-by-using-iot-hub-message-routing"></a>Фильтрация событий с помощью маршрутизации сообщений, предоставляемой Центром Интернета вещей

При добавлении подписки службы "Сетка событий" в функцию Azure маршрут обмена сообщениями автоматически создается в указанном центре Интернета вещей. Маршрутизация сообщений позволяет маршрутизировать различные типы данных к разным конечным точкам. Вы можете, например, маршрутизировать сообщения телеметрии устройства, события жизненного цикла устройства и события изменения двойников устройств. Дополнительные сведения см. в статье [Использование маршрутизации сообщений Центра Интернета вещей](https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-messages-d2c).

:::image type="content" source="./media/tutorial-iot-hub-maps/hub-route.png" alt-text="Схема обзора системы.":::

В этом примере сценария необходимо получать сообщения только при перемещении автомобиля. Создайте запрос маршрутизации для фильтрации событий, в которых значение свойства `Engine` равно **ON**. Чтобы создать запрос маршрутизации, выберите маршрут **RouteToEventGrid** и замените значение **Запрос маршрутизации** на строку **"Engine='ON'"** . Затем нажмите кнопку **Save** (Сохранить). Теперь центр Интернета вещей будет публиковать только те события телеметрии устройств, в которых параметр Engine имеет значение ON.

:::image type="content" source="./media/tutorial-iot-hub-maps/hub-filter.png" alt-text="Схема обзора системы.":::

>[!TIP]
>Запросить сообщения, отправляемые с устройства Интернета вещей в облако, можно несколькими способами. Дополнительные сведения о синтаксисе см. в статье о [маршрутизации сообщений с помощью Центра Интернета вещей](https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-routing-query-syntax).

## <a name="send-telemetry-data-to-iot-hub"></a>Отправка данных телеметрии в Центр Интернета вещей

Когда функция Azure работает, можно отправить данные телеметрии в центр Интернета вещей, который передаст их в Сетку событий. Используйте приложение C# для имитации данных о местоположении, получаемых с устройства в арендованном автомобиле. Чтобы запустить это приложение, на компьютере разработки необходимо установить пакет SDK для .NET Core версии 2.1.0 или более поздней. Выполните эти действия, чтобы отправить имитированные данные телеметрии в центр Интернета вещей.

1. Если вы еще не сделали этого, скачайте проект [rentalCarSimulation](https://github.com/Azure-Samples/iothub-to-azure-maps-geofencing/tree/master/src/rentalCarSimulation) на C#.

2. Откройте файл `simulatedCar.cs` в любом текстовом редакторе и замените в нем значение `connectionString` тем, которое вы сохранили при регистрации устройства. Сохраните изменения в файле.

3. Убедитесь, что на компьютере установлена платформа .NET Core. В локальном окне терминала перейдите к корневой папке проекта C# и выполните следующие команды, чтобы установить необходимые пакеты для приложения имитированного устройства.

    ```cmd/sh
    dotnet restore
    ```

4. В том же окне терминала выполните команды, которые компилируют и запускают приложение имитации устройства в арендованном автомобиле:

    ```cmd/sh
    dotnet run
    ```


  Теперь окно терминала должно содержать примерно такие данные:

:::image type="content" source="./media/tutorial-iot-hub-maps/terminal.png" alt-text="Схема обзора системы.":::

Теперь вы можете открыть контейнер хранилища больших двоичных объектов и убедиться, что в нем созданы четыре двоичных объекта для расположений, в которых автомобиль находится за пределами геозоны.

:::image type="content" source="./media/tutorial-iot-hub-maps/blob.png" alt-text="Схема обзора системы.":::

На следующей карте показаны четыре точки обнаружения автомобилей за пределами геозоны. Каждое расположение записывается в журнал через регулярные интервалы времени.

:::image type="content" source="./media/tutorial-iot-hub-maps/violation-map.png" alt-text="Схема обзора системы.":::

## <a name="explore-azure-maps-and-iot"></a>Знакомство с Azure Maps и Интернетом вещей

Для изучения API-интерфейсов Azure Maps, используемых в этом учебнике, прочтите следующие статьи.

* [Get Search Address Reverse](https://docs.microsoft.com/rest/api/maps/search/getsearchaddressreverse)
* [Получение геозоны](https://docs.microsoft.com/rest/api/maps/spatial/getgeofence)

Полный список API-интерфейсов Azure Maps вы найдете здесь:

* [Интерфейсы REST API для Azure Maps](https://docs.microsoft.com/rest/api/maps/spatial/getgeofence)

* статье об [IoT Plug and Play](https://docs.microsoft.com/azure/iot-pnp).

Список устройств, сертифицированных для работы с Интернетом вещей в Azure, размещен на этой странице:

* [Сертифицированные для Azure устройства](https://catalog.azureiotsolutions.com/)

## <a name="next-steps"></a>Дальнейшие действия

Дополнительные сведения о том, как отправлять данные телеметрии с устройства в облако или обратно, см. здесь:


> [!div class="nextstepaction"]
> [Отправка данных телеметрии с устройства](https://docs.microsoft.com/azure/iot-hub/quickstart-send-telemetry-dotnet)