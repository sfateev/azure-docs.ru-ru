---
title: Распространенные проблемы при сертификации образов виртуальных машин для Azure Marketplace
description: В этой статье описываются распространенные сообщения об ошибках и проблемы при тестировании и сертификации образов виртуальных машин для Azure Marketplace. Здесь также обсуждаются связанные решения.
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: troubleshooting
author: iqshahmicrosoft
ms.author: iqshah
ms.date: 10/14/2020
ms.openlocfilehash: 1a8dbbb42a548a8c4e9a1117166aa621e8734208
ms.sourcegitcommit: 2e72661f4853cd42bb4f0b2ded4271b22dc10a52
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/14/2020
ms.locfileid: "92044502"
---
# <a name="common-issues-when-certifying-virtual-machine-images-for-azure-marketplace"></a>Распространенные проблемы при сертификации образов виртуальных машин для Azure Marketplace

При публикации образа виртуальной машины в Azure Marketplace группа Azure проверяет ее, чтобы обеспечить ее загрузку, безопасность и совместимость с Azure. Если какое-либо из высококачественных тестов завершится сбоем, публикация завершится ошибкой, и появится сообщение об ошибке с описанием проблемы.

В этой статье описываются распространенные сообщения об ошибках во время публикации образов виртуальных машин, а также связанные решения.

