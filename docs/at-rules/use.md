---
title: "@use"
icon: lucide/at-sign
---

> Правило `@use` загружает [миксины](../at-rules/mixin), [функции](../at-rules/function) и [переменные](../variables) из других таблиц стилей Sass и объединяет CSS из нескольких таблиц стилей вместе. Таблицы стилей, загруженные с помощью `@use`, называются «модулями». Sass также предоставляет [встроенные модули](../modules), полные полезных функций.

Простейшее правило `@use` записывается как `@use "<url>"`, что загружает модуль по указанному URL. Любые стили, загруженные таким образом, будут включены в скомпилированный CSS ровно один раз, независимо от того, сколько раз эти стили загружаются.

!!! warning "Внимание"

    Правила `@use` таблицы стилей должны идти перед любыми другими правилами, кроме `@forward`, включая [правила стилей](../style-rules). Однако вы можете объявлять переменные перед правилами `@use` для использования при [настройке модулей](#configuration).

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
    @use 'foundation/code';
    @use 'foundation/lists';
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
    @use 'foundation/code'
    @use 'foundation/lists'
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

## Загрузка элементов модуля {#loading-members}

Вы можете получить доступ к переменным, функциям и миксинам из другого модуля, написав `<namespace>.<variable>`, `<namespace>.<function>()` или `@include <namespace>.<mixin>()`. По умолчанию пространством имён является просто последний компонент URL модуля.

Элементы (переменные, функции и миксины), загруженные с помощью `@use`, видны только в той таблице стилей, которая их загружает. Другим таблицам стилей нужно будет написать свои собственные правила `@use`, если они также хотят получить к ним доступ. Это помогает легко определить, откуда именно берется каждый элемент. Если вы хотите загрузить элементы из множества файлов одновременно, вы можете использовать [правило `@forward`](../at-rules/forward), чтобы перенаправить их все из одного общего файла.

!!! info "Примечание"

    Поскольку `@use` добавляет пространства имён к именам элементов, при написании таблицы стилей безопасно выбирать очень простые имена, такие как `$radius` или `$width`. Это отличается от старого [правила `@import`](../at-rules/import), которое побуждало пользователей писать длинные имена, такие как `$mat-corner-radius`, чтобы избежать конфликтов с другими библиотеками, и это помогает сохранять ваши таблицы стилей понятными и легко читаемыми!

<div class="grid" markdown>

=== "SCSS"

    ```scss title="src/_corners.scss"
    $radius: 3px;

    @mixin rounded {
      border-radius: $radius;
    }
    ```
    ```scss title="style.scss"
    @use "src/corners";

    .button {
      @include corners.rounded;
      padding: 5px + corners.$radius;
    }
    ```

=== "Sass"

    ```sass title="src/_corners.sass"
    $radius: 3px

    @mixin rounded
      border-radius: $radius
    ```
    ```sass title="style.sass"
    @use "src/corners"

    .button
      @include corners.rounded
      padding: 5px + corners.$radius
    ```

```css title="CSS"
.button {
  border-radius: 3px;
  padding: 8px;
}
```

</div>

### Выбор пространства имён {#choosing-a-namespace}

По умолчанию пространством имён модуля является просто последняя часть его URL-адреса без расширения файла. Однако иногда вы можете захотеть выбрать другое пространство имён — например, использовать более короткое имя для модуля, к которому часто обращаетесь, или загружать несколько модулей с одинаковым именем файла. Для этого пишите `@use "<url>" as <namespace>`.

<div class="grid" markdown>

=== "SCSS"

    ```scss title="src/_corners.scss"
    $radius: 3px;

    @mixin rounded {
      border-radius: $radius;
    }
    ```
    ```scss title="style.scss"
    @use "src/corners" as c;

    .button {
      @include c.rounded;
      padding: 5px + c.$radius;
    }
    ```

=== "Sass"

    ```sass title="src/_corners.sass"
    $radius: 3px

    @mixin rounded
      border-radius: $radius
    ```
    ```sass title="style.sass"
    @use "src/corners" as c

    .button
      @include c.rounded
      padding: 5px + c.$radius
    ```

```css title="CSS"
.button {
  border-radius: 3px;
  padding: 8px;
}
```

</div>

Вы даже можете загрузить модуль *без* пространства имён, написав `@use "<url>" as *`. Однако рекомендуется делать это только для таблиц стилей, написанных вами; в противном случае они могут ввести новые элементы, вызывающие конфликты имён!

<div class="grid" markdown>

=== "SCSS"

    ```scss title="src/_corners.scss"
    $radius: 3px;

    @mixin rounded {
      border-radius: $radius;
    }
    ```
    ```scss title="style.scss"
    @use "src/corners" as *;

    .button {
      @include rounded;
      padding: 5px + $radius;
    }
    ```

=== "Sass"

    ```sass title="src/_corners.sass"
    $radius: 3px

    @mixin rounded
      border-radius: $radius
    ```
    ```sass title="style.sass"
    @use "src/corners" as *

    .button
      @include rounded
      padding: 5px + $radius
    ```

```css title="CSS"
.button {
  border-radius: 3px;
  padding: 8px;
}
```

</div>

### Приватные элементы {#private-members}

Как автор таблицы стилей, вы можете не хотеть, чтобы все элементы, которые вы определяете, были доступны за пределами вашей таблицы стилей. Sass упрощает определение приватного элемента — достаточно начать его имя с символа `-` или `_`. Эти элементы будут работать точно так же, как обычные, внутри таблицы стилей, которая их определяет, но они не будут частью публичного API модуля. Это означает, что таблицы стилей, загружающие ваш модуль, не смогут их увидеть!

!!! tip "Совет"

    Если вы хотите сделать элемент приватным для всего *пакета*, а не только для одного модуля, просто не [перенаправляйте его модуль](../at-rules/forward) из каких-либо точек входа вашего пакета (таблиц стилей, которые вы говорите пользователям загружать для использования вашего пакета). Вы даже можете [скрыть этот элемент](../at-rules/forward#controlling-visibility), перенаправляя остальную часть его модуля!

<div class="grid" markdown>

=== "SCSS"

    ```scss title="src/_corners.scss"
    $-radius: 3px;

    @mixin rounded {
      border-radius: $-radius;
    }
    ```
    ```scss title="style.scss"
    @use "src/corners";

    .button {
      @include corners.rounded;

      // Это ошибка! $-radius не виден за пределами `_corners.scss`.
      padding: 5px + corners.$-radius;
    }
    ```

=== "Sass"

    ```sass title="src/_corners.sass"
    $-radius: 3px

    @mixin rounded
      border-radius: $-radius
    ```
    ```sass title="style.sass"
    @use "src/corners"

    .button
      @include corners.rounded

      // Это ошибка! $-radius не виден за пределами `_corners.scss`.
      padding: 5px + corners.$-radius
    ```

</div>

## Настройка {#configuration}

Таблица стилей может определять переменные с [флагом `!default`](../variables#default-values), чтобы сделать их настраиваемыми. Чтобы загрузить модуль с настройкой, пишите `@use <url> with (<variable>: <value>, <variable>: <value>)`. Настроенные значения перезапишут значения по умолчанию этих переменных.

<div class="grid" markdown>

=== "SCSS"

    ```scss title="_library.scss"
    $black: #000 !default;
    $border-radius: 0.25rem !default;
    $box-shadow: 0 0.5rem 1rem rgba($black, 0.15) !default;

    code {
      border-radius: $border-radius;
      box-shadow: $box-shadow;
    }
    ```
    ```scss title="style.scss"
    @use 'library' with (
      $black: #222,
      $border-radius: 0.1rem
    );
    ```

=== "Sass"

    ```sass title="_library.sass"
    $black: #000 !default
    $border-radius: 0.25rem !default
    $box-shadow: 0 0.5rem 1rem rgba($black, 0.15) !default

    code
      border-radius: $border-radius
      box-shadow: $box-shadow
    ```
    ```sass title="style.sass"
    @use 'library' with ($black: #222, $border-radius: 0.1rem)
    ```

```css title="CSS"
code {
  border-radius: 0.1rem;
  box-shadow: 0 0.5rem 1rem rgba(34, 34, 34, 0.15);
}
```

</div>

Модуль сохранит одну и ту же настройку (или отсутствие настройки) даже при многократной загрузке. Из-за этого `@use ... with` можно использовать только один раз для каждого модуля — при первой его загрузке. Это также позволяет использовать настройку для создания «тем», которые применяются на протяжении всей компиляции:

<div class="grid" markdown>

=== "SCSS"

    ```scss title="components/_button.scss"
    @use "../theme";

    button {
      color: theme.$text-color;
      background-color: theme.$background-color;
    }
    ```
    ```scss title="_theme.scss"
    $text-color: black !default;
    $background-color: white !default;
    ```
    ```scss title="dark.scss"
    @use "theme" with (
      $text-color: white,
      $background-color: black,
    );

    @use "components/button";
    // Здесь обычно импортируются дополнительные компоненты.
    ```

=== "Sass"

    ```sass title="components/_button.scss"
    @use "../theme"

    button
      color: theme.$text-color
      background-color: theme.$background-color
    ```
    ```sass title="_theme.scss"
    $text-color: black !default
    $background-color: white !default
    ```
    ```sass title="dark.scss"
    @use "theme" with (
      $text-color: white,
      $background-color: black,
    )

    @use "components/button"
    // Здесь обычно импортируются дополнительные компоненты.
    ```

```css title="CSS"
button {
  color: white;
  background-color: black;
}
```

</div>

!!! tip "Совет"

    Если вы хотите настроить модуль *и* дать ему пользовательское пространство имён, клауза `as` должна идти перед клаузой `with`, как в `@use ... as ... with ...`.

### С миксинами {#with-mixins}

Настройка модулей с помощью `@use ... with` может быть очень удобной, особенно при использовании библиотек, которые изначально писались для работы с [правилом `@import`](../at-rules/import). Но это не очень гибко, и для более сложных случаев мы это не рекомендуем. Если вы хотите настроить много переменных сразу, передать [карты](../values/maps) в качестве настройки или обновить настройку после загрузки модуля, рассмотрите написание миксина для установки ваших переменных и другого миксина для внедрения ваших стилей.

<div class="grid" markdown>

=== "SCSS"

    ```scss title="_library.scss"
    $-black: #000;
    $-border-radius: 0.25rem;
    $-box-shadow: null;

    /// Если пользователь настроил `$-box-shadow`, возвращает его настроенное значение.
    /// В противном случае возвращает значение, полученное из `$-black`.
    @function -box-shadow() {
      @return $-box-shadow or (0 0.5rem 1rem rgba($-black, 0.15));
    }

    @mixin configure($black: null, $border-radius: null, $box-shadow: null) {
      @if $black {
        $-black: $black !global;
      }
      @if $border-radius {
        $-border-radius: $border-radius !global;
      }
      @if $box-shadow {
        $-box-shadow: $box-shadow !global;
      }
    }

    @mixin styles {
      code {
        border-radius: $-border-radius;
        box-shadow: -box-shadow();
      }
    }
    ```
    ```scss title="style.scss"
    @use 'library';

    @include library.configure(
      $black: #222,
      $border-radius: 0.1rem
    );

    @include library.styles;
    ```

=== "Sass"

    ```sass title="_library.sass"
    $-black: #000
    $-border-radius: 0.25rem
    $-box-shadow: null

    /// Если пользователь настроил `$-box-shadow`, возвращает его настроенное значение.
    /// В противном случае возвращает значение, полученное из `$-black`.
    @function -box-shadow()
      @return $-box-shadow or (0 0.5rem 1rem rgba($-black, 0.15))

    @mixin configure($black: null, $border-radius: null, $box-shadow: null)
      @if $black
        $-black: $black !global
      @if $border-radius
        $-border-radius: $border-radius !global
      @if $box-shadow
        $-box-shadow: $box-shadow !global

    @mixin styles
      code
        border-radius: $-border-radius
        box-shadow: -box-shadow()
    ```
    ```sass title="style.sass"
    @use 'library'
    @include library.configure($black: #222, $border-radius: 0.1rem)
    @include library.styles
    ```

```css title="CSS"
code {
  border-radius: 0.1rem;
  box-shadow: 0 0.5rem 1rem rgba(34, 34, 34, 0.15);
}
```

</div>

### Переприсваивание переменных {#reassigning-variables}

После загрузки модуля вы можете переприсвоить его переменные.

<div class="grid" markdown>

=== "SCSS"

    ```scss title="_library.scss"
    $color: red;
    ```
    ```scss title="_override.scss"
    @use 'library';
    library.$color: blue;
    ```
    ```scss title="style.scss"
    @use 'library';
    @use 'override';
    @debug library.$color;  //=> blue
    ```

=== "Sass"

    ```sass title="_library.sass"
    $color: red
    ```
    ```sass title="_override.sass"
    @use 'library'
    library.$color: blue
    ```
    ```sass title="style.sass"
    @use 'library'
    @use 'override'
    @debug library.$color  //=> blue
    ```

</div>

Это работает даже если вы импортируете модуль без пространства имён с помощью `as *`. Присвоение переменной, определенной в этом модуле, перезапишет её значение в этом модуле.

!!! warning "Предупреждение"

    Переменные встроенных модулей (такие как [`math.$pi`](../modules/math#$pi)) нельзя переприсваивать.

## Поиск модуля {#finding-the-module}

Не было бы весело писать абсолютные URL для каждой загружаемой таблицы стилей, поэтому алгоритм Sass для поиска модуля немного упрощает задачу. Для начала вам не нужно явно указывать расширение файла, который хотите загрузить; `@use "variables"` автоматически загрузит `variables.scss`, `variables.sass` или `variables.css`.

!!! tip "Совет"

    Чтобы таблицы стилей работали на всех операционных системах, Sass загружает файлы по *URL*, а не по *путям к файлам*. Это означает, что нужно использовать прямые слеши, а не обратные, даже в Windows.

    Это также означает, что URL чувствительны к регистру, поэтому Sass будет считать `Styles.scss` и `styles.scss` разными модулями, даже если вы используете файловую систему, нечувствительную к регистру. Убедитесь, что ваши URL соответствуют реальному регистру файлов на диске, иначе таблицы стилей могут загрузиться дважды и точно не будут работать на других операционных системах.

### Пути загрузки {#load-paths}

Все реализации Sass позволяют пользователям указывать *пути загрузки*: пути в файловой системе, в которых Sass будет искать модули. Например, если вы передадите `node_modules/susy/sass` в качестве пути загрузки, вы сможете использовать `@use "susy"` для загрузки `node_modules/susy/sass/susy.scss` (хотя [URL-адреса `pkg:`](#pkg-ur-ls) — лучший способ для этого).

Однако модули всегда сначала загружаются относительно текущего файла. Пути загрузки используются только если не существует относительного файла, соответствующего URL модуля. Это гарантирует, что вы не сможете случайно нарушить свои относительные импорты при добавлении новой библиотеки.

!!! note "Примечание"

    В отличие от некоторых других языков, Sass не требует использовать `./` для относительных импортов. Относительные импорты всегда доступны.

### Фрагменты {#partials}

По соглашению, файлы Sass, которые предназначены только для загрузки как модули, а не для самостоятельной компиляции, начинаются с `_` (например, `_code.scss`). Такие файлы называются *фрагментами*, и они сообщают инструментам Sass не пытаться компилировать их самостоятельно. При импорте фрагмента можно опускать `_`.

### Индексные файлы {#index-files}

Если вы создадите файл `_index.scss` или `_index.sass` в папке, индексный файл будет загружен автоматически при загрузке URL самой папки.

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
    @use 'code';
    @use 'lists';
    ```
    ```scss title="style.scss"
    @use 'foundation';
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
    @use 'code'
    @use 'lists'
    ```
    ```sass title="style.sass"
    @use 'foundation'
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

## URL-адреса `pkg:` {#pkg-ur-ls}

Sass использует схему URL `pkg:` для загрузки таблиц стилей, распространяемых различными менеджерами пакетов. Поскольку Sass используется в контексте многих разных языков программирования с разными соглашениями управления пакетами, URL-адреса `pkg:` почти не имеют фиксированного значения. Вместо этого пользователям рекомендуется реализовывать собственные импортеры (с помощью [JS API](https://sass-lang.com/documentation/js-api/interfaces/Options/#importers) или [протокола Embedded Sass](https://github.com/sass/sass/blob/main/spec/embedded-protocol.md)), которые разрешают эти URL с использованием логики родного менеджера пакетов.

Это позволяет URL-адресам `pkg:` и таблицам стилей, использующим их, быть переносимыми между разными экосистемами языков. Независимо от того, устанавливаете ли вы библиотеку Sass через npm (для которого Sass предоставляет [встроенный импортер `pkg:`](#node-js-package-importer)) или самый экзотический менеджер пакетов, который можно найти, если написать `@use 'pkg:library'`, это сработает правильно.

!!! note "Примечание"

    `pkg:` URL используются не только для `@use`. Их можно использовать везде, где можно загрузить файл Sass, включая [`@forward`](../at-rules/forward), [`meta.load-css()`](../modules/meta/#load-css) и даже старое [правило `@import`](../at-rules/import).

### Правила для импортера `pkg:` {#rules-for-a-pkg-importer}

Существует несколько общих правил, которых Sass ожидает от всех импортеров `pkg:`. Эти правила помогают обеспечить единообразную обработку `pkg:` всеми менеджерами пакетов, чтобы таблицы стилей были максимально переносимыми.

Помимо стандартных правил для пользовательских импортеров, импортер `pkg:` должен обрабатывать только неканонические URL, которые:

* имеют схему `pkg`, и
* чей путь начинается с имени пакета, и
* опционально за которыми следует путь с сегментами, разделёнными прямым слешем.

Имя пакета может содержать прямые слеши в зависимости от того, поддерживает ли это конкретный менеджер пакетов. Например, npm позволяет имена пакетов вроде `@namespace/name`. Обратите внимание, что имена пакетов, содержащие неалфавитно-цифровые символы, могут быть менее переносимыми между разными менеджерами пакетов.

Импортеры `pkg:` должны отклонять следующие шаблоны:

* URL, чей путь начинается с `/`.
* URL с непустым/ненулевым именем пользователя, паролем, хостом, портом, запросом или фрагментом.

Если импортер `pkg:` встречает URL, нарушающий соглашения собственного менеджера пакетов, но *не* указанные выше правила, он должен просто отказаться загружать этот URL, а не выбрасывать ошибку. Это позволяет пользователям использовать несколько импортеров `pkg:` одновременно при необходимости.

### Импортер пакетов Node.js {#node-js-package-importer}

Поскольку Sass чаще всего используется в экосистеме Node.js, он поставляется с импортером `pkg:`, который использует тот же алгоритм, что и Node.js, для загрузки таблиц стилей Sass. Этот импортер не включен по умолчанию, но его легко активировать:

* Если вы используете JavaScript API, просто добавьте [`new NodePackageImporter()`](https://sass-lang.com/documentation/js-api/classes/NodePackageImporter/) в опцию `importers`.

* Если вы используете Dart API, добавьте [`NodePackageImporter()`](https://pub.dev../sass/latest/sass/NodePackageImporter-class.html) в опцию `importers`.

* Если вы используете командную строку, передайте [`--pkg-importer=node`](../cli/dart-sass/#pkg-importer-node).

Если вы загружаете URL-адрес `pkg:`, импортер `pkg:` для Node.js посмотрит в файл `package.json`, чтобы определить, какой файл Sass загружать. Он проверит по порядку:

* Поле [`"exports"`](https://nodejs.org/api/packages.html#conditional-exports) с условиями `"sass"`, `"style"` и `"default"`. Это рекомендуемый способ для пакетов предоставлять точки входа Sass в будущем.

* Поле `"sass"` или `"style"`, которое должно содержать путь к файлу Sass. Это работает только если URL `pkg:` не имеет подпутей — `pkg:library` загрузит файл, указанный в поле `"sass"`, но `pkg:library/button` загрузит `button.scss` из корня пакета.

* [Индексный файл](../at-rules/use/#index-files) в корне пакета. Это также работает только если URL `pkg:` не имеет подпутей.

Импортер `pkg:` для Node.js поддерживает полный спектр возможностей `"exports"`, поэтому вы также можете указать разные расположения для разных подпутей (обратите внимание, что ключ должен включать расширение файла):

```json
{
  "exports": {
    ".": {
      "sass": "styles/index.scss",
    },
    "./button.scss": {
      "sass": "styles/button.scss",
    },
    "./accordion.scss": {
      "sass": "styles/accordion.scss",
    }
  }
}
```

...или даже шаблоны:

```json
{
  "exports": {
    ".": {
      "sass": "styles/index.scss",
    },
    "./*.scss": {
      "sass": "styles/*.scss",
    },
  }
}
```

## Загрузка CSS {#loading-css}

Помимо загрузки файлов `.sass` и `.scss`, Sass может загружать обычные файлы `.css`.

<div class="grid" markdown>

=== "SCSS"

    ```scss title="code.css"
    code {
      padding: .25em;
      line-height: 0;
    }
    ```
    ```scss title="style.scss"
    @use 'code';
    ```

=== "Sass"

    ```sass title="code.css"
    code {
      padding: .25em;
      line-height: 0;
    }
    ```
    ```sass title="style.sass"
    @use 'code'
    ```

```css title="CSS"
code {
  padding: .25em;
  line-height: 0;
}
```

</div>

Файлы CSS, загруженные как модули, не поддерживают специальные возможности Sass и поэтому не могут предоставлять переменные, функции или миксины Sass. Чтобы авторы не писали Sass в своих CSS случайно, все возможности Sass, которые не являются валидным CSS, вызовут ошибки. В остальном CSS будет выведен как есть. Его даже можно [расширять](../at-rules/extend)!

## Отличия от `@import` {#differences-from-import}

Правило `@use` предназначено для замены старого [правила `@import`](../at-rules/import), но специально спроектировано для работы по-другому. Вот основные отличия между ними:

* `@use` делает переменные, функции и миксины доступными только в области видимости текущего файла. Оно никогда не добавляет их в глобальную область видимости. Это упрощает определение источника каждого имени, на которое ссылается ваш файл Sass, и позволяет использовать короткие имена без риска коллизий.

* `@use` загружает каждый файл только один раз. Это гарантирует, что вы не дублируете CSS зависимостей случайно множество раз.

* `@use` должно находиться в начале файла и не может быть вложено в правила стилей. Вложенные импорты можно мигрировать в [вызовы миксинов или `meta.load-css()`](../breaking-changes/import/#nested-imports).

* Каждое правило `@use` может содержать только один URL.

* `@use` требует кавычек вокруг URL, даже при использовании [синтаксиса с отступами](../syntax#the-indented-syntax).
