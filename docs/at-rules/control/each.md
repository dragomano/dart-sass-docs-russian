---
title: "@each"
icon: lucide/at-sign
---

> Правило `@each` упрощает вывод стилей или выполнение кода для каждого элемента [списка](../values/lists) или каждой пары в [карте](../values/maps). Оно отлично подходит для повторяющихся стилей, которые отличаются лишь небольшими вариациями. Обычно оно записывается как `@each <переменная> in <выражение> { ... }`, где [выражение](../syntax/structure#expressions) возвращает список. Блок последовательно выполняется для каждого элемента этого списка, который по очереди присваивается указанной переменной.

<div class="grid" markdown>

=== "SCSS"

    ```scss
    $sizes: 40px, 50px, 80px;

    @each $size in $sizes {
      .icon-#{$size} {
        font-size: $size;
        height: $size;
        width: $size;
      }
    }
    ```

=== "SASS"

    ```sass
    $sizes: 40px, 50px, 80px

    @each $size in $sizes
      .icon-#{$size}
        font-size: $size
        height: $size
        width: $size
    ```

```css title="CSS"
.icon-40px {
  font-size: 40px;
  height: 40px;
  width: 40px;
}

.icon-50px {
  font-size: 50px;
  height: 50px;
  width: 50px;
}

.icon-80px {
  font-size: 80px;
  height: 80px;
  width: 80px;
}
```

</div>

## С картами {#with-maps}

Вы также можете использовать `@each` для перебора каждой пары ключ/значение в карте, записав его как `@each <переменная>, <переменная> in <выражение> { ... }`. Ключ присваивается первой переменной, а значение — второй.

<div class="grid" markdown>

=== "SCSS"

    ```scss
    $icons: ("eye": "\f112", "start": "\f12e", "stop": "\f12f");

    @each $name, $glyph in $icons {
      .icon-#{$name}:before {
        display: inline-block;
        font-family: "Icon Font";
        content: $glyph;
      }
    }
    ```

=== "SASS"

    ```sass
    $icons: ("eye": "\f112", "start": "\f12e", "stop": "\f12f")

    @each $name, $glyph in $icons
      .icon-#{$name}:before
        display: inline-block
        font-family: "Icon Font"
        content: $glyph
    ```

```css title="CSS"
.icon-eye:before {
  display: inline-block;
  font-family: "Icon Font";
  content: "\f112";
}

.icon-start:before {
  display: inline-block;
  font-family: "Icon Font";
  content: "\f12e";
}

.icon-stop:before {
  display: inline-block;
  font-family: "Icon Font";
  content: "\f12f";
}
```

</div>

## Деструктуризация {#destructuring}

Если у вас есть список списков, вы можете использовать `@each` для автоматического присваивания переменных каждому из значений из внутренних списков, записав его как `@each <переменная...> in <выражение> { ... }`. Это называется *деструктуризацией*, поскольку переменные соответствуют структуре внутренних списков. Каждое имя переменной получает значение на соответствующей позиции в списке, или [`null`](../values/null), если в списке недостаточно значений.

<div class="grid" markdown>

=== "SCSS"

    ```scss
    $icons:
      "eye" "\f112" 12px,
      "start" "\f12e" 16px,
      "stop" "\f12f" 10px;

    @each $name, $glyph, $size in $icons {
      .icon-#{$name}:before {
        display: inline-block;
        font-family: "Icon Font";
        content: $glyph;
        font-size: $size;
      }
    }
    ```

=== "SASS"

    ```sass
    $icons: "eye" "\f112" 12px, "start" "\f12e" 16px, "stop" "\f12f" 10px

    @each $name, $glyph, $size in $icons
      .icon-#{$name}:before
        display: inline-block
        font-family: "Icon Font"
        content: $glyph
        font-size: $size
    ```

```css title="CSS"
.icon-eye:before {
  display: inline-block;
  font-family: "Icon Font";
  content: "\f112";
  font-size: 12px;
}

.icon-start:before {
  display: inline-block;
  font-family: "Icon Font";
  content: "\f12e";
  font-size: 16px;
}

.icon-stop:before {
  display: inline-block;
  font-family: "Icon Font";
  content: "\f12f";
  font-size: 10px;
}
```

</div>

!!! note "Примечание"

    Поскольку `@each` поддерживает деструктуризацию, а [карты считаются списками списков](../values/maps), поддержка карт в `@each` работает без необходимости специальной реализации именно для карт.
