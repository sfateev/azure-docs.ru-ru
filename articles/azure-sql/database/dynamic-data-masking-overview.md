---
title: Динамическое маскирование данных
description: Динамическое маскирование данных ограничивает раскрытие конфиденциальных данных за счет маскировки их на непривилегированных пользователей для базы данных SQL Azure, Azure SQL Управляемый экземпляр и Azure синапсе Analytics.
services: sql-database
ms.service: sql-db-mi
ms.subservice: security
ms.custom: sqldbrb=1
ms.devlang: ''
ms.topic: conceptual
author: DavidTrigano
ms.author: datrigan
ms.reviewer: vanto
ms.date: 08/04/2020
tags: azure-synpase
ms.openlocfilehash: 0689cea221142ec9c9bdbb18ab82fab00a3e2fe5
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2020
ms.locfileid: "91398618"
---
# <a name="dynamic-data-masking"></a>Динамическое маскирование данных 
[!INCLUDE[appliesto-sqldb-sqlmi-asa](../includes/appliesto-sqldb-sqlmi-asa.md)]

База данных SQL Azure, Azure SQL Управляемый экземпляр и Azure синапсе Analytics поддерживают динамическое Маскирование данных. Динамическое маскирование данных ограничивает возможность раскрытия конфиденциальных данных за счет маскирования этих данных для непривилегированных пользователей. 

Динамическое маскирование данных помогает предотвратить несанкционированный доступ к конфиденциальным данным, позволяя клиентам определить объем раскрываемых конфиденциальных данных с минимальным влиянием на уровень приложения. Эта функция безопасности на основе политики скрывает конфиденциальные данные в результирующем наборе запроса в заданных полях базы данных, а данные в базе данных не меняются.

Например, представитель отдела обслуживания клиентов в центре обработки вызовов может идентифицировать абонентов по нескольким цифрам номера кредитной карты, но эти элементы данных не должны предоставляться ему полностью. Правило маскирования может быть задано таким образом, чтобы в результатах выполнения запросов маскировалось все, кроме последних четырех цифр номера кредитной карты. В качестве другого примера можно определить подходящую маску данных для защиты персональных данных, чтобы разработчик мог отправлять запросы в рабочие среды для устранения неполадок без нарушения нормативных требований.

## <a name="dynamic-data-masking-basics"></a>Основные сведения о динамическом маскировании данных

Политика динамического маскирования данных настраивается в портал Azure, выбрав колонку **Динамическое маскирование данных** в разделе **Безопасность** в области Конфигурация базы данных SQL. Эта функция не может быть задана с помощью портала для Azure синапсе (используйте PowerShell или REST API) или SQL Управляемый экземпляр. Дополнительные сведения см. в разделе [Dynamic Data Masking](/sql/relational-databases/security/dynamic-data-masking).

### <a name="dynamic-data-masking-permissions"></a>Разрешения для динамической маскировки данных

