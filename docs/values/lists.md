---
title: Списки
icon: lucide/list-checks
---

Списки содержат последовательность других значений. В Sass элементы в списках могут быть разделены запятыми (`Helvetica, Arial, sans-serif`), пробелами (`10px 15px 0 0`) или [слешами](#slash-separated-lists), если это единообразно в пределах списка. В отличие от большинства других языков, списки в Sass не требуют специальных скобок; любые [выражения](../syntax/structure#expressions), разделённые пробелами или запятыми, считаются списком. Однако вы можете записывать списки с квадратными скобками (`[line1 line2]`), что полезно при использовании [`grid-template-columns`](https://developer.mozilla.org/en-US/docs/Web/CSS/grid-template-columns).

При записи списков без скобок вы можете использовать круглые скобки для вложения списков друг в друга или для устранения неоднозначности между разделителями списка и другими использованиями пробелов или запятых. Например, `(1, 2), (3, 4)` — это список, содержащий два списка, каждый из которых содержит два числа; а `adjust-font-stack((Helvetica, Arial, sans-serif))` передает один аргумент, содержащий три имени шрифтов, функции `adjust-font-stack`.

Списки Sass могут содержать один или даже ноль элементов. Список из одного элемента можно записать как `(<выражение>,)` или `[<выражение>]`, а список из нуля элементов можно записать как `()` или `[]`. Кроме того, все [функции списков](../modules/list) будут обрабатывать отдельные значения, которые не находятся в списках, так, как если бы они были списками, содержащими это значение, что означает, что вам редко нужно явно создавать списки из одного элемента.

!!! tip "Совет"

    Пустые списки без скобок не являются допустимым CSS, поэтому Sass не позволит вам использовать их в значении свойства.

## Списки, разделённые слешами {#slash-separated-lists}

Списки в Sass могут быть разделены слешами для представления значений вроде сокращения `font: 12px/30px` для установки `font-size` и `line-height` или синтаксиса `hsl(80 100% 50% / 0.5)` для создания цвета с заданным значением прозрачности. Однако **списки, разделённые слешами, в настоящее время нельзя записать буквально.** Sass исторически использовал символ `/` для обозначения деления, поэтому пока существующие таблицы стилей переходят на использование [`math.div()`](../modules/math#div), списки, разделённые слешами, можно записать только с помощью [`list.slash()`](../modules/list#slash).

Для получения более подробной информации см. [Критическое изменение: слеш как деление](../breaking-changes/slash-div).

## Использование списков {#using-lists}

Sass предоставляет несколько [функций](../modules/list), которые позволяют использовать списки для написания мощных библиотек стилей или для того, чтобы сделать таблицу стилей вашего приложения чище и более удобной в обслуживании.

### Индексы {#indexes}

Многие из этих функций принимают или возвращают числа, называемые *индексами*, которые ссылаются на элементы в списке. Индекс 1 указывает на первый элемент списка. Обратите внимание, что это отличается от многих языков программирования, где индексы начинаются с 0! Sass также упрощает ссылку на конец списка. Индекс -1 ссылается на последний элемент в списке, -2 ссылается на предпоследний и так далее.

### Доступ к элементу {#access-an-element}

Списки мало полезны, если вы не можете извлечь из них значения. Вы можете использовать [функцию `list.nth($list, $n)`](../modules/list#nth), чтобы получить элемент по заданному индексу в списке. Первый аргумент — это сам список, а второй — индекс значения, которое вы хотите получить.

=== "SCSS"

    ```scss
    @use 'sass:list';

    @debug list.nth(10px 12px 16px, 2); // 12px
    @debug list.nth([line1, line2, line3], -1); // line3
    ```

=== "SASS"

    ```sass
    @use 'sass:list'

    @debug list.nth(10px 12px 16px, 2)  // 12px
    @debug list.nth([line1, line2, line3], -1)  // line3
    ```

### Выполнение действий для каждого элемента {#do-something-for-every-element}

Это на самом деле не использует функцию, но это всё ещё один из наиболее распространённых способов использования списков. [Правило `@each`](../at-rules/control/each) выполняет блок стилей для каждого элемента в списке и присваивает этот элемент переменной.

<div class="grid" markdown>

=== "SCSS"

    ```scss
    $sizes: 40px, 50px, 80px;

    @each $size in $sizes {
      .icon-#{$size} {
        font-size: $size;
        height: $size;
        width: $size;
      }
    }
    ```

=== "SASS"

    ```sass
    $sizes: 40px, 50px, 80px

    @each $size in $sizes
      .icon-#{$size}
        font-size: $size
        height: $size
        width: $size
    ```

```css title="CSS"
.icon-40px {
  font-size: 40px;
  height: 40px;
  width: 40px;
}

.icon-50px {
  font-size: 50px;
  height: 50px;
  width: 50px;
}

.icon-80px {
  font-size: 80px;
  height: 80px;
  width: 80px;
}
```

</div>

### Добавление в список {#add-to-a-list}

Также полезно добавлять элементы в список. [Функция `list.append($list, $val)`](../modules/list#append) принимает список и значение и возвращает копию списка с добавленным в конец значением. Обратите внимание, что, поскольку списки Sass являются [неизменяемыми](#immutability), оригинальный список не изменяется.

=== "SCSS"

    ```scss
    @debug append(10px 12px 16px, 25px); // 10px 12px 16px 25px
    @debug append([col1-line1], col1-line2); // [col1-line1, col1-line2]
    ```

=== "SASS"

    ```sass
    @debug append(10px 12px 16px, 25px)  // 10px 12px 16px 25px
    @debug append([col1-line1], col1-line2)  // [col1-line1, col1-line2]
    ```

### Поиск элемента в списке {#find-an-element-in-a-list}

Если вам нужно проверить, находится ли элемент в списке, или узнать, под каким индексом он находится, используйте [функцию `list.index($list, $value)`](../modules/list#index). Она принимает список и значение, которое нужно найти в этом списке, и возвращает индекс этого значения.

=== "SCSS"

    ```scss
    @use 'sass:list';

    @debug list.index(1px solid red, 1px); // 1
    @debug list.index(1px solid red, solid); // 2
    @debug list.index(1px solid red, dashed); // null
    ```

=== "SASS"

    ```sass
    @use 'sass:list'

    @debug list.index(1px solid red, 1px)  // 1
    @debug list.index(1px solid red, solid)  // 2
    @debug list.index(1px solid red, dashed)  // null
    ```

Если значения нет в списке вообще, `list.index()` возвращает [`null`](../values/null). Поскольку `null` считается [ложным значением](../at-rules/control/if#truthiness-and-falsiness), вы можете использовать `list.index()` с [`@if`](../at-rules/control/if) или [`if()`](../syntax/special-functions#if), чтобы проверить, содержит ли список заданное значение или нет.

=== "SCSS"

    ```scss
    @use "sass:list";

    $valid-sides: top, bottom, left, right;

    @mixin attach($side) {
      @if not list.index($valid-sides, $side) {
        @error "#{$side} is not a valid side. Expected one of #{$valid-sides}.";
      }

      // ...
    }
    ```

=== "SASS"

    ```sass
    @use "sass:list"

    $valid-sides: top, bottom, left, right

    @mixin attach($side)
      @if not list.index($valid-sides, $side)
        @error "#{$side} is not a valid side. Expected one of #{$valid-sides}."

      // ...
    ```

## Неизменяемость {#immutability}

Списки в Sass *неизменяемы*, что означает, что содержимое значения-списка никогда не меняется. Все функции для работы со списками в Sass возвращают новые списки, а не изменяют исходные. Неизменяемость помогает избежать множества скрытых ошибок, которые могут возникать, когда один и тот же список используется в разных частях таблицы стилей.

Тем не менее, вы всё равно можете обновлять состояние со временем, присваивая новые списки одной и той же переменной. Это часто используется в функциях и миксинах для сбора набора значений в один список.

=== "SCSS"

    ```scss
    @use "sass:list";
    @use "sass:map";

    $prefixes-by-browser: ("firefox": moz, "safari": webkit, "ie": ms);

    @function prefixes-for-browsers($browsers) {
      $prefixes: ();
      @each $browser in $browsers {
        $prefixes: list.append($prefixes, map.get($prefixes-by-browser, $browser));
      }
      @return $prefixes;
    }

    @debug prefixes-for-browsers("firefox" "ie"); // moz ms
    ```

=== "SASS"

    ```sass
    @use "sass:list"
    @use "sass:map"

    $prefixes-by-browser: ("firefox": moz, "safari": webkit, "ie": ms)

    @function prefixes-for-browsers($browsers)
      $prefixes: ()
      @each $browser in $browsers
        $prefixes: list.append($prefixes, map.get($prefixes-by-browser, $browser))

      @return $prefixes

    @debug prefixes-for-browsers("firefox" "ie")  // moz ms
    ```

## Списки аргументов {#argument-lists}

Когда вы объявляете миксин или функцию, принимающую [произвольные аргументы](../at-rules/mixin#taking-arbitrary-arguments), значение, которое вы получаете, представляет собой специальный список, называемый *списком аргументов*. Он ведет себя точно так же, как обычный список, содержащий все аргументы, переданные миксину или функции, с одной дополнительной возможностью: если пользователь передал именованные аргументы, к ним можно получить доступ как к карте (map), передав список аргументов функции [`meta.keywords()`](../modules/meta#keywords).

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
