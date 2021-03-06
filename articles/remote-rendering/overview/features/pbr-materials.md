---
title: Материалы PBR
description: Описывает тип материала PBR.
author: jakrams
ms.author: jakras
ms.date: 02/11/2020
ms.topic: article
ms.openlocfilehash: ad7fc7d9d02cd9a9a6fe74534a7c674fe0ac778d
ms.sourcegitcommit: b437bd3b9c9802ec6430d9f078c372c2a411f11f
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2020
ms.locfileid: "91893260"
---
# <a name="pbr-materials"></a>Материалы PBR

*Материалы* типа "PBR" — это один из поддерживаемых [типов материалов](../../concepts/materials.md) в службе удаленной подготовки Azure. Они используются для [сеток](../../concepts/meshes.md) , которые должны получить реалистичное освещение.

PBR означает **P**хисикалли **B**АСЕД **R**ендеринг и означает, что материал описывает визуальные свойства поверхности физически правдоподобные способом, чтобы реалистичные результаты были доступны при любых условиях освещения. Большинство современных игровых ядер и средств создания содержимого поддерживают PBR-материалы, так как они считаются наилучшей аппроксимацией реальных сценариев для отрисовки в режиме реального времени.

![Образец модели Helmet Глтф, отображаемый при помощи ARR](media/helmet.png)

Однако материалы по PBR не являются универсальными решениями. Существуют материалы, отражающие цвет по-разному в зависимости от угла обзора. Например, некоторые структуры или автомобиль закрашивается. Эти виды материалов не обрабатываются стандартной моделью PBR и сейчас не поддерживаются службой удаленной подготовки Azure. Сюда входят расширения PBR, такие как *тонкие пленки* (многоуровневые поверхности) и *четкие покрытие* (для рисования автомобилей).

## <a name="common-material-properties"></a>Общие свойства материала

Эти свойства являются общими для всех материалов:

* **албедоколор:** Этот цвет умножается на другие цвета, например *албедомап* или * :::no-loc text="vertex "::: цвета*. Если в материале включена *прозрачность* , альфа-канал используется для настройки непрозрачности, `1` что означает полностью прозрачное и абсолютное `0` значение. Значение по умолчанию — белый.

  > [!NOTE]
  > Если материал PBR является полностью прозрачным, как и полностью чистый фрагмент, он по-прежнему отражает среду. Яркие участки, такие как солнце, по-прежнему отображаются в отражении. Это отличается для [цветовых материалов](color-materials.md).

* **албедомап:** [Двухмерная текстура](../../concepts/textures.md) для значений албедо на пиксель.

* **алфаклипенаблед** и **Алфаклипсрешолд:** если *алфаклипенаблед* имеет значение true, все пиксели, где значение албедо Alpha меньше, чем *алфаклипсрешолд* , не будут выводиться. Альфа-обрезание можно использовать даже без включения прозрачности и гораздо быстрее для визуализации. Тем не менее, альфа обрезанные материалы по-прежнему медленнее отображаются, чем полностью непрозрачные материалы. По умолчанию альфа-обрезка отключена.

* **текстурекурдинатескале** и **текстурекурдинатеоффсет.** шкала умножается на координаты плоскости UV, к которой добавляется смещение. Можно использовать для растяжения и сдвига текстур. Масштаб по умолчанию — (1, 1), а смещение — (0, 0).

* **усевертексколор:** Если сетка содержит :::no-loc text="vertex"::: цвета и этот параметр включен, то :::no-loc text="vertex"::: цвет сеток умножается на *албедоколор* и *албедомап*. По умолчанию *усевертексколор* отключен.

* **исдаублесидед:** Если для параметра двусторонняя задано значение true, треугольники с этим материалом подготавливаются, даже если камера просматривает их задние грани. Для освещения «Освещение материалов» также правильно вычисляются для задних граней. По умолчанию этот параметр отключен. См. также [ :::no-loc text="Single-sided"::: Отображение](single-sided-rendering.md).

* **Транспаренцивритесдепс:** Если в материале установлен флаг Транспаренцивритесдепс, а материал является прозрачным, то объекты, использующие этот материал, также будут участвовать в конечном буфере глубины. См. флаг PBR Material *прозрачен* в следующем разделе. Включение этой функции рекомендуется, если Вашему варианту использования требуется более правдоподобные [позднее репроецирование](late-stage-reprojection.md) полностью прозрачных сцен. Для смешанных прозрачных и прозрачных сцен этот параметр может привести к появлению предполагающий поведения РЕПРОЕКЦИИ или репроектных артефактов. По этой причине значение по умолчанию и рекомендуемый вариант общего использования — отключить этот флаг. Записанные значения глубины взяты с уровня глубины на пиксель объекта, расположенного ближе всего к камере.

## <a name="pbr-material-properties"></a>Свойства "PBR Material"

