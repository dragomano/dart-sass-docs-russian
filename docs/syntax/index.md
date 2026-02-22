---
title: Синтаксис
icon: lucide/braces
---

> Sass поддерживает два разных синтаксиса. Каждый из них может загружать другой, так что выбор между ними зависит от вас и вашей команды.

## SCSS

Синтаксис SCSS использует расширение файла `.scss`. За некоторыми небольшими исключениями, это надмножество CSS, что означает, по сути, что **весь валидный CSS также является валидным SCSS**. Благодаря схожести с CSS, это самый простой синтаксис для освоения и самый популярный.

SCSS выглядит так:

```scss
@mixin button-base() {
  @include typography(button);
  @include ripple-surface;
  @include ripple-radius-bounded;

  display: inline-flex;
  position: relative;
  height: $button-height;
  border: none;
  vertical-align: middle;

  &:hover {
    cursor: pointer;
  }

  &:disabled {
    color: $mdc-button-disabled-ink-color;
    cursor: default;
    pointer-events: none;
  }
}
```

## Синтаксис с отступами {#the-indented-syntax}

Синтаксис с отступами был оригинальным синтаксисом Sass, поэтому он использует расширение файла `.sass`. Из-за этого расширения его иногда просто называют «Sass». Синтаксис с отступами поддерживает все те же функции, что и SCSS, но использует отступы вместо фигурных скобок и точек с запятой для описания формата документа.

В общем случае, везде, где вы бы написали фигурные скобки в CSS или SCSS, вы можете просто добавить отступ на один уровень глубже в синтаксисе с отступами. И везде, где строка заканчивается в месте, где может закончиться выражение, это считается точкой с запятой. Также существует несколько дополнительных различий в синтаксисе с отступами, которые отмечены в справочнике.

Синтаксис с отступами выглядит так:

```sass
@mixin button-base()
  @include typography(button)
  @include ripple-surface
  @include ripple-radius-bounded

  display: inline-flex
  position: relative
  height: $button-height
  border: none
  vertical-align: middle

  &:hover
    cursor: pointer

  &:disabled
    color: $mdc-button-disabled-ink-color
    cursor: default
    pointer-events: none
```

### Многострочные выражения {#multiline-statements}

В синтаксисе с отступами выражения могут занимать несколько строк, если перенос происходит в местах, где выражение не может быть завершено: внутри скобок любого типа или между ключевыми словами в Sass-специфичных @-директивах.

```sass
.grid
  display: grid
  grid-template: (
    "header" min-content
    "main" 1fr
  )

@for
  $i from
  1 through 3
    ul:nth-child(3n + #{$i})
      margin-left: $i * 10
```
