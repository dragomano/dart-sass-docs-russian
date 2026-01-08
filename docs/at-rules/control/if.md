---
title: "@if и @else"
icon: lucide/at-sign
---

> Правило `@if` записывается как `@if <выражение> { ... }` и управляет тем, будет ли его блок вычислен (включая вывод любых стилей в CSS). Оно использует [выражение](../syntax/structure#expressions), которое обычно возвращает [`true` или `false`](../values/booleans): если выражение возвращает `true`, блок вычисляется, а если выражение возвращает `false`, он не вычисляется.

<div class="grid" markdown>

=== "SCSS"

    ```scss
    @use "sass:math";

    @mixin avatar($size, $circle: false) {
      width: $size;
      height: $size;

      @if $circle {
        border-radius: math.div($size, 2);
      }
    }

    .square-av {
      @include avatar(100px, $circle: false);
    }
    .circle-av {
      @include avatar(100px, $circle: true);
    }
    ```

=== "SASS"

    ```sass
    @use "sass:math"

    @mixin avatar($size, $circle: false)
      width: $size
      height: $size

      @if $circle
        border-radius: math.div($size, 2)

    .square-av
      @include avatar(100px, $circle: false)

    .circle-av
      @include avatar(100px, $circle: true)
    ```

```css title="CSS"
.square-av {
  width: 100px;
  height: 100px;
}

.circle-av {
  width: 100px;
  height: 100px;
  border-radius: 50px;
}
```

</div>

## `@else`

За правилом `@if` опционально может следовать правило `@else`, записываемое как `@else { ... }`. Блок этого правила вычисляется, если выражение `@if` возвращает `false`.

<div class="grid" markdown>

=== "SCSS"

    ```scss
    $light-background: #f2ece4;
    $light-text: #036;
    $dark-background: #6b717f;
    $dark-text: #d2e1dd;

    @mixin theme-colors($light-theme: true) {
      @if $light-theme {
        background-color: $light-background;
        color: $light-text;
      } @else {
        background-color: $dark-background;
        color: $dark-text;
      }
    }

    .banner {
      @include theme-colors($light-theme: true);
      body.dark & {
        @include theme-colors($light-theme: false);
      }
    }
    ```

=== "SASS"

    ```sass
    $light-background: #f2ece4
    $light-text: #036
    $dark-background: #6b717f
    $dark-text: #d2e1dd

    @mixin theme-colors($light-theme: true)
      @if $light-theme
        background-color: $light-background
        color: $light-text
      @else
        background-color: $dark-background
        color: $dark-text

    .banner
      @include theme-colors($light-theme: true)
      body.dark &
        @include theme-colors($light-theme: false)
    ```

```css title="CSS"
.banner {
  background-color: #f2ece4;
  color: #036;
}
body.dark .banner {
  background-color: #6b717f;
  color: #d2e1dd;
}
```

</div>

Условные выражения могут содержать [булевы операторы](../operators/boolean) (`and`, `or`, `not`).

### `@else if`

Вы также можете выбрать, будет ли вычисляться блок правила `@else`, записав его как `@else if <выражение> { ... }`. В этом случае блок вычисляется только если выражение предыдущего `@if` возвращает `false` *и* выражение `@else if` возвращает `true`.

Фактически, вы можете создать цепочку из любого количества `@else if` после `@if`. Будет вычислен первый блок в цепочке, чьё выражение вернёт `true`, и никакие другие. Если в конце цепочки есть обычный `@else`, его блок будет вычислен, если все остальные блоки не подошли.

<div class="grid" markdown>

=== "SCSS"

    ```scss
    @use "sass:math";

    @mixin triangle($size, $color, $direction) {
      height: 0;
      width: 0;

      border-color: transparent;
      border-style: solid;
      border-width: math.div($size, 2);

      @if $direction == up {
        border-bottom-color: $color;
      } @else if $direction == right {
        border-left-color: $color;
      } @else if $direction == down {
        border-top-color: $color;
      } @else if $direction == left {
        border-right-color: $color;
      } @else {
        @error "Unknown direction #{$direction}.";
      }
    }

    .next {
      @include triangle(5px, black, right);
    }
    ```

=== "SASS"

    ```sass
    @use "sass:math"

    @mixin triangle($size, $color, $direction)
      height: 0
      width: 0

      border-color: transparent
      border-style: solid
      border-width: math.div($size, 2)

      @if $direction == up
        border-bottom-color: $color
      @else if $direction == right
        border-left-color: $color
      @else if $direction == down
        border-top-color: $color
      @else if $direction == left
        border-right-color: $color
      @else
        @error "Unknown direction #{$direction}."

    .next
      @include triangle(5px, black, right)
    ```

```css title="CSS"
.next {
  height: 0;
  width: 0;
  border-color: transparent;
  border-style: solid;
  border-width: 2.5px;
  border-left-color: black;
}
```

</div>

## Истинность и ложность {#truthiness-and-falsiness}

Везде, где допускаются `true` или `false`, можно использовать и другие значения. Значения `false` и [`null`](../values/null) считаются *ложными* (falsey), то есть Sass воспринимает их как обозначающие ложь и приводящие к тому, что условия не выполняются. Все остальные значения считаются *истинными* (truthy), поэтому Sass воспринимает их как эквивалент `true`, и такие значения приводят к выполнению условий.

Например, если нужно проверить, содержит ли строка пробел, можно просто написать `string.index($string, " ")`. Функция [`string.index()`](../modules/string#index) возвращает `null`, если подстрока не найдена, и число в противном случае.

!!! note "Примечание"

    Некоторые языки считают ложными больше значений, чем просто `false` и `null`. Sass к таким языкам не относится: пустые строки, пустые списки и число `0` в Sass являются истинными (truthy).
