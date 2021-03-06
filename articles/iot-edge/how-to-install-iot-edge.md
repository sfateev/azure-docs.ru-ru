---
title: Установка Azure IoT Edge | Документация Майкрософт
description: Azure IoT Edge инструкции по установке на устройствах Windows или Linux
author: kgremban
manager: philmea
ms.reviewer: veyalla
ms.service: iot-edge
services: iot-edge
ms.topic: conceptual
ms.date: 10/07/2020
ms.author: kgremban
ms.openlocfilehash: 7ab62b04f8bea76c7efb587665f87ccaf123da24
ms.sourcegitcommit: ae6e7057a00d95ed7b828fc8846e3a6281859d40
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/16/2020
ms.locfileid: "92109006"
---
# <a name="install-or-uninstall-the-azure-iot-edge-runtime"></a>Установка или удаление среды выполнения Azure IoT Edge

Среда выполнения Azure IoT Edge превращает устройство в устройство IoT Edge. Среду выполнения можно развернуть на всех устройствах — от небольшого Raspberry Pi до технического сервера. После настройки устройства с помощью среды выполнения IoT Edge можно приступить к развертыванию в ней бизнес-логики из облака. Дополнительные сведения см. [в разделе Знакомство со средой выполнения Azure IOT EDGE и ее архитектурой](iot-edge-runtime.md).

Настройка устройства IoT Edge выполняется в два этапа. Первым шагом является установка среды выполнения и ее зависимостей, которые рассматриваются в этой статье. Вторым шагом является подключение устройства к удостоверению в облаке и Настройка проверки подлинности с помощью центра Интернета вещей. Эти действия приведены в следующих статьях.

В этой статье перечислены действия по установке среды выполнения Azure IoT Edge на устройствах Linux или Windows. Для устройств Windows у вас есть дополнительный вариант использования контейнеров Linux или контейнеров Windows. В настоящее время в рабочих сценариях рекомендуется использовать контейнеры Windows в Windows. Контейнеры Linux в Windows полезны для сценариев разработки и тестирования, особенно при разработке на компьютере под управлением Windows для развертывания на устройствах Linux.

## <a name="prerequisites"></a>Предварительные требования

