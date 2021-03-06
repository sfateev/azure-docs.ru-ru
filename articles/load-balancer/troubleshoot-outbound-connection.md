---
title: Устранение неполадок исходящих подключений в Azure Load Balancer
description: Способы решения распространенных проблем с исходящим подключением через Azure Load Balancer.
services: load-balancer
author: erichrt
ms.service: load-balancer
ms.topic: troubleshooting
ms.date: 05/7/2020
ms.author: errobin
ms.openlocfilehash: c37c0e9b914854ff41053526740d3454c5c23f90
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2020
ms.locfileid: "91629001"
---
# <a name="troubleshooting-outbound-connections-failures"></a><a name="obconnecttsg"></a> Устранение сбоев исходящих подключений

Эта статья предназначена для решения распространенных проблем, связанных с исходящими подключениями из Azure Load Balancer. Большинство проблем с исходящими подключениями клиентов из-за нехватки портов SNAT и превышения времени ожидания подключения, приводящего к удаленным пакетам. В этой статье приводятся шаги по устранению каждой из этих проблем.

## <a name="managing-snat-pat-port-exhaustion"></a><a name="snatexhaust"></a>Управление нехваткой портов SNAT (PAT)
[Временные порты](load-balancer-outbound-connections.md) , используемые для [Pat](load-balancer-outbound-connections.md) , — это ограниченным ресурсом ресурс, как описано в разделе [Автономная виртуальная машина без общедоступного IP-адреса](load-balancer-outbound-connections.md) и [виртуальной машины с БАЛАНСИРОВКОЙ нагрузки без общедоступного IP-адреса](load-balancer-outbound-connections.md). Вы можете отслеживать использование временных портов и сравнивать его с текущим выделением, чтобы определить риск или подтвердить нехватку SNAT с помощью [этого](https://docs.microsoft.com/azure/load-balancer/load-balancer-standard-diagnostics#how-do-i-check-my-snat-port-usage-and-allocation) руководством.

При инициировании множества исходящих TCP- или UDP-подключений к одним и тем же конечным IP-адресу и порту подключения будут завершаться сбоем или же вы будете получать уведомления от службы поддержки о нехватке SNAT (выделенных [временных портов](load-balancer-outbound-connections.md#preallocatedports), используемых для [PAT](load-balancer-outbound-connections.md)). У вас есть несколько вариантов устранения этой проблемы. Ознакомьтесь с ними, чтобы выбрать доступный и подходящий вариант для своего сценария. Возможно, один или несколько вариантов помогут вам решить проблему.

Если вам непонятно поведение исходящих подключений, можно использовать статистику распределения IP-адресов (netstat) или записывать пакеты. Такую запись можно выполнять в гостевой ОС экземпляра или с помощью [Наблюдателя за сетями](../network-watcher/network-watcher-packet-capture-manage-portal.md). 

## <a name="manually-allocate-snat-ports-to-maximize-snat-ports-per-vm"></a><a name ="manualsnat"></a>Выделение портов SNAT вручную для максимального увеличения числа портов SNAT на одну виртуальную машину
Как указано в определении [предварительно выделенных портов](load-balancer-outbound-connections.md#preallocatedports), подсистема балансировки нагрузки автоматически выделяет порты на основе количества виртуальных машин в серверной части. По умолчанию это сделано консервативно, чтобы обеспечить масштабируемость. Если известно максимальное количество виртуальных машин, которые будут находиться в серверной части, можно вручную выделить порты SNAT в каждом исходящем правиле. Например, если известно, что максимальное количество виртуальных машин составляет 10, можно выделить 6400 портов SNAT на каждую виртуальную машину, а не 1024 по умолчанию. 

## <a name="modify-the-application-to-reuse-connections"></a><a name="connectionreuse"></a>Изменение приложения для повторного использования подключений 
Вы можете снизить потребность во временных портах, используемых для SNAT, повторно используя подключения в приложении. Повторное использование подключения особенно важно для таких протоколов, как HTTP/1.1, где повторное использование подключения является значением по умолчанию. В свою очередь, это может положительно повлиять на другие протоколы, использующие HTTP в качестве транспортировки (например, REST). 

Повторное использование всегда лучше, чем отдельные, атомарные TCP-подключения для каждого запроса. Оно позволяет повысить производительность и эффективность транзакций TCP.

## <a name="modify-the-application-to-use-connection-pooling"></a><a name="connection pooling"></a>Изменение приложения для использования пулов подключений
Можно использовать в приложении схему пула подключений, при которой внутренний механизм распределяет запросы по фиксированному набору подключений (каждое из которых повторно используется, если это возможно). Эта схема ограничивает количество используемых временных портов и делает среду более предсказуемой. Кроме того, она может повысить пропускную способность запросов, позволяя одновременно выполнять несколько операций, когда отдельное подключение блокируется, ожидая результата операции.  

Возможно, пулы подключений уже существуют в платформе, которая используется вами для разработки приложения или параметров конфигурации приложения. Их можно объединить с повторным использованием подключений. В этом случае для обработки нескольких запросов можно будет использовать фиксированное, прогнозируемое число портов для одних и тех же конечных IP-адреса и порта. Кроме того, вы можете воспользоваться преимуществами повышенной эффективности транзакций TCP, сокращающей задержки и использование ресурсов. Транзакции UDP также могут предоставлять преимущества, так как возможность изменить число потоков UDP позволяет избежать нехватки и управлять использованием портов SNAT.

## <a name="modify-the-application-to-use-less-aggressive-retry-logic"></a><a name="retry logic"></a>Изменение приложения для использования менее "жесткой" логики повторных попыток
При нехватке [предварительно выделенных временных портов](load-balancer-outbound-connections.md#preallocatedports) для [PAT](load-balancer-outbound-connections.md) или сбоях приложения строгая логика повтора или логика повтора методом подбора без логики отхода и времени сохранения не позволяет устранить проблему. Вы можете снизить потребность во временных портах, используя менее "жесткую" логику повторных попыток. 

Для временных портов установлено время ожидания простоя 4 минуты (не регулируется). Если повторные попытки слишком частые, нехватка будет наблюдаться в любом случае. Поэтому знать, как и как часто приложение повторяет транзакции, очень важно для проектирования.

## <a name="assign-a-public-ip-to-each-vm"></a><a name="assignilpip"></a>Назначение общедоступного IP-адреса каждой виртуальной машине
Назначение общедоступного IP-адреса изменяет свой сценарий на [Общедоступный IP-адрес для виртуальной машины](load-balancer-outbound-connections.md). Все временные порты общедоступного IP-адреса, используемые для каждой виртуальной машины, доступны для каждой из них. (В отличие от сценариев, в которых временные порты общедоступного IP-адреса используются совместно со всеми виртуальными машинами, связанными с соответствующим внутренним пулом.) Существуют компромиссы, которые следует учитывать, например дополнительную стоимость общедоступных IP-адресов, и возможное влияние фильтрации большого количества отдельных IP-адресов.

>[!NOTE] 
>Такая возможность недоступна для рабочих ролей.

## <a name="use-multiple-frontends"></a><a name="multifesnat"></a>Использование нескольких внешних интерфейсов
При использовании общедоступного Load Balancer категории "Стандартный" назначаются [несколько внешних IP-адресов для исходящих подключений](load-balancer-outbound-connections.md) и [количество доступных портов SNAT увеличивается](load-balancer-outbound-connections.md#preallocatedports).  Создайте внешнюю IP-конфигурацию, правило и внутренний пул для активации программирования SNAT для общедоступного IP-адреса интерфейса.  Правило не должно функционировать, и проверка работоспособности не должна завершиться успешно.  Если используются несколько внешних интерфейсов для входящих подключений (а не только для исходящих), нужно использовать специальные проверки работоспособности, чтобы обеспечить надежность.

>[!NOTE]
>В большинстве случаев нехватка портов SNAT является признаком плохой конфигурации.  Прежде чем использовать больше интерфейсов для добавления портов SNAT, убедитесь, что вы понимаете, почему не хватает портов.  Возможно, где-то прячется проблема, которая может привести к сбою позже.

## <a name="scale-out"></a><a name="scaleout"></a>Горизонтальное увеличение масштаба
[Предварительно выделенные порты](load-balancer-outbound-connections.md#preallocatedports) назначаются в соответствии с размером внутреннего пула и группируются по уровням. Это позволяет минимизировать нарушения работы, когда нужно перераспределить несколько портов для размещения следующего более крупного уровня пула серверной части.  Вы можете увеличить использование порта SNAT для данного интерфейса путем масштабирования внутреннего пула до максимального размера для данного уровня.  Учитывая, что для эффективного масштабирования приложения требуется выделение портов по умолчанию без риска нехватки SNAT.

Например, для двух виртуальных машин в пуле серверной части выделяется 1024 порта SNAT на каждую конфигурацию IP, что позволяет использовать в общей сложности 2048 портов SNAT в этом развертывании.  Если вы увеличите это развертывание до 50 виртуальных машин, сохраняя прежнее число выделяемых портов на виртуальную машину, развертывание сможет использовать в общей сложности 51 200 портов SNAT (50 x 1024).  Если вы хотите масштабировать развертывание, проверьте количество предварительно [выделенных портов](load-balancer-outbound-connections.md#preallocatedports) на каждый уровень, чтобы определить масштабное масштабирование для соответствующего уровня.  Если в описанном выше развертывании вы горизонтально увеличите масштаб числа экземпляров до 51 (вместо 50), вы перейдете на следующий уровень, и число портов SNAT окажется меньшим как для каждой виртуальной машины, так и суммарно.

При горизонтальном увеличении масштаба до следующего более крупного уровня размеров пула серверной части существует вероятность того, что некоторые из ваших исходящих подключений будут отключены, если выделенные порты не будут перераспределены.  Если используются только некоторые из портов SNAT, масштабирование в следующем большем размере северного пула не имеет значения.  Половина существующих портов будет перераспределяться каждый раз, когда вы переходите на следующий уровень северного пула.  Если же перераспределение нежелательно, выбирайте параметры развертывания в соответствии с текущим уровнем.  В качестве альтернативного решения можно реализовать в приложении логику отслеживания и повторных попыток.  Проверка активности TCP поможет определять, когда порты SNAT теряют работоспособность из-за перераспределения.

## <a name="use-keepalives-to-reset-the-outbound-idle-timeout"></a><a name="idletimeout"></a>Использование проверки активности для сброса времени ожидания простоя исходящих подключений
Для исходящих подключений задано время ожидания простоя 4 минуты. Это время ожидания регулируется с помощью [правил для исходящего трафика](../load-balancer/load-balancer-outbound-rules-overview.md#idletimeout). Также можно использовать проверку активности транспортного уровня (например, TCP-подключений) или уровня приложения, чтобы обновлять простаивающие потоки и сбрасывать время ожидания, если необходимо.  

Если вы решите использовать проверку активности TCP, ее достаточно организовать на одной стороне соединения. Например, проверка активности со стороны сервера будет успешно сбрасывать таймер ожидания для последовательности, избавляя от необходимости поддерживать активность TCP-соединения с обеих сторон.  Этот же подход применим и для уровня приложений, в том числе в системах баз данных "клиент — сервер".  Проверьте на стороне сервера, какие параметры существуют для конкретного приложения проверку активности.

## <a name="next-steps"></a>Next Steps
Мы всегда хотим улучшить работу наших клиентов. Если у вас возникли проблемы с исходящими подключениями, которые не перечислены или не разрешены в этой статье, отправьте отзыв через GitHub в конце этой страницы, и мы будем обращаться к вашим отзывам как можно скорее.
