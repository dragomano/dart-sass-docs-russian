---
title: "true и false"
icon: lucide/binary
---

> Булевы значения — это логические значения `true` и `false`. Помимо их буквальных форм, булевы значения возвращаются операторами [равенства](../operators/equality) и [сравнения](../operators/relational), а также многими встроенными функциями, такими как [`math.comparable()`](../modules/math#comparable) и [`map.has-key()`](../modules/map#has-key).

=== "SCSS"

    ```scss
    @use "sass:math";

    @debug 1px == 2px; // false
    @debug 1px == 1px; // true
    @debug 10px < 3px; // false
    @debug math.comparable(100px, 3in); // true
    ```

=== "SASS"

    ```sass
    @use "sass:math"

    @debug 1px == 2px // false
    @debug 1px == 1px // true
    @debug 10px < 3px // false
    @debug math.comparable(100px, 3in) // true
    ```

Вы можете работать с булевыми значениями, используя [булевы операторы](../operators/boolean). Оператор `and` возвращает `true`, если *обе* стороны равны `true`, а оператор `or` возвращает `true`, если *любая* из сторон равна `true`. Оператор `not` возвращает противоположное значение одного булева значения.

=== "SCSS"

    ```scss
    @debug true and true; // true
    @debug true and false; // false

    @debug true or false; // true
    @debug false or false; // false

    @debug not true; // false
    @debug not false; // true
    ```

=== "SASS"

    ```sass
    @debug true and true // true
    @debug true and false // false

    @debug true or false // true
    @debug false or false // false

    @debug not true // false
    @debug not false // true
    ```

## Использование булевых значений {#using-booleans}

Вы можете использовать булевы значения, чтобы выбирать, выполнять ли различные действия в Sass. Правило [`@if`](../at-rules/control/if) вычисляет блок стилей, если его аргумент равен `true`:

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

Функция [`if()`](../syntax/special-functions#if) возвращает одно значение, если её аргумент равен `true`, и другое, если аргумент равен `false`:

=== "SCSS"

    ```scss
    @debug if(true: 10px; else: 30px); // 10px
    @debug if(false: 10px; else: 30px); // 30px
    ```

=== "SASS"

    ```sass
    @debug if(true: 10px; else: 30px) // 10px
    @debug if(false: 10px; else: 30px) // 30px
    ```

## Истинность и ложность {#truthiness-and-falsiness}

Везде, где допускаются `true` или `false`, можно использовать и другие значения. Значения `false` и [`null`](../values/null) считаются *ложными* (falsey), то есть Sass воспринимает их как обозначающие ложь и приводящие к тому, что условия не выполняются. Все остальные значения считаются *истинными* (truthy), поэтому Sass воспринимает их как эквивалент `true`, и такие значения приводят к выполнению условий.

Например, если нужно проверить, содержит ли строка пробел, можно просто написать `string.index($string, " ")`. Функция [`string.index()`](../modules/string#index) возвращает `null`, если подстрока не найдена, и число в противном случае.

!!! note "Примечание"

    Некоторые языки считают ложными больше значений, чем просто `false` и `null`. Sass к таким языкам не относится: пустые строки, пустые списки и число `0` в Sass являются истинными (truthy).

