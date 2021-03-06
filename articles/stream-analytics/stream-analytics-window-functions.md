---
title: Общие сведения о функции управления окнами Azure Stream Analytics
description: В этой статье описаны четыре функции управления окнами ("переворачивающееся", "прыгающее", "скользящее" и "сеансовое" окно), которые используются в заданиях Azure Stream Analytics.
author: jseb225
ms.author: jeanb
ms.reviewer: mamccrea
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 09/16/2020
ms.openlocfilehash: 4c8d2143d2b6e18de2669a6b45961e601cc394bb
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2020
ms.locfileid: "91707563"
---
# <a name="introduction-to-stream-analytics-windowing-functions"></a>Общие сведения о функциях управления окнами Stream Analytics

При использовании потоковой передачи в реальном времени необходимо выполнять операции с теми данными, которые содержатся во временных окнах. В Stream Analytics имеется встроенная поддержка функций управления окнами. Это позволяет разработчикам выполнять сложные задания по обработке потоков с минимальными усилиями.

Существует пять видов временных окон: [**"переворачивающегося"**](https://docs.microsoft.com/stream-analytics-query/tumbling-window-azure-stream-analytics), [**прыгающее»**](https://docs.microsoft.com/stream-analytics-query/hopping-window-azure-stream-analytics), [**скользящий**](https://docs.microsoft.com/stream-analytics-query/sliding-window-azure-stream-analytics), [**сеанс**](https://docs.microsoft.com/stream-analytics-query/session-window-azure-stream-analytics)и [**снимок**](https://docs.microsoft.com/stream-analytics-query/snapshot-window-azure-stream-analytics) окна.  Функции управления окнами используются в предложении [**GROUP BY**](https://docs.microsoft.com/stream-analytics-query/group-by-azure-stream-analytics) в синтаксических конструкциях запросов в заданиях Stream Analytics. Можно также выполнять статистическую обработку событий в нескольких окнах с помощью [функции **Windows ()** ](https://docs.microsoft.com/stream-analytics-query/windows-azure-stream-analytics).

Все операции [управления окнами](https://docs.microsoft.com/stream-analytics-query/windowing-azure-stream-analytics) выводят результаты в **конце** окна. Обратите внимание, что при запуске задания Stream Analytics можно указать *время начала вывода задания* , и система будет автоматически получать предыдущие события во входящих потоках для вывода первого окна в указанное время. Например, если начать с параметра *Now* , он начнет немедленно выдавать данные. Результатом для окна будет единичное событие, полученное на основе выбранной статистической функции. Метка времени для выходного события соответствует времени завершения окна, и все функции управления окнами выполняются с фиксированной длительностью. 

![Концепции функций управления окнами в Stream Analytics](media/stream-analytics-window-functions/stream-analytics-window-functions-conceptual.png)

## <a name="tumbling-window"></a>"Переворачивающееся" окно
Функции "переворачивающегося" окна используются для разделения потока данных на отдельные сегменты времени, для которых и выполняются эти функции, как показано в примере ниже. "Переворачивающееся" окно характеризуется тем, что такие окна повторяются и не перекрываются, а любое событие может принадлежать только к одному "переворачивающемуся" окну.

!["Переворачивающееся" окно Stream Analytics](media/stream-analytics-window-functions/stream-analytics-window-functions-tumbling-intro.png)

## <a name="hopping-window"></a>"Прыгающее" окно
Функции "прыгающего" окна смещаются вперед на фиксированный отрезок времени. Их можно легко представить как "переворачивающегося" окна, которые могут перекрываться и выдаваться чаще, чем размер окна. События могут принадлежать более чем к одному результирующему набору окон прыгающее». Если для "прыгающего" окна указать длину прыжка, точно совпадающую с размером окна, оно будет полностью идентично "переворачивающемуся" окну. 

!["Прыгающее" окно Stream Analytics](media/stream-analytics-window-functions/stream-analytics-window-functions-hopping-intro.png)

## <a name="sliding-window"></a>"Скользящее" окно

Скользящие окна, в отличие от окон "переворачивающегося" или прыгающее», выводят события только для точек во времени при фактическом изменении содержимого окна. Иными словами, когда событие входит в окно или выходит из него. Каждое окно имеет по крайней мере одно событие, как в случае прыгающее» окон, события могут принадлежать к более чем одному скользящему окну.

!["Скользящее" окно Stream Analytics](media/stream-analytics-window-functions/stream-analytics-window-functions-sliding-intro.png)

## <a name="session-window"></a>"Сеансовое" окно
Функции "сеансового" окна группируют события, поступающие одновременно, отфильтровывая периоды времени, в течение которых данные отсутствовали. У такого окна три основных параметра: время ожидания, максимальная длительность и ключ секционирования (необязательно).

!["Сеансовое" окно Stream Analytics](media/stream-analytics-window-functions/stream-analytics-window-functions-session-intro.png)

"Сеансовое" окно начинается, когда происходит первое событие. Если в течение указанного времени ожидания после последнего полученного события происходит еще одно событие, то окно расширяется для включения этого нового события. В противном случае, если в течение времени ожидания события отсутствуют, то по истечении этого времени окно закрывается.

Если события непрерывно происходят в течение указанного времени ожидания, то "сеансовое" окно будет расширяться, пока не будет достигнута максимальная длительность. Интервалы проверки максимальной продолжительности задаются равными указанной максимальной длительности. Например, если максимальная длительность равна 10, то проверка превышения максимальной длительности окна будет выполняться при t = 0, 10, 20, 30 и т. д.

Если указан ключ секции, то события группируются по ключу, и "сеансовое" окно применяется отдельно к каждой группе. Такое секционирование удобно в случаях, когда требуются разные "сеансовые" окна для различных пользователей или устройств.

## <a name="snapshot-window"></a>окно моментального снимка;

Окна моментального снимка группирует события, имеющие одинаковую метку времени. В отличие от других типов окон, для которых требуется определенная функция окна (например, [сессионвиндов ()](https://docs.microsoft.com/stream-analytics-query/session-window-azure-stream-analytics), можно применить окно моментального снимка, добавив System. timestamp () в предложение GROUP BY.

![Окно моментального снимка Stream Analytics](media/stream-analytics-window-functions/snapshot.png)

## <a name="next-steps"></a>Дальнейшие действия
* [Введение в Azure Stream Analytics](stream-analytics-introduction.md)
* [Приступая к работе с Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md)
* [Масштабирование заданий в службе Azure Stream Analytics](stream-analytics-scale-jobs.md)
* [Справочник по языку запросов Azure Stream Analytics](https://docs.microsoft.com/stream-analytics-query/stream-analytics-query-language-reference)
* [Справочник по API-интерфейсу REST управления Stream Analytics](https://msdn.microsoft.com/library/azure/dn835031.aspx)

