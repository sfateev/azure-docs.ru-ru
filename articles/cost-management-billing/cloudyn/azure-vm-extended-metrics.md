---
title: Добавление расширенных метрик для виртуальных машин Azure
description: В этой статье описывается, как включить и настроить расширенные метрики диагностики для виртуальных машин Azure.
author: bandersmsft
ms.reviewer: vitavor
ms.author: banders
ms.date: 03/12/2020
ms.topic: conceptual
ms.service: cost-management-billing
ms.subservice: cloudyn
ms.custom: seodec18
ROBOTS: NOINDEX
ms.openlocfilehash: 89084f0631b52631708db68a11595cb24d1b9fee
ms.sourcegitcommit: 56cbd6d97cb52e61ceb6d3894abe1977713354d9
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/20/2020
ms.locfileid: "88690125"
---
# <a name="add-extended-metrics-for-azure-virtual-machines"></a>Добавление расширенных метрик для виртуальных машин Azure

Служба Cloudyn использует данные метрик виртуальных машин Azure, чтобы показать подробные сведения о ресурсах. Кроме того, эти данные, также называемые счетчиками производительности, используются службой Cloudyn для создания отчетов. Тем не менее служба Cloudyn не собирает все данные метрик Azure из гостевых виртуальных машин автоматически — для этого необходимо включить коллекцию метрик. В этой статье описывается, как включить и настроить дополнительные метрики диагностики для виртуальных машин Azure.

После включения коллекции метрик вы можете:

- узнавать, когда виртуальные машины достигают ограничений ресурсов памяти, диска и ЦП;
- обнаруживать тенденции и аномалии потребления;
- управлять расходами, изменяя размеры в соответствии с потреблением;
- получать рекомендации по выбору размера с учетом рентабельности из службы Cloudyn.

Например, вам может потребоваться отслеживать процент используемой памяти и загрузки ЦП виртуальных машин Azure. Для этого используются такие метрики виртуальной машины Azure, как _Загрузка ЦП_ и _\Память\% использование выделенной памяти (в байтах)_ .

> [!NOTE]
> Расширенный сбор данных метрик поддерживается только с помощью мониторинга Azure на уровне гостя. Служба Cloudyn не совместима с [агентом Log Analytics](../../azure-monitor/platform/agents-overview.md).

[!INCLUDE [cloudyn-note](../../../includes/cloudyn-note.md)]

## <a name="determine-whether-extended-metrics-are-enabled"></a>Определение доступности расширенных метрик

