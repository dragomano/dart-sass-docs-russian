---
title: "sass:list"
icon: lucide/list
---

!!! info "Информация"

    В Sass каждая [карта](../values/maps) считается списком, который содержит двухэлементный список для каждой пары «ключ/значение». Например, `(1: 2, 3: 4)` считается как `(1 2, 3 4)`. Поэтому все эти функции работают и с картами!

    Отдельные значения также считаются списками. Все эти функции обрабатывают `1px` как список, содержащий значение `1px`.

## append

```scss
list.append($list, $val, $separator: auto)
append($list, $val, $separator: auto) //=> список
```

Возвращает копию `$list` со значением `$val`, добавленным в конец списка.

Если `$separator` равен `comma`, `space` или `slash`, возвращаемый список будет соответственно разделён запятыми, пробелами или слешами. Если указано `auto` (значение по умолчанию), возвращаемый список будет использовать тот же разделитель, что и `$list` (или `space`, если у `$list` нет разделителя). Другие значения недопустимы.

Обратите внимание, что в отличие от [`list.join()`](#join), если `$val` является списком, он помещается как вложенный список внутрь возвращаемого списка, а не «распаковывается» с добавлением всех его элементов в возвращаемый список.

=== "SCSS"

    ```scss
    @use 'sass:list';

    @debug list.append(10px 20px, 30px); // 10px 20px 30px
    @debug list.append((blue, red), green); // blue, red, green
    @debug list.append(10px 20px, 30px 40px); // 10px 20px (30px 40px)
    @debug list.append(10px, 20px, $separator: comma); // 10px, 20px
    @debug list.append((blue, red), green, $separator: space); // blue red green
    ```

=== "SASS"

    ```sass
    @use 'sass:list'

    @debug list.append(10px 20px, 30px) // 10px 20px 30px
    @debug list.append((blue, red), green) // blue, red, green
    @debug list.append(10px 20px, 30px 40px) // 10px 20px (30px 40px)
    @debug list.append(10px, 20px, $separator: comma) // 10px, 20px
    @debug list.append((blue, red), green, $separator: space) // blue red green
    ```

## index

```scss
list.index($list, $value)
index($list, $value) //=> число | null
```

Возвращает [индекс](../values/lists#indexes) значения `$value` в списке `$list`.

Если `$value` не найдено в `$list`, функция возвращает [`null`](../values/null). Если `$value` встречается в `$list` несколько раз, возвращается индекс его первого вхождения.

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

    @debug list.index(1px solid red, 1px) // 1
    @debug list.index(1px solid red, solid) // 2
    @debug list.index(1px solid red, dashed) // null
    ```

## is-bracketed

```scss
list.is-bracketed($list)
is-bracketed($list) //=> булево значение
```

Возвращает, имеет ли `$list` квадратные скобки.

=== "SCSS"

    ```scss
    @use 'sass:list';

    @debug list.is-bracketed(1px 2px 3px); // false
    @debug list.is-bracketed([1px, 2px, 3px]); // true
    ```

=== "SASS"

    ```sass
    @use 'sass:list'

    @debug list.is-bracketed(1px 2px 3px) // false
    @debug list.is-bracketed([1px, 2px, 3px]) // true
    ```

## join

```scss
list.join($list1, $list2, $separator: auto, $bracketed: auto)
join($list1, $list2, $separator: auto, $bracketed: auto) //=> список
```

!!! tip "Совет"

    Поскольку отдельные значения считаются списками из одного элемента, возможно использовать `list.join()` для добавления значения в конец списка. Однако *это не рекомендуется*, поскольку если это значение является списком, оно будет конкатенировано, что, скорее всего, не то, чего вы ожидаете.

    Вместо этого используйте [`list.append()`](#append) для добавления одного значения в список. Используйте `list.join()` только для объединения двух списков в один.

Возвращает новый список, содержащий элементы `$list1`, за которыми следуют элементы `$list2`.

Если `$separator` равен `comma`, `space` или `slash`, возвращаемый список будет соответственно разделён запятыми, пробелами или слешами. Если указано `auto` (значение по умолчанию), возвращаемый список будет использовать тот же разделитель, что и `$list1`, если он есть; иначе — разделитель из `$list2`, если он есть; иначе — `space`. Другие значения недопустимы.

Если `$bracketed` равно `auto` (значение по умолчанию), возвращаемый список будет в скобках, если `$list1` в скобках. В противном случае возвращаемый список будет иметь квадратные скобки, если `$bracketed` [истинно](../values/booleans#truthiness-and-falsiness), и не будет иметь скобок, если `$bracketed` ложно.

=== "SCSS"

    ```scss
    @use 'sass:list';

    @debug list.join(10px 20px, 30px 40px); // 10px 20px 30px 40px
    @debug list.join((blue, red), (#abc, #def)); // blue, red, #abc, #def
    @debug list.join(10px, 20px); // 10px 20px
    @debug list.join(10px, 20px, $separator: comma); // 10px, 20px
    @debug list.join((blue, red), (#abc, #def), $separator: space); // blue red #abc #def
    @debug list.join([10px], 20px); // [10px 20px]
    @debug list.join(10px, 20px, $bracketed: true); // [10px 20px]
    ```

=== "SASS"

    ```sass
    @use 'sass:list'

    @debug list.join(10px 20px, 30px 40px) // 10px 20px 30px 40px
    @debug list.join((blue, red), (#abc, #def)) // blue, red, #abc, #def
    @debug list.join(10px, 20px) // 10px 20px
    @debug list.join(10px, 20px, comma) // 10px, 20px
    @debug list.join((blue, red), (#abc, #def), space) // blue red #abc #def
    @debug list.join([10px], 20px) // [10px 20px]
    @debug list.join(10px, 20px, $bracketed: true) // [10px 20px]
    ```

## length

```scss
list.length($list)
length($list) //=> число
```

Возвращает длину `$list`.

Также может возвращать количество пар в карте.

=== "SCSS"

    ```scss
    @use 'sass:list';

    @debug list.length(10px); // 1
    @debug list.length(10px 20px 30px); // 3
    @debug list.length((width: 10px, height: 20px)); // 2
    ```

=== "SASS"

    ```sass
    @use 'sass:list'

    @debug list.length(10px) // 1
    @debug list.length(10px 20px 30px) // 3
    @debug list.length((width: 10px, height: 20px)) // 2
    ```

## separator

```scss
list.separator($list)
list-separator($list) //=> строка без кавычек
```

Возвращает имя разделителя, используемого в `$list`: `space`, `comma` или `slash`.

Если у `$list` нет разделителя, возвращает `space`.

=== "SCSS"

    ```scss
    @use 'sass:list';

    @debug list.separator(1px 2px 3px); // space
    @debug list.separator((1px, 2px, 3px)); // comma
    @debug list.separator('Helvetica'); // space
    @debug list.separator(()); // space
    ```

=== "SASS"

    ```sass
    @use 'sass:list'

    @debug list.separator(1px 2px 3px) // space
    @debug list.separator((1px, 2px, 3px)) // comma
    @debug list.separator('Helvetica') // space
    @debug list.separator(()) // space
    ```

## nth

```scss
list.nth($list, $n)
nth($list, $n)
```

Возвращает элемент `$list` по [индексу](../values/lists#indexes) `$n`.

Если `$n` отрицательный, отсчёт ведётся с конца `$list`. Выбрасывает ошибку, если элемента с индексом `$n` не существует.

=== "SCSS"

    ```scss
    @use 'sass:list';

    @debug list.nth(10px 12px 16px, 2); // 12px
    @debug list.nth([line1, line2, line3], -1); // line3
    ```

=== "SASS"

    ```sass
    @use 'sass:list'

    @debug list.nth(10px 12px 16px, 2) // 12px
    @debug list.nth([line1, line2, line3], -1) // line3
    ```

## set-nth

```scss
list.set-nth($list, $n, $value)
set-nth($list, $n, $value) //=> список
```

Возвращает копию `$list` с элементом по [индексу](../values/lists#indexes) `$n`, заменённым на `$value`.

Если `$n` отрицательный, отсчёт ведётся с конца `$list`. Выбрасывает ошибку, если элемента с индексом `$n` не существует.

=== "SCSS"

    ```scss
    @use 'sass:list';

    @debug list.set-nth(10px 20px 30px, 1, 2em); // 2em 20px 30px
    @debug list.set-nth(10px 20px 30px, -1, 8em); // 10px, 20px, 8em
    @debug list.set-nth((Helvetica, Arial, sans-serif), 3, Roboto); // Helvetica, Arial, Roboto
    ```

=== "SASS"

    ```sass
    @use 'sass:list'

    @debug list.set-nth(10px 20px 30px, 1, 2em); // 2em 20px 30px
    @debug list.set-nth(10px 20px 30px, -1, 8em); // 10px, 20px, 8em
    @debug list.set-nth((Helvetica, Arial, sans-serif), 3, Roboto); // Helvetica, Arial, Roboto
    ```

## slash

```scss
list.slash($elements...) //=> список
```

Возвращает список, разделённый слешами, содержащий `$elements`.

!!! note "Примечание"

    Эта функция — временное решение для создания списков с разделителем слеш. В будущем они будут писаться буквально со слешами, например `1px / 2px / solid`, но пока [слеши используются для деления](../breaking-changes/slash-div), Sass не может использовать их для новой синтаксиса, пока старый синтаксис не будет удалён.

=== "SCSS"

    ```scss
    @use 'sass:list';

    @debug list.slash(1px, 50px, 100px); // 1px / 50px / 100px
    ```

=== "SASS"

    ```sass
    @use 'sass:list'

    @debug list.slash(1px, 50px, 100px)  // 1px / 50px / 100px
    ```

## zip

```scss
list.zip($lists...)
zip($lists...) //=> список
```

Объединяет каждый список из `$lists` в единый список подсписков.

Каждый элемент возвращаемого списка содержит все элементы на этой позиции в `$lists`. Длина возвращаемого списка равна длине самого короткого списка в `$lists`.

Возвращаемый список всегда разделён запятыми, а подсписки — пробелами.

=== "SCSS"

    ```scss
    @use 'sass:list';

    @debug list.zip(10px 50px 100px, short mid long); // 10px short, 50px mid, 100px long
    @debug list.zip(10px 50px 100px, short mid); // 10px short, 50px mid
    ```

=== "SASS"

    ```sass
    @use 'sass:list'

    @debug list.zip(10px 50px 100px, short mid long)  // 10px short, 50px mid, 100px long
    @debug list.zip(10px 50px 100px, short mid)  // 10px short, 50px mid
    ```
