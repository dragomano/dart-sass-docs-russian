---
title: Синтаксис переменных CSS
icon: lucide/newspaper
---

> Старые версии LibSass и Ruby Sass разбирали объявления пользовательских свойств так же, как любые другие объявления свойств, разрешая полный диапазон [выражений SassScript](../syntax/structure#expressions) в качестве значений. Но это не было совместимо с CSS.

Спецификация CSS разрешает использовать практически любую строку символов в объявлении пользовательского свойства. Хотя эти значения могут не иметь смысла для любого свойства CSS, они могут быть получены из JavaScript. Когда они разбирались как значения SassScript, синтаксис, который был бы допустимым чистым CSS, не разбирался. Например, [библиотека Polymer](https://polymer-library.polymer-project.org/3.0/docs/devguide/custom-css-properties#use-custom-css-mixins) использовала это для поддержки чистых CSS-миксинов:

=== "SCSS"

    ```scss
    :root {
      --flex-theme: {
        border: 1px solid var(--theme-dark-blue);
        font-family: var(--theme-font-family);
        padding: var(--theme-wide-padding);
        background-color: var(--theme-light-blue);
      };
    }
    ```

=== "CSS"

    ```css
    :root {
      --flex-theme: {
        border: 1px solid var(--theme-dark-blue);
        font-family: var(--theme-font-family);
        padding: var(--theme-wide-padding);
        background-color: var(--theme-light-blue);
      };
    }
    ```

Чтобы обеспечить максимальную совместимость с чистым CSS, более новые версии Sass требуют, чтобы выражения SassScript в значениях пользовательских свойств записывались внутри [интерполяции](../interpolation). Интерполяция также будет работать в старых версиях Sass, поэтому рекомендуется использовать её во всех таблицах стилей.

<div class="grid" markdown>

=== "SCSS"

    ```scss
    $accent-color: #fbbc04;

    :root {
      // НЕПРАВИЛЬНО, не работает в последних версиях Sass.
      --accent-color-wrong: $accent-color;

      // ПРАВИЛЬНО, работает во всех версиях Sass.
      --accent-color-right: #{$accent-color};
    }
    ```

=== "SASS"

    ```sass
    $accent-color: #fbbc04

    :root
      // НЕПРАВИЛЬНО, не работает в последних версиях Sass.
      --accent-color-wrong: $accent-color

      // ПРАВИЛЬНО, работает во всех версиях Sass.
      --accent-color-right: #{$accent-color}
    ```

```css title="CSS"
:root {
  --accent-color-wrong: $accent-color;
  --accent-color-right: #fbbc04;
}
```

</div>

!!! tip "Совет"

    Поскольку интерполяция убирает кавычки из строк в кавычках, может потребоваться обернуть их в [функцию `meta.inspect()`](../modules/meta#inspect) для сохранения.

    <div class="grid" markdown>

    === "SCSS"

        ```scss
        @use "sass:meta";

        $font-family-monospace: Menlo, Consolas, "Courier New", monospace;

        :root {
          --font-family-monospace: #{meta.inspect($font-family-monospace)};
        }
        ```

    === "SASS"

        ```sass
        @use "sass:meta"

        $font-family-monospace: Menlo, Consolas, "Courier New", monospace

        :root
          --font-family-monospace: #{meta.inspect($font-family-monospace)}
        ```

    ```css title="CSS"
    :root {
      --font-family-monospace: Menlo, Consolas, "Courier New", monospace;
    }
    ```

    </div>
