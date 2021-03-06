---
title: Устранение неполадок в службе "Сетка событий Azure" IoT Edge | Документация Майкрософт
description: Устранение неполадок в сетке событий на IoT Edge.
author: VidyaKukke
manager: rajarv
ms.author: vkukke
ms.reviewer: spelluru
ms.date: 07/08/2020
ms.topic: article
ms.openlocfilehash: 0196522618d4b61f615f7cc6faeacbe9a8c7c5b4
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2020
ms.locfileid: "86171352"
---
# <a name="common-issues"></a>Распространенные проблемы

При возникновении проблем с использованием службы "Сетка событий Azure" на IoT Edge в вашей среде используйте эту статью в качестве рекомендации по устранению неполадок и решению проблем.

## <a name="view-event-grid-module-logs"></a>Просмотр журналов модулей сетки событий

Для устранения неполадок может потребоваться доступ к журналам модулей сетки событий. Для этого на виртуальной машине, где развернут модуль, выполните следующую команду:

В Windows,

```sh
docker -H npipe:////./pipe/iotedge_moby_engine container logs eventgridmodule
```

В Linux,

```sh
sudo docker logs eventgridmodule
```

## <a name="unable-to-make-https-requests"></a>Не удалось выполнить запросы HTTPS

* Сначала **Убедитесь, что** модуль "Сетка событий" имеет значение **"входящий": serverAuth: тлсполици** **.**

* При обмене данными между модулями убедитесь, что вы выполняете вызов через порт **4438** , а имя модуля соответствует развернутому. 

  Например, если модуль сетки событий был развернут с именем **евентгридмодуле** , URL-адрес должен иметь значение **https://eventgridmodule:4438** . Убедитесь, что регистр и номер порта указаны правильно.
    
* Если это из модуля Интернета вещей, убедитесь, что порт сетки событий сопоставлен с хост-компьютером во время развертывания, например

    ```json
    "HostConfig": {
                "PortBindings": {
                  "4438/tcp": [
                    {
                        "HostPort": "4438"
                    }
                  ]
                }
     }
    ```

## <a name="unable-to-make-http-requests"></a>Не удается выполнить HTTP-запросы

* Сначала убедитесь, что модуль "Сетка событий" имеет значение **"входящий": serverAuth: тлсполици** ( **включено** или **отключено**).

* При обмене данными между модулями убедитесь, что вы выполняете вызов через порт **5888** , а имя модуля соответствует развернутому. 

  Например, если модуль сетки событий был развернут с именем **евентгридмодуле** , URL-адрес должен иметь значение **http://eventgridmodule:5888** . Убедитесь, что регистр и номер порта указаны правильно.
    
* Если это из модуля Интернета вещей, убедитесь, что порт сетки событий сопоставлен с хост-компьютером во время развертывания, например

    ```json
    "HostConfig": {
                "PortBindings": {
                  "5888/tcp": [
                    {
                        "HostPort": "5888"
                    }
                  ]
                }
    }
    ```

## <a name="certificate-chain-was-issued-by-an-authority-thats-not-trusted"></a>Цепочка сертификатов была выдана центром сертификации, который не является доверенным

По умолчанию модуль сетки событий настроен для проверки подлинности клиентов с сертификатом, выданным управляющей программой IoT Edge безопасности. Убедитесь, что клиент представляет сертификат, который является корневым для этой цепочки.

Класс **иотсекурити** в [https://github.com/Azure/event-grid-iot-edge](https://github.com/Azure/event-grid-iot-edge) показывает, как извлечь сертификаты из управляющей программы IOT Edge безопасности и использовать их для настройки исходящих вызовов.

Если это не производственная среда, можно отключить проверку подлинности клиента. Дополнительные сведения о том, как это сделать, см. в разделе [безопасность и проверка подлинности](security-authentication.md) .

## <a name="debug-events-not-received-by-subscriber"></a>События отладки, не полученные подписчиком

Типичные причины для этого:

* Событие никогда не было успешно Опубликовано. При размещении события в модуле сетки событий должно быть получено значение HTTP StatusCode, равное 200 (ОК).

* Проверьте подписку на событие, чтобы проверить следующее:
    * URL-адрес конечной точки является допустимым
    * Все фильтры в подписке не вызывают удаление события.

* Проверьте, выполняется ли модуль подписчика

* Войдите на виртуальную машину, где развернут модуль "Сетка событий", и просмотрите его журналы.

* Включите ведение журнала доставки, установив **Broker: логделиверисукцесс = true** и повторно развернув модуль сетки событий и повторив запрос. Включение ведения журнала на доставку может повлиять на пропускную способность и задержку. После завершения отладки мы рекомендуем сделать это обратно на **Broker: логделиверисукцесс = false**  и повторно развернуть модуль сетки событий.

* Включите метрики, задав **метрики: репортертипе = Console** и повторное развертывание модуля сетки событий. Любые операции после этого приведут к тому, что метрики будут регистрироваться в консоли модуля "Сетка событий", который можно использовать для дальнейшей отладки. Мы рекомендуем включить метрики только для отладки и однократного завершения, чтобы отключить их, задав **метрики: репортертипе = None** и повторное развертывание модуля сетки событий.

## <a name="next-steps"></a>Дальнейшие действия

Сообщите о проблемах, предложениях с использованием сетки событий для IoT Edge по адресу [https://github.com/Azure/event-grid-iot-edge/issues](https://github.com/Azure/event-grid-iot-edge/issues) .