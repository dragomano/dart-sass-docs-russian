---
title: Селекторы-заполнители
icon: lucide/square-dashed-mouse-pointer
---

> В Sass есть специальный вид селектора, известный как «заполнитель». Он выглядит и ведёт себя во многом как селектор класса, но начинается с `%` и не включается в выходной CSS. Фактически, любой сложный селектор (те, что между запятыми), который даже *содержит* селектор-заполнитель, не включается в CSS, как и любое правило стиля, все селекторы которого содержат заполнители.

<div class="grid" markdown>

=== "SCSS"

    ```scss
    .alert:hover, %strong-alert {
      font-weight: bold;
    }

    %strong-alert:hover {
      color: red;
    }
    ```

=== "Sass"

    ```sass
    .alert:hover, %strong-alert
      font-weight: bold

    %strong-alert:hover
      color: red
    ```

```css title="CSS"
.alert:hover {
  font-weight: bold;
}
```

</div>

Какая польза от селектора, который не выводится? Его всё ещё можно [расширить](../at-rules/extend)! В отличие от селекторов классов, заполнители не засоряют CSS, если они не расширены, и не требуют от пользователей библиотеки использовать конкретные имена классов для их HTML.

<div class="grid" markdown>

=== "SCSS"

    ```scss
    %toolbelt {
      box-sizing: border-box;
      border-top: 1px rgba(#000, .12) solid;
      padding: 16px 0;
      width: 100%;

      &:hover { border: 2px rgba(#000, .5) solid; }
    }

    .action-buttons {
      @extend %toolbelt;
      color: #4285f4;
    }

    .reset-buttons {
      @extend %toolbelt;
      color: #cddc39;
    }
    ```

=== "Sass"

    ```sass
    %toolbelt
      box-sizing: border-box
      border-top: 1px rgba(#000, .12) solid
      padding: 16px 0
      width: 100%

      &:hover
        border: 2px rgba(#000, .5) solid

    .action-buttons
      @extend %toolbelt
      color: #4285f4


    .reset-buttons
      @extend %toolbelt
      color: #cddc39
    ```

```css title="CSS"
.reset-buttons, .action-buttons {
  box-sizing: border-box;
  border-top: 1px rgba(0, 0, 0, 0.12) solid;
  padding: 16px 0;
  width: 100%;
}
.reset-buttons:hover, .action-buttons:hover {
  border: 2px rgba(0, 0, 0, 0.5) solid;
}

.action-buttons {
  color: #4285f4;
}

.reset-buttons {
  color: #cddc39;
}
```

</div>

Селекторы-заполнители полезны при написании библиотеки Sass, где каждое правило стиля может использоваться или не использоваться. Как правило, если вы пишете таблицу стилей только для своего собственного приложения, часто лучше просто расширить селектор класса, если он доступен.
