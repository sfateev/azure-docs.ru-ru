---
title: Управление сбором данных инвентаризации с виртуальных машин в службе автоматизации Azure | Документация Майкрософт
description: В этой статье описывается, как управлять сбором данных инвентаризации с виртуальных машин.
services: automation
ms.subservice: change-inventory-management
keywords: данные инвентаризации, автоматизация, отслеживание
ms.date: 06/30/2020
ms.topic: conceptual
ms.openlocfilehash: 32d3c17a5f3d152f32b19ffbfd5c9793a7a34b80
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2020
ms.locfileid: "86185727"
---
# <a name="manage-inventory-collection-from-vms"></a>Управление сбором данных инвентаризации из виртуальных машин

Отслеживание инвентаризации для виртуальной машины Azure можно включить на странице ресурсов виртуальной машины. Вы можете собирать и просматривать следующие данные инвентаризации по вашим компьютерам:

- обновления Windows, приложения Windows, службы, файлы и разделы реестра;
- пакеты ПО Linux, управляющие программы и файлы.

Отслеживание изменений и инвентаризация в службе автоматизации Azure предоставляет браузерный пользовательский интерфейс для установки и настройки сбора данных инвентаризации.

## <a name="before-you-begin"></a>Перед началом

Если у вас еще нет подписки Azure, [создайте бесплатную учетную запись](https://azure.microsoft.com/free/).

В этой статье предполагается, что у вас уже есть виртуальная машина, для которой можно включить Отслеживание изменений и инвентаризацию. Если у вас нет виртуальной машины Azure, вы можете [создать ее](../virtual-machines/windows/quick-create-portal.md).

## <a name="sign-in-to-the-azure-portal"></a>Вход на портал Azure

Войдите на [портал Azure](https://portal.azure.com/).

## <a name="enable-inventory-collection-from-the-vm-resource-page"></a>Включение сбора данных инвентаризации на странице ресурсов виртуальной машины

1. В левой области на портале Azure выберите **Виртуальные машины**.
2. Выберите в списке нужную виртуальную машину.
3. В меню **Ресурс** в разделе **Операции** выберите **Инвентаризация**.
4. Выберите рабочую область Log Analytics для хранения журналов данных.
    Если рабочая область для этого региона недоступна, вам будет предложено создать рабочую область по умолчанию и учетную запись службы автоматизации.
5. Чтобы включить компьютер, щелкните **Включить**.

   ![Просмотр параметров подключения](./media/automation-vm-inventory/inventory-onboarding-options.png)

    Строка состояния проинформирует о том, что включается Отслеживание изменений и инвентаризация. Этот процесс может занять до 15 минут. На этот период можно закрыть окно или же оставить его открытым, чтобы узнать о включении этой возможности. Состояние развертывания можно отслеживать на панели уведомлений.

   ![Просмотр данных инвентаризации](./media/automation-vm-inventory/inventory-onboarded.png)

После завершения развертывания строка состояния исчезнет. Система по-прежнему собирает данные инвентаризации, которые еще могут не отображаться. На сбор всех данных может потребоваться 24 часа.

## <a name="configure-your-inventory-settings"></a>Настройка параметров инвентаризации

По умолчанию для сбора настраиваются программное обеспечение, службы Windows и управляющие программы Linux. Для сбора данных реестра Windows и инвентаризации файлов настройте параметры сбора данных инвентаризации.

1. На странице "Инвентаризация" в верхней части страницы щелкните **Изменить параметры**.
2. Чтобы добавить новый параметр сбора, перейдите к вкладке с нужной категорией параметров: **Реестр Windows**, **Файлы Windows** или **Файлы Linux**.
3. Выберите подходящую категорию и щелкните **Добавить** в верхней части окна.

В следующих разделах приводятся сведения о каждом свойстве, которое можно настроить для разных категорий.

### <a name="windows-registry"></a>реестр Windows;

|Свойство  |Описание  |
|---------|---------|
|Активировано     | Определяет, применяется ли параметр        |
|Имя элемента     | Понятное имя файла для отслеживания        |
|Группа     | Имя группы для логического группирования файлов        |
|Раздел реестра Windows   | Путь для проверки наличия файла. Например: HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Explorer\User Shell Folders\Common Startup      |

### <a name="windows-files"></a>файлы Windows.

|Свойство  |Описание  |
|---------|---------|
|Активировано     | Значение true, если параметр применен, и false, если нет.        |
|Имя элемента     | Понятное имя отслеживаемого файла.        |
|Группа     | Имя группы для логического группирования файлов.       |
|Укажите путь     | Путь для проверки наличия файла. Например, **c:\temp\myfile.txt**.

### <a name="linux-files"></a>Файлы Linux

|Свойство  |Описание  |
|---------|---------|
|Активировано     | Значение true, если параметр применен, и false, если нет.        |
|Имя элемента     | Понятное имя отслеживаемого файла.        |
|Группа     | Имя группы для логического группирования файлов.        |
|Укажите путь     | Путь для проверки наличия файла, например **/etc/*.conf**.       |
|Тип пути     | Тип отслеживаемого элемента. Допускаются значения "Файл" и "Каталог".        |
|Рекурсия     | Значение true, если используется рекурсия при поиске отслеживаемого элемента, или false, если нет.        |
|Использование sudo     | Значение true, если используется sudo при проверке наличия элемента, или false, если нет.         |
|Ссылки     | Значение, которое определяет правила проверки символических ссылок при обходе каталогов. Возможны следующие значения: <br> "Игнорировать" — игнорирование символических ссылок без включения файлов и каталогов, на которые они указывают.<br>"Переходить" — переход по символическим ссылкам во время рекурсии и включение файлов и каталогов, на которые они указывают.<br>"Управлять" — переход по символическим ссылкам и возможность изменить способ обработки полученного содержимого.      |

## <a name="manage-machine-groups"></a>Управление группами компьютеров

Инвентаризация позволяет создавать и просматривать группы компьютеров журналах Azure Monitor. Группы компьютеров — это коллекции компьютеров, определенные запросом в журналах Azure Monitor.

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../includes/azure-monitor-log-analytics-rebrand.md)]

Чтобы просмотреть группы компьютеров, на странице "Инвентаризация" щелкните вкладку **Группы компьютеров**.

![Просмотр групп компьютеров на странице инвентаризации](./media/automation-vm-inventory/inventory-machine-groups.png)

Чтобы открыть страницу "Группы компьютеров", выберите группу из списка. На этой странице отображаются сведения о конкретной группе, Эти сведения включают Azure Monitor запрос журнала, который используется для определения группы. В нижней части приведен список всех компьютеров, которые включены в эту группу.

![Просмотр страницы группы компьютеров](./media/automation-vm-inventory/machine-group-page.png)

Чтобы клонировать группу компьютеров, щелкните **+ Клонировать**. Здесь нужно задать новое имя и псевдоним для группы. Также на этом этапе можно изменить определение. После этого щелкните **Проверить запрос**, чтобы просмотреть выбираемые компьютеры. Если вас все устраивает, нажмите кнопку **Создать**, чтобы создать группу компьютеров.

Чтобы создать новую группу компьютеров, щелкните **+ Создать группу компьютеров**. Откроется страница **Создание группы компьютеров**, на которой вы можете определить новую группу. Щелкните **Создать**, чтобы создать группу.

![Создание новой группы компьютеров](./media/automation-vm-inventory/create-new-group.png)

## <a name="disconnect-your-vm-from-management"></a>Отмена управления виртуальной машиной

Чтобы отменить управление виртуальной машиной Отслеживанием изменений и инвентаризацией, сделайте следующее.

1. В области слева на портале Azure щелкните **Log Analytics**, а затем выберите рабочую область, которую вы указали при подключении виртуальной машины к Отслеживанию изменений и инвентаризации.
2. На странице **log Analytics** откройте меню **ресурсов** .
3. Выберите **Виртуальные машины** в разделе **Источники данных рабочей области**.
4. В открывшемся списке выберите виртуальные машины, которые нужно отключить. Для этих виртуальных машин отобразится зеленая галочка рядом с меткой **Эта рабочая область** в столбце **Подключение OMS**.

   >[!NOTE]
   >Operations Management Suite (OMS) теперь переименован в журналы Azure Monitor.

5. В верхней части следующей страницы щелкните **Отключить**.
6. В окне подтверждения щелкните **Да**, чтобы отменить управление компьютером.

>[!NOTE]
>Компьютеры по-прежнему отображаются после отмены регистрации, так как мы получаем отчет о всех компьютерах, инвентаризованных за последние 24 часа. После отключения компьютера необходимо подождать 24 часа, прежде чем они перестанут отображаться в списке.

## <a name="next-steps"></a>Дальнейшие действия

* Дополнительные сведения о работе с этой возможностью см. в статье [Управление решением для Отслеживания изменений и инвентаризации](change-tracking-file-contents.md).
* Дополнительные сведения об отслеживании изменений в ПО см. в статье [Общие сведения об Отслеживании изменений и инвентаризации](./change-tracking.md).
* Сведения об устранении общих проблем в работе этой функции см. в статье [Устранение неполадок с Отслеживанием изменений и инвентаризацией](troubleshoot/change-tracking.md).
