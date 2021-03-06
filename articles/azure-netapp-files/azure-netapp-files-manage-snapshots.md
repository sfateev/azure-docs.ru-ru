---
title: Управление моментальными снимками с помощью Azure NetApp Files | Документы Майкрософт
description: Описывает создание моментальных снимков и управление ими с помощью Azure NetApp Files.
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
ms.topic: how-to
ms.date: 09/04/2020
ms.author: b-juche
ms.openlocfilehash: e9f2a1f9125d25caa9506e954cab3b94dfcb5c24
ms.sourcegitcommit: 50802bffd56155f3b01bfb4ed009b70045131750
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2020
ms.locfileid: "91932283"
---
# <a name="manage-snapshots-by-using-azure-netapp-files"></a>Управление моментальными снимками с помощью Azure NetApp Files

Azure NetApp Files поддерживает создание моментальных снимков по запросу и использование политик моментальных снимков для планирования автоматического создания моментальных снимков.  Можно также восстановить моментальный снимок в новый том или восстановить отдельный файл с помощью клиента.  

## <a name="create-an-on-demand-snapshot-for-a-volume"></a>Создание моментального снимка по запросу для тома

Моментальные снимки томов можно создавать по запросу. 

1.  Перейдите к тому, для которого нужно создать моментальный снимок. Щелкните **моментальные снимки**.

    ![Переход к моментальным снимкам](../media/azure-netapp-files/azure-netapp-files-navigate-to-snapshots.png)

2.  Чтобы создать моментальный снимок по запросу для тома, щелкните **+ Добавить моментальный снимок**.

    ![Добавление моментального снимка](../media/azure-netapp-files/azure-netapp-files-add-snapshot.png)

3.  В окне "Создание моментального снимка" введите имя создаваемого моментального снимка.   

    ![Создание моментального снимка.](../media/azure-netapp-files/azure-netapp-files-new-snapshot.png)

4. Нажмите кнопку **ОК**. 

## <a name="manage-snapshot-policies"></a>Управление политиками моментальных снимков

Вы можете запланировать автоматическое создание моментальных снимков томов с помощью политик моментальных снимков. Кроме того, можно изменить политику моментальных снимков или удалить политику моментальных снимков, которая больше не нужна.  

### <a name="register-the-feature"></a>регистрация компонента

Функция **политики моментальных снимков** сейчас доступна в предварительной версии. Если эта функция используется впервые, необходимо сначала зарегистрировать эту функцию. 

1. Зарегистрируйте функцию: 

    ```azurepowershell-interactive
    Register-AzProviderFeature -ProviderNamespace Microsoft.NetApp -FeatureName ANFSnapshotPolicy
    ```

2. Проверьте состояние регистрации компонента: 

    > [!NOTE]
    > **RegistrationState** может находиться в состоянии до `Registering` 60 минут до перехода на `Registered` . Прежде чем продолжить, подождите, пока состояние не будет **зарегистрировано** .

    ```azurepowershell-interactive
    Get-AzProviderFeature -ProviderNamespace Microsoft.NetApp -FeatureName ANFSnapshotPolicy
    ```
Вы также можете использовать [Azure CLI команды](/cli/azure/feature?preserve-view=true&view=azure-cli-latest) `az feature register` , `az feature show` чтобы зарегистрировать эту функцию и отобразить состояние регистрации. 

### <a name="create-a-snapshot-policy"></a>Создание политики моментальных снимков 

Политика моментальных снимков позволяет указать частоту создания моментального снимка в ежечасном, ежедневном, еженедельном или ежемесячном цикле. Также необходимо указать максимальное число моментальных снимков, которые будут храниться в томе.  

1.  В представлении учетной записи NetApp щелкните **Политика моментальных снимков**.

    ![Навигация по политике моментальных снимков](../media/azure-netapp-files/snapshot-policy-navigation.png)

2.  В окне Политика моментальных снимков задайте для параметра Состояние политики значение **включено**. 

3.  Щелкните вкладку **ежечасно**, **ежедневно**, **еженедельно**или **ежемесячно** , чтобы создать почасовую, ежедневную, еженедельную или ежемесячную политику моментальных снимков. Укажите **число хранимых моментальных снимков**.  

    См. раздел [ограничения ресурсов для Azure NetApp Files](azure-netapp-files-resource-limits.md) о максимальном количестве моментальных снимков, разрешенных для тома. 

    В следующем примере показана конфигурация политики ежечасных моментальных снимков. 

    ![Политика моментальных снимков — ежечасно](../media/azure-netapp-files/snapshot-policy-hourly.png)

    В следующем примере показана ежедневная Настройка политики моментальных снимков.

    ![Ежедневная политика моментальных снимков](../media/azure-netapp-files/snapshot-policy-daily.png)

    В следующем примере показана еженедельная Настройка политики моментальных снимков.

    ![Политика моментальных снимков еженедельно](../media/azure-netapp-files/snapshot-policy-weekly.png)

    В следующем примере показана ежемесячная Настройка политики моментальных снимков.

    ![Политика моментальных снимков ежемесячно](../media/azure-netapp-files/snapshot-policy-monthly.png) 

4.  Выберите команду **Сохранить**.  

Если необходимо создать дополнительные политики моментальных снимков, повторите шаг 3.
Созданные политики отображаются на странице Политика моментальных снимков.

