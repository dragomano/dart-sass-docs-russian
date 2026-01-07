---
title: "@debug"
icon: lucide/at-sign
---

> Иногда полезно видеть значение [переменной](../variables) или [выражения](../syntax/structure#expressions) во время разработки таблицы стилей. Для этого существует правило `@debug`: оно записывается как `@debug <выражение>` и выводит значение этого выражения вместе с именем файла и номером строки.

<div class="grid" markdown>

=== "SCSS"

    ```scss
    @mixin inset-divider-offset($offset, $padding) {
      $divider-offset: (2 * $padding) + $offset;
      @debug "divider offset: #{$divider-offset}";

      margin-left: $divider-offset;
      width: calc(100% - #{$divider-offset});
    }
    ```

=== "SASS"

    ```sass
    @mixin inset-divider-offset($offset, $padding)
      $divider-offset: (2 * $padding) + $offset
      @debug "divider offset: #{$divider-offset}"

      margin-left: $divider-offset
      width: calc(100% - #{$divider-offset})
    ```

</div>

Точный формат отладочного сообщения различается в зависимости от реализации. Вот как это выглядит в Dart Sass:

```sh
test.scss:3 Debug: divider offset: 132px
```

!!! tip "Совет"

    Вы можете передавать в `@debug` любое значение, а не только строку! Оно выводит то же представление этого значения, что и функция [`meta.inspect()`](../modules/meta#inspect).
