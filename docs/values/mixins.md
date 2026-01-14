---
title: Миксины
icon: lucide/book-x
---

[Миксины](../at-rules/mixin) также могут быть значениями! Вы не можете напрямую записать миксин как значение, но можете передать имя миксина функции [`meta.get-mixin()`](../modules/meta#get-mixin), чтобы получить его как значение. Получив значение миксина, вы можете передать его миксину [`meta.apply()`](../modules/meta#apply) для его вызова. Это позволяет библиотекам быть расширяемыми сложными и мощными способами.

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
