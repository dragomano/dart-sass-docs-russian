---
title: CSS-функция if()
icon: lucide/newspaper
---

> Устаревшая функция `if()` в Sass заменяется официальным синтаксисом CSS-функции `if()`. Этот синтаксис позволяет свободно комбинировать условия Sass и CSS.

В 2010 году, вскоре после добавления [типа булевых значений](../values/booleans), Sass добавил глобальную функцию `if()` как способ легко использовать булевы значения в одном выражении без необходимости писать целое [правило `@if`](../at-rules/control/if). Эта функция имела сигнатуру `if($condition, $if-true, $if-false)` и возвращала `$if-true`, если `$condition` было [истинным](../at-rules/control/if#truthiness-and-falsiness), и `$if-false` в противном случае.

В то время браузеры даже не поддерживали `@media` запросы, и мы никогда не представляли, что CSS когда-нибудь будет поддерживать собственную функцию `if()`. Но пятнадцать лет спустя поддержка [CSS-функции `if()`](https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/Values/if) начала появляться в браузерах, и нам также пришлось это сделать, чтобы оставаться полностью совместимыми с CSS.

Sass теперь поддерживает [чистый CSS-синтаксис `if()`](../syntax/special-functions#if), а также специальное условие `sass(...)`, которое вычисляет выражения Sass. Чтобы избежать избыточности и стандартизировать наиболее совместимый с CSS вариант, мы планируем в конечном итоге удалить устаревшую функцию `if()` из языка.

Вы можете использовать [мигратор Sass](../cli/migrator/#if) для автоматической миграции с устаревшей функции `if()` на синтаксис CSS `if()`.

=== "SCSS"

    ```scss
    @use 'sass:meta';

    // Вместо if(true, 10px, 15px)
    @debug if(sass(true): 10px; else: 15px);

    // Вместо if(meta.variable-defined($var), $var, null)
    @debug if(sass(meta.variable-defined($var)): $var);
    ```

=== "SASS"

    ```sass
    @use 'sass:meta'

    // Вместо if(true, 10px, 15px)
    @debug if(sass(true): 10px; else: 15px)

    // Вместо if(meta.variable-defined($var), $var, null)
    @debug if(sass(meta.variable-defined($var)): $var)
    ```

--8<-- "includes/silence-warnings.md"