Основная идея физической отрисовки заключается в использовании свойств *басеколор*, *металлического*и *грубого* представления для имитации широкого спектра реальных материалов. Подробное описание параметра PBR выходит за рамки этой статьи. Дополнительные сведения о параметре PBR см. в разделе [другие источники](http://www.pbr-book.org). Следующие свойства относятся к материалам PBR:

* **басеколор:** В материалах PBR *Цвет албедо* называется *базовым цветом*. В удаленной отрисовке Azure свойство *цвета албедо* уже присутствует через общие свойства материала, поэтому дополнительное свойство базового цвета отсутствует.

* «нечеткость» и « **раугхнессмап:** грубая **степень» определяет** , насколько грубая или гладкая поверхность. Приблизительные поверхности разбросают освещение в большем направлении, чем гладкие поверхности, что делает отрезки размытия, а не резки. Диапазон значений — от `0.0` до `1.0` . Если значение `roughness` равно `0.0` , отражения будут очень четкими. При `roughness` значении Equals `0.5` отражение станет размытым.

  Если указаны как значение грубости, так и таблица грубых значений, то окончательное значение будет являться произведением двух.

* **металлическое** и **металнессмапе:** в физике это свойство соответствует тому, является ли поверхность назначением или электрическим. У обучающих материалов разные отражающие свойства, и они, как правило, могут быть отражены без албедо цвета. В материалах PBR это свойство влияет на то, насколько поверхность отражает окружающую среду. Диапазон значений — от `0.0` до `1.0` . Когда состояние металла имеет значение `0.0` , цвет албедо будет полностью виден, а материал будет выглядеть как пластик или церамикс. При наличии металлического металла `0.5` он выглядит как окрашенный металл. Когда металлическая `1.0` , поверхность почти полностью теряет свой цвет албедо и отражает только окружающую окружение. Например, если `metalness` имеет значение `1.0` и, то `roughness` `0.0` поверхность выглядит как фактическое зеркало.

  Если передаются и значение металла, и схема металла, Последнее значение будет являться произведением двух.

  ![Шарик, отображаемые с различными значениями металлического и грубого](./media/metalness-roughness.png)

  На рисунке выше сфера в правом нижнем углу выглядит как реальный материал, Нижняя левая часть выглядит как церамик или пластик. Цвет албедо также изменяется в соответствии с физическими свойствами. При увеличении неровности материал теряет четкость отражения.

* **нормалмап:** Для имитации детализированной детализации можно предоставить [нормальную карту](https://en.wikipedia.org/wiki/Normal_mapping) .

* **окклусионмап** и **аоскале:** [внешний перекрытия](https://en.wikipedia.org/wiki/Ambient_occlusion) делает объекты с кревицес более реалистичными, добавляя тени в перекрыто области. Значение перекрытия в диапазоне от `0.0` до `1.0` , где `0.0` означает затемнение (перекрыто) и `1.0` означает отсутствие окклусионс. Если в качестве перекрытияной схемы указана двухмерная текстура, то этот результат будет включен, а *аоскале* выступает в качестве множителя.

  ![Объект, отображаемый с окружающими перекрытия и без него](./media/boom-box-ao2.gif)

* **прозрачный:** Для PBR-материалов существует только один параметр прозрачности: он включен или нет. Непрозрачность определяется альфа-каналом цвета албедо. Если этот флажок включен, вызывается более сложный конвейер отрисовки для рисования полупрозрачных поверхностей. Удаленная визуализация Azure реализует [независимую от порядка прозрачность](https://en.wikipedia.org/wiki/Order-independent_transparency) (OIT).

  Прозрачная геометрия является дорогостоящей для визуализации. Если вам нужны только отверстия на поверхности, например для конечных элементов дерева, лучше использовать альфа-обрезку.

  ![Шарик отображается с нулевым ](./media/transparency.png) уведомлением о прозрачности на изображении выше, то, как самая правая сфера полностью прозрачна, но отражение остается видимым.

  > [!IMPORTANT]
  > Если какой-либо материал должен быть переключен от непрозрачного на прозрачный во время выполнения, [модуль подготовки отчетов должен использовать режим рендеринга](../../concepts/rendering-modes.md) *тилебаседкомпоситион* . Это ограничение не относится к материалам, которые преобразуются в виде прозрачных материалов, начинающихся с.

## <a name="technical-details"></a>Технические сведения

Удаленная визуализация Azure использует Cook-Torrance Micro-Facet БРДФ с ГГКС NDF, Счликк Френелю, а также сопоставленное с ГГКС Смит условие видимости с Ламбертаным термином "подсветка". В настоящее время эта модель представляет собой промышленный стандарт де-факто. Более подробные сведения см. в этой статье: [физическая визуализация — Торранце Кука](http://www.codinglabs.net/article_physically_based_rendering_cook_torrance.aspx)

 Альтернативой *модели PBR,* используемой в удаленной отрисовке Azure, является модель PBR с *отраженным глянца* . Эта модель может представлять более широкий спектр материалов. Однако это более дорого и обычно не работает для случаев в реальном времени.
Не всегда можно выполнить преобразование из *блика-глянца* в *металлическую* , так как существуют пары значений *(диффузные, отражающие)* , которые не могут быть преобразованы в *(басеколор, металл)*. Преобразование в другом направлении проще и точнее, так как все пары *(басеколор, металлы)* соответствуют четко определенным *(диффузию, отражающим)* парам.

## <a name="api-documentation"></a>Документирование API

* [Класс C# Пбрматериал](https://docs.microsoft.com/dotnet/api/microsoft.azure.remoterendering.pbrmaterial)
* [C# Ремотеманажер. Креатематериал ()](https://docs.microsoft.com/dotnet/api/microsoft.azure.remoterendering.remotemanager.creatematerial)
* [Класс C++ Пбрматериал](https://docs.microsoft.com/cpp/api/remote-rendering/pbrmaterial)
* [C++ Ремотеманажер:: Креатематериал ()](https://docs.microsoft.com/cpp/api/remote-rendering/remotemanager#creatematerial)

## <a name="next-steps"></a>Дальнейшие шаги

* [Цветовые материалы](color-materials.md)
* [Текстуры](../../concepts/textures.md)
* [Сетки](../../concepts/meshes.md)
