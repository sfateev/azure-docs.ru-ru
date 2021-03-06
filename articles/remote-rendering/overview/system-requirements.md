---
title: Требования к системе
description: Список требований к системе для удаленной подготовки к просмотру Azure
author: florianborn71
ms.author: flborn
ms.date: 02/03/2020
ms.topic: article
ms.openlocfilehash: 31fde0c7af652bc50eb5f06743c5dd5807a1762e
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2020
ms.locfileid: "91323731"
---
# <a name="system-requirements"></a>Требования к системе

> [!IMPORTANT]
> **Удаленная отрисовка Azure** в настоящее время находится в общедоступной предварительной версии.
> Эта предварительная версия предоставляется без соглашения об уровне обслуживания и не рекомендована для использования рабочей среде. Некоторые функции могут не поддерживаться или их возможности могут быть ограничены. Дополнительные сведения см. в статье [Дополнительные условия использования предварительных выпусков Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

В этой главе перечислены минимальные системные требования для работы с *удаленной визуализацией Azure* (ARR).

## <a name="development-pc"></a>ПК для разработки

* Windows 10 версии 1903 или более поздней.
* Последние версии драйверов графики.
* Необязательно: [H265 аппаратный видеодекодер](https://www.microsoft.com/p/hevc-video-extensions/9nmzlz57r3t7), если вы хотите использовать локальную предварительную версию удаленного просмотра содержимого (например, в Unity).

> [!IMPORTANT]
> Windows Update не всегда доставляет самые последние драйверы GPU, проверьте наличие последних версий драйверов на веб-сайте производителя GPU:
>
> * [Драйверы AMD](https://www.amd.com/en/support)
> * [Драйверы Intel](https://www.intel.com/content/www/us/en/support/detect.html)
> * [Драйверы NVIDIA](https://www.nvidia.com/Download/index.aspx)

В таблице ниже перечислены графические процессоры, поддерживающие H265 аппаратное декодирование видео.

| Производитель GPU | Поддерживаемые модели |
|-----------|:-----------|
| NVIDIA | Просмотрите **таблицу поддержки нвдек** [в нижней части этой страницы](https://developer.nvidia.com/video-encode-decode-gpu-support-matrix). Для графического процессора требуется значение Да в столбце **H. 265 4:2:0 8-bit** . |
| AMD | GPU, версия [единого видеодекодера](https://en.wikipedia.org/wiki/Unified_Video_Decoder#UVD_6)AMD не ниже 6. |
| Intel | Skylake и более новые ЦП |

Несмотря на то, что может быть установлен правильный кодек H265, свойства безопасности в библиотеках DLL кодека могут вызвать сбои инициализации кодека. В [руководстве по устранению неполадок](../resources/troubleshoot.md#h265-codec-not-available) описаны действия по решению этой проблемы. Ошибка библиотеки DLL может возникнуть только при использовании службы в классическом приложении, например в Unity.

## <a name="devices"></a>Устройства

В настоящее время удаленная визуализация Azure поддерживает только **HoloLens 2** и Windows Desktop в качестве целевого устройства. См. раздел [ограничения платформы](../reference/limits.md#platform-limitations) .

Очень важно использовать последний кодек HEVC, так как более новые версии значительно улучшили задержку. Чтобы проверить, какая версия установлена на вашем устройстве, выполните следующие действия.

1. Запустите **Microsoft Store**.
1. Нажмите кнопку **"..."** в правом верхнем углу.
1. Выберите **загрузки и обновления**.
1. Найдите в списке **расширения видео HEVC у производителя устройства**. Если этот элемент не указан в списке обновлений, то последняя версия уже установлена.
1. Убедитесь, что указанный кодек имеет версию не ниже **1.0.21821.0**.
1. Нажмите кнопку **Get Updates (получить обновления** ) и дождитесь ее установки.

## <a name="network"></a>Сеть

Устойчивое сетевое подключение с низкой задержкой очень важно для удобства работы пользователей.

[Требования к сети](../reference/network-requirements.md)см. в разделе "выделенная глава".

Сведения об устранении неполадок в сети см. в [руководстве по устранению неполадок](../resources/troubleshoot.md#unstable-holograms).

### <a name="network-ports"></a>Сетевые порты

Убедитесь, что брандмауэры (на устройстве, в маршрутизаторах и т. д.) не блокируют следующие порты:

| Порт              | Протокол | Allow    | Описание |
|-------------------|----------|----------|-------------|
| 50051             | TCP      | Исходящий | Начальное соединение (подтверждение HTTP) |
| 8266              | Протокол UDP      | Исходящий | Передача данных |
| 5000, 5433, 8443  | TCP      | Исходящий | Требуется для [средства арринспектор](../resources/tools/arr-inspector.md)|


## <a name="software"></a>Программное обеспечение

Необходимо установить следующее программное обеспечение:

* Последняя версия **Visual Studio 2019** [(Загрузка)](https://visualstudio.microsoft.com/vs/older-downloads/)
* [Средства Visual Studio для службы "Смешанная реальность"](https://docs.microsoft.com/windows/mixed-reality/install-the-tools). В частности, обязательно установить следующие *рабочие нагрузки*:
  * **Разработка классических приложений на C++** .
  * **Разработка приложений для универсальной платформы Windows (UWP)** .
* **Windows SDK 10.0.18362.0** [(Загрузка)](https://developer.microsoft.com/windows/downloads/windows-10-sdk)
* **Git** [(Загрузка)](https://git-scm.com/downloads)
* Необязательно. для просмотра видеопотока с сервера на настольном компьютере необходимы **расширения HEVC Video** [(ссылка Microsoft Store)](https://www.microsoft.com/p/hevc-video-extensions/9nmzlz57r3t7). Убедитесь, что установлена последняя версия, проверив наличие обновлений в хранилище.

## <a name="unity"></a>Unity

Для разработки с Unity установите

* Unity 2019.3.1 [(скачать)](https://unity3d.com/get-unity/download).
* Установите следующие модули в Unity:
  * **UWP** — обеспечивает поддержку для создания приложений универсальной платформы Windows;
  * **IL2CPP** — обеспечивает поддержку сборки для Windows (IL2CPP).

## <a name="next-steps"></a>Дальнейшие действия

* [Краткое руководство. Отрисовка модели с помощью Unity](../quickstarts/render-model.md)
