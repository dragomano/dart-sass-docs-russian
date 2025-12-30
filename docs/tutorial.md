---
title: Обучение
icon: lucide/notepad-text
---

> Прежде чем вы сможете использовать Sass, вам необходимо настроить его в своём проекте. Если вы хотите просто просмотреть информацию здесь, пожалуйста, но мы рекомендуем сначала установить Sass. [Перейдите сюда](./install), если хотите узнать, как всё настроить.

## Препроцессинг {#preprocessing}

CSS сам по себе может быть интересным, но таблицы стилей становятся всё больше, сложнее и труднее в обслуживании. Вот где может помочь препроцессор. Sass имеет функции, которых пока нет в CSS, такие как вложенность, миксины, наследование и другие полезные возможности, которые помогают вам писать надёжный и удобный в обслуживании CSS.

Как только вы начнёте работать с Sass, он возьмёт ваш предварительно обработанный файл Sass и сохранит его как обычный CSS-файл, который вы можете использовать на своём веб-сайте.

Самый прямой способ сделать это — в терминале. После установки Sass вы можете компилировать ваш Sass в CSS с помощью команды `sass`. Вам нужно указать Sass, из какого файла выполнять сборку и куда выводить CSS. Например, выполнение `sass input.scss output.css` в терминале возьмёт один файл Sass, `input.scss`, и скомпилирует его в `output.css`.

Вы также можете отслеживать отдельные файлы или каталоги с помощью флага `--watch`. Флаг watch указывает Sass следить за изменениями в исходных файлах и перекомпилировать CSS каждый раз, когда вы сохраняете свой Sass. Если вы хотите отслеживать (вместо ручной сборки) ваш файл `input.scss`, вам просто нужно добавить флаг `--watch` к вашей команде следующим образом:

```shell
sass --watch input.scss output.css
```

Вы можете отслеживать и выводить в каталоги, используя пути к папкам в качестве ввода и вывода, разделяя их двоеточием. В этом примере:

```shell
sass --watch app/sass:public/stylesheets
```

Sass будет отслеживать все файлы в папке `app/sass` на предмет изменений и компилировать CSS в папку `public/stylesheets`.

!!! info "Информация"

    У Sass есть два синтаксиса! Синтаксис SCSS (`.scss`) используется чаще всего. Это надмножество CSS, что означает, что весь валидный CSS также является валидным SCSS. Синтаксис с отступами (`.sass`) более необычен: он использует отступы вместо фигурных скобок для вложенности операторов и новые строки вместо точек с запятой для их разделения. Все наши примеры доступны в обоих синтаксисах.

---

## Переменные {#variables}

Думайте о переменных как о способе хранения информации, которую вы хотите повторно использовать во всей таблице стилей. Вы можете хранить такие вещи, как цвета, наборы шрифтов или любое значение CSS, которое, по вашему мнению, вы захотите использовать повторно. Sass использует символ `$` для создания переменной. Вот пример:

<div class="grid" markdown>

=== "SCSS"

    ```scss
    $font-stack: Helvetica, sans-serif;
    $primary-color: #333;

    body {
      font: 100% $font-stack;
      color: $primary-color;
    }
    ```

=== "Sass"

    ```sass
    $font-stack: Helvetica, sans-serif
    $primary-color: #333

    body
      font: 100% $font-stack
      color: $primary-color
    ```

```css title="CSS"
body {
  font: 100% Helvetica, sans-serif;
  color: #333;
}
```

</div>

Когда Sass обрабатывается, он берёт переменные, которые мы определяем для `$font-stack` и `$primary-color`, и выводит обычный CSS с нашими значениями переменных, размещёнными в CSS. Это может быть чрезвычайно мощным инструментом при работе с фирменными цветами и обеспечении их единообразия по всему сайту.

---

## Вложенность {#nesting}

При написании HTML вы, вероятно, заметили, что он имеет чёткую вложенную и визуальную иерархию. CSS, с другой стороны, этого не имеет.

Sass позволит вам вкладывать CSS-селекторы таким образом, который следует той же визуальной иерархии, что и ваш HTML. Имейте в виду, что чрезмерно вложенные правила приведут к избыточно квалифицированному CSS, который может оказаться трудным в обслуживании и обычно считается плохой практикой.

С учётом этого, вот пример некоторых типичных стилей для навигации сайта:

<div class="grid" markdown>

