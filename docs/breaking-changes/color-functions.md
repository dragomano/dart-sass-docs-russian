---
title: Цветовые функции
icon: lucide/newspaper
---

> Некоторые цветовые функции, разработанные с предположением, что все цвета взаимосовместимы, теперь больше не имеют смысла, поскольку Sass поддерживает все цветовые пространства CSS Color 4.

Исторически все цветовые значения Sass охватывали одну и ту же гамму: независимо от того, определялись ли цвета как RGB, HSL или HWB, они покрывали только [гамму `sRGB`](https://en.wikipedia.org/wiki/SRGB) и могли представлять только те цвета, которые мониторы могли отображать с середины 1990-х годов. Когда Sass добавлял свой первоначальный набор цветовых функций, они предполагали, что все цвета можно свободно конвертировать между любыми из этих представлений и что для каждого названия канала, такого как `red` или `hue`, существует однозначное значение.

Выпуск [CSS Color 4](https://developer.mozilla.org/en-US/blog/css-color-module-level-4/) изменил всё это. Он добавил поддержку многих новых цветовых пространств с другими (более широкими) гаммами, чем `sRGB`. Чтобы поддерживать эти цвета, Sass пришлось переосмыслить работу цветовых функций. В дополнение к добавлению новых функций, таких как [`color.channel()`](../modules/color/#channel) и [`color.to-space()`](../modules/color/#to-space), ряд старых функций был признан устаревшим, поскольку они основывались на предположениях, которые больше не соответствуют действительности.

### Старые функции каналов {#old-channel-functions}

Названия каналов теперь неоднозначны в разных цветовых пространствах. Наследуемое RGB-пространство имеет канал `red`, но такие же каналы есть и в `display-p3`, `rec2020` и многих других. Поэтому [`color.red()`](../modules/color/#red), [`color.green()`](../modules/color/#green), [`color.blue()`](../modules/color/#blue), [`color.hue()`](../modules/color/#hue), [`color.saturation()`](../modules/color/#saturation), [`color.lightness()`](../modules/color/#lightness), [`color.whiteness()`](../modules/color/#whiteness), [`color.blackness()`](../modules/color/#blackness), [`color.alpha()`](../modules/color/#alpha) и [`color.opacity()`](../modules/color/#opacity) будут удалены. Вместо этого вы можете использовать функцию [`color.channel()`](../modules/color/#channel), чтобы получить значение конкретного канала, обычно с явным аргументом `$space`, указывающим, с каким цветовым пространством вы работаете.

=== "SCSS"

    ```scss
    @use "sass:color";

    $color: #c71585;
    @debug color.channel($color, "red", $space: rgb);
    @debug color.channel($color, "red", $space: display-p3);
    @debug color.channel($color, "hue", $space: oklch);
    ```

=== "SASS"

    ```sass
    @use "sass:color"

    $color: #c71585
    @debug color.channel($color, "red", $space: rgb)
    @debug color.channel($color, "red", $space: display-p3)
    @debug color.channel($color, "hue", $space: oklch)
    ```

### Функции корректировки одного канала {#single-channel-adjustment-functions}

У этих функций такая же проблема неоднозначности, как и у старых функций каналов, а _ещё_ они уже были избыточными по отношению к [`color.adjust()`](../modules/color/#adjust) ещё до добавления поддержки Color 4. Более того, часто лучше использовать [`color.scale()`](../modules/color/#scale), поскольку она лучше подходит для внесения изменений относительно существующего цвета, а не в абсолютных величинах. Поэтому [`adjust-hue()`](../modules/color/#adjust-hue), [`saturate()`](../modules/color/#saturate), [`desaturate()`](../modules/color/#desaturate), [`lighten()`](../modules/color/#lighten), [`darken()`](../modules/color/#darken), [`opacify()`](../modules/color/#opacify), [`fade-in()`](../modules/color/#fade-in), [`transparentize()`](../modules/color/#transparentize) и [`fade-out()`](../modules/color/#fade-out) будут удалены. Обратите внимание, что у этих функций никогда не было модульных аналогов, поскольку их использование уже не рекомендовалось.

=== "SCSS"

    ```scss
    @use "sass:color";

    $color: #c71585;
    @debug color.adjust($color, $lightness: 15%, $space: hsl);
    @debug color.adjust($color, $lightness: 15%, $space: oklch);
    @debug color.scale($color, $lightness: 15%, $space: oklch);
    ```

=== "SASS"

    ```sass
    @use "sass:color"

    $color: #c71585
    @debug color.adjust($color, $lightness: 15%, $space: hsl)
    @debug color.adjust($color, $lightness: 15%, $space: oklch)
    @debug color.scale($color, $lightness: 15%, $space: oklch)
    ```

## Переходный период {#transition-period}

Сначала мы будем выдавать предупреждения об устаревании для всех использований функций, которые планируется удалить. В Dart Sass 2.0.0 эти функции будут полностью удалены. Попытки вызвать модульные версии вызовут ошибку, в то время как глобальные функции будут рассматриваться как обычные CSS-функции и выводиться как обычные строки.

Вы можете использовать [мигратор Sass](https://sass-lang.com/documentation/cli/migrator/#color), чтобы автоматически перенести устаревшие API на их новые замены.

--8<-- "includes/silence-warnings.md"
