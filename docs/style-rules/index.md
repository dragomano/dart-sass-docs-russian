---
title: Правила стилей
icon: lucide/square-library
---

> Правила стилей — это основа Sass, точно так же, как и для CSS. И они работают одинаково: вы выбираете элементы для стилизации с помощью селектора и [объявляете свойства](./declarations), которые влияют на то, как эти элементы выглядят.

<div class="grid" markdown>

=== "SCSS"

    ```scss
    .button {
      padding: 3px 10px;
      font-size: 12px;
      border-radius: 3px;
      border: 1px solid #e1e4e8;
    }
    ```

=== "Sass"

    ```sass
    .button
      padding: 3px 10px
      font-size: 12px
      border-radius: 3px
      border: 1px solid #e1e4e8
    ```

```css title="CSS"
.button {
  padding: 3px 10px;
  font-size: 12px;
  border-radius: 3px;
  border: 1px solid #e1e4e8;
}
```

</div>

## Вложенность {#nesting}

Но Sass хочет облегчить вам жизнь. Вместо того чтобы повторять одни и те же селекторы снова и снова, вы можете писать одно правило стиля внутри другого. Sass автоматически объединит селектор внешнего правила с селектором внутреннего правила.

<div class="grid" markdown>

=== "SCSS"

    ```scss
    nav {
      ul {
        margin: 0;
        padding: 0;
        list-style: none;
      }

      li { display: inline-block; }

      a {
        display: block;
        padding: 6px 12px;
        text-decoration: none;
      }
    }
    ```

=== "Sass"

    ```sass
    nav
      ul
        margin: 0
        padding: 0
        list-style: none

      li
        display: inline-block

      a
        display: block
        padding: 6px 12px
        text-decoration: none
    ```

```css title="CSS"
nav ul {
  margin: 0;
  padding: 0;
  list-style: none;
}
nav li {
  display: inline-block;
}
nav a {
  display: block;
  padding: 6px 12px;
  text-decoration: none;
}
```

</div>

!!! warning "Внимание"

    Вложенные правила очень полезны, но они также могут затруднить визуализацию того, сколько CSS вы фактически генерируете. Чем глубже вложенность, тем больше требуется пропускной способности для передачи вашего CSS и тем больше работы требуется браузеру для его отрисовки. Держите эти селекторы неглубокими!

### Списки селекторов {#selector-lists}

Вложенные правила умело обрабатывают списки селекторов (то есть селекторы, разделённые запятыми). Каждый сложный селектор (те, что между запятыми) вкладывается отдельно, а затем они снова объединяются в список селекторов.

<div class="grid" markdown>

=== "SCSS"

    ```scss
    .alert, .warning {
      ul, p {
        margin-right: 0;
        margin-left: 0;
        padding-bottom: 0;
      }
    }
    ```

=== "Sass"

    ```sass
    .alert, .warning
      ul, p
        margin-right: 0
        margin-left: 0
        padding-bottom: 0
    ```

```css title="CSS"
.alert ul, .alert p, .warning ul, .warning p {
  margin-right: 0;
  margin-left: 0;
  padding-bottom: 0;
}
```

</div>

### Комбинаторы селекторов {#selector-combinators}

Вы также можете вкладывать селекторы, использующие [комбинаторы](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Selectors#Combinators#Combinators). Вы можете поместить комбинатор в конец внешнего селектора, в начало внутреннего селектора или даже отдельно между ними.

<div class="grid" markdown>

=== "SCSS"

    ```scss
    ul > {
      li {
        list-style-type: none;
      }
    }

    h2 {
      + p {
        border-top: 1px solid gray;
      }
    }

    p {
      ~ {
        span {
          opacity: 0.8;
        }
      }
    }
    ```

=== "Sass"

    ```sass
    ul >
      li
        list-style-type: none

    h2
      + p
        border-top: 1px solid gray

    p
      ~
        span
          opacity: 0.8
    ```

```css title="CSS"
ul > li {
  list-style-type: none;
}

h2 + p {
  border-top: 1px solid gray;
}

p ~ span {
  opacity: 0.8;
}
```

</div>

### Продвинутая вложенность {#advanced-nesting}

Если вы хотите сделать больше с вашими вложенными правилами стилей, чем просто объединить их по порядку с комбинатором потомков (то есть обычным пробелом), разделяющим их, Sass вас поддержит. Смотрите [документацию по родительскому селектору](./parent-selector) для более подробной информации.

## Интерполяция {#interpolation}

Вы можете использовать [интерполяцию](../interpolation) для внедрения значений из [выражений](../syntax/structure#expressions), таких как переменные и вызовы функций, в ваши селекторы. Это особенно полезно, когда вы пишете [миксины](../at-rules/mixin), поскольку это позволяет создавать селекторы из параметров, передаваемых вашими пользователями.

<div class="grid" markdown>

=== "SCSS"

    ```scss
    @mixin define-emoji($name, $glyph) {
      span.emoji-#{$name} {
        font-family: IconFont;
        font-variant: normal;
        font-weight: normal;
        content: $glyph;
      }
    }

    @include define-emoji("women-holding-hands", "👭");
    ```

=== "Sass"

    ```sass
    @mixin define-emoji($name, $glyph)
      span.emoji-#{$name}
        font-family: IconFont
        font-variant: normal
        font-weight: normal
        content: $glyph

    @include define-emoji("women-holding-hands", "👭")
    ```

```css title="CSS"
@charset "UTF-8";
span.emoji-women-holding-hands {
  font-family: IconFont;
  font-variant: normal;
  font-weight: normal;
  content: "👭";
}
```

</div>

!!! note "Заметка"

    Sass анализирует селекторы *только после* того, как интерполяция будет разрешена. Это означает, что вы можете безопасно использовать интерполяцию для генерации любой части селектора, не беспокоясь о том, что она не будет проанализирована.

Вы можете комбинировать интерполяцию с родительским селектором `&`, [правилом `@at-root`](../at-rules/at-root) и [функциями селекторов](../modules/selector), чтобы получить серьёзные возможности при динамической генерации селекторов. Для получения дополнительной информации смотрите [документацию по родительскому селектору](./parent-selector).
