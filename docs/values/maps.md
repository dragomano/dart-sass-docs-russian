---
title: Карты
icon: lucide/map
---

Карты в Sass содержат пары ключей и значений и упрощают поиск значения по соответствующему ключу. Они записываются как `(<выражение>: <выражение>, <выражение>: <выражение>)`, где [выражение](../syntax/structure#expressions) до `:` — это ключ, а выражение после `:` — связанное с ним значение. Ключи должны быть уникальными, но значения могут повторяться. В отличие от [списков](../values/lists), карты *обязательно* записываются в круглых скобках. Пустая карта записывается как `()`.

!!! note "Примечание"

    Проницательные читатели могут заметить, что пустая карта `()` записывается так же, как и пустой список. Это потому, что она считается и картой, и списком. На самом деле *все* карты считаются списками! Каждая карта считается списком, который содержит двухэлементный список для каждой пары ключ/значение. Например, `(1: 2, 3: 4)` считается `(1 2, 3 4)`.

Карты позволяют использовать любые Sass-значения в качестве ключей. Для определения равенства двух ключей используется оператор [`==`](../operators/equality).

!!! tip "Совет"

    В большинстве случаев лучше использовать [строки в кавычках](../values/strings#quoted), а не [строки без кавычек](../values/strings#unquoted) в качестве ключей карт. Это связано с тем, что некоторые значения, такие как имена цветов, могут *выглядеть* как строки без кавычек, но на самом деле быть другими типами. Чтобы избежать путаницы в будущем, просто используйте кавычки!

## Использование карт {#using-maps}

Поскольку карты не являются допустимыми CSS-значениями, сами по себе они ничего не делают. Поэтому Sass предоставляет набор [функций](../modules/map) для создания карт и доступа к хранящимся в них значениям.

### Поиск значения {#look-up-a-value}

Карты предназначены для связывания ключей и значений, поэтому, естественно, существует способ получить значение по ключу: это функция [`map.get($map, $key)`](../modules/map#get)! Эта функция возвращает значение в карте, связанное с указанным ключом. Если карта не содержит такого ключа, она возвращает [`null`](../values/null).

=== "SCSS"

    ```scss
    @use "sass:map";
    $font-weights: ("regular": 400, "medium": 500, "bold": 700);

    @debug map.get($font-weights, "medium"); // 500
    @debug map.get($font-weights, "extra-bold"); // null
    ```

=== "SASS"

    ```sass
    @use "sass:map"
    $font-weights: ("regular": 400, "medium": 500, "bold": 700)

    @debug map.get($font-weights, "medium") // 500
    @debug map.get($font-weights, "extra-bold") // null
    ```

### Выполняем действие для каждой пары {#do-something-for-every-pair}

Это на самом деле не использует функцию, но всё равно является одним из самых распространённых способов работы с картами. Правило [`@each`](../at-rules/control/each) вычисляет блок стилей для каждой пары ключ/значение в карте. Ключ и значение присваиваются переменным, чтобы к ним можно было легко обратиться внутри блока.

<div class="grid" markdown>

=== "SCSS"

    ```scss
    $icons: ("eye": "\f112", "start": "\f12e", "stop": "\f12f");

    @each $name, $glyph in $icons {
      .icon-#{$name}:before {
        display: inline-block;
        font-family: "Icon Font";
        content: $glyph;
      }
    }
    ```

=== "SASS"

    ```sass
    $icons: ("eye": "\f112", "start": "\f12e", "stop": "\f12f")

    @each $name, $glyph in $icons
      .icon-#{$name}:before
        display: inline-block
        font-family: "Icon Font"
        content: $glyph
    ```

```css title="CSS"
.icon-eye:before {
  display: inline-block;
  font-family: "Icon Font";
  content: "\f112";
}

.icon-start:before {
  display: inline-block;
  font-family: "Icon Font";
  content: "\f12e";
}

.icon-stop:before {
  display: inline-block;
  font-family: "Icon Font";
  content: "\f12f";
}
```

</div>

### Добавление в карту {#add-to-a-map}

Также полезно добавлять новые пары в карту или заменять значение для существующего ключа. Это делает функция [`map.set($map, $key, $value)`](../modules/map#set): она возвращает копию `$map` со значением `$value` для ключа `$key`.

=== "SCSS"

    ```scss
    @use "sass:map";

    $font-weights: ("regular": 400, "medium": 500, "bold": 700);

    @debug map.set($font-weights, "extra-bold", 900);
    // ("regular": 400, "medium": 500, "bold": 700, "extra-bold": 900)
    @debug map.set($font-weights, "bold", 900);
    // ("regular": 400, "medium": 500, "bold": 900)
    ```

=== "SASS"

    ```sass
    @use "sass:map"

    $font-weights: ("regular": 400, "medium": 500, "bold": 700)

    @debug map.set($font-weights, "extra-bold": 900)
    // ("regular": 400, "medium": 500, "bold": 700, "extra-bold": 900)
    @debug map.set($font-weights, "bold", 900)
    // ("regular": 400, "medium": 500, "bold": 900)
    ```

Вместо установки значений по одному можно также объединить две существующие карты с помощью [`map.merge($map1, $map2)`](../modules/map#merge).

=== "SCSS"

    ```scss
    @use "sass:map";

    $light-weights: ("lightest": 100, "light": 300);
    $heavy-weights: ("medium": 500, "bold": 700);

    @debug map.merge($light-weights, $heavy-weights);
    // ("lightest": 100, "light": 300, "medium": 500, "bold": 700)
    ```

=== "SASS"

    ```sass
    @use "sass:map"

    $light-weights: ("lightest": 100, "light": 300)
    $heavy-weights: ("medium": 500, "bold": 700)

    @debug map.merge($light-weights, $heavy-weights)
    // ("lightest": 100, "light": 300, "medium": 500, "bold": 700)
    ```

Если обе карты имеют одинаковые ключи, в возвращаемой карте используются значения из второй карты.

=== "SCSS"

    ```scss
    @use "sass:map";

    $weights: ("light": 300, "medium": 500);

    @debug map.merge($weights, ("medium": 700));
    // ("light": 300, "medium": 700)
    ```

=== "SASS"

    ```sass
    @use "sass:map";

    $weights: ("light": 300, "medium": 500)

    @debug map.merge($weights, ("medium": 700))
    // ("light": 300, "medium": 700)
    ```

Обратите внимание, что поскольку карты в Sass являются [неизменяемыми](#immutability), `map.set()` и `map.merge()` не модифицируют исходный список.

## Неизменяемость {#immutability}

Карты в Sass *неизменяемы*, что означает, что содержимое значения карты никогда не меняется. Все функции карт Sass возвращают новые карты, а не изменяют оригиналы. Неизменяемость помогает избежать множества скрытых ошибок, которые могут возникнуть, когда одна и та же карта используется в разных частях таблицы стилей.

Тем не менее, вы можете обновлять состояние с течением времени, присваивая новые карты той же переменной. Это часто используется в функциях и миксинах для отслеживания конфигурации в карте.

=== "SCSS"

    ```scss
    @use "sass:map";

    $prefixes-by-browser: ("firefox": moz, "safari": webkit, "ie": ms);

    @mixin add-browser-prefix($browser, $prefix) {
      $prefixes-by-browser: map.merge($prefixes-by-browser, ($browser: $prefix)) !global;
    }

    @include add-browser-prefix("opera", o);
    @debug $prefixes-by-browser;
    // ("firefox": moz, "safari": webkit, "ie": ms, "opera": o)
    ```

=== "SASS"

    ```sass
    @use "sass:map"

    $prefixes-by-browser: ("firefox": moz, "safari": webkit, "ie": ms)

    @mixin add-browser-prefix($browser, $prefix)
      $prefixes-by-browser: map.merge($prefixes-by-browser, ($browser: $prefix)) !global

    @include add-browser-prefix("opera", o)
    @debug $prefixes-by-browser
    // ("firefox": moz, "safari": webkit, "ie": ms, "opera": o)
    ```
