---
title: Операторы равенства
icon: lucide/equal
---

Операторы равенства возвращают, совпадают ли два значения. Они записываются как `<выражение> == <выражение>`, что означает, равны ли два [выражения](../syntax/structure#expressions), и `<выражение> != <выражение>`, что означает, *не* равны ли два выражения. Два значения считаются равными, если у них совпадают тип *и* значение, причём для разных типов это определяется по‑разному:

* [Числа](../values/numbers) равны, если у них одинаковое значение *и* одинаковые единицы измерения, либо если их значения совпадают после преобразования единиц друг в друга.
* [Строки](../values/strings) ведут себя необычно, так как [без кавычек](../values/strings#unquoted) и [в кавычках](../values/strings#quoted) с одинаковым содержимым считаются равными.
* [Цвета](../values/colors) равны, если они находятся в одном [цветовом пространстве](../values/colors#color-spaces) и имеют одинаковые значения каналов *или* если они оба в [устаревших цветовых пространствах](../values/colors#legacy-color-spaces) и имеют одинаковые RGBA‑каналы.
* [Списки](../values/lists) равны, если их содержимое совпадает. Списки, разделённые запятыми, не равны спискам, разделённым пробелами, а списки в скобках — спискам без скобок.
* [Карты](../values/maps) равны, если совпадают и ключи, и значения.
* [Вычисления](../values/calculations) равны, если у них одинаковые имена и аргументы. Аргументы‑операции сравниваются текстово.
* [`true`, `false`](../values/booleans) и [`null`](../values/null) равны только сами себе.
* [Функции](../values/functions) равны только одной и той же функции. Функции сравниваются *по ссылке*, поэтому даже функции с одинаковым именем и определением считаются разными, если они определены в разных местах.

=== "SCSS"

    ```scss
    @debug 1px == 1px; // true
    @debug 1px != 1em; // true
    @debug 1 != 1px; // true
    @debug 96px == 1in; // true

    @debug "Helvetica" == Helvetica; // true
    @debug "Helvetica" != "Arial"; // true

    @debug hsl(34, 35%, 92.1%) == #f2ece4; // true
    @debug rgba(179, 115, 153, 0.5) != rgba(179, 115, 153, 0.8); // true

    @debug (5px 7px 10px) == (5px 7px 10px); // true
    @debug (5px 7px 10px) != (10px 14px 20px); // true
    @debug (5px 7px 10px) != (5px, 7px, 10px); // true
    @debug (5px 7px 10px) != [5px 7px 10px]; // true

    $theme: ("venus": #998099, "nebula": #d2e1dd);
    @debug $theme == ("venus": #998099, "nebula": #d2e1dd); // true
    @debug $theme != ("venus": #998099, "iron": #dadbdf); // true

    @debug true == true; // true
    @debug true != false; // true
    @debug null != false; // true

    @debug get-function("rgba") == get-function("rgba"); // true
    @debug get-function("rgba") != get-function("hsla"); // true
    ```

=== "SASS"

    ```sass
    @debug 1px == 1px // true
    @debug 1px != 1em // true
    @debug 1 != 1px // true
    @debug 96px == 1in // true

    @debug "Helvetica" == Helvetica // true
    @debug "Helvetica" != "Arial" // true

    @debug hsl(34, 35%, 92.1%) == #f2ece4 // true
    @debug rgba(179, 115, 153, 0.5) != rgba(179, 115, 153, 0.8) // true

    @debug (5px 7px 10px) == (5px 7px 10px) // true
    @debug (5px 7px 10px) != (10px 14px 20px) // true
    @debug (5px 7px 10px) != (5px, 7px, 10px) // true
    @debug (5px 7px 10px) != [5px 7px 10px] // true

    $theme: ("venus": #998099, "nebula": #d2e1dd)
    @debug $theme == ("venus": #998099, "nebula": #d2e1dd) // true
    @debug $theme != ("venus": #998099, "iron": #dadbdf) // true

    @debug calc(10px + 10%) == calc(10px + 10%) // true
    @debug calc(10% + 10px) == calc(10px + 10%) // false

    @debug true == true // true
    @debug true != false // true
    @debug null != false // true

    @debug get-function("rgba") == get-function("rgba") // true
    @debug get-function("rgba") != get-function("hsla") // true
    ```
