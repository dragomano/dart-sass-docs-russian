---
title: "sass:selector"
icon: lucide/square-dashed-mouse-pointer
---

## Значения селекторов {#selector-values}

Функции этого модуля анализируют и изменяют селекторы. Возвращаемый селектор — это список, разделённый запятыми [../values/lists] (список селекторов), содержащий списки, разделённые пробелами (сложные селекторы), со [строками без кавычек](../values/strings#unquoted) внутри (комбинированные селекторы). Например, селектор `.main aside:hover, .sidebar p` будет возвращён как:

```scss
@debug ((unquote(".main") unquote("aside:hover")),
        (unquote(".sidebar") unquote("p")));
// .main aside:hover, .sidebar p
```

Аргументы-селекторы этих функций могут быть в таком же формате, но также могут быть обычными строками (в кавычках или без), или их комбинацией. Например, `".main aside:hover, .sidebar p"` — допустимый аргумент-селектор.

### is-superselector

```scss
selector.is-superselector($super, $sub)
is-superselector($super, $sub) //=> булево значение
```

Возвращает, соответствует ли селектор `$super` всем элементам, которые соответствует селектор `$sub`.

Возвращает `true`, даже если `$super` соответствует *больше* элементов, чем `$sub`.

Селекторы `$super` и `$sub` могут содержать [селекторы-заполнители](../style-rules/placeholder-selectors), но не [родительские селекторы](../style-rules/parent-selector).

=== "SCSS"

    ```scss
    @use "sass:selector";

    @debug selector.is-superselector("a", "a.disabled"); // true
    @debug selector.is-superselector("a.disabled", "a"); // false
    @debug selector.is-superselector("a", "sidebar a"); // true
    @debug selector.is-superselector("sidebar a", "a"); // false
    @debug selector.is-superselector("a", "a"); // true
    ```

=== "SASS"

    ```sass
    @use "sass:selector"

    @debug selector.is-superselector("a", "a.disabled") // true
    @debug selector.is-superselector("a.disabled", "a") // false
    @debug selector.is-superselector("a", "sidebar a") // true
    @debug selector.is-superselector("sidebar a", "a") // false
    @debug selector.is-superselector("a", "a") // true
    ```

### append

```scss
selector.append($selectors...)
selector-append($selectors...) //=> селектор
```

Объединяет `$selectors` без [комбинаторов потомков](https://developer.mozilla.org/en-US/docs/Web/CSS/Descendant_selectors) — то есть без пробелов между ними.

Если любой селектор в `$selectors` является списком селекторов, каждый сложный селектор объединяется отдельно.

`$selectors` могут содержать [селекторы-заполнители](../style-rules/placeholder-selectors), но не [родительские селекторы](../style-rules/parent-selector).

См. также [`selector.nest()`](#nest).

=== "SCSS"

    ```scss
    @use "sass:selector";

    @debug selector.append("a", ".disabled"); // a.disabled
    @debug selector.append(".accordion", "__copy"); // .accordion__copy
    @debug selector.append(".accordion", "__copy, __image");
    // .accordion__copy, .accordion__image
    ```

=== "SASS"

    ```sass
    @use "sass:selector"

    @debug selector.append("a", ".disabled") // a.disabled
    @debug selector.append(".accordion", "__copy") // .accordion__copy
    @debug selector.append(".accordion", "__copy, __image")
    // .accordion__copy, .accordion__image
    ```

### extend

```scss
selector.extend($selector, $extendee, $extender)
selector-extend($selector, $extendee, $extender) //=> селектор
```

Расширяет `$selector` так же, как правило [`@extend`](../at-rules/extend).

Возвращает копию `$selector`, изменённую следующим правилом `@extend`:

```scss
#{$extender} {
  @extend #{$extendee};
}
```

Иными словами, заменяет все вхождения `$extendee` в `$selector` на `$extendee, $extender`. Если `$extendee` отсутствует в `$selector`, возвращает его без изменений.

Селекторы `$selector`, `$extendee` и `$extender` могут содержать [селекторы-заполнители](../style-rules/placeholder-selectors), но не [родительские селекторы](../style-rules/parent-selector).

См. также [`selector.replace()`](#replace).

=== "SCSS"

    ```scss
    @use "sass:selector";

    @debug selector.extend("a.disabled", "a", ".link"); // a.disabled, .link.disabled
    @debug selector.extend("a.disabled", "h1", "h2"); // a.disabled
    @debug selector.extend(".guide .info", ".info", ".content nav.sidebar");
    // .guide .info, .guide .content nav.sidebar, .content .guide nav.sidebar
    ```

=== "SASS"

    ```sass
    @use "sass:selector"

    @debug selector.extend("a.disabled", "a", ".link") // a.disabled, .link.disabled
    @debug selector.extend("a.disabled", "h1", "h2") // a.disabled
    @debug selector.extend(".guide .info", ".info", ".content nav.sidebar")
    // .guide .info, .guide .content nav.sidebar, .content .guide nav.sidebar
    ```

### nest

```scss
selector.nest($selectors...)
selector-nest($selectors...) //=> селектор
```

Объединяет `$selectors` так, как если бы они были вложены друг в друга в таблице стилей.

`$selectors` могут содержать [селекторы-заполнители](../style-rules/placeholder-selectors). В отличие от других функций селекторов, все они, кроме первого, также могут содержать [родительские селекторы](../style-rules/parent-selector).

См. также [`selector.append()`](#append).

=== "SCSS"

    ```scss
    @use "sass:selector";

    @debug selector.nest("ul", "li"); // ul li
    @debug selector.nest(".alert, .warning", "p"); // .alert p, .warning p
    @debug selector.nest(".alert", "&:hover"); // .alert:hover
    @debug selector.nest(".accordion", "&__copy"); // .accordion__copy
    ```

=== "SASS"

    ```sass
    @use "sass:selector"

    @debug selector.nest("ul", "li") // ul li
    @debug selector.nest(".alert, .warning", "p") // .alert p, .warning p
    @debug selector.nest(".alert", "&:hover") // .alert:hover
    @debug selector.nest(".accordion", "&__copy") // .accordion__copy
    ```

### parse

```scss
selector.parse($selector)
selector-parse($selector) //=> селектор
```

Возвращает `$selector` в формате [значения селектора](#selector-values).

=== "SCSS"

    ```scss
    @use "sass:selector";

    @debug selector.parse(".main aside:hover, .sidebar p");
    // ((unquote(".main") unquote("aside:hover")),
    //  (unquote(".sidebar") unquote("p")))
    ```

=== "SASS"

    ```sass
    @use "sass:selector"

    @debug selector.parse(".main aside:hover, .sidebar p")
    // ((unquote(".main") unquote("aside:hover")),
    //  (unquote(".sidebar") unquote("p")))
    ```

### replace

```scss
selector.replace($selector, $original, $replacement)
selector-replace($selector, $original, $replacement) //=> селектор
```

Возвращает копию `$selector` с заменой всех вхождений `$original` на `$replacement`.

Использует [умное объединение](../at-rules/extend#how-it-works) правила [`@extend`](../at-rules/extend), чтобы `$replacement` безупречно интегрировался в `$selector`. Если `$original` отсутствует в `$selector`, возвращает его без изменений.

Селекторы `$selector`, `$original` и `$replacement` могут содержать [селекторы-заполнители](../style-rules/placeholder-selectors), но не [родительские селекторы](../style-rules/parent-selector).

См. также [`selector.extend()`](#extend).

=== "SCSS"

    ```scss
    @use "sass:selector";

    @debug selector.replace("a.disabled", "a", ".link"); // .link.disabled
    @debug selector.replace("a.disabled", "h1", "h2"); // a.disabled
    @debug selector.replace(".guide .info", ".info", ".content nav.sidebar");
    // .guide .content nav.sidebar, .content .guide nav.sidebar
    ```

=== "SASS"

    ```sass
    @use "sass:selector"

    @debug selector.replace("a.disabled", "a", ".link")  // .link.disabled
    @debug selector.replace("a.disabled", "h1", "h2")  // a.disabled
    @debug selector.replace(".guide .info", ".info", ".content nav.sidebar")
    // .guide .content nav.sidebar, .content .guide nav.sidebar
    ```

### unify

```scss
selector.unify($selector1, $selector2)
selector-unify($selector1, $selector2) //=> селектор | null
```

Возвращает селектор, который соответствует только элементам, соответствующим *обоим* `$selector1` и `$selector2`.

Возвращает `null`, если `$selector1` и `$selector2` не соответствуют одним и тем же элементам или если нет селектора, выражающего их пересечение.

Как и селекторы, генерируемые правилом [`@extend`](../at-rules/extend#html-heuristics), возвращаемый селектор не гарантирует соответствие *всем* элементам, соответствующим обоим `$selector1` и `$selector2`, если это сложные селекторы.

=== "SCSS"

    ```scss
    @use "sass:selector";

    @debug selector.unify("a", ".disabled"); // a.disabled
    @debug selector.unify("a.disabled", "a.outgoing"); // a.disabled.outgoing
    @debug selector.unify("a", "h1"); // null
    @debug selector.unify(".warning a", "main a"); // .warning main a, main .warning a
    ```

=== "SASS"

    ```sass
    @use "sass:selector"

    @debug selector.unify("a", ".disabled") // a.disabled
    @debug selector.unify("a.disabled", "a.outgoing") // a.disabled.outgoing
    @debug selector.unify("a", "h1") // null
    @debug selector.unify(".warning a", "main a") // .warning main a, main .warning a
    ```

### simple-selectors

```scss
selector.simple-selectors($selector)
simple-selectors($selector) //=> список
```

Возвращает список простых селекторов в `$selector`.

`$selector` должен быть одной строкой, содержащей комбинированный селектор. Это означает, что он не может содержать комбинаторы (включая пробелы) или запятые.

Возвращаемый список разделён запятыми, а простые селекторы представлены строками без кавычек.

=== "SCSS"

    ```scss
    @use "sass:selector";

    @debug selector.simple-selectors("a.disabled"); // a, .disabled
    @debug selector.simple-selectors("main.blog:after"); // main, .blog, :after
    ```

=== "SASS"

    ```sass
    @use "sass:selector"

    @debug selector.simple-selectors("a.disabled") // a, .disabled
    @debug selector.simple-selectors("main.blog:after") // main, .blog, :after
    ```
