---
title: Что такое издатель OPC — Azure | Документация Майкрософт
description: В этой статье приведены общие сведения о функциях OPC Publisher. OPC Publisher дает возможность публиковать закодированные данные телеметрии JSON с помощью полезных данных JSON в Центре Интернета вещей Azure.
author: dominicbetts
ms.author: dobett
ms.date: 06/10/2019
ms.topic: overview
ms.service: industrial-iot
services: iot-industrialiot
manager: philmea
ms.custom:
- amqp
- mqtt
ms.openlocfilehash: 49ca12ed4f408e2a3fce1c8e59f541778f35311e
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/25/2020
ms.locfileid: "91281785"
---
# <a name="what-is-opc-publisher"></a>Что такое издатель OPC?

> [!IMPORTANT]
> Актуальную информацию по этой теме см. в статье [Промышленный Интернет вещей в Azure](https://azure.github.io/Industrial-IoT/).

Издатель OPC —это эталонная реализация, демонстрирующая следующее:

- подключения к имеющимся серверам на основе унифицированной архитектуры (UA) OPC;
- публикации данных телеметрии в кодировке JSON с серверов OPC UA в формате "публикация/подписка" OPC UA с использованием полезных данных JSON в Центре Интернета вещей.

Вы можете использовать любой из транспортных протоколов, которые поддерживает пакет SDK клиента Центра Интернета вещей: HTTPS, AMQP и MQTT.

Эталонная реализация включает:

- Клиент *OPC UA* для подключения к имеющимся серверам OPC UA в вашей сети.
- *Сервер* OPC UA, подключенный через порт 62222, который можно использовать для управления публикациями и предлагающий прямые методы для выполнения аналогичных операций с помощью Центра Интернета вещей.

Вы можете скачать [эталонную реализацию издателя OPC](https://github.com/Azure/iot-edge-opc-publisher) с GitHub.

Приложение реализовано на базе .NET Core и его можно запускать на платформах, которые поддерживает .NET Core.

Издатель OPC внедряет логику повторного выполнения для установки подключений к конечным точкам, которые не отвечают на определенное количество запросов проверки активности. Например, когда сервер OPC UA перестает отвечать из-за отключения питания.

Для каждого отдельного интервала публикации на сервер OPC UA создается отдельная подписка, где обновляются все узлы в этом интервале публикации.

Издатель OPC поддерживает пакетную обработку данных, отправленных в Центр Интернета вещей для снижения нагрузки на сеть. При пакетной обработке пакет отправляется в Центр Интернета вещей, только если достигнут настроенный размер пакета.

Это приложение использует эталонный стек OPC Foundation OPC UA в качестве пакетов NuGet. Ознакомьтесь с условиями лицензирования по адресу [https://opcfoundation.org/license/redistributables/1.3/](https://opcfoundation.org/license/redistributables/1.3/).

## <a name="next-steps"></a>Дальнейшие действия

Итак, вы узнали, что представляет собой издатель OPC. Далее мы рекомендуем ознакомиться со следующей статьей:

[Настройка издателя OPC](howto-opc-publisher-configure.md)