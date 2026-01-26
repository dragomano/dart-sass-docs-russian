---
title: Расширение составных селекторов
icon: lucide/newspaper
---

> В настоящее время LibSass позволяет [расширять](../at-rules/extend) составные селекторы, такие как `.message.info`, но способ, которым они расширяются, не соответствует тому, как должен работать `@extend`.

Когда один селектор расширяет другой, Sass стилизует все элементы, которые соответствуют расширяющему селектору, так, как если бы они также соответствовали расширяемому классу. Другими словами, если вы напишете `.heads-up {@extend .info}`, это работает так же, как если бы вы заменили `class="heads-up"` в вашем HTML на `class="heads-up info"`.

Следуя этой логике, вы ожидали бы, что `.heads-up {@extend .message.info}` будет работать как замена `class="heads-up"` на `class="heads-up info message"`. Но сейчас это работает не так в LibSass и Ruby Sass — вместо добавления `.heads-up` к каждому селектору, который имеет *либо `.info`, либо `.message`*, он добавляет его только к селекторам, которые имеют *`.info.message` вместе*.

=== "SCSS"

    ```scss
    // Оба они должны быть расширены, но этого не произойдет.
    .message {
      border: 1px solid black;
    }
    .info {
      font-size: 1.5rem;
    }

    .heads-up {
      @extend .message.info;
    }
    ```

=== "SASS"

    ```sass
    // Оба они должны быть расширены, но этого не произойдет.
    .message
      border: 1px solid black

    .info
      font-size: 1.5rem

    .heads-up
      @extend .message.info
    ```

Чтобы исправить эту проблему, избежать дополнительной путаницы и поддерживать реализацию чистой и эффективной, возможность расширения составных селекторов не поддерживается в Dart Sass и будет удалена в будущей версии LibSass. Для совместимости пользователи должны вместо этого расширять каждый простой селектор отдельно:

<div class="grid" markdown>

=== "SCSS"

    ```scss
    .message {
      border: 1px solid black;
    }
    .info {
      font-size: 1.5rem;
    }

    .heads-up {
      @extend .message, .info;
    }
    ```

=== "SASS"

    ```sass
    .message
      border: 1px solid black

    .info
      font-size: 1.5rem

    .heads-up
      @extend .message, .info
    ```

```css title="CSS"
.message, .heads-up {
  border: 1px solid black;
}

.info, .heads-up {
  font-size: 1.5rem;
}
```

</div>

!!! tip "Совет"

    Поскольку Sass не знает деталей HTML, который будет стилизовать CSS, любой `@extend` может потребовать генерации дополнительных селекторов, которые не будут применяться к вашему конкретному HTML. Это особенно верно при отказе от расширения составных селекторов.

    В большинстве случаев эти дополнительные селекторы не вызовут никаких проблем и добавят лишь пару дополнительных байтов к сжатому gzip CSS. Но некоторые таблицы стилей могут более сильно полагаться на старое поведение. В этом случае мы рекомендуем заменить составной селектор на [селектор-заполнитель](../style-rules/placeholder-selectors).

    <div class="grid" markdown>

    === "SCSS"

        ```scss
        // Вместо просто `.message.info`.
        %message-info, .message.info {
          border: 1px solid black;
          font-size: 1.5rem;
        }

        .heads-up {
          // Вместо `.message.info`.
          @extend %message-info;
        }
        ```

    === "SASS"

        ```sass
        // Вместо просто `.message.info`.
        %message-info, .message.info
          border: 1px solid black
          font-size: 1.5rem


        .heads-up
          // Вместо `.message.info`.
          @extend %message-info
        ```

    ```css title="CSS"
    .heads-up, .message.info {
      border: 1px solid black;
      font-size: 1.5rem;
    }
    ```

    </div>
