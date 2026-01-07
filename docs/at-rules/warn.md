---
title: "@warn"
icon: lucide/at-sign
---

> При написании [миксинов](../at-rules/mixin) и [функций](../at-rules/function) вы можете захотеть отговорить пользователей от передачи определённых аргументов или определённых значений. Они могут передавать устаревшие аргументы, которые теперь не рекомендуются, или они могут вызывать ваш API способом, который не совсем оптимален.

Правило `@warn` создано именно для этого. Оно записывается как `@warn <выражение>` и выводит значение [выражения](../syntax/structure#expressions) (обычно строку) для пользователя вместе с трассировкой стека, указывающей, как был вызван текущий миксин или функция. Однако, в отличие от [правила `@error`](../at-rules/error), оно не останавливает Sass полностью.

<div class="grid" markdown>

=== "SCSS"

    ```scss
    $known-prefixes: webkit, moz, ms, o;

    @mixin prefix($property, $value, $prefixes) {
      @each $prefix in $prefixes {
        @if not index($known-prefixes, $prefix) {
          @warn "Unknown prefix #{$prefix}.";
        }

        -#{$prefix}-#{$property}: $value;
      }
      #{$property}: $value;
    }

    .tilt {
      // Ой, мы опечатались в "webkit", написав "wekbit"!
      @include prefix(transform, rotate(15deg), wekbit ms);
    }
    ```

=== "SASS"

    ```sass
    $known-prefixes: webkit, moz, ms, o

    @mixin prefix($property, $value, $prefixes)
      @each $prefix in $prefixes
        @if not index($known-prefixes, $prefix)
          @warn "Unknown prefix #{$prefix}."

        -#{$prefix}-#{$property}: $value

      #{$property}: $value

    .tilt
      // Ой, мы опечатались в "webkit", написав "wekbit"!
      @include prefix(transform, rotate(15deg), wekbit ms)
    ```

```css title="CSS"
.tilt {
  -wekbit-transform: rotate(15deg);
  -ms-transform: rotate(15deg);
  transform: rotate(15deg);
}
```

</div>

Точный формат предупреждения и трассировки стека различается в зависимости от реализации. Вот как это выглядит в Dart Sass:

```sh
Warning: Unknown prefix wekbit.
    example.scss 6:7   prefix()
    example.scss 16:3  root stylesheet
```
