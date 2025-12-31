---
title: "@import"
icon: lucide/at-sign
---

> Sass расширяет [правило `@import`](https://developer.mozilla.org/en-US/docs/Web/CSS/@import) из CSS, добавляя возможность импортировать таблицы стилей Sass и CSS, предоставляя доступ к [миксинам](../at-rules/mixin), [функциям](../at-rules/function) и [переменным](../variables) и объединяя CSS нескольких таблиц стилей вместе. В отличие от обычных импортов CSS, которые требуют от браузера выполнения нескольких HTTP-запросов при отрисовке страницы, импорты Sass полностью обрабатываются во время компиляции.

Импорты Sass имеют тот же синтаксис, что и импорты CSS, за исключением того, что они позволяют разделять несколько импортов запятыми, а не требовать для каждого отдельного `@import`. Кроме того, в [синтаксисе с отступами](../syntax#the-indented-syntax) импортируемые URL не обязаны быть в кавычках.

!!! warning "Внимание"

    Начиная с Dart Sass 1.80.0, правило `@import` [объявлено устаревшим](../breaking-changes/import) и будет удалено из языка в Dart Sass 3.0.0. Вместо него используйте [правило `@use`](../at-rules/use).

    <h4>Что не так с `@import`?</h4>

    У правила `@import` есть ряд серьезных проблем:

    * `@import` делает все переменные, миксины и функции глобально доступными. Это очень затрудняет понимание (как людям, так и инструментам), где что определено.

    * Поскольку всё является глобальным, библиотеки должны добавлять префикс ко всем своим элементам, чтобы избежать конфликтов имён.

    * [Правила `@extend`](../at-rules/extend) также являются глобальными, что затрудняет предсказание того, какие правила стилей будут расширены.

    * Каждая таблица стилей выполняется и её CSS выводится *каждый раз*, когда она импортируется через `@import`, что увеличивает время компиляции и создает раздутый результат.

    * Не было способа определить приватные элементы или селекторы-заполнители, недоступные для последующих таблиц стилей.

    Новая модульная система и правило `@use` решают все эти проблемы.

    <h4>Как мне мигрировать?</h4>

    Мы написали [инструмент миграции](../cli/migrator), который автоматически и мгновенно конвертирует большую часть кода на `@import` в код на `@use`. Просто укажите ему на ваши точки входа и запустите!

<div class="grid" markdown>

=== "SCSS"

    ```scss title="foundation/_code.scss"
    code {
      padding: .25em;
      line-height: 0;
    }
    ```
    ```scss title="foundation/_lists.scss"
    ul, ol {
      text-align: left;

      & & {
        padding: {
          bottom: 0;
          left: 0;
        }
      }
    }
    ```
    ```scss title="style.scss"
    @import 'foundation/code', 'foundation/lists';
    ```

=== "Sass"

    ```sass title="foundation/_code.sass"
    code
      padding: .25em
      line-height: 0
    ```
    ```sass title="foundation/_lists.sass"
    ul, ol
      text-align: left

      & &
        padding:
          bottom: 0
          left: 0
    ```
    ```sass title="style.sass"
    @import foundation/code, foundation/lists
    ```

```css title="CSS"
code {
  padding: .25em;
  line-height: 0;
}

ul, ol {
  text-align: left;
}
ul ul, ol ol {
  padding-bottom: 0;
  padding-left: 0;
}
```

</div>

Когда Sass импортирует файл, этот файл вычисляется так, как если бы его содержимое находилось непосредственно на месте `@import`. Любые [миксины](../at-rules/mixin), [функции](../at-rules/function) и [переменные](../variables) из импортированного файла становятся доступными, а весь его CSS включается точно в то место, где был написан `@import`. Более того, любые миксины, функции или переменные, определенные до `@import` (включая те, что были получены из других `@import`), доступны в импортированной таблице стилей.

!!! note "Примечание"

    Если одна и та же таблица стилей импортируется более одного раза, она будет вычисляться заново каждый раз. Если она просто определяет функции и миксины, это обычно не проблема, но если она содержит правила стилей, они будут скомпилированы в CSS более одного раза.

## Поиск файла {#finding-the-file}

Не было бы весело писать абсолютные URL для каждой импортируемой таблицы стилей, поэтому алгоритм Sass для поиска файла для импорта немного упрощает задачу. Для начала вам не нужно явно указывать расширение файла, который хотите импортировать; `@import "variables"` автоматически загрузит `variables.scss`, `variables.sass` или `variables.css`.

!!! tip "Совет"

    Чтобы таблицы стилей работали на всех операционных системах, Sass импортирует файлы по *URL*, а не по *путям к файлам*. Это означает, что нужно использовать прямые слеши, а не обратные, даже в Windows.

### Пути загрузки {#load-paths}

Все реализации Sass позволяют пользователям указывать *пути загрузки*: пути в файловой системе, в которых Sass будет искать файлы при разрешении импортов. Например, если вы передадите `node_modules/susy/sass` в качестве пути загрузки, вы сможете использовать `@import "susy"` для загрузки `node_modules/susy/sass/susy.scss`.

Однако импорты всегда сначала разрешаются относительно текущего файла. Пути загрузки используются только если не существует относительного файла, соответствующего импорту. Это гарантирует, что вы не сможете случайно нарушить свои относительные импорты при добавлении новой библиотеки.

!!! note "Примечание"

    В отличие от некоторых других языков, Sass не требует использовать `./` для относительных импортов. Относительные импорты всегда доступны.

### Фрагменты {#partials}

По соглашению, файлы Sass, которые предназначены только для импорта, а не для самостоятельной компиляции, начинаются с `_` (например, `_code.scss`). Такие файлы называются *фрагментами*, и они сообщают инструментам Sass не пытаться компилировать их самостоятельно. При импорте фрагмента можно опускать `_`.

### Индексные файлы {#index-files}

Если вы создадите файл `_index.scss` или `_index.sass` в папке, то при импорте самой папки этот файл будет загружен вместо неё.

<div class="grid" markdown>

=== "SCSS"

    ```scss title="foundation/_code.scss"
    code {
      padding: .25em;
      line-height: 0;
    }
    ```
    ```scss title="foundation/_lists.scss"
    ul, ol {
      text-align: left;

      & & {
        padding: {
          bottom: 0;
          left: 0;
        }
      }
    }
    ```
    ```scss title="foundation/_index.scss"
    @import 'code', 'lists';
    ```
    ```scss title="style.scss"
    @import 'foundation';
    ```

=== "Sass"

    ```sass title="foundation/_code.sass"
    code
      padding: .25em
      line-height: 0
    ```
    ```sass title="foundation/_lists.sass"
    ul, ol
      text-align: left

      & &
        padding:
          bottom: 0
          left: 0
    ```
    ```sass title="foundation/_index.sass"
    @import code, lists
    ```
    ```sass title="style.sass"
    @import foundation
    ```

```css title="CSS"
code {
  padding: .25em;
  line-height: 0;
}

ul, ol {
  text-align: left;
}
ul ul, ol ol {
  padding-bottom: 0;
  padding-left: 0;
}
```

</div>

### Пользовательские импортеры {#custom-importers}

Все реализации Sass предоставляют способ определения пользовательских импортеров, которые контролируют, как `@import` находит таблицы стилей:

* [Node Sass](https://npmjs.com/package/node-sass) и [Dart Sass на npm](https://npmjs.com/package/sass) предоставляют опцию [`importer`](https://github.com/sass/node-sass#importer--v200---experimental) как часть своего JS API.

* [Dart Sass на pub](https://pub.dartlang.org/packages/sass) предоставляет абстрактный класс [`Importer`](https://pub.dartlang.org/documentation/sass/latest/sass/Importer-class.html), который может быть расширен пользовательским импортером.

* [Ruby Sass](https://sass-lang.com/ruby-sass/) предоставляет абстрактный класс [`Importers::Base`](https://www.rubydoc.info/gems/sass/Sass/Importers/Base), который может быть расширен пользовательским импортером.

## Вложенность {#nesting}

Обычно импорты пишутся на верхнем уровне таблицы стилей, но это не обязательно. Их можно вкладывать в [правила стилей](../style-rules) или [обычные at-правила CSS](../at-rules/css). Импортированный CSS вкладывается в этот контекст, что делает вложенные импорты полезными для ограничения области действия фрагмента CSS определённым элементом или медиа-запросом. Миксины, функции и переменные верхнего уровня, определенные во вложенном импорте, доступны только во вложенном контексте.

<div class="grid" markdown>

=== "SCSS"

    ```scss title="_theme.scss"
    pre, code {
      font-family: 'Source Code Pro', Helvetica, Arial;
      border-radius: 4px;
    }
    ```
    ```scss title="style.scss"
    .theme-sample {
      @import "theme";
    }
    ```

=== "Sass"

    ```sass title="_theme.sass"
    pre, code
      font-family: 'Source Code Pro', Helvetica, Arial
      border-radius: 4px
    ```
    ```sass title="style.sass"
    .theme-sample
      @import theme
    ```

```css title="CSS"
.theme-sample pre, .theme-sample code {
  font-family: 'Source Code Pro', Helvetica, Arial;
  border-radius: 4px;
}
```

</div>

!!! tip "Совет"

    Вложенные импорты очень полезны для ограничения области действия сторонних таблиц стилей, но если вы являетесь автором импортируемой таблицы стилей, обычно лучше написать свои стили в [миксине](../at-rules/mixin) и подключить этот миксин во вложенный контекст. Миксин можно использовать более гибкими способами, и при просмотре импортируемой таблицы стилей становится понятнее, как она должна использоваться.

!!! note "Примечание"

    CSS во вложенных импортах вычисляется как миксин, что означает, что любые [родительские селекторы](../style-rules/parent-selector) будут ссылаться на селектор, в который вложена таблица стилей.

    <div class="grid" markdown>

    === "SCSS"

        ```scss title="_theme.scss"
        ul li {
          $padding: 16px;
          padding-left: $padding;
          [dir=rtl] & {
            padding: {
              left: 0;
              right: $padding;
            }
          }
        }
        ```
        ```scss title="style.scss"
        .theme-sample {
          @import "theme";
        }
        ```

    === "Sass"

        ```sass title="_theme.sass"
        ul li
          $padding: 16px
          padding-left: $padding
          [dir=rtl] &
            padding:
              left: 0
              right: $padding
        ```
        ```sass title="style.sass"
        .theme-sample
          @import theme
        ```

    ```css title="CSS"
    .theme-sample ul li {
      padding-left: 16px;
    }
    [dir=rtl] .theme-sample ul li {
      padding-left: 0;
      padding-right: 16px;
    }
    ```

    </div>

## Импорт CSS {#importing-css}

Помимо импорта файлов `.sass` и `.scss`, Sass может импортировать обычные старые файлы `.css`. Единственное правило заключается в том, что импорт *не должен* явно включать расширение `.css`, так как оно используется для обозначения [встроенной директивы `@import`](#plain-css-imports).

<div class="grid" markdown>

=== "SCSS"

    ```scss title="code.css"
    code {
      padding: .25em;
      line-height: 0;
    }
    ```
    ```scss title="style.scss"
    @import 'code';
    ```

=== "Sass"

    ```sass title="code.css"
    code {
      padding: .25em;
      line-height: 0;
    }
    ```
    ```sass title="style.sass"
    @import code
    ```

```css title="CSS"
code {
  padding: .25em;
  line-height: 0;
}
```

</div>

Файлы CSS, импортируемые Sass, не поддерживают никаких специальных возможностей Sass. Чтобы авторы случайно не писали Sass в своем CSS, все функции Sass, которые не являются валидным CSS, будут вызывать ошибки. В остальном CSS будет отрендерен как есть. Его даже можно [расширять](../at-rules/extend)!

## Встроенная директива `@import` {#plain-css-imports}

Поскольку директива `@import` существует и в обычном CSS, Sass необходим способ компиляции обычных CSS-импортов без попытки импортировать файлы во время компиляции. Чтобы достичь этого и гарантировать, что SCSS является надмножеством CSS настолько, насколько это возможно, Sass будет компилировать любые `@import` со следующими характеристиками в обычные CSS-импорты:

* Импорты, где URL заканчивается на `.css`.
* Импорты, где URL начинается с `http://` или `https://`.
* Импорты, где URL записан как `url()`.
* Импорты, которые имеют медиа-запросы.

<div class="grid" markdown>

=== "SCSS"

    ```scss title="code.css"
    @import "theme.css";
    @import "http://fonts.googleapis.com/css?family=Droid+Sans";
    @import url(theme);
    @import "landscape" screen and (orientation: landscape);
    ```

=== "Sass"

    ```sass title="code.css"
    @import "theme.css"
    @import "http://fonts.googleapis.com/css?family=Droid+Sans"
    @import url(theme)
    @import "landscape" screen and (orientation: landscape)
    ```

</div>

### Интерполяция {#interpolation}

Хотя импорты Sass не могут использовать [интерполяцию](../interpolation) (чтобы всегда можно было определить, откуда берутся [миксины](../at-rules/mixin), [функции](../at-rules/function) и [переменные](../variables)), обычные CSS-импорты могут. Это позволяет динамически генерировать импорты, например, на основе параметров миксина.

<div class="grid" markdown>

=== "SCSS"

    ```scss title="code.css"
    @mixin google-font($family) {
      @import url("http://fonts.googleapis.com/css?family=#{$family}");
    }

    @include google-font("Droid Sans");
    ```

=== "Sass"

    ```sass title="code.css"
    @mixin google-font($family)
      @import url("http://fonts.googleapis.com/css?family=#{$family}")

    @include google-font("Droid Sans")
    ```

</div>

## Импорт и модули {#import-and-modules}

[Модульная система](../at-rules/use) Sass бесшовно интегрируется с `@import`, независимо от того, импортируете ли вы файл, содержащий правила `@use`, или загружаете файл, содержащий импорты, как модуль. Мы хотим сделать переход с `@import` на `@use` как можно более плавным.

### Импорт файла модульной системы {#importing-a-module-system-file}

Когда вы импортируете файл, содержащий правила `@use`, импортирующий файл получает доступ ко всем элементам (даже приватным), определённым непосредственно в этом файле, но *не* к элементам из модулей, которые этот файл загрузил. Однако, если этот файл содержит [правила `@forward`](../at-rules/forward), импортирующий файл будет иметь доступ к перенаправленным элементам. Это означает, что вы можете импортировать библиотеку, написанную для использования с модульной системой.

!!! warning "Осторожно"

    Когда импортируется файл с правилами `@use`, весь CSS, транзитивно загруженный ими, включается в результирующую таблицу стилей, даже если он уже был включен другим импортом. Если не быть осторожным, это может привести к раздуванию выходного CSS!

#### Файлы только для импорта {#import-only-files}

API, который имеет смысл для `@use`, может не иметь смысла для `@import`. Например, `@use` по умолчанию добавляет пространство имён ко всем элементам, поэтому вы можете безопасно использовать короткие имена, но `@import` этого не делает, поэтому вам может понадобиться что-то более длинное. Если вы автор библиотеки, вы можете беспокоиться, что при обновлении библиотеки для использования новой модульной системы ваши существующие пользователи, использующие `@import`, столкнутся с проблемами.

Чтобы облегчить это, Sass также поддерживает *файлы только для импорта*. Если вы назовете файл `<name>.import.scss`, он будет загружаться только при импорте, но не при использовании `@use`. Таким образом, вы можете сохранить совместимость для пользователей `@import`, предоставляя при этом удобный API для пользователей новой модульной системы.

<div class="grid" markdown>

=== "SCSS"

    ```scss title="_reset.scss"

    // Пользователи модульной системы пишут `@include reset.list()`.
    @mixin list() {
      ul {
        margin: 0;
        padding: 0;
        list-style: none;
      }
    }
    ```
    ```scss title="_reset.import.scss"

    // Пользователи устаревшего импорта могут продолжать писать `@include reset-list()`.
    @forward "reset" as reset-*;
    ```

=== "Sass"

    ```sass title="_reset.sass"

    // Пользователи модульной системы пишут `@include reset.list()`.
    @mixin list()
      ul
        margin: 0
        padding: 0
        list-style: none
    ```
    ```sass title="_reset.import.sass"

    // Пользователи устаревшего импорта могут продолжать писать `@include reset-list()`.
    @forward "reset" as reset-*
    ```

</div>

#### Настройка модулей через импорт {#configuring-modules-through-imports}

Вы можете [настраивать модули](../at-rules/use#configuration), которые загружаются через `@import`, определяя глобальные переменные перед `@import`, который впервые загружает этот модуль.

<div class="grid" markdown>

=== "SCSS"

    ```scss title="_library.scss"
    $color: blue !default;

    a {
      color: $color;
    }
    ```
    ```scss title="_library.import.scss"
    @forward 'library' as lib-*;
    ```
    ```scss title="style.sass"
    $lib-color: green;
    @import "library";
    ```

=== "Sass"

    ```sass title="_library.sass"
    $color: blue !default

    a
      color: $color
    ```
    ```sass title="_library.import.sass"
    @forward 'library' as lib-*
    ```
    ```sass title="style.sass"
    $lib-color: green
    @import "library"
    ```

```css title="CSS"
a {
  color: green;
}
```

</div>

!!! info "Информация"

    Модули загружаются только один раз, поэтому если вы измените конфигурацию после первого `@import` модуля (даже косвенного), изменение будет проигнорировано, если вы снова сделаете `@import` этого модуля.

### Загрузка модуля, содержащего импорты {#loading-a-module-that-contains-imports}

Когда вы используете `@use` (или `@forward`) для загрузки модуля, который использует `@import`, этот модуль будет содержать все публичные элементы, определённые загружаемой вами таблицей стилей, *и* всё, что эта таблица стилей транзитивно импортирует. Другими словами, всё, что импортируется, рассматривается так, как если бы оно было написано в одной большой таблице стилей.

Это позволяет легко начать использовать `@use` в таблице стилей ещё до того, как все библиотеки, от которых вы зависите, перейдут на новую модульную систему. Однако имейте в виду, что если они перейдут, их API вполне могут измениться!
