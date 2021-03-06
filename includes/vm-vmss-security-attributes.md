---
author: msmbaldwin
ms.service: virtual-machines
ms.topic: include
ms.date: 07/10/2019
ms.author: mbaldwin
ms.openlocfilehash: 520fd50b2b0864f43c08687f05de377679b36d84
ms.sourcegitcommit: 7c5a2a3068e5330b77f3c6738d6de1e03d3c3b7d
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/11/2019
ms.locfileid: "70886939"
---
## <a name="preventative"></a>профилактическая;

| Управление безопасностью | Да или нет | Примечания |
|---|---|--|
| Шифрование неактивных (таких как шифрование на стороне сервера, шифрование на стороне сервера с помощью управляемых клиентом ключей и другие функции шифрования). | Да | Узнайте, [как зашифровать виртуальную машину Linux в Azure](/azure/virtual-machines/linux/encrypt-disks) и зашифровать [Виртуальные диски на виртуальной машине Windows](/azure/virtual-machines/windows/encrypt-disks). |
| Шифрование при передаче (например, шифрование ExpressRoute, Шифрование виртуальной сети и шифрование виртуальной сети)| Да | Виртуальные машины Azure поддерживают шифрование [ExpressRoute](/azure/expressroute) и виртуальной сети. См. раздел [Шифрование транзитного пути в виртуальных машинах](/azure/security/security-azure-encryption-overview#in-transit-encryption-in-vms). |
| Обработка ключа шифрования (CMK, BYOK и т. д.)| Да | Ключи, управляемые клиентом, являются поддерживаемым сценарием шифрования Azure. см. раздел [Общие сведения о шифровании Azure](/azure/security/security-azure-encryption-overview#in-transit-encryption-in-vms).|
| Шифрование на уровне столбцов (службы данных Azure)| Н/Д | |
| Вызовы API в зашифрованном виде| Да | Через HTTPS и SSL. |

## <a name="network-segmentation"></a>сегментация сети;

| Управление безопасностью | Да или нет | Примечания |
|---|---|--|
| Поддержка конечных точек службы| Да | |
| Поддержка внедрения виртуальной сети| Да | . |
| Поддержка сетевой изоляции и брандмауэров| Да |  |
| Поддержка принудительного туннелирования| Да | См. раздел [Настройка принудительного туннелирования с помощью модели развертывания Azure Resource Manager](/azure/vpn-gateway/vpn-gateway-forced-tunneling-rm). |

## <a name="detection"></a>Обнаружение

| Управление безопасностью | Да или нет | Примечания|
|---|---|--|
| Поддержка мониторинга Azure (log Analytics, App Insights и т. д.)| Да | См. статью [мониторинг и обновление виртуальной машины Linux в Azure](/azure/virtual-machines/linux/tutorial-monitoring) и [Отслеживание и обновление виртуальной машины Windows в Azure](/azure/virtual-machines/windows/tutorial-monitoring). |

## <a name="identity-and-access-management"></a>Управление удостоверениями и доступом

| Управление безопасностью | Да или нет | Примечания|
|---|---|--|
| Проверка подлинности| Да |  |
| Authorization| Да |  |


## <a name="audit-trail"></a>журнал аудита;

| Управление безопасностью | Да или нет | Примечания|
|---|---|--|
| Ведение журнала и аудит в плоскости управления и управления| Да |  |
| Ведение журнала и аудит в плоскости данных | Нет |  |

## <a name="configuration-management"></a>Управление конфигурацией

| Управление безопасностью | Да или нет | Примечания|
|---|---|--|
| Поддержка управления конфигурацией (управление версиями конфигураций и т. д.)| Да |  | 
