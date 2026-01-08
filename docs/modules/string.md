---
title: "sass:string"
icon: lucide/message-square-quote
---

## quote

```scss
string.quote($string)
quote($string) //=> string
```

Возвращает `$string` как строку в кавычках.

=== "SCSS"

    ```scss
    @use "sass:string";

    @debug string.quote(Helvetica); // "Helvetica"
    @debug string.quote("Helvetica"); // "Helvetica"
    ```

=== "SASS"

    ```sass
    @use "sass:string"

    @debug string.quote(Helvetica)  // "Helvetica"
    @debug string.quote("Helvetica")  // "Helvetica"
    ```

## index

```scss
string.index($string, $substring)
str-index($string, $substring) //=> number
```

Возвращает первый [индекс](../values/strings#string-indexes) подстроки `$substring` в строке `$string` или `null`, если `$string` не содержит `$substring`.

=== "SCSS"

    ```scss
    @use "sass:string";

    @debug string.index("Helvetica Neue", "Helvetica"); // 1
    @debug string.index("Helvetica Neue", "Neue"); // 11
    ```

=== "SASS"

    ```sass
    @use "sass:string"

    @debug string.index("Helvetica Neue", "Helvetica")  // 1
    @debug string.index("Helvetica Neue", "Neue")  // 11
    ```

## insert

```scss
string.insert($string, $insert, $index)
str-insert($string, $insert, $index) //=> string
```

Возвращает копию строки `$string` с вставленной подстрокой `$insert` в позиции [`$index`](../values/strings#string-indexes).

=== "SCSS"

    ```scss
    @use "sass:string";

    @debug string.insert("Roboto Bold", " Mono", 7); // "Roboto Mono Bold"
    @debug string.insert("Roboto Bold", " Mono", -6); // "Roboto Mono Bold"
    ```

=== "SASS"

    ```sass
    @use "sass:string"

    @debug string.insert("Roboto Bold", " Mono", 7)  // "Roboto Mono Bold"
    @debug string.insert("Roboto Bold", " Mono", -6)  // "Roboto Mono Bold"
    ```

Если `$index` больше длины строки `$string`, `$insert` добавляется в конец. Если `$index` меньше отрицательной длины строки, `$insert` добавляется в начало.

=== "SCSS"

    ```scss
    @use "sass:string";

    @debug string.insert("Roboto", " Bold", 100); // "Roboto Bold"
    @debug string.insert("Bold", "Roboto ", -100); // "Roboto Bold"
    ```

=== "SASS"

    ```sass
    @use "sass:string"

    @debug string.insert("Roboto", " Bold", 100)  // "Roboto Bold"
    @debug string.insert("Bold", "Roboto ", -100)  // "Roboto Bold"
    ```

## length

```scss
string.length($string)
str-length($string) //=> number
```

Возвращает количество символов в строке `$string`.

=== "SCSS"

    ```scss
    @use "sass:string";

    @debug string.length("Helvetica Neue"); // 14
    @debug string.length(bold); // 4
    @debug string.length(""); // 0
    ```

=== "SASS"

    ```sass
    @use "sass:string"

    @debug string.length("Helvetica Neue")  // 14
    @debug string.length(bold)  // 4
    @debug string.length("")  // 0
    ```

## slice

```scss
string.slice($string, $start-at, $end-at: -1)
str-slice($string, $start-at, $end-at: -1) //=> string
```

Возвращает фрагмент строки `$string`, начинающийся с [индекса](../values/strings#string-indexes) `$start-at` и заканчивающийся индексом `$end-at` (оба включительно).

=== "SCSS"

    ```scss
    @use "sass:string";

    @debug string.slice("Helvetica Neue", 11); // "Neue"
    @debug string.slice("Helvetica Neue", 1, 3); // "Hel"
    @debug string.slice("Helvetica Neue", 1, -6); // "Helvetica"
    ```

=== "SASS"

    ```sass
    @use "sass:string"

    @debug string.slice("Helvetica Neue", 11)  // "Neue"
    @debug string.slice("Helvetica Neue", 1, 3)  // "Hel"
    @debug string.slice("Helvetica Neue", 1, -6)  // "Helvetica"
    ```

## split

```scss
string.split($string, $separator, $limit: null) //=> list
```

Возвращает заключённый в скобки список подстрок `$string`, разделённых запятыми, которые были разделены с помощью `$separator`. Сами `$separator` не включаются в эти подстроки.

Если `$limit` — это число `1` или выше, разделение происходит не более чем по указанному количеству `$separator` (и, следовательно, возвращается не более `$limit + 1` строк). Последняя подстрока содержит остаток строки, включая все оставшиеся `$separator`.

=== "SCSS"

    ```scss
    @use "sass:string";

    @debug string.split("Segoe UI Emoji", " "); // ["Segoe", "UI", "Emoji"]
    @debug string.split("Segoe UI Emoji", " ", $limit: 1); // ["Segoe", "UI Emoji"]
    ```

=== "SASS"

    ```sass
    @use "sass:string"

    @debug string.split("Segoe UI Emoji", " ")  // ["Segoe", "UI", "Emoji"]
    @debug string.split("Segoe UI Emoji", " ", $limit: 1)  // ["Segoe", "UI Emoji"]
    ```

## to-upper-case

```scss
string.to-upper-case($string)
to-upper-case($string) //=> string
```

Возвращает копию строки `$string` с буквами [ASCII](https://ru.wikipedia.org/wiki/ASCII), преобразованными в верхний регистр.

=== "SCSS"

    ```scss
    @use "sass:string";

    @debug string.to-upper-case("Bold"); // "BOLD"
    @debug string.to-upper-case(sans-serif); // SANS-SERIF
    ```

=== "SASS"

    ```sass
    @use "sass:string"

    @debug string.to-upper-case("Bold")  // "BOLD"
    @debug string.to-upper-case(sans-serif)  // SANS-SERIF
    ```

## to-lower-case

```scss
string.to-lower-case($string)
to-lower-case($string) //=> string
```

Возвращает копию строки `$string` с буквами [ASCII](https://ru.wikipedia.org/wiki/ASCII), преобразованными в нижний регистр.

=== "SCSS"

    ```scss
    @use "sass:string";

    @debug string.to-lower-case("Bold"); // "bold"
    @debug string.to-lower-case(SANS-SERIF); // sans-serif
    ```

=== "SASS"

    ```sass
    @use "sass:string"

    @debug string.to-lower-case("Bold")  // "bold"
    @debug string.to-lower-case(SANS-SERIF)  // sans-serif
    ```

## unique-id

```scss
string.unique-id()
unique-id() //=> string
```

Возвращает случайно сгенерированную строку без кавычек, которая гарантированно является валидным CSS-идентификатором и уникальна в рамках текущей компиляции Sass.

=== "SCSS"

    ```scss
    @use "sass:string";

    @debug string.unique-id(); // uabtrnzug
    @debug string.unique-id(); // u6w1b1def
    ```

=== "SASS"

    ```sass
    @use "sass:string"

    @debug string.unique-id(); // uabtrnzug
    @debug string.unique-id(); // u6w1b1def
    ```

## unquote

```scss
string.unquote($string)
unquote($string) //=> string
```

Возвращает `$string` как строку без кавычек. Это может приводить к строкам, которые не являются валидным CSS, поэтому использовать следует с осторожностью.

=== "SCSS"

    ```scss
    @use "sass:string";

    @debug string.unquote("Helvetica"); // Helvetica
    @debug string.unquote(".widget:hover"); // .widget:hover
    ```

=== "SASS"

    ```sass
    @use "sass:string"

    @debug string.unquote("Helvetica")  // Helvetica
    @debug string.unquote(".widget:hover")  // .widget:hover
    ```
