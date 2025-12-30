---
title: Объявления свойств
icon: lucide/table-properties
---

> В Sass, как и в CSS, объявления свойств определяют, как стилизуются элементы, которые совпадают с селектором. Но Sass добавляет дополнительные возможности, упрощающие их написание и автоматизацию. Прежде всего, значение объявления может быть любым [выражением SassScript](../syntax/structure#expressions), которое будет вычислено и включено в результат.

<div class="grid" markdown>

=== "SCSS"

    ```scss
    .circle {
      $size: 100px;
      width: $size;
      height: $size;
      border-radius: $size * 0.5;
    }
    ```

=== "Sass"

    ```sass
    .circle
      $size: 100px
      width: $size
      height: $size
      border-radius: $size * 0.5
    ```

```css title="CSS"
.circle {
  width: 100px;
  height: 100px;
  border-radius: 50px;
}
```

</div>

## Интерполяция {#interpolation}

Имя свойства может включать [интерполяцию](../interpolation), что позволяет динамически генерировать свойства по мере необходимости. Вы даже можете интерполировать всё имя свойства целиком!

<div class="grid" markdown>

=== "SCSS"

    ```scss
    @mixin prefix($property, $value, $prefixes) {
      @each $prefix in $prefixes {
        -#{$prefix}-#{$property}: $value;
      }
      #{$property}: $value;
    }

    .gray {
      @include prefix(filter, grayscale(50%), moz webkit);
    }
    ```

=== "Sass"

    ```sass
    @mixin prefix($property, $value, $prefixes)
      @each $prefix in $prefixes
        -#{$prefix}-#{$property}: $value

      #{$property}: $value

    .gray
      @include prefix(filter, grayscale(50%), moz webkit)
    ```

```css title="CSS"
.gray {
  -moz-filter: grayscale(50%);
  -webkit-filter: grayscale(50%);
  filter: grayscale(50%);
}
```

</div>

## Вложенность {#nesting}

Многие CSS-свойства начинаются с одного и того же префикса, который действует как своего рода пространство имён. Например, `font-family`, `font-size` и `font-weight` начинаются с `font-`. Sass делает это проще и менее избыточным, позволяя вкладывать объявления свойств. Имена внешних свойств добавляются к внутренним через дефис.

<div class="grid" markdown>

=== "SCSS"

    ```scss
    .enlarge {
      font-size: 14px;
      transition: {
        property: font-size;
        duration: 4s;
        delay: 2s;
      }

      &:hover { font-size: 36px; }
    }
    ```

=== "Sass"

    ```sass
    .enlarge
      font-size: 14px
      transition:
        property: font-size
        duration: 4s
        delay: 2s

      &:hover
        font-size: 36px
    ```

```css title="CSS"
.enlarge {
  font-size: 14px;
  transition-property: font-size;
  transition-duration: 4s;
  transition-delay: 2s;
}
.enlarge:hover {
  font-size: 36px;
}
```

</div>

Некоторые из этих CSS-свойств имеют сокращённые версии, которые используют пространство имён в качестве имени свойства. Для них вы можете писать как сокращённое значение, *так и* более явные вложенные версии.

<div class="grid" markdown>

=== "SCSS"

    ```scss
    .info-page {
      margin: auto {
        bottom: 10px;
        top: 2px;
      }
    }
    ```

=== "Sass"

    ```sass
    .info-page
      margin: auto
        bottom: 10px
        top: 2px
    ```

```css title="CSS"
.info-page {
  margin: auto;
  margin-bottom: 10px;
  margin-top: 2px;
}
```

</div>

## Скрытые объявления {#hidden-declarations}

Иногда вы хотите, чтобы объявление свойства отображалось только в некоторых случаях. Если значение объявления — [`null`](../values/null) или пустая [строка без кавычек](../values/strings#unquoted), Sass вообще не скомпилирует это объявление в CSS.

<div class="grid" markdown>

=== "SCSS"

    ```scss
    $rounded-corners: false;

    .button {
      border: 1px solid black;
      border-radius: if(sass($rounded-corners): 5px);
    }
    ```

=== "Sass"

    ```sass
    $rounded-corners: false

    .button
      border: 1px solid black
      border-radius: if(sass($rounded-corners): 5px)
    ```

```css title="CSS"
.button {
  border: 1px solid black;
}
```

</div>

## Пользовательские свойства {#custom-properties}

[Пользовательские свойства CSS](https://developer.mozilla.org/en-US/docs/Web/CSS/--*), также известные как CSS-переменные, имеют необычный синтаксис объявления: они позволяют использовать практически любой текст в значениях своих объявлений. Более того, эти значения доступны для JavaScript, поэтому любое значение потенциально может быть важным для пользователя. Это включает значения, которые обычно распознавались бы как SassScript.

Из-за этого Sass анализирует объявления пользовательских свойств иначе, чем другие объявления свойств. Все токены, включая те, которые выглядят как SassScript, передаются в CSS как есть. Единственное исключение — [интерполяция](../interpolation), которая является единственным способом внедрить динамические значения в пользовательское свойство.

<div class="grid" markdown>

=== "SCSS"

    ```scss
    $primary: #81899b;
    $accent: #302e24;
    $warn: #dfa612;

    :root {
      --primary: #{$primary};
      --accent: #{$accent};
      --warn: #{$warn};

      // Хотя это выглядит как переменная Sass,
      // это допустимый CSS, поэтому он не вычисляется.
      --consumed-by-js: $primary;
    }
    ```

=== "Sass"

    ```sass
    $primary: #81899b
    $accent: #302e24
    $warn: #dfa612

    :root
      --primary: #{$primary}
      --accent: #{$accent}
      --warn: #{$warn}

      // Хотя это выглядит как переменная Sass,
      // это допустимый CSS, поэтому он не вычисляется.
      --consumed-by-js: $primary
    ```

```css title="CSS"
:root {
  --primary: #81899b;
  --accent: #302e24;
  --warn: #dfa612;
  --consumed-by-js: $primary;
}
```

</div>

!!! note "Примечание"

    К сожалению, [интерполяция](/interpolation) удаляет кавычки из строк, что затрудняет использование строк в кавычках в качестве значений для пользовательских свойств, когда они поступают из переменных Sass. В качестве обходного пути вы можете использовать функцию [`meta.inspect()`](/modules/meta#inspect) для сохранения кавычек.

    <div class="grid" markdown>

    === "SCSS"

        ```scss
        @use "sass:meta";

        $font-family-sans-serif: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto;
        $font-family-monospace: SFMono-Regular, Menlo, Monaco, Consolas;

        :root {
          --font-family-sans-serif: #{meta.inspect($font-family-sans-serif)};
          --font-family-monospace: #{meta.inspect($font-family-monospace)};
        }
        ```

    === "Sass"

        ```sass
        @use "sass:meta"

        $font-family-sans-serif: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto
        $font-family-monospace: SFMono-Regular, Menlo, Monaco, Consolas

        :root
          --font-family-sans-serif: #{meta.inspect($font-family-sans-serif)}
          --font-family-monospace: #{meta.inspect($font-family-monospace)}
        ```

    ```css title="CSS"
    :root {
      --font-family-sans-serif: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto;
      --font-family-monospace: SFMono-Regular, Menlo, Monaco, Consolas;
    }
    ```

    </div>

### Результаты `@function` {#function-results}

Свойство `result` [правила `@function` в чистом CSS](https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/At-rules/@function) работает как пользовательское свойство: оно также может быть включено в любое место значения свойства, доступно из JavaScript и, следовательно, может принимать практически любое возможное значение. Sass анализирует его так же, как значение пользовательского свойства, что означает, что вы должны использовать интерполяцию для включения значений SassScript в него.

<div class="grid" markdown>

=== "SCSS"

    ```scss
    $highlight: #ddf;

    @function --highlight() {
      result: var(--highlight, #{$highlight});
    }
    ```

=== "Sass"

    ```sass
    $highlight: #ddf

    @function --highlight()
      result: var(--highlight, #{$highlight})
    ```

```css title="CSS"
@function --highlight() {
  result: var(--highlight, #ddf);
}
```

</div>