Динамическую маскировку данных может настраивать администратор базы данных SQL Azure, администратор сервера или [диспетчер безопасности SQL](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#sql-security-manager).

### <a name="dynamic-data-masking-policy"></a>Политика динамической маскировки данных

* **Пользователи SQL, исключенные из маскирования** — набор пользователей SQL или удостоверений Azure AD, которые получают немаскированные данные в результатах SQL-запроса. Пользователи с привилегия администратора всегда исключаются из правил маскирования и видят исходные данные без какой-либо маски.
* **Правила маскирования** — набор правил, определяющих поля, подлежащие маскированию, и используемую функцию маскирования. Задать такие поля можно с помощью имени схемы базы данных, имени таблицы и имени столбца.
* **Функции маскирования** — набор методов, управляющих доступностью данных для различных сценариев.

| Функция маскирования | Логика маскирования |
| --- | --- |
| **Default** |**Полное маскирование в соответствии с типами данных указанных полей**<br/><br/>• Для строковых типов данных (nchar, ntext, nvarchar) используется XXXX или меньшее количество символов "X", если размер поля меньше 4 символов.<br/>• Для числовых типов данных (bigint, bit, decimal, int, money, numeric, smallint, smallmoney, tinyint, float, real) используется нулевое значение.<br/>• Для типов данных даты и времени (date, datetime2, datetime, datetimeoffset, smalldatetime, time) используется формат 01-01-1900.<br/>• Для варианта SQL используется значение по умолчанию текущего типа.<br/>• Для XML-документа используется \<masked/>.<br/>.• Используйте пустое значение для специальных типов данных (таблица меток времени, hierarchyid, GUID, binary, image, пространственные типы varbinary). |
| **Кредитная карта** |**Метод маскирования, который предоставляет последние четыре цифры назначенных полей** и добавляет строковую константу в качестве префикса в формате номера кредитной карты.<br/><br/>XXXX-XXXX-XXXX-1234 |
| **Электронная почта** |**Метод маскирования, который предоставляет первую букву и заменяет домен на XXX.com,** используя префикс постоянной строки в форме адреса электронной почты.<br/><br/>aXX@XXXX.com |
| **Случайное число** |**Метод маскирования, который формирует случайное число** в соответствии с выбранными границами и типами фактических данных. Если установленные границы равны, то функция маскирования будет постоянным числом.<br/><br/>![Снимок экрана, на котором показан метод маскирования для создания случайного числа.](./media/dynamic-data-masking-overview/1_DDM_Random_number.png) |
| **Пользовательский текст** |**Метод маскирования, при котором видны первая и последняя буквы** и добавляется пользовательская строка заполнения в середине. Если исходная строка короче открытых префикса и суффикса, то будет использоваться только строка заполнения. <br/>префикс[заполнение]суффикс<br/><br/>![Область навигации](./media/dynamic-data-masking-overview/2_DDM_Custom_text.png) |

<a name="Anchor1"></a>

### <a name="recommended-fields-to-mask"></a>Рекомендуемые поля для маскирования

Система рекомендаций DDM помечает в базе данных поля, которые могут содержать конфиденциальные данные и являются хорошими кандидатами на применение маскирования. Список рекомендуемых для маскирования столбцов в вашей базе данных см. на портале в колонке «Динамическое маскирование данных». Вам нужно только щелкнуть **Добавить маску** для одного или нескольких столбцов, а затем щелкнуть **Сохранить**, чтобы применить маски к выбранным полям.

## <a name="set-up-dynamic-data-masking-for-your-database-using-powershell-cmdlets"></a>Настройка динамического маскирования данных для базы данных с помощью командлетов PowerShell

### <a name="data-masking-policies"></a>Политики маскирования данных

- [Get-Азсклдатабаседатамаскингполици](https://docs.microsoft.com/powershell/module/az.sql/Get-AzSqlDatabaseDataMaskingPolicy)
- [Set-Азсклдатабаседатамаскингполици](https://docs.microsoft.com/powershell/module/az.sql/Set-AzSqlDatabaseDataMaskingPolicy)

### <a name="data-masking-rules"></a>Правила маскирования данных

- [Get-Азсклдатабаседатамаскингруле](https://docs.microsoft.com/powershell/module/az.sql/Get-AzSqlDatabaseDataMaskingRule)
- [New-Азсклдатабаседатамаскингруле](https://docs.microsoft.com/powershell/module/az.sql/New-AzSqlDatabaseDataMaskingRule)
- [Remove-Азсклдатабаседатамаскингруле](https://docs.microsoft.com/powershell/module/az.sql/Remove-AzSqlDatabaseDataMaskingRule)
- [Set-Азсклдатабаседатамаскингруле](https://docs.microsoft.com/powershell/module/az.sql/Set-AzSqlDatabaseDataMaskingRule)

## <a name="set-up-dynamic-data-masking-for-your-database-using-the-rest-api"></a>Настройка динамического маскирования данных для базы данных с помощью REST API

REST API можно использовать для программного управления политикой маскирования данных и правилами. Опубликованная REST API поддерживает следующие операции:

### <a name="data-masking-policies"></a>Политики маскирования данных

- [Создать или обновить](https://docs.microsoft.com/rest/api/sql/datamaskingpolicies/createorupdate): создает или обновляет политику маскирования данных базы данных.
- [Get](https://docs.microsoft.com/rest/api/sql/datamaskingpolicies/get): получает политику маскирования данных базы данных. 

### <a name="data-masking-rules"></a>Правила маскирования данных

- [Создать или обновить](https://docs.microsoft.com/rest/api/sql/datamaskingrules/createorupdate): создает или обновляет правило маскирования данных в базе данных.
- [Список по базе данных](https://docs.microsoft.com/rest/api/sql/datamaskingrules/listbydatabase): Возвращает список правил маскирования данных базы данных.
