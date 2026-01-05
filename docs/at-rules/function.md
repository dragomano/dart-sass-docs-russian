---
title: "@function"
icon: lucide/at-sign
---

> Функции позволяют определять сложные операции над значениями SassScript, которые можно повторно использовать в вашей таблице стилей. Они облегчают абстрагирование распространённых формул и поведений в удобочитаемом виде.

Функции определяются с помощью at-правила `@function`, которое записывается как `@function <имя>(<аргументы...>) { ... }`. Имя функции может быть любым идентификатором Sass, который не начинается с `--`. Оно может содержать только [универсальные инструкции](../syntax/structure#universal-statements), а также [at-правило `@return`](#return), которое указывает значение для использования в качестве результата вызова функции. Функции вызываются с использованием обычного синтаксиса CSS-функций.

<div class="grid" markdown>

=== "SCSS"

    ```scss
    @function fibonacci($n) {
      $sequence: 0 1;
      @for $_ from 1 through $n {
        $new: nth($sequence, length($sequence)) + nth($sequence, length($sequence) - 1);
        $sequence: append($sequence, $new);
      }
      @return nth($sequence, length($sequence));
    }

    .sidebar {
      float: left;
      margin-left: fibonacci(4) * 1px;
    }
    ```

=== "SASS"

    ```sass
    @function fibonacci($n)
      $sequence: 0 1
      @for $_ from 1 through $n
        $new: nth($sequence, length($sequence)) + nth($sequence, length($sequence) - 1)
        $sequence: append($sequence, $new)
      @return nth($sequence, length($sequence))

    .sidebar
      float: left
      margin-left: fibonacci(4) * 1px
    ```

```css title="CSS"
.sidebar {
  float: left;
  margin-left: 5px;
}
```

</div>

!!! note "Примечание"

    Имена функций, как и все идентификаторы Sass, относятся к дефисам и подчёркиваниям как к одному и тому же символу. Это означает, что `scale-color` и `scale_color` ссылаются на одну и ту же функцию. Это историческое наследие самых ранних времён Sass, когда в именах идентификаторов *разрешались только* символы подчёркивания. Когда в Sass появилась поддержка дефисов, чтобы соответствовать синтаксису CSS, эти два варианта сделали эквивалентными, чтобы упростить миграцию.

!!! tip "Совет"

    Хотя технически функции могут иметь побочные эффекты, например устанавливать [глобальные переменные](../variables#scope), делать так настоятельно не рекомендуется. Для побочных эффектов лучше использовать [миксины](../at-rules/mixin), а функции использовать только для вычисления значений.

## Аргументы {#arguments}

Аргументы позволяют настраивать поведение функций при каждом их вызове. Аргументы указываются в правиле `@function` после имени функции в виде списка имён переменных, заключённых в круглые скобки. Функцию необходимо вызывать с тем же количеством аргументов в виде [SassScript‑выражений](../syntax/structure#expressions). Значения этих выражений доступны внутри тела функции как соответствующие переменные.

!!! info "Информация"

    Списки аргументов также могут оканчиваться запятой! Это упрощает избегание синтаксических ошибок при рефакторинге таблиц стилей.

### Необязательные аргументы {#optional-arguments}

Обычно каждый аргумент, объявленный функцией, должен быть передан при её вызове. Однако можно сделать аргумент необязательным, задав для него *значение по умолчанию*, которое будет использоваться, если этот аргумент не передан. Значения по умолчанию используют тот же синтаксис, что и [объявления переменных](../variables): имя переменной, за которым следует двоеточие и [SassScript‑выражение](../syntax/structure#expressions). Это позволяет легко определять гибкие API функций, которые можно использовать как в простых, так и в более сложных сценариях.

<div class="grid" markdown>

=== "SCSS"

    ```scss
    @function invert($color, $amount: 100%) {
      $inverse: change-color($color, $hue: hue($color) + 180);
      @return mix($inverse, $color, $amount);
    }

    $primary-color: #036;
    .header {
      background-color: invert($primary-color, 80%);
    }
    ```

=== "SASS"

    ```sass
    @function invert($color, $amount: 100%)
      $inverse: change-color($color, $hue: hue($color) + 180)
      @return mix($inverse, $color, $amount)

    $primary-color: #036
    .header
      background-color: invert($primary-color, 80%)
    ```

```css title="CSS"
.header {
  background-color: rgb(81.6, 51, 20.4);
}
```

</div>

!!! tip "Совет"

    Значения по умолчанию могут быть любым SassScript-выражением и даже могут ссылаться на предыдущие аргументы!

### Именованные аргументы {#keyword-arguments}

При вызове функции аргументы можно передавать по имени в дополнение к передаче их по позиции в списке аргументов. Это особенно полезно для функций с несколькими необязательными аргументами или с [логическими](../values/booleans) аргументами, значение которых без имени не очевидно. Именованные аргументы используют тот же синтаксис, что и [объявления переменных](../variables) и [необязательные аргументы](#optional-arguments).

<div class="grid" markdown>

=== "SCSS"

    ```scss
    $primary-color: #036;
    .banner {
      background-color: $primary-color;
      color: scale-color($primary-color, $lightness: +40%);
    }
    ```

=== "SASS"

    ```sass
    $primary-color: #036
    .banner
      background-color: $primary-color
      color: scale-color($primary-color, $lightness: +40%)
    ```

```css title="CSS"
.banner {
  background-color: #036;
  color: rgb(10.2, 132.6, 255);
}
```

</div>

!!! warning "Предупреждение"

    Поскольку *любой* аргумент может быть передан по имени, будьте осторожны при переименовании аргументов функции... это может нарушить работу у ваших пользователей! Может быть полезно сохранить старое имя в качестве [необязательного аргумента](#optional-arguments) на некоторое время и выводить [предупреждение](../at-rules/warn), если кто-то его передаёт, чтобы они знали о необходимости перейти на новый аргумент.

### Принятие произвольных аргументов {#taking-arbitrary-arguments}

Иногда полезно, чтобы функция могла принимать любое количество аргументов. Если последний аргумент в объявлении `@function` заканчивается на `...`, то все дополнительные аргументы, переданные этой функции, передаются в этот аргумент в виде [списка](../values/lists). Такой аргумент называется [списком аргументов](../values/lists#argument-lists).

<div class="grid" markdown>

=== "SCSS"

    ```scss
    @function sum($numbers...) {
      $sum: 0;
      @each $number in $numbers {
        $sum: $sum + $number;
      }
      @return $sum;
    }

    .micro {
      width: sum(50px, 30px, 100px);
    }
    ```

=== "SASS"

    ```sass
    @function sum($numbers...)
      $sum: 0
      @each $number in $numbers
        $sum: $sum + $number

      @return $sum

    .micro
      width: sum(50px, 30px, 100px)
    ```

```css title="CSS"
.micro {
  width: 180px;
}
```

</div>

#### Принятие произвольных именованных аргументов {#taking-arbitrary-keyword-arguments}

Списки аргументов также могут использоваться для принятия произвольных именованных аргументов. Функция [`meta.keywords()`](../modules/meta#keywords) принимает список аргументов и возвращает все дополнительные именованные аргументы, которые были переданы функции, в виде [карты](../values/maps) от имён аргументов (без `$`) к их значениям.

!!! note "Примечание"

    Если вы никогда не передаёте список аргументов в функцию [`meta.keywords()`](../modules/meta#keywords), то этот список аргументов не будет разрешать дополнительные именованные аргументы. Это помогает вызывающим вашу функцию убедиться, что они случайно не допустили опечатку в имени аргумента.

#### Передача произвольных аргументов {#passing-arbitrary-arguments}

Так же, как списки аргументов позволяют функциям принимать произвольные позиционные или именованные аргументы, тот же синтаксис можно использовать для *передачи* позиционных и именованных аргументов в функцию. Если вы передаёте список, за которым следует `...`, в качестве последнего аргумента вызова функции, его элементы будут обработаны как дополнительные позиционные аргументы. Аналогично, карта, за которой следует `...`, будет обработана как дополнительные именованные аргументы. Вы даже можете передать и то, и другое одновременно!

<div class="grid" markdown>

=== "SCSS"

    ```scss
    $widths: 50px, 30px, 100px;
    .micro {
      width: min($widths...);
    }
    ```

=== "SASS"

    ```sass
    $widths: 50px, 30px, 100px
    .micro
      width: min($widths...)
    ```

```css title="CSS"
.micro {
  width: 30px;
}
```

</div>

!!! info "Информация"

    Поскольку [список аргументов](../values/lists#argument-lists) отслеживает как позиционные, так и именованные аргументы, вы можете использовать его для одновременной передачи обоих типов в другую функцию. Это делает создание псевдонима для функции очень простым!

    <div class="grid" markdown>

    === "SCSS"

        ```scss
        @function fg($args...) {
          @warn "The fg() function is deprecated. Call foreground() instead.";
          @return foreground($args...);
        }
        ```

    === "SASS"

        ```sass
        @function fg($args...)
          @warn "The fg() function is deprecated. Call foreground() instead."
          @return foreground($args...)
        ```

    </div>

## `@return` {#return}

At-правило `@return` указывает значение, которое будет использоваться в качестве результата вызова функции. Оно разрешено только внутри тела `@function`, и каждое определение `@function` должна заканчиваться `@return`.

Когда встречается выражение `@return`, оно немедленно завершает выполнение функции и возвращает её результат. Досрочный возврат может быть полезен для обработки граничных случаев или ситуаций, когда доступен более эффективный алгоритм, без необходимости оборачивать всю функцию в [блок `@else`](../at-rules/control/if#else).

<div class="grid" markdown>

=== "SCSS"

    ```scss
    @use "sass:string";

    @function str-insert($string, $insert, $index) {
      // Избегаем создания новых строк, если в этом нет необходимости.
      @if string.length($string) == 0 {
        @return $insert;
      }

      $before: string.slice($string, 0, $index);
      $after: string.slice($string, $index);
      @return $before + $insert + $after;
    }
    ```

=== "SASS"

    ```sass
    @use "sass:string"

    @function str-insert($string, $insert, $index)
      // Избегаем создания новых строк, если в этом нет необходимости.
      @if string.length($string) == 0
        @return $insert


      $before: string.slice($string, 0, $index)
      $after: string.slice($string, $index)
      @return $before + $insert + $after
    ```

</div>

## Другие функции {#other-functions}

Помимо пользовательских функций, Sass предоставляет обширную [основную библиотеку](../modules) встроенных функций, которые всегда доступны для использования. Реализации Sass также позволяют определять [пользовательские функции](../js-api/interfaces/LegacySharedOptions#functions) на языке среды выполнения. И, конечно, вы всегда можете вызывать [обычные CSS-функции](#plain-css-functions) (даже те, что имеют [необычный синтаксис](../syntax/special-functions)).

### Обычные CSS-функции {#plain-css-functions}

Любой вызов функции, который не является ни пользовательской, ни [встроенной](../modules) функцией, компилируется в обычную CSS-функцию (если только не используется [синтаксис аргументов Sass](/documentation/at-rules/function/#arguments)). Аргументы будут скомпилированы в CSS и включены в вызов функции как есть. Это гарантирует, что Sass поддерживает все CSS-функции без необходимости выпускать новые версии каждый раз, когда добавляется новая функция.

<div class="grid" markdown>

=== "SCSS"

    ```scss
    @debug var(--main-bg-color); // var(--main-bg-color)

    $primary: #f2ece4;
    $accent: #e1d7d2;
    @debug radial-gradient($primary, $accent); // radial-gradient(#f2ece4, #e1d7d2)
    ```

=== "SASS"

    ```sass
    @debug var(--main-bg-color)  // var(--main-bg-color)

    $primary: #f2ece4
    $accent: #e1d7d2
    @debug radial-gradient($primary, $accent)  // radial-gradient(#f2ece4, #e1d7d2)
    ```

</div>

!!! tip "Совет"

    Поскольку любая неизвестная функция будет скомпилирована в CSS, легко не заметить опечатку в имени функции. Рассмотрите возможность запуска [CSS-линтера](https://stylelint.io/) на результате компиляции вашей таблицы стилей, чтобы получать уведомления о таких случаях!

!!! note "Примечание"

    Некоторые CSS-функции, такие как `calc()` и `element()`, имеют необычный синтаксис. Sass [обрабатывает эти функции особым образом](../syntax/special-functions) как [строки без кавычек](../values/strings#unquoted).
