---
title: Автоматическое восстановление экземпляра с помощью масштабируемых наборов виртуальных машин Azure
description: Узнайте, как настроить политику автоматического восстановления для экземпляров виртуальных машин в масштабируемом наборе.
author: avirishuv
ms.author: avverma
ms.topic: conceptual
ms.service: virtual-machine-scale-sets
ms.subservice: availability
ms.date: 02/28/2020
ms.reviewer: jushiman
ms.custom: avverma
ms.openlocfilehash: 45c316c1d1dd56f6d920423a725b2488df1a5032
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2020
ms.locfileid: "86527427"
---
# <a name="automatic-instance-repairs-for-azure-virtual-machine-scale-sets"></a>Автоматическое исправление экземпляра для масштабируемых наборов виртуальных машин Azure

Включение автоматического восстановления экземпляров для масштабируемых наборов виртуальных машин Azure обеспечивает высокий уровень доступности для приложений за счет поддержания набора работоспособных экземпляров. Если экземпляр в масштабируемом наборе находится в состоянии неработоспособности, о котором сообщает [расширение работоспособности приложения](./virtual-machine-scale-sets-health-extension.md) или [проверки работоспособности балансировщика нагрузки](../load-balancer/load-balancer-custom-probe-overview.md), эта функция автоматически выполняет восстановление экземпляра, удаляя неработоспособный экземпляр и создавая новый, чтобы заменить его.

## <a name="requirements-for-using-automatic-instance-repairs"></a>Требования к использованию автоматического восстановления экземпляров

**Включение мониторинга работоспособности приложений для масштабируемого набора**

В масштабируемом наборе должен быть включен мониторинг работоспособности приложений для экземпляров. Это можно сделать с помощью [расширения работоспособности приложения](./virtual-machine-scale-sets-health-extension.md) или [проб работоспособности подсистемы балансировки нагрузки](../load-balancer/load-balancer-custom-probe-overview.md). В каждый момент времени можно включить только один из них. Расширение работоспособности приложения или балансировщик нагрузки проверяет связь с конечной точкой приложения, настроенной на экземплярах виртуальной машины, чтобы определить состояние работоспособности приложения. Это состояние работоспособности используется масштабируемым набором Orchestrator для отслеживания работоспособности экземпляра и выполнения восстановления при необходимости.

**Настройка конечной точки для предоставления состояния работоспособности**

Прежде чем включить политику автоматического восстановления экземпляра, убедитесь, что в экземплярах масштабируемого набора настроена конечная точка приложения для отправки состояния работоспособности приложения. Когда экземпляр возвращает в конечной точке приложения состояние 200 (ОК), экземпляр помечается как "Исправен". Во всех остальных случаях экземпляр помечается как "неработоспособный", включая следующие сценарии.

- Если в экземплярах виртуальной машины не настроена конечная точка приложения для предоставления состояния работоспособности приложения
- Если конечная точка приложения настроена неправильно
- Если конечная точка приложения недоступна

Для экземпляров, помеченных как "неработоспособные", автоматическое восстановление инициируется масштабируемым набором. Убедитесь, что конечная точка приложения настроена правильно, прежде чем включать политику автоматического восстановления, чтобы избежать непреднамеренного восстановления экземпляров, пока выполняется настройка конечной точки.

**Включить одну группу размещения**

