---
title: Специальные функции
icon: lucide/square-function
---

> CSS определяет множество функций, и большинство из них прекрасно работают с обычным синтаксисом функций Sass. Они анализируются как вызовы функций, преобразуются в [простые CSS-функции](/at-rules/function/#plain-css-functions) и компилируются как есть в CSS. Однако есть несколько исключений, которые имеют специальный синтаксис, не поддающийся простому анализу как [выражение SassScript](structure#expressions). Все специальные вызовы функций возвращают [строки без кавычек](/values/strings#unquoted).

## `if()`

Sass поддерживает [CSS-функцию `if()`](https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/Values/if) с одним важным дополнением: условие `sass(...)`, которое принимает выражение SassScript и совпадает, если это выражение вычисляется в [истинное](/at-rules/control/if#truthiness-and-falsiness) значение. Функция `if()`, которая содержит только условия `sass(...)` (и опционально `else`), будет полностью вычислена Sass и вернёт соответствующее значение.

SassScript в условиях функции `if()` разрешён *только* внутри условия `sass(...)` или в [интерполяции](/interpolation). Значения, с другой стороны, являются обычными выражениями SassScript и не требуют никакой специальной обёртки. Будет вычислено только то значение, чьё условие совпадёт, поэтому другие значения могут ссылаться на несуществующие переменные или вызывать функции, которые вызовут ошибку.

Если ни одно условие в чистой Sass-функции `if()` не совпадёт, она возвращает `null`.

=== "SCSS"

    ```scss
    @use 'sass:meta';

    $hungry: true;
    @debug if(sass($hungry): breakfast burrito; else: cereal); // breakfast burrito

    // Вы можете использовать CSS булевы выражения с условиями sass(...).
    @debug if(not sass($hungry): skip lunch); // null

    // Вычисляется только соответствующая ветка.
    @debug if(sass(meta.variable-exists("thirsty")): thirsty; else: hungry); // hungry
    ```

=== "Sass"

    ```sass
    @use 'sass:meta'

    $hungry: true
    @debug if(sass($hungry): breakfast burrito; else: cereal)  // breakfast burrito

    // Вы можете использовать CSS булевы выражения с условиями sass(...).
    @debug if(not sass($hungry): skip lunch)  // null

    // Вычисляется только соответствующая ветка.
    @debug if(sass(meta.variable-exists("thirsty")): thirsty; else: hungry)  // hungry
    ```

Условия `sass(...)` также могут комбинироваться с обычными CSS-условиями. Условия Sass будут вычислены Sass, но если останутся какие-либо CSS-условия, Sass вернёт весь результат в виде строки.

=== "SCSS"

    ```scss
    $support-widescreen: true;
    @debug if(
      sass($support-widescreen) and media(width >= 3000px): big;
      else: small
    ); // if(media(width >= 3000px): big; else: small)

    // Если условия Sass означают, что ветка никогда не совпадёт (или всегда совпадёт), Sass
    // немедленно удаляет эту ветку и возвращает окончательное значение, если это возможно.
    $support-widescreen: false;
    @debug if(
      sass($support-widescreen) and media(width >= 3000px): big;
      else: small
    ); // small
    ```

=== "Sass"

    ```sass
    $support-widescreen: true
    @debug if(
      sass($support-widescreen) and media(width >= 3000px): big;
      else: small
    )  // if(media(width >= 3000px): big; else: small)

    // Если условия Sass означают, что ветка никогда не совпадёт (или всегда совпадёт), Sass
    // немедленно удаляет эту ветку и возвращает окончательное значение, если это возможно.
    $support-widescreen: false
    @debug if(
      sass($support-widescreen) and media(width >= 3000px): big;
      else: small
    )  // small
    ```

## `url()`

[Функция `url()`](https://developer.mozilla.org/en-US/docs/Web/CSS/url) обычно используется в CSS, но её синтаксис отличается от других функций: она может принимать как URL в кавычках, *так и* без кавычек. Поскольку URL без кавычек не является валидным выражением SassScript, Sass требуется специальная логика для его анализа.

Если аргумент `url()` является валидным URL без кавычек, Sass анализирует его как есть, хотя [интерполяция](/interpolation) также может использоваться для внедрения значений SassScript. Если это не валидный URL без кавычек — например, если он содержит [переменные](variables) или [вызовы функций](at-rules/function) — он анализируется как обычный [вызов простой CSS-функции](at-rules/function/#plain-css-functions).

<div class="grid" markdown>

=== "SCSS"

    ```scss
    $roboto-font-path: "../fonts/roboto";

    @font-face {
      // Это анализируется как обычный вызов функции, который принимает строку в кавычках.
      src: url("#{$roboto-font-path}/Roboto-Thin.woff2") format("woff2");

      font-family: "Roboto";
      font-weight: 100;
    }

    @font-face {
      // Это анализируется как обычный вызов функции, который принимает арифметическое
      // выражение.
      src: url($roboto-font-path + "/Roboto-Light.woff2") format("woff2");

      font-family: "Roboto";
      font-weight: 300;
    }

    @font-face {
      // Это анализируется как специальная функция с интерполяцией.
      src: url(#{$roboto-font-path}/Roboto-Regular.woff2) format("woff2");

      font-family: "Roboto";
      font-weight: 400;
    }
    ```

=== "Sass"

    ```sass
    $roboto-font-path: "../fonts/roboto"

    @font-face
      // Это анализируется как обычный вызов функции, который принимает строку в кавычках.
      src: url("#{$roboto-font-path}/Roboto-Thin.woff2") format("woff2")

      font-family: "Roboto"
      font-weight: 100


    @font-face
      // Это анализируется как обычный вызов функции, который принимает арифметическое
      // выражение.
      src: url($roboto-font-path + "/Roboto-Light.woff2") format("woff2")

      font-family: "Roboto"
      font-weight: 300


    @font-face
      // Это анализируется как специальная функция с интерполяцией.
      src: url(#{$roboto-font-path}/Roboto-Regular.woff2) format("woff2")

      font-family: "Roboto"
      font-weight: 400
    ```

```css title="CSS"
@font-face {
  src: url("../fonts/roboto/Roboto-Thin.woff2") format("woff2");
  font-family: "Roboto";
  font-weight: 100;
}
@font-face {
  src: url("../fonts/roboto/Roboto-Light.woff2") format("woff2");
  font-family: "Roboto";
  font-weight: 300;
}
@font-face {
  src: url(../fonts/roboto/Roboto-Regular.woff2) format("woff2");
  font-family: "Roboto";
  font-weight: 400;
}
```

</div>

## `element()`, `progid:...()` и `expression()` {#element-progid-and-expression}

[Функция `element()`](https://developer.mozilla.org/en-US/docs/Web/CSS/element) определена в спецификации CSS, и поскольку её идентификаторы могут быть проанализированы как цвета, они требуют специального анализа.

[`expression()`](https://blogs.msdn.microsoft.com/ie/2008/10/16/ending-expressions/) и функции, начинающиеся с [`progid:`](https://blogs.msdn.microsoft.com/ie/2009/02/19/the-css-corner-using-filters-in-ie8/), являются устаревшими функциями Internet Explorer, которые используют нестандартный синтаксис. Хотя они больше не поддерживаются современными браузерами, Sass продолжает их анализировать для обратной совместимости.

Sass разрешает *любой текст* в этих вызовах функций, включая вложенные скобки. Ничто не интерпретируется как выражение SassScript, за исключением того, что [интерполяция](/interpolation) может использоваться для внедрения динамических значений.


<div class="grid" markdown>

=== "SCSS"

    ```scss
    $logo-element: logo-bg;

    .logo {
      background: element(##{$logo-element});
    }
    ```

=== "Sass"

    ```sass
    $logo-element: logo-bg

    .logo
      background: element(##{$logo-element})
    ```

```css title="CSS"
.logo {
  background: element(#logo-bg);
}
```

</div>
