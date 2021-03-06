---
title: 'Применение преобразования: ссылка на модуль'
titleSuffix: Azure Machine Learning
description: Узнайте, как использовать модуль "применить преобразование" в Машинное обучение Azure для изменения входного набора данных на основе ранее вычисленного преобразования.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: likebupt
ms.author: keli19
ms.date: 06/05/2020
ms.openlocfilehash: 7573abbbee479bfb0d1710beba3b95d084a5e657
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2020
ms.locfileid: "90898870"
---
# <a name="apply-transformation-module"></a>Модуль применения преобразования

В этой статье описывается модуль в конструкторе Машинное обучение Azure.

Этот модуль используется для изменения входного набора данных на основе ранее вычисленного преобразования.

Например, если вы использовали z-баллы для нормализации обучающих данных с помощью модуля **нормализация данных** , необходимо использовать значение z-оценки, которое было вычислено для обучения на этапе оценки. В Машинное обучение Azure можно сохранить метод нормализации как преобразование, а затем применить **Преобразование** для применения z-оценки к входным данным до оценки.

## <a name="how-to-save-transformations"></a>Сохранение преобразований

Конструктор позволяет сохранять данные в виде **наборов** данных, чтобы их можно было использовать в других конвейерах.

1. Выберите модуль преобразования данных, который был успешно выполнен.

1. Перейдите на вкладку **выходы и журналы** .

1. Найдите выход преобразования и выберите **набор данных зарегистрировать** , чтобы сохранить его как модуль в разделе **наборы данных** в палитре модулей.

## <a name="how-to-use-apply-transformation"></a>Использование преобразования «применить преобразование»  
  
1. Добавьте модуль **Применить преобразование** в конвейер. Этот модуль можно найти в разделе **Оценка модели & оценки** палитры модулей. 
  
1. Найдите сохраненное преобразование, которое нужно использовать, в разделе **наборы данных** в палитре модулей.

1. Подключите выходные данные сохраненного преобразования к левому порту ввода модуля **Применить преобразование** .

    Набор данных должен иметь одну и ту же схему (число столбцов, имена столбцов, типы данных) в качестве набора данных, для которого было изначально создано преобразование.  
  
1. Подключите выходные данные требуемого модуля к правому порту ввода модуля **Применить преобразование** .
  
1. Чтобы применить преобразование к новому набору данных, запустите конвейер.  

## <a name="next-steps"></a>Дальнейшие действия

Ознакомьтесь с [набором доступных модулей](module-reference.md) в службе Машинного обучения Azure. 