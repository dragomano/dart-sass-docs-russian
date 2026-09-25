---
title: Смешанные объявления
icon: lucide/newspaper
---

> CSS изменил способ обработки объявлений, смешанных с вложенными правилами, и мы убедились, что Sass соответствует этому поведению.

## История до сих пор {#the-story-so-far}

Исторически, если вы смешивали вложенные правила и объявления в Sass, он выносил все объявления в начало правила, чтобы избежать избыточного дублирования внешнего селектора. Например:

<div class="grid" markdown>

=== "SCSS"

    ```scss
    .example {
      color: red;

      &--serious {
        font-weight: bold;
      }

      font-weight: normal;
    }
    ```

=== "SASS"

    ```sass
    .example
      color: red

      &--serious
        font-weight: bold

      font-weight: normal
    ```

```css title="CSS"
 .example {
  color: red;
  font-weight: normal;
}

.example--serious {
  font-weight: bold;
}
```

</div>

Когда [вложенность в обычном CSS](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_nesting) была впервые введена, она вела себя точно так же. Однако после некоторых обсуждений [рабочая группа CSS решила](https://github.com/w3c/csswg-drafts/issues/8738), что логичнее делать так, чтобы объявления применялись в том порядке, в котором они появляются в документе:

<div class="grid" markdown>

=== "SCSS"

    ```scss
    .example {
      color: red;

      &--serious {
        font-weight: bold;
      }

      font-weight: normal;
    }
    ```

=== "SASS"

    ```sass
    .example
      color: red

      &--serious
        font-weight: bold

      font-weight: normal
    ```

```css title="CSS"
.example {
  color: red;
}

.example--serious {
  font-weight: bold;
}

.example {
  font-weight: normal;
}
```

</div>

## Устаревание старого способа {#deprecating-the-old-way}

Использование объявлений _после_ вложенных правил было впервые устаревшим, чтобы уведомить пользователей о предстоящем изменении и дать им время сделать свои таблицы стилей совместимыми с ним.

Пользователи, которые хотели раньше перейти на новую семантику CSS, могли обернуть свои вложенные объявления в `& {}`:

<div class="grid" markdown>

=== "SCSS"

    ```scss
    .example {
      color: red;

      &--serious {
        font-weight: bold;
      }

      & {
        font-weight: normal;
      }
    }
    ```

=== "SASS"

    ```sass
    .example
      color: red

      &--serious
        font-weight: bold

      &
        font-weight: normal
    ```

```css title="CSS"
.example {
  color: red;
}
.example--serious {
  font-weight: bold;
}
.example {
  font-weight: normal;
}
```

</div>

--8<-- "includes/silence-warnings.md"

## Новый способ {#the-new-way}

Современные версии Sass ведут себя так же, как обычный CSS: объявления выводятся в том же порядке, в котором они написаны, даже если это требует дублирования правила, содержащего их, чтобы учесть переплетённые правила. То же самое относится к громким комментариям (`/* */`-стиля) и at-правилам без дочерних элементов.
