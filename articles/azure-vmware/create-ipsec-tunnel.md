---
title: Создание туннеля IPSec в решении VMware для Azure
description: Узнайте, как создать виртуальный концентратор глобальной сети для создания туннеля IPSec в решениях VMware для Azure.
ms.topic: how-to
ms.date: 10/02/2020
ms.openlocfilehash: 74cc31abf432954008cbb20bf64825d199732dab
ms.sourcegitcommit: a2d8acc1b0bf4fba90bfed9241b299dc35753ee6
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/12/2020
ms.locfileid: "91951134"
---
# <a name="create-an-ipsec-tunnel-into-azure-vmware-solution"></a>Создание туннеля IPSec в решении VMware для Azure

В этой статье мы проверим, как установить туннель VPN типа "сеть-сеть" (IPsec IKEv1 и IKEv2), завершающий работу в Microsoft Azure виртуальном концентраторе глобальной сети. Мы создадим виртуальный концентратор глобальной сети Azure и VPN-шлюз с подключенным к нему общедоступным IP-адресом. Затем мы создадим шлюз Azure ExpressRoute и устанавливаем конечную точку решения VMware для Azure. Также будут рассмотрены сведения о включении локальной установки VPN на основе политик. 

## <a name="topology"></a>Топология

![Архитектура туннеля VPN типа "сеть — сеть".](media/create-ipsec-tunnel/vpn-s2s-tunnel-architecture.png)

Виртуальный концентратор Azure содержит шлюз ExpressRoute решения Azure VMware и шлюз VPN типа "сеть — сеть". Он подключает локальное VPN-устройство к конечной точке решения VMware Azure.

## <a name="before-you-begin"></a>Перед началом

Чтобы создать VPN-туннель типа "сеть — сеть", необходимо создать общедоступный IP-адрес, заканчивающийся на локальном VPN-устройстве.

## <a name="create-a-virtual-wan-hub"></a>Создание концентратора Виртуальной глобальной сети

1. В портал Azure выполните поиск по **виртуальным глобальным сетям**. Щелкните **+Добавить**. Откроется страница Создание глобальной сети.  

2. Введите необходимые поля на странице **Создание глобальной сети** , а затем выберите **проверить и создать**.
   
   | Поле | Значение |
   | --- | --- |
   | **Подписка** | Значение предварительно заполнено подпиской, принадлежащей группе ресурсов. |
   | **Группа ресурсов** | Виртуальная глобальная сеть является глобальным ресурсом и не ограничивается конкретным регионом.  |
   | **Расположение группы ресурсов** | Чтобы создать виртуальный концентратор глобальной сети, необходимо задать расположение для группы ресурсов.  |
   | **имя**; |   |
   | **Тип** | Выберите **стандартный**, что позволит использовать не только трафик VPN-шлюза.  |


    :::image type="content" source="media/create-ipsec-tunnel/create-wan.png" alt-text="Создайте глобальную сеть.":::

3. В портал Azure выберите виртуальную глобальную сеть, созданную на предыдущем шаге, щелкните **создать виртуальный концентратор**, введите необходимые поля, а затем выберите **Далее: сайт-сайт**. 

   | Поле | Значение |
   | --- | --- |
   | **Регион** | Выбор региона требуется с точки зрения управления.  |
   | **Имя** |    |
   | **Пространство частных адресов концентратора** | Введите подсеть с помощью `/24` (минимум).  |

    :::image type="content" source="media/create-ipsec-tunnel/create-virtual-hub.png" alt-text="Создайте глобальную сеть." определите шлюз "сеть — сеть", задав общую пропускную способность из раскрывающегося списка **единицы масштабирования шлюза** . 

   >[!TIP]
   >Одна единица масштабирования = 500 Мбит/с. Единицы масштабирования являются парами для обеспечения избыточности, каждая из которых поддерживает 500 Мбит/с.
  
5. На вкладке **expressroute** создайте шлюз ExpressRoute. 

   >[!TIP]
   >Значение единицы масштабирования — 2 Гбит/с. 

    Создание каждого концентратора занимает около 30 минут. 

## <a name="create-a-vpn-site"></a>Создание VPN-сайта 

1. В списке **недавние ресурсы** в портал Azure выберите виртуальную глобальную сеть, созданную в предыдущем разделе.

2. В **обзоре** виртуального концентратора выберите **Подключение**  >  **VPN (сеть-сеть)**, а затем выберите **создать новый VPN-сайт**.


    :::image type="content" source="media/create-ipsec-tunnel/create-vpn-site-basics.png" alt-text="Создайте глобальную сеть.":::  
 
3. На вкладке **Основные сведения** введите необходимые поля и нажмите кнопку **Далее: ссылки**. 

   | Поле | Значение |
   | --- | --- |
   | **Регион** | Тот же регион, который вы указали в предыдущем разделе.  |
   | **Имя** |  |
   | **Поставщик устройства** |  |
   | **Протокол BGP** | Установите для **включения** , чтобы обеспечить как решение Azure VMware, так и локальные серверы, объявляющие их маршруты через туннель. Если этот параметр отключен, подсети, которые необходимо объявить, должны поддерживаться вручную. Если подсети отсутствуют, ХККС не сможет сформировать сетку службы. Дополнительные сведения см. в статье  [об BGP с VPN-шлюзом Azure](../vpn-gateway/vpn-gateway-bgp-overview.md). |
   | **Частное адресное пространство**  | Введите локальный блок CIDR.  Он используется для маршрутизации всего трафика, привязанного к локальной сети по туннелю.  Блок CIDR требуется только в том случае, если не включен протокол BGP. |
   | **Подключение к** |   |

