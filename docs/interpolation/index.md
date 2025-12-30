---
title: Интерполяция
icon: lucide/chart-spline
---

> Интерполяцию можно использовать почти везде в таблице стилей Sass, чтобы встроить результат [выражения SassScript](../syntax/structure#expressions) в фрагмент CSS. Просто оберните выражение в `#{}` в любом из следующих мест:

* [Селекторы в правилах стилей](../style-rules#interpolation)
* [Имена свойств в объявлениях](../style-rules/declarations#interpolation)
* [Значения пользовательских свойств](../style-rules/declarations#custom-properties)
* [At-правила CSS](../at-rules/css)
* [`@extend`](../at-rules/extend)
* [Обычные CSS `@import`](../at-rules/import/#plain-css-imports)
* [Строки в кавычках или без](../values/strings)
* [Специальные функции](../syntax/special-functions)
* [Имена обычных функций CSS](../at-rules/function/#plain-css-functions)
* [Громкие комментарии](../syntax/comments)

<div class="grid" markdown>

=== "SCSS"

    ```scss
    @mixin corner-icon($name, $top-or-bottom, $left-or-right) {
      .icon-#{$name} {
        background-image: url("/icons/#{$name}.svg");
        position: absolute;
        #{$top-or-bottom}: 0;
        #{$left-or-right}: 0;
      }
    }

    @include corner-icon("mail", top, left);
    ```

=== "Sass"

    ```sass
    @mixin corner-icon($name, $top-or-bottom, $left-or-right)
      .icon-#{$name}
        background-image: url("/icons/#{$name}.svg")
        position: absolute
        #{$top-or-bottom}: 0
        #{$left-or-right}: 0

    @include corner-icon("mail", top, left)
    ```

```css title="CSS"
.icon-mail {
  background-image: url("/icons/mail.svg");
  position: absolute;
  top: 0;
  left: 0;
}
```

</div>

## В SassScript {#in-sass-script}

Интерполяция может использоваться в SassScript для внедрения SassScript в [строки без кавычек](../values/strings#unquoted). Это особенно полезно при динамической генерации имён (например, для анимаций) или при использовании [значений, разделенных косой чертой](../operators/numeric#slash-separated-values). Обратите внимание, что интерполяция в SassScript всегда возвращает строку без кавычек.

<div class="grid" markdown>

=== "SCSS"

    ```scss
    @mixin inline-animation($duration) {
      $name: inline-#{unique-id()};

      @keyframes #{$name} {
        @content;
      }

      animation-name: $name;
      animation-duration: $duration;
      animation-iteration-count: infinite;
    }

    .pulse {
      @include inline-animation(2s) {
        from { background-color: yellow }
        to { background-color: red }
      }
    }
    ```

=== "Sass"

    ```sass
    @mixin inline-animation($duration)
      $name: inline-#{unique-id()}

      @keyframes #{$name}
        @content


      animation-name: $name
      animation-duration: $duration
      animation-iteration-count: infinite


    .pulse
      @include inline-animation(2s)
        from
          background-color: yellow
        to
          background-color: red
    ```

```css title="CSS"
.pulse {
  animation-name: inline-uifpe6h;
  animation-duration: 2s;
  animation-iteration-count: infinite;
}
@keyframes inline-uifpe6h {
  from {
    background-color: yellow;
  }
  to {
    background-color: red;
  }
}
```

</div>

!!! tip "Совет"

    Интерполяция полезна для внедрения значений в строки, но, кроме этого, она редко нужна в выражениях SassScript. Вам точно *не* нужно использовать её просто для того, чтобы применить переменную в значении свойства. Вместо того чтобы писать `color: #{$accent}`, вы можете просто написать `color: $accent`!

!!! warning "Предупреждение"

    Использование интерполяции с числами — это почти всегда плохая идея. Интерполяция возвращает строки без кавычек, которые нельзя использовать для дальнейших математических вычислений, и это обходит встроенные средства защиты Sass, гарантирующие правильное использование единиц измерения.

    Вместо этого в Sass есть мощная [арифметика единиц измерения](../values/numbers#units). Например, вместо того чтобы писать `#{$width}px`, пишите `$width * 1px` — или, что ещё лучше, изначально объявляйте переменную `$width` в `px`. Таким образом, если `$width` уже имеет единицы измерения, вы получите понятное сообщение об ошибке вместо компиляции некорректного CSS.

## Строки в кавычках {#quoted-strings}

В большинстве случаев интерполяция вставляет точно такой же текст, который использовался бы, если бы выражение применялось как [значение свойства](../style-rules/declarations). Но есть одно исключение: кавычки вокруг строк в кавычках удаляются (даже если эти строки находятся в списках). Это позволяет писать строки в кавычках, содержащие синтаксис, недопустимый в SassScript (например, селекторы), и интерполировать их в правила стилей.

<div class="grid" markdown>

=== "SCSS"

    ```scss
    .example {
      unquoted: #{"string"};
    }
    ```

=== "Sass"

    ```sass
    .example
      unquoted: #{"string"}
    ```

```css title="CSS"
.example {
  unquoted: string;
}
```

</div>

!!! tip "Совет"

    Хотя заманчиво использовать эту возможность для преобразования строк в кавычках в строки без кавычек, гораздо понятнее использовать [функцию `string.unquote()`](../modules/string#unquote). Вместо `#{$string}` пишите `string.unquote($string)`!
