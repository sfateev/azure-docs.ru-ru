---
title: Границы объектов
description: Объяснение того, как можно запрашивать границы пространственных объектов
author: florianborn71
ms.author: flborn
ms.date: 02/03/2020
ms.topic: conceptual
ms.custom: devx-track-csharp
ms.openlocfilehash: bd2b1026f69b5ec5814124a92d6a78d7f0225143
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2020
ms.locfileid: "89613820"
---
# <a name="object-bounds"></a>Границы объектов

Границы объектов представляют том, занятый [сущностью](entities.md) и ее дочерними элементами. В удаленной отрисовке Azure границы объектов всегда задаются в виде *выровненных по оси ограничивающих прямоугольников* (AABB). Границы объектов могут находиться либо в *локальном пространстве*, либо в *мировом пространстве*. В любом случае они всегда выравниваются по оси. Это означает, что экстенты и объемы могут отличаться между локальным и мировым представлением пространства.

## <a name="querying-object-bounds"></a>Запросы границ объектов

Локальный AABB [сетки](meshes.md) можно запрашивать непосредственно из ресурса сетки. Эти границы можно преобразовать в локальное или мировое пространство сущности с помощью преобразования сущности.

Таким образом можно вычислить границы всей иерархии объектов, но для этого необходимо выполнить обход иерархии, запросить границы для каждой сетки и объединить их вручную. Эта операция утомительна и неэффективна.

Лучше будет вызвать `QueryLocalBoundsAsync` или `QueryWorldBoundsAsync` на сущности. Тогда вычисления передаются на сервер и возвращаются с минимальной задержкой.

```cs
private BoundsQueryAsync _boundsQuery = null;

public void GetBounds(Entity entity)
{
    _boundsQuery = entity.QueryWorldBoundsAsync();
    _boundsQuery.Completed += (BoundsQueryAsync bounds) =>
    {
        if (bounds.IsRanToCompletion)
        {
            Double3 aabbMin = bounds.Result.min;
            Double3 aabbMax = bounds.Result.max;
            // ...
        }
    };
}
```

```cpp
void GetBounds(ApiHandle<Entity> entity)
{
    ApiHandle<BoundsQueryAsync> boundsQuery = *entity->QueryWorldBoundsAsync();
    boundsQuery->Completed([](ApiHandle<BoundsQueryAsync> bounds)
    {
        if (bounds->GetIsRanToCompletion())
        {
            Double3 aabbMin = bounds->GetResult().min;
            Double3 aabbMax = bounds->GetResult().max;
            // ...
        }
    });
}
```

## <a name="api-documentation"></a>Документирование API

* [C# Entity. Куерилокалбаундсасинк](https://docs.microsoft.com/dotnet/api/microsoft.azure.remoterendering.entity.querylocalboundsasync)
* [Сущность C++:: Куерилокалбаундсасинк](https://docs.microsoft.com/cpp/api/remote-rendering/entity#querylocalboundsasync)

## <a name="next-steps"></a>Дальнейшие действия

* [Пространственные запросы](../overview/features/spatial-queries.md)
