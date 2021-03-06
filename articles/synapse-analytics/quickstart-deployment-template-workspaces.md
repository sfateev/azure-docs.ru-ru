---
title: Краткое руководство. Создание рабочей области Azure Synapse с помощью шаблона Azure Resource Manager
description: Узнайте, как создать рабочую область Synapse с помощью шаблона Azure Resource Manager.
services: azure-resource-manager
author: julieMSFT
ms.service: azure-resource-manager
ms.topic: quickstart
ms.custom: subject-armqs
ms.author: jrasnick
ms.date: 08/07/2020
ms.openlocfilehash: dc6d36f2316e0ae19ce8b813fa9eb127b1a9cf1f
ms.sourcegitcommit: eb6bef1274b9e6390c7a77ff69bf6a3b94e827fc
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/05/2020
ms.locfileid: "91568999"
---
# <a name="quickstart-create-an-azure-synapse-workspace-using-a-deployment-template"></a>Краткое руководство. Создание рабочей области Azure Synapse с помощью шаблона развертывания

Этот шаблон Azure Resource Manager (шаблон ARM) создаст рабочую область Azure Synapse с базовым репозиторием Data Lake Storage. Рабочая область Azure Synapse — это защищаемая ограниченная область совместной работы для выполнения аналитических процессов в Azure Synapse Analytics.

[!INCLUDE [About Azure Resource Manager](../../includes/resource-manager-quickstart-introduction.md)]

Если среда соответствует предварительным требованиям и вы знакомы с использованием шаблонов ARM, нажмите кнопку **Развертывание в Azure**. Шаблон откроется на портале Azure.

[![Развертывание в Azure (шаг 1)](../media/template-deployments/deploy-to-azure.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure-Samples%2FSynapse%2Fmaster%2FManage%2FDeployWorkspace%2Fazuredeploy.json)

## <a name="prerequisites"></a>Предварительные требования

Если у вас еще нет подписки Azure, [создайте бесплатную учетную запись](https://azure.microsoft.com/free/?WT.mc_id=A261C142F), прежде чем начинать работу.

## <a name="review-the-template"></a>Изучение шаблона

Чтобы просмотреть шаблон, щелкните ссылку **Визуализация**:

[![Визуализация](../media/template-deployments/template-visualize-button.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure-Samples%2FSynapse%2Fmaster%2FManage%2FDeployWorkspace%2Fazuredeploy.json)

Шаблон определяет два ресурса:

- Учетная запись хранения
- Рабочая область

## <a name="deploy-the-template"></a>Развертывание шаблона

1. Выберите следующее изображение, чтобы войти на портал Azure и открыть шаблон. Этот шаблон создает рабочую область Synapse.
   
   [![Развертывание в Azure (шаг 2)](../media/template-deployments/deploy-to-azure.svg)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure-Samples%2FSynapse%2Fmaster%2FManage%2FDeployWorkspace%2Fazuredeploy.json)

1. Введите или измените следующие значения:

   * **Подписка**: Выберите подписку Azure.
   * **Группа ресурсов.** Выберите **Создать**, введите уникальное имя группы ресурсов и щелкните **ОК**. Использование новой группы ресурсов упростит очистку ресурсов.
   * **Регион**. Выберите регион.  Например, **центральная часть США**.
   * **Name** (Имя). Укажите имя рабочей области.
   * **Имя входа администратора SQL.** Введите имя пользователя администратора для сервера SQL Server.
   * **Пароль администратора SQL.** Введите пароль администратора для сервера SQL Server.
   * **Значения тегов.** Примите значение по умолчанию. 
   * **Просмотр и создание.** Установите этот флажок.
   * **Создание**. Установите этот флажок.

## <a name="next-steps"></a>Дальнейшие действия

Дополнительные сведения об Azure Synapse Analytics и Azure Resource Manager см. в статьях ниже.

- См. статью [Сведения об Azure Synapse Analytics](../synapse-analytics/sql-data-warehouse/sql-data-warehouse-overview-what-is.md) 
- Сведения об [Azure Resource Manager](../azure-resource-manager/management/overview.md)
- [Создание и развертывание первого шаблона ARM](../azure-resource-manager/templates/template-tutorial-create-first-template.md)
