---
title: "@for"
icon: lucide/at-sign
---

> Правило `@for` записывается как `@for <переменная> from <выражение> to <выражение> { ... }` или `@for <переменная> from <выражение> through <выражение> { ... }` и выполняет счёт вверх или вниз от одного числа (результат первого [выражения](../syntax/structure#expressions)) до другого (результат второго), вычисляя блок для каждого числа в этом диапазоне. Каждое число по пути присваивается указанному имени переменной. Если используется `to`, конечное число исключается; если используется `through`, оно включается.

<div class="grid" markdown>

=== "SCSS"

    ```scss
    $base-color: #036;

    @for $i from 1 through 3 {
      ul:nth-child(3n + #{$i}) {
        background-color: lighten($base-color, $i * 5%);
      }
    }
    ```

=== "SASS"

    ```sass
    $base-color: #036

    @for $i from 1 through 3
      ul:nth-child(3n + #{$i})
        background-color: lighten($base-color, $i * 5%)
    ```

```css title="CSS"
ul:nth-child(3n+1) {
  background-color: rgb(0, 63.75, 127.5);
}

ul:nth-child(3n+2) {
  background-color: rgb(0, 76.5, 153);
}

ul:nth-child(3n+3) {
  background-color: rgb(0, 89.25, 178.5);
}
```

</div>
