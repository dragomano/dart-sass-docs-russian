---
title: "@while"
icon: lucide/at-sign
---

> Правило `@while` записывается как `@while <выражение> { ... }` и вычисляет свой блок, если его [выражение](../syntax/structure#expressions) возвращает `true`. Затем, если выражение по-прежнему возвращает `true`, оно снова вычисляет свой блок. Это продолжается до тех пор, пока выражение наконец не вернёт `false`.

<div class="grid" markdown>

=== "SCSS"

    ```scss
    @use "sass:math";

    /// Делит `$value` на `$ratio` до тех пор, пока значение не станет меньше `$base`.
    @function scale-below($value, $base, $ratio: 1.618) {
      @while $value > $base {
        $value: math.div($value, $ratio);
      }
      @return $value;
    }

    $normal-font-size: 16px;
    sup {
      font-size: scale-below(20px, 16px);
    }
    ```

=== "SASS"

    ```sass
    @use "sass:math"

    /// Делит `$value` на `$ratio` до тех пор, пока значение не станет меньше `$base`.
    @function scale-below($value, $base, $ratio: 1.618)
      @while $value > $base
        $value: math.div($value, $ratio)
      @return $value

    $normal-font-size: 16px
    sup
      font-size: scale-below(20px, 16px)
    ```

```css title="CSS"
sup {
  font-size: 12.3609394314px;
}
```

</div>

!!! tip "Совет"

    Хотя `@while` необходим для некоторых особенно сложных таблиц стилей, обычно лучше использовать либо правило [`@each`](../at-rules/control/each), либо [`@for`](../at-rules/control/for), если одно из них подходит. Они более понятны для читателя и часто компилируются быстрее.

## Истинность и ложность {#truthiness-and-falsiness}

Везде, где допускаются `true` или `false`, можно использовать и другие значения. Значения `false` и [`null`](../values/null) считаются *ложными* (falsey), то есть Sass воспринимает их как обозначающие ложь и приводящие к тому, что условия не выполняются. Все остальные значения считаются *истинными* (truthy), поэтому Sass воспринимает их как эквивалент `true`, и такие значения приводят к выполнению условий.

Например, если нужно проверить, содержит ли строка пробел, можно просто написать `string.index($string, " ")`. Функция [`string.index()`](../modules/string#index) возвращает `null`, если подстрока не найдена, и число в противном случае.

!!! note "Примечание"

    Некоторые языки считают ложными больше значений, чем просто `false` и `null`. Sass к таким языкам не относится: пустые строки, пустые списки и число `0` в Sass являются истинными (truthy).
