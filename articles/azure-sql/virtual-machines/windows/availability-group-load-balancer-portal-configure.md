---
title: Настройка прослушивателей группы доступности и Load Balancer (портал Azure)
description: Пошаговые инструкции по созданию прослушивателя для группы доступности AlwaysOn для SQL Server в службе виртуальных машин Azure.
services: virtual-machines
documentationcenter: na
author: MashaMSFT
editor: monicar
ms.assetid: d1f291e9-9af2-41ba-9d29-9541e3adcfcf
ms.service: virtual-machines-sql
ms.topic: how-to
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 02/16/2017
ms.author: mathoma
ms.custom: seo-lt-2019
ms.openlocfilehash: b3f2e8b56af41d1729b9786adda3abdcc4eb0b02
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2020
ms.locfileid: "91325034"
---
# <a name="configure-a-load-balancer-for-a-sql-server-always-on-availability-group-in-azure-virtual-machines"></a>Настройка подсистемы балансировки нагрузки для SQL Server Always On группы доступности на виртуальных машинах Azure

[!INCLUDE[appliesto-sqlvm](../../includes/appliesto-sqlvm.md)]


В этой статье объясняется, как создать подсистему балансировки нагрузки для SQL Server Always On группы доступности на виртуальных машинах Azure, работающих с Azure Resource Manager. Группа доступности требует наличия балансировщика нагрузки, если экземпляры SQL Server находятся на виртуальных машинах Azure. Балансировщик нагрузки хранит IP-адрес для прослушивателя группы доступности. Если группа доступности распространяется на несколько регионов, для каждого из них нужен отдельный балансировщик.

Для выполнения этой задачи необходимо, чтобы группа доступности SQL Server Always On была развернута в виртуальных машинах Azure, работающих с диспетчер ресурсов. Обе виртуальные машины SQL Server должны принадлежать к одной и той же группе доступности. Вы можете использовать [шаблон Майкрософт](availability-group-azure-marketplace-template-configure.md), чтобы автоматически создать группу доступности в Resource Manager. Этот шаблон также автоматически создает внутреннюю подсистему балансировки нагрузки. 

При необходимости [группу доступности можно настроить вручную](availability-group-manually-configure-tutorial.md).

Здесь предполагается, что группы доступности уже настроены.  

Просмотреть связанные статьи:

* [Введение в группы доступности AlwaysOn SQL Server на виртуальных машинах Azure](availability-group-manually-configure-tutorial.md)   
* [Настройка подключения между виртуальными сетями с помощью Azure Resource Manager и PowerShell](../../../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md)

Последовательно выполнив все действия этой статьи, вы создадите и настроите подсистему балансировки нагрузки на портале Azure. По завершении процесса вы настроите в кластере IP-адрес подсистемы балансировки нагрузки для прослушивателя группы доступности.

## <a name="create--configure-load-balancer"></a>Создание & Настройка балансировщика нагрузки 

В этой части задачи выполните следующие действия.

1. Создадим подсистему балансировки нагрузки и настроим IP-адрес на портале Azure.
2. Настроим серверный пул.
3. Создадим пробу. 
4. Задайте правила балансировки нагрузки.

> [!NOTE]
> Если экземпляры SQL Server расположены в нескольких группах ресурсов и регионах, необходимо выполнить все эти действия дважды — по одному разу в каждой группе ресурсов.
> 

### <a name="step-1-create-the-load-balancer-and-configure-the-ip-address"></a>Шаг 1. Создадим балансировщик нагрузки и настроим IP-адрес.

Сначала создайте подсистему балансировки нагрузки. 

1. На портале Azure откройте группу ресурсов, в которой содержатся виртуальные машины SQL Server. 

2. В группе ресурсов щелкните **Добавить**.

3. Найдите **балансировщик нагрузки**. В результатах поиска выберите **Load Balancer** (Опубликовано **корпорацией Майкрософт**).

4. В колонке **Load Balancer** щелкните **Создать**.

