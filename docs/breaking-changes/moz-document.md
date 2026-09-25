---
title: "-moz-document"
icon: lucide/newspaper
---

> Ранее в Firefox существовало правило @-moz-document, требовавшее специального разбора. Поскольку поддержка этого правила удаляется из Firefox, Sass находится в процессе удаления поддержки его специального разбора.

Исторически Sass поддерживал специальный разбор правила `@-moz-document`. По мере того как [Firefox отказался от поддержки этого правила](https://web.archive.org/web/20200528221656/https://www.fxsitecompat.dev/en-CA/docs/2018/moz-document-support-has-been-dropped-except-for-empty-url-prefix/), Sass также откажется от специального разбора и будет рассматривать его как неизвестное at-правило.

**Есть одно исключение**: по‑прежнему допускается функция с пустым url‑prefix, так как она используется в хаке, нацеленном на Firefox.

<div class="grid" markdown>

=== "SCSS"

    ```scss
    @-moz-document url-prefix() {
      .error {
        color: red;
      }
    }
    ```

=== "SASS"

    ```sass
    @-moz-document url-prefix()
      .error
        color: red
    ```

```css title="CSS"
@-moz-document url-prefix() {
  .error {
    color: red;
  }
}
```

</div>

## Переходный период {#transition-period}

Сначала мы будем выдавать предупреждения об устаревании для всех использований `@-moz-document`, за исключением хака с пустым url-prefix.

В Dart Sass 2.0 `@-moz-document` будет рассматриваться как неизвестное at-правило.

--8<-- "includes/silence-warnings.md"
