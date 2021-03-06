---
title: включить файл
description: включить файл
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 08/27/2020
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 517acc5137d70c722d8defade1e218a3b2e78f86
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2020
ms.locfileid: "89052462"
---
Чтобы создать виртуальную сеть с помощью модели развертывания Resource Manager и портала Azure, следуйте приведенным ниже инструкциям. См. дополнительные сведения о [виртуальных сетях](../articles/virtual-network/virtual-networks-overview.md).

>[!NOTE]
>При использовании виртуальной сети в качестве части распределенной архитектуры вам необходимо обратиться к администратору локальной сети, чтобы выделить диапазон IP-адресов, который будет использоваться специально для этой виртуальной сети. Если повторяющийся диапазон адресов существует на обеих сторонах VPN-подключения, трафик будет маршрутизироваться неправильно. Кроме того, если вы хотите подключить эту виртуальную сеть к другой виртуальной сети, ее адресное пространство не может перекрываться с адресным пространством другой виртуальной сети. Спланируйте конфигурацию сети соответствующим образом.
>
>

1. Войдите на [портал Azure](https://portal.azure.com).
1. В поле **Поиск ресурсов, служб и документов (G+/)** введите *виртуальная сеть*.

   ![Страница поиска ресурса виртуальной сети](./media/vpn-gateway-basic-vnet-rm-portal-include/marketplace.png "Страница поиска ресурса виртуальной сети")
1. Выберите **Виртуальная сеть** в результатах **Marketplace**.

   ![Выбор виртуальной сети](./media/vpn-gateway-basic-vnet-rm-portal-include/marketplace-results.png "Страница поиска ресурса виртуальной сети")
1. На странице **Виртуальная сеть** выберите **Создать**.

   ![Страница виртуальной сети](./media/vpn-gateway-basic-vnet-rm-portal-include/vnet-click-create.png "Нажмите кнопку "Создать"")
1. После выбора пункта **Создать** откроется страница **Создание виртуальной сети**.
1. На вкладке **Основные сведения** введите **сведения о проекте** и **сведения об экземпляре** в параметрах виртуальной сети.

   ![Вкладка основных сведений](./media/vpn-gateway-basic-vnet-rm-portal-include/basics.png "Вкладка "Основные сведения"") Если введенные в поля значения допустимы, рядом с полем появится значок зеленой галочки. Некоторые значения автоматически заполнены. Вы можете заменить их собственными значениями:

   - **Подписка**. Убедитесь, что указана правильная подписка. Подписки можно менять с помощью раскрывающегося списка.
   - **Группа ресурсов.** Выберите существующую группу ресурсов или щелкните **Создать**, чтобы создать новую. Дополнительные сведения о группах ресурсов см. в статье [Общие сведения об Azure Resource Manager](../articles/azure-resource-manager/management/overview.md#resource-groups).
   - **Name** (Имя). Введите имя виртуальной сети.
   - **Регион**. Выберите расположение для виртуальной сети. Расположение определяет, где будут находиться ресурсы, развертываемые в этой виртуальной сети.

1. На вкладке **IP-адреса** настройте значения. Значения ниже приведены только для примера. Измените их в соответствии с нужными вам параметрами.

   ![Вкладка IP-адресов](./media/vpn-gateway-basic-vnet-rm-portal-include/addresses.png "Вкладка IP-адресов")  
   - **Диапазон IPv4-адресов.** По умолчанию адресное пространство создается автоматически. Вы можете щелкнуть адресное пространство, чтобы изменить значения на нужные вам. Вы также можете добавить дополнительные адресные пространства.
   - **Подсеть.** Если используется адресное пространство по умолчанию, подсеть по умолчанию создается автоматически. При изменении адресного пространства необходимо добавить подсеть. Выберите **+ Добавить подсеть**, чтобы открыть окно **Добавление подсети**. Настройте следующие параметры и выберите **Добавить**, чтобы добавить значения:
      - **Имя подсети**. В этом примере используется подсеть с именем FrontEnd.
      - **Диапазон адресов подсети.** Диапазон адресов для этой подсети.

1. На вкладке **Безопасность** пока оставьте значения по умолчанию:

   - **Защита от атак DDos.** Basic
   - **Брандмауэр.** Выключено
1. Выберите **Проверка и создание**, чтобы проверить настройки виртуальной сети.
1. После проверки настроек выберите **Создать**.