> [!NOTE]
> Если у вас есть вопросы или отзывы для улучшения, обратитесь в [службу поддержки центра партнеров](https://partner.microsoft.com/support/v2/?stage=1).

## <a name="approved-base-image"></a>Утвержденный базовый образ

При отправке запроса на повторную публикацию образа с обновлениями тестовый случай проверки числа частей может завершиться ошибкой. В случае сбоя образ не будет утвержден.

Эта ошибка возникает, если используется базовый образ, принадлежащий другому издателю, и вы обновили образ. В этом случае вам запрещено публиковать образ.

Чтобы устранить эту проблему, извлеките образ из Azure Marketplace и внесите в него изменения. Дополнительные сведения см. в следующих статьях:

- [Образы Linux](../../virtual-machines/linux/endorsed-distros.md?toc=/azure/virtual-machines/linux/toc.json)
- [Образы Windows](create-azure-vm-technical-asset.md#create-a-vm-image-using-an-approved-base)

> [!Note]
> Если вы используете базовый образ Linux, который не берется из Marketplace, можно сместить первую секцию на 2048 КБ. Это позволяет использовать неформатированное пространство для добавления новых сведений о выставлении счетов и позволяет Azure выполнять публикацию виртуальной машины в Marketplace.  

## <a name="vm-extension-failure"></a>Сбой расширения виртуальной машины

Проверьте, поддерживает ли образ ВМ расширения.

Чтобы включить расширения виртуальной машины, выполните следующие действия.

1. Выберите виртуальную машину Linux.
1. Перейдите в раздел **параметры диагностики**.
1. Включите базовые матрицы, обновив **учетную запись хранения**.
1. Щелкните **Сохранить**.

   ![Включение мониторинга на гостевом уровне](./media/vm-certification-issues-solutions-1.png)

Чтобы убедиться, что расширения виртуальной машины активированы правильно, выполните следующие действия.

1. На виртуальной машине перейдите на вкладку **расширения ВМ** и проверьте состояние **расширения системы диагностики Linux**.
    * Если состояние — *Подготовка успешно*, тестовый случай был пройден.  
    * Если состояние — *Ошибка подготовки*, тестовый случай не удалось выполнить, и необходимо установить флаг зафиксированного состояния.

      ![Снимок экрана, показывающий, что подготовка выполнена успешно](./media/vm-certification-issues-solutions-2.png)

      Если расширение виртуальной машины завершается сбоем, см. раздел [Использование диагностического расширения Linux для мониторинга метрик и журналов](../../virtual-machines/extensions/diagnostics-linux.md) для включения. Если вы не хотите, чтобы расширение виртуальной машины было включено, обратитесь в службу поддержки и попросите его отключить.

## <a name="vm-provisioning-issue"></a>Ошибка подготовки виртуальной машины

Перед отправкой предложения убедитесь, что вы следите за процессом подготовки виртуальной машины. Чтобы просмотреть формат JSON для подготовки ВИРТУАЛЬНОЙ машины, см. статью [сертификация образа виртуальной машины Azure](azure-vm-image-certification.md).

Проблемы подготовки могут включать в себя следующие сценарии сбоя:

|Сценарий|Ошибка|Причина|Решение|
|---|---|---|---|
|1|Недопустимый виртуальный жесткий диск (VHD)|Если указанное значение файла cookie в нижнем колонтитуле VHD неверно, VHD будет считаться недопустимым.|Повторно создайте образ и отправьте запрос.|
|2|Недопустимый тип BLOB-объекта|Не удалось подготовить виртуальную машину, так как используемый блок является типом большого двоичного объекта, а не типом страницы.|Повторно создайте образ и отправьте запрос.|
|3|Время ожидания подготовки или неправильное обобщенное|Возникла ошибка при обходе обобщения виртуальной машины.|Повторно создайте образ с обобщением и отправьте запрос.|

> [!NOTE]
> Дополнительные сведения о обходе обобщения виртуальных машин см. в следующих статьях:
> - [Документация по Linux](create-azure-vm-technical-asset.md#generalize-the-image)
> - [Документация по Windows](../../virtual-machines/windows/capture-image-resource.md#generalize-the-windows-vm-using-sysprep)

## <a name="software-compliance-for-windows"></a>Соответствие программного обеспечения требованиям для Windows

Если ваш запрос на использование образа Windows отклонен из-за проблемы соответствия программного обеспечения, возможно, вы создали образ Windows с установленным экземпляром SQL Server, а не получили соответствующий базовый образ версии SQL из Azure Marketplace.

Не создавайте собственный образ Windows с установленным SQL Server. Вместо этого используйте утвержденные базовые образы SQL (Enterprise/Standard/Web) из Azure Marketplace.

Если вы пытаетесь установить Visual Studio или любой продукт, лицензированный по Office, обратитесь в службу поддержки для получения предварительного утверждения.

Дополнительные сведения о выборе утвержденной базы данных см. в статье [Создание технических ресурсов виртуальной машины Azure](create-azure-vm-technical-asset.md#create-a-vm-image-using-an-approved-base).

## <a name="tool-kit-test-case-execution-failed"></a>Сбой при выполнении тестового случая для набора инструментов 

Набор средств сертификации Майкрософт может помочь в выполнении тестовых случаев и проверке совместимости VHD или образа с средой Azure.

Загрузите [Microsoft Certification Toolkit](azure-vm-image-certification.md).

## <a name="linux-test-cases"></a>Тестовые случаи Linux

В следующей таблице перечислены тестовые случаи Linux, которые будут выполняться набором инструментов. Проверка теста указывается в описании.

|Сценарий|Тестовый случай|Описание|
|---|---|---|
|1|Журнал bash|Перед созданием образа виртуальной машины необходимо очистить файлы журнала bash.|
|2|Версия агента Linux|Необходимо установить агент Linux для Azure 2.2.41 или более поздней версии.|
|3|Обязательные параметры ядра|Проверяет, установлены ли следующие параметры ядра: <br>Console = ttyS0<br>еарлипринтк = ttyS0<br>рутделай = 300|
|4|Переключение раздела на диск ОС|Проверяет, что разделы подкачки не создаются на диске операционной системы.|
|5|Корневой раздел на диске ОС|Создайте один корневой раздел для диска ОС.|
|6|Версия OpenSSL|Версия OpenSSL должна быть версии 0.9.8 или более поздней.|
|7|Версия Python|Настоятельно рекомендуется использовать Python версии 2,6 или более поздней.|
|8|Интервал активности клиента|Задайте для ClientAliveInterval значение 180. В приложении необходимо установить значение от 30 до 235. Если вы включаете SSH для конечных пользователей, это значение должно быть задано в соответствии с описанием.|
|9|Архитектура ОС|Поддерживаются только 64-разрядные операционные системы.|
|10|Автоматическое обновление|Указывает, включено ли автоматическое обновление агента Linux.|

### <a name="common-errors-found-while-executing-previous-test-cases"></a>При выполнении предыдущих тестовых случаев обнаружены распространенные ошибки

В следующей таблице перечислены распространенные ошибки, обнаруженные при выполнении предыдущих тестовых случаев.
 
|Сценарий|Тестовый случай|Error|Решение|
|---|---|---|---|
|1|Тестовый случай версии агента Linux|Минимальная версия агента Linux — 2.2.41 или более поздняя. Это требование было обязательным с 1 мая 2020 г.|Обновите версию агента Linux, и она должна быть 2,241 или более поздней версии. Дополнительные сведения см. на [странице обновления версии агента Linux](https://support.microsoft.com/help/4049215/extensions-and-virtual-machine-agent-minimum-version-support).|
|2|Тестовый случай истории bash|Если размер журнала Bash в отправленном изображении превышает 1 килобайт (КБ), появится сообщение об ошибке. Размер ограничен 1 КБ, чтобы все потенциально конфиденциальные сведения не захватываются в файле журнала bash.|Чтобы устранить эту проблему, подключите виртуальный жесткий диск к любой другой рабочей виртуальной машине и внесите необходимые изменения (например, удалите файлы журнала *. Bash* ), чтобы уменьшить размер до 1 КБ или меньше.|
|3|Обязательный параметр ядра тестовый случай|Эта ошибка возникает, когда для параметра **Console** не задано значение **ttyS0**. Проверьте, выполнив следующую команду:<br>`cat /proc/cmdline`|Задайте для параметра **Console** значение **ttyS0**и повторите запрос.|
|4|Тестовый случай интервала Клиенталиве|Если в результате набора средств вы получите неудачный результат для этого тестового случая, для **ClientAliveInterval**будет указано неверное значение.|Задайте для параметра **ClientAliveInterval** значение меньше или равное 235, а затем повторно отправьте запрос.|

### <a name="windows-test-cases"></a>Тестовые случаи для Windows

В следующей таблице перечислены тестовые случаи Windows, которые будут запускаться с помощью набора средств, а также описание проверки теста.

|Сценарий |Проверочные варианты|Описание|
|---|---|---|---|
|1|Архитектура ОС|Azure поддерживает только 64-разрядные операционные системы.|
|2|Зависимость учетной записи пользователя|Выполнение приложения не должно зависеть от учетной записи администратора.|
|3|Отказоустойчивый кластер|Компонент отказоустойчивой кластеризации Windows Server пока не поддерживается. Приложение не должно зависеть от этой функции.|
|4|IPV6|Протокол IPv6 пока не поддерживается в среде Azure. Приложение не должно зависеть от этой функции.|
|5|DHCP|Роль сервера протокола динамической настройки узла пока не поддерживается. Приложение не должно зависеть от этой функции.|
|6|Hyper-V|Роль сервера Hyper-V пока не поддерживается. Приложение не должно зависеть от этой функции.|
|7|Удаленный доступ.|Роль сервера "удаленный доступ (прямой доступ)" пока не поддерживается. Приложение не должно зависеть от этой функции.|
|8|службы Rights Management Services|Службы Rights Management. Роль сервера пока не поддерживается. Приложение не должно зависеть от этой функции.|
|9|Windows Deployment Services|Службы развертывания Windows. Роль сервера пока не поддерживается. Приложение не должно зависеть от этой функции.|
|10|Шифрование диска BitLocker|Шифрование диска BitLocker не поддерживается на жестком диске операционной системы, но может использоваться на дисках данных.|
|11|Сервер имен интернет-хранилища|Компонент сервера службы "имя хранилища Интернета" пока не поддерживается. Приложение не должно зависеть от этой функции.|
|12|Multipath I/O;|Multipath I/O. Эта функция сервера пока не поддерживается. Приложение не должно зависеть от этой функции.|
|13|Network Load Balancing|Балансировка сетевой нагрузки. Эта функция сервера пока не поддерживается. Приложение не должно зависеть от этой функции.|
|14|Протокол PNRP|Протокол разрешения имен одноранговых узлов. Эта функция сервера пока не поддерживается. Приложение не должно зависеть от этой функции.|
|15|Службы SNMP|Компонент служб SNMP пока не поддерживается. Приложение не должно зависеть от этой функции.|
|16|Служба WINS|Служба Windows Internet Name Service. Эта функция сервера пока не поддерживается. Приложение не должно зависеть от этой функции.|
|17|Служба беспроводной локальной сети|Служба беспроводной локальной сети. Эта функция сервера пока не поддерживается. Приложение не должно зависеть от этой функции.|

Если вы пойдете за любые сбои с предыдущими тестовыми случаями, обратитесь к столбцу **Description** в таблице решения. Если вам нужны дополнительные сведения, обратитесь в службу поддержки. 

## <a name="data-disk-size-verification"></a>Проверка размера диска данных

Если размер любого запроса, отправляемого с диска данных, превышает 1023 гигабайт (ГБ), запрос не будет утвержден. Это правило применяется как к Linux, так и к Windows.

Повторно отправьте запрос, размер которого меньше или равен 1023 ГБ.

## <a name="os-disk-size-validation"></a>Проверка размера диска ОС

Сведения об ограничениях размера диска ОС см. в следующих правилах. При отправке любого запроса убедитесь, что размер диска ОС находится в пределах ограничений для Linux или Windows.

|OS|Рекомендуемый размер VHD|
|---|---|
|Linux|от 30 ГБ до 1023 ГБ|
|Windows|от 30 ГБ до 250 ГБ|

Так как виртуальные машины разрешают доступ к базовой операционной системе, убедитесь, что размер виртуального жесткого диска достаточно велик для виртуального жесткого диска. Так как диски не могут быть расширены без простоя, используйте размер диска от 30 ГБ до 50 ГБ.

|Размер виртуального жесткого диска|Фактический объем занятого объема|Решение|
|---|---|---|
|>500 тебибитес (тиб)|Недоступно|Обратитесь в службу поддержки за исключением утверждения.|
|250-500 тиб|Разница >200 гибибайтах (гиб) от размера BLOB-объекта|Обратитесь в службу поддержки за исключением утверждения.|

> [!NOTE]
> Увеличение размера диска приводит к более высокой стоимости, и в процессе установки и репликации будет вызываться задержка. Из-за этой задержки и стоимости Группа поддержки может найти обоснование для утверждения исключения.

## <a name="wannacry-patch-verification-test-for-windows"></a>Проверка исправления WannaCry для Windows

Чтобы предотвратить потенциальную атаку, связанную с вирусом WannaCry, убедитесь, что все запросы образа Windows обновлены с использованием последнего исправления.

Чтобы просмотреть исправленную версию Windows Server для сведений о ОС и минимальную версию, которая будет поддерживаться, ознакомьтесь со следующей таблицей: 

Версию файла изображения можно проверить с `C:\windows\system32\drivers\srv.sys` или `srv2.sys` .

> [!NOTE]
> Windows Server 2019 не имеет обязательных требований к версии.

|Операционная система|Version|
|---|---|
|Windows обслуживает 2008 R2|6.1.7601.23689|
|Windows Server 2012|6.2.9200.22099|
|Windows Server 2012 R2|6.3.9600.18604|
|Windows Server 2016|10.0.14393.953|
|Windows Server 2019|Н/Д|

## <a name="sack-vulnerability-patch-verification"></a>Проверка исправления с уязвимостью SACK

При отправке образа Linux запрос может быть отклонен из-за проблем с версиями ядра.

Обновите ядро, используя утвержденную версию, и отправьте запрос повторно. Утвержденную версию ядра можно найти в следующей таблице. Номер версии должен быть больше или равен числу, указанному здесь.

Если образ не установлен с одной из следующих версий ядра, обновите его с помощью правильных исправлений. Запросите необходимое утверждение у группы поддержки после обновления образа с помощью этих необходимых исправлений:

- CVE-2019-11477 
- CVE-2019-11478 
- CVE-2019-11479

|Семейство ОС|Версия|Ядро|
|---|---|---|
|Ubuntu|14.04 LTS|4.4.0 — 151| 
||14.04 LTS|4.15.0-1049-* — Azure|
||16.04 LTS|4.15.0 — 1049|
||18.04 LTS|4.18.0-1023|
||18.04 LTS|5.0.0-1025|
||18.10 |4.18.0-1023|
||19.04 |5.0.0-1010|
||19.04 |5.3.0-1004|
|ОС RHEL и цент|6.10|2.6.32 — 754.15.3|
||7.2|3.10.0 — 327.79.2|
||7.3|3.10.0 — 514.66.2|
||7.4|3.10.0 — 693.50.3|
||7.5|3.10.0 — 862.34.2|
||7.6|3.10.0 — 957.21.3|
||7.7|3.10.0 — 1062.1.1|
||8.0|4.18.0 — 80.4.2|
||8.1|4.18.0-147|
||"7-RAW" (7,6)||
||"7-LVM" (7,6)|3.10.0 — 957.21.3|
||RHEL — SAP 7,4|TBD|
||RHEL — SAP 7,5|TBD|
|SLES|SLES11SP4 (включая SAP)|3.0.101 — 108.95.2|
||SLES12SP1 для SAP|3.12.74 — 60.64.115.1|
||SLES12SP2 для SAP|4.4.121 — 92.114.1|
||SLES12SP3|4.4180-4.31.1 (ядро — Azure)|
||SLES12SP3 для SAP|4.4.180 — 94.97.1|
||SLES12SP4|4.12.14-6.15.2 (ядро — Azure)|
||SLES12SP4 для SAP|4.12.14 — 95.19.1|
||SLES15|4.12.14-5.30.1 (ядро — Azure)|
||SLES15 для SAP|4.12.14-5.30.1 (ядро — Azure)|
||SLES15SP1|4.12.14-5.30.1 (ядро — Azure)|
|Oracle;|6.10|UEK2 2.6.39 — 400.312.2<br>UEK3 3.8.13 — 118.35.2<br>RHCK 2.6.32 — 754.15.3 
||7.0 — 7,5|UEK3 3.8.13 — 118.35.2<br>UEK4 4.1.12 — 124.28.3<br>RHCK соответствует RHEL выше|
||7.6|RHCK 3.10.0 — 957.21.3<br>UEK5 4.14.35 — 1902.2.0|
|CoreOS стабильный 2079.6.0|4.19.43 *|
||Бета-версия 2135.3.1|4.19.50 *|
||Альфа-2163.2.1|4.19.50 *|
|Debian|Jessie (безопасность)|3.16.68-2|
||неjessieные порты|4.9.168-1 + deb9u3|
||Stretch (безопасность)|4.9.168-1 + deb9u3|
||Debian GNU/Linux 10 (бустер)|Debian 6.3.0-18 + deb9u1|
||бустер, ИД безопасности (непереносимые порты Stretch)|4.19.37-5|

## <a name="image-size-should-be-in-multiples-of-megabytes"></a>Размер изображения должен быть кратен мегабайтам

Виртуальным жестким дискам в Azure должен быть назначен виртуальный размер, кратный одному мегабайту (МБ). Если виртуальный жесткий диск не соответствует рекомендуемому виртуальному размеру, запрос может быть отклонен.

Следуйте рекомендациям при преобразовании с необработанного диска на виртуальный жесткий диск и убедитесь, что размер необработанного диска равен 1 МБ. Дополнительные сведения см. в разделе сведения о нерекомендуемых [дистрибутивах](../../virtual-machines/linux/create-upload-generic.md).

## <a name="vm-access-denied"></a>Доступ к виртуальной машине запрещен

Если при выполнении тестовых случаев на виртуальной машине возникли проблемы с отказом в доступе, это может быть вызвано недостаточными привилегиями для выполнения тестовых случаев.

Проверьте, включен ли правильный доступ для учетной записи, в которой выполняются самотестовые случаи. Если доступ не включен, включите его для выполнения тестовых случаев. Если вы не хотите разрешать доступ, вы можете поделиться результатами самотестирования с группой поддержки.

## <a name="download-failure"></a>Сбой скачивания
    
Сведения о проблемах, возникающих при скачивании образа виртуальной машины с помощью URL-адреса SAS см. в следующей таблице.

|Сценарий|Ошибка|Причина|Решение|
|---|---|---|---|
|1|Не удалось найти большой двоичный объект.|Виртуальный жесткий диск может быть удален или перемещен из указанного расположения.|| 
|2|Используемый BLOB-объект|Виртуальный жесткий диск используется другим внутренним процессом.|Виртуальный жесткий диск должен быть в используемом состоянии при скачивании с использованием URL-адреса SAS.|
|3|Недопустимый URL-адрес SAS|Неправильный URL-адрес SAS для виртуального жесткого диска.|Получите правильный URL-адрес SAS.|
|4|Недопустимая подпись|Неправильный URL-адрес SAS для виртуального жесткого диска.|Получите правильный URL-адрес SAS.|
|6|Условный заголовок HTTP|Недопустимый URL-адрес SAS.|Получите правильный URL-адрес SAS.|
|7|Недопустимое имя виртуального жесткого диска|Проверьте, нет ли специальных символов, например знака процента (%) или кавычки ("), существуют в имени виртуального жесткого диска.|Переименуйте VHD-файл, удалив специальные символы.|

## <a name="first-mb-2048-kb-partition-only-for-linux"></a>Раздел "первые МБ (2048 КБ)" (только для Linux)

При отправке виртуального жесткого диска убедитесь, что первая 2048 КБ виртуального жесткого диска пуста. В противном случае запрос завершится ошибкой *.

>[!NOTE]
>* Для некоторых специальных образов, например, созданных на основе базовых образов Azure, полученных из Azure Marketplace, мы проверяем тег выставления счетов и проигнорируйте секцию MB, если тег выставления счетов существует и соответствует нашим внутренним доступным значениям.

## <a name="default-credentials"></a>Учетные данные по умолчанию

Всегда обеспечьте, чтобы учетные данные по умолчанию не отправлялись с помощью отправленного виртуального жесткого диска. Добавление учетных данных по умолчанию делает виртуальный жесткий диск более уязвимым для угроз безопасности. Вместо этого создайте собственные учетные данные при отправке виртуального жесткого диска.
  
## <a name="datadisk-mapped-incorrectly"></a>Неверно сопоставленный диск с подсистемами

Если запрос отправляется с несколькими дисками данных, но их порядок не является последовательным, это считается проблемой сопоставления. Например, если имеется три диска данных, порядок нумерации должен быть *равен 0, 1, 2*. Любой другой порядок рассматривается как вопрос о сопоставлении.

Повторно отправьте запрос с соответствующей последовательностью дисков данных.

## <a name="incorrect-os-mapping"></a>Неправильное сопоставление ОС

Созданное изображение может быть сопоставлено с неправильной меткой ОС или назначено ей. Например, при выборе **Windows** в качестве части имени ОС во время создания образа диск операционной системы должен быть установлен только с Windows. Те же требования применимы и к Linux.

## <a name="vm-not-generalized"></a>Виртуальная машина не обобщена

Если все образы, взятые из Azure Marketplace, необходимо использовать повторно, VHD операционной системы должен быть обобщенным.

* Для **Linux**следующий процесс ОБобщает виртуальную машину Linux и повторно развертывает ее как ОТДЕЛЬную виртуальную машину.

  В окне SSH введите следующую команду: `sudo waagent -deprovision+user`

* Для **Windows**вы обобщаете образы Windows с помощью `sysreptool` .

Дополнительные сведения об этом средстве см. в разделе [Обзор подготовки системы (Sysprep)]( https://docs.microsoft.com/windows-hardware/manufacture/desktop/sysprep--system-preparation--overview).

## <a name="datadisk-errors"></a>Ошибки на диске

Для устранения ошибок, связанных с диском данных, используйте следующую таблицу:

|Ошибка|Причина|Решение|
|---|---|---|
|`DataDisk- InvalidUrl:`|Эта ошибка может возникать из-за недопустимого числа, указанного для логического номера устройства (LUN) при отправке предложения.|Убедитесь, что последовательность номеров LUN для диска данных находится в центре партнеров.|
|`DataDisk- NotFound:`|Эта ошибка может возникать, если диск данных не находится по указанному URL-адресу SAS.|Убедитесь, что диск данных расположен по URL-адресу SAS, указанному в запросе.|

## <a name="remote-access-issue"></a>Проблемы с удаленным доступом

Если параметр протокол удаленного рабочего стола (RDP) не включен для образа Windows, вы получите эту ошибку. 

Включите доступ по протоколу RDP для образов Windows, прежде чем отправлять их.

## <a name="bash-history-failed"></a>Сбой журнала bash

Эта ошибка появится, если размер журнала Bash в отправленном изображении превышает 1 КБ. Размер ограничен 1 КБ, чтобы все потенциально конфиденциальные сведения не захватываются в файле журнала bash.

Ниже приведены инструкции по удалению "журнала bash".

Шаг 1. Разверните виртуальную машину и выберите команду "выполнить команду" на портал Azure.
![Выполнить команду на портал Azure](./media/vm-certification-issues-solutions-3.png)

Шаг 2. Выберите первый вариант "Руншеллскрипт" и выполните приведенную ниже команду.

Команда: "Cat/Дев/Нулл > ~/.bash_history && History-c" ![ команда журнала bash на портал Azure](./media/vm-certification-issues-solutions-4.png)

Шаг 3. После успешного выполнения команды перезапустите виртуальную машину.

Шаг 4. Подготовьте виртуальную машину к использованию образа VHD и завершите работу виртуальной машины.

Шаг 5.     Re-Submit обобщенного образа.

## <a name="requesting-exceptions-custom-templates-on-vm-images-for-selective-tests"></a>Запрос исключений (пользовательские шаблоны) в образах виртуальных машин для выборочных тестов

Издатели могут обращаться к исключениям для выполнения нескольких тестов во время сертификации виртуальной машины. Исключения предоставляются в чрезвычайно редких случаях, когда издатель предоставляет свидетельство для поддержки запроса.
Группа сертификации оставляет за собой право отклонять или утверждать исключения в любой момент времени.

В следующих разделах мы поговорим о главных сценариях, где требуются исключения и как запросить исключение.

Сценарии для исключения

Существуют три сценария или случая, когда издатели обычно запрашивают эти исключения. 

* **Исключение для одного или нескольких тестовых случаев:** Издатели могут обратиться к [службе поддержки издателей Marketplace](https://aka.ms/marketplacepublishersupport) за исключениями запросов для тестовых случаев. 

* **Заблокированные виртуальные машины/нет корневого доступа:** У нескольких издателей есть сценарии, в которых виртуальные машины должны быть заблокированы, так как на виртуальной машине установлены такие программы, как брандмауэры. 
       В этом случае издатели могут загрузить [сертифицированное средство тестирования](https://aka.ms/AzureCertificationTestTool) и предоставить отчет в [службе поддержки издателей Marketplace](https://aka.ms/marketplacepublishersupport) .


* **Пользовательские шаблоны:** Некоторые издатели публикуют образы виртуальных машин, для которых требуется пользовательский шаблон ARM для развертывания виртуальных машин. В этом случае издатели запрашивают предоставление пользовательских шаблонов в [службе поддержки издателя Marketplace](https://aka.ms/marketplacepublishersupport) , чтобы группа сертификации могла использовать их для проверки. 

### <a name="information-to-provide-for-exception-scenarios"></a>Сведения, предоставляемые для сценариев исключений

Издатели должны обратиться в службу поддержки [издателя Marketplace](https://aka.ms/marketplacepublishersupport) , чтобы запросить исключения для приведенного выше сценария с дополнительными сведениями:

   1.   Идентификатор издателя — идентификатор издателя на портале центра партнеров.
   2.   ИД предложения/имя — идентификатор или имя предложения, для которого запрашивается исключение. 
   3.   Номер SKU или плана — идентификатор плана или номер SKU предложения виртуальной машины, для которого запрашивается исключение.
   4.    Версия — версия предложения виртуальной машины, для которой запрашивается исключение.
   5.   Тип исключения — тесты, заблокированная виртуальная машина, пользовательские шаблоны
   6.   Причина запроса — причина этого исключения и сведения о тестах, которые необходимо исключить. 
   7. Временная шкала — Дата, до которой было запрошено это исключение 
   8.   Вложение. Прикрепите все документы с доказательствами важности. Для заблокированных виртуальных машин присоедините тестовый отчет и пользовательские шаблоны и предоставьте пользовательский шаблон ARM в качестве вложения. Сбой при присоединении отчета для заблокированных виртуальных машин и пользовательского шаблона ARM для пользовательских шаблонов приведет к отказу в запросе

## <a name="how-to-address-a-vulnerability-or-exploit-in-a-vm-offer"></a>Как устранить уязвимость или воспользоваться уязвимостью в предложении виртуальной машины

Эта часто задаваемые вопросы помогут вам предоставить образ виртуальной машины при обнаружении уязвимости или атаки с помощью одного из образов виртуальных машин. Это часто задаваемые вопросы применимы только к предложениям виртуальных машин Azure, опубликованным в Azure Marketplace.

> [!NOTE]
> Вы не можете удалить последний образ виртуальной машины из плана, и вы не сможете продать последний план предложения.

Выполните одно из следующих действий.

1. Если у вас есть новый образ ВИРТУАЛЬНОЙ машины для замены уязвимого образа виртуальной машины, перейдите к разделу [как предоставить фиксированный образ виртуальной](#how-to-provide-a-fixed-vm-image)машины.
1. Если у вас нет нового образа виртуальной машины для замены единственного образа виртуальной машины в плане и если вы закончили работу с планом, вы можете приступать [к продаже этого плана](update-existing-offer.md#stop-selling-an-offer-or-plan).
1. Если вы не планируете замену единственного образа виртуальной машины в предложении, мы рекомендуем вам [отменить продажи предложения](update-existing-offer.md#stop-selling-an-offer-or-plan).

### <a name="how-to-provide-a-fixed-vm-image"></a>Как предоставить фиксированный образ виртуальной машины

Чтобы предоставить фиксированный образ виртуальной машины для замены образа виртуальной машины с уязвимостью или эксплойтом, необходимо выполнить следующие действия.

1. Предоставьте новый образ виртуальной машины для устранения уязвимости или использования уязвимости системы безопасности.
1. Удалите образ виртуальной машины с уязвимостью или уязвимостью системы безопасности.
1. Повторно опубликуйте предложение.

#### <a name="provide-a-new-vm-image-to-address-the-security-vulnerability-or-exploit"></a>Предоставление нового образа виртуальной машины для устранения уязвимости или эксплойта системы безопасности

Чтобы выполнить эти действия, необходимо подготовить технический ресурс для добавляемого образа виртуальной машины. Дополнительные сведения см. в статье [Создание технических ресурсов для виртуальной машины Azure Marketplace](create-azure-vm-technical-asset.md) и [Получение URI SAS для образа виртуальной машины](get-sas-uri.md).

1. Войдите в [Центр партнеров](https://partner.microsoft.com/dashboard/home).
1. В меню навигации слева выберите пункт Обзор **коммерческого рынка**  >  **Overview**.
1. В столбце **псевдоним предложения** выберите предложение.
1. На вкладке **Обзор плана** в столбце **имя** выберите план, в который требуется добавить виртуальную машину.
1. На вкладке **Техническая конфигурация** в разделе **образы виртуальных машин**выберите **+ Добавить образ виртуальной машины**.
   > [!NOTE]
   > В каждый момент времени в план можно добавить только один образ виртуальной машины. Чтобы добавить несколько образов виртуальных машин, опубликуйте первый из них и дождитесь достижения этапа _утверждения издателя_ перед добавлением следующего образа виртуальной машины.
1. В появившихся полях укажите новую версию диска и образ виртуальной машины.
1. Выберите элемент **Сохранить черновик**.
1. Перейдите к следующему разделу, чтобы удалить образ виртуальной машины с уязвимостью системы безопасности.

#### <a name="remove-the-vm-image-that-has-the-security-vulnerability-or-exploit"></a>Удаление образа виртуальной машины с уязвимостью или эксплойтом системы безопасности

Войдите в [Центр партнеров](https://partner.microsoft.com/dashboard/home).
1. В меню навигации слева выберите пункт Обзор **коммерческого рынка**  >  **Overview**.
1. В столбце **псевдоним предложения** выберите предложение.
1. На вкладке **Обзор плана** в столбце **имя** выберите план с виртуальной машиной, которую необходимо удалить.
1. На вкладке **Техническая конфигурация** в разделе **образы виртуальных**машин рядом с изображением виртуальной машины, которое необходимо удалить, выберите **Удалить образ виртуальной машины**.
1. В открывшемся диалоговом окне выберите **продолжить**.
1. Выберите элемент **Сохранить черновик**.
1. Перейдите к следующему разделу, чтобы повторно опубликовать предложение.

#### <a name="republish-the-offer"></a>Повторная публикация предложения

После удаления или замены образа виртуальной машины необходимо повторно опубликовать предложение.
1. Выберите **Проверка и публикация**.
1. Если необходимо предоставить сведения группе сертификации, добавьте ее в поле **Примечания для сертификации** .
1. Нажмите **Публиковать**.

Дополнительные сведения о процессе публикации см. в статье [как просмотреть и опубликовать предложение в коммерческом магазине](../review-publish-offer.md).

## <a name="next-steps"></a>Дальнейшие действия

Если у вас есть вопросы или отзывы для улучшения, обратитесь в [службу поддержки центра партнеров](https://partner.microsoft.com/support/v2/?stage=1).
