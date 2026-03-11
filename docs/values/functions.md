---
title: Функции
icon: lucide/square-function
---

[Функции](../at-rules/function) поддерживают передачу в качестве значений: используйте [`meta.get-function()`](../modules/meta#get-function) для получения ссылки и [`meta.call()`](../modules/meta#call) для её вызова. Это незаменимо при написании функций высшего порядка.

<div class="grid" markdown>

=== "SCSS"

    ```scss
    @use "sass:list";
    @use "sass:meta";
    @use "sass:string";

    /// Возвращает копию $list с удалёнными элементами,
    /// для которых $condition возвращает `true`.
    @function remove-where($list, $condition) {
      $new-list: ();
      $separator: list.separator($list);
      @each $element in $list {
        @if not meta.call($condition, $element) {
          $new-list: list.append($new-list, $element, $separator: $separator);
        }
      }
      @return $new-list;
    }

    $fonts: Tahoma, Geneva, "Helvetica Neue", Helvetica, Arial, sans-serif;

    .content {
      @function contains-helvetica($string) {
        @return string.index($string, "Helvetica");
      }
      font-family: remove-where($fonts, meta.get-function("contains-helvetica"));
    }
    ```

=== "SASS"

    ```sass
    @use "sass:list"
    @use "sass:meta"
    @use "sass:string"

    /// Возвращает копию $list с удалёнными элементами,
    /// для которых $condition возвращает `true`.
    @function remove-where($list, $condition)
      $new-list: ()
      $separator: list.separator($list)
      @each $element in $list
        @if not meta.call($condition, $element)
          $new-list: list.append($new-list, $element, $separator: $separator)

      @return $new-list

    $fonts: Tahoma, Geneva, "Helvetica Neue", Helvetica, Arial, sans-serif

    .content
      @function contains-helvetica($string)
        @return string.index($string, "Helvetica")

      font-family: remove-where($fonts, meta.get-function("contains-helvetica"))
    ```

```css title="CSS"
.content {
  font-family: Tahoma, Geneva, Arial, sans-serif;
}
```

</div>
