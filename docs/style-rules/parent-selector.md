---
title: Родительский селектор
icon: lucide/square-mouse-pointer
---

> Родительский селектор, `&`, — это специальный селектор, изобретённый Sass, который используется во [вложенных селекторах](#nesting) для ссылки на внешний селектор. Он позволяет повторно использовать внешний селектор более сложными способами, например добавляя [псевдокласс](https://developer.mozilla.org/en-US/docs/Web/CSS/Pseudo-classes) или добавляя селектор *перед* родительским.

Когда родительский селектор используется во внутреннем селекторе, он заменяется соответствующим внешним селектором. Это происходит вместо обычного поведения вложенности.

<div class="grid" markdown>

=== "SCSS"

    ```scss
    .alert {
      // Родительский селектор может использоваться для
      // добавления псевдоклассов к внешнему селектору.
      &:hover {
        font-weight: bold;
      }

      // Он также может использоваться для стилизации
      // внешнего селектора в определённом контексте,
      // например, в body, настроенном на
      // использование языка с письмом справа налево.
      [dir=rtl] & {
        margin-left: 0;
        margin-right: 10px;
      }

      // Вы даже можете использовать его в качестве
      // аргумента для селекторов псевдоклассов.
      :not(&) {
        opacity: 0.8;
      }
    }
    ```

=== "Sass"

    ```sass
    .alert
      // Родительский селектор может использоваться для
      // добавления псевдоклассов к внешнему селектору.
      &:hover
        font-weight: bold


      // Он также может использоваться для стилизации
      // внешнего селектора в определённом контексте,
      // например, в body, настроенном на
      // использование языка с письмом справа налево.
      [dir=rtl] &
        margin-left: 0
        margin-right: 10px


      // Вы даже можете использовать его в качестве
      // аргумента для селекторов псевдоклассов.
      :not(&)
        opacity: 0.8
    ```

```css title="CSS"
.alert:hover {
  font-weight: bold;
}
[dir=rtl] .alert {
  margin-left: 0;
  margin-right: 10px;
}
:not(.alert) {
  opacity: 0.8;
}
```

</div>

!!! note "Примечание"

    Поскольку родительский селектор может быть заменён селектором типа, таким как `h1`, он разрешён только в начале составных селекторов, где селектор типа также был бы разрешён. Например, `span&` не допускается.

    Мы рассматриваем возможность ослабления этого ограничения. Если вы хотите помочь в этом, ознакомьтесь с [этим issue на GitHub](https://github.com/sass/sass/issues/1425).

## Добавление суффиксов {#adding-suffixes}

Вы также можете использовать родительский селектор для добавления дополнительных суффиксов к внешнему селектору. Это особенно полезно при использовании методологии, такой как [BEM](https://getbem.com/), которая использует высокоструктурированные имена классов. Пока внешний селектор заканчивается буквенно-цифровым именем (например, селекторы класса, ID и элемента), вы можете использовать родительский селектор для добавления дополнительного текста.

<div class="grid" markdown>

=== "SCSS"

    ```scss
    .accordion {
      max-width: 600px;
      margin: 4rem auto;
      width: 90%;
      font-family: "Raleway", sans-serif;
      background: #f4f4f4;

      &__copy {
        display: none;
        padding: 1rem 1.5rem 2rem 1.5rem;
        color: gray;
        line-height: 1.6;
        font-size: 14px;
        font-weight: 500;

        &--open {
          display: block;
        }
      }
    }
    ```

=== "Sass"

    ```sass
    .accordion
      max-width: 600px
      margin: 4rem auto
      width: 90%
      font-family: "Raleway", sans-serif
      background: #f4f4f4

      &__copy
        display: none
        padding: 1rem 1.5rem 2rem 1.5rem
        color: gray
        line-height: 1.6
        font-size: 14px
        font-weight: 500

        &--open
          display: block
    ```

```css title="CSS"
.accordion {
  max-width: 600px;
  margin: 4rem auto;
  width: 90%;
  font-family: "Raleway", sans-serif;
  background: #f4f4f4;
}
.accordion__copy {
  display: none;
  padding: 1rem 1.5rem 2rem 1.5rem;
  color: gray;
  line-height: 1.6;
  font-size: 14px;
  font-weight: 500;
}
.accordion__copy--open {
  display: block;
}
```

</div>

## В SassScript {#in-sass-script}

Родительский селектор также может использоваться в SassScript. Это специальное выражение, которое возвращает текущий родительский селектор в том же формате, который используется [функциями селекторов](../modules/selector#selector-values): список, разделённый запятыми (список селекторов), который содержит списки, разделённые пробелами (сложные селекторы), которые содержат строки без кавычек (составные селекторы).

<div class="grid" markdown>

=== "SCSS"

    ```scss
    .main aside:hover,
    .sidebar p {
      parent-selector: &;
      // => ((unquote(".main") unquote("aside:hover")),
      //     (unquote(".sidebar") unquote("p")))
    }
    ```

=== "Sass"

    ```sass
    .main aside:hover,
    .sidebar p
      parent-selector: &
      // => ((unquote(".main") unquote("aside:hover")),
      //     (unquote(".sidebar") unquote("p")))
    ```

```css title="CSS"
.main aside:hover,
.sidebar p {
  parent-selector: .main aside:hover, .sidebar p;
}
```

</div>

Если выражение `&` используется вне каких-либо правил стилей, оно возвращает `null`. Поскольку `null` является [ложным](../at-rules/control/if#truthiness-and-falsiness), это означает, что вы можете легко использовать его для определения, вызывается ли миксин в правиле стиля или нет.

<div class="grid" markdown>

=== "SCSS"

    ```scss
    @mixin app-background($color) {
      #{if(sass(&): '&.app-background'; else: '.app-background')} {
        background-color: $color;
        color: rgba(#fff, 0.75);
      }
    }

    @include app-background(#036);

    .sidebar {
      @include app-background(#c6538c);
    }
    ```

=== "Sass"

    ```sass
    @mixin app-background($color)
      #{if(sass(&): '&.app-background'; else: '.app-background')}
        background-color: $color
        color: rgba(#fff, 0.75)

    @include app-background(#036)

    .sidebar
      @include app-background(#c6538c)
    ```

```css title="CSS"
.app-background {
  background-color: #036;
  color: rgba(255, 255, 255, 0.75);
}

.sidebar.app-background {
  background-color: #c6538c;
  color: rgba(255, 255, 255, 0.75);
}
```

</div>

### Продвинутая вложенность {#advanced-nesting}

Вы можете использовать `&` как обычное выражение SassScript, что означает, что вы можете передавать его в функции или включать в интерполяцию — даже в другие селекторы! Использование его в комбинации с [функциями селекторов](../modules/selector#selector-values) и [правилом `@at-root`](../at-rules/at-root) позволяет вкладывать селекторы очень мощными способами.

Например, предположим, вы хотите написать селектор, который соответствует внешнему селектору *и* селектору элемента. Вы можете написать миксин, подобный этому, который использует [функцию `selector.unify()`](../modules/selector#unify) для объединения `&` с селектором пользователя.

<div class="grid" markdown>

=== "SCSS"

    ```scss
    @use "sass:selector";

    @mixin unify-parent($child) {
      @at-root #{selector.unify(&, $child)} {
        @content;
      }
    }

    .wrapper .field {
      @include unify-parent("input") {
        /* ... */
      }
      @include unify-parent("select") {
        /* ... */
      }
    }
    ```

=== "Sass"

    ```sass
    @use "sass:selector"

    @mixin unify-parent($child)
      @at-root #{selector.unify(&, $child)}
        @content

    .wrapper .field
      @include unify-parent("input")
        /* ... */

      @include unify-parent("select")
        /* ... */
    ```

```css title="CSS"
.wrapper input.field {
  /* ... */
}

.wrapper select.field {
  /* ... */
}
```

</div>

!!! tip "Совет"

    Когда Sass вкладывает селекторы, он не знает, какая интерполяция была использована для их генерации. Это означает, что он автоматически добавит внешний селектор к внутреннему селектору *даже если* вы использовали `&` как выражение SassScript. Вот почему вам нужно явно использовать [правило `@at-root`](../at-rules/at-root), чтобы указать Sass не включать внешний селектор.
