---
title: "At-правила CSS"
icon: lucide/at-sign
---

Sass поддерживает все at-правила, которые являются частью стандартного CSS. Для обеспечения гибкости и совместимости с будущими версиями CSS, Sass имеет общую поддержку, которая по умолчанию охватывает почти все at-правила. CSS at-правило записывается как `@<name> <value>`, `@<name> { ... }` или `@<name> <value> { ... }`. Имя должно быть идентификатором, а значение (если оно есть) может быть практически любым. И имя, и значение могут содержать [интерполяцию](../interpolation).

<div class="grid" markdown>

=== "SCSS"

    ```scss
    @namespace svg url(http://www.w3.org/2000/svg);

    @font-face {
      font-family: "Open Sans";
      src: url("/fonts/OpenSans-Regular-webfont.woff2") format("woff2");
    }

    @counter-style thumbs {
      system: cyclic;
      symbols: "\1F44D";
    }
    ```

=== "SASS"

    ```sass
    @namespace svg url(http://www.w3.org/2000/svg)

    @font-face
      font-family: "Open Sans"
      src: url("/fonts/OpenSans-Regular-webfont.woff2") format("woff2")

    @counter-style thumbs
      system: cyclic
      symbols: "\1F44D"
    ```

```css title="CSS"
@charset "UTF-8";
@namespace svg url(http://www.w3.org/2000/svg);
@font-face {
  font-family: "Open Sans";
  src: url("/fonts/OpenSans-Regular-webfont.woff2") format("woff2");
}
@counter-style thumbs {
  system: cyclic;
  symbols: "👍";
}
```

</div>

Если CSS at-правило вложено внутри стилевого правила, они автоматически меняются местами так, что at-правило оказывается на верхнем уровне CSS-вывода, а стилевое правило — внутри него. Это упрощает добавление условных стилей без необходимости переписывать селектор стилевого правила.

<div class="grid" markdown>

=== "SCSS"

    ```scss
    .print-only {
      display: none;

      @media print { display: block; }
    }
    ```

=== "SASS"

    ```sass
    .print-only
      display: none

      @media print
        display: block
    ```

```css title="CSS"
.print-only {
  display: none;
}
@media print {
  .print-only {
    display: block;
  }
}
```

</div>

## `@media`

Правило [`@media`](https://developer.mozilla.org/en-US/docs/Web/CSS/Media_Queries/Using_media_queries) делает всё вышеперечисленное и даже больше. Помимо поддержки интерполяции, оно позволяет использовать [выражения SassScript](../syntax/structure#expressions) непосредственно в [узконаправленных медиазапросах](https://developer.mozilla.org/en-US/docs/Web/CSS/Media_Queries/Using_media_queries#Targeting_media_features).

<div class="grid" markdown>

=== "SCSS"

    ```scss
    $layout-breakpoint-small: 960px;

    @media (min-width: $layout-breakpoint-small) {
      .hide-extra-small {
        display: none;
      }
    }
    ```

=== "SASS"

    ```sass
    $layout-breakpoint-small: 960px

    @media (min-width: $layout-breakpoint-small)
      .hide-extra-small
        display: none
    ```

```css title="CSS"
@media (min-width: 960px) {
  .hide-extra-small {
    display: none;
  }
}
```

</div>

Когда это возможно, Sass также объединяет медиазапросы, которые вложены друг в друга, чтобы упростить поддержку браузеров, которые ещё не поддерживают нативно вложенные правила `@media`.

<div class="grid" markdown>

=== "SCSS"

    ```scss
    @media (hover: hover) {
      .button:hover {
        border: 2px solid black;

        @media (color) {
          border-color: #036;
        }
      }
    }
    ```

=== "SASS"

    ```sass
    @media (hover: hover)
      .button:hover
        border: 2px solid black

        @media (color)
          border-color: #036
    ```

```css title="CSS"
@media (hover: hover) {
  .button:hover {
    border: 2px solid black;
  }
}
@media (hover: hover) and (color) {
  .button:hover {
    border-color: #036;
  }
}
```

</div>

## `@supports`

Правило [`@supports`](https://developer.mozilla.org/en-US/docs/Web/CSS/@supports) также позволяет использовать [выражения SassScript](../syntax/structure#expressions) в запросах на поддержку свойств.

<div class="grid" markdown>

=== "SCSS"

    ```scss
    @mixin sticky-position {
      position: fixed;
      @supports (position: sticky) {
        position: sticky;
      }
    }

    .banner {
      @include sticky-position;
    }
    ```

=== "SASS"

    ```sass
    @mixin sticky-position
      position: fixed
      @supports (position: sticky)
        position: sticky

    .banner
      @include sticky-position
    ```

```css title="CSS"
.banner {
  position: fixed;
}
@supports (position: sticky) {
  .banner {
    position: sticky;
  }
}
```

</div>

## `@keyframes`

Правило [`@keyframes`](https://developer.mozilla.org/en-US/docs/Web/CSS/@keyframes) работает так же, как и обычное at-правило, за исключением того, что его дочерние правила должны быть валидными правилами ключевых кадров (`<number>%`, `from` или `to`), а не обычными селекторами.

<div class="grid" markdown>

=== "SCSS"

    ```scss
    @keyframes slide-in {
      from {
        margin-left: 100%;
        width: 300%;
      }

      70% {
        margin-left: 90%;
        width: 150%;
      }

      to {
        margin-left: 0%;
        width: 100%;
      }
    }
    ```

=== "SASS"

    ```sass
    @keyframes slide-in
      from
        margin-left: 100%
        width: 300%

      70%
        margin-left: 90%
        width: 150%

      to
        margin-left: 0%
        width: 100%
    ```

```css title="CSS"
@keyframes slide-in {
  from {
    margin-left: 100%;
    width: 300%;
  }
  70% {
    margin-left: 90%;
    width: 150%;
  }
  to {
    margin-left: 0%;
    width: 100%;
  }
}
```

</div>
