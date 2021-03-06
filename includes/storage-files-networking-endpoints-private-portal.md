---
title: включить файл
description: включить файл
services: storage
author: roygara
ms.service: storage
ms.topic: include
ms.date: 5/11/2020
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: b941084c8a196081c2443364ed3fb52868386670
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2020
ms.locfileid: "84465063"
---
Перейдите к учетной записи хранения, для которой вы хотите создать частную конечную точку. В таблице содержимого для учетной записи хранения щелкните **Подключения к частной конечной точке**, а затем выберите **+ Частная конечная точка**, чтобы создать частную конечную точку. 

[![Снимок экрана: элемент "Подключения к частным конечным точкам" в таблице содержимого для учетной записи хранения](media/storage-files-networking-endpoints-private-portal/create-private-endpoint-0.png)](media/storage-files-networking-endpoints-private-portal/create-private-endpoint-0.png#lightbox)

В мастере нужно будет заполнить несколько страниц.

В колонке **Основные сведения** выберите нужную группу ресурсов, имя и регион для частной конечной точки. Они могут быть любыми, но не должны совпадать с учетной записью хранения. Тем не менее необходимо создать частную конечную точку в том же регионе, что и виртуальная сеть, в которой вы хотите создать частную конечную точку.

![Снимок экрана раздела "Основные сведения" в разделе "Создание частной конечной точки"](media/storage-files-networking-endpoints-private-portal/create-private-endpoint-1.png)

В колонке **Ресурс** установите переключатель рядом с пунктом **Подключиться к ресурсу Azure в моем каталоге**. В разделе **Тип ресурса** выберите **Microsoft.Storage/storageAccounts** в качестве типа. Поле **Ресурсы** — это учетная запись хранения с общей папкой Azure, к которой требуется подключиться. Целевой подресурс — это **файл**, так как это требуется для Файлов Azure.

В колонке **Конфигурация** можно выбрать определенную виртуальную сеть и подсеть, в которую вы хотите добавить частную конечную точку. В подсети, в которую вы добавили конечную точку службы, необходимо выбрать отдельную подсеть. В колонке "Конфигурация" также содержатся сведения для создания или обновления частной зоны DNS. Мы рекомендуем использовать стандартную зону `privatelink.file.core.windows.net`.

![Снимок экрана: раздел "Конфигурация"](media/storage-files-networking-endpoints-private-portal/create-private-endpoint-2.png)

Щелкните **Просмотр и создание**, чтобы создать частную конечную точку. 