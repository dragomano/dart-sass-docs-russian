---
title: Вычисления
icon: lucide/calculator
---

> Вычисления (calculations) — это способ, которым Sass представляет функцию `calc()`, а также похожие функции, такие как `clamp()`, `min()` и `max()`. Sass упрощает эти выражения настолько, насколько это возможно, даже когда они комбинируются друг с другом.

=== "SCSS"

    ```scss
    @debug calc(400px + 10%); // calc(400px + 10%)
    @debug calc(400px / 2); // 200px
    @debug min(100px, calc(1rem + 10%)); // min(100px, 1rem + 10%)
    ```

=== "SASS"

    ```sass
    @debug calc(400px + 10%) // calc(400px + 10%)
    @debug calc(400px / 2) // 200px
    @debug min(100px, calc(1rem + 10%)) // min(100px, 1rem + 10%)
    ```

Вычисления используют специальный синтаксис, отличный от обычного SassScript. Это тот же синтаксис, что и у CSS `calc()`, но с дополнительной возможностью использовать [переменные Sass](../variables) и вызывать [функции Sass](../modules). Это означает, что `/` всегда является оператором деления внутри вычисления!

!!! note "Примечание"

    Аргументы вызова функции Sass используют обычный синтаксис Sass, а не специальный синтаксис вычислений!

Вы также можете использовать [интерполяцию](../interpolation) в вычислении. Однако если вы это сделаете, никакие операции, включающие эту интерполяцию, не будут упрощены или проверены по типу, поэтому легко получить излишне многословный или даже недействительный CSS. Вместо `calc(10px + #{$var})` просто напишите `calc(10px + $var)`!

## Упрощение {#simplification}

Sass упростит смежные операции в вычислениях, если они используют единицы измерения, которые можно объединить на этапе компиляции, например `1in + 10px` или `5s * 2`. Если возможно, он даже упростит всё вычисление до одного числа — например, `clamp(0px, 30px, 20px)` вернёт `20px`.

