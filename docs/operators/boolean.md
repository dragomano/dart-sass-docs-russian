---
title: Логические операторы
icon: lucide/circle-slash-2
---

> В отличие от таких языков, как JavaScript, Sass использует слова, а не символы для своих [логических](../values/booleans) операторов.

* `not <выражение>` возвращает противоположное значение выражения: превращает `true` в `false`, а `false` в `true`.
* `<выражение> and <выражение>` возвращает `true`, если значения *обоих* выражений равны `true`, и `false`, если хотя бы одно из них равно `false`.
* `<выражение> or <выражение>` возвращает `true`, если значение *хотя бы одного* выражения равно `true`, и `false`, если оба равны `false`.

=== "SCSS"

    ```scss
    @debug not true; // false
    @debug not false; // true

    @debug true and true; // true
    @debug true and false; // false

    @debug true or false; // true
    @debug false or false; // false
    ```

=== "SASS"

    ```sass
    @debug not true // false
    @debug not false // true

    @debug true and true // true
    @debug true and false // false

    @debug true or false // true
    @debug false or false // false
    ```

## Истинность и ложность {#truthiness-and-falsiness}

Везде, где допускаются `true` или `false`, можно использовать и другие значения. Значения `false` и [`null`](../values/null) считаются *ложными* (falsey), то есть Sass воспринимает их как обозначающие ложь и приводящие к тому, что условия не выполняются. Все остальные значения считаются *истинными* (truthy), поэтому Sass воспринимает их как эквивалент `true`, и такие значения приводят к выполнению условий.

Например, если нужно проверить, содержит ли строка пробел, можно просто написать `string.index($string, " ")`. Функция [`string.index()`](../modules/string#index) возвращает `null`, если подстрока не найдена, и число в противном случае.

!!! note "Примечание"

    Некоторые языки считают ложными больше значений, чем просто `false` и `null`. Sass к таким языкам не относится: пустые строки, пустые списки и число `0` в Sass являются истинными (truthy).