1. Войдите на портал Azure по адресу [https://portal.azure.com](https://portal.azure.com).
2. В разделе **Виртуальные машины** выберите виртуальную машину, а затем в разделе **Мониторинг** выберите **Метрики**. Отобразится список доступных метрик.
3. Выберите несколько метрик, и на графике отобразятся их данные.  
    ![Пример метрики. Загрузка ЦП на узле](./media/azure-vm-extended-metrics/metric01.png)

В предыдущем примере для узлов доступен ограниченный набор стандартных метрик и недоступны метрики памяти. Метрики памяти относятся к расширенным метрикам. В этом случае расширенные метрики не доступны для виртуальной машины. Чтобы включить расширенные метрики, необходимо выполнить некоторые дополнительные действия. Они приведены в следующем разделе.

## <a name="enable-extended-metrics-in-the-azure-portal"></a>Включение расширенных метрик на портале Azure

Стандартные метрики являются метриками главного компьютера. Метрика _Загрузка ЦП_ — это один из примеров. Существуют также базовые метрики для гостевых виртуальных машин, которые также называются расширенными метриками. Примеры расширенных метрик — _\Память\% использование выделенной памяти (в байтах)_ и _\Память\доступные байты_.

Включить расширенные метрики достаточно просто. Включите мониторинг на уровне гостя для каждой виртуальной машины. При включении мониторинга на уровне гостя на виртуальную машину устанавливается агент диагностики Azure. Базовый набор расширенных метрик добавляется по умолчанию. Следующий процесс одинаков для классических и обычных виртуальных машин с ОС Windows и Linux.

Имейте в виду, что для мониторинга Azure и Linux на гостевом уровне требуется учетная запись хранения. При включении мониторинга на уровне гостя, если учетная запись хранения не выбрана, то она будет создана автоматически.

### <a name="enable-guest-level-monitoring-on-existing-vms"></a>Включение мониторинга на гостевом уровне в имеющихся виртуальных машинах

1. В разделе **Виртуальные машины** просмотрите список виртуальных машин и выберите нужную.
2. В разделе **Мониторинг** выберите **Параметры диагностики**.
3. На странице параметров диагностики щелкните **Включить мониторинг на гостевом уровне**.  
    ![Включение мониторинга на гостевом уровне на странице "Обзор"](./media/azure-vm-extended-metrics/enable-guest-monitoring.png)
4. Через несколько минут агент диагностики Azure будет установлен на виртуальную машину. Добавляется базовый набор метрик. Обновите страницу. Добавленные счетчики производительности отображаются на вкладке "Обзор".
5. В разделе "Мониторинг" выберите **Метрики**.
6. На диаграмме метрик в разделе **Пространство имен метрики** выберите **Гость (классическая модель)** .
7. В списке метрик можно просмотреть все доступные счетчики производительности для гостевой виртуальной машины.  
    ![Список примеров расширенных метрик](./media/azure-vm-extended-metrics/extended-metrics.png)

### <a name="enable-guest-level-monitoring-on-new-vms"></a>Включение мониторинга на гостевом уровне в новых виртуальных машинах

При создании новых виртуальных машин на вкладке "Управление" выберите значение **Включить** для параметра **диагностики гостевой ОС**.

![Включение диагностики гостевой ОС](./media/azure-vm-extended-metrics/new-enable-diag.png)

Дополнительные сведения о включении расширенных метрик для виртуальных машин Azure см. в руководствах по использованию [агента Linux для Azure](../../virtual-machines/extensions/agent-linux.md) и [агента виртуальной машины Azure](../../virtual-machines/extensions/agent-windows.md).

## <a name="resource-manager-credentials"></a>Учетные данные Resource Manager

После включения расширенных метрик убедитесь, что служба Cloudyn имеет доступ к [учетным данным Resource Manager](../../cost-management/activate-subs-accounts.md). Они нужны ей для сбора и отображения данных о производительности виртуальных машин. Они также используются для создания рекомендаций по оптимизации затрат. Службе Cloudyn требуются данные о производительности из экземпляра по крайней мере за три дня, чтобы определить, целесообразно ли уменьшение размера.

## <a name="enable-vm-metrics-with-a-script"></a>Включение метрик виртуальной машины с помощью скрипта

Вы можете включить метрики виртуальной машины с помощью скриптов Azure PowerShell. Если у вас много виртуальных машин, для которых необходимо включить метрики, можно использовать скрипт для автоматизации процесса. Примеры скриптов находятся в GitHub на странице о [включении диагностики Azure](https://github.com/Cloudyn/azure-enable-diagnostics).

## <a name="view-azure-performance-metrics"></a>Просмотр метрик производительности Azure

Чтобы просмотреть метрики производительности своих экземпляров Azure на портале Cloudyn, выберите **Ресурсы** > **Вычисления** > **Instance Explorer** (Обозреватель экземпляров). В списке экземпляров виртуальных машин разверните экземпляр, а затем разверните ресурс, чтобы просмотреть подробные сведения.

![Справочные данные, отображаемые в Обозревателе экземпляра](./media/azure-vm-extended-metrics/instance-explorer.png)

## <a name="next-steps"></a>Дальнейшие действия

- Если доступ к API Azure Resource Manager в учетных записях еще не включен, перейдите к разделу [Активация подписок и учетных записей Azure с помощью Cloudyn](../../cost-management/activate-subs-accounts.md).
