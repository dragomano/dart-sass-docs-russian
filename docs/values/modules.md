---
title: Модули
icon: lucide/chart-candlestick
---

[Модули](../at-rules/use) также могут быть значениями! Нельзя напрямую записать модуль как значение, но можно обратиться к модулю, подключённому с помощью `@use`, через [`meta.get-module()`](../modules/meta#get-module) или загрузить новый модуль с помощью [`meta.load()`](../modules/meta#load). Значения модулей позволяют выбирать, загружать ли модуль, исходя из логики таблицы стилей, а также условно загружать разные модули.

Получив значение модуля, можно использовать различные дополнительные функции для его анализа и взаимодействия с ним:

* [`meta.module-variables()`](../modules/meta#module-variables) и [`meta.global-variable-exists()`](../modules/meta#global-variable-exists) позволяют анализировать его переменные.
* [`meta.get-function()`](../modules/meta#get-function), [`meta.module-functions()`](../modules/meta#module-functions) и [`meta.function-exists()`](../modules/meta#function-exists) позволяют анализировать его функции.
* [`meta.get-mixin()`](../modules/meta#get-mixin), [`meta.module-mixins()`](../modules/meta#module-mixins) и [`meta.mixin-exists()`](../modules/meta#mixin-exists) позволяют анализировать его миксины.

Кроме того, можно использовать миксин [`meta.css()`](../modules/meta/#css), чтобы включить CSS, содержащийся в значении модуля.

<div class="grid" markdown>

=== "SCSS"

    ```scss title="_themes.scss"
    @use 'sass:map';
    @use 'sass:meta';

    @function load-theme($theme-name) {
      $module: meta.load('themes/#{$theme-name}');

      @each $token in ['foreground', 'background', 'highlight'] {
        @if not meta.variable-exists($token, $module: $module) {
          @error "#{$theme} is missing $#{$token}";
        }
      }

      @return meta.module-variables($module);
    }
    ```
    ```scss title="themes/_desert.scss"
    $foreground: #783314;
    $background: #ec883c;
    $highlight: #a9c6e6;
    ```

=== "Sass"

    ```sass title="_themes.sass"
    @use 'sass:map'
    @use 'sass:meta'

    @function load-theme($theme-name)
      $module: meta.load('themes/#{$theme-name}')

      @each $token in ['foreground', 'background', 'highlight']
        @if not meta.variable-exists($token, $module: $module)
          @error "#{$theme} is missing $#{$token}"

    @return meta.module-variables($module)
    ```
    ```scss title="themes/_desert.sass"
    $foreground: #783314
    $background: #ec883c
    $highlight: #a9c6e6
    ```

</div>
