---
title: "sass:math"
icon: lucide/calculator
---

## Переменные {#variables}

### $e

```scss
math.$e
```

Ближайшее приближение [математической константы *e*](https://ru.wikipedia.org/wiki/E_(%D1%81%D0%B8%D1%81%D0%BB%D0%BE)) с помощью 64-битного числа с плавающей точкой.

=== "SCSS"

    ```scss
    @use 'sass:math';

    @debug math.$e; // 2.7182818285
    ```

=== "SASS"

    ```sass
    @use 'sass:math'

    @debug math.$e // 2.7182818285
    ```

### $epsilon

```scss
math.$epsilon
```

Разность между 1 и наименьшим числом с плавающей точкой в формате 64 бита, которое больше 1 согласно правилам сравнения чисел с плавающей точкой. Из‑за [10 знаков точности](../values/numbers) чисел Sass во многих случаях это будет выглядеть как 0.

### $max-number

```scss
math.$max-number
```

Максимальное конечное число, которое может быть представлено как 64-битное число с плавающей точкой.

=== "SCSS"

    ```scss
    @use 'sass:math';

    @debug math.$max-number; // 179769313486231570000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000
    ```

=== "SASS"

    ```sass
    @use 'sass:math'

    @debug math.$max-number // 179769313486231570000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000
    ```

### $max-safe-integer

```scss
math.$max-safe-integer
```

Максимальное целое число `n` такое, что и `n`, и `n + 1` могут быть точно представлены как 64-битные числа с плавающей точкой.

=== "SCSS"

    ```scss
    @use 'sass:math';

    @debug math.$max-safe-integer; // 9007199254740991
    ```

=== "SASS"

    ```sass
    @use 'sass:math'

    @debug math.$max-safe-integer // 9007199254740991
    ```

### $min-number

```scss
math.$min-number
```

Наименьшее положительное число, которое может быть представлено как 64‑битное число с плавающей точкой. Из‑за [10 знаков точности](../values/numbers) чисел Sass во многих случаях это будет выглядеть как 0.

### $min-safe-integer

```scss
math.$min-safe-integer
```

Минимальное целое число `n` такое, что и `n`, и `n - 1` могут быть точно представлены как 64-битные числа с плавающей точкой.

=== "SCSS"

    ```scss
    @use 'sass:math';

    @debug math.$min-safe-integer; // -9007199254740991
    ```

=== "SASS"

    ```sass
    @use 'sass:math'

    @debug math.$min-safe-integer // -9007199254740991
    ```

### $pi

```scss
math.$pi
```

Ближайшее приближение [математической константы *π*](https://ru.wikipedia.org/wiki/%D0%9F%D0%B8_(%D1%87%D0%B8%D1%81%D0%BB%D0%BE)) с помощью 64-битного числа с плавающей точкой.

=== "SCSS"

    ```scss
    @use 'sass:math';

    @debug math.$pi; // 3.1415926536
    ```

=== "SASS"

    ```sass
    @use 'sass:math'

    @debug math.$pi // 3.1415926536
    ```

## Функции округления {#bounding-functions}

### ceil

```scss
math.ceil($number)
ceil($number) //=> число
```

Округляет `$number` вверх до следующего наибольшего целого числа.

=== "SCSS"

    ```scss
    @use 'sass:math';

    @debug math.ceil(4); // 4
    @debug math.ceil(4.2); // 5
    @debug math.ceil(4.9); // 5
    ```

=== "SASS"

    ```sass
    @use 'sass:math'

    @debug math.ceil(4) // 4
    @debug math.ceil(4.2) // 5
    @debug math.ceil(4.9) // 5
    ```

### clamp

```scss
math.clamp($min, $number, $max) //=> число
```

Ограничивает `$number` диапазоном между `$min` и `$max`. Если `$number` меньше `$min`, возвращается `$min`, а если больше `$max`, возвращается `$max`.

`$min`, `$number` и `$max` должны иметь совместимые единицы измерения или все должны быть безразмерными.

=== "SCSS"

    ```scss
    @use 'sass:math';

    @debug math.clamp(-1, 0, 1); // 0
    @debug math.clamp(1px, -1px, 10px); // 1px
    @debug math.clamp(-1in, 1cm, 10mm); // 10mm
    ```

=== "SASS"

    ```sass
    @use 'sass:math'

    @debug math.clamp(-1, 0, 1) // 0
    @debug math.clamp(1px, -1px, 10px) // 1px
    @debug math.clamp(-1in, 1cm, 10mm) // 10mm
    ```

### floor

```scss
math.floor($number)
floor($number) //=> число
```

Округляет `$number` вниз до следующего наименьшего целого числа.

=== "SCSS"

    ```scss
    @use 'sass:math';

    @debug math.floor(4); // 4
    @debug math.floor(4.2); // 4
    @debug math.floor(4.9); // 4
    ```

=== "SASS"

    ```sass
    @use 'sass:math'

    @debug math.floor(4) // 4
    @debug math.floor(4.2) // 4
    @debug math.floor(4.9) // 4
    ```

### max

```scss
math.max($number...)
max($number...) //=> число
```

Возвращает наибольшее из одного или нескольких чисел.

=== "SCSS"

    ```scss
    @use 'sass:math';

    @debug math.max(1px, 4px); // 4px

    $widths: 50px, 30px, 100px;
    @debug math.max($widths...); // 100px
    ```

=== "SASS"

    ```sass
    @use 'sass:math'

    @debug math.max(1px, 4px) // 4px

    $widths: 50px, 30px, 100px
    @debug math.max($widths...) // 100px
    ```

### min

```scss
math.min($number...)
min($number...) //=> число
```

Возвращает наименьшее из одного или нескольких чисел.

=== "SCSS"

    ```scss
    @use 'sass:math';

    @debug math.min(1px, 4px); // 1px

    $widths: 50px, 30px, 100px;
    @debug math.min($widths...); // 30px
    ```

=== "SASS"

    ```sass
    @use 'sass:math'

    @debug math.min(1px, 4px) // 1px

    $widths: 50px, 30px, 100px
    @debug math.min($widths...) // 30px
    ```

### round

```scss
math.round($number)
round($number) //=> число
```

Округляет `$number` до ближайшего целого числа.

=== "SCSS"

    ```scss
    @use 'sass:math';

    @debug math.round(4); // 4
    @debug math.round(4.2); // 4
    @debug math.round(4.9); // 5
    ```

=== "SASS"

    ```sass
    @use 'sass:math'

    @debug math.round(4) // 4
    @debug math.round(4.2) // 4
    @debug math.round(4.9) // 5
    ```

## Функции расстояния {#distance-functions}

Возвращает [абсолютную величину](https://ru.wikipedia.org/wiki/%D0%90%D0%B1%D1%81%D0%BE%D0%BB%D1%8E%D1%82%D0%BD%D0%B0%D1%8F_%D0%B2%D0%B5%D0%BB%D0%B8%D1%87%D0%B8%D0%BD%D0%B0) числа `$number`. Если `$number` отрицательное, возвращается `-$number`, а если `$number` положительное, возвращается `$number` без изменений.

### abs

```scss
math.abs($number)
abs($number) //=> число
```

=== "SCSS"

    ```scss
    @use 'sass:math';

    @debug math.abs(10px); // 10px
    @debug math.abs(-10px); // 10px
    ```

=== "SASS"

    ```sass
    @use 'sass:math'

    @debug math.abs(10px) // 10px
    @debug math.abs(-10px) // 10px
    ```

### hypot

```scss
math.hypot($number...) //=> число
```

Возвращает длину *n*-мерного [вектора](https://ru.wikipedia.org/wiki/%D0%92%D0%B5%D0%BA%D1%82%D0%BE%D1%80_(%D0%B3%D0%B5%D0%BE%D0%BC%D0%B5%D1%82%D1%80%D0%B8%D1%8F)), компоненты которого равны каждому из чисел `$number`. Например, для трёх чисел *a*, *b* и *c* возвращается квадратный корень из *a² + b² + c²*.

Числа должны либо все иметь совместимые единицы измерения, либо все быть безразмерными. Поскольку единицы измерения чисел могут различаться, результат принимает единицу измерения первого числа.

=== "SCSS"

    ```scss
    @use 'sass:math';

    @debug math.hypot(3, 4); // 5

    $lengths: 1in, 10cm, 50px;
    @debug math.hypot($lengths...); // 4.0952775683in
    ```

=== "SASS"

    ```sass
    @use 'sass:math'

    @debug math.hypot(3, 4) // 5

    $lengths: 1in, 10cm, 50px
    @debug math.hypot($lengths...) // 4.0952775683in
    ```

## Экспоненциальные функции {#exponential-functions}

### log

```scss
math.log($number, $base: null) //=> число
```

Возвращает [логарифм](https://ru.wikipedia.org/wiki/%D0%9B%D0%BE%D0%B3%D0%B0%D1%80%D0%B8%D1%84%D0%BC) числа `$number` по основанию `$base`. Если `$base` — `null`, вычисляется [натуральный логарифм](https://ru.wikipedia.org/wiki/%D0%9D%D0%B0%D1%82%D1%83%D1%80%D0%B0%D0%BB%D1%8C%D0%BD%D1%8B%D0%B9_%D0%BB%D0%BE%D0%B3%D0%B0%D1%80%D0%B8%D1%84%D0%BC`).

`$number` и `$base` должны быть безразмерными.

=== "SCSS"

    ```scss
    @use 'sass:math';

    @debug math.log(10); // 2.302585093
    @debug math.log(10, 10); // 1
    ```

=== "SASS"

    ```sass
    @use 'sass:math'

    @debug math.log(10) // 2.302585093
    @debug math.log(10, 10) // 1
    ```

### pow

```scss
math.pow($base, $exponent) //=> число
```

Возводит `$base` [в степень](https://ru.wikipedia.org/wiki/%D0%92%D0%BE%D0%B7%D0%B2%D0%B5%D0%B4%D0%B5%D0%BD%D0%B8%D0%B5_%D0%B2_%D1%81%D1%82%D0%B5%D0%BF%D0%B5%D0%BD%D1%8C) `$exponent`.

`$base` и `$exponent` должны быть безразмерными.

=== "SCSS"

    ```scss
    @use 'sass:math';

    @debug math.pow(10, 2); // 100
    @debug math.pow(100, math.div(1, 3)); // 4.6415888336
    @debug math.pow(5, -2); // 0.04
    ```

=== "SASS"

    ```sass
    @use 'sass:math'

    @debug math.pow(10, 2) // 100
    @debug math.pow(100, math.div(1, 3)) // 4.6415888336
    @debug math.pow(5, -2) // 0.04
    ```

### sqrt

```scss
math.sqrt($number) //=> число
```

Возвращает [квадратный корень](https://ru.wikipedia.org/wiki/%D0%9A%D0%B2%D0%B0%D0%B4%D1%80%D0%B0%D1%82%D0%BD%D1%8B%D0%B9_%D0%BA%D0%BE%D1%80%D0%B5%D0%BD%D1%8C) числа `$number`.

`$number` должен быть безразмерным.

=== "SCSS"

    ```scss
    @use 'sass:math';

    @debug math.sqrt(100); // 10
    @debug math.sqrt(math.div(1, 3)); // 0.5773502692
    @debug math.sqrt(-1); // NaN
    ```

=== "SASS"

    ```sass
    @use 'sass:math'

    @debug math.sqrt(100) // 10
    @debug math.sqrt(math.div(1, 3)) // 0.5773502692
    @debug math.sqrt(-1) // NaN
    ```

## Тригонометрические функции {#trigonometric-functions}

### cos

```scss
math.cos($number) //=> число
```

Возвращает [косинус](https://ru.wikipedia.org/wiki/%D0%A2%D1%80%D0%B8%D0%B3%D0%BE%D0%BD%D0%BE%D0%BC%D0%B5%D1%82%D1%80%D0%B8%D1%87%D0%B5%D1%81%D0%BA%D0%B8%D0%B5_%D1%84%D1%83%D0%BD%D0%BA%D1%86%D0%B8%D0%B8) числа `$number`.

`$number` должен быть углом (его единицы должны быть совместимы с `deg`) или безразмерным. Если у `$number` нет единиц, предполагается, что он задан в `rad`.

=== "SCSS"

    ```scss
    @use 'sass:math';

    @debug math.cos(100deg); // -0.1736481777
    @debug math.cos(1rad); // 0.5403023059
    @debug math.cos(1); // 0.5403023059
    ```

=== "SASS"

    ```sass
    @use 'sass:math'

    @debug math.cos(100deg) // -0.1736481777
    @debug math.cos(1rad) // 0.5403023059
    @debug math.cos(1) // 0.5403023059
    ```

### sin

```scss
math.sin($number) //=> число
```

Возвращает [синус](https://ru.wikipedia.org/wiki/%D0%A2%D1%80%D0%B8%D0%B3%D0%BE%D0%BD%D0%BE%D0%BC%D0%B5%D1%82%D1%80%D0%B8%D1%87%D0%B5%D1%81%D0%BA%D0%B8%D0%B5_%D1%84%D1%83%D0%BD%D0%BA%D1%86%D0%B8%D0%B8) числа `$number`.

`$number` должен быть углом (его единицы должны быть совместимы с `deg`) или безразмерным. Если у `$number` нет единиц, предполагается, что он задан в `rad`.

=== "SCSS"

    ```scss
    @use 'sass:math';

    @debug math.sin(100deg); // 0.984807753
    @debug math.sin(1rad); // 0.8414709848
    @debug math.sin(1); // 0.8414709848
    ```

=== "SASS"

    ```sass
    @use 'sass:math'

    @debug math.sin(100deg) // 0.984807753
    @debug math.sin(1rad) // 0.8414709848
    @debug math.sin(1) // 0.8414709848
    ```

### tan

```scss
math.tan($number) //=> число
```

Возвращает [тангенс](https://ru.wikipedia.org/wiki/%D0%A2%D1%80%D0%B8%D0%B3%D0%BE%D0%BD%D0%BE%D0%BC%D0%B5%D1%82%D1%80%D0%B8%D1%87%D0%B5%D1%81%D0%BA%D0%B8%D0%B5_%D1%84%D1%83%D0%BD%D0%BA%D1%86%D0%B8%D0%B8) числа `$number`.

`$number` должен быть углом (его единицы должны быть совместимы с `deg`) или безразмерным. Если у `$number` нет единиц, предполагается, что он задан в `rad`.

=== "SCSS"

    ```scss
    @use 'sass:math';

    @debug math.tan(100deg); // -5.6712818196
    @debug math.tan(1rad); // 1.5574077247
    @debug math.tan(1); // 1.5574077247
    ```

=== "SASS"

    ```sass
    @use 'sass:math'

    @debug math.tan(100deg) // -5.6712818196
    @debug math.tan(1rad) // 1.5574077247
    @debug math.tan(1) // 1.5574077247
    ```

### acos

```scss
math.acos($number) //=> число
```

Возвращает [арккосинус](https://ru.wikipedia.org/wiki/%D0%9E%D0%B1%D1%80%D0%B0%D1%82%D0%BD%D1%8B%D0%B5_%D1%82%D1%80%D0%B8%D0%B3%D0%BE%D0%BD%D0%BE%D0%BC%D0%B5%D1%82%D1%80%D0%B8%D1%87%D0%B5%D1%81%D0%BA%D0%B8%D0%B5_%D1%84%D1%83%D0%BD%D0%BA%D1%86%D0%B8%D0%B8) числа `$number` в градусах (`deg`).

`$number` должен быть безразмерной величиной.

=== "SCSS"

    ```scss
    @use 'sass:math';

    @debug math.acos(0.5); // 60deg
    @debug math.acos(2); // NaNdeg
    ```

=== "SASS"

    ```sass
    @use 'sass:math'

    @debug math.acos(0.5) // 60deg
    @debug math.acos(2) // NaNdeg
    ```

### asin

```scss
math.asin($number) //=> число
```

Возвращает [арксинус](https://ru.wikipedia.org/wiki/%D0%9E%D0%B1%D1%80%D0%B0%D1%82%D0%BD%D1%8B%D0%B5_%D1%82%D1%80%D0%B8%D0%B3%D0%BE%D0%BD%D0%BE%D0%BC%D0%B5%D1%82%D1%80%D0%B8%D1%87%D0%B5%D1%81%D0%BA%D0%B8%D0%B5_%D1%84%D1%83%D0%BD%D0%BA%D1%86%D0%B8%D0%B8) числа `$number` в градусах (`deg`).

`$number` должен быть безразмерной величиной.

=== "SCSS"

    ```scss
    @use 'sass:math';

    @debug math.asin(0.5); // 30deg
    @debug math.asin(2); // NaNdeg
    ```

=== "SASS"

    ```sass
    @use 'sass:math'

    @debug math.asin(0.5) // 30deg
    @debug math.asin(2) // NaNdeg
    ```

### atan

```scss
math.atan($number) //=> число
```

Возвращает [арктангенс](https://ru.wikipedia.org/wiki/%D0%9E%D0%B1%D1%80%D0%B0%D1%82%D0%BD%D1%8B%D0%B5_%D1%82%D1%80%D0%B8%D0%B3%D0%BE%D0%BD%D0%BE%D0%BC%D0%B5%D1%82%D1%80%D0%B8%D1%87%D0%B5%D1%81%D0%BA%D0%B8%D0%B5_%D1%84%D1%83%D0%BD%D0%BA%D1%86%D0%B8%D0%B8) числа `$number` в градусах (`deg`).

`$number` должен быть безразмерной величиной.

=== "SCSS"

    ```scss
    @use 'sass:math';

    @debug math.atan(10); // 84.2894068625deg
    ```

=== "SASS"

    ```sass
    @use 'sass:math'

    @debug math.atan(10) // 84.2894068625deg
    ```

### atan2

```scss
math.atan2($y, $x) //=> число
```

Возвращает [двухаргументный арктангенс](https://en.wikipedia.org/wiki/Atan2) `$y` и `$x` в градусах (`deg`).

`$y` и `$x` должны иметь совместимые единицы или быть безразмерными.

!!! note "Примечание"

    `math.atan2($y, $x)` отличается от `atan(math.div($y, $x))`, поскольку сохраняет четверть точки. Например, `math.atan2(1, -1)` соответствует точке `(-1, 1)` и возвращает `135deg`. В отличие от него, `math.atan(math.div(1, -1))` и `math.atan(math.div(-1, 1))` сначала сводятся к `atan(-1)`, поэтому оба возвращают `-45deg`.

=== "SCSS"

    ```scss
    @use 'sass:math';

    @debug math.atan2(-1, 1); // 135deg
    ```

=== "SASS"

    ```sass
    @use 'sass:math'

    @debug math.atan2(-1, 1) // 135deg
    ```

## Функции единиц {#unit-functions}

### compatible

```scss
math.compatible($number1, $number2)
comparable($number1, $number2) //=> булево значение
```

Если функция возвращает `true`, `$number1` и `$number2` можно безопасно [сложить](../operators/numeric), [вычесть](../operators/numeric) и [сравнить](../operators/relational). В противном случае это вызовет ошибки.

!!! info "Информация"

    Глобальное имя этой функции — <code>compa<strong>ra</strong>ble</code>, но при добавлении в модуль `sass:math` название было изменено на <code>compa<strong>ti</strong>ble</code>, чтобы более точно отражать её назначение.

=== "SCSS"

    ```scss
    @use 'sass:math';

    @debug math.compatible(2px, 1px); // true
    @debug math.compatible(100px, 3em); // false
    @debug math.compatible(10cm, 3mm); // true
    ```

=== "SASS"

    ```sass
    @use 'sass:math'

    @debug math.compatible(2px, 1px) // true
    @debug math.compatible(100px, 3em) // false
    @debug math.compatible(10cm, 3mm) // true
    ```

### is-unitless

```scss
math.is-unitless($number)
unitless($number) //=> булево значение
```

Возвращает, является ли `$number` безразмерной величиной.

=== "SCSS"

    ```scss
    @use 'sass:math';

    @debug math.is-unitless(100); // true
    @debug math.is-unitless(100px); // false
    ```

=== "SASS"

    ```sass
    @use 'sass:math'

    @debug math.is-unitless(100) // true
    @debug math.is-unitless(100px) // false
    ```

### unit

```scss
math.unit($number)
unit($number) //=> строка в кавычках
```

Возвращает строковое представление единиц измерения `$number`.

!!! note "Примечание"

    Эта функция предназначена для отладки; формат её вывода не гарантируется постоянным между версиями или реализациями Sass.

=== "SCSS"

    ```scss
    @use 'sass:math';

    @debug math.unit(100); // ""
    @debug math.unit(100px); // "px"
    @debug math.unit(5px * 10px); // "px*px"
    @debug math.unit(math.div(5px, 1s)); // "px/s"
    ```

=== "SASS"

    ```sass
    @use 'sass:math'

    @debug math.unit(100) // ""
    @debug math.unit(100px) // "px"
    @debug math.unit(5px * 10px) // "px*px"
    @debug math.unit(math.div(5px, 1s)) // "px/s"
    ```

## Другие функции {#other-functions}

### div

```scss
math.div($number1, $number2) //=> число
```

Возвращает результат деления `$number1` на `$number2`.

Любые общие единицы измерения сократятся. Единицы из `$number1`, которых нет в `$number2`, окажутся в числителе результата, а единицы из `$number2`, которых нет в `$number1`, — в знаменателе.

!!! note "Примечание"

    Для обратной совместимости эта функция возвращает точно тот же результат, что и устаревший оператор `/`, включая конкатенацию двух строк символом `/` между ними. Однако это поведение будет удалено в будущем и не должно использоваться в новых таблицах стилей.

=== "SCSS"

    ```scss
    @use 'sass:math';

    @debug math.div(1, 2); // 0.5
    @debug math.div(100px, 5px); // 20
    @debug math.div(100px, 5); // 20px
    @debug math.div(100px, 5s); // 20px/s
    ```

=== "SASS"

    ```sass
    @use 'sass:math'

    @debug math.div(1, 2) // 0.5
    @debug math.div(100px, 5px) // 20
    @debug math.div(100px, 5) // 20px
    @debug math.div(100px, 5s) // 20px/s
    ```

### percentage

```scss
math.percentage($number)
percentage($number) //=> число
```

Преобразует безразмерное число `$number` (обычно десятичную дробь от 0 до 1) в процент.

!!! note "Примечание"

    Эта функция идентична выражению `$number * 100%`.

=== "SCSS"

    ```scss
    @use 'sass:math';

    @debug math.percentage(0.2); // 20%
    @debug math.percentage(math.div(100px, 50px)); // 200%
    ```

=== "SASS"

    ```sass
    @use 'sass:math'

    @debug math.percentage(0.2) // 20%
    @debug math.percentage(math.div(100px, 50px)) // 200%
    ```

### random

```scss
math.random($limit: null)
random($limit: null) //=> число
```

Если `$limit` равно `null`, возвращает случайное десятичное число от 0 до 1.

=== "SCSS"

    ```scss
    @use 'sass:math';

    @debug math.random(); // 0.2821251858
    @debug math.random(); // 0.6221325814
    ```

=== "SASS"

    ```sass
    @use 'sass:math'

    @debug math.random() // 0.2821251858
    @debug math.random() // 0.6221325814
    ```

Если `$limit` — число больше или равное 1, возвращает случайное целое число от 1 до `$limit`.

!!! note "Примечание"

    `random()` игнорирует единицы измерения у `$limit`. Это поведение устаревает и в будущем `random($limit)` будет возвращать случайное целое число с теми же единицами измерения, что и аргумент `$limit`.

    === "SCSS"

        ```scss
        @use 'sass:math';

        @debug math.random(100px); // 42
        ```

    === "SASS"

        ```sass
        @use 'sass:math'

        @debug math.random(100px) // 42
        ```

=== "SCSS"

    ```scss
    @use 'sass:math';

    @debug math.random(10); // 4
    @debug math.random(10000); // 5373
    ```

=== "SASS"

    ```sass
    @use 'sass:math'

    @debug math.random(10) // 4
    @debug math.random(10000) // 5373
    ```
