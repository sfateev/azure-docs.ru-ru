---
title: Настройка правил брандмауэра IP-адресов для Служебной шины Azure
description: Использование правил брандмауэра для разрешения подключений к Служебной шине Azure с определенных IP-адресов.
ms.topic: article
ms.date: 06/23/2020
ms.openlocfilehash: 561ee90fb6d1e25123d15a09bbf143aef59bcf6f
ms.sourcegitcommit: 1b47921ae4298e7992c856b82cb8263470e9e6f9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/14/2020
ms.locfileid: "92058069"
---
# <a name="allow-access-to-azure-service-bus-namespace-from-specific-ip-addresses-or-ranges"></a>Разрешить доступ к пространству имен служебной шины Azure с конкретных IP-адресов или диапазонов
По умолчанию пространства имен Служебной шины доступны из Интернета при условии, что запрос поступает с действительной аутентификацией и авторизацией. С помощью IP-брандмауэра такой доступ можно дополнительно ограничить набором или диапазоном IPv4-адресов, введя их в нотации [CIDR](https://en.wikipedia.org/wiki/Classless_Inter-Domain_Routing).

Эта возможность полезна в сценариях, в которых Служебная шина Azure должна быть доступна только с определенных хорошо известных сайтов. Правила брандмауэра позволяют настроить правила приема трафика, поступающего с определенных IPv4-адресов. Например, при использовании Служебной шины с помощью [Azure ExpressRoute][express-route] можно создать **правило брандмауэра**, разрешающее трафик только с IP-адресов в локальной инфраструктуре или адресов корпоративного шлюза NAT. 

> [!IMPORTANT]
> Брандмауэры и виртуальные сети поддерживаются только в службе "Служебная шина" **ценовой категории "Премиум"** . Если обновления до уровня **Premier** не подходит, рекомендуется защитить маркер подписанного URL-адреса (SAS) и предоставить к нему доступ только полномочным пользователям. Дополнительные сведения о проверке подлинности SAS см. в разделе [Проверка подлинности и авторизация](service-bus-authentication-and-authorization.md#shared-access-signature).

## <a name="ip-firewall-rules"></a>Правила брандмауэра для IP-адресов
Правила брандмауэра для IP-адресов применяются на уровне пространства имен Служебной шины. Поэтому они действуют для всех клиентских подключений по любым поддерживаемым протоколам. Любые попытки подключения с IP-адресов, которые не соответствуют правилу разрешенных IP-адресов для пространства имен Служебной шины, отклоняются. В ответе клиенту правило фильтрации IP-адресов не упоминается. Правила фильтрации IP-адресов применяются по порядку, поэтому первое правило, которое соответствует IP-адресу, определяет действие (принять или отклонить).

>[!WARNING]
> Реализация правил брандмауэра может предотвратить взаимодействие других служб Azure со Служебной шиной.
>
> Доверенные службы Майкрософт не поддерживаются, если реализована фильтрация IP (правила брандмауэра). Их поддержка будет доступна в ближайшее время.
>
> Распространенные сценарии Azure, которые не работают с фильтрацией IP (обратите внимание, что список **НЕ** является исчерпывающим):
> - Интеграция со службой "Сетка событий Azure".
> - Маршруты Центра Интернета вещей Azure.
> - Device Explorer Интернета вещей Azure.
>
> В виртуальной сети должны присутствовать следующие службы Майкрософт:
> - Служба приложений Azure
> - Функции Azure
> - Azure Monitor (параметр диагностики)

## <a name="use-azure-portal"></a>Использование портала Azure
В этом разделе показано, как использовать портал Azure для создания правил брандмауэра для пространства имен Служебной шины Azure. 

1. Перейдите к **пространству имен Служебной шины** на [портале Azure](https://portal.azure.com).
2. В меню слева выберите параметр **сеть** в разделе **Параметры**.  

    > [!NOTE]
    > Вкладка " **сеть** " отображается только для пространств имен " **премиум** ".  
    
    По умолчанию выбран параметр **Выбранные сети** . Если на этой странице не добавлено по крайней мере одно правило брандмауэра IP или виртуальная сеть, доступ к этому пространству имен можно получить через общедоступный Интернет (с помощью ключа доступа).

    :::image type="content" source="./media/service-bus-ip-filtering/default-networking-page.png" alt-text="Страница &quot;Сетевые подключения&quot; — по умолчанию" lightbox="./media/service-bus-ip-filtering/default-networking-page.png":::
    
    Если выбран параметр **все сети** , пространство имен служебной шины принимает подключения с любого IP-адреса. То есть, такое значение параметра по умолчанию равноценно правилу, которое принимает диапазон IP-адресов 0.0.0.0/0. 

    ![Снимок экрана со страницей портал Azure сети. Параметр Разрешить доступ из всех сетей выбран на вкладке брандмауэры и виртуальные сети.](./media/service-bus-ip-filtering/firewall-all-networks-selected.png)
1. Чтобы разрешить доступ только с указанного IP-адреса, выберите вариант **Выбранные сети** , если он еще не выбран. В разделе **Брандмауэр** выполните следующие шаги:
    1. Выберите вариант **Добавить IP-адрес клиента**, чтобы предоставить текущему IP-адресу клиента доступ к пространству имен. 
    2. В качестве **диапазона адресов** введите определенный IPv4-адрес или диапазон IPv4-адресов в нотации CIDR. 
    3. Укажите, следует ли **разрешить доверенным службам Майкрософт обходить этот брандмауэр**. 

        > [!WARNING]
        > Если вы выберете вариант **Выбранные сети** и не укажете IP-адрес или диапазон IP-адресов, служба разрешит трафик из всех сетей. 

        ![Снимок экрана со страницей портал Azure сети. Выбран параметр Разрешить доступ из выбранных сетей, и раздел брандмауэр выделен.](./media/service-bus-ip-filtering/firewall-selected-networks-trusted-access-disabled.png)
3. На панели инструментов нажмите **Сохранить**, чтобы сохранить параметры. Подождите несколько минут, чтобы подтверждение появилось в уведомлениях портала.

    > [!NOTE]
    > Чтобы ограничить доступ к определенным виртуальным сетям, см. раздел [разрешение доступа из конкретных сетей](service-bus-service-endpoints.md).

## <a name="use-resource-manager-template"></a>Использование шаблона Resource Manager
В этом разделе приведен пример шаблона Azure Resource Manager, который создает виртуальную сеть и правило брандмауэра.


Следующий шаблон Resource Manager позволяет добавить правило виртуальной сети в пространство имен существующей Служебной шины.

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
      "servicebusNamespaceName": {
        "type": "string",
        "metadata": {
          "description": "Name of the Service Bus namespace"
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
      "namespaceNetworkRuleSetName": "[concat(parameters('servicebusNamespaceName'), concat('/', 'default'))]",
    },
    "resources": [
      {
        "apiVersion": "2018-01-01-preview",
        "name": "[parameters('servicebusNamespaceName')]",
        "type": "Microsoft.ServiceBus/namespaces",
        "location": "[parameters('location')]",
        "sku": {
          "name": "Premium",
          "tier": "Premium"
        },
        "properties": { }
      },
      {
        "apiVersion": "2018-01-01-preview",
        "name": "[variables('namespaceNetworkRuleSetName')]",
        "type": "Microsoft.ServiceBus/namespaces/networkrulesets",
        "dependsOn": [
          "[concat('Microsoft.ServiceBus/namespaces/', parameters('servicebusNamespaceName'))]"
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

Инструкции по ограничению доступа к Служебной шине из виртуальных сетей Azure, см. по следующей ссылке:

- [Use Virtual Network service endpoints with Azure Service Bus][lnk-vnet] (Использование конечных точек служб для виртуальной сети для Служебной шины Azure)

<!-- Links -->

[lnk-deploy]: ../azure-resource-manager/templates/deploy-powershell.md
[lnk-vnet]: service-bus-service-endpoints.md
[express-route]:  ../expressroute/expressroute-faqs.md#supported-services