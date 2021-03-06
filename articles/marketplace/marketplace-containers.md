---
title: Рекомендации по публикации для предложений контейнеров в Azure Marketplace
description: В этой статье описываются требования к публикации предложений контейнеров в Azure Marketplace.
services: Azure, Marketplace, Compute, Storage, Networking, Blockchain, Security
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
author: keferna
ms.author: keferna
ms.date: 09/04/2020
ms.openlocfilehash: c52fabcfc2ff22df2de6dd93f2543d625310baef
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2020
ms.locfileid: "89484347"
---
# <a name="publishing-guide-for-container-offers"></a>Рекомендации по публикации для предложений контейнеров

С помощью предложений контейнеров вы сможете опубликовать образ контейнера в Azure Marketplace. Сведения в этом руководстве помогут понять требования к этому предложению. 

Предложения контейнеров — это предложения транзакций, которые развертываются и выставляются через Azure Marketplace. Параметр List, отображаемый пользователем, — «получить сейчас».

Используйте тип предложения *контейнера* , если решение представляет собой образ контейнера DOCKER, настроенный как экземпляр службы контейнеров Azure на основе Kubernetes. 

> [!NOTE]
> Примерами экземпляров службы контейнеров Azure на основе Kubernetes являются служба Kubernetes Azure или службы "экземпляры контейнеров Azure", выбранные клиенты Azure для среды выполнения контейнеров на основе Kubernetes.  

В настоящее время корпорация Майкрософт поддерживает свободное лицензирование и модель с использованием собственной лицензии (BYOL).

## <a name="container-offer-requirements"></a>Требования к контейнерному предложению

| Требование | Сведения |  
|:--- |:--- |  
| Выставление счетов и ценообразование | Поддерживается либо бесплатная, либо модель выставления счетов BYOL.<br><br> |  
| Образ, созданный из Dockerfile | Образы контейнеров должны быть основаны на спецификации образа DOCKER и построены на основе Dockerfile.<br> <br>Дополнительные сведения о создании образов DOCKER см. в разделе "использование" [справочника по Dockerfile](https://docs.docker.com/engine/reference/builder/#usage).<br><br> |  
| Размещение в репозитории реестра контейнеров Azure | Образы контейнеров должны размещаться в репозитории реестра контейнеров Azure.<br> <br>Дополнительные сведения о работе с реестром контейнеров Azure см. [в разделе Краткое руководство. создание закрытого реестра контейнеров с помощью портал Azure](../container-registry/container-registry-get-started-portal.md).<br><br> |  
| Добавление тегов изображений | Образы контейнеров должны содержать по крайней мере один тег (максимальное число тегов: 16).<br><br>Дополнительные сведения о добавлении тегов к изображению см `docker tag` . на странице сайта [документации DOCKER](https://docs.docker.com/engine/reference/commandline/tag) .<br><br> |  

## <a name="next-steps"></a>Дальнейшие действия

Если вы еще не сделали этого, ознакомьтесь со статьей [Расширение облачного бизнеса с помощью Azure Marketplace](https://azuremarketplace.microsoft.com/sell).

Чтобы зарегистрироваться и начать работу в Центре партнеров, выполните следующие действия.

- [Войдите в Центр партнеров](https://partner.microsoft.com/dashboard/account/v3/enrollment/introduction/partnership), чтобы создать или завершить свое предложение.
- Дополнительные сведения см. [в разделе Создание предложения контейнера Azure](./partner-center-portal/create-azure-container-offer.md) .
