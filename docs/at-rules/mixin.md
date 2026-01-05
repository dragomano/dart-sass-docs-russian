---
title: "@mixin и @include"
icon: lucide/at-sign
---

> Миксины позволяют вам определять стили, которые можно переиспользовать во всей таблице стилей. Они позволяют легко избежать использования несемантических классов, таких как `.float-left`, и распространять наборы стилей в библиотеках.

Миксины определяются с помощью at-правила `@mixin`, которое записывается как `@mixin <name> { ... }` или `@mixin name(<arguments...>) { ... }`. Имя миксина может быть любым идентификатором Sass, который не начинается с `--`, и он может содержать любые [инструкции](../syntax/structure#statements), кроме [инструкций верхнего уровня](../syntax/structure#top-level-statements). Их можно использовать для инкапсуляции стилей, которые можно вставить в одно [правило стиля](../style-rules); они могут содержать собственные правила стилей, которые могут быть вложены в другие правила или включены на верхнем уровне таблицы стилей; или они могут просто служить для изменения переменных.

Миксины включаются в текущий контекст с помощью at-правила `@include`, которое записывается как `@include <name>` или `@include <name>(<arguments...>)`, где указывается имя включаемого миксина.

<div class="grid" markdown>

=== "SCSS"

    ```scss
    @mixin reset-list {
      margin: 0;
      padding: 0;
      list-style: none;
    }

    @mixin horizontal-list {
      @include reset-list;

      li {
        display: inline-block;
        margin: {
          left: -2px;
          right: 2em;
        }
      }
    }

    nav ul {
      @include horizontal-list;
    }
    ```

=== "Sass"

    ```sass
    @mixin reset-list
      margin: 0
      padding: 0
      list-style: none

    @mixin horizontal-list
      @include reset-list

      li
        display: inline-block
        margin:
          left: -2px
          right: 2em

    nav ul
      @include horizontal-list
    ```

```css title="CSS"
nav ul {
  margin: 0;
  padding: 0;
  list-style: none;
}
nav ul li {
  display: inline-block;
  margin-left: -2px;
  margin-right: 2em;
}
```

</div>

!!! note "Примечание"

    Имена миксинов, как и все идентификаторы Sass, считают дефисы и подчеркивания идентичными. Это означает, что `reset-list` и `reset_list` ссылаются на один и тот же миксин. Это историческое наследие с самых ранних дней Sass, когда он *разрешал только* подчеркивания в именах идентификаторов. Когда Sass добавил поддержку дефисов для соответствия синтаксису CSS, они были сделаны эквивалентными, чтобы облегчить миграцию.

## Аргументы {#arguments}

Миксины также могут принимать аргументы, что позволяет настраивать их поведение при каждом вызове. Аргументы указываются в правиле `@mixin` после имени миксина в виде списка имен переменных в круглых скобках. Затем миксин должен быть включен с тем же количеством аргументов в форме [выражений SassScript](../syntax/structure#expressions). Значения этих выражений доступны внутри тела миксина в виде соответствующих переменных.

<div class="grid" markdown>

=== "SCSS"

    ```scss
    @mixin rtl($property, $ltr-value, $rtl-value) {
      #{$property}: $ltr-value;

      [dir=rtl] & {
        #{$property}: $rtl-value;
      }
    }

    .sidebar {
      @include rtl(float, left, right);
    }
    ```

=== "Sass"

    ```sass
    @mixin rtl($property, $ltr-value, $rtl-value)
      #{$property}: $ltr-value

      [dir=rtl] &
        #{$property}: $rtl-value

    .sidebar
      @include rtl(float, left, right)
    ```

```css title="CSS"
.sidebar {
  float: left;
}
[dir=rtl] .sidebar {
  float: right;
}
```

</div>

!!! info "Информация"

    Списки аргументов также могут иметь висячие запятые! Это упрощает избежание синтаксических ошибок при рефакторинге ваших таблиц стилей.

### Необязательные аргументы {#optional-arguments}

Обычно каждый аргумент, объявленный миксином, должен быть передан при включении этого миксина. Однако вы можете сделать аргумент необязательным, определив *значение по умолчанию*, которое будет использоваться, если аргумент не передан. Значения по умолчанию используют тот же синтаксис, что и [объявления переменных](../variables): имя переменной, за которым следует двоеточие и [выражение SassScript](../syntax/structure#expressions). Это позволяет легко определять гибкие API миксинов, которые можно использовать простыми или сложными способами.

<div class="grid" markdown>

=== "SCSS"

    ```scss
    @mixin replace-text($image, $x: 50%, $y: 50%) {
      text-indent: -99999em;
      overflow: hidden;
      text-align: left;

      background: {
        image: $image;
        repeat: no-repeat;
        position: $x $y;
      }
    }

    .mail-icon {
      @include replace-text(url("/images/mail.svg"), 0);
    }
    ```

=== "Sass"

    ```sass
    @mixin replace-text($image, $x: 50%, $y: 50%)
      text-indent: -99999em
      overflow: hidden
      text-align: left

      background:
        image: $image
        repeat: no-repeat
        position: $x $y

    .mail-icon
      @include replace-text(url("/images/mail.svg"), 0)
    ```

```css title="CSS"
.mail-icon {
  text-indent: -99999em;
  overflow: hidden;
  text-align: left;
  background-image: url("/images/mail.svg");
  background-repeat: no-repeat;
  background-position: 0 50%;
}
```

</div>

!!! tip "Совет"

    Значения по умолчанию могут быть любым выражением SassScript, и они даже могут ссылаться на предыдущие аргументы!

### Именованные аргументы {#keyword-arguments}

При включении миксина аргументы можно передавать по имени, в дополнение к передаче их по позиции в списке аргументов. Это особенно полезно для миксинов с несколькими необязательными аргументами или с [булевыми](../values/booleans) аргументами, смысл которых не очевиден без сопровождающего имени. Именованные аргументы используют тот же синтаксис, что и [объявления переменных](../variables) и [необязательные аргументы](#optional-arguments).

<div class="grid" markdown>

=== "SCSS"

    ```scss
    @mixin square($size, $radius: 0) {
      width: $size;
      height: $size;

      @if $radius != 0 {
        border-radius: $radius;
      }
    }

    .avatar {
      @include square(100px, $radius: 4px);
    }
    ```

=== "Sass"

    ```sass
    @mixin square($size, $radius: 0)
      width: $size
      height: $size

      @if $radius != 0
        border-radius: $radius

    .avatar
      @include square(100px, $radius: 4px)
    ```

```css title="CSS"
.avatar {
  width: 100px;
  height: 100px;
  border-radius: 4px;
}
```

</div>

!!! note "Примечание"

    Поскольку *любой* аргумент можно передать по имени, будьте осторожны при переименовании аргументов миксина... это может сломать код ваших пользователей! Может быть полезно на некоторое время оставить старое имя в виде [необязательного аргумента](#optional-arguments) и выводить [предупреждение](../at-rules/warn), если кто-то его передает, чтобы они знали о необходимости миграции на новый аргумент.

### Прием произвольных аргументов {#taking-arbitrary-arguments}

Иногда полезно, чтобы миксин мог принимать любое количество аргументов. Если последний аргумент в объявлении `@mixin` заканчивается на `...`, то все лишние аргументы этого миксина передаются в этот аргумент в виде [списка](../values/lists). Этот аргумент известен как [список аргументов](../values/lists#argument-lists).

<div class="grid" markdown>

=== "SCSS"

    ```scss
    @mixin order($height, $selectors...) {
      @for $i from 0 to length($selectors) {
        #{nth($selectors, $i + 1)} {
          position: absolute;
          height: $height;
          margin-top: $i * $height;
        }
      }
    }

    @include order(150px, "input.name", "input.address", "input.zip");
    ```

=== "Sass"

    ```sass
    @mixin order($height, $selectors...)
      @for $i from 0 to length($selectors)
        #{nth($selectors, $i + 1)}
          position: absolute
          height: $height
          margin-top: $i * $height

    @include order(150px, "input.name", "input.address", "input.zip")
    ```

```css title="CSS"
input.name {
  position: absolute;
  height: 150px;
  margin-top: 0px;
}

input.address {
  position: absolute;
  height: 150px;
  margin-top: 150px;
}

input.zip {
  position: absolute;
  height: 150px;
  margin-top: 300px;
}
```

</div>

#### Прием произвольных именованных аргументов {#taking-arbitrary-keyword-arguments}

Списки аргументов также можно использовать для приема произвольных именованных аргументов. Функция [`meta.keywords()`](../modules/meta#keywords) принимает список аргументов и возвращает все дополнительные именованные аргументы, переданные миксину, в виде [карты](../values/maps), где ключами являются имена аргументов (без `$`), а значениями — соответствующие значения этих аргументов.

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

=== "Sass"

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

!!! tip "Совет"

    Если вы никогда не передаёте список аргументов в функцию [`meta.keywords()`](../modules/meta#keywords), этот список аргументов не будет принимать дополнительные именованные аргументы. Это помогает вызывающим ваш миксин убедиться, что они случайно не написали имя аргумента с ошибкой.

#### Передача произвольных аргументов {#passing-arbitrary-arguments}

Как списки аргументов позволяют миксинам принимать произвольные позиционные или именованные аргументы, так же тот же синтаксис можно использовать для *передачи* позиционных и именованных аргументов в миксин. Если вы передаёте список, за которым следует `...` в качестве последнего аргумента включения, его элементы будут рассматриваться как дополнительные позиционные аргументы. Аналогично, карта, за которой следует `...`, будет рассматриваться как дополнительные именованные аргументы. Вы даже можете передать оба одновременно!

<div class="grid" markdown>

=== "SCSS"

    ```scss
    $form-selectors: "input.name", "input.address", "input.zip" !default;

    @include order(150px, $form-selectors...);
    ```

=== "Sass"

    ```sass
    $form-selectors: "input.name", "input.address", "input.zip" !default

    @include order(150px, $form-selectors...)
    ```

</div>

!!! note "Примечание"

    Поскольку [список аргументов](../values/lists#argument-lists) отслеживает как позиционные, так и именованные аргументы, вы используете его для передачи обоих типов одновременно в другой миксин. Это делает невероятно простым определение псевдонима для миксина!

    <div class="grid" markdown>

    === "SCSS"

        ```scss
        @mixin btn($args...) {
          @warn "The btn() mixin is deprecated. Include button() instead.";
          @include button($args...);
        }
        ```

    === "Sass"

        ```sass
        @mixin btn($args...)
          @warn "The btn() mixin is deprecated. Include button() instead."
          @include button($args...)
        ```

    </div>

## Блоки содержимого {#content-blocks}

Помимо приема аргументов, миксин может принимать целый блок стилей, называемый *блоком содержимого*. Миксин может объявить, что принимает блок содержимого, включив в свое тело at-правило `@content`. Блок содержимого передается с помощью фигурных скобок, как любой другой блок в Sass, и вставляется на место правила `@content`.

<div class="grid" markdown>

=== "SCSS"

    ```scss
    @mixin hover {
      &:not([disabled]):hover {
        @content;
      }
    }

    .button {
      border: 1px solid black;
      @include hover {
        border-width: 2px;
      }
    }
    ```

=== "Sass"

    ```sass
    @mixin hover
      &:not([disabled]):hover
        @content

    .button
      border: 1px solid black
      @include hover
        border-width: 2px
    ```

```css title="CSS"
.button {
  border: 1px solid black;
}
.button:not([disabled]):hover {
  border-width: 2px;
}
```

</div>

!!! info "Информация"

    Миксин может включать несколько at-правил `@content`. Если это так, блок содержимого будет включен отдельно для каждого `@content`.

!!! note "Примечание"

    Блок содержимого имеет *лексическую область видимости*, что означает, что он может видеть только [локальные переменные](../variables#scope) в области видимости, где включен миксин. Он не может видеть переменные, определённые в миксине, в который он передается, даже если они определены до вызова блока содержимого.

### Передача аргументов блокам содержимого {#passing-arguments-to-content-blocks}

Миксин может передавать аргументы своему блоку содержимого так же, как он передавал бы аргументы другому миксину, написав `@content(<arguments...>)`. Пользователь, пишущий блок содержимого, может принимать аргументы, написав `@include <name> using (<arguments...>)`. Список аргументов для блока содержимого работает точно так же, как список аргументов миксина, а аргументы, переданные ему `@content`, работают точно так же, как передача аргументов миксину.

!!! tip "Совет"

    Если миксин передает аргументы своему блоку содержимого, этот блок содержимого *должен* объявить, что принимает эти аргументы. Это означает, что лучше передавать аргументы по позиции (а не по имени), и это означает, что передача большего количества аргументов — это разрушительное изменение.

    Если вы хотите быть гибкими в информации, которую передаете блоку содержимого, рассмотрите передачу ему [карты](../values/maps), содержащей нужную информацию!

<div class="grid" markdown>

=== "SCSS"

    ```scss
    @mixin media($types...) {
      @each $type in $types {
        @media #{$type} {
          @content($type);
        }
      }
    }

    @include media(screen, print) using ($type) {
      h1 {
        font-size: 40px;
        @if $type == print {
          font-family: Calluna;
        }
      }
    }
    ```

=== "Sass"

    ```sass
    @mixin media($types...)
      @each $type in $types
        @media #{$type}
          @content($type)

    @include media(screen, print) using ($type)
      h1
        font-size: 40px
        @if $type == print
          font-family: Calluna
    ```

```css title="CSS"
@media screen {
  h1 {
    font-size: 40px;
  }
}
@media print {
  h1 {
    font-size: 40px;
    font-family: Calluna;
  }
}
```

</div>

## Синтаксис с отступами для миксинов {#indented-mixin-syntax}

[Синтаксис с отступами](../syntax#the-indented-syntax) предлагает альтернативный способ работы с миксинами помимо стандартных `@mixin` и `@include`. Миксины определяются символом `=`, а вызываются с помощью `+`. Этот вариант короче, но менее интуитивен для чтения, поэтому его не рекомендуют использовать.

<div class="grid" markdown>

=== "SASS"

    ```sass
    =reset-list
      margin: 0
      padding: 0
      list-style: none

    =horizontal-list
      +reset-list

      li
        display: inline-block
        margin:
          left: -2px
          right: 2em

    nav ul
      +horizontal-list
    ```

```css title="CSS"
nav ul {
  margin: 0;
  padding: 0;
  list-style: none;
}
nav ul li {
  display: inline-block;
  margin-left: -2px;
  margin-right: 2em;
}
```

</div>
