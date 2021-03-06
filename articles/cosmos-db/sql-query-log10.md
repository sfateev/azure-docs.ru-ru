---
title: LOG10 на языке запросов Azure Cosmos DB
description: Сведения о системной функции LOG10 SQL в Azure Cosmos DB для возврата десятичного логарифма указанного числового выражения
author: ginamr
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 09/13/2019
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: 6869abb3040feb6431d60799536c9986c6ccb954
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2020
ms.locfileid: "88798250"
---
# <a name="log10-azure-cosmos-db"></a>LOG10 (Azure Cosmos DB)
 Возвращает десятичный логарифм от указанного числового выражения.  
  
## <a name="syntax"></a>Синтаксис
  
```sql
LOG10 (<numeric_expr>)  
```  
  
## <a name="arguments"></a>Аргументы
  
*numeric_expression*  
   Числовое выражение.  
  
## <a name="return-types"></a>Типы возвращаемых данных
  
  Возвращает числовое выражение.  
  
## <a name="remarks"></a>Комментарии
  
  Функции LOG10 и POWER находятся в обратной зависимости друг от друга. Например, 10 ^ LOG10(n) = n. Эта системная функция не будет использовать индекс.
  
## <a name="examples"></a>Примеры
  
  В примере ниже объявляется переменная и возвращается значение LOG10 указанной переменной (100).  
  
```sql
SELECT LOG10(100) AS log10 
```  
  
 Результирующий набор:  
  
```json
[{log10: 2}]  
```  

## <a name="next-steps"></a>Дальнейшие действия

- [Математические функции Azure Cosmos DB](sql-query-mathematical-functions.md)
- [Системные функции Azure Cosmos DB](sql-query-system-functions.md)
- [Знакомство со службой Azure Cosmos DB. API DocumentDB](introduction.md)