4. На вкладке **ссылки** заполните обязательные поля и выберите **проверить и создать**. Указание имен ссылок и поставщиков позволяет отличать любое количество шлюзов, которые в конечном итоге могут быть созданы как часть концентратора. Идентификаторы BGP и автономной системы (ASN) должны быть уникальными в пределах организации.
 
## <a name="optional-defining-a-vpn-site-for-policy-based-vpn-site-to-site-tunnels"></a>Используемых Определение VPN-сайта для туннелей VPN типа "сеть-сеть" на основе политик

Этот раздел относится только к VPN на основе политик. Настройки VPN на основе политик (или статические) в большинстве случаев определяются локальными возможностями VPN-устройств. Для них требуется указать локальные сети и решения Azure VMware. Для решения Azure VMware с виртуальным концентратором глобальной сети Azure нельзя выбрать *какую-либо* сеть. Вместо этого необходимо указать все соответствующие диапазоны виртуальных глобальных сетей и виртуальные глобальные сети решения Azure VMware. Эти диапазоны концентраторов используются для указания домена шифрования локальной конечной точки VPN-туннеля политики. На стороне решения VMware для Azure требуется включить индикатор селектора трафика на основе политики. 

1. В портал Azure перейдите на свой виртуальный сайт концентратора глобальной сети. в разделе **Подключение**выберите **VPN (сайт-сайт)**.

2. Выберите имя VPN-сайта, а затем многоточие (...) в правом углу; затем выберите **изменить раздел VPN в этом концентраторе**.
 
    :::image type="content" source="media/create-ipsec-tunnel/edit-vpn-section-to-this-hub.png" alt-text="Создайте глобальную сеть." lightbox="media/create-ipsec-tunnel/edit-vpn-section-to-this-hub.png":::

3. Измените подключение между сайтом VPN и концентратором, а затем нажмите кнопку **сохранить**.
   - Internet Protocol Security (IPSec), выберите **Custom (настраиваемая**).
   - Используйте селектор трафика на основе политики, выберите **включить** .
   - Укажите сведения для **IKE-этапа 1** и **IKE Phase 2 (IPSec)**. 
 
    :::image type="content" source="media/create-ipsec-tunnel/edit-vpn-connection.png" alt-text="Создайте глобальную сеть."::: 
 
    Селекторы трафика или подсети, которые являются частью домена шифрования на основе политик, должны быть:
    
    - Виртуальный концентратор глобальной сети/24
    - Частное облако решения Azure VMware/22
    - Подключенная виртуальная сеть Azure (если она есть)

## <a name="connect-your-vpn-site-to-the-hub"></a>Подключение VPN-сайта к концентратору

1. Установите флажок рядом с именем VPN-сайта (см. предыдущий снимок экрана **VPN-сайта для сайта** ), а затем выберите **подключить VPN-сайты**. В поле **общий ключ** введите ключ, определенный ранее для локальной конечной точки. Если у вас нет ранее определенного ключа, это поле можно оставить пустым, и ключ будет создан автоматически. 
 
    Включайте только **маршрут по умолчанию** , если вы развертываете брандмауэр в центре и он является следующим прыжком для подключений через этот туннель.

    Нажмите кнопку **Подключиться**. На экране состояния подключения отобразится состояние создания туннеля.

2. Перейдите к обзору виртуальной глобальной сети. Откройте страницу VPN-сайта и скачайте файл конфигурации VPN, чтобы применить его к локальной конечной точке.  

3. Теперь мы отправим решение Azure VMware в виртуальном концентраторе глобальной сети. (Для этого шага требуется сначала создать частное облако.)

    Перейдите к разделу **подключения** в частном облаке решения Azure VMware. На вкладке **ExpressRoute** выберите **+ запрос ключа авторизации**. Присвойте ему имя и выберите **создать**. (Создание ключа может занять около 30 секунд.) Скопируйте идентификатор ExpressRoute и ключ авторизации. 

    :::image type="content" source="media/create-ipsec-tunnel/express-route-connectivity.png" alt-text="Создайте глобальную сеть.":::

    > [!NOTE]
    > Ключ авторизации исчезнет через некоторое время, поэтому скопируйте его сразу после того, как он появится.

4. Теперь мы свяжем решение Azure VMware и VPN-шлюз вместе в виртуальном концентраторе глобальной сети. В портал Azure Откройте виртуальную глобальную сеть, созданную ранее. Выберите созданный виртуальный концентратор глобальной сети, а затем в левой области выберите **ExpressRoute** . Выберите **+ активировать ключ авторизации**.

    :::image type="content" source="media/create-ipsec-tunnel/redeem-authorization-key.png" alt-text="Создайте глобальную сеть.":::

    Вставьте ключ авторизации в поле ключа авторизации и идентификатор ExpressRoute в поле **URI одноранговой цепи** . Обязательно установите флажок **автоматически связывать этот канал ExpressRoute с концентратором.** Нажмите кнопку **Добавить** , чтобы установить связь. 

5. Чтобы проверить подключение, [Создайте сегмент НСКС-T](./tutorial-nsx-t-network-segment.md) и ПОДготавливает виртуальную машину в сети. Выполните тестирование, обратившись к локальным конечным точкам и решениям Azure VMware.