Если требуется, чтобы том использовал политику моментальных снимков, необходимо [Применить политику к тому](azure-netapp-files-manage-snapshots.md#apply-a-snapshot-policy-to-a-volume). 

### <a name="apply-a-snapshot-policy-to-a-volume"></a>Применение политики моментальных снимков к тому

Если вы хотите, чтобы том использовал созданную политику моментальных снимков, необходимо применить политику к тому. 

1.  Перейдите на страницу **тома** , щелкните правой кнопкой мыши том, к которому требуется применить политику моментальных снимков, и выберите пункт **изменить**.

    ![Меню "тома" щелкните правой кнопкой мыши](../media/azure-netapp-files/volume-right-cick-menu.png) 

2.  В окне редактирования в разделе **Политика моментальных снимков**выберите политику, которая будет использоваться для тома.  Нажмите кнопку **ОК** , чтобы применить политику.  

    ![Изменение политики моментальных снимков](../media/azure-netapp-files/snapshot-policy-edit.png) 

### <a name="modify-a-snapshot-policy"></a>Изменение политики моментальных снимков 

Можно изменить существующую политику моментальных снимков, чтобы изменить состояние политики, периодичность моментальных снимков (ежечасно, ежедневно, еженедельно или ежемесячно) или число моментальных снимков.  
 
1.  В представлении учетной записи NetApp щелкните **Политика моментальных снимков**.

2.  Щелкните правой кнопкой мыши политику моментальных снимков, которую необходимо изменить, и выберите **изменить**.

    ![Меню "политика моментальных снимков" щелкните правой кнопкой мыши](../media/azure-netapp-files/snapshot-policy-right-click-menu.png) 

3.  Внесите изменения в появившемся окне Политика моментальных снимков, а затем нажмите кнопку **сохранить**. 

### <a name="delete-a-snapshot-policy"></a>Удаление политики моментальных снимков 

Вы можете удалить политику моментальных снимков, которую больше не нужно поддерживать.   

1.  В представлении учетной записи NetApp щелкните **Политика моментальных снимков**.

2.  Щелкните правой кнопкой мыши политику моментальных снимков, которую необходимо изменить, а затем выберите **Удалить**.

    ![Меню "политика моментальных снимков" щелкните правой кнопкой мыши](../media/azure-netapp-files/snapshot-policy-right-click-menu.png) 

3.  Нажмите кнопку **Да** , чтобы подтвердить удаление политики моментальных снимков.   

    ![Подтверждение удаления политики моментальных снимков](../media/azure-netapp-files/snapshot-policy-delete-confirm.png) 

## <a name="restore-a-snapshot-to-a-new-volume"></a>Восстановление моментального снимка в новый том

Сейчас моментальный снимок можно восстановить только в новый том. 
1. В колонке "том" выберите " **моментальные снимки** ", чтобы отобразить список моментальных снимков. 
2. Щелкните правой кнопкой мыши моментальный снимок, который необходимо восстановить, и выберите пункт **восстановить в новый том** в меню.  

    ![Восстановление моментального снимка в новый том](../media/azure-netapp-files/azure-netapp-files-snapshot-restore-to-new-volume.png)

3. В окне Создание тома введите сведения для нового тома:  
    * **Name**   
        Укажите имя создаваемого тома.  
        
        Имя должно быть уникальным в пределах группы ресурсов. Длина имени должна быть не менее трех знаков.  Имя может состоять из любых алфавитно-цифровых символов.

    * **Квота**  
        Укажите объем логического хранилища, который необходимо выделить для тома.  

    ![Восстановить в новый том](../media/azure-netapp-files/snapshot-restore-new-volume.png) 

4. Щелкните **проверить и создать**.  Нажмите кнопку **Создать**.   
    Новый том использует тот же протокол, который используется моментальным снимком.   
    Новый том, на котором восстанавливается моментальный снимок, отображается в колонке "Тома".

## <a name="restore-a-file-from-a-snapshot-using-a-client"></a>Восстановление файла из моментального снимка с помощью клиента

Если вы не хотите [восстанавливать весь моментальный снимок на том](#restore-a-snapshot-to-a-new-volume), можно восстановить файл из моментального снимка с помощью клиента, подключенного к тому.  

Подключенный том содержит каталог моментальных снимков с именем  `.snapshot` (в NFS-клиентах) или `~snapshot` (в клиентах SMB), доступный клиенту. Каталог моментальных снимков содержит подкаталоги, соответствующие моментальным снимкам тома. Каждый подкаталог содержит файлы моментального снимка. Если вы случайно удалили или перезаписали файл, можно восстановить его в родительском каталоге, доступном для чтения и записи, скопировав файл из подкаталога моментальных снимков в каталог, доступный для чтения и записи. 

Если при создании тома был установлен флажок Скрыть путь моментального снимка, то каталог моментальных снимков будет скрыт. Чтобы просмотреть состояние пути скрытия моментального снимка тома, выберите том. Можно изменить параметр Скрыть путь к моментальному снимку, нажав кнопку **изменить** на странице тома.  

![Изменить параметры моментального снимка тома](../media/azure-netapp-files/volume-edit-snapshot-options.png) 

### <a name="restore-a-file-by-using-a-linux-nfs-client"></a>Восстановление файла с помощью клиента NFS Linux 

1. Используйте `ls` команду Linux, чтобы получить список файлов, которые требуется восстановить из `.snapshot` каталога. 

    Пример:

    `$ ls my.txt`   
    `ls: my.txt: No such file or directory`   

    `$ ls .snapshot`   
    `daily.2020-05-14_0013/              hourly.2020-05-15_1106/`   
    `daily.2020-05-15_0012/              hourly.2020-05-15_1206/`   
    `hourly.2020-05-15_1006/             hourly.2020-05-15_1306/`   

    `$ ls .snapshot/hourly.2020-05-15_1306/my.txt`   
    `my.txt`

2. Используйте `cp` команду, чтобы скопировать файл в родительский каталог.  

    Пример: 

    `$ cp .snapshot/hourly.2020-05-15_1306/my.txt .`   

    `$ ls my.txt`   
    `my.txt`   

### <a name="restore-a-file-by-using-a-windows-client"></a>Восстановление файла с помощью клиента Windows 

1. Если `~snapshot` каталог тома скрыт, отобразите [скрытые элементы](https://support.microsoft.com/help/4028316/windows-view-hidden-files-and-folders-in-windows-10) в родительском каталоге `~snapshot` .

    ![Отображение скрытых элементов](../media/azure-netapp-files/snapshot-show-hidden.png) 

2. Перейдите к подкаталогу в, `~snapshot` чтобы найти файл, который нужно восстановить.  Щелкните файл правой кнопкой мыши. Нажмите **Копировать**.  

    ![Копировать файл для восстановления](../media/azure-netapp-files/snapshot-copy-file-restore.png) 

3. Вернитесь к родительскому каталогу. Щелкните правой кнопкой мыши родительский каталог и выберите команду `Paste` , чтобы вставить файл в каталог.

    ![Вставить файл для восстановления](../media/azure-netapp-files/snapshot-paste-file-restore.png) 

4. Можно также щелкнуть правой кнопкой мыши родительский каталог, выбрать пункт **Свойства**, щелкнуть вкладку **предыдущие версии** , чтобы просмотреть список моментальных снимков, и выбрать **восстановить** для восстановления файла.  

    ![Свойства предыдущих версий](../media/azure-netapp-files/snapshot-properties-previous-version.png) 

## <a name="next-steps"></a>Дальнейшие шаги

* [Устранение неполадок с политиками моментальных снимков](troubleshoot-snapshot-policies.md)
* [Ограничения ресурсов для службы Azure NetApp Files](azure-netapp-files-resource-limits.md)
* [Видео о Azure NetApp Filesных моментальных снимках 101](https://www.youtube.com/watch?v=uxbTXhtXCkw&feature=youtu.be)