!!! tip "Совет"

    Это означает, что выражение вычисления не обязательно всегда возвращает вычисление! Если вы пишете библиотеку Sass, вы всегда можете использовать функцию [`meta.type-of()`](../modules/meta#type-of), чтобы определить, с каким типом вы имеете дело.

Вычисления также будут упрощены внутри других вычислений. В частности, если `calc()` окажется внутри любого другого вычисления, вызов функции будет удалён и заменён на обычную операцию.

<div class="grid" markdown>

=== "SCSS"

    ```scss
    $width: calc(400px + 10%);

    .sidebar {
      width: $width;
      padding-left: calc($width / 4);
    }
    ```

=== "SASS"

    ```sass
    $width: calc(400px + 10%)

    .sidebar
      width: $width
      padding-left: calc($width / 4)
    ```

```css title="CSS"
.sidebar {
  width: calc(400px + 10%);
  padding-left: calc((400px + 10%) / 4);
}
```

</div>

## Операции {#operations}

Вы не можете использовать вычисления с обычными операциями SassScript, такими как `+` и `*`. Если вы хотите написать математические функции, допускающие вычисления, просто напишите их внутри собственных выражений `calc()` — если им передаются числа с совместимыми единицами измерения, они также вернут обычные числа, а если им передаются вычисления, они вернут вычисления.

Это ограничение введено для того, чтобы гарантировать: если вычисления *не* нужны, они вызовут ошибку как можно раньше. Вычисления нельзя использовать везде, где можно использовать обычные числа: их нельзя вставлять в идентификаторы CSS (например, `.item-#{$n}`), и их нельзя передавать встроенным [математическим функциям](../modules/math) Sass. Резервирование операций SassScript для обычных чисел чётко показывает, где вычисления разрешены, а где нет.

=== "SCSS"

    ```scss
    $width: calc(100% + 10px);
    @debug $width * 2; // Error!
    @debug calc($width * 2); // calc((100% + 10px) * 2);
    ```

=== "SASS"

    ```sass
    $width: calc(100% + 10px)
    @debug $width * 2 // Error!
    @debug calc($width * 2) // calc((100% + 10px) * 2)
    ```

## Константы {#constants}

Вычисления также могут содержать константы, которые записываются как идентификаторы CSS. Для совместимости с будущими спецификациями CSS разрешены *все* идентификаторы, и по умолчанию они просто обрабатываются как строки без кавычек, которые передаются как есть.

=== "SCSS"

    ```scss
    @debug calc(h + 30deg); // calc(h + 30deg);
    ```

=== "SASS"

    ```sass
    @debug calc(h + 30deg) // calc(h + 30deg)
    ```

Sass автоматически преобразует несколько специальных имён констант, указанных в CSS, в безразмерные числа:

* `pi` — сокращение для [математической константы *π*](https://ru.wikipedia.org/wiki/%D0%9F%D0%B8_(%D1%87%D0%B8%D1%81%D0%BB%D0%BE)).

* `e` — сокращение для [математической константы *e*](https://ru.wikipedia.org/wiki/E_(%D1%87%D0%B8%D1%81%D0%BB%D0%BE)).

* `infinity`, `-infinity` и `NaN` представляют соответствующие значения с плавающей точкой.

=== "SCSS"

    ```scss
    @use 'sass:math';

    @debug calc(pi); // 3.1415926536
    @debug calc(e);  // 2.7182818285
    @debug calc(infinity) > math.$max-number;  // true
    @debug calc(-infinity) < math.$min-number; // true
    ```

=== "SASS"

    ```sass
    @use 'sass:math'

    @debug calc(pi) // 3.1415926536
    @debug calc(e)  // 2.7182818285
    @debug calc(infinity) > math.$max-number  // true
    @debug calc(-infinity) < math.$min-number // true
    ```

## Функции вычислений {#calculation-functions}

Sass воспринимает следующие функции как [вычисления](https://www.w3.org/TR/css-values-4/#math):

* Функции сравнения: [`min()`](https://developer.mozilla.org/en-US/docs/Web/CSS/min), [`max()`](https://developer.mozilla.org/en-US/docs/Web/CSS/max) и [`clamp()`](https://developer.mozilla.org/en-US/docs/Web/CSS/clamp).
* Функции ступенчатых значений: [`round()`](https://developer.mozilla.org/en-US/docs/Web/CSS/round), [`mod()`](https://developer.mozilla.org/en-US/docs/Web/CSS/mod) и [`rem()`](https://developer.mozilla.org/en-US/docs/Web/CSS/rem).
* Тригонометрические функции: [`sin()`](https://developer.mozilla.org/en-US/docs/Web/CSS/sin), [`cos()`](https://developer.mozilla.org/en-US/docs/Web/CSS/cos), [`tan()`](https://developer.mozilla.org/en-US/docs/Web/CSS/tan), [`asin()`](https://developer.mozilla.org/en-US/docs/Web/CSS/asin), [`acos()`](https://developer.mozilla.org/en-US/docs/Web/CSS/acos), [`atan()`](https://developer.mozilla.org/en-US/docs/Web/CSS/atan) и [`atan2()`](https://developer.mozilla.org/en-US/docs/Web/CSS/atan2).
* Показательные функции: [`pow()`](https://developer.mozilla.org/en-US/docs/Web/CSS/pow), [`sqrt()`](https://developer.mozilla.org/en-US/docs/Web/CSS/sqrt), [`hypot()`](https://developer.mozilla.org/en-US/docs/Web/CSS/hypot), [`log()`](https://developer.mozilla.org/en-US/docs/Web/CSS/log) и [`exp()`](https://developer.mozilla.org/en-US/docs/Web/CSS/exp).
* Функции, связанные со знаком числа: [`abs()`](https://developer.mozilla.org/en-US/docs/Web/CSS/abs) и [`sign()`](https://developer.mozilla.org/en-US/docs/Web/CSS/sign).

!!! note "Примечание"

    Если вы определили [функцию Sass](../at-rules/function) с тем же именем, что и функция вычисления, Sass всегда вызовет вашу функцию, а не создаст вычислительное значение.

### Устаревшие глобальные функции {#legacy-global-functions}

CSS добавил поддержку [математических выражений](https://www.w3.org/TR/css-values-4/#math) в Values and Units Level 4. Однако Sass поддерживал свои собственные функции [`round()`](../modules/math#round), [`abs()`](../modules/math#abs), [`min()`](../modules/math#min) и [`max()`](../modules/math#max) задолго до этого, и ему нужно было быть обратно совместимым со всеми существующими таблицами стилей. Это привело к необходимости особой синтаксической изобретательности.

Если вызов `round()`, `abs()`, `min()` или `max()` является допустимым выражением вычисления, он будет разобран как вычисление. Но как только любая часть вызова содержит функцию SassScript, которая не поддерживается в вычислениях, например [оператор деления по модулю](../operators/numeric/), он разбирается как вызов соответствующей математической функции Sass.

Поскольку вычисления по возможности упрощаются до чисел, единственное существенное различие заключается в том, что функции Sass поддерживают только единицы измерения, которые можно комбинировать на этапе сборки, поэтому `min(12px % 10, 10%)` выдаст ошибку.

!!! note "Примечание"

    Другие вычисления не допускают сложение, вычитание или сравнение безразмерных чисел с числами с единицами измерения. Однако [`min()`](https://developer.mozilla.org/en-US/docs/Web/CSS/min), [`max()`](https://developer.mozilla.org/en-US/docs/Web/CSS/max), [`abs()`](https://developer.mozilla.org/en-US/docs/Web/CSS/abs) и [одноаргументная `round()`](https://developer.mozilla.org/en-US/docs/Web/CSS/round) отличаются: для обратной совместимости с глобальными устаревшими функциями Sass, которые исторически допускают смешивание единиц измерения с безразмерными значениями, эти единицы можно смешивать, если они содержатся непосредственно внутри вычисления `min()`, `max()`, `abs()` или одноаргументной `round()`.

    Например, `min(5 + 10px, 20px)` даст результат `15px`. Однако `sqrt(5 + 10px)` выдаст ошибку, так как `sqrt(5 + 10px)` никогда не была глобальной функцией Sass, и это несовместимые единицы измерения.

#### `min()` и `max()` {#min-and-max}

<div class="grid" markdown>

=== "SCSS"

    ```scss
    $padding: 12px;

    .post {
      // Поскольку эти вызовы max() являются допустимыми выражениями
      // вычислений, они разбираются как вычисления.
      padding-left: max($padding, env(safe-area-inset-left));
      padding-right: max($padding, env(safe-area-inset-right));
    }

    .sidebar {
      // Поскольку они используют оператор деления по модулю, доступный
      // только в SassScript, они разбираются как вызовы функций SassScript.
      padding-left: max($padding % 10, 20px);
      padding-right: max($padding % 10, 20px);
    }
    ```

=== "SASS"

    ```sass
    $padding: 12px

    .post
      // Поскольку эти вызовы max() являются допустимыми выражениями
      // вычислений, они разбираются как вычисления.
      padding-left: max($padding, env(safe-area-inset-left))
      padding-right: max($padding, env(safe-area-inset-right))


    .sidebar
      // Поскольку они используют оператор деления по модулю, доступный
      // только в SassScript, они разбираются как вызовы функций SassScript.
      padding-left: max($padding % 10, 20px)
      padding-right: max($padding % 10, 20px)
    ```

```css title="CSS"
.post {
  padding-left: max(12px, env(safe-area-inset-left));
  padding-right: max(12px, env(safe-area-inset-right));
}

.sidebar {
  padding-left: 20px;
  padding-right: 20px;
}
```

</div>

#### `round()`

Функция [`round(<strategy>, number, step)`](https://developer.mozilla.org/en-US/docs/Web/CSS/round#parameter) принимает необязательную стратегию округления, значение для округления и интервал округления `step`. `strategy` может быть `nearest`, `up`, `down` или `to-zero`.

<div class="grid" markdown>

=== "SCSS"

    ```scss
    $number: 12.5px;
    $step: 15px;

    .post-image {
      // Поскольку эти вызовы round() являются допустимыми
      // выражениями вычислений, они разбираются как вычисления.
      padding-left: round(nearest, $number, $step);
      padding-right: round($number + 10px);
      padding-bottom: round($number + 10px, $step + 10%);
    }
    ```

=== "SASS"

    ```sass
    $number: 12.5px
    $step: 15px

    .post-image
      // Поскольку эти вызовы round() являются допустимыми
      // выражениями вычислений, они разбираются как вычисления.
      padding-left: round(nearest, $number, $step)
      padding-right: round($number + 10px)
      padding-bottom: round($number + 10px, $step + 10%)
    ```

```css title="CSS"
.post-image {
  padding-left: 15px;
  padding-right: 23px;
  padding-bottom: round(22.5px, 15px + 10%);
}
```

</div>

#### `abs()`

!!! note "Примечание"

    Совместимость глобальной функции `abs()` с [параметрами в единицах % устарела](../breaking-changes/abs-percent/). В будущем это будет генерировать CSS-функцию abs() для разрешения браузером.

Функция [`abs(value)`](https://developer.mozilla.org/en-US/docs/Web/CSS/abs) принимает одно выражение в качестве параметра и возвращает абсолютное значение `$value`. Если `$value` отрицательное, возвращает `-$value`, а если `$value` положительное, возвращает `$value` как есть.

<div class="grid" markdown>

=== "SCSS"

    ```scss
    .post-image {
      // Поскольку эти вызовы abs() являются допустимыми
      // выражениями вычислений, они разбираются как вычисления.
      padding-left: abs(10px);
      padding-right: math.abs(-7.5%);
      padding-top: abs(1 + 1px);
    }
    ```

=== "SASS"

    ```sass
    .post-image
      // Поскольку эти вызовы abs() являются допустимыми
      // выражениями вычислений, они разбираются как вычисления.
      padding-left: abs(-10px)
      padding-right: math.abs(-7.5%)
      padding-top: abs(1 + 1px)
    ```

```css title="CSS"
.post-image {
  padding-left: 10px;
  padding-right: 7.5%;
  padding-top: 2px;
}
```

</div>
