---
title: "@at-root"
icon: lucide/at-sign
---

> Правило `@at-root` обычно записывается как `@at-root <селектор> { ... }` и приводит к тому, что всё внутри него выводится в корне документа вместо использования обычной вложенности. Чаще всего оно используется при выполнении [продвинутой вложенности](../style-rules/parent-selector#advanced-nesting) с [родительским селектором SassScript](../style-rules/parent-selector#in-sassscript) и [функциями для работы с селекторами](../modules/selector).

Например, предположим, что вы хотите написать селектор, который соответствует внешнему селектору *и* селектору элемента. Вы можете написать миксин, подобный этому, который использует [функцию `selector.unify()`](../modules/selector#unify) для объединения `&` с селектором пользователя.

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

=== "SASS"

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

Правило `@at-root` здесь необходимо, потому что при выполнении вложения селекторов Sass не знает, какая интерполяция использовалась для формирования селектора. Это означает, что он автоматически добавит внешний селектор к внутреннему *даже если* вы использовали `&` как SassScript‑выражение. `@at-root` явно указывает Sass не включать внешний селектор (хотя он *всегда* будет включён в `&` как выражение).

!!! tip "Совет"

    Правило `@at-root` также можно записать как `@at-root { ... }`, чтобы поместить несколько стилевых правил в корень документа. Фактически, запись `@at-root <селектор> { ... }` — это всего лишь сокращение для `@at-root { <селектор> { ... } }`!

## За пределами стилевых правил {#beyond-style-rules}

Само по себе `@at-root` убирает только [стилевые правила](../style-rules). Любые at‑правила, такие как [`@media`](../at-rules/css#media) или [`@supports`](../at-rules/css#supports), останутся. Если это не то, что вам нужно, вы можете точно контролировать, какие правила включаются или исключаются, используя синтаксис, похожий на [медиа‑функции](https://developer.mozilla.org/en-US/docs/Web/CSS/Media_Queries/Using_media_queries#Targeting_media_features), в виде `@at-root (with: <rules...>) { ... }` или `@at-root (without: <rules...>) { ... }`. Запрос `(without: ...)` говорит Sass, какие правила следует исключить; запрос `(with: ...)` исключает все правила, *кроме* перечисленных.

<div class="grid" markdown>

=== "SCSS"

    ```scss
    @media print {
      .page {
        width: 8in;

        @at-root (without: media) {
          color: #111;
        }

        @at-root (with: rule) {
          font-size: 1.2em;
        }
      }
    }
    ```

=== "SASS"

    ```sass
    @media print
      .page
        width: 8in

        @at-root (without: media)
          color: #111

        @at-root (with: rule)
          font-size: 1.2em
    ```

```css title="CSS"
@media print {
  .page {
    width: 8in;
  }
}
.page {
  color: #111;
}
.page {
  font-size: 1.2em;
}
```

</div>

В дополнение к именам at-правил существует два специальных значения, которые можно использовать в запросах:

* `rule` относится к стилевым правилам. Например, `@at-root (with: rule)` исключает все at-правила, но сохраняет стилевые правила.

* `all` относится ко всем at-правилам *и* стилевым правилам, которые должны быть исключены.
