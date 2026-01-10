---
title: "sass:color"
icon: lucide/paintbrush
---

## adjust

```scss
color.adjust($color,
  $red: null, $green: null, $blue: null,
  $hue: null, $saturation: null, $lightness: null,
  $whiteness: null, $blackness: null,
  $x: null, $y: null, $z: null,
  $chroma: null,
  $alpha: null,
  $space: null)
adjust-color(...) //=> цвет
```

Увеличивает или уменьшает один или несколько каналов `$color` на фиксированную величину.

Добавляет значение, переданное для каждого именованного аргумента, к соответствующему каналу цвета и возвращает скорректированный цвет. По умолчанию можно изменять только каналы в цветовом пространстве `$color`, но можно передать другое пространство в `$space`, чтобы изменять каналы уже в нем. Всегда возвращает цвет в том же пространстве, что и `$color`.

!!! tip "Совет"

    По историческим причинам, если `$color` находится в [устаревшем цветовом пространстве](../values/colors#legacy-color-spaces), можно изменять *любые* каналы устаревших пространств. Однако возникает ошибка, если указать RGB‑канал (`$red`, `$green` и/или `$blue`) одновременно с HSL‑каналом (`$hue`, `$saturation` и/или `$lightness`), либо любые из них одновременно с каналом [HWB](https://en.wikipedia.org/wiki/HWB_color_model) (`$hue`, `$whiteness` и/или `$blackness`).

    Даже в этом случае рекомендуется явно передавать `$space` и для устаревших цветов.

Все аргументы каналов должны быть числами и иметь те единицы, которые допустимо передавать в конструктор соответствующего цветового пространства. Если сумма текущего значения канала и смещения выходит за нативный диапазон канала, значение ограничивается этим диапазоном для:

* красного, зелёного и синего каналов в пространстве `rgb`;
* канала яркости в пространствах `lab`, `lch`, `oklab` и `oklch`;
* нижней границы каналов насыщенности и цветности в пространствах `hsl`, `lch` и `oklch`;
* альфа‑канала во всех пространствах.

См. также:

* [`color.scale()`](#scale) для плавного масштабирования свойств цвета.
* [`color.change()`](#change) для установки свойств цвета.

=== "SCSS"

    ```scss
    @use 'sass:color';

    @debug color.adjust(#6b717f, $red: 15); // #7a717f
    @debug color.adjust(lab(40% 30 40), $lightness: 10%, $a: -20); // lab(50% 10 40)
    @debug color.adjust(#d2e1dd, $hue: 45deg, $space: oklch);
    // rgb(209.7987626149, 223.8632000471, 229.3988769575)
    ```

=== "SASS"

    ```sass
    @use 'sass:color'

    @debug color.adjust(#6b717f, $red: 15)  // #7a717f
    @debug color.adjust(lab(40% 30 40), $lightness: 10%, $a: -20)  // lab(50% 10 40)
    @debug color.adjust(#d2e1dd, $hue: 45deg, $space: oklch)
    // rgb(209.7987626149, 223.8632000471, 229.3988769575)
    ```

## change

```scss
color.change($color,
  $red: null, $green: null, $blue: null,
  $hue: null, $saturation: null, $lightness: null,
  $whiteness: null, $blackness: null,
  $x: null, $y: null, $z: null,
  $chroma: null,
  $alpha: null,
  $space: null)
change-color(...) //=> цвет
```

Устанавливает один или несколько каналов цвета в новые значения.

Использует значение, переданное для каждого именованного аргумента, вместо соответствующего канала цвета и возвращает изменённый цвет. По умолчанию можно изменять только каналы в пространстве `$color`, но можно передать другое цветовое пространство как `$space`, чтобы изменять каналы в нем. Всегда возвращает цвет в том же пространстве, что и `$color`.

!!! tip "Совет"

    По историческим причинам, если `$color` находится в [устаревшем цветовом пространстве](../values/colors#legacy-color-spaces), можно изменять *любые* каналы устаревших пространств. Однако возникает ошибка при указании RGB-канала (`$red`, `$green` и/или `$blue`) одновременно с HSL-каналом (`$hue`, `$saturation` и/или `$lightness`), либо любых из них одновременно с каналом [HWB](https://en.wikipedia.org/wiki/HWB_color_model) (`$hue`, `$whiteness` и/или `$blackness`).

    Даже в этом случае рекомендуется явно передавать `$space` и для устаревших цветов.

Все аргументы каналов должны быть числами и иметь те единицы измерения, которые допустимы в конструкторе соответствующего цветового пространства. Каналы никогда не обрезаются для `color.change()`.

См. также:

* [`color.scale()`](#scale) для плавного масштабирования свойств цвета.
* [`color.adjust()`](#adjust) для изменения свойств цвета на фиксированные величины.

=== "SCSS"

    ```scss
    @use 'sass:color';

    @debug color.change(#6b717f, $red: 100); // #64717f
    @debug color.change(color(srgb 0 0.2 0.4), $red: 0.8, $blue: 0.1);
    // color(srgb 0.8 0.2 0.1)
    @debug color.change(#998099, $lightness: 30%, $space: oklch);
    // rgb(58.0719961509, 37.2631531594, 58.4201613409)
    ```

=== "SASS"

    ```sass
    @use 'sass:color'

    @debug color.change(#6b717f, $red: 100)  // #64717f
    @debug color.change(color(srgb 0 0.2 0.4), $red: 0.8, $blue: 0.1)
    // color(srgb 0.8 0.2 0.1)
    @debug color.change(#998099, $lightness: 30%, $space: oklch)
    // rgb(58.0719961509, 37.2631531594, 58.4201613409)
    ```

## channel

```scss
color.channel($color, $channel, $space: null) //=> число
```

Возвращает значение канала `$channel` в пространстве `$space`, которое по умолчанию равно цветовому пространству `$color`. `$channel` должен быть строкой в кавычках, а `$space` — строкой без кавычек.

Возвращает число с единицей `deg` для канала `hue` в пространствах `hsl`, `hwb`, `lch` и `oklch`. Возвращает число с единицей `%` для каналов `saturation`, `lightness`, `whiteness` и `blackness` в пространствах `hsl`, `hwb`, `lab`, `lch`, `oklab` и `oklch`. Для всех остальных каналов возвращает число без единиц.

Возвращает `0` (возможно с соответствующей единицей), если канал `$channel` отсутствует в `$color`. Можно использовать [`color.is-missing()`](#is-missing) для явной проверки отсутствующих каналов.

=== "SCSS"

    ```scss
    @use 'sass:color';

    @debug color.channel(hsl(80deg 30% 50%), "hue"); // 80deg
    @debug color.channel(hsl(80deg 30% 50%), "hue", $space: oklch); // 124.279238779deg
    @debug color.channel(hsl(80deg 30% 50%), "red", $space: rgb); // 140.25
    ```

=== "SASS"

    ```sass
    @use 'sass:color'

    @debug color.channel(hsl(80deg 30% 50%), "hue") // 80deg
    @debug color.channel(hsl(80deg 30% 50%), "hue", $space: oklch) // 124.279238779deg
    @debug color.channel(hsl(80deg 30% 50%), "red", $space: rgb) // 140.25
    ```

## complement

```scss
color.complement($color, $space: null)
complement($color, $space: null) //=> цвет
```

Возвращает [комплементарный цвет](https://en.wikipedia.org/wiki/Complementary_colors) для `$color` в пространстве `$space`.

Поворачивает тон `$color` на `180deg` в пространстве `$space`. Это означает, что `$space` должно быть полярным цветовым пространством: `hsl`, `hwb`, `lch` или `oklch`. Всегда возвращает цвет в том же пространстве, что и `$color`.

!!! tip "Совет"

    По историческим причинам `$space` является опциональным, если `$color` находится в [устаревшем цветовом пространстве](../values/colors#legacy-color-spaces). В этом случае `$space` по умолчанию равно `hsl`. Тем не менее, всегда рекомендуется явно указывать `$space`.

=== "SCSS"

    ```scss
    @use 'sass:color';

    // HSL оттенок 222deg становится 42deg.
    @debug color.complement(#6b717f); // #7f796b

    // Oklch оттенок 267.1262408996deg становится 87.1262408996deg
    @debug color.complement(#6b717f, oklch);
    // rgb(118.8110604298, 112.5123650034, 98.1616586336)

    // Оттенок 70deg становится 250deg.
    @debug color.complement(oklch(50% 0.12 70deg), oklch); // oklch(50% 0.12 250deg)
    ```

=== "SASS"

    ```sass
    @use 'sass:color'

    // HSL оттенок 222deg становится 42deg.
    @debug color.complement(#6b717f) // #7f796b

    // Oklch оттенок 267.1262408996deg становится 87.1262408996deg
    @debug color.complement(#6b717f, oklch)
    // rgb(118.8110604298, 112.5123650034, 98.1616586336)

    // Оттенок 70deg становится 250deg.
    @debug color.complement(oklch(50% 0.12 70deg), oklch) // oklch(50% 0.12 250deg)
    ```

## grayscale

```scss
color.grayscale($color)
grayscale($color) //=> цвет
```

Возвращает серый цвет с той же яркостью, что и `$color`.

Если `$color` находится в [устаревшем цветовом пространстве](../values/colors#legacy-color-spaces), устанавливает HSL-насыщенность (saturation) в 0%. В противном случае устанавливает Oklch-цветность (chroma) в 0%.

=== "SCSS"

    ```scss
    @use 'sass:color';

    @debug color.grayscale(#6b717f); // #757575
    @debug color.grayscale(color(srgb 0.4 0.2 0.6)); // color(srgb 0.3233585271 0.3233585411 0.3233585792)
    @debug color.grayscale(oklch(50% 80% 270deg)); // oklch(50% 0% 270deg)
    ```

=== "SASS"

    ```sass
    @use 'sass:color'

    @debug color.grayscale(#6b717f) // #757575
    @debug color.grayscale(color(srgb 0.4 0.2 0.6)) // color(srgb 0.3233585271 0.3233585411 0.3233585792)
    @debug color.grayscale(oklch(50% 80% 270deg)) // oklch(50% 0% 270deg)
    ```

## ie-hex-str

```scss
color.ie-hex-str($color)
ie-hex-str($color) //=> строка без кавычек
```

Возвращает строку без кавычек, представляющую `$color` в формате `#AARRGGBB`, ожидаемом свойством [`-ms-filter`](https://learn.microsoft.com/en-us/previous-versions/ms530752(v=vs.85)) в Internet Explorer.

Если `$color` ещё не находится в цветовом пространстве `rgb`, он конвертируется в `rgb` и при необходимости выполняется приведение к гамуту (подгонка под диапазон доступных цветов). Конкретный алгоритм приведения к гамуту может измениться в будущих версиях Sass по мере развития технологий; в настоящее время используется [`local-minde`](#to-gamut).

=== "SCSS"

    ```scss
    @use 'sass:color';

    @debug color.ie-hex-str(#b37399); // #FFB37399
    @debug color.ie-hex-str(rgba(242, 236, 228, 0.6)); // #99F2ECE4
    @debug color.ie-hex-str(oklch(70% 10% 120deg)); // #FF9BA287
    ```

=== "SASS"

    ```sass
    @use 'sass:color'

    @debug color.ie-hex-str(#b37399) // #FFB37399
    @debug color.ie-hex-str(rgba(242, 236, 228, 0.6)) // #99F2ECE4
    @debug color.ie-hex-str(oklch(70% 10% 120deg)) // #FF9BA287
    ```

## invert

```scss
color.invert($color, $weight: 100%, $space: null)
invert($color, $weight: 100%, $space: null) //=> цвет
```

Возвращает инверсию или [негатив](https://en.wikipedia.org/wiki/Negative_(photography)) цвета `$color` в пространстве `$space`.

Значение `$weight` должно быть числом от `0%` до `100%` (включительно). Большее значение означает, что результат будет ближе к негативу, а меньшее — ближе к `$color`. Значение `50%` всегда дает серый цвет средней яркости в пространстве `$space`.

!!! tip "Совет"

    По историческим причинам `$space` является опциональным, если `$color` находится в [устаревшем цветовом пространстве](../values/colors#legacy-color-spaces). В этом случае `$space` по умолчанию равно собственному пространству `$color`. Тем не менее, всегда рекомендуется явно указывать `$space`.

=== "SCSS"

    ```scss
    @use 'sass:color';

    @debug color.invert(#b37399, $space: rgb); // #4c8c66
    @debug color.invert(#550e0c, 20%, $space: display-p3); // rgb(103.4937692017, 61.3720912206, 59.430641338)
    ```

=== "SASS"

    ```sass
    @use 'sass:color';

    @debug color.invert(#b37399, $space: rgb) // #4c8c66
    @debug color.invert(#550e0c, 20%, $space: display-p3) // rgb(103.4937692017, 61.3720912206, 59.430641338)
    ```

## is-legacy

```scss
color.is-legacy($color) //=> булево значение
```

Возвращает, находится ли `$color` в [устаревшем цветовом пространстве](../values/colors#legacy-color-spaces).

=== "SCSS"

    ```scss
    @use 'sass:color';

    @debug color.is-legacy(#b37399); // true
    @debug color.is-legacy(hsl(90deg 30% 90%)); // true
    @debug color.is-legacy(oklch(70% 10% 120deg)); // false
    ```

=== "SASS"

    ```sass
    @use 'sass:color'

    @debug color.is-legacy(#b37399) // true
    @debug color.is-legacy(hsl(90deg 30% 90%)) // true
    @debug color.is-legacy(oklch(70% 10% 120deg)) // false
    ```

## is-missing

```scss
color.is-missing($color, $channel) //=> булево значение
```

Возвращает, является ли канал `$channel` [отсутствующим](../values/colors#missing-channels) в `$color`. `$channel` должен быть строкой в кавычках.

=== "SCSS"

    ```scss
    @use 'sass:color';

    @debug color.is-missing(#b37399, "green"); // false
    @debug color.is-missing(rgb(100 none 200), "green"); // true
    @debug color.is-missing(color.to-space(grey, lch), "hue"); // true
    ```

=== "SASS"

    ```sass
    @use 'sass:color'

    @debug color.is-legacy(#b37399) // true
    @debug color.is-legacy(hsl(90deg 30% 90%)) // true
    @debug color.is-legacy(oklch(70% 10% 120deg)) // false
    ```

## is-powerless

```scss
color.is-powerless($color, $channel, $space: null) //=> булево значение
```

Возвращает, является ли канал `$channel` цвета `$color` [неэффективным](../values/colors#powerless-channels) в пространстве `$space`, которое по умолчанию равно пространству `$color`. `$channel` должен быть строкой в кавычках, а `$space` — строкой без кавычек.

Каналы считаются неэффективными в следующих случаях:

* В пространстве `hsl` канал `hue` неэффективен, если `saturation` равна 0%.
* В пространстве `hwb` канал `hue` неэффективен, если сумма `whiteness` и `blackness` превышает 100%.
* В пространствах `lch` и `oklch` канал `hue` неэффективен, если `chroma` равна 0%.

=== "SCSS"

    ```scss
    @use 'sass:color';

    @debug color.is-powerless(hsl(180deg 0% 40%), "hue"); // true
    @debug color.is-powerless(hsl(180deg 0% 40%), "saturation"); // false
    @debug color.is-powerless(#999, "hue", $space: hsl); // true
    ```

=== "SASS"

    ```sass
    @use 'sass:color'

    @debug color.is-powerless(hsl(180deg 0% 40%), "hue") // true
    @debug color.is-powerless(hsl(180deg 0% 40%), "saturation") // false
    @debug color.is-powerless(#999, "hue", $space: hsl) // true
    ```

## mix

```scss
color.mix($color1, $color2, $weight: 50%, $method: null)
mix($color1, $color2, $weight: 50%, $method: null) //=> цвет
```

Возвращает цвет, являющийся смесью `$color1` и `$color2`, используя метод `$method`, который представляет собой название цветового пространства, опционально за которым следует [метод интерполяции оттенка](https://developer.mozilla.org/en-US/docs/Web/CSS/hue-interpolation-method), если это полярное цветовое пространство (`hsl`, `hwb`, `lch` или `oklch`).

Использует тот же алгоритм смешивания цветов, что и [CSS-функция `color-mix()`](https://developer.mozilla.org/en-US/docs/Web/CSS/color_value/color-mix). Это также означает, что если у любого из цветов есть [отсутствующий канал](../values/colors#missing-channels) в пространстве интерполяции, он примет соответствующее значение канала из другого цвета. Всегда возвращает цвет в пространстве `$color1`.

Значение `$weight` должно быть числом от `0%` до `100%` (включительно). Большее значение означает, что будет использовано больше `$color1`, а меньшее — больше `$color2`.

!!! info "Информация"

    По историческим причинам `$method` является опциональным, если `$color1` и `$color2` оба находятся в [устаревших цветовых пространствах](../values/colors#legacy-color-spaces). В этом случае смешивание цветов выполняется с использованием того же алгоритма, который Sass использовал исторически, где и `$weight`, и относительная прозрачность каждого цвета определяют, сколько каждого цвета будет в результате.

=== "SCSS"

    ```scss
    @use 'sass:color';

    @debug color.mix(#036, #d2e1dd, $method: rgb); // #698aa2
    @debug color.mix(#036, #d2e1dd, $method: oklch); // rgb(87.864037264, 140.601918773, 154.2876826946)
    @debug color.mix(
      color(rec2020 1 0.7 0.1),
      color(rec2020 0.8 none 0.3),
      $weight: 75%,
      $method: rec2020
    ); // color(rec2020 0.95 0.7 0.15)
    @debug color.mix(
      oklch(80% 20% 0deg),
      oklch(50% 10% 120deg),
      $method: oklch longer hue
    ); // oklch(65% 0.06 240deg)
    ```

=== "SASS"

    ```sass
    @use 'sass:color';

    @debug color.mix(#036, #d2e1dd, $method: rgb) // #698aa2
    @debug color.mix(#036, #d2e1dd, $method: oklch) // rgb(87.864037264, 140.601918773, 154.2876826946)
    @debug color.mix(color(rec2020 1 0.7 0.1), color(rec2020 0.8 none 0.3), $weight: 75%, $method: rec2020) // color(rec2020 0.95 0.7 0.15)

    @debug color.mix(oklch(80% 20% 0deg), oklch(50% 10% 120deg), $method: oklch longer hue) // oklch(65% 0.06 240deg)
    ```

## same

```scss
color.same($color1, $color2) //=> булево значение
```

Возвращает, визуально отображаются ли `$color1` и `$color2` как один и тот же цвет. В отличие от `==`, считает цвета эквивалентными, даже если они находятся в разных цветовых пространствах, при условии что они представляют одно и то же значение цвета в цветовом пространстве `xyz`. Рассматривает [отсутствующие каналы](../values/colors#missing-channels) как эквивалентные нулю.

=== "SCSS"

    ```scss
    @use 'sass:color';

    @debug color.same(#036, #036); // true
    @debug color.same(#036, #037); // false
    @debug color.same(#036, color.to-space(#036, oklch)); // true
    @debug color.same(hsl(none 50% 50%), hsl(0deg 50% 50%)); // true
    ```

=== "SASS"

    ```sass
    @use 'sass:color'

    @debug color.same(#036, #036) // true
    @debug color.same(#036, #037) // false
    @debug color.same(#036, color.to-space(#036, oklch)) // true
    @debug color.same(hsl(none 50% 50%), hsl(0deg 50% 50%)) // true
    ```

## scale

```scss
color.scale($color,
  $red: null, $green: null, $blue: null,
  $saturation: null, $lightness: null,
  $whiteness: null, $blackness: null,
  $x: null, $y: null, $z: null,
  $chroma: null,
  $alpha: null,
  $space: null)
scale-color(...) //=> цвет
```

Плавно масштабирует одно или несколько свойств цвета `$color`.

Каждый именованный аргумент должен быть числом от `-100%` до `100%` (включительно). Это указывает, насколько далеко соответствующее свойство должно быть перемещено от своей исходной позиции к максимуму (если аргумент положительный) или к минимуму (если аргумент отрицательный). Это означает, что, например, `$lightness: 50%` сделает все цвета на `50%` ближе к максимальной яркости, не делая их полностью белыми. По умолчанию может масштабировать только каналы в пространстве `$color`, но можно передать другое цветовое пространство в `$space`, чтобы масштабировать каналы в нем. Всегда возвращает цвет в том же пространстве, что и `$color`.

!!! tip "Совет"

    По историческим причинам, если `$color` находится в [устаревшем цветовом пространстве](../values/colors#legacy-color-spaces), можно масштабировать *любые* каналы устаревших пространств. Однако возникает ошибка при указании RGB-канала (`$red`, `$green` и/или `$blue`) одновременно с HSL-каналом (`$saturation` и/или `$lightness`), либо любых из них одновременно с каналом [HWB](https://en.wikipedia.org/wiki/HWB_color_model) (`$hue`, `$whiteness` и/или `$blackness`).

    Даже в этом случае рекомендуется явно передавать `$space` и для устаревших цветов.

См. также:

* [`color.adjust()`](#adjust) для изменения свойств цвета на фиксированные величины.
* [`color.change()`](#change) для установки свойств цвета.

=== "SCSS"

    ```scss
    @use 'sass:color';

    @debug color.scale(#6b717f, $red: 15%); // rgb(129.2, 113, 127)
    @debug color.scale(#d2e1dd, $lightness: -10%, $space: oklch);
    // rgb(181.2580722731, 195.8949200496, 192.0059024063)
    @debug color.scale(oklch(80% 20% 120deg), $chroma: 50%, $alpha: -40%);
    // oklch(80% 0.24 120deg / 0.6)
    ```

=== "SASS"

    ```sass
    @use 'sass:color'

    @debug color.scale(#6b717f, $red: 15%) // rgb(129.2, 113, 127)
    @debug color.scale(#d2e1dd, $lightness: -10%, $space: oklch)
    // rgb(181.2580722731, 195.8949200496, 192.0059024063)
    @debug color.scale(oklch(80% 20% 120deg), $chroma: 50%, $alpha: -40%)
    // oklch(80% 0.24 120deg / 0.6)
    ```

## space

```scss
color.space($color) //=> строка без кавычек
```

Возвращает название пространства цвета `$color` в виде строки без кавычек.

=== "SCSS"

    ```scss
    @use 'sass:color';

    @debug color.space(#036); // rgb
    @debug color.space(hsl(120deg 40% 50%)); // hsl
    @debug color.space(color(xyz-d65 0.1 0.2 0.3)); // xyz
    ```

=== "SASS"

    ```sass
    @use 'sass:color'

    @debug color.space(#036) // rgb
    @debug color.space(hsl(120deg 40% 50%)) // hsl
    @debug color.space(color(xyz-d65 0.1 0.2 0.3)) // xyz
    ```

## to-gamut

```scss
color.to-gamut($color, $space: null, $method: null) //=> цвет
```

Возвращает визуально похожий цвет для `$color` в пределах диапазона доступных цветов пространства `$space`, которое по умолчанию равно пространству `$color`. Если `$color` уже находится в пределах диапазона `$space`, возвращается без изменений. Всегда возвращает цвет в исходном пространстве `$color`. `$space` должно быть строкой без кавычек.

Параметр `$method` указывает, как Sass должен выбирать «похожий» цвет:

* `local-minde`: метод, рекомендованный спецификацией CSS Colors 4. Бинарный поиск в пространстве Oklch цветности до нахождения цвета, чье значение после подгонки под диапазон максимально близко к варианту с уменьшенной цветностью.

* `clip`: просто обрезает все каналы до границ диапазона `$space`, устанавливая минимальные или максимальные значения при выходе за пределы.

!!! note "Примечание"

    Рабочая группа CSS и разработчики браузеров продолжают активно обсуждать альтернативные варианты рекомендованного алгоритма подгонки под диапазон цветов. Пока не будет принято решение, параметр `$method` обязателен в `color.to-gamut()`, чтобы в будущем можно было сделать его значение по умолчанию таким же, как в CSS.

=== "SCSS"

    ```scss
    @use 'sass:color';

    @debug color.to-gamut(#036, $method: local-minde); // #036
    @debug color.to-gamut(oklch(60% 70% 20deg), $space: rgb, $method: local-minde);
    // oklch(61.2058838235% 0.2466052584 22.0773325274deg)
    @debug color.to-gamut(oklch(60% 70% 20deg), $space: rgb, $method: clip);
    // oklch(62.5026609544% 0.2528579741 24.1000466758deg)
    ```

=== "SASS"

    ```sass
    @use 'sass:color'

    @debug color.to-gamut(#036, $method: local-minde) // #036
    @debug color.to-gamut(oklch(60% 70% 20deg), $space: rgb, $method: local-minde)
    // oklch(61.2058838235% 0.2466052584 22.0773325274deg)
    @debug color.to-gamut(oklch(60% 70% 20deg), $space: rgb, $method: clip)
    // oklch(62.5026609544% 0.2528579741 24.1000466758deg)
    ```

## to-space

```scss
color.to-space($color, $space) //=> цвет
```

Конвертирует `$color` в указанное пространство `$space`, которое должно быть строкой без кавычек.

Если гамма исходного пространства `$color` шире гаммы `$space`, может вернуть цвет, выходящий за пределы гаммы `$space`. Для получения похожего цвета в пределах гаммы используйте [`color.to-gamut()`](#to-gamut).

Может создавать цвета с [отсутствующими каналами](../values/colors#missing-channels), если у `$color` есть [аналогичный отсутствующий канал](https://www.w3.org/TR/css-color-4/#analogous-components) или если канал [неэффективен](../values/colors#powerless-channels) в целевом пространстве. Для обеспечения совместимости с устаревшими браузерами при конвертации в устаревшие пространства новые отсутствующие каналы не создаются.

!!! note "Примечание"

    Это единственная функция Sass, которая возвращает цвет в другом пространстве, чем входное.

=== "SCSS"

    ```scss
    @use 'sass:color';

    @debug color.to-space(#036, display-p3); // lch(20.7457453073% 35.0389733355 273.0881809283deg)
    @debug color.to-space(oklab(44% 0.09 -0.13)); // rgb(103.1328911972, 50.9728091281, 150.8382311692)
    @debug color.to-space(xyz(0.8 0.1 0.1)); // color(a98-rgb 1.2177586808 -0.7828263424 0.3516847577)
    @debug color.to-space(grey, lch); // lch(53.5850134522% 0 none)
    @debug color.to-space(lch(none 10% 30deg), oklch); // oklch(none 0.3782382429 11.1889160032deg)
    ```

=== "SASS"

    ```sass
    @use 'sass:color'

    @debug color.to-space(#036, display-p3) // lch(20.7457453073% 35.0389733355 273.0881809283deg)
    @debug color.to-space(oklab(44% 0.09 -0.13)) // rgb(103.1328911972, 50.9728091281, 150.8382311692)
    @debug color.to-space(xyz(0.8 0.1 0.1)) // color(a98-rgb 1.2177586808 -0.7828263424 0.3516847577)
    @debug color.to-space(grey, lch) // lch(53.5850134522% 0 none)
    @debug color.to-space(lch(none 10% 30deg), oklch) // oklch(none 0.3782382429 11.1889160032deg)
    ```

## Устаревшие функции {#deprecated-functions}

### adjust-hue

```scss
adjust-hue($color, $degrees) //=> цвет
```

Увеличивает или уменьшает HSL-оттенок цвета `$color`.

Значение `$hue` должно быть числом от `-360deg` до `360deg` (включительно), которое добавляется к оттенку `$color`. Может быть [без единиц измерения](../values/numbers#units) или иметь любую угловую единицу. `$color` должен находиться в [устаревшем цветовом пространстве](../values/colors#legacy-color-spaces).

См. также [`color.adjust()`](#adjust), которая может изменять любое свойство цвета.

!!! tip "Совет"

    Поскольку `adjust-hue()` дублирует [`color.adjust()`](#adjust), она не включена напрямую в новую модульную систему. Вместо `adjust-hue($color, $amount)` используйте [`color.adjust($color, $hue: $amount, $space: hsl)`](#adjust).

=== "SCSS"

    ```scss
    // Оттенок 222deg становится 282deg.
    @debug adjust-hue(#6b717f, 60deg); // #796b7f

    // Оттенок 164deg становится 104deg.
    @debug adjust-hue(#d2e1dd, -60deg); // #d6e1d2

    // Оттенок 210deg становится 255deg.
    @debug adjust-hue(#036, 45); // #1a0066
    ```

=== "SASS"

    ```sass
    // Оттенок 222deg становится 282deg.
    @debug adjust-hue(#6b717f, 60deg) // #796b7f

    // Оттенок 164deg становится 104deg.
    @debug adjust-hue(#d2e1dd, -60deg) // #d6e1d2

    // Оттенок 210deg становится 255deg.
    @debug adjust-hue(#036, 45) // #1a0066
    ```

### alpha

```scss
color.alpha($color)
alpha($color)
opacity($color) //=> число
```

Возвращает альфа-канал `$color` в виде числа от 0 до 1.

`$color` должен находиться в [устаревшем цветовом пространстве](../values/colors#legacy-color-spaces).

В особом случае поддерживает синтаксис Internet Explorer `alpha(opacity=20)`, для которого возвращает [строку без кавычек](../values/strings#unquoted).

!!! tip "Совет"

    Поскольку `color.alpha()` дублирует [`color.channel()`](#channel), она больше не рекомендуется. Вместо `color.alpha($color)` используйте [`color.channel($color, "alpha")`](#channel).

=== "SCSS"

    ```scss
    @use 'sass:color';

    @debug color.alpha(#e1d7d2); // 1
    @debug color.opacity(rgb(210, 225, 221, 0.4)); // 0.4
    @debug alpha(opacity=20); // alpha(opacity=20)
    ```

=== "SASS"

    ```sass
    @use 'sass:color'

    @debug color.alpha(#e1d7d2) // 1
    @debug color.opacity(rgb(210, 225, 221, 0.4)) // 0.4
    @debug alpha(opacity=20) // alpha(opacity=20)
    ```

### blackness

```scss
color.blackness($color)
blackness($color) //=> число
```

Возвращает [HWB-черноту](https://en.wikipedia.org/wiki/HWB_color_model) цвета `$color` в виде числа от `0%` до `100%`.

`$color` должен находиться в [устаревшем цветовом пространстве](../values/colors#legacy-color-spaces).

!!! tip "Совет"

    Поскольку `color.blackness()` дублирует [`color.channel()`](#channel), она больше не рекомендуется. Вместо `color.blackness($color)` используйте [`color.channel($color, "blackness")`](#channel).

=== "SCSS"

    ```scss
    @use 'sass:color';

    @debug color.blackness(#e1d7d2); // 11.7647058824%
    @debug color.blackness(white); // 0%
    @debug color.blackness(black); // 100%
    ```

=== "SASS"

    ```sass
    @use 'sass:color'

    @debug color.blackness(#e1d7d2) // 11.7647058824%
    @debug color.blackness(white) // 0%
    @debug color.blackness(black) // 100%
    ```

### blue

```scss
color.blue($color)
blue($color) //=> число
```

Возвращает синий канал `$color` в виде числа от 0 до 255.

`$color` должен находиться в [устаревшем цветовом пространстве](../values/colors#legacy-color-spaces).

!!! tip "Совет"

    Поскольку `color.blue()` дублирует [`color.channel()`](#channel), она больше не рекомендуется. Вместо `color.blue($color)` используйте [`color.channel($color, "blue")`](#channel).

=== "SCSS"

    ```scss
    @use 'sass:color';

    @debug color.blue(#e1d7d2); // 210
    @debug color.blue(white); // 255
    @debug color.blue(black); // 0
    ```

=== "SASS"

    ```sass
    @use 'sass:color'

    @debug color.blue(#e1d7d2) // 210
    @debug color.blue(white) // 255
    @debug color.blue(black) // 0
    ```

### darken

```scss
darken($color, $amount) //=> цвет
```

Делает `$color` темнее.

`$color` должен находиться в [устаревшем цветовом пространстве](../values/colors#legacy-color-spaces).

`$amount` должен быть числом от `0%` до `100%` (включительно). Уменьшает яркость HSL `$color` на это значение.

!!! tip "Совет"

    Функция `darken()` уменьшает яркость на фиксированную величину, что часто не дает желаемого эффекта. Чтобы сделать цвет на определённый процент темнее, чем был изначально, используйте [`color.scale()`](#scale).

    Поскольку `darken()` обычно не является лучшим способом затемнения цвета, она не включена напрямую в новую модульную систему. Однако для сохранения существующего поведения `darken($color, $amount)` можно записать как [`color.adjust($color, $lightness: -$amount, $space: hsl)`](#adjust).

    === "SCSS"

        ```scss
        @use 'sass:color';

        // #036 имеет яркость 20%, поэтому при вычитании 30%
        // darken() возвращает черный.
        @debug darken(#036, 30%); // black

        // scale() вместо этого делает его на 30% темнее, чем был изначально.
        @debug color.scale(#036, $lightness: -30%); // #002447
        ```

    === "SASS"

        ```sass
        @use 'sass:color'

        // #036 имеет яркость 20%, поэтому при вычитании 30%
        // darken() возвращает черный.
        @debug darken(#036, 30%) // black

        // scale() вместо этого делает его на 30% темнее, чем был изначально.
        @debug color.scale(#036, $lightness: -30%) // #002447
        ```

=== "SCSS"

    ```scss
    // Яркость 92% становится 72%.
    @debug darken(#b37399, 20%); // #7c4465

    // Яркость 85% становится 45%.
    @debug darken(#f2ece4, 40%); // #b08b5a

    // Яркость 20% становится 0%.
    @debug darken(#036, 30%); // black
    ```

=== "SASS"

    ```sass
    // Яркость 92% становится 72%.
    @debug darken(#b37399, 20%) // #7c4465

    // Яркость 85% становится 45%.
    @debug darken(#f2ece4, 40%) // #b08b5a

    // Яркость 20% становится 0%.
    @debug darken(#036, 30%) // black
    ```

### desaturate

```scss
desaturate($color, $amount) //=> цвет
```

Делает `$color` менее насыщенным.

`$color` должен находиться в [устаревшем цветовом пространстве](../values/colors#legacy-color-spaces).

`$amount` должен быть числом от `0%` до `100%` (включительно). Уменьшает HSL-насыщенность `$color` на это значение.

!!! tip "Совет"

    Функция `desaturate()` уменьшает насыщенность на фиксированную величину, что часто не дает желаемого эффекта. Чтобы сделать цвет на определённый процент менее насыщенным, чем был изначально, используйте [`color.scale()`](#scale).

    Поскольку `desaturate()` обычно не является лучшим способом уменьшения насыщенности, она не включена напрямую в новую модульную систему. Однако для сохранения существующего поведения `desaturate($color, $amount)` можно записать как [`color.adjust($color, $saturation: -$amount, $space: hsl)`](#adjust).

    === "SCSS"

        ```scss
        @use 'sass:color';

        // #d2e1dd имеет насыщенность 20%, поэтому при вычитании 30%
        // desaturate() возвращает серый.
        @debug desaturate(#d2e1dd, 30%); // #dadada

        // scale() вместо этого делает его на 30% менее насыщенным, чем был изначально.
        @debug color.scale(#6b717f, $saturation: -30%); // #6e727c
        ```

    === "SASS"

        ```sass
        @use 'sass:color'

        // #d2e1dd имеет насыщенность 20%, поэтому при вычитании 30%
        // desaturate() возвращает серый.
        @debug desaturate(#d2e1dd, 30%)  // #dadada

        // scale() вместо этого делает его на 30% менее насыщенным, чем был изначально.
        @debug color.scale(#6b717f, $saturation: -30%) // #6e727c
        ```

=== "SCSS"

    ```scss
    // Насыщенность 100% становится 80%.
    @debug desaturate(#036, 20%); // #0a335c

    // Насыщенность 35% становится 15%.
    @debug desaturate(#f2ece4, 20%); // #eeebe8

    // Насыщенность 20% становится 0%.
    @debug desaturate(#d2e1dd, 30%); // #dadada
    ```

=== "SASS"

    ```sass
    // Насыщенность 100% становится 80%.
    @debug desaturate(#036, 20%)  // #0a335c

    // Насыщенность 35% становится 15%.
    @debug desaturate(#f2ece4, 20%)  // #eeebe8

    // Насыщенность 20% становится 0%.
    @debug desaturate(#d2e1dd, 30%)  // #dadada
    ```

### green

```scss
color.green($color)
green($color) //=> цвет
```

Возвращает зелёный канал `$color` в виде числа от 0 до 255.

`$color` должен находиться в [устаревшем цветовом пространстве](../values/colors#legacy-color-spaces).

!!! tip "Совет"

    Поскольку `color.green()` дублирует [`color.channel()`](#channel), она больше не рекомендуется. Вместо `color.green($color)` используйте [`color.channel($color, "green")`](#channel).

=== "SCSS"

    ```scss
    @use 'sass:color';

    @debug color.green(#e1d7d2); // 215
    @debug color.green(white); // 255
    @debug color.green(black); // 0
    ```

=== "SASS"

    ```sass
    @use 'sass:color'

    @debug color.green(#e1d7d2) // 215
    @debug color.green(white) // 255
    @debug color.green(black) // 0
    ```

### hue

```scss
color.hue($color)
hue($color) //=> число
```

Возвращает оттенок `$color` в виде числа от `0deg` до `360deg`.

`$color` должен находиться в [устаревшем цветовом пространстве](../values/colors#legacy-color-spaces).

!!! tip "Совет"

    Поскольку `color.hue()` дублирует [`color.channel()`](#channel), она больше не рекомендуется. Вместо `color.hue($color)` используйте [`color.channel($color, "hue")`](#channel).

=== "SCSS"

    ```scss
    @use 'sass:color';

    @debug color.hue(#e1d7d2); // 20deg
    @debug color.hue(#f2ece4); // 34.2857142857deg
    @debug color.hue(#dadbdf); // 228deg
    ```

=== "SASS"

    ```sass
    @use 'sass:color'

    @debug color.hue(#e1d7d2) // 20deg
    @debug color.hue(#f2ece4) // 34.2857142857deg
    @debug color.hue(#dadbdf) // 228deg
    ```

### lighten

```scss
lighten($color, $amount) //=> цвет
```

Делает `$color` светлее.

`$color` должен находиться в [устаревшем цветовом пространстве](../values/colors#legacy-color-spaces).

`$amount` должен быть числом от `0%` до `100%` (включительно). Увеличивает HSL-яркость `$color` на это значение.

!!! tip "Совет"

    Функция `lighten()` увеличивает яркость на фиксированную величину, что часто не дает желаемого эффекта. Чтобы сделать цвет на определённый процент светлее, чем был изначально, используйте [`color.scale()`](#scale).

    Поскольку `lighten()` обычно не является лучшим способом осветления цвета, она не включена напрямую в новую модульную систему. Однако для сохранения существующего поведения `lighten($color, $amount)` можно записать как [`color.adjust($color, $lightness: $amount, $space: hsl)`](#adjust).

    === "SCSS"

        ```scss
        @use 'sass:color';

        // #e1d7d2 имеет яркость 85%, поэтому при добавлении 30%
        // lighten() возвращает белый.
        @debug lighten(#e1d7d2, 30%); // white

        // scale() вместо этого делает его на 30% светлее, чем был изначально.
        @debug color.scale(#e1d7d2, $lightness: 30%); // #eae3e0
        ```

    === "SASS"

        ```sass
        @use 'sass:color'

        // #e1d7d2 имеет яркость 85%, поэтому при добавлении 30%
        // lighten() возвращает белый.
        @debug lighten(#e1d7d2, 30%)  // white

        // scale() вместо этого делает его на 30% светлее, чем был изначально.
        @debug color.scale(#e1d7d2, $lightness: 30%)  // #eae3e0
        ```

=== "SCSS"

    ```scss
    // Яркость 46% становится 66%.
    @debug lighten(#6b717f, 20%); // #a1a5af

    // Яркость 20% становится 80%.
    @debug lighten(#036, 60%); // #99ccff

    // Яркость 85% становится 100%.
    @debug lighten(#e1d7d2, 30%); // white
    ```

=== "SASS"

    ```sass
    // Яркость 46% становится 66%.
    @debug lighten(#6b717f, 20%) // #a1a5af

    // Яркость 20% становится 80%.
    @debug lighten(#036, 60%) // #99ccff

    // Яркость 85% становится 100%.
    @debug lighten(#e1d7d2, 30%) // white
    ```

### lightness

```scss
color.lightness($color)
lightness($color) //=> число
```

Возвращает HSL-яркость `$color` в виде числа от `0%` до `100%`.

`$color` должен находиться в [устаревшем цветовом пространстве](../values/colors#legacy-color-spaces).

!!! tip "Совет"

    Поскольку `color.lightness()` дублирует [`color.channel()`](#channel), она больше не рекомендуется. Вместо `color.lightness($color)` используйте [`color.channel($color, "lightness")`](#channel).

=== "SCSS"

    ```scss
    @use 'sass:color';

    @debug color.lightness(#e1d7d2); // 85.2941176471%
    @debug color.lightness(#f2ece4); // 92.1568627451%
    @debug color.lightness(#dadbdf); // 86.4705882353%
    ```

=== "SASS"

    ```sass
    @use 'sass:color'

    @debug color.lightness(#e1d7d2) // 85.2941176471%
    @debug color.lightness(#f2ece4) // 92.1568627451%
    @debug color.lightness(#dadbdf) // 86.4705882353%
    ```

### opacify

```scss
opacify($color, $amount)
fade-in($color, $amount) //=> цвет
```

Делает `$color` более непрозрачным.

`$color` должен находиться в [устаревшем цветовом пространстве](../values/colors#legacy-color-spaces).

`$amount` должен быть числом от `0` до `1` (включительно). Увеличивает альфа-канал `$color` на это значение.

!!! tip "Совет"

    Функция `opacify()` увеличивает альфа‑канал на фиксированную величину, что часто не дает желаемого эффекта. Чтобы сделать цвет на определённый процент более непрозрачным, чем он был раньше, используйте вместо этого [`scale()`](#scale).

    Поскольку `opacify()` обычно не является лучшим способом сделать цвет более непрозрачным, она не включена напрямую в новую модульную систему. Однако, если нужно сохранить имеющееся поведение, `opacify($color, $amount)` можно записать как [`color.adjust($color, $alpha: -$amount)`](#adjust).

    === "SCSS"

        ```scss
        @use 'sass:color';

        // rgba(#036, 0.7) имеет альфа-канал 0.7, поэтому при добавлении 0.3
        // opacify() возвращает полностью непрозрачный цвет.
        @debug opacify(rgba(#036, 0.7), 0.3); // #036

        // scale() вместо этого делает его на 30% более непрозрачным, чем был изначально.
        @debug color.scale(rgba(#036, 0.7), $alpha: 30%); // rgba(0, 51, 102, 0.79)
        ```

    === "SASS"

        ```sass
        @use 'sass:color'

        // rgba(#036, 0.7) имеет альфа-канал 0.7, поэтому при добавлении 0.3
        // opacify() возвращает полностью непрозрачный цвет.
        @debug opacify(rgba(#036, 0.7), 0.3) // #036

        // scale() вместо этого делает его на 30% более непрозрачным, чем был изначально.
        @debug color.scale(rgba(#036, 0.7), $alpha: 30%) // rgba(0, 51, 102, 0.79)
        ```

=== "SCSS"

    ```scss
    @debug opacify(rgba(#6b717f, 0.5), 0.2); // rgba(107, 113, 127, 0.7)
    @debug fade-in(rgba(#e1d7d2, 0.5), 0.4); // rgba(225, 215, 210, 0.9)
    @debug opacify(rgba(#036, 0.7), 0.3); // #036
    ```

=== "SASS"

    ```sass
    @debug opacify(rgba(#6b717f, 0.5), 0.2) // rgba(107, 113, 127, 0.7)
    @debug fade-in(rgba(#e1d7d2, 0.5), 0.4) // rgba(225, 215, 210, 0.9)
    @debug opacify(rgba(#036, 0.7), 0.3) // #036
    ```

### red

```scss
color.red($color)
red($color) //=> число
```

Возвращает красный канал `$color` в виде числа от 0 до 255.

`$color` должен находиться в [устаревшем цветовом пространстве](../values/colors#legacy-color-spaces).

!!! tip "Совет"

    Поскольку `color.red()` дублирует [`color.channel()`](#channel), она больше не рекомендуется. Вместо `color.red($color)` используйте [`color.channel($color, "red")`](#channel).

=== "SCSS"

    ```scss
    @use 'sass:color';

    @debug color.red(#e1d7d2); // 225
    @debug color.red(white); // 255
    @debug color.red(black); // 0
    ```

=== "SASS"

    ```sass
    @use 'sass:color'

    @debug color.red(#e1d7d2) // 225
    @debug color.red(white) // 255
    @debug color.red(black) // 0
    ```

### saturate

```scss
saturate($color, $amount) //=> цвет
```

Делает `$color` более насыщенным.

`$color` должен находиться в [устаревшем цветовом пространстве](../values/colors#legacy-color-spaces).

`$amount` должен быть числом от `0%` до `100%` (включительно). Увеличивает HSL-насыщенность `$color` на это значение.

!!! tip "Совет"

    Функция `saturate()` увеличивает насыщенность на фиксированную величину, что часто не дает желаемого эффекта. Чтобы сделать цвет на определённый процент более насыщенным, чем был изначально, используйте [`color.scale()`](#scale) вместо этого.

    Поскольку `saturate()` обычно не является лучшим способом повышения насыщенности, она не включена напрямую в новую модульную систему. Однако для сохранения существующего поведения `saturate($color, $amount)` можно записать как [`color.adjust($color, $saturation: $amount, $space: hsl)`](#adjust).

    === "SCSS"

        ```scss
        @use 'sass:color';

        // #0e4982 имеет насыщенность 80%, поэтому при добавлении 30%
        // saturate() становится полностью насыщенным.
        @debug saturate(#0e4982, 30%); // #004990

        // scale() вместо этого делает его на 30% более насыщенным, чем был изначально.
        @debug color.scale(#0e4982, $saturation: 30%); // #0a4986
        ```

    === "SASS"

        ```sass
        @use 'sass:color'

        // #0e4982 имеет насыщенность 80%, поэтому при добавлении 30%
        // saturate() становится полностью насыщенным.
        @debug saturate(#0e4982, 30%)  // #004990

        // scale() вместо этого делает его на 30% более насыщенным, чем был изначально.
        @debug color.scale(#0e4982, $saturation: 30%)  // #0a4986
        ```

=== "SCSS"

    ```scss
    // Насыщенность 50% становится 70%.
    @debug saturate(#c69, 20%); // #e05299

    // Насыщенность 35% становится 85%.
    @debug desaturate(#f2ece4, 50%); // #ebebeb

    // Насыщенность 80% становится 100%.
    @debug saturate(#0e4982, 30%); // #004990
    ```

=== "SASS"

    ```sass
    // Насыщенность 50% становится 70%.
    @debug saturate(#c69, 20%) // #e05299

    // Насыщенность 35% становится 85%.
    @debug desaturate(#f2ece4, 50%) // #ebebeb

    // Насыщенность 80% становится 100%.
    @debug saturate(#0e4982, 30%) // #004990
    ```

### saturation

```scss
color.saturation($color)
saturation($color) //=> число
```

Возвращает HSL-насыщенность `$color` в виде числа от `0%` до `100%`.

`$color` должен находиться в [устаревшем цветовом пространстве](../values/colors#legacy-color-spaces).

!!! tip "Совет"

    Поскольку `color.saturation()` дублирует [`color.channel()`](#channel), она больше не рекомендуется. Вместо `color.saturation($color)` используйте [`color.channel($color, "saturation")`](#channel).

=== "SCSS"

    ```scss
    @use 'sass:color';

    @debug color.saturation(#e1d7d2); // 20%
    @debug color.saturation(#f2ece4); // 30%
    @debug color.saturation(#dadbdf); // 7.2463768116%
    ```

=== "SASS"

    ```sass
    @use 'sass:color'

    @debug color.saturation(#e1d7d2) // 20%
    @debug color.saturation(#f2ece4) // 30%
    @debug color.saturation(#dadbdf) // 7.2463768116%
    ```

### transparentize

```scss
transparentize($color, $amount)
fade-out($color, $amount) //=> число
```

Делает `$color` более прозрачным.

`$color` должен находиться в [устаревшем цветовом пространстве](../values/colors#legacy-color-spaces).

`$amount` должен быть числом от `0` до `1` (включительно). Уменьшает альфа-канал `$color` на это значение.

!!! tip "Совет"

    Функция `transparentize()` уменьшает альфа-канал на фиксированную величину, что часто не дает желаемого эффекта. Чтобы сделать цвет на определённый процент более прозрачным, чем был изначально, используйте [`color.scale()`](#scale) вместо этого.

    Поскольку `transparentize()` обычно не является лучшим способом сделать цвет более прозрачным, она не включена напрямую в новую модульную систему. Однако для сохранения существующего поведения `transparentize($color, $amount)` можно записать как [`color.adjust($color, $alpha: -$amount)`](#adjust).

    === "SCSS"

        ```scss
        @use 'sass:color';

        // rgba(#036, 0.3) имеет альфа-канал 0.3, поэтому при вычитании 0.3
        // transparentize() возвращает полностью прозрачный цвет.
        @debug transparentize(rgba(#036, 0.3), 0.3); // rgba(0, 51, 102, 0)

        // scale() вместо этого делает его на 30% более прозрачным, чем был изначально.
        @debug color.scale(rgba(#036, 0.3), $alpha: -30%); // rgba(0, 51, 102, 0.21)
        ```

    === "SASS"

        ```sass
        @use 'sass:color'

        // rgba(#036, 0.3) имеет альфа-канал 0.3, поэтому при вычитании 0.3
        // transparentize() возвращает полностью прозрачный цвет.
        @debug transparentize(rgba(#036, 0.3), 0.3)  // rgba(0, 51, 102, 0)

        // scale() вместо этого делает его на 30% более прозрачным, чем был изначально.
        @debug color.scale(rgba(#036, 0.3), $alpha: -30%)  // rgba(0, 51, 102, 0.21)
        ```

=== "SCSS"

    ```scss
    @debug transparentize(rgba(#6b717f, 0.5), 0.2); // rgba(107, 113, 127, 0.3)
    @debug fade-out(rgba(#e1d7d2, 0.5), 0.4); // rgba(225, 215, 210, 0.1)
    @debug transparentize(rgba(#036, 0.3), 0.3); // rgba(0, 51, 102, 0)
    ```

=== "SASS"

    ```sass
    @debug transparentize(rgba(#6b717f, 0.5), 0.2) // rgba(107, 113, 127, 0.3)
    @debug fade-out(rgba(#e1d7d2, 0.5), 0.4) // rgba(225, 215, 210, 0.1)
    @debug transparentize(rgba(#036, 0.3), 0.3) // rgba(0, 51, 102, 0)
    ```

### whiteness

```scss
color.whiteness($color) //=> число
```

Возвращает [HWB-белизну](https://en.wikipedia.org/wiki/HWB_color_model) `$color` в виде числа от `0%` до `100%`.

`$color` должен находиться в [устаревшем цветовом пространстве](../values/colors#legacy-color-spaces).

!!! tip "Совет"

    Поскольку `color.whiteness()` дублирует [`color.channel()`](#channel), она больше не рекомендуется. Вместо `color.whiteness($color)` используйте [`color.channel($color, "whiteness")`](#channel).

=== "SCSS"

    ```scss
    @use 'sass:color';

    @debug color.whiteness(#e1d7d2); // 82.3529411765%
    @debug color.whiteness(white); // 100%
    @debug color.whiteness(black); // 0%
    ```

=== "SASS"

    ```sass
    @use 'sass:color'

    @debug color.whiteness(#e1d7d2) // 82.3529411765%
    @debug color.whiteness(white) // 100%
    @debug color.whiteness(black) // 0%
    ```