=== "SCSS"

    ```scss
    nav {
      ul {
        margin: 0;
        padding: 0;
        list-style: none;
      }

      li { display: inline-block; }

      a {
        display: block;
        padding: 6px 12px;
        text-decoration: none;
      }
    }
    ```

=== "Sass"

    ```sass
    nav
      ul
        margin: 0
        padding: 0
        list-style: none

      li
        display: inline-block

      a
        display: block
        padding: 6px 12px
        text-decoration: none
    ```

```css title="CSS"
nav ul {
  margin: 0;
  padding: 0;
  list-style: none;
}
nav li {
  display: inline-block;
}
nav a {
  display: block;
  padding: 6px 12px;
  text-decoration: none;
}
```

</div>

Вы заметите, что селекторы `ul`, `li` и `a` вложены внутри селектора `nav`. Это отличный способ организовать ваш CSS и сделать его более читаемым.

---

## Фрагменты {#partials}

Вы можете создавать фрагменты Sass, которые содержат небольшие блоки CSS для включения в другие файлы. Это отличный способ модулировать ваш CSS и упростить его обслуживание. Фрагмент — это файл Sass, имя которого начинается с подчёркивания, например `_partial.scss`. Подчёркивание даёт Sass понять, что файл является лишь фрагментом и что он не должен генерироваться в CSS-файл. Фрагменты Sass используются с правилом `@use`.

---

## Модули {#modules}

