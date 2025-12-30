---
title: Переменные
icon: lucide/variable
---

> Переменные Sass просты: вы присваиваете значение имени, которое начинается с `$`, а затем можете ссылаться на это имя вместо самого значения. Но, несмотря на простоту, это один из самых полезных инструментов, которые Sass привносит в работу. Переменные позволяют сократить повторения, выполнять сложные вычисления, настраивать библиотеки и многое другое.

Объявление переменной очень похоже на [объявление свойства](../style-rules/declarations): оно записывается как `<переменная>: <выражение>`. В отличие от свойства, которое можно объявлять только в правиле стиля или at-правиле, переменные можно объявлять где угодно. Чтобы использовать переменную, просто включите её в значение.

<div class="grid" markdown>

=== "SCSS"

    ```scss
    $base-color: #c6538c;
    $border-dark: rgba($base-color, 0.88);

    .alert {
      border: 1px solid $border-dark;
    }
    ```

=== "Sass"

    ```sass
    $base-color: #c6538c
    $border-dark: rgba($base-color, 0.88)

    .alert
      border: 1px solid $border-dark
    ```

```css title="CSS"
.alert {
  border: 1px solid rgba(198, 83, 140, 0.88);
}
```

</div>

!!! note "Примечание"

    В CSS есть [собственные переменные](/style-rules/declarations#custom-properties), которые полностью отличаются от переменных Sass. Знайте различия!

    * Все переменные Sass компилируются самим Sass. Переменные CSS включаются в выходной CSS.

    * Переменные CSS могут иметь разные значения для разных элементов, но переменные Sass имеют только одно значение в каждый момент времени.

    * Переменные Sass — *императивные*: это означает, что если вы используете переменную, а затем измените её значение, предыдущее использование останется прежним. Переменные CSS — *декларативные*: это означает, что если вы измените значение, это повлияет как на предыдущие, так и на последующие использования.

    <div class="grid" markdown>

    === "SCSS"

        ```scss
        $variable: value 1;
        .rule-1 {
          value: $variable;
        }

        $variable: value 2;
        .rule-2 {
          value: $variable;
        }
        ```

    === "Sass"

        ```sass
        $variable: value 1
        .rule-1
          value: $variable

        $variable: value 2
        .rule-2
          value: $variable
        ```

    ```css title="CSS"
    .rule-1 {
      value: value 1;
    }

    .rule-2 {
      value: value 2;
    }
    ```

    </div>

!!! info "Информация"

    Переменные Sass, как и все идентификаторы Sass, воспринимают дефисы и нижние подчёркивания одинаково. Это означает, что `$font-size` и `$font_size` ссылаются на одну и ту же переменную. Это историческое наследие самых ранних дней Sass, когда в именах идентификаторов допускались *только* нижние подчёркивания. После того как Sass добавил поддержку дефисов для соответствия синтаксису CSS, эти два варианта сделали эквивалентными, чтобы облегчить переход.

## Значения по умолчанию {#default-values}

Обычно при присваивании значения переменной, если эта переменная уже имела значение, старое значение перезаписывается. Но если вы пишете библиотеку Sass, вы можете захотеть разрешить пользователям настраивать переменные библиотеки перед тем, как использовать их для генерации CSS.

Для этого Sass предоставляет флаг `!default`. Это присваивает значение переменной *только если* эта переменная не определена или её значение равно [`null`](../values/null). В противном случае будет использовано существующее значение.

### Настройка модулей {#configuring-modules}

Переменные, определённые с `!default`, можно настраивать при загрузке модуля с помощью [правила `@use`](../at-rules/use). Библиотеки Sass часто используют переменные `!default`, чтобы позволить пользователям настраивать CSS библиотеки.

Чтобы загрузить модуль с настройками, пишите `@use <url> with (<переменная>: <значение>, <переменная>: <значение>)`. Настроенные значения переопределят значения по умолчанию переменных. Настраивать можно только переменные, написанные на верхнем уровне таблицы стилей с флагом `!default`.

<div class="grid" markdown>

=== "SCSS"

    ```scss title="_library.scss"
    $black: #000 !default;
    $border-radius: 0.25rem !default;
    $box-shadow: 0 0.5rem 1rem rgba($black, 0.15) !default;

    code {
      border-radius: $border-radius;
      box-shadow: $box-shadow;
    }
    ```
    ```scss title="style.scss"
    @use 'library' with (
      $black: #222,
      $border-radius: 0.1rem
    );
    ```

=== "Sass"

    ```sass title="_library.sass"
    $black: #000 !default
    $border-radius: 0.25rem !default
    $box-shadow: 0 0.5rem 1rem rgba($black, 0.15) !default

    code
      border-radius: $border-radius
      box-shadow: $box-shadow
    ```
    ```sass title="style.sass"
    @use 'library' with ($black: #222, $border-radius: 0.1rem)
    ```

```css title="CSS"
code {
  border-radius: 0.1rem;
  box-shadow: 0 0.5rem 1rem rgba(34, 34, 34, 0.15);
}
```

</div>

## Встроенные переменные {#built-in-variables}

Переменные, определённые [встроенным модулем](../modules), нельзя изменять.

<div class="grid" markdown>

=== "SCSS"

    ```scss
    @use "sass:math" as math;

    // Это присваивание завершится ошибкой.
    math.$pi: 0;
    ```

=== "Sass"

    ```sass
    @use "sass:math" as math

    // Это присваивание завершится ошибкой.
    math.$pi: 0
    ```

</div>

## Область видимости {#scope}

Переменные, объявленные на верхнем уровне таблицы стилей, являются *глобальными*. Это означает, что к ним можно обращаться из любого места в их модуле после объявления. Но это не относится ко всем переменным. Те, что объявлены в блоках (фигурные скобки в SCSS или отступленный код в Sass), обычно являются *локальными* и доступны только внутри блока, в котором они объявлены.

<div class="grid" markdown>

=== "SCSS"

    ```scss
    $global-variable: global value;

    .content {
      $local-variable: local value;
      global: $global-variable;
      local: $local-variable;
    }

    .sidebar {
      global: $global-variable;

      // Завершится ошибкой, потому что $local-variable не в области видимости:
      // local: $local-variable;
    }
    ```

=== "Sass"

    ```sass
    $global-variable: global value

    .content
      $local-variable: local value
      global: $global-variable
      local: $local-variable

    .sidebar
      global: $global-variable

      // Завершится ошибкой, потому что $local-variable не в области видимости:
      // local: $local-variable
    ```

```css title="CSS"
.content {
  global: global value;
  local: local value;
}

.sidebar {
  global: global value;
}
```

</div>

### Перекрытие {#shadowing}

Локальные переменные даже могут объявляться с тем же именем, что и глобальная переменная. Если это происходит, на самом деле существуют две разные переменные с одинаковым именем: одна локальная и одна глобальная. Это помогает гарантировать, что автор, пишущий локальную переменную, не случайно изменит значение глобальной переменной, о существовании которой он даже не подозревает.

<div class="grid" markdown>

=== "SCSS"

    ```scss
    $variable: global value;

    .content {
      $variable: local value;
      value: $variable;
    }

    .sidebar {
      value: $variable;
    }
    ```

=== "Sass"

    ```sass
    $variable: global value

    .content
      $variable: local value
      value: $variable

    .sidebar
      value: $variable
    ```

```css title="CSS"
.content {
  value: local value;
}

.sidebar {
  value: global value;
}
```

</div>

Если вам нужно установить значение глобальной переменной из локальной области видимости (например, в миксине), вы можете использовать флаг `!global`. Объявление переменной с флагом `!global` *всегда* будет присваивать значение в глобальную область видимости.

<div class="grid" markdown>

=== "SCSS"

    ```scss
    $variable: first global value;

    .content {
      $variable: second global value !global;
      value: $variable;
    }

    .sidebar {
      value: $variable;
    }
    ```

=== "Sass"

    ```sass
    $variable: first global value

    .content
      $variable: second global value !global
      value: $variable

    .sidebar
      value: $variable
    ```

```css title="CSS"
.content {
  value: second global value;
}

.sidebar {
  value: second global value;
}
```

</div>

!!! warning "Предупреждение"

    Флаг `!global` может использоваться только для установки переменной, которая уже была объявлена на верхнем уровне файла. Он *не может* использоваться для объявления новой переменной.

### Область видимости управляющих конструкций {#flow-control-scope}

Переменные, объявленные в [правилах управляющих конструкций](../at-rules/control), имеют особые правила области видимости: они не перекрывают переменные на том же уровне, что и правило управляющей конструкции. Вместо этого они просто присваивают значения этим переменным. Это сильно упрощает условное присваивание значения переменной или накопление значения в цикле.

<div class="grid" markdown>

=== "SCSS"

    ```scss
    $dark-theme: true !default;
    $primary-color: #f8bbd0 !default;
    $accent-color: #6a1b9a !default;

    @if $dark-theme {
      $primary-color: darken($primary-color, 60%);
      $accent-color: lighten($accent-color, 60%);
    }

    .button {
      background-color: $primary-color;
      border: 1px solid $accent-color;
      border-radius: 3px;
    }
    ```

=== "Sass"

    ```sass
    $dark-theme: true !default
    $primary-color: #f8bbd0 !default
    $accent-color: #6a1b9a !default

    @if $dark-theme
      $primary-color: darken($primary-color, 60%)
      $accent-color: lighten($accent-color, 60%)

    .button
      background-color: $primary-color
      border: 1px solid $accent-color
      border-radius: 3px
    ```

```css title="CSS"
.button {
  background-color: rgb(116.96, 12.04, 48.16);
  border: 1px solid rgb(245.4696132597, 235.4309392265, 251.5690607735);
  border-radius: 3px;
}
```

</div>

!!! note "Примечание"

    Переменные в области видимости управляющих конструкций могут присваивать значения существующим переменным во внешней области видимости, но новые переменные, объявленные в области видимости управляющих конструкций, не будут доступны во внешней области. Убедитесь, что переменная уже объявлена перед присваиванием ей значения, даже если нужно объявить её как `null`.

## Продвинутые функции переменных {#advanced-variable-functions}

Основная библиотека Sass предоставляет несколько продвинутых функций для работы с переменными. Функция [`meta.variable-exists()`](../modules/meta#variable-exists) возвращает, существует ли переменная с заданным именем в текущей области видимости, а функция [`meta.global-variable-exists()`](../modules/meta#global-variable-exists) делает то же самое, но только для глобальной области видимости.

!!! tip "Совет"

    Пользователи иногда хотят использовать интерполяцию для определения имени переменной на основе другой переменной. Sass не позволяет это делать, поскольку это сильно затрудняет понимание, какие переменные где определены. Однако вы *можете* определить [карту](../values/maps) из имён к значениям, к которой затем можно обращаться с помощью переменных.

    <div class="grid" markdown>

    === "SCSS"

        ```scss
        @use "sass:map";

        $theme-colors: (
          "success": #28a745,
          "info": #17a2b8,
          "warning": #ffc107,
        );

        .alert {
          // Вместо $theme-color-#{warning}
          background-color: map.get($theme-colors, "warning");
        }
        ```

    === "Sass"

        ```sass
        @use "sass:map"

        $theme-colors: ("success": #28a745, "info": #17a2b8, "warning": #ffc107)

        .alert
          // Вместо $theme-color-#{warning}
          background-color: map.get($theme-colors, "warning")
        ```

    ```css title="CSS"
    .alert {
      background-color: #ffc107;
    }
    ```

    </div>
