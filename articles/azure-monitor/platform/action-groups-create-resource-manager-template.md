---
title: Создание групп действий с помощью шаблонов Resource Manager
description: Узнайте, как создать группу действий с помощью шаблона Azure Resource Manager.
author: dkamstra
services: azure-monitor
ms.topic: conceptual
ms.date: 02/16/2018
ms.author: dukek
ms.subservice: alerts
ms.openlocfilehash: 5c02812d4c97b94667fcddcb275243e7a9b36b29
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2020
ms.locfileid: "87321910"
---
# <a name="create-an-action-group-with-a-resource-manager-template"></a>Создание группы действий с помощью шаблона Resource Manager
В этой статье показано, как можно использовать [шаблон Azure Resource Manager](../../azure-resource-manager/templates/template-syntax.md) для настройки групп действий. С помощью шаблонов можно автоматически настроить группы действий, которые можно использовать повторно в определенных типах оповещений. С помощью этих групп действий обеспечивается уведомление соответствующих участников при активации оповещения.

Основными шагами являются:

1. Создайте шаблон в виде JSON-файла, который описывает создание группы действий.

2. Разверните шаблон, используя [любой метод развертывания](../../azure-resource-manager/templates/deploy-powershell.md).

Ниже описано, как создать шаблон Resource Manager для группы действий, в котором определения действий жестко запрограммированы, а также представлены способы создания шаблона, принимающего данные конфигурации веб-перехватчика в качестве входных параметров при развертывании шаблона.

## <a name="resource-manager-templates-for-an-action-group"></a>Шаблоны Resource Manager для группы действий

Чтобы создать группу действий с помощью шаблона Resource Manager, создайте ресурс типа `Microsoft.Insights/actionGroups`. Затем следует заполнить все связанные свойства. Ниже представлено два примера шаблонов для создания группы действий.

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "actionGroupName": {
      "type": "string",
      "metadata": {
        "description": "Unique name (within the Resource Group) for the Action group."
      }
    },
    "actionGroupShortName": {
      "type": "string",
      "metadata": {
        "description": "Short name (maximum 12 characters) for the Action group."
      }
    }
  },
  "resources": [
    {
      "type": "Microsoft.Insights/actionGroups",
      "apiVersion": "2018-03-01",
      "name": "[parameters('actionGroupName')]",
      "location": "Global",
      "properties": {
        "groupShortName": "[parameters('actionGroupShortName')]",
        "enabled": true,
        "smsReceivers": [
          {
            "name": "contosoSMS",
            "countryCode": "1",
            "phoneNumber": "5555551212"
          },
          {
            "name": "contosoSMS2",
            "countryCode": "1",
            "phoneNumber": "5555552121"
          }
        ],
        "emailReceivers": [
          {
            "name": "contosoEmail",
            "emailAddress": "devops@contoso.com"
          },
          {
            "name": "contosoEmail2",
            "emailAddress": "devops2@contoso.com"
          }
        ],
        "webhookReceivers": [
          {
            "name": "contosoHook",
            "serviceUri": "http://requestb.in/1bq62iu1"
          },
          {
            "name": "contosoHook2",
            "serviceUri": "http://requestb.in/1bq62iu2"
          }
        ]
      }
    }
  ],
  "outputs":{
      "actionGroupId":{
          "type":"string",
          "value":"[resourceId('Microsoft.Insights/actionGroups',parameters('actionGroupName'))]"
      }
  }
}
```

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "actionGroupName": {
      "type": "string",
      "metadata": {
        "description": "Unique name (within the Resource Group) for the Action group."
      }
    },
    "actionGroupShortName": {
      "type": "string",
      "metadata": {
        "description": "Short name (maximum 12 characters) for the Action group."
      }
    },
    "webhookReceiverName": {
      "type": "string",
      "metadata": {
        "description": "Webhook receiver service Name."
      }
    },    
    "webhookServiceUri": {
      "type": "string",
      "metadata": {
        "description": "Webhook receiver service URI."
      }
    }    
  },
  "resources": [
    {
      "type": "Microsoft.Insights/actionGroups",
      "apiVersion": "2018-03-01",
      "name": "[parameters('actionGroupName')]",
      "location": "Global",
      "properties": {
        "groupShortName": "[parameters('actionGroupShortName')]",
        "enabled": true,
        "smsReceivers": [
        ],
        "emailReceivers": [
        ],
        "webhookReceivers": [
          {
            "name": "[parameters('webhookReceiverName')]",
            "serviceUri": "[parameters('webhookServiceUri')]"
          }
        ]
      }
    }
  ],
  "outputs":{
      "actionGroupResourceId":{
          "type":"string",
          "value":"[resourceId('Microsoft.Insights/actionGroups',parameters('actionGroupName'))]"
      }
  }
}
```


## <a name="next-steps"></a>Дальнейшие действия
* Дополнительные сведения о группах действий см. в статье [Создание групп действий и управление ими на портале Azure](./action-groups.md).
* Дополнительные сведения об [оповещениях](alerts-overview.md).
* Узнайте, как добавить [оповещения с помощью шаблона Resource Manager](./alerts-activity-log.md).

