---
title: Создание DHCP и управление им
description: В этой статье объясняется, как управлять DHCP в решении Azure VMware.
ms.topic: conceptual
ms.date: 05/04/2020
ms.openlocfilehash: 2c059918f57b7f01058a031f1bf281b243855661
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2020
ms.locfileid: "91332837"
---
# <a name="how-to-create-and-manage-dhcp-in-azure-vmware-solution"></a>Создание DHCP и управление им в решении Azure VMWare

НСКС-T предоставляет возможность настройки DHCP для частного облака. Если вы планируете использовать НСКС-T для размещения DHCP-сервера, см. раздел [Создание DHCP-сервера](#create-dhcp-server). В противном случае, если в сети есть внешний DHCP-сервер стороннего производителя и вы хотите ретранслировать запросы к этому DHCP-серверу, см. раздел [Создание службы DHCP-ретрансляции](#create-dhcp-relay-service).

## <a name="create-dhcp-server"></a>Создание DHCP-сервера

Выполните следующие действия, чтобы настроить DHCP-сервер на НСКС — T.

В НСКС Manager перейдите на вкладку **Сетевые подключения** и в разделе **Управление IP-адресами**выберите **DHCP** . Нажмите кнопку **Добавить сервер** . Затем укажите имя сервера и IP-адрес сервера. После этого нажмите кнопку **сохранить**.

:::image type="content" source="./media/manage-dhcp/dhcp-server-settings.png" alt-text="добавить DHCP-сервер" border="true":::

### <a name="connect-dhcp-server-to-the-tier-1-gateway"></a>Подключите DHCP-сервер к шлюзу уровня 1.

1. Выберите **шлюзы уровня 1**, шлюз, а затем щелкните **изменить** .

   :::image type="content" source="./media/manage-dhcp/edit-tier-1-gateway.png" alt-text="добавить DHCP-сервер" border="true":::

1. Добавьте подсеть, выбрав параметр " **нет набора выделений IP-адресов** ".

   :::image type="content" source="./media/manage-dhcp/add-subnet.png" alt-text="добавить DHCP-сервер" border="true":::

1. На следующем экране в раскрывающемся списке **Тип** выберите **локальный сервер DHCP** . Для **DHCP-сервера**выберите **по умолчанию DHCP** и нажмите кнопку **сохранить**.

   :::image type="content" source="./media/manage-dhcp/set-ip-address-management.png" alt-text="добавить DHCP-сервер" border="true":::

1. В окне **шлюз уровня 1** выберите **сохранить**. На следующем экране отобразятся **сохраненные изменения**, а затем нажмите кнопку **Закрыть редактирование** для завершения.

### <a name="add-a-network-segment"></a>Добавление сегмента сети

После создания DHCP-сервера необходимо добавить к нему сегменты сети.

1. В НСКС-T перейдите на вкладку " **сеть** " и выберите " **сегменты** " в разделе " **Подключение**". Выберите **Добавить сегмент**. Присвойте сегменту имя и подключение к шлюзу уровня 1. Затем выберите **задать подсети** , чтобы настроить новую подсеть. 

   :::image type="content" source="./media/manage-dhcp/add-segment.png" alt-text="добавить DHCP-сервер" border="true":::

1. В окне **задать подсети** выберите **добавить подсеть**. Введите IP-адрес шлюза и диапазон DHCP и нажмите кнопку **Добавить** , а затем — **Применить** .

   :::image type="content" source="./media/manage-dhcp/add-subnet-segment.png" alt-text="добавить DHCP-сервер" border="true":::

1. По завершении нажмите кнопку **сохранить** , чтобы завершить добавление сегмента сети.

   :::image type="content" source="./media/manage-dhcp/segments-complete.png" alt-text="добавить DHCP-сервер" border="true":::

## <a name="create-dhcp-relay-service"></a>Создание службы ретранслятора DHCP

1. В окне NXT-T выберите вкладку **сеть** и в разделе **Управление IP-адресами**выберите **DHCP**. Выберите **Добавить сервер**. Выберите ретранслятор DHCP для **типа сервера** и введите имя сервера и IP-адрес сервера ретрансляции. Выберите **Сохранить**, чтобы сохранить изменения.

   :::image type="content" source="./media/manage-dhcp/create-dhcp-relay.png" alt-text="добавить DHCP-сервер" border="true":::

1. Выберите **шлюзы уровня 1** в разделе **Подключение**. Щелкните вертикальное многоточие на шлюзе уровня 1 и выберите **изменить**.

   :::image type="content" source="./media/manage-dhcp/edit-tier-1-gateway-relay.png" alt-text="добавить DHCP-сервер" border="true":::

1. Выберите **не устанавливать выделение IP-** адресов, чтобы определить выделение IP-адреса.

   :::image type="content" source="./media/manage-dhcp/edit-ip-address-allocation.png" alt-text="добавить DHCP-сервер" border="true":::

1. В диалоговом окне в поле **тип**выберите **DHCP-сервер ретранслятора**. В раскрывающемся списке **ретранслятор DHCP** выберите сервер DHCP-ретрансляции. По завершении нажмите кнопку **сохранить** .

   :::image type="content" source="./media/manage-dhcp/set-ip-address-management-relay.png" alt-text="добавить DHCP-сервер" border="true":::

## <a name="specify-a-dhcp-range-ip-on-segment"></a>Укажите IP-адрес диапазона DHCP в сегменте

> [!NOTE]
> Эта конфигурация необходима для реализации функций ретранслятора DHCP в сегменте DHCP-клиента. 

1. В разделе **Подключение**выберите **сегменты**. Щелкните вертикальные многоточие и выберите **изменить**. Вместо этого, если вы хотите добавить новый сегмент, можно выбрать **Добавить сегмент** , чтобы создать новый сегмент.

   :::image type="content" source="./media/manage-dhcp/edit-segments.png" alt-text="добавить DHCP-сервер" border="true":::

1. Добавьте сведения о сегменте. Выберите значение в разделе **подсети** или **Задайте** подсети, чтобы добавить или изменить подсеть.

   :::image type="content" source="./media/manage-dhcp/network-segments.png" alt-text="добавить DHCP-сервер" border="true":::

1. Щелкните вертикальные многоточие и выберите **изменить**. Если необходимо создать новую подсеть, выберите **добавить подсеть** , чтобы создать шлюз и настроить диапазон DHCP. Укажите диапазон пула IP-адресов и нажмите кнопку **Применить**, а затем выберите **сохранить** .

   :::image type="content" source="./media/manage-dhcp/edit-subnet.png" alt-text="добавить DHCP-сервер" border="true":::

1. Теперь сегменту назначается пул DHCP-серверов.

   :::image type="content" source="./media/manage-dhcp/assigned-to-segment.png" alt-text="добавить DHCP-сервер" border="true":::
