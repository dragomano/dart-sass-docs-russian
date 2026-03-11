---
title: Миксины
icon: lucide/book-x
---

В Sass [миксины](../at-rules/mixin) можно использовать как значения. С помощью [`meta.get-mixin()`](../modules/meta#get-mixin) вы получаете объект миксина, который затем можно вызвать через [`meta.apply()`](../modules/meta#apply). Это позволяет реализовывать сложные сценарии интеграции и делает архитектуру библиотек по-настоящему масштабируемой

<div class="grid" markdown>

=== "SCSS"

    ```scss
    @use "sass:meta";
    @use "sass:string";

    /// Передаёт каждый элемент $list в отдельный вызов $mixin.
    @mixin apply-to-all($mixin, $list) {
      @each $element in $list {
        @include meta.apply($mixin, $element);
      }
    }

    @mixin font-class($size) {
      .font-#{$size} {
        font-size: $size;
      }
    }

    $sizes: [8px, 12px, 2rem];

    @include apply-to-all(meta.get-mixin("font-class"), $sizes);
    ```

=== "SASS"

    ```sass
    @use "sass:meta"
    @use "sass:string"

    /// Передаёт каждый элемент $list в отдельный вызов $mixin.
    @mixin apply-to-all($mixin, $list)
      @each $element in $list
        @include meta.apply($mixin, $element)

    @mixin font-class($size)
      .font-#{$size}
        font-size: $size

    $sizes: 8px, 12px 2rem

    @include apply-to-all(meta.get-mixin("font-class"), $sizes)
    ```

```css title="CSS"
.font-8px {
  font-size: 8px;
}

.font-12px {
  font-size: 12px;
}

.font-2rem {
  font-size: 2rem;
}
```

</div>
