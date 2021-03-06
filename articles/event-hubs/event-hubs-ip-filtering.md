---
title: Правила брандмауэра Центров событий Azure | Документация Майкрософт
description: Использование правил брандмауэра для разрешения подключений к Центрам событий Azure с определенных IP-адресов.
ms.topic: article
ms.date: 07/16/2020
ms.openlocfilehash: 596d506c0c4f6d79696b3019fd903e549149c656
ms.sourcegitcommit: 1b47921ae4298e7992c856b82cb8263470e9e6f9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/14/2020
ms.locfileid: "92056214"
---
# <a name="allow-access-to-azure-event-hubs-namespaces-from-specific-ip-addresses-or-ranges"></a>Разрешить доступ к пространствам имен концентраторов событий Azure с конкретных IP-адресов или диапазонов
По умолчанию пространства имен Центров событий доступны из Интернета при условии, что запрос поступает с действительной аутентификацией и авторизацией. С помощью IP-брандмауэра такой доступ можно дополнительно ограничить набором или диапазоном IPv4-адресов, введя их в нотации [CIDR](https://en.wikipedia.org/wiki/Classless_Inter-Domain_Routing).

Эта возможность полезна в сценариях, в которых Центры событий Azure должны быть доступны только из определенных хорошо известных сайтов. Правила брандмауэра позволяют настроить правила приема трафика, поступающего с определенных IPv4-адресов. Например, при использовании Центров событий с помощью [Azure ExpressRoute][express-route] можно создать **правило брандмауэра**, разрешающее трафик только с IP-адресов в локальной инфраструктуре. 

>[!IMPORTANT]
> Включение правил брандмауэра для пространства имен концентраторов событий блокирует входящие запросы по умолчанию, если только запросы не были запущены из разрешенных общедоступных IP-адресов. Запросы от других служб Azure, в том числе портала Azure, служб метрики и ведения журналов, блокируются. 
>
> Ниже приведены некоторые службы, которые не могут получить доступ к ресурсам концентраторов событий, если включена фильтрация IP-адресов. Обратите внимание, что список **не** является исчерпывающим.
>
> - Azure Stream Analytics
> - Маршруты Центра Интернета вещей Azure.
> - Device Explorer Интернета вещей Azure.
> - Сетка событий Azure
> - Azure Monitor (параметры диагностики)
>
> В качестве исключения можно разрешить доступ к ресурсам концентраторов событий из определенных доверенных служб, даже если включена фильтрация IP-адресов. Список доверенных служб см. в разделе [Доверенные службы Майкрософт](#trusted-microsoft-services).

## <a name="ip-firewall-rules"></a>Правила брандмауэра для IP-адресов
Правила брандмауэра для IP-адресов применяются на уровне пространства имен Центров событий. Таким образом, правила применяются ко всем подключениям клиентов с помощью любого поддерживаемого протокола. Любая попытка подключения с IP-адреса, который не соответствует разрешенному правилу IP-адресов в пространстве имен концентраторов событий, отклоняется как несанкционированный. В ответе не упоминается правило IP-адресов. Правила фильтрации IP-адресов применяются по порядку, поэтому первое правило, которое соответствует IP-адресу, определяет действие (принять или отклонить).

## <a name="use-azure-portal"></a>Использование портала Azure
В этом разделе показано, как использовать портал Azure для создания правил брандмауэра для пространства имен Центров событий. 

1. Перейдите к **пространству имен Центров событий** на [портале Azure](https://portal.azure.com).
4. Выберите **сеть** в разделе **Параметры** в меню слева. Вкладка " **сеть** " отображается только для **стандартных** или **выделенных** пространств имен. 
    > [!NOTE]
    > По умолчанию выбран параметр **Выбранные сети** , как показано на следующем рисунке. Если не указать правило брандмауэра IP-адресов или добавить виртуальную сеть на этой странице, доступ к пространству имен можно получить через **общедоступный Интернет** (с помощью ключа доступа).  

    :::image type="content" source="./media/event-hubs-firewall/selected-networks.png" alt-text="Вкладка &quot;сети&quot; — параметр &quot;выбранные сети&quot;" lightbox="./media/event-hubs-firewall/selected-networks.png":::    

    Если выбран параметр **все сети** , концентратор событий принимает подключения с любого IP-адреса (с помощью ключа доступа). Такое значение параметра равноценно правилу, которое принимает диапазон IP-адресов 0.0.0.0/0. 

    ![Снимок экрана, на котором отображается страница "брандмауэр и виртуальные сети" с выбранным параметром "все сети".](./media/event-hubs-firewall/firewall-all-networks-selected.png)
1. Чтобы ограничить доступ к определенным IP-адресам, убедитесь, что выбран параметр **Выбранные сети** . В разделе **Брандмауэр** выполните следующие шаги:
    1. Выберите вариант **Добавить IP-адрес клиента**, чтобы предоставить текущему IP-адресу клиента доступ к пространству имен. 
    2. В качестве **диапазона адресов** введите определенный IPv4-адрес или диапазон IPv4-адресов в нотации CIDR. 
3. Укажите, следует ли **разрешить доверенным службам Майкрософт обходить этот брандмауэр**. Дополнительные сведения см. в разделе [Доверенные службы Майкрософт](#trusted-microsoft-services) . 

      ![Брандмауэр — выбран вариант "Все сети"](./media/event-hubs-firewall/firewall-selected-networks-trusted-access-disabled.png)
3. На панели инструментов нажмите **Сохранить**, чтобы сохранить параметры. Подождите несколько минут, чтобы подтверждение появилось в уведомлениях портала.

    > [!NOTE]
    > Чтобы ограничить доступ к определенным виртуальным сетям, см. раздел [разрешение доступа из конкретных сетей](event-hubs-service-endpoints.md).

[!INCLUDE [event-hubs-trusted-services](../../includes/event-hubs-trusted-services.md)]


## <a name="use-resource-manager-template"></a>Использование шаблона Resource Manager

> [!IMPORTANT]
> Правила брандмауэра поддерживаются на **стандартном** и **выделенном** уровнях Центров событий. В "базовом" уровне не поддерживается.

Следующий шаблон Resource Manager позволяет добавить правило фильтрации IP-адресов в существующее пространство имен Центров событий.

Параметры шаблона:

- Зачение **ipMask** — это один IPv4-адрес или блок IP-адресов в нотации CIDR. Например, значение 70.37.104.0/24 в нотации CIDR представляет 256 IPv4-адресов в диапазоне от 70.37.104.0 до 70.37.104.255. Число 24 обозначает количество значимых битов префикса для адресов этого диапазона.

> [!NOTE]
> Хотя запрещающие правила отсутствуют, в шаблоне Azure Resource Manager для действия по умолчанию установлено значение **Разрешить**, которое не ограничивает подключения.
> При создании правил виртуальной сети или брандмауэров необходимо изменить значение параметра ***defaultAction***.
> 
> из
> ```json
> "defaultAction": "Allow"
> ```
> значение
> ```json
> "defaultAction": "Deny"
> ```
>

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
      "eventhubNamespaceName": {
        "type": "string",
        "metadata": {
          "description": "Name of the Event Hubs namespace"
        }
      },
      "location": {
        "type": "string",
        "metadata": {
          "description": "Location for Namespace"
        }
      }
    },
    "variables": {
      "namespaceNetworkRuleSetName": "[concat(parameters('eventhubNamespaceName'), concat('/', 'default'))]",
    },
    "resources": [
      {
        "apiVersion": "2018-01-01-preview",
        "name": "[parameters('eventhubNamespaceName')]",
        "type": "Microsoft.EventHub/namespaces",
        "location": "[parameters('location')]",
        "sku": {
          "name": "Standard",
          "tier": "Standard"
        },
        "properties": { }
      },
      {
        "apiVersion": "2018-01-01-preview",
        "name": "[variables('namespaceNetworkRuleSetName')]",
        "type": "Microsoft.EventHub/namespaces/networkrulesets",
        "dependsOn": [
          "[concat('Microsoft.EventHub/namespaces/', parameters('eventhubNamespaceName'))]"
        ],
        "properties": {
          "virtualNetworkRules": [<YOUR EXISTING VIRTUAL NETWORK RULES>],
          "ipRules": 
          [
            {
                "ipMask":"10.1.1.1",
                "action":"Allow"
            },
            {
                "ipMask":"11.0.0.0/24",
                "action":"Allow"
            }
          ],
          "trustedServiceAccessEnabled": false,
          "defaultAction": "Deny"
        }
      }
    ],
    "outputs": { }
  }
```

Инструкции по развертыванию шаблона см. в статье [Развертывание ресурсов с помощью шаблонов ARM и Azure PowerShell][lnk-deploy].

## <a name="next-steps"></a>Дальнейшие действия

Инструкции по ограничению доступа к Центрам событий из виртуальных сетей Azure, см. по следующей ссылке:

- [Конечные точки служб для виртуальной сети для Центров событий][lnk-vnet]

<!-- Links -->

[express-route]:  ../expressroute/expressroute-faqs.md#supported-services
[lnk-deploy]: ../azure-resource-manager/templates/deploy-powershell.md
[lnk-vnet]: event-hubs-service-endpoints.md
