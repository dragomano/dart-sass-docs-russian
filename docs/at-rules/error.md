---
title: "@error"
icon: lucide/at-sign
---

> При написании [миксинов](../at-rules/mixin) и [функций](../at-rules/function), которые принимают аргументы, обычно требуется убедиться, что эти аргументы имеют типы и форматы, которые ожидает ваш API. Если это не так, пользователя необходимо уведомить, а ваши миксин/функция должны прекратить выполнение.

Sass упрощает это с помощью правила `@error`, которое записывается как `@error <выражение>`. Оно выводит значение [выражения](../syntax/structure#expressions) (обычно строку) вместе с трассировкой стека, указывающей, как был вызван текущий миксин или функция. После вывода ошибки Sass прекращает компиляцию таблицы стилей и сообщает системе, которая её запускает, что произошла ошибка.

<div class="grid" markdown>

=== "SCSS"

    ```scss
    @mixin reflexive-position($property, $value) {
      @if $property != left and $property != right {
        @error "Property #{$property} must be either left or right.";
      }

      $left-value: if(sass($property == right): initial; else: $value);
      $right-value: if(sass($property == right): $value; else: initial);

      left: $left-value;
      right: $right-value;
      [dir=rtl] & {
        left: $right-value;
        right: $left-value;
      }
    }

    .sidebar {
      @include reflexive-position(top, 12px);
    }
    ```

=== "SASS"

    ```sass
    @mixin reflexive-position($property, $value)
      @if $property != left and $property != right
        @error "Property #{$property} must be either left or right."

      $left-value: if(sass($property == right): initial; else: $value)
      $right-value: if(sass($property == right): $value; else: initial)

      left: $left-value
      right: $right-value
      [dir=rtl] &
        left: $right-value
        right: $left-value

    .sidebar
      @include reflexive-position(top, 12px)
    ```

```sh title="Ошибка"
"Property top must be either left or right."
   ╷
18 │   @include reflexive-position(top, 12px);
   │   ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
   ╵
  - 18:3  root stylesheet
```

</div>