Вам не нужно писать весь ваш Sass в одном файле. Вы можете разделить его как угодно с помощью правила `@use`. Это правило загружает другой файл Sass как *модуль*, что означает, что вы можете обращаться к его переменным, [миксинам](#mixins) и [функциям](../at-rules/functions) в вашем файле Sass с пространством имён, основанным на имени файла. Использование файла также включит CSS, который он генерирует, в ваш скомпилированный вывод!

<div class="grid" markdown>

=== "SCSS"

    ```scss title="_base.scss"
    $font-stack: Helvetica, sans-serif;
    $primary-color: #333;

    body {
      font: 100% $font-stack;
      color: $primary-color;
    }
    ```
    ```scss title="styles.scss"
    @use 'base';

    .inverse {
      background-color: base.$primary-color;
      color: white;
    }
    ```

=== "Sass"

    ```sass title="_base.sass"
    $font-stack: Helvetica, sans-serif
    $primary-color: #333

    body
      font: 100% $font-stack
      color: $primary-color
    ```
    ```sass title="styles.sass"
    @use 'base'

    .inverse
      background-color: base.$primary-color
      color: white
    ```

```css title="CSS"
body {
  font: 100% Helvetica, sans-serif;
  color: #333;
}

.inverse {
  background-color: #333;
  color: white;
}
```

</div>

Обратите внимание, что мы используем `@use 'base';` в файле `styles.scss`. При использовании файла вам не нужно указывать его расширение. Sass достаточно умён и разберётся сам.

---

## Миксины {#mixins}

Некоторые вещи в CSS писать довольно утомительно, особенно с CSS3 и множеством существующих вендорных префиксов. Миксин позволяет создавать группы CSS-деклараций для повторного использования по всему сайту. Это помогает поддерживать <abbr title="Don't Repeat Yourself — Не повторяйся">DRY</abbr> в вашем Sass. Вы можете передавать в миксины параметры, делая их более гибкими. Вот пример для `theme`.

<div class="grid" markdown>

=== "SCSS"

    ```scss
    @mixin theme($theme: DarkGray) {
      background: $theme;
      box-shadow: 0 0 1px rgba($theme, .25);
      color: #fff;
    }

    .info {
      @include theme;
    }
    .alert {
      @include theme($theme: DarkRed);
    }
    .success {
      @include theme($theme: DarkGreen);
    }
    ```

=== "Sass"

    ```sass
    @mixin theme($theme: DarkGray)
      background: $theme
      box-shadow: 0 0 1px rgba($theme, .25)
      color: #fff

    .info
      @include theme

    .alert
      @include theme($theme: DarkRed)

    .success
      @include theme($theme: DarkGreen)
    ```

```css title="CSS"
.info {
  background: DarkGray;
  box-shadow: 0 0 1px rgba(169, 169, 169, 0.25);
  color: #fff;
}

.alert {
  background: DarkRed;
  box-shadow: 0 0 1px rgba(139, 0, 0, 0.25);
  color: #fff;
}

.success {
  background: DarkGreen;
  box-shadow: 0 0 1px rgba(0, 100, 0, 0.25);
  color: #fff;
}
```

</div>

Чтобы создать миксин, вы используете директиву `@mixin` и даёте ему имя. Мы назвали наш миксин `theme`. Мы также используем переменную `$theme` внутри круглых скобок, чтобы можно было передать любую желаемую тему `theme`. После создания миксина вы можете использовать его как CSS-декларацию, начинающуюся с `@include`, за которым следует имя миксина.

---

## Расширение/Наследование {#extend-inheritance}

Использование `@extend` позволяет передавать набор CSS-свойств от одного селектора к другому. В нашем примере мы создадим простую серию сообщений для ошибок, предупреждений и успешных действий, используя другую функцию, которая идёт рука об руку с расширением — классы-заполнители. Класс-заполнитель — это особый тип класса, который выводится только при расширении и помогает сохранять скомпилированный CSS аккуратным и чистым.

<div class="grid" markdown>

=== "SCSS"

    ```scss
    /* Этот CSS будет выведен, потому что %message-shared расширяется. */
    %message-shared {
      border: 1px solid #ccc;
      padding: 10px;
      color: #333;
    }

    // Этот CSS не будет выведен, потому что %equal-heights никогда не расширяется.
    %equal-heights {
      display: flex;
      flex-wrap: wrap;
    }

    .message {
      @extend %message-shared;
    }

    .success {
      @extend %message-shared;
      border-color: green;
    }

    .error {
      @extend %message-shared;
      border-color: red;
    }

    .warning {
      @extend %message-shared;
      border-color: yellow;
    }
    ```

=== "Sass"

    ```sass
    /* Этот CSS будет выведен, потому что %message-shared расширяется. */
    %message-shared
      border: 1px solid #ccc
      padding: 10px
      color: #333

    // Этот CSS не будет выведен, потому что %equal-heights никогда не расширяется.
    %equal-heights
      display: flex
      flex-wrap: wrap

    .message
      @extend %message-shared

    .success
      @extend %message-shared
      border-color: green

    .error
      @extend %message-shared
      border-color: red

    .warning
      @extend %message-shared
      border-color: yellow
    ```

```css title="CSS"
/* Этот CSS будет выведен, потому что %message-shared расширяется. */
.warning, .error, .success, .message {
  border: 1px solid #ccc;
  padding: 10px;
  color: #333;
}

.success {
  border-color: green;
}

.error {
  border-color: red;
}

.warning {
  border-color: yellow;
}
```

</div>

Приведённый выше код указывает `.message`, `.success`, `.error` и `.warning` вести себя так же, как `%message-shared`. Это означает, что везде, где появляется `%message-shared`, также появятся `.message`, `.success`, `.error` и `.warning`. Магия происходит в сгенерированном CSS, где каждый из этих классов получит те же CSS-свойства, что и `%message-shared`. Это помогает избежать необходимости писать несколько имён классов на HTML-элементах.

Вы можете расширять большинство простых CSS-селекторов в дополнение к классам-заполнителям в Sass, но использование заполнителей — самый простой способ убедиться, что вы не расширяете класс, который вложен где-то ещё в ваших стилях, что может привести к непреднамеренным селекторам в вашем CSS.

Обратите внимание, что CSS в `%equal-heights` не генерируется, потому что `%equal-heights` никогда не расширяется.

---

## Операторы {#operators}

Выполнение математических операций в CSS очень полезно. Sass имеет несколько стандартных математических операторов, таких как `+`, `-`, `*`, `math.div()` и `%`. В нашем примере мы выполним простые вычисления для расчёта ширины для `article` и `aside`.

<div class="grid" markdown>

=== "SCSS"

    ```scss
    @use "sass:math";

    .container {
      display: flex;
    }

    article[role="main"] {
      width: math.div(600px, 960px) * 100%;
    }

    aside[role="complementary"] {
      width: math.div(300px, 960px) * 100%;
      margin-left: auto;
    }
    ```

=== "Sass"

    ```sass
    @use "sass:math"

    .container
      display: flex

    article[role="main"]
      width: math.div(600px, 960px) * 100%

    aside[role="complementary"]
      width: math.div(300px, 960px) * 100%
      margin-left: auto
    ```

```css title="CSS"
.container {
  display: flex;
}

article[role=main] {
  width: 62.5%;
}

aside[role=complementary] {
  width: 31.25%;
  margin-left: auto;
}
```

</div>

Мы создали очень простую адаптивную сетку на основе 960px. Операции в Sass позволяют нам делать такие вещи, как брать значения в пикселях и конвертировать их в проценты без особых хлопот.