5. В диалоговом окне **Создание подсистемы балансировки нагрузки** настройте подсистему балансировки нагрузки, задав следующие параметры:

   | Параметр | Значение |
   | --- | --- |
   | **имя**; |Текст, представляющий имя балансировщика нагрузки. Например, **sqlLB**. |
   | **Тип** |**Внутренние**. В большинстве реализаций используется внутренняя подсистема балансировки нагрузки, которая позволяет приложениям в одной и той же виртуальной сети подключаться к группе доступности.  </br> **Внешние**. Позволяет устанавливать общедоступное интернет-подключение к группе доступности. |
   | **SKU** |**Базовый**: параметр по умолчанию. Допустимо только в том случае, если SQL Server экземпляры находятся в одной группе доступности. </br> **Стандартный**: предпочтительный. Действителен, если SQL Server экземпляры находятся в одной группе доступности. Требуется, если экземпляры SQL Server находятся в разных зонах доступности. |
   | **Виртуальная сеть** |Выберите виртуальную сеть, в которой расположены экземпляры SQL Server. |
   | **Подсеть** |Выберите подсеть, в которой расположены экземпляры SQL Server. |
   | **Назначение IP-адресов** |**Статическое** |
   | **Частный IP-адрес** |Укажите свободный IP-адрес из нужной подсети. Используйте этот IP-адрес при создании прослушивателя в кластере. Далее вам понадобится использовать этот адрес для переменной `$ILBIP` в скрипте PowerShell. |
   | **Подписка** |Это поле может появиться, если у вас несколько подписок. Выберите подписку, которую требуется связать с этим ресурсом. Обычно это подписка, с которой связаны все ресурсы группы доступности. |
   | **Группа ресурсов** |Выберите группу ресурсов, в которой расположены экземпляры SQL Server. |
   | **Расположение** |Выберите расположение Azure, в котором расположены экземпляры SQL Server. |

6. Нажмите кнопку **создания**. 

Azure создаст подсистему балансировки нагрузки. Балансировщик нагрузки относится к конкретной сети, подсети, группе ресурсов и расположению. После создания подсистемы балансировки нагрузки проверьте ее параметры в Azure. 

### <a name="step-2-configure-the-back-end-pool"></a>Шаг 2. Настройка серверного пула

В Azure серверный пул адресов называется просто *серверным пулом*. В нашем случае это адреса двух экземпляров SQL Server в группе доступности. 

1. В группе ресурсов выберите созданную подсистему балансировки нагрузки. 

2. В меню **Параметры**выберите **серверные пулы**.

3. В **серверных пулах**выберите **Добавить** , чтобы создать пул внутренних адресов. 

4. В колонке **Добавление серверного пула** в поле **Имя** введите имя серверного пула.

5. В разделе **виртуальные машины**выберите **добавить виртуальную машину**. 

6. В разделе **выберите виртуальные машины**выберите **пункт выбрать группу доступности**и укажите группу доступности, к которой принадлежат SQL Server виртуальные машины.

7. Выбрав группу доступности, щелкните **выбрать виртуальные машины**, выберите две виртуальные машины, на которых размещены экземпляры SQL Server, в группе доступности, а затем нажмите кнопку **выбрать**. 

8. Нажмите кнопку **ОК** , чтобы закрыть колонки для **выбора виртуальных машин**и **Добавить внутренний пул**. 

После этого Azure обновит параметры для серверного пула адресов. Теперь в вашей группе доступности есть пул из двух экземпляров SQL Server.

### <a name="step-3-create-a-probe"></a>Шаг 3. Создание пробы

Проба определяет, как Azure будет проверять, какому из экземпляров SQL Server в данный момент принадлежит прослушиватель группы доступности. Azure проверяет службу по IP-адресу и порту, которые вы указываете при создании пробы.

1. В колонке **Параметры** подсистемы балансировки нагрузки выберите **зонды работоспособности**. 

2. В колонке **зонды работоспособности** выберите **Добавить**.

3. Настройте пробу в колонке **Добавление пробы** . Для настройки пробы используйте следующие значения:

   | Параметр | Значение |
   | --- | --- |
   | **имя**; |Текст, представляющий имя пробы. Например, **SQLAlwaysOnEndPointProbe**. |
   | **протокол**; |**TCP** |
   | **порт**. |Вы можете использовать любой доступный порт. Например, *59999*. |
   | **Интервал** |*5* |
   | **Пороговое значение сбоя** |*2* |

4.  Щелкните **ОК**. 

