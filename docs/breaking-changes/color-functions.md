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

## Можно ли отключить предупреждения? {#can-i-silence-the-warnings}

Sass предоставляет мощный набор опций для управления тем, какие предупреждения об устаревании вы видите и когда.

### Краткий и подробный режим {#terse-and-verbose-mode}

По умолчанию Sass работает в кратком режиме, где он будет выводить каждый тип предупреждения об устаревании только пять раз, прежде чем отключит дополнительные предупреждения. Это помогает гарантировать, что пользователи знают, когда им нужно быть в курсе предстоящего критического изменения, не создавая при этом огромного количества шума в консоли.

Если вы запустите Sass в подробном режиме, он будет выводить *каждое* предупреждение об устаревании, с которым столкнется. Это может быть полезно для отслеживания оставшейся работы, которую необходимо выполнить при исправлении устареваний. Вы можете включить подробный режим, используя [флаг `--verbose`](../cli/dart-sass/#verbose) в командной строке, или [опцию `verbose`](https://sass-lang.com/documentation/js-api/interfaces/Options/#verbose) в JavaScript API.

!!! tip "Совет"

    При запуске из [JS API](https://sass-lang.com/documentation/js-api/) Sass не делится никакой информацией между компиляциями, поэтому по умолчанию он будет выводить пять предупреждений для *каждой таблицы стилей*, которая компилируется. Однако вы можете это исправить, написав (или попросив автора плагина Sass вашего любимого фреймворка написать) [пользовательский `Logger`](https://sass-lang.com/documentation/js-api/interfaces/Logger-1/), который выводит только пять ошибок на каждое устаревание и может использоваться совместно для нескольких компиляций.

### Отключение устареваний в зависимостях {#silencing-deprecations-in-dependencies}

Иногда ваши зависимости имеют предупреждения об устаревании, с которыми вы ничего не можете поделать. Вы можете отключить предупреждения об устаревании из зависимостей, продолжая при этом выводить их для вашего приложения, используя [флаг `--quiet-deps`](../cli/dart-sass/#quiet-deps) в командной строке, или [опцию `quietDeps`](https://sass-lang.com/documentation/js-api/interfaces/Options/#quietDeps) в JavaScript API.

Для целей этого флага «зависимость» — это любая таблица стилей, которая не является просто серией относительных загрузок из входной таблицы стилей. Это означает всё, что поступает из пути загрузки, и большинство таблиц стилей, загруженных через пользовательские импортеры.

### Отключение конкретных устареваний {#silencing-specific-deprecations}

Если вы знаете, что одно конкретное устаревание не является для вас проблемой, вы можете отключить предупреждения для этого конкретного устаревания, используя [флаг `--silence-deprecation`](../cli/dart-sass/#silence-deprecation) в командной строке, или [опцию `silenceDeprecations`](https://sass-lang.com/documentation/js-api/interfaces/Options/#silenceDeprecations) в JavaScript API.
