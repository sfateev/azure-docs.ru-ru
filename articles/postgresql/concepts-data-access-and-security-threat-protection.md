---
title: Расширенная защита от угроз — база данных Azure для PostgreSQL — один сервер
description: Узнайте об использовании Advanced Threat Protection для обнаружения аномальных действий базы данных, указывающих на потенциальные угрозы безопасности для базы данных.
author: sunilagarwal
ms.author: sunila
ms.service: postgresql
ms.topic: conceptual
ms.date: 5/6/2019
ms.openlocfilehash: cfe565c45ea6aa0a4bcfecc95b1e1149b17542a2
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2020
ms.locfileid: "91708056"
---
# <a name="advanced-threat-protection-in-azure-database-for-postgresql---single-server"></a>Расширенная защита от угроз в базе данных Azure для PostgreSQL — один сервер

Служба "Расширенная защита от угроз" для Базы данных Azure для PostgreSQL позволяет выявить подозрительную активность, указывающую на необычные и потенциально опасные попытки получить доступ к базам данных или воспользоваться ими.

> [!NOTE]
> Расширенная защита от угроз доступна в общедоступной предварительной версии.

Защита от угроз входит в состав предложения "Расширенная защита от угроз (ATP)", которое представляет собой унифицированный пакет расширенных возможностей безопасности. К расширенной защите угроз можно получить доступ и управлять ими с помощью [портал Azure](https://portal.azure.com) или [REST API](/rest/api/postgresql). Эта функция доступна для общего назначения и оптимизированных для памяти серверов.

> [!NOTE]
> Компонент "Расширенная защита от угроз" **недоступен** в следующих регионах облака правительства и национального облака Azure: US Gov (Техас), US Gov (Аризона), US Gov (Айова), US Gov (Вирджиния), восточный регион US DoD, центральный регион US DoD, Центральная Германия, Северная Германия, Восточный Китай, Восточный Китай 2. Сведения об общей доступности продукта по регионам см. на [этой странице](https://azure.microsoft.com/global-infrastructure/services/).

## <a name="what-is-advanced-threat-protection"></a>Что являет собою Расширенная защита от угроз Azure?

Отображая оповещения системы безопасности о подозрительных действиях, Расширенная защита от угроз Azure для Базы данных Azure для PostgreSQL предоставляет дополнительный уровень защиты, позволяя клиентам определять потенциальные угрозы по мере возникновения и реагировать на них. Пользователи получают оповещения о подозрительных действиях с базами данных, потенциальных уязвимостях и аномальных закономерностях в доступе к базам данных и шаблонам запросов. Служба "Расширенная защита от угроз" для Базы данных Azure для PostgreSQL интегрирует оповещения с [центром безопасности Azure](https://azure.microsoft.com/services/security-center/), который предоставляет сведения о подозрительных операциях и дает рекомендации о том, как определить причину угрозы и устранить ее. Служба "Расширенная защита от угроз" для Базы данных Azure для PostgreSQL упрощает устранение потенциальных угроз безопасности базы данных, не требуя от пользователя экспертных навыков в сфере безопасности либо умения работать в сложных системах отслеживания угроз. 

:::image type="content" source="media/concepts-data-access-and-security-threat-protection/advanced-threat-protection-concept.png" alt-text="Общее представление о службе &quot;Расширенная защита от угроз&quot;":::

## <a name="advanced-threat-protection-alerts"></a>Оповещения Расширенной защиты от угроз 
Служба "Расширенная защита от угроз" для Базы данных Azure для PostgreSQL позволяет выявить подозрительную активность, указывающую на необычные и потенциально опасные попытки получить доступ к базам данных или воспользоваться ими, и может активировать следующие оповещения.
- **Доступ из незнакомого расположения**. Это предупреждение активируется при изменении шаблона доступа к серверу Базы данных Azure для PostgreSQL, если пользователь вошел на этот сервер из незнакомого географического расположения. Иногда предупреждение определяет допустимые действия (новое приложение, обслуживание разработчиком). В других случаях предупреждение обнаруживает угрозу (бывшего сотрудника, внешнего злоумышленника).
- **Доступ из стороннего центра обработки данных Azure.** Это предупреждение активируется при изменении шаблона доступа к серверу Базы данных Azure для PostgreSQL, если кто-то за последнее время выполнил вход на сервер из стороннего центра обработки данных Azure. Иногда предупреждение определяет допустимые действия (новое приложение в Azure, Power BI, редактор запросов Базы данных Azure для PostgreSQL). В других случаях предупреждение обнаруживает угрозу от ресурса или службы Azure (бывшего сотрудника, внешнего злоумышленника).
- **Доступ из незнакомого субъекта**. Это предупреждение активируется при изменении шаблона доступа к серверу Базы данных Azure для PostgreSQL, если пользователь вошел на этот сервер из незнакомого субъекта (Пользователь База данных Azure для PostgreSQL). Иногда предупреждение определяет допустимые действия (новое приложение, обслуживание разработчиком). В других случаях предупреждение обнаруживает угрозу (бывшего сотрудника, внешнего злоумышленника).
- **Доступ из потенциально опасного приложения.** Это предупреждение активируется, когда доступ к базе данных получают потенциально опасные приложения. В некоторых случаях предупреждения обнаруживают выполнение теста на проникновение. В других случаях — атаку с помощью обычных средств.
- **Подбор учетных данных Базы данных Azure для PostgreSQL**. Это предупреждение активируется при большом количестве неудачных попыток входа с разными учетными данными. В некоторых случаях предупреждения обнаруживают выполнение теста на проникновение. В других случаях — атаку методом подбора.

## <a name="next-steps"></a>Дальнейшие шаги

* Узнайте больше о [центре безопасности Azure](https://docs.microsoft.com/azure/security-center/security-center-intro).
* Дополнительные сведения о ценах см. на [странице цен на Базу данных Azure для PostgreSQL](https://azure.microsoft.com/pricing/details/postgresql/). 
* Настройка [Расширенной защиты от угроз для Базы данных Azure для PostgreSQL](howto-database-threat-protection-portal.md) с помощью портала Azure  