В настоящее время эта функция доступна только для масштабируемых наборов, развернутых в одной группе размещения. Свойству *singlePlacementGroup* должно быть присвоено *значение true* , чтобы в масштабируемом наборе использовалась функция автоматического восстановления экземпляров. Дополнительные сведения о [группах размещения](./virtual-machine-scale-sets-placement-groups.md#placement-groups).

**Версия API**

Политика автоматического исправления поддерживается для API вычислений версии 2018-10-01 или более поздней.

**Ограничения на перемещение ресурсов или подписок**

Перемещение ресурсов или подписок в настоящее время не поддерживается для масштабируемых наборов, если включена функция автоматического исправления.

**Ограничение для масштабируемых наборов Service Fabric**

В настоящее время эта функция не поддерживается для масштабируемых наборов Service Fabric.

## <a name="how-do-automatic-instance-repairs-work"></a>Как автоматически выполняется восстановление экземпляра?

Функция автоматического восстановления экземпляра основана на мониторинге работоспособности отдельных экземпляров в масштабируемом наборе. Экземпляры виртуальных машин в масштабируемом наборе можно настроить для отправки состояния работоспособности приложения с помощью [расширения работоспособности приложения](./virtual-machine-scale-sets-health-extension.md) или [проб работоспособности балансировщика нагрузки](../load-balancer/load-balancer-custom-probe-overview.md). Если экземпляр находится в неработоспособном состоянии, то масштабируемый набор выполняет действие по восстановлению, удаляя неработоспособный экземпляр и создавая новый, чтобы заменить его. Для создания нового экземпляра используется последняя модель масштабируемого набора виртуальных машин. Эту функцию можно включить в модели масштабируемого набора виртуальных машин с помощью объекта *аутоматикрепаирсполици* .

### <a name="batching"></a>Пакетная обработка

Операции автоматического восстановления экземпляров выполняются в пакетах. В любой момент времени не будет выполнено восстановление более 5% экземпляров в масштабируемом наборе с помощью политики автоматического восстановления. Это позволяет избежать одновременного удаления и повторного создания большого количества экземпляров при обнаружении неработоспособности в то же время.

### <a name="grace-period"></a>Льготный период

Когда экземпляр проходит операцию изменения состояния из-за выполнения операции помещения, исправления или публикации в масштабируемом наборе (например, переобразировать, повторное развертывание, обновление и т. д.), все действия по восстановлению этого экземпляра выполняются только после ожидания льготного периода. Период отсрочки — это время, в течение которого экземпляр возвращается в работоспособное состояние. Льготный период начинается после изменения состояния. Это помогает избежать выполнения преждевременных или случайных операций восстановления. Льготный период учитывается для всех вновь созданных экземпляров в масштабируемом наборе (включая созданный в результате операции восстановления). Льготный период указывается в минутах в формате ISO 8601 и может быть задан с помощью свойства *аутоматикрепаирсполици. грацепериод*. Льготный период может варьироваться от 30 минут до 90 минут и по умолчанию равен 30 минутам.

### <a name="suspension-of-repairs"></a>Приостановка восстановления 

Масштабируемые наборы виртуальных машин предоставляют возможность временного приостановки автоматического восстановления экземпляра при необходимости. *ServiceState* для автоматического восстановления в свойстве *орчестратионсервицес* в представлении экземпляров масштабируемого набора виртуальных машин показывает текущее состояние автоматического восстановления. Если масштабируемый набор выбран в автоматическом восстановлении, для параметра *serviceState* устанавливается значение *работает*. Если автоматическое восстановление приостановлено для масштабируемого набора, параметр *serviceState* имеет значение *suspended*. Если *аутоматикрепаирсполици* определен в масштабируемом наборе, но функция автоматического восстановления не включена, то параметру *serviceState* будет присвоено значение " *не работает*".

Если вновь созданные экземпляры для замены неработоспособных объектов в масштабируемом наборе продолжают оставаться неработоспособными даже после повторного выполнения операций восстановления, то в качестве меры безопасности платформа обновляет *serviceState* для автоматического восстановления до *приостановки*. Вы можете снова возобновить автоматическое восстановление, установив значение *serviceState* для параметра Автоматическое восстановление на *Запуск*. Подробные инструкции приведены в разделе [Просмотр и обновление состояния службы политики автоматического восстановления](#viewing-and-updating-the-service-state-of-automatic-instance-repairs-policy) для масштабируемого набора. 

Процесс автоматического восстановления экземпляра работает следующим образом.

1. [Модуль](./virtual-machine-scale-sets-health-extension.md) проверки работоспособности приложения или [подсистемы балансировки нагрузки проверяет](../load-balancer/load-balancer-custom-probe-overview.md) связь с конечной точкой приложения в каждой виртуальной машине в масштабируемом наборе, чтобы получить состояние работоспособности приложения для каждого экземпляра.
2. Если конечная точка реагирует на состояние 200 (ОК), экземпляр помечается как "Исправен". Во всех остальных случаях (в том числе если конечная точка недоступна) экземпляр помечается как "неработоспособный".
3. Если экземпляр находится в неработоспособном состоянии, масштабируемый набор активирует действие восстановления, удаляя неработоспособный экземпляр и создавая новый, чтобы заменить его.
4. Восстановление экземпляров выполняется в пакетах. В любой момент времени будет восстановлено не более 5% всего экземпляров в масштабируемом наборе. Если масштабируемый набор содержит менее 20 экземпляров, то восстановление выполняется для одного неработоспособного экземпляра за раз.
5. Описанный выше процесс будет продолжен до тех пор, пока не будут восстановлены все неработоспособные экземпляры в масштабируемом наборе.

## <a name="instance-protection-and-automatic-repairs"></a>Защита экземпляра и автоматическое восстановление

Если экземпляр в масштабируемом наборе защищен путем применения одной из [политик защиты](./virtual-machine-scale-sets-instance-protection.md), автоматическое восстановление не выполняется на этом экземпляре. Это относится как к политикам защиты: *Защита от масштабирования* , так и *Защита от действий по масштабированию* . 

## <a name="terminatenotificationandautomaticrepairs"></a>Завершение уведомлений и автоматическое восстановление

Если функция [прекращения уведомлений](./virtual-machine-scale-sets-terminate-notification.md) включена в масштабируемом наборе, то во время автоматической операции восстановления удаление неработоспособного экземпляра соответствует конфигурации уведомления о прекращении. Уведомление об увольнении отправляется через службу метаданных Azure — запланированные события, а удаление экземпляра откладывается на продолжительность настроенного времени ожидания задержки. Однако создание нового экземпляра для замены неработоспособного состояния не ждет завершения времени ожидания задержки.

## <a name="enabling-automatic-repairs-policy-when-creating-a-new-scale-set"></a>Включение политики автоматического восстановления при создании нового масштабируемого набора

Чтобы включить автоматическую политику восстановления при создании масштабируемого набора, убедитесь, что выполнены все [требования](#requirements-for-using-automatic-instance-repairs) к включению этой функции. Конечная точка приложения должна быть правильно настроена для экземпляров масштабируемых наборов, чтобы избежать инициации непреднамеренного восстановления во время настройки конечной точки. Для вновь созданных масштабируемых наборов все операции восстановления экземпляров выполняются только после ожидания в течение периода отсрочки. Чтобы включить автоматическое восстановление экземпляров в масштабируемом наборе, используйте объект *аутоматикрепаирсполици* в модели масштабируемого набора виртуальных машин.

Этот [шаблон краткого руководства](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-automatic-repairs-slb-health-probe) также можно использовать для развертывания масштабируемого набора виртуальных машин с проверкой работоспособности подсистемы балансировки нагрузки и автоматического восстановления экземпляров с льготным периодом 30 минут.

### <a name="azure-portal"></a>Портал Azure
 
При создании нового масштабируемого набора следует выполнить следующие действия, включив политику автоматического восстановления.
 
1. Перейдите к **масштабируемым наборам виртуальных машин**.
1. Выберите **+ Добавить** , чтобы создать новый масштабируемый набор.
1. Перейдите на вкладку **работоспособность** . 
1. Откройте раздел **работоспособность** .
1. Включите параметр **отслеживать работоспособность приложения** .
1. Откройте раздел **политика автоматического восстановления** .
1. Включите **параметр** **Автоматическое восстановление** .
1. В поле **льготный период (мин.)** укажите льготный период в минутах, допустимые значения — от 30 до 90 минут. 
1. Завершив создание нового масштабируемого набора, нажмите кнопку " **проверить и создать** ".

### <a name="rest-api"></a>REST API

В следующем примере показано, как включить автоматическое восстановление экземпляров в модели масштабируемого набора. Используйте API версии 2018-10-01 или более поздней.

```
PUT or PATCH on '/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Compute/virtualMachineScaleSets/{vmScaleSetName}?api-version=2019-07-01'
```

```json
{
  "properties": {
    "automaticRepairsPolicy": {
            "enabled": "true",
            "gracePeriod": "PT30M"
        }
    }
}
```

### <a name="azure-powershell"></a>Azure PowerShell

Функцию автоматического восстановления экземпляра можно включить при создании нового масштабируемого набора с помощью командлета [New-азвмссконфиг](/powershell/module/az.compute/new-azvmssconfig) . Этот пример скрипта проходит по созданию масштабируемого набора и связанных ресурсов с помощью файла конфигурации: [Создание полного масштабируемого набора виртуальных машин](./scripts/powershell-sample-create-complete-scale-set.md). Политику автоматического восстановления экземпляра можно настроить, добавив параметры *енаблеаутоматикрепаир* и *аутоматикрепаирграцепериод* в объект конфигурации для создания масштабируемого набора. В следующем примере включается функция с льготным периодом 30 минут.

```azurepowershell-interactive
New-AzVmssConfig `
 -Location "EastUS" `
 -SkuCapacity 2 `
 -SkuName "Standard_DS2" `
 -UpgradePolicyMode "Automatic" `
 -EnableAutomaticRepair $true `
 -AutomaticRepairGracePeriod "PT30M"
```

### <a name="azure-cli-20"></a>Azure CLI 2.0

В следующем примере включается политика автоматического восстановления при создании нового масштабируемого набора с помощью команды *[AZ vmss Create](/cli/azure/vmss?view=azure-cli-latest#az-vmss-create)*. Сначала создайте группу ресурсов, а затем создайте масштабируемый набор с периодом отсрочки политики автоматического восстановления, равным 30 минутам.

```azurecli-interactive
az group create --name <myResourceGroup> --location <VMSSLocation>
az vmss create \
  --resource-group <myResourceGroup> \
  --name <myVMScaleSet> \
  --image UbuntuLTS \
  --admin-username <azureuser> \
  --generate-ssh-keys \
  --load-balancer <existingLoadBalancer> \
  --health-probe <existingHealthProbeUnderLoaderBalancer> \
  --automatic-repairs-grace-period 30
```

В приведенном выше примере используется имеющаяся подсистема балансировки нагрузки и проверка работоспособности для наблюдения за состоянием работоспособности приложений экземпляров. Если вместо этого вы предпочитаете использовать расширение работоспособности приложения для мониторинга, можно создать масштабируемый набор, настроить расширение работоспособности приложения, а затем включить политику автоматического восстановления экземпляра с помощью команды *AZ vmss Update*, как описано в следующем разделе.

## <a name="enabling-automatic-repairs-policy-when-updating-an-existing-scale-set"></a>Включение политики автоматического восстановления при обновлении существующего масштабируемого набора

Прежде чем включить автоматическую политику восстановления в существующем масштабируемом наборе, убедитесь, что выполнены все [требования](#requirements-for-using-automatic-instance-repairs) к включению этой функции. Конечная точка приложения должна быть правильно настроена для экземпляров масштабируемых наборов, чтобы избежать инициации непреднамеренного восстановления во время настройки конечной точки. Чтобы включить автоматическое восстановление экземпляров в масштабируемом наборе, используйте объект *аутоматикрепаирсполици* в модели масштабируемого набора виртуальных машин.

После обновления модели существующего масштабируемого набора убедитесь, что последняя модель применяется ко всем экземплярам шкалы. Ознакомьтесь с инструкциями [о том, как обеспечить актуальность виртуальных машин с помощью последней модели масштабируемого набора](./virtual-machine-scale-sets-upgrade-scale-set.md#how-to-bring-vms-up-to-date-with-the-latest-scale-set-model).

### <a name="azure-portal"></a>Портал Azure

Политику автоматического восстановления существующего масштабируемого набора можно изменить с помощью портал Azure. 
 
1. Перейдите к существующему масштабируемому набору виртуальных машин.
1. В разделе **Параметры** в меню слева выберите **работоспособность и восстановление**.
1. Включите параметр **отслеживать работоспособность приложения** .
1. Откройте раздел **политика автоматического восстановления** .
1. Включите **параметр** **Автоматическое восстановление** .
1. В поле **льготный период (мин.)** укажите льготный период в минутах, допустимые значения — от 30 до 90 минут. 
1. По завершении нажмите кнопку **Сохранить**. 

### <a name="rest-api"></a>REST API

В следующем примере включается политика с льготным периодом в 40 минут. Используйте API версии 2018-10-01 или более поздней.

```
PUT or PATCH on '/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Compute/virtualMachineScaleSets/{vmScaleSetName}?api-version=2019-07-01'
```

```json
{
  "properties": {
    "automaticRepairsPolicy": {
            "enabled": "true",
            "gracePeriod": "PT40M"
        }
    }
}
```

### <a name="azure-powershell"></a>Azure PowerShell

Используйте командлет [Update-азвмсс](/powershell/module/az.compute/update-azvmss) , чтобы изменить конфигурацию функции автоматического восстановления экземпляра в существующем масштабируемом наборе. В следующем примере период отсрочки обновляется до 40 минут.

```azurepowershell-interactive
Update-AzVmss `
 -ResourceGroupName "myResourceGroup" `
 -VMScaleSetName "myScaleSet" `
 -EnableAutomaticRepair $true `
 -AutomaticRepairGracePeriod "PT40M"
```

### <a name="azure-cli-20"></a>Azure CLI 2.0

Ниже приведен пример обновления политики автоматического восстановления экземпляра существующего масштабируемого набора с помощью команды *[AZ vmss Update](/cli/azure/vmss?view=azure-cli-latest#az-vmss-update)*.

```azurecli-interactive
az vmss update \  
  --resource-group <myResourceGroup> \
  --name <myVMScaleSet> \
  --enable-automatic-repairs true \
  --automatic-repairs-grace-period 30
```

## <a name="viewing-and-updating-the-service-state-of-automatic-instance-repairs-policy"></a>Просмотр и обновление состояния службы политика восстановления автоматического экземпляра

### <a name="rest-api"></a>REST API 

Используйте функцию [получения представления экземпляров](/rest/api/compute/virtualmachinescalesets/getinstanceview) с API версии 2019-12-01 или выше для масштабируемого набора виртуальных машин, чтобы просмотреть *serviceState* для автоматического восстановления в свойстве *орчестратионсервицес*. 

```http
GET '/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Compute/virtualMachineScaleSets/{vmScaleSetName}/instanceView?api-version=2019-12-01'
```

```json
{
  "orchestrationServices": [
    {
      "serviceName": "AutomaticRepairs",
      "serviceState": "Running"
    }
  ]
}
```

Используйте API *сеторчестратионсервицестате* с api версии 2019-12-01 или более поздней в масштабируемом наборе виртуальных машин, чтобы задать состояние автоматического восстановления. Когда масштабируемый набор будет включен в функцию автоматического восстановления, этот API можно использовать для приостановки или возобновления автоматического восстановления для масштабируемого набора. 

 ```http
 POST '/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Compute/virtualMachineScaleSets/{vmScaleSetName}/setOrchestrationServiceState?api-version=2019-12-01'
 ```

```json
{
  "orchestrationServices": [
    {
      "serviceName": "AutomaticRepairs",
      "serviceState": "Suspend"
    }
  ]
}
```

### <a name="azure-cli"></a>Azure CLI 

Чтобы просмотреть *serviceState* для автоматического восстановления экземпляра, используйте командлет [Get-instance-View](/cli/azure/vmss?view=azure-cli-latest#az-vmss-get-instance-view) . 

```azurecli-interactive
az vmss get-instance-view \
    --name MyScaleSet \
    --resource-group MyResourceGroup
```

Используйте командлет [Set-оркестрации-Service-State](/cli/azure/vmss?view=azure-cli-latest#az-vmss-set-orchestration-service-state) , чтобы обновить *serviceState* для автоматического восстановления экземпляров. После того как масштабируемый набор будет включен в функцию автоматического восстановления, этот командлет можно использовать для приостановки или возобновления автоматического восстановления для масштабируемого набора. 

```azurecli-interactive
az vmss set-orchestration-service-state \
    --service-name AutomaticRepairs \
    --action Resume \
    --name MyScaleSet \
    --resource-group MyResourceGroup
```
### <a name="azure-powershell"></a>Azure PowerShell

Используйте командлет [Get-азвмсс](/powershell/module/az.compute/get-azvmss?view=azps-3.7.0) с параметром *InstanceView* , чтобы просмотреть *ServiceState* для автоматического восстановления экземпляра.

```azurepowershell-interactive
Get-AzVmss `
    -ResourceGroupName "myResourceGroup" `
    -VMScaleSetName "myScaleSet" `
    -InstanceView
```

Используйте командлет Set-AzVmssOrchestrationServiceState, чтобы обновить *serviceState* для автоматического восстановления экземпляров. Когда масштабируемый набор будет включен в функцию автоматического восстановления, этот командлет можно использовать для приостановки или возобновления автоматического восстановления для масштабируемого набора.

```azurepowershell-interactive
Set-AzVmssOrchestrationServiceState `
    -ResourceGroupName "myResourceGroup" `
    -VMScaleSetName "myScaleSet" `
    -ServiceName "AutomaticRepairs" `
    -Action "Suspend"
```

## <a name="troubleshoot"></a>Диагностика

**Не удалось включить политику автоматического восстановления**

Если вы получаете ошибку "BadRequest" с сообщением "не удалось найти член" Аутоматикрепаирсполици "для объекта типа" Properties ", проверьте версию API, используемую для масштабируемого набора виртуальных машин. Для этой функции требуется API версии 2018-10-01 или выше.

**Экземпляр не восстановился, даже если включена политика**

Экземпляр может находиться в льготном периоде. Время ожидания после любого изменения состояния в экземпляре перед выполнением восстановления. Это позволяет избежать преждевременного или случайного восстановления. Действие по восстановлению должно выполняться по окончании периода отсрочки для экземпляра.

**Просмотр состояния работоспособности приложения для экземпляров масштабируемых наборов**

Вы можете использовать [API получения представления](/rest/api/compute/virtualmachinescalesetvms/getinstanceview) экземпляров для экземпляров в масштабируемом наборе виртуальных машин, чтобы просмотреть состояние работоспособности приложения. С Azure PowerShell можно использовать командлет [Get-азвмссвм](/powershell/module/az.compute/get-azvmssvm) с флагом *-InstanceView* . Состояние работоспособности приложения указывается в свойстве *вмхеалс*.

В портал Azure также можно просмотреть состояние работоспособности. Перейдите к существующему масштабируемому набору, выберите **экземпляры** в меню слева и просмотрите столбец **состояние работоспособности** для состояния работоспособности каждого экземпляра масштабируемого набора. 

## <a name="next-steps"></a>Дальнейшие действия

Узнайте, как настроить [расширение работоспособности приложения](./virtual-machine-scale-sets-health-extension.md) или [пробы работоспособности подсистемы балансировки нагрузки](../load-balancer/load-balancer-custom-probe-overview.md) для масштабируемых наборов.
