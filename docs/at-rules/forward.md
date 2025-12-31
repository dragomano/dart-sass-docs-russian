---
title: "@forward"
icon: lucide/at-sign
---

> Правило `@forward` загружает таблицу стилей Sass и делает её [миксины](../at-rules/mixin), [функции](../at-rules/function) и [переменные](../variables) доступными, когда ваша таблица стилей загружается с помощью [правила `@use`](../at-rules/use). Это позволяет организовать библиотеки Sass в множестве файлов, позволяя их пользователям загружать один файл точки входа.

Правило записывается как `@forward "<url>"`. Оно загружает модуль по указанному URL так же, как `@use`, но делает [публичные](../at-rules/use#private-members) элементы загруженного модуля доступными пользователям вашего модуля, как если бы они были определены непосредственно в вашем модуле. Однако эти элементы недоступны в вашем модуле — если вы этого хотите, вам нужно будет также написать правило `@use`. Не волнуйтесь, модуль загрузится только один раз!

Если вы всё же пишете и `@forward`, и `@use` для одного и того же модуля в одном файле, всегда полезно писать `@forward` первым. Таким образом, если ваши пользователи захотят [настроить перенаправленный модуль](../at-rules/use#configuration), эта конфигурация будет применена к `@forward` до того, как ваш `@use` загрузит его без какой-либо конфигурации.

!!! note "Примечание"

    Правило `@forward` действует так же, как `@use`, когда речь идет о CSS модуля. Стили из перенаправленного модуля будут включены в скомпилированный CSS, и модуль с `@forward` может [расширять](../at-rules/extend) его, даже если он также не загружен через `@use`.

<div class="grid" markdown>

=== "SCSS"

    ```scss title="src/_list.scss"
    @mixin list-reset {
      margin: 0;
      padding: 0;
      list-style: none;
    }
    ```
    ```scss title="bootstrap.scss"
    @forward "src/list";
    ```
    ```scss title="styles.scss"
    @use "bootstrap";

    li {
      @include bootstrap.list-reset;
    }
    ```

=== "Sass"

    ```sass title="src/_list.sass"
    @mixin list-reset
      margin: 0
      padding: 0
      list-style: none
    ```
    ```sass title="bootstrap.sass"
    @forward "src/list"
    ```
    ```sass title="styles.sass"
    @use "bootstrap"

    li
      @include bootstrap.list-reset
    ```

```css title="CSS"
li {
  margin: 0;
  padding: 0;
  list-style: none;
}
```

</div>

## Добавление префикса {#adding-a-prefix}

Поскольку элементы модуля обычно используются с [пространством имён](../at-rules/use#loading-members), короткие и простые имена, как правило, являются наиболее читаемым вариантом. Но эти имена могут не иметь смысла за пределами модуля, в котором они определены, поэтому у `@forward` есть опция добавления дополнительного префикса ко всем элементам, которые он перенаправляет.

Это записывается как `@forward "<url>" as <prefix>-*`, и добавляет указанный префикс в начало каждого имени миксина, функции и переменной, перенаправляемого модулем. Например, если модуль определяет элемент с именем `reset` и он перенаправляется как `list-*`, последующие таблицы стилей будут ссылаться на него как `list-reset`.

<div class="grid" markdown>

=== "SCSS"

    ```scss title="src/_list.scss"
    @mixin reset {
      margin: 0;
      padding: 0;
      list-style: none;
    }
    ```
    ```scss title="bootstrap.scss"
    @forward "src/list" as list-*;
    ```
    ```scss title="styles.scss"
    @use "bootstrap";

    li {
      @include bootstrap.list-reset;
    }
    ```

=== "Sass"

    ```sass title="src/_list.sass"
    @mixin reset
      margin: 0
      padding: 0
      list-style: none
    ```
    ```sass title="bootstrap.sass"
    @forward "src/list" as list-*
    ```
    ```sass title="styles.sass"
    @use "bootstrap"

    li
      @include bootstrap.list-reset
    ```

```css title="CSS"
li {
  margin: 0;
  padding: 0;
  list-style: none;
}
```

</div>

## Управление видимостью {#controlling-visibility}

Иногда вы не хотите перенаправлять *каждый* элемент из модуля. Вы можете захотеть сохранить некоторые элементы приватными, чтобы только ваш пакет мог их использовать, или потребовать от пользователей загружать некоторые элементы другим способом. Вы можете точно контролировать, какие элементы перенаправляются, написав `@forward "<url>" hide <members...>` или `@forward "<url>" show <members...>`.

Форма `hide` означает, что перечисленные элементы не должны перенаправляться, а всё остальное — должно. Форма `show` означает, что *только* указанные элементы должны перенаправляться. В обеих формах вы перечисляете имена миксинов, функций или переменных (включая `$`).

<div class="grid" markdown>

=== "SCSS"

    ```scss title="src/_list.scss"
    $horizontal-list-gap: 2em;

    @mixin list-reset {
      margin: 0;
      padding: 0;
      list-style: none;
    }

    @mixin list-horizontal {
      @include list-reset;

      li {
        display: inline-block;
        margin: {
          left: -2px;
          right: $horizontal-list-gap;
        }
      }
    }
    ```
    ```scss title="bootstrap.scss"
    @forward "src/list" hide list-reset, $horizontal-list-gap;
    ```

=== "Sass"

    ```sass title="src/_list.sass"
    $horizontal-list-gap: 2em

    @mixin list-reset
      margin: 0
      padding: 0
      list-style: none


    @mixin list-horizontal
      @include list-rest

      li
        display: inline-block
        margin:
          left: -2px
          right: $horizontal-list-gap
    ```
    ```sass title="bootstrap.sass"
    @forward "src/list" hide list-reset, $horizontal-list-gap
    ```

</div>

## Настройка модулей {#configuring-modules}

Правило `@forward` также может загружать модуль с [настройкой](../at-rules/use#configuration). Это в основном работает так же, как и для `@use`, с одним дополнением: конфигурация правила `@forward` может использовать флаг `!default`. Это позволяет модулю изменять значения по умолчанию вышестоящей таблицы стилей, при этом позволяя последующим таблицам стилей переопределять их.

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
    ```scss title="_opinionated.scss"
    @forward 'library' with (
      $black: #222 !default,
      $border-radius: 0.1rem !default
    );
    ```
    ```scss title="style.scss"
    @use 'opinionated' with ($black: #333);
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
    ```sass title="_opinionated.sass"
    @forward 'library' with ($black: #222 !default, $border-radius: 0.1rem !default)
    ```
    ```sass title="style.sass"
    @use 'opinionated' with ($black: #333)
    ```

```css title="CSS"
code {
  border-radius: 0.1rem;
  box-shadow: 0 0.5rem 1rem rgba(51, 51, 51, 0.15);
}
```

</div>