Последние сведения о том, какие операционные системы в настоящее время поддерживаются в рабочих сценариях, см. в разделе [Azure IOT Edge Поддерживаемые системы](support.md#operating-systems) .

# <a name="linux"></a>[Linux](#tab/linux)

Наличие устройства Linux x64, ARM32 или ARM64. Корпорация Майкрософт предоставляет установочные пакеты для Ubuntu Server 16,04, Ubuntu Server 18,04 и Raspbian Stretch операционной системы.

>[!NOTE]
>Поддержка устройств ARM64 доступна в [общедоступной предварительной версии](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Подготовьте устройство для доступа к пакетам установки Майкрософт.

1. Установите конфигурацию репозитория, соответствующую операционной системе устройства.

   * **Ubuntu Server 16,04**:

     ```bash
     curl https://packages.microsoft.com/config/ubuntu/16.04/multiarch/prod.list > ./microsoft-prod.list
     ```

   * **Ubuntu Server 18,04**:

     ```bash
     curl https://packages.microsoft.com/config/ubuntu/18.04/multiarch/prod.list > ./microsoft-prod.list
     ```

   * **Raspbian Stretch**:

     ```bash
     curl https://packages.microsoft.com/config/debian/stretch/multiarch/prod.list > ./microsoft-prod.list
     ```

2. Скопируйте созданный список в каталог sources. List. d.

   ```bash
   sudo cp ./microsoft-prod.list /etc/apt/sources.list.d/
   ```

3. Установите открытый ключ Microsoft GPG.

   ```bash
   curl https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor > microsoft.gpg
   sudo cp ./microsoft.gpg /etc/apt/trusted.gpg.d/
   ```

# <a name="windows"></a>[Windows](#tab/windows)

### <a name="windows-version"></a>Версия Windows

Для IoT Edge с контейнерами Windows требуется версия Windows 1809/Build 17762, которая является последней [сборкой долгосрочной поддержки Windows](/windows/release-information/). Для сценариев разработки и тестирования любой SKU (Pro, Enterprise, Server и т. д.), поддерживающий функцию контейнеров, будет работать. Однако перед переходом в рабочую среду обязательно ознакомьтесь со списком поддерживаемых систем.

IoT Edge с контейнерами Linux могут работать в любой версии Windows, которая соответствует [требованиям для работы с DOCKER Desktop](https://docs.docker.com/docker-for-windows/install/#what-to-know-before-you-install).

### <a name="container-engine-requirements"></a>Требования к подсистеме контейнеров

Azure IoT Edge полагается на подсистему контейнера, [совместимую с OCI](https://www.opencontainers.org/) . Убедитесь, что устройство может поддерживать контейнеры.

Если вы устанавливаете IoT Edge на виртуальную машину, включите вложенную виртуализацию и выделите не менее 2 ГБ памяти. Для виртуальных машин Hyper-V версии 2 вложенная виртуализация включена по умолчанию. Для VMware существует переключатель, позволяющий включить эту функцию на виртуальной машине.

Если вы устанавливаете IoT Edge на устройстве IoT Core, используйте следующую команду в [удаленном сеансе PowerShell](/windows/iot-core/connect-your-device/powershell) , чтобы проверить, поддерживаются ли на устройстве контейнеры Windows:

```powershell
Get-Service vmcompute
```

---

Azure IoT Edge пакеты программного обеспечения подчиняются условиям лицензии, расположенным в каждом пакете ( `usr/share/doc/{package-name}` или в `LICENSE` каталоге). Прочтите условия лицензионного соглашения перед использованием пакета. Установка и использование пакета является принятием этих условий. Если вы не согласны с условиями лицензии, не используйте этот пакет.

## <a name="install-a-container-engine"></a>Установка подсистемы контейнеров

Служба Azure IoT Edge использует среду выполнения контейнера, совместимую с OCI. Для рабочих сценариев рекомендуется использовать подсистему на основе значок Кита. Подсистема значок Кита — это единственный механизм контейнеров, официально поддерживаемый Azure IoT Edge. Образы контейнеров Docker (Community Edition или Enterprise Edition) совместимы со средой выполнения Moby.

# <a name="linux"></a>[Linux](#tab/linux)

Обновление списков пакетов на устройстве.

   ```bash
   sudo apt-get update
   ```

Установите модуль Moby.

   ```bash
   sudo apt-get install moby-engine
   ```

Если при установке подсистемы контейнеров значок Кита возникают ошибки, проверьте совместимость ядра Linux с значок Кита. Некоторые производители встраиваемых устройств доставляют образы устройств, которые содержат пользовательские ядра Linux, без функций, необходимых для совместимости с модулем контейнеров. Выполните следующую команду, которая использует [Скрипт Check-config](https://github.com/moby/moby/blob/master/contrib/check-config.sh) , предоставленный значок Кита, для проверки конфигурации ядра:

   ```bash
   curl -sSL https://raw.githubusercontent.com/moby/moby/master/contrib/check-config.sh -o check-config.sh
   chmod +x check-config.sh
   ./check-config.sh
   ```

В выходных данных скрипта убедитесь, что все элементы в разделе `Generally Necessary` и `Network Drivers` включены. Если функции отсутствуют, включите их, перестройте ядро из источника и выберите связанные модули для включения в соответствующий файл kernel. config. Аналогичным образом, если вы используете генератор конфигурации ядра `defconfig` , например или `menuconfig` , найдите и включите соответствующие функции и перестройте ядро соответствующим образом. После развертывания недавно измененного ядра запустите сценарий Check-config еще раз, чтобы убедиться, что все необходимые компоненты были успешно включены.

# <a name="windows"></a>[Windows](#tab/windows)

Для рабочих сценариев используйте подсистему на основе значок Кита, включенную в скрипт установки. Нет дополнительных действий для установки подсистемы.

Для IoT Edge с контейнерами Linux необходимо предоставить собственную среду выполнения контейнера. Установите на устройстве [DOCKER Desktop](https://docs.docker.com/docker-for-windows/install/) и настройте его для [использования контейнеров Linux](https://docs.docker.com/docker-for-windows/#switch-between-windows-and-linux-containers) перед продолжением.

---

## <a name="install-the-iot-edge-security-daemon"></a>Установка управляющей программы IoT Edge Security

Управляющая программа IoT Edge безопасности предоставляет и поддерживает стандарты безопасности на устройстве IoT Edge. Управляющая программа запускается при каждой загрузке устройства и перезагружает устройство, запуская остальные компоненты среды выполнения IoT Edge.

Действия, описанные в этом разделе, представляют типичный процесс установки последней версии на устройстве с подключением к Интернету. Если необходимо установить определенную версию, например предварительную версию или установить ее в автономном режиме, следуйте инструкциям по [установке автономной или конкретной версии](#offline-or-specific-version-installation) в следующем разделе.

# <a name="linux"></a>[Linux](#tab/linux)

Обновление списков пакетов на устройстве.

   ```bash
   sudo apt-get update
   ```

Проверьте, какие версии IoT Edge доступны.

   ```bash
   apt list -a iotedge
   ```

Если вы хотите установить последнюю версию управляющей программы безопасности, используйте следующую команду, которая также устанавливает последнюю версию пакета **либиоссм-STD** :

   ```bash
   sudo apt-get install iotedge
   ```

Если вы хотите установить определенную версию управляющей программы безопасности, укажите ее версию в выходных данных списка APT. Также укажите ту же версию для пакета **либиоссм-STD** , которая в противном случае установит последнюю версию. Например, следующая команда устанавливает последнюю версию выпуска 1.0.8:

   ```bash
   sudo apt-get install iotedge=1.0.8* libiothsm-std=1.0.8*
   ```

Если версия, которую требуется установить, отсутствует в списке, выполните действия по [установке автономной или конкретной версии](#offline-or-specific-version-installation) в следующем разделе. В этом разделе показано, как ориентироваться на любую предыдущую версию управляющей программы IoT Edge или версии-кандидата.

# <a name="windows"></a>[Windows](#tab/windows)

>[!TIP]
>Для устройств IoT Core рекомендуется запускать команды установки с помощью удаленного сеанса PowerShell. Дополнительные сведения см. [в статье Использование PowerShell для Windows IOT](/windows/iot-core/connect-your-device/powershell).

1. Откройте сеанс PowerShell от имени администратора.

   Используйте сеанс AMD64 PowerShell, а не PowerShell (x86). Если вы не уверены, какой тип сеанса вы используете, выполните следующую команду:

   ```powershell
   (Get-Process -Id $PID).StartInfo.EnvironmentVariables["PROCESSOR_ARCHITECTURE"]
   ```

2. Выполните команду [deploy-IoTEdge](reference-windows-scripts.md#deploy-iotedge) , которая выполняет следующие задачи:

   * Проверяет, что компьютер Windows находится в поддерживаемой версии.
   * Включает функцию контейнеров.
   * Скачивает модуль значок Кита и среду выполнения IoT Edge.

   ```powershell
   . {Invoke-WebRequest -useb https://aka.ms/iotedge-win} | Invoke-Expression; `
   Deploy-IoTEdge
   ```

   `Deploy-IoTEdge`Команда по умолчанию использует контейнеры Windows. Если вы хотите использовать контейнеры Linux, добавьте `ContainerOs` параметр:

   ```powershell
   . {Invoke-WebRequest -useb https://aka.ms/iotedge-win} | Invoke-Expression; `
   Deploy-IoTEdge -ContainerOs Linux
   ```

3. На этом этапе устройства IoT Core могут перезапускаться автоматически. На устройствах Windows 10 или Windows Server может появиться запрос на перезагрузку. Если да, перезагрузите устройство прямо сейчас.

При установке IoT Edge на устройстве можно использовать дополнительные параметры для изменения процесса, включая:

* Направить трафик через прокси-сервер.
* Наведите установщик на локальный каталог для автономной установки.

Дополнительные сведения об этих дополнительных параметрах см. [в статье сценарии PowerShell для IOT EDGE в Windows](reference-windows-scripts.md).

---

Теперь, когда модуль контейнера и среда выполнения IoT Edge установлены на устройстве, вы готовы к следующему этапу, который заключается в регистрации устройства в центре Интернета вещей и настройке устройства с удостоверением облака и сведениями о проверке подлинности.

Выберите следующую статью в зависимости от типа проверки подлинности, который вы хотите использовать.

* [Проверка подлинности с симметричным ключом](how-to-manual-provision-symmetric-key.md) выполняется быстрее, чтобы начать работу.
* [Проверка подлинности на сертификате X. 509](how-to-manual-provision-x509.md) более безопасна в рабочих сценариях.

## <a name="offline-or-specific-version-installation"></a>Установка в автономном режиме или с определенной версией

Действия, описанные в этом разделе, предназначены для сценариев, не охваченных стандартными шагами установки. Это может быть:

* Установка IoT Edge в автономном режиме
* Установка версии-кандидата
* В Windows установите версию, отличную от последней

# <a name="linux"></a>[Linux](#tab/linux)

Если требуется установить определенную версию среды выполнения Azure IoT Edge, которая недоступна через, выполните действия, описанные в этом разделе `apt-get install` . Список пакетов Майкрософт содержит только ограниченный набор последних версий и их подверсий, поэтому эти действия предназначены для всех, кто хочет установить более старую версию или версию-кандидат.

С помощью фигурных команд можно ориентироваться на файлы компонентов непосредственно из репозитория IoT Edge GitHub.

<!-- TODO: Offline installation? -->

1. Перейдите к [Azure IOT Edge выпускам](https://github.com/Azure/azure-iotedge/releases)и найдите целевую версию для выпуска.

2. Разверните раздел **активы** для этой версии.

3. Каждый выпуск должен иметь новые файлы для управляющей программы IoT Edge безопасности и хсмлиб. Используйте следующие команды для обновления этих компонентов.

   1. Найдите файл **либиоссм-STD** , соответствующий архитектуре устройства IOT Edge. Щелкните ссылку на файл правой кнопкой мыши и скопируйте адрес ссылки.

   2. Чтобы установить эту версию хсмлиб, используйте скопированную ссылку в следующей команде:

      ```bash
      curl -L <libiothsm-std link> -o libiothsm-std.deb && sudo dpkg -i ./libiothsm-std.deb
      ```

   3. Найдите файл **iotedge** , соответствующий архитектуре устройства IOT Edge. Щелкните ссылку на файл правой кнопкой мыши и скопируйте адрес ссылки.

   4. Используйте ссылку копировать в следующей команде, чтобы установить эту версию управляющей программы IoT Edge безопасности.

      ```bash
      curl -L <iotedge link> -o iotedge.deb && sudo dpkg -i ./iotedge.deb
      ```

# <a name="windows"></a>[Windows](#tab/windows)

Во время установки будут скачаны три файла:

* Сценарий PowerShell, содержащий инструкции по установке
* Microsoft Azure IoT Edge CAB-файл, содержащий управляющую программу IoT Edge (иотеджед), контейнер значок Кита и интерфейс командной строки значок Кита
* Установщик Visual C++ распространяемого пакета (среда выполнения VC)

Если устройство будет отключено во время установки или вы хотите установить определенную версию IoT Edge, эти файлы можно загрузить на устройство заранее. Когда пора установить, наведите сценарий установки в каталог, содержащий Скачанные файлы. Установщик сначала проверяет каталог, а затем загружает только те компоненты, которые не найдены. Если все файлы доступны в автономном режиме, можно установить без подключения к Интернету.

1. Последние файлы установки IoT Edge вместе с предыдущими версиями см. в разделе [Azure IOT Edge выпуски](https://github.com/Azure/azure-iotedge/releases).

2. Найдите версию, которую вы хотите установить, и скачайте следующие файлы из раздела " **активы** " заметки о выпуске на устройство IOT:

   * IoTEdgeSecurityDaemon.ps1
   * Microsoft-Azure-IoTEdge-amd64.cab из выпусков 1.0.9 или более поздней версии или Microsoft-Azure-IoTEdge.cab из выпусков 1.0.8 и более старых версий.

   Microsoft-Azure-IotEdge-arm32.cab также доступна начиная с 1.0.9 только в целях тестирования. IoT Edge в настоящее время не поддерживается на устройствах Windows ARM32.

   Очень важно использовать сценарий PowerShell из того же выпуска, что и используемый CAB-файл, так как функциональность изменяется для поддержки функций в каждом выпуске.

3. Если загруженный CAB-файл имеет суффикс архитектуры, переименуйте файл в **Microsoft-Azure-IoTEdge.cab**.

4. При необходимости скачайте установщик для распространяемого пакета Visual C++. Например, сценарий PowerShell использует эту версию: [vc_redist.x64.exe](https://download.microsoft.com/download/0/6/4/064F84EA-D1DB-4EAA-9A5C-CC2F0FF6A638/vc_redist.x64.exe). Сохраните установщик в той же папке на устройстве IoT, что и файлы IoT Edge.

5. Чтобы установить с автономными компонентами, [источником точки](/powershell/module/microsoft.powershell.core/about/about_scripts#script-scope-and-dot-sourcing) является локальная копия сценария PowerShell. 

6. Выполните команду [deploy-IoTEdge](reference-windows-scripts.md#deploy-iotedge) с `-OfflineInstallationPath` параметром. Укажите абсолютный путь к каталогу файлов. Например:

   ```powershell
   . <path>\IoTEdgeSecurityDaemon.ps1
   Deploy-IoTEdge -OfflineInstallationPath <path>
   ```

   Команда развертывания будет использовать все компоненты, найденные в указанном каталоге локальных файлов. Если CAB-файл или установщик Visual C++ отсутствуют, он попытается скачать их.

---

Теперь, когда модуль контейнера и среда выполнения IoT Edge установлены на устройстве, вы можете перейти к следующему этапу, который предназначен для [проверки Подлинности IOT Edge устройства в центре Интернета вещей](how-to-manual-provision-symmetric-key.md).

## <a name="uninstall-iot-edge"></a>Удаление IoT Edge

Если вы хотите удалить IoT Edge установку с устройства, используйте следующие команды.

# <a name="linux"></a>[Linux](#tab/linux)

Удалите среду выполнения IoT Edge.

```bash
sudo apt-get remove --purge iotedge
```

При удалении среды выполнения IoT Edge все созданные контейнеры останавливаются, но остаются на устройстве. Просмотрите все контейнеры, чтобы увидеть, какие из них остаются.

```bash
sudo docker ps -a
```

Удалите контейнеры с устройства, включая два контейнера в среде выполнения.

```bash
sudo docker rm -f <container name>
```

Наконец, удалите среду выполнения контейнера с устройства.

```bash
sudo apt-get remove --purge moby-cli
sudo apt-get remove --purge moby-engine
```

# <a name="windows"></a>[Windows](#tab/windows)

Если вы хотите удалить IoT Edge установку с устройства Windows, используйте команду [uninstall-IoTEdge](reference-windows-scripts.md#uninstall-iotedge) в административном окне PowerShell. Эта команда удаляет среду выполнения IoT Edge вместе с существующей конфигурацией и данными подсистемы Moby.

```powershell
. {Invoke-WebRequest -useb aka.ms/iotedge-win} | Invoke-Expression; `
Uninstall-IoTEdge
```

`Uninstall-IoTEdge`Команда не работает в Windows IOT базовая. Чтобы удалить IoT Edge, необходимо повторно развернуть образ Windows IoT Core.

Для получения дополнительных сведений о параметрах удаления используйте команду `Get-Help Uninstall-IoTEdge -full` .

---

## <a name="next-steps"></a>Дальнейшие действия

После установки IoT Edge среды выполнения настройте устройство для подключения к центру Интернета вещей. В следующих статьях подробно рассматривается регистрация нового устройства в облаке с последующим предоставлением ему удостоверений и сведений о проверке подлинности.

Выберите следующую статью в зависимости от типа проверки подлинности, которую вы хотите использовать:

* **Симметричный ключ**: центр Интернета вещей и устройство IOT EDGE имеют копию безопасного ключа. Когда устройство подключается к центру Интернета вещей, они проверяют, совпадают ли ключи. Этот метод проверки подлинности быстрее приступит к работе, но не защищен.

  [Настройка Azure IoT Edge устройства с проверкой подлинности с помощью симметричного ключа](how-to-manual-provision-symmetric-key.md)

* **X. 509 Self-подпись**: устройство IOT Edge имеет сертификаты удостоверений x. 509, а центр Интернета вещей получает отпечаток сертификатов. Когда устройство подключается к центру Интернета вещей, оно сравнивает сертификат с его отпечатком. Этот метод проверки подлинности более безопасен и рекомендуется для рабочих сценариев.

  [Настройка Azure IoT Edge устройства с проверкой подлинности на сертификате X. 509](how-to-manual-provision-x509.md)