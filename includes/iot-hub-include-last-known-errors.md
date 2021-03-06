---
title: включить файл
description: включить файл
author: robinsh
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 04/22/2019
ms.author: robinsh
ms.custom: include file
ms.openlocfilehash: d03579f704879bd8d012bb0bb326659d1f778dee
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2020
ms.locfileid: "84793295"
---
[Получение работоспособности конечной точки](https://docs.microsoft.com/rest/api/iothub/iothubresource/getendpointhealth#iothubresource_getendpointhealth) в REST API предоставляет состояние работоспособности конечных точек, а также последнюю известную ошибку для определения причины, по которой конечная точка находится в неработоспособном состоянии. В следующей таблице перечислены наиболее распространенные ошибки.

|Последняя известная ошибка|Описание/когда происходит|Возможное устранение рисков|
|-----|-----|-----|
|Временный|Произошла временная ошибка, и центр Интернета вещей повторит операцию.|Обратите внимание на маршруты [журналов диагностики](https://docs.microsoft.com/azure/iot-hub/iot-hub-monitor-resource-health#routes).|
|InternalError|Произошла ошибка при доставке сообщения в конечную точку.|Это внутреннее исключение, но также обратите внимание на [журналы диагностики](https://docs.microsoft.com/azure/iot-hub/iot-hub-monitor-resource-health#routes)маршрутов.|
|Не авторизовано|Центр Интернета вещей не имеет прав для отправки сообщений в указанную конечную точку.|Проверьте, что строка подключения актуальна для конечной точки. Если оно изменилось, рассмотрите возможность обновления в центре Интернета вещей. Если конечная точка использует управляемое удостоверение, убедитесь, что у субъекта центра Интернета вещей есть необходимые разрешения на целевом объекте.|
|Ожидает повтора|Центр Интернета вещей регулируется при записи сообщений в конечную точку.|Проверьте ограничения регулирования для затронутой конечной точки. Измените конфигурации для конечной точки, если это необходимо.|
|Время ожидания|Время ожидания операции истекло.|Повторите операцию.|
|Не найдено|Целевой ресурс не существует.|Убедитесь, что целевой ресурс существует.|
|Контейнер не найден|Контейнер хранилища не существует.|Убедитесь, что контейнер хранилища существует.|
|Контейнер отключен|Контейнер хранилища отключен.|Убедитесь, что контейнер хранилища включен.|
|максмессажесизиксцеедед|Размер сообщения для маршрутизации сообщений ограничен 256 КБ. размер отправляемого сообщения превысил это ограничение.|Проверьте, можно ли уменьшить размер сообщения, используя меньше свойств приложения или меньшее число дополнений сообщений.|
|партитионинганддупликатедетектионнотсуппортед|Возможно, для служебной шины не включено обнаружение дубликатов.|Отключите обнаружение дубликатов из служебной шины или используйте сущность без обнаружения дубликатов.|
|сессионфулентитинотсуппортед|В служебной шине могут быть отключены сеансы.|Отключите сеанс из служебной шины или используйте сущность без сеансов.|
|номатчингсубскриптионсформессаже|Подписка на запись сообщений в разделе служебной шины отсутствует.|Создайте подписку для сообщений центра Интернета вещей для маршрутизации.|
|ендпоинтекстерналлидисаблед|Конечная точка не находится в активном состоянии, поэтому центр Интернета вещей может отправить ему сообщения.|Включите конечную точку, чтобы вернуть ее в активное состояние.|
|DeviceMaximumQueueDepthExceeded|Достигнут предел размера служебной шины.|Рассмотрите возможность удаления сообщений из целевых концентраторов событий, чтобы новые сообщения принимались в концентраторы событий.|