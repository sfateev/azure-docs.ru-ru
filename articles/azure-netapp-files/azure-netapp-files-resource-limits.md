---
title: Ограничения ресурсов для службы Azure NetApp Files | Документация Майкрософт
description: Описание ограничений для Azure NetApp Filesных ресурсов и способов запроса увеличения лимита ресурсов.
services: azure-netapp-files
documentationcenter: ''
author: b-juche
manager: ''
editor: ''
ms.assetid: ''
ms.service: azure-netapp-files
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 10/14/2020
ms.author: b-juche
ms.openlocfilehash: 6963a1f39534573bca39431febe391e89d462875
ms.sourcegitcommit: a92fbc09b859941ed64128db6ff72b7a7bcec6ab
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/15/2020
ms.locfileid: "92072787"
---
# <a name="resource-limits-for-azure-netapp-files"></a>Ограничения ресурсов для службы Azure NetApp Files

Понимание ограничений ресурсов для службы Azure NetApp Files помогает в управлении томами.

## <a name="resource-limits"></a>Ограничения ресурсов

В следующей таблице описаны ограничения ресурсов для Azure NetApp Files.

|  Ресурс  |  Ограничение по умолчанию  |  Регулируемый запрос поддержки Via  |
|----------------|---------------------|--------------------------------------|
|  Число учетных записей NetApp в каждом регионе Azure   |  10    |  Да   |
|  Число пулов мощностей на учетную запись NetApp   |    25     |   Да   |
|  Число томов на подписку   |    500     |   Да   |
|  Число томов на пул емкости     |    500   |    Да     |
|  Число моментальных снимков на том       |    255     |    Нет        |
|  Число подсетей, делегированных в Azure NetApp Files (Microsoft. NetApp/Volumes) на виртуальную сеть Azure    |   1   |    Нет    |
|  Количество используемых IP-адресов в виртуальной сети (включая немедленное одноранговое виртуальных сетей) с Azure NetApp Files   |    1000   |    Нет   |
|  Минимальный размер одного пула емкости   |  4 ТиБ     |    Нет  |
|  Максимальный размер одного пула емкости    |  500 ТиБ   |   Нет   |
|  Минимальный размер одного тома    |    100 ГиБ    |    нет    |
|  Максимальный размер одного тома     |    100 Тиб    |    Нет    |
|  Максимальный размер одного файла     |    16 ТиБ    |    Нет    |    
|  Максимальный размер метаданных каталога в одном каталоге      |    320 МБ    |    Нет    |    
|  Максимальное количество файлов ([MAXSIZE](#maxfiles)) на том     |    100 млн    |    Да    |    
|  Минимальная назначенная пропускная способность для тома QoS вручную     |    1 MiB/с   |    Нет    |    
|  Максимальная назначенная пропускная способность для тома QoS вручную     |    4 500 MiB/с    |    Нет    |    
|  Число томов защиты данных репликации между регионами (конечные тома)     |    5    |    Да    |     

Дополнительные сведения см. в разделе [вопросы и ответы по управлению емкостью](azure-netapp-files-faqs.md#capacity-management-faqs).

## <a name="maxfiles-limits"></a>Ограничения MAXSIZE <a name="maxfiles"></a> 

Azure NetApp Files тома имеют ограничение с именем *MAXSIZE*. Ограничение MAXSIZE — число файлов, которое может содержаться в томе. Ограничение MaxSize для тома Azure NetApp Files индексируется в зависимости от размера (квоты) тома. Ограничение MaxSize для тома увеличивается или уменьшается по частоте 20 000 000 файлов на Тиб подготовленного тома. 

Служба динамически корректирует ограничение MaxSize для тома на основе его подготовленного размера. Например, для тома, изначально настроенного с размером 1 Тиб, MAXSIZE ограничение в 20 000 000. Последующие изменения размера тома приведут к автоматической повторной настройке ограничения MAXSIZE на основе следующих правил. 

|    Размер тома (квота)     |  Автоматическая перенастройка ограничения MAXSIZE    |
|----------------------------|-------------------|
|    <= 1 тиб                |    20 млн     |
|    > 1 Тиб, но <= 2 тиб    |    40 000 000     |
|    > 2 Тиб, но <= 3 тиб    |    60 млн     |
|    > 3 Тиб, но <= 4 тиб    |    80 000 000     |
|    > 4 тиб                 |    100 млн    |

Если вы уже выделили по крайней мере 4 Тиб квоты для тома, вы можете отправить [запрос в службу поддержки](#limit_increase) , чтобы увеличить ограничение maxsize свыше 100 000 000. Для каждых 100 000 000 файлов (или их дробной части) необходимо увеличить соответствующую квоту тома на 4 тиб.  Например, если увеличить ограничение MAXSIZE с 100 000 000 файлов на 200 000 000 (или любое число в диапазоне), необходимо увеличить квоту тома с 4 Тиб до 8 тиб.

## <a name="request-limit-increase"></a>Увеличение лимита запросов <a name="limit_increase"></a> 

Вы можете создать запрос в службу поддержки Azure, чтобы увеличить регулируемые ограничения из приведенной выше таблицы. 

Из портал Azureной плоскости навигации: 

1. Щелкните **Справка и поддержка**.
2. Щелкните **+ новый запрос в службу поддержки**.
3. На вкладке Основные сведения укажите следующую информацию. 
    1. Тип проблемы: выберите **пределы службы и подписки (квоты)**.
    2. Подписки. Выберите подписку на ресурс, для которого требуется увеличить квоту.
    3. Тип квоты: выберите **хранилище: пределы Azure NetApp Files**.
    4. Нажмите кнопку **Далее: решения**.
4. На вкладке сведения выполните следующие действия.
    1. В поле Описание укажите следующие сведения для соответствующего типа ресурса.

        |  Ресурс  |    Родительские ресурсы      |    Запрошенные новые ограничения     |    Причина увеличения квоты       |
        |----------------|------------------------------|---------------------------------|------------------------------------------|
        |  Учетная запись |  *Идентификатор подписки*   |  *Запрошен новый максимальный номер **учетной записи***    |  *Какой сценарий или вариант использования запрашивает запрос?*  |
        |  пул    |  *Идентификатор подписки, URI учетной записи NetApp*  |  *Запрошен новый максимальный номер **пула***   |  *Какой сценарий или вариант использования запрашивает запрос?*  |
        |  Том  |  *Идентификатор подписки, URI учетной записи NetApp, URI пула ресурсов*   |  *Запрошен новый максимальный номер **тома***     |  *Какой сценарий или вариант использования запрашивает запрос?*  |
        |  MAXSIZE  |  *Идентификатор подписки, универсальный код ресурса (URI) учетной записи NetApp, URI пула ресурсов, URI тома*   |  *Запрошено новое максимальное число **MAXSIZE***     |  *Какой сценарий или вариант использования запрашивает запрос?*  |    
        |  Тома защиты данных репликации между регионами  |  *Идентификатор подписки, URI конечной учетной записи NetApp, URI целевого пула емкости, URI исходной учетной записи NetApp, URI исходного пула емкости, URI исходного тома*   |  *Запрошено новое максимальное число **томов защиты данных репликации между регионами (конечные тома)***     |  *Какой сценарий или вариант использования запрашивает запрос?*  |    

    2. Укажите соответствующий метод поддержки и укажите сведения о контракте.

    3. Чтобы создать запрос, нажмите кнопку **Далее: Проверка + создать** . 


## <a name="next-steps"></a>Дальнейшие действия  

- [Общие сведения об иерархии хранилища Azure NetApp Files](azure-netapp-files-understand-storage-hierarchy.md)
- [Модель затрат для Azure NetApp Files](azure-netapp-files-cost-model.md)
