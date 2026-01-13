---
title: "sass:meta"
icon: lucide/file-code
---

## Миксины {#mixins}

### apply

```scss
meta.apply($mixin, $args...)
```

Включает `$mixin` с аргументами `$args`. Если передан блок [`@content`](../at-rules/mixin#content-blocks), он передаётся в `$mixin`.

`$mixin` должен быть значением миксина, таким как возвращаемое функцией [`meta.get-mixin()`](#get-mixin).

<div class="grid" markdown>

=== "SCSS"

    ```scss
    @use "sass:meta";
    @use "sass:string";

    /// Передаёт каждый элемент `$list` в отдельный вызов `$mixin`.
    @mixin apply-to-all($mixin, $list) {
      @each $element in $list {
        @include meta.apply($mixin, $element);
      }
    }

    @mixin font-class($size) {
      .font-#{$size} {
        font-size: $size;
      }
    }

    $sizes: [8px, 12px, 2rem];

    @include apply-to-all(meta.get-mixin("font-class"), $sizes);
    ```

=== "SASS"

    ```sass
    @use "sass:meta"
    @use "sass:string"

    /// Передаёт каждый элемент `$list` в отдельный вызов `$mixin`.
    @mixin apply-to-all($mixin, $list)
      @each $element in $list
        @include meta.apply($mixin, $element)

    @mixin font-class($size)
      .font-#{$size}
        font-size: $size

    $sizes: 8px, 12px 2rem

    @include apply-to-all(meta.get-mixin("font-class"), $sizes)
    ```

```css title="CSS"
.font-8px {
  font-size: 8px;
}

.font-12px {
  font-size: 12px;
}

.font-2rem {
  font-size: 2rem;
}
```

</div>

### load-css

```scss
meta.load-css($url, $with: null)
```

Загружает [модуль](../at-rules/use) по адресу `$url` и включает его CSS так, как если бы он был написан в содержимом этого миксина. Параметр `$with` предоставляет [конфигурацию](../at-rules/use#configuration) для модулей; если он передан, это должна быть карта из имён переменных (без `$`) в значения этих переменных для использования в загружаемом модуле.

Если `$url` относительный, он интерпретируется относительно файла, в котором вызывается `meta.load-css()`.

**Как и правило [`@use`](../at-rules/use)**:

* Загружает модуль только один раз, даже если он загружается несколько раз разными способами.

* Не может предоставить конфигурацию модулю, который уже загружен, независимо от того, был ли он загружен с конфигурацией.

**В отличие от правила [`@use`](../at-rules/use)**:

* Не делает доступными в текущем модуле никакие элементы из загруженного модуля.

* Может использоваться где угодно в таблице стилей. Можно даже вкладывать в правила стилей для создания вложенных стилей!

* URL загружаемого модуля может быть переменной и включать [интерполяцию](../interpolation).

{% headsUp %}
Параметр `$url` должен быть строкой с URL, как вы передали бы в правило `@use`. Это не должен быть CSS `url()`!
{% endheadsUp %}

<div class="grid" markdown>

=== "SCSS"

    ```scss title="dark-theme/_code.scss"
    $border-contrast: false !default;

    code {
      background-color: #6b717f;
      color: #d2e1dd;
      @if $border-contrast {
        border-color: #dadbdf;
      }
    }
    ```
    ```scss title="style.scss"
    @use "sass:meta";

    body.dark {
      @include meta.load-css("dark-theme/code",
          $with: ("border-contrast": true));
    }
    ```

=== "SASS"

    ```sass title="dark-theme/_code.sass"
    $border-contrast: false !default

    code
      background-color: #6b717f
      color: #d2e1dd
      @if $border-contrast
        border-color: #dadbdf
    ```
    ```sass title="style.sass"
    @use "sass:meta"

    body.dark
      $configuration: ("border-contrast": true)
      @include meta.load-css("dark-theme/code", $with: $configuration)
    ```

```css title="CSS"
body.dark code {
  background-color: #6b717f;
  color: #d2e1dd;
  border-color: #dadbdf;
}
```

</div>

## Функции {#functions}

### accepts-content

```scss
meta.accepts-content($mixin) //=> булево значение
```

Возвращает, может ли данное значение миксина принять блок [`@content`](../at-rules/mixin#content-blocks).

Возвращает `true`, если для миксина возможно принять блок `@content`, даже если он не всегда это делает.

### calc-args

```scss
meta.calc-args($calc) //=> список
```

Возвращает аргументы для данного [вычисления](../values/calculations).

Если аргумент является числом или вложенным вычислением, он возвращается как этот тип. В противном случае возвращается как строка без кавычек.

=== "SCSS"

    ```scss
    @use 'sass:meta';

    @debug meta.calc-args(calc(100px + 10%)); // unquote("100px + 10%")
    @debug meta.calc-args(clamp(50px, var(--width), 1000px)); // 50px, unquote("var(--width)"), 1000px
    ```

=== "SASS"

    ```sass
    @use 'sass:meta'

    @debug meta.calc-args(calc(100px + 10%)) // unquote("100px + 10%")
    @debug meta.calc-args(clamp(50px, var(--width), 1000px)) // 50px, unquote("var(--width)"), 1000px
    ```

### calc-name

```scss
meta.calc-name($calc) //=> строка в кавычках
```

Возвращает имя данного [вычисления](../values/calculations).

=== "SCSS"

    ```scss
    @use 'sass:meta';

    @debug meta.calc-name(calc(100px + 10%)); // "calc"
    @debug meta.calc-name(clamp(50px, var(--width), 1000px)); // "clamp"
    ```

=== "SASS"

    ```sass
    @use 'sass:meta'

    @debug meta.calc-name(calc(100px + 10%)) // "calc"
    @debug meta.calc-name(clamp(50px, var(--width), 1000px)) // "clamp"
    ```

### call

```scss
meta.call($function, $args...)
call($function, $args...)
```

Вызывает `$function` с аргументами `$args` и возвращает результат.

`$function` должна быть значением функции, таким как возвращаемое функцией [`meta.get-function()`](#get-function).

<div class="grid" markdown>

=== "SCSS"

    ```scss
    @use "sass:list";
    @use "sass:meta";
    @use "sass:string";

    /// Возвращает копию $list, из которой удалены все элементы,
    /// для которых $condition возвращает `true`
    @function remove-where($list, $condition) {
      $new-list: ();
      $separator: list.separator($list);
      @each $element in $list {
        @if not meta.call($condition, $element) {
          $new-list: list.append($new-list, $element, $separator: $separator);
        }
      }
      @return $new-list;
    }

    $fonts: Tahoma, Geneva, "Helvetica Neue", Helvetica, Arial, sans-serif;

    .content {
      @function contains-helvetica($string) {
        @return string.index($string, "Helvetica");
      }
      font-family: remove-where($fonts, meta.get-function("contains-helvetica"));
    }
    ```

=== "SASS"

    ```sass
    @use "sass:list"
    @use "sass:meta"
    @use "sass:string"

    /// Возвращает копию $list, из которой удалены все элементы,
    /// для которых $condition возвращает `true`
    @function remove-where($list, $condition)
      $new-list: ()
      $separator: list.separator($list)
      @each $element in $list
        @if not meta.call($condition, $element)
          $new-list: list.append($new-list, $element, $separator: $separator)

      @return $new-list

    $fonts: Tahoma, Geneva, "Helvetica Neue", Helvetica, Arial, sans-serif

    .content
      @function contains-helvetica($string)
        @return string.index($string, "Helvetica")

      font-family: remove-where($fonts, meta.get-function("contains-helvetica"))
    ```

```css title="CSS"
.content {
  font-family: Tahoma, Geneva, Arial, sans-serif;
}
```

</div>

### content-exists

```scss
meta.content-exists()
content-exists() //=> булево значение
```

Возвращает, был ли передан блок [`@content`](../at-rules/mixin#content-blocks) в текущий миксин.

Выдаёт ошибку при вызове вне миксина.

=== "SCSS"

    ```scss
    @use 'sass:meta';

    @mixin debug-content-exists {
      @debug meta.content-exists();
      @content;
    }

    @include debug-content-exists; // false
    @include debug-content-exists { // true
      // Content!
    }
    ```

=== "SASS"

    ```sass
    @use 'sass:meta'

    @mixin debug-content-exists
      @debug meta.content-exists()
      @content


    @include debug-content-exists // false
    @include debug-content-exists // true
      // Content!
    ```

### feature-exists

```scss
meta.feature-exists($feature)
feature-exists($feature) //=> булево значение
```

Возвращает, поддерживает ли текущая реализация Sass функцию `$feature`.

`$feature` должна быть строкой. В настоящее время распознаются следующие функции:

* `global-variable-shadowing` — означает, что локальная переменная будет [затенять](../variables#shadowing) глобальную переменную, если у неё нет флага `!global`.
* `extend-selector-pseudoclass` — означает, что правило [`@extend`](../at-rules/extend) будет влиять на селекторы, вложенные в псевдоклассы вроде `:not()`.
* `units-level3` — означает, что [арифметика единиц](../values/numbers#units) поддерживает единицы, определённые в [CSS Values and Units Level 3](http://www.w3.org/TR/css3-values).
* `at-error` — означает, что правило [`@error`](../at-rules/error) поддерживается.
* `custom-property` — означает, что значения [объявлений пользовательских свойств](../style-rules/declarations#custom-properties) не поддерживают никакие [выражения](../syntax/structure#expressions), кроме [интерполяции](../interpolation).

Возвращает `false` для любой нераспознанной `$feature`.

!!! tip "Совет"

    Эта функция устарела и её следует избегать. Подробности см. на [странице критических изменений](../breaking-changes/feature-exists).

=== "SCSS"

    ```scss
    @use "sass:meta";

    @debug meta.feature-exists("at-error"); // true
    @debug meta.feature-exists("unrecognized"); // false
    ```

=== "SASS"

    ```sass
    @use "sass:meta"

    @debug meta.feature-exists("at-error") // true
    @debug meta.feature-exists("unrecognized") // false
    ```

### function-exists

```scss
meta.function-exists($name, $module: null)
function-exists($name) //=> булево значение
```

Возвращает, определена ли функция с именем `$name`, либо как встроенная функция, либо как пользовательская.

Если передан `$module`, также проверяет модуль с именем `$module` на наличие определения функции. `$module` должен быть строкой, соответствующей пространству имён правила [`@use`](../at-rules/use) в текущем файле.

=== "SCSS"

    ```scss
    @use "sass:meta";
    @use "sass:math";

    @debug meta.function-exists("div", "math"); // true
    @debug meta.function-exists("scale-color"); // true
    @debug meta.function-exists("add"); // false

    @function add($num1, $num2) {
      @return $num1 + $num2;
    }
    @debug meta.function-exists("add"); // true
    ```

=== "SASS"

    ```sass
    @use "sass:meta"
    @use "sass:math"

    @debug meta.function-exists("div", "math") // true
    @debug meta.function-exists("scale-color") // true
    @debug meta.function-exists("add") // false

    @function add($num1, $num2)
      @return $num1 + $num2

    @debug meta.function-exists("add") // true
    ```

### get-function

```scss
meta.get-function($name, $css: false, $module: null)
get-function($name, $css: false, $module: null) //=> функция
```

Возвращает [значение функции](../values/functions) с именем `$name`.

Если `$module` равен `null`, возвращает функцию с именем `$name` без пространства имён (включая [глобальные встроенные функции](../modules#global-functions)). В противном случае `$module` должен быть строкой, соответствующей пространству имён правила [`@use`](../at-rules/use) в текущем файле, и в этом случае возвращается функция из этого модуля с именем `$name`.

По умолчанию выдаёт ошибку, если `$name` не относится к функции Sass. Однако если `$css` равен `true`, вместо этого возвращает [обычную CSS-функцию](../at-rules/function/#plain-css-functions).

Возвращённую функцию можно вызвать с помощью [`meta.call()`](#call).

<div class="grid" markdown>

=== "SCSS"

    ```scss
    @use "sass:list";
    @use "sass:meta";
    @use "sass:string";

    /// Возвращает копию $list, из которой удалены все элементы,
    /// для которых $condition возвращает `true`
    @function remove-where($list, $condition) {
      $new-list: ();
      $separator: list.separator($list);
      @each $element in $list {
        @if not meta.call($condition, $element) {
          $new-list: list.append($new-list, $element, $separator: $separator);
        }
      }
      @return $new-list;
    }

    $fonts: Tahoma, Geneva, "Helvetica Neue", Helvetica, Arial, sans-serif;

    .content {
      @function contains-helvetica($string) {
        @return string.index($string, "Helvetica");
      }
      font-family: remove-where($fonts, meta.get-function("contains-helvetica"));
    }
    ```

=== "SASS"

    ```sass
    @use "sass:list"
    @use "sass:meta"
    @use "sass:string"

    /// Возвращает копию $list, из которой удалены все элементы,
    /// для которых $condition возвращает `true`
    @function remove-where($list, $condition)
      $new-list: ()
      $separator: list.separator($list)
      @each $element in $list
        @if not meta.call($condition, $element)
          $new-list: list.append($new-list, $element, $separator: $separator)

      @return $new-list

    $fonts: Tahoma, Geneva, "Helvetica Neue", Helvetica, Arial, sans-serif

    .content
      @function contains-helvetica($string)
        @return string.index($string, "Helvetica")

      font-family: remove-where($fonts, meta.get-function("contains-helvetica"))
    ```

```css title="CSS"
.content {
  font-family: Tahoma, Geneva, Arial, sans-serif;
}
```

</div>

### get-mixin

```scss
meta.get-mixin($name, $module: null) //=> функция
```

Возвращает [значение миксина](../values/mixins) с именем `$name`.

Если `$module` равен `null`, возвращает миксин с именем `$name`, определённый в текущем модуле. В противном случае `$module` должен быть строкой, соответствующей пространству имён правила [`@use`](../at-rules/use) в текущем файле, и в этом случае возвращается миксин из этого модуля с именем `$name`.

По умолчанию выдаёт ошибку, если `$name` не относится к миксину.

Возвращённый миксин можно включить с помощью [`meta.apply()`](#apply).

<div class="grid" markdown>

=== "SCSS"

    ```scss
    @use "sass:meta";
    @use "sass:string";

    /// Передаёт каждый элемент $list в отдельный вызов $mixin.
    @mixin apply-to-all($mixin, $list) {
      @each $element in $list {
        @include meta.apply($mixin, $element);
      }
    }

    @mixin font-class($size) {
      .font-#{$size} {
        font-size: $size;
      }
    }

    $sizes: [8px, 12px, 2rem];

    @include apply-to-all(meta.get-mixin("font-class"), $sizes);
    ```

=== "SASS"

    ```sass
    @use "sass:meta"
    @use "sass:string"

    /// Передаёт каждый элемент $list в отдельный вызов $mixin.
    @mixin apply-to-all($mixin, $list)
      @each $element in $list
        @include meta.apply($mixin, $element)

    @mixin font-class($size)
      .font-#{$size}
        font-size: $size

    $sizes: 8px, 12px 2rem

    @include apply-to-all(meta.get-mixin("font-class"), $sizes)
    ```

```css title="CSS"
.font-8px {
  font-size: 8px;
}

.font-12px {
  font-size: 12px;
}

.font-2rem {
  font-size: 2rem;
}
```

</div>

### global-variable-exists

```scss
meta.global-variable-exists($name, $module: null)
global-variable-exists($name, $module: null) //=> булево значение
```

Возвращает, существует ли [глобальная переменная](../variables#scope) с именем `$name` (без знака `$`).

Если `$module` равен `null`, возвращает, существует ли переменная с именем `$name` без пространства имён. В противном случае `$module` должен быть строкой, соответствующей пространству имён правила [`@use`](../at-rules/use) в текущем файле, и в этом случае возвращает, есть ли в этом модуле переменная с именем `$name`.

См. также [`meta.variable-exists()`](#variable-exists).

=== "SCSS"

    ```scss
    @use "sass:meta";

    @debug meta.global-variable-exists("var1"); // false

    $var1: value;
    @debug meta.global-variable-exists("var1"); // true

    h1 {
      // $var2 - локальная переменная.
      $var2: value;
      @debug meta.global-variable-exists("var2"); // false
    }
    ```

=== "SASS"

    ```sass
    @use "sass:meta"

    @debug meta.global-variable-exists("var1") // false

    $var1: value
    @debug meta.global-variable-exists("var1") // true

    h1
      // $var2 - локальная переменная.
      $var2: value
      @debug meta.global-variable-exists("var2") // false
    ```

### inspect

```scss
meta.inspect($value)
inspect($value) //=> строка без кавычек
```

Возвращает строковое представление `$value`.

Возвращает представление *любого* значения Sass, а не только тех, которые могут быть представлены в CSS. Таким образом, её возвращаемое значение не гарантируется валидным CSS.

!!! note "Примечание"

    Эта функция предназначена для отладки; формат её вывода не гарантируется согласованным между версиями или реализациями Sass.

=== "SCSS"

    ```scss
    @use "sass:meta";

    @debug meta.inspect(10px 20px 30px); // unquote("10px 20px 30px")
    @debug meta.inspect(("width": 200px)); // unquote('("width": 200px)')
    @debug meta.inspect(null); // unquote("null")
    @debug meta.inspect("Helvetica"); // unquote('"Helvetica"')
    ```

=== "SASS"

    ```sass
    @use "sass:meta"

    @debug meta.inspect(10px 20px 30px) // unquote("10px 20px 30px")
    @debug meta.inspect(("width": 200px)) // unquote('("width": 200px)')
    @debug meta.inspect(null) // unquote("null")
    @debug meta.inspect("Helvetica") // unquote('"Helvetica"')
    ```

### keywords

```scss
meta.keywords($args)
keywords($args) //=> карта
```

Возвращает ключевые слова, переданные миксину или функции, которая принимает [произвольные аргументы](../at-rules/mixin#taking-arbitrary-arguments). Аргумент `$args` должен быть [списком аргументов](../values/lists#argument-lists).

Ключевые слова возвращаются в виде карты от имён аргументов как строк без кавычек (без знака `$`) к значениям этих аргументов.

<div class="grid" markdown>

=== "SCSS"

    ```scss
    @use "sass:meta";

    @mixin syntax-colors($args...) {
      @debug meta.keywords($args);
      // (string: #080, comment: #800, variable: #60b)

      @each $name, $color in meta.keywords($args) {
        pre span.stx-#{$name} {
          color: $color;
        }
      }
    }

    @include syntax-colors(
      $string: #080,
      $comment: #800,
      $variable: #60b,
    )
    ```

=== "SASS"

    ```sass
    @use "sass:meta"

    @mixin syntax-colors($args...)
      @debug meta.keywords($args)
      // (string: #080, comment: #800, variable: #60b)

      @each $name, $color in meta.keywords($args)
        pre span.stx-#{$name}
          color: $color

    @include syntax-colors($string: #080, $comment: #800, $variable: #60b)
    ```

```css title="CSS"
pre span.stx-string {
  color: #080;
}

pre span.stx-comment {
  color: #800;
}

pre span.stx-variable {
  color: #60b;
}
```

</div>

### mixin-exists

```scss
meta.mixin-exists($name, $module: null)
mixin-exists($name, $module: null) //=> булево значение
```

Возвращает, существует ли [миксин](../at-rules/mixin) с именем `$name`.

Если `$module` равен `null`, возвращает, существует ли миксин с именем `$name` без пространства имён. В противном случае `$module` должен быть строкой, соответствующей пространству имён правила [`@use`](../at-rules/use) в текущем файле, и в этом случае возвращает, есть ли в этом модуле миксин с именем `$name`.

=== "SCSS"

    ```scss
    @use "sass:meta";

    @debug meta.mixin-exists("shadow-none"); // false

    @mixin shadow-none {
      box-shadow: none;
    }

    @debug meta.mixin-exists("shadow-none"); // true
    ```

=== "SASS"

    ```sass
    @use "sass:meta"

    @debug meta.mixin-exists("shadow-none") // false

    @mixin shadow-none
      box-shadow: none

    @debug meta.mixin-exists("shadow-none") // true
    ```

### module-functions

```scss
meta.module-functions($module) //=> карта
```

Возвращает все функции, определённые в модуле, в виде карты от имён функций к [значениям функций](../values/functions).

Параметр `$module` должен быть строкой, соответствующей пространству имён правила [`@use`](../at-rules/use) в текущем файле.

=== "SCSS"

    ```scss title="_functions.scss"
    @function pow($base, $exponent) {
      $result: 1;
      @for $_ from 1 through $exponent {
        $result: $result * $base;
      }
      @return $result;
    }
    ```
    ```scss
    @use "sass:map";
    @use "sass:meta";
    @use "functions";

    @debug meta.module-functions("functions"); // ("pow": get-function("pow"))
    @debug meta.call(map.get(meta.module-functions("functions"), "pow"), 3, 4); // 81
    ```

=== "SASS"

    ```sass title="_functions.sass"
    @function pow($base, $exponent)
      $result: 1
      @for $_ from 1 through $exponent
        $result: $result * $base

      @return $result
    ```
    ```sass
    @use "sass:map"
    @use "sass:meta"
    @use "functions"

    @debug meta.module-functions("functions") // ("pow": get-function("pow"))
    @debug meta.call(map.get(meta.module-functions("functions"), "pow"), 3, 4) // 81
    ```

### module-mixins

```scss
meta.module-mixins($module) //=> карта
```

Возвращает все миксины, определённые в модуле, в виде карты от имён миксинов к [значениям миксинов](../values/mixins).

Параметр `$module` должен быть строкой, соответствующей пространству имён правила [`@use`](../at-rules/use) в текущем файле.

<div class="grid" markdown>

=== "SCSS"

    ```scss title="_mixins.scss"
    @mixin stretch() {
      align-items: stretch;
      display: flex;
      flex-direction: row;
    }
    ```
    ```scss
    @use "sass:map";
    @use "sass:meta";
    @use "mixins";

    @debug meta.module-mixins("mixins"); // => ("stretch": get-mixin("stretch"))

    .header {
      @include meta.apply(map.get(meta.module-mixins("mixins"), "stretch"));
    }
    ```

=== "SASS"

    ```sass title="_mixins.scss"
    @mixin stretch()
      align-items: stretch
      display: flex
      flex-direction: row
    ```
    ```sass
    @use "sass:map"
    @use "sass:meta"
    @use "mixins"

    @debug meta.module-mixins("mixins") // => ("stretch": get-mixin("stretch"))

    .header
      @include meta.apply(map.get(meta.module-mixins("mixins"), "stretch"))
    ```

```css title="CSS"
.header {
  align-items: stretch;
  display: flex;
  flex-direction: row;
}
```

</div>

### module-variables

```scss
meta.module-variables($module) //=> карта
```

Возвращает все переменные, определённые в модуле, в виде карты от имён переменных (без `$`) к значениям этих переменных.

Параметр `$module` должен быть строкой, соответствующей пространству имён правила [`@use`](../at-rules/use) в текущем файле.

=== "SCSS"

    ```scss title="_variables.scss"
    $hopbush: #c69;
    $midnight-blue: #036;
    $wafer: #e1d7d2;
    ```
    ```scss
    @use "sass:meta";
    @use "variables";

    @debug meta.module-variables("variables");
    // (
    //   "hopbush": #c69,
    //   "midnight-blue": #036,
    //   "wafer": #e1d7d2
    // )
    ```

=== "SASS"

    ```sass title="_variables.sass"
    $hopbush: #c69
    $midnight-blue: #036
    $wafer: #e1d7d2
    ```
    ```sass
    @use "sass:meta"
    @use "variables"

    @debug meta.module-variables("variables")
    // (
    //   "hopbush": #c69,
    //   "midnight-blue": #036,
    //   "wafer": #e1d7d2
    // )
    ```

### type-of

```scss
meta.type-of($value)
type-of($value) //=> строка без кавычек
```

Возвращает тип `$value`.

Это может вернуть следующие значения:

* [`number`](../values/numbers)
* [`string`](../values/strings)
* [`color`](../values/colors)
* [`list`](../values/lists)
* [`map`](../values/maps)
* [`calculation`](../values/calculations)
* [`bool`](../values/booleans)
* [`null`](../values/null)
* [`function`](../values/functions)
* [`mixin`](../values/mixins)
* [`arglist`](../values/lists#argument-lists)

Новые возможные значения могут быть добавлены в будущем. Может вернуть либо `list`, либо `map` для `()`, в зависимости от того, была ли она возвращена [функцией карты](../modules/map).

=== "SCSS"

    ```scss
    @use 'sass:meta';

    @debug meta.type-of(10px); // number
    @debug meta.type-of(10px 20px 30px); // list
    @debug meta.type-of(()); // list
    ```

=== "SASS"

    ```sass
    @use 'sass:meta'

    @debug meta.type-of(10px) // number
    @debug meta.type-of(10px 20px 30px) // list
    @debug meta.type-of(()) // list
    ```

### variable-exists

```scss
meta.variable-exists($name)
variable-exists($name) //=> булево значение
```

Возвращает, существует ли переменная с именем `$name` (без знака `$`) в текущей [области видимости](../variables#scope).

См. также [`meta.global-variable-exists()`](#global-variable-exists).

=== "SCSS"

    ```scss
    @use "sass:meta";

    @debug meta.variable-exists("var1"); // false

    $var1: value;
    @debug meta.variable-exists("var1"); // true

    h1 {
      // $var2 - локальная переменная.
      $var2: value;
      @debug meta.variable-exists("var2"); // true
    }
    ```

=== "SASS"

    ```sass
    @use "sass:meta"

    @debug meta.variable-exists("var1") // false

    $var1: value
    @debug meta.variable-exists("var1") // true

    h1
      // $var2 - локальная переменная.
      $var2: value
      @debug meta.variable-exists("var2") // true
    ```
