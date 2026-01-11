---
title: "null"
icon: lucide/circle-slash-2
---

> Значение `null` является единственным значением своего типа. Оно представляет собой отсутствие значения и часто возвращается [функциями](../at-rules/function), чтобы указать на отсутствие результата.

=== "SCSS"

    ```scss
    @use "sass:map";
    @use "sass:string";

    @debug string.index("Helvetica Neue", "Roboto"); // null
    @debug map.get(("large": 20px), "small"); // null
    @debug &; // null
    ```

=== "SASS"

    ```sass
    @use "sass:map"
    @use "sass:string"

    @debug string.index("Helvetica Neue", "Roboto") // null
    @debug map.get(("large": 20px), "small") // null
    @debug & // null
    ```

Если [список](../values/lists) содержит `null`, этот `null` исключается из сгенерированного CSS.

<div class="grid" markdown>

=== "SCSS"

    ```scss
    $fonts: ("serif": "Helvetica Neue", "monospace": "Consolas");

    h3 {
      font: 18px bold map-get($fonts, "sans");
    }
    ```

=== "SASS"

    ```sass
    $fonts: ("serif": "Helvetica Neue", "monospace": "Consolas")

    h3
      font: 18px bold map-get($fonts, "sans")
    ```

```css title="CSS"
h3 {
  font: 18px bold;
}
```

</div>

Если значение свойства равно `null`, это свойство полностью исключается.

<div class="grid" markdown>

=== "SCSS"

    ```scss
    $fonts: ("serif": "Helvetica Neue", "monospace": "Consolas");

    h3 {
      font: {
        size: 18px;
        weight: bold;
        family: map-get($fonts, "sans");
      }
    }
    ```

=== "SASS"

    ```sass
    $fonts: ("serif": "Helvetica Neue", "monospace": "Consolas")

    h3
      font:
        size: 18px
        weight: bold
        family: map-get($fonts, "sans")
    ```

```css title="CSS"
h3 {
  font-size: 18px;
  font-weight: bold;
}
```

</div>

`null` также является [*ложным*](../at-rules/control/if#truthiness-and-falsiness), что означает, что он считается `false` для любых правил или [операторов](../operators/boolean), которые принимают булевы значения. Это упрощает использование значений, которые могут быть `null`, в качестве условий для [`@if`](../at-rules/control/if) и [`if()`](../syntax/special-functions#if).

<div class="grid" markdown>

=== "SCSS"

    ```scss
    @mixin app-background($color) {
      #{if(sass(&): '&.app-background'; else: '.app-background')} {
        background-color: $color;
        color: rgba(#fff, 0.75);
      }
    }

    @include app-background(#036);

    .sidebar {
      @include app-background(#c6538c);
    }
    ```

=== "SASS"

    ```sass
    @mixin app-background($color)
      #{if(sass(&): '&.app-background'; else: '.app-background')}
        background-color: $color
        color: rgba(#fff, 0.75)

    @include app-background(#036)

    .sidebar
      @include app-background(#c6538c)
    ```

```css title="CSS"
.app-background {
  background-color: #036;
  color: rgba(255, 255, 255, 0.75);
}

.sidebar.app-background {
  background-color: #c6538c;
  color: rgba(255, 255, 255, 0.75);
}
```

</div>