> [!NOTE]
> Указанный порт должен быть открыт в брандмауэре обоих экземпляров SQL Server. Для обоих экземпляров требуется правило для входящего трафика используемого TCP-порта. Дополнительные сведения см. в статье [Добавление и изменение правила брандмауэра](https://technet.microsoft.com/library/cc753558.aspx). 
> 

Azure создаст пробу и с ее помощью будет проверять, в каком экземпляре SQL Server есть прослушиватель для группы доступности.

### <a name="step-4-set-the-load-balancing-rules"></a>Шаг 4. Настройка правил балансировки нагрузки

Правила балансировки нагрузки определяют способ направления трафика подсистемой балансировки нагрузки в экземплярах SQL Server. Для этой подсистемы балансировки нагрузки включите прямой ответ от сервера, так как одновременно только один из двух экземпляров SQL Server может содержать ресурс прослушивателя группы доступности.

1. В колонке **Параметры** подсистемы балансировки нагрузки выберите **правила балансировки нагрузки**. 

2. В колонке **правила балансировки нагрузки** выберите **Добавить**.

3. В колонке **Добавление правил балансировки нагрузки** настройте правило балансировки нагрузки. Используйте следующие параметры: 

   | Параметр | Значение |
   | --- | --- |
   | **имя**; |Текстовое имя, представляющее правила балансировки нагрузки. Например, **SQLAlwaysOnEndPointListener**. |
   | **протокол**; |**TCP** |
   | **порт**. |*1433* |
   | **Серверный порт** |*1433*. Это значение игнорируется, поскольку правило использует **плавающий IP-адрес (прямой ответ от сервера)** . |
   | **Проба** |Используйте имя пробы, созданной для этого балансировщика нагрузки. |
   | **Сохранение сеанса** |**None** |
   | **Время ожидания простоя (в минутах)** |*4* |
   | **Плавающий IP-адрес (прямой ответ от сервера)** |**Enabled** |

   > [!NOTE]
   > Возможно, потребуется прокрутить колонку вниз, чтобы просмотреть все параметры.
   > 

4. Щелкните **ОК**. 

5. Azure настроит правило балансировки нагрузки. Теперь подсистема балансировки нагрузки настроена для маршрутизации трафика в экземпляр SQL Server, на котором размещается прослушиватель для группы доступности. 

На этом этапе группа ресурсов содержит подсистему балансировки нагрузки, через которую происходит подключение к обеим виртуальным машинам SQL Server. Подсистема балансировки нагрузки также содержит IP-адрес прослушивателя группы доступности AlwaysOn для SQL Server. Таким образом, любая виртуальная машина может отвечать на запросы для групп доступности.

> [!NOTE]
> Если экземпляры SQL Server находятся в разных регионах, повторите эти действия еще раз в другом регионе. Балансировщик нагрузки необходим для каждого региона. 
> 

## <a name="configure-the-cluster-to-use-the-load-balancer-ip-address"></a>Настройка использования IP-адреса балансировщика нагрузки для кластера

Теперь необходимо настроить прослушиватель в кластере и подключить его. Выполните следующие шаги: 

1. Создайте прослушиватель группы доступности в отказоустойчивом кластере. 

2. Перевод прослушивателя в режим «в сети».

### <a name="step-5-create-the-availability-group-listener-on-the-failover-cluster"></a>Шаг 5. Создайте прослушиватель группы доступности в отказоустойчивом кластере.

На этом шаге вручную создается прослушиватель группы доступности в диспетчере отказоустойчивости кластеров и службе SQL Server Management Studio.

[!INCLUDE [ag-listener-configure](../../../../includes/virtual-machines-ag-listener-configure.md)]

### <a name="verify-the-configuration-of-the-listener"></a>Проверка конфигурации прослушивателя

Если ресурсы и зависимости кластера настроены правильно, вы увидите прослушиватель в SQL Server Management Studios. Чтобы задать порт прослушивателя, выполните следующие действия.

1. Запустите SQL Server Management Studio и подключитесь к основной реплике.

2. Перейдите в раздел **Высокий уровень доступности AlwaysOn** > **Группы доступности** > **Прослушиватели группы доступности**.  

    Теперь вы увидите имя прослушивателя, созданного в диспетчере отказоустойчивости кластеров. 

3. Щелкните правой кнопкой мыши имя прослушивателя и выберите пункт **Свойства**.

4. В поле **порт** укажите номер порта для прослушивателя группы доступности, используя $EndpointPort, который использовался ранее (по умолчанию 1433), а затем нажмите кнопку **ОК**.

Теперь у вас есть группа доступности на виртуальных машинах Azure, работающих в режиме Resource Manager. 

## <a name="test-the-connection-to-the-listener"></a>Проверка подключения к прослушивателю

Проверьте подключение, выполнив следующие действия.

1. Используйте протокол удаленного рабочего стола (RDP) для подключения к экземпляру SQL Server, который находится в той же виртуальной сети, но не является владельцем реплики. Этот сервер может быть другим экземпляром SQL Server в кластере.

2. Для проверки подключения используйте служебную программу **sqlcmd** . Например, в следующем сценарии **sqlcmd** подключается к основной реплике через прослушиватель с использованием аутентификации Windows.

    ```console
    sqlcmd -S <listenerName> -E
    ```

SQLCMD автоматически подключается к экземпляру SQL Server, на котором размещена основная реплика. 

## <a name="create-an-ip-address-for-an-additional-availability-group"></a>Создание IP-адреса для дополнительной группы доступности

Каждая группа доступности использует отдельный прослушиватель. У каждого прослушивателя имеется собственный IP-адрес. Используйте одну подсистему балансировки нагрузки для обслуживания IP-адреса для дополнительных прослушивателей. После создания группы доступности добавьте IP-адрес в подсистему балансировки нагрузки, а затем настройте прослушиватель.

Чтобы добавить IP-адрес в подсистему балансировки нагрузки с портал Azure, выполните следующие действия.

1. В портал Azure откройте группу ресурсов, содержащую подсистему балансировки нагрузки, а затем выберите подсистему балансировки нагрузки. 

2. В разделе **Параметры**выберите **интерфейсный пул IP-адресов**, а затем нажмите кнопку **добавить**. 

3. В разделе **Добавить внешний IP-адрес** укажите имя внешнего интерфейса. 

4. Убедитесь, что указаны те же **виртуальная сеть** и **подсеть**, что и для экземпляров SQL Server.

5. Задайте IP-адрес для прослушивателя. 
   
   >[!TIP]
   >Можно задать статический IP-адрес, который в настоящее время не используется в подсети. Кроме того, можно задать динамический IP-адрес и сохранить новый пул внешних IP-адресов. При этом портал Azure автоматически назначит пулу доступный IP-адрес. Затем можно будет повторно открыть пул внешних IP-адресов и изменить этот адрес на статический. 

6. Сохраните IP-адрес прослушивателя. 

7. Добавьте пробу работоспособности, используя следующие параметры:

   |Параметр |Значение
   |:-----|:----
   |**имя**; |Имя для идентификации пробы.
   |**протокол**; |TCP
   |**порт**. |Неиспользуемый TCP-порт, который должен быть доступен для всех виртуальных машин. Он не должен использоваться для других целей. Два прослушивателя не могут использовать один и тот же порт пробы. 
   |**Интервал** |Промежуток времени между повторными попытками выполнения пробы. Используйте значение по умолчанию (5).
   |**Пороговое значение сбоя** |Количество последовательных нарушений пороговых значений, после которого виртуальная машина считается неработоспособной.

8. Нажмите кнопку **ОК** , чтобы сохранить зонд. 

9. Создайте правило балансировки нагрузки. Выберите **правила балансировки нагрузки**, а затем нажмите кнопку **Добавить**.

10. Настройте новое правило балансировки нагрузки, используя следующие параметры.

    |Параметр |Значение
    |:-----|:----
    |**Имя** |Имя, идентифицирующее правило балансировки нагрузки. 
    |**Frontend IP address** (Интерфейсный IP-адрес) |Выберите IP-адрес, который вы создали. 
    |**протокол**; |TCP
    |**порт**. |Укажите порт, используемый экземплярами SQL Server. Экземпляр по умолчанию использует порт 1433, если только вы не изменили его. 
    |**Внутренний порт** |Укажите то же значение, что и для параметра **Порт**.
    |**Внутренний пул** |Пул, содержащий виртуальные машины с экземплярами SQL Server. 
    |**Зонд работоспособности** |Выберите пробу, которую вы создали.
    |**Сохранение сеанса** |None
    |**Время ожидания простоя (в минутах)** |Оставьте значение по умолчанию (4).
    |**Плавающий IP-адрес (прямой ответ от сервера)** | Активировано

### <a name="configure-the-availability-group-to-use-the-new-ip-address"></a>Настройка группы доступности для использования нового IP-адреса

Чтобы завершить настройку кластера, повторите действия, которые были выполнены при создании первой группы доступности. То есть настройте [кластер для использования нового IP-адреса](#configure-the-cluster-to-use-the-load-balancer-ip-address). 

После добавления IP-адреса для прослушивателя настройте дополнительную группу доступности, выполнив следующие действия. 

1. Убедитесь, что порт пробы для нового IP-адреса открыт на обеих виртуальных машинах SQL Server. 

2. [Создайте точку доступа клиента в диспетчере кластеров.](#addcap)

3. [Настройте ресурс IP-адреса для группы доступности](#congroup).

   >[!IMPORTANT]
   >При создании IP-адрес используйте IP-адрес, добавленный в подсистему балансировки нагрузки.  

4. [Сделайте так, чтобы ресурс группы доступности SQL Server зависел от точки доступа клиента.](#dependencyGroup)

5. [Сделайте так, чтобы ресурс точки доступа клиента зависел от IP-адреса](#listname).
 
6. [Настройте параметры кластера в PowerShell](#setparam).

Настроив группу доступности для использования нового IP-адреса, настройте подключение к прослушивателю. 

## <a name="add-load-balancing-rule-for-distributed-availability-group"></a>Добавление правила балансировки нагрузки для распределенной группы доступности

Если группа доступности включена в распределенную группу доступности, подсистеме балансировки нагрузки требуется дополнительное правило. В этом правиле указан порт, используемый прослушивателем распределенной группы доступности.

>[!IMPORTANT]
>Этот шаг применяется, только если группа доступности включена в [распределенную группу доступности](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/configure-distributed-availability-groups). 

1. На каждом сервере, включенном в распределенную группу доступности, создайте входящее правило на TCP-порту прослушивателя этой группы доступности. В документации во многих примерах используется порт 5022. 

1. В портал Azure выберите подсистему балансировки нагрузки и щелкните **правила балансировки нагрузки**, а затем выберите **+ Добавить**. 

1. Создайте правило балансировки нагрузки со следующими параметрами:

   |Параметр |Значение
   |:-----|:----
   |**имя**; |Имя для идентификации правила балансировки нагрузки для распределенной группы доступности. 
   |**Frontend IP address** (Интерфейсный IP-адрес) |Используйте тот же интерфейсный IP-адрес, что и для группы доступности.
   |**протокол**; |TCP
   |**порт**. |5022 — порт для [прослушивателя конечной точки распределенной группы доступности](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/configure-distributed-availability-groups).</br> Это может быть любой доступный порт.  
   |**Внутренний порт** | 5022 — укажите то же значение, что и для параметра **Порт**.
   |**Внутренний пул** |Пул, содержащий виртуальные машины с экземплярами SQL Server. 
   |**Зонд работоспособности** |Выберите пробу, которую вы создали.
   |**Сохранение сеанса** |None
   |**Время ожидания простоя (в минутах)** |Оставьте значение по умолчанию (4).
   |**Плавающий IP-адрес (прямой ответ от сервера)** | Активировано

Повторите эти шаги для подсистемы балансировки нагрузки в других группах доступности, участвующих в распределенных группах доступности.

Если у вас есть группа безопасности сети Azure для ограничения доступа, убедитесь, что правила разрешения включают:
- IP-адреса серверной SQL Server виртуальной машины
- Плавающие IP-адреса балансировщика нагрузки для прослушивателя доступности
- Основной IP-адрес кластера, если применимо.

## <a name="next-steps"></a>Дальнейшие действия

- [Настройка группы доступности AlwaysOn на виртуальных машинах Azure в разных регионах](availability-group-manually-configure-multiple-regions.md)
