---
title: "sass:map"
icon: lucide/map
---

!!! info "Информация"

    Библиотеки Sass и дизайн-системы, как правило, совместно используют и переопределяют конфигурации, которые представлены в виде вложенных карт (карт, содержащих карты, которые содержат карты).

    Чтобы помочь вам работать с вложенными картами, некоторые функции карт поддерживают глубокие операции. Например, если вы передаёте несколько ключей в `map.get()`, она будет следовать по этим ключам, чтобы найти нужную вложенную карту:

    === "SCSS"

        ```scss
        @use "sass:map";

        $config: (a: (b: (c: d)));
        @debug map.get($config, a, b, c); // d
        ```

    === "SASS"

        ```sass
        @use "sass:map"

        $config: (a: (b: (c: d)))
        @debug map.get($config, a, b, c) // d
        ```

## deep-merge

```scss
map.deep-merge($map1, $map2) //=> карта
```

Идентична [`map.merge()`](#merge), за исключением того, что значения вложенных карт *также* рекурсивно объединяются.

=== "SCSS"

    ```scss
    @use "sass:map";

    $helvetica-light: (
      "weights": (
        "lightest": 100,
        "light": 300
      )
    );
    $helvetica-heavy: (
      "weights": (
        "medium": 500,
        "bold": 700
      )
    );

    @debug map.deep-merge($helvetica-light, $helvetica-heavy);
    // (
    //   "weights": (
    //     "lightest": 100,
    //     "light": 300,
    //     "medium": 500,
    //     "bold": 700
    //   )
    // )
    @debug map.merge($helvetica-light, $helvetica-heavy);
    // (
    //   "weights": (
    //     "medium: 500,
    //     "bold": 700
    //   )
    // )
    ```

=== "SASS"

    ```sass
    @use "sass:map"

    $helvetica-light: ("weights": ("lightest": 100, "light": 300))
    $helvetica-heavy: ("weights": ("medium": 500, "bold": 700))

    @debug map.deep-merge($helvetica-light, $helvetica-heavy)
    // (
    //   "weights": (
    //     "lightest": 100,
    //     "light": 300,
    //     "medium": 500,
    //     "bold": 700
    //   )
    // )
    @debug map.merge($helvetica-light, $helvetica-heavy);
    // (
    //   "weights": (
    //     "medium: 500,
    //     "bold": 700
    //   )
    // )
    ```

## deep-remove

```scss
map.deep-remove($map, $key, $keys...) //=> карта
```

Если `$keys` пустой, возвращает копию `$map` без значения, связанного с `$key`.

=== "SCSS"

    ```scss
    @use "sass:map";

    $font-weights: ("regular": 400, "medium": 500, "bold": 700);

    @debug map.deep-remove($font-weights, "regular");
    // ("medium": 500, "bold": 700)
    ```

=== "SASS"

    ```sass
    @use "sass:map"

    $font-weights: ("regular": 400, "medium": 500, "bold": 700)

    @debug map.deep-remove($font-weights, "regular")
    // ("medium": 500, "bold": 700)
    ```

Если `$keys` не пустой, следует по набору ключей, включая `$key` и исключая последний ключ в `$keys`, слева направо, чтобы найти вложенную карту для обновления.

Возвращает копию `$map`, где целевая карта не имеет значения, связанного с последним ключом в `$keys`.

=== "SCSS"

    ```scss
    @use "sass:map";

    $fonts: (
      "Helvetica": (
        "weights": (
          "regular": 400,
          "medium": 500,
          "bold": 700
        )
      )
    );

    @debug map.deep-remove($fonts, "Helvetica", "weights", "regular");
    // (
    //   "Helvetica": (
    //     "weights: (
    //       "medium": 500,
    //       "bold": 700
    //     )
    //   )
    // )
    ```

=== "SASS"

    ```sass
    @use "sass:map"

    $fonts: ("Helvetica": ("weights": ("regular": 400, "medium": 500, "bold": 700)))

    @debug map.deep-remove($fonts, "Helvetica", "weights", "regular")
    // (
    //   "Helvetica": (
    //     "weights: (
    //       "medium": 500,
    //       "bold": 700
    //     )
    //   )
    // )
    ```

## get

```scss
map.get($map, $key, $keys...)
map-get($map, $key, $keys...)
```

Если `$keys` пустой, возвращает значение в `$map`, связанное с `$key`.

Если `$map` не имеет значения, связанного с `$key`, возвращает [`null`](../values/null).

=== "SCSS"

    ```scss
    @use "sass:map";
    $font-weights: ("regular": 400, "medium": 500, "bold": 700);

    @debug map.get($font-weights, "medium"); // 500
    @debug map.get($font-weights, "extra-bold"); // null
    ```

=== "SASS"

    ```sass
    @use "sass:map"
    $font-weights: ("regular": 400, "medium": 500, "bold": 700)

    @debug map.get($font-weights, "medium") // 500
    @debug map.get($font-weights, "extra-bold") // null
    ```

Если `$keys` не пустой, следует по набору ключей, включая `$key` и исключая последний ключ в `$keys`, слева направо, чтобы найти вложенную карту для поиска.

Возвращает значение в целевой карте, связанное с последним ключом в `$keys`.

Возвращает [`null`](../values/null), если карта не имеет значения, связанного с ключом, или если какой-либо ключ в `$keys` отсутствует в карте или ссылается на значение, которое не является картой.

=== "SCSS"

    ```scss
    @use "sass:map";

    $fonts: (
      "Helvetica": (
        "weights": (
          "regular": 400,
          "medium": 500,
          "bold": 700
        )
      )
    );

    @debug map.get($fonts, "Helvetica", "weights", "regular"); // 400
    @debug map.get($fonts, "Helvetica", "colors"); // null
    ```

=== "SASS"

    ```sass
    @use "sass:map"

    $fonts: ("Helvetica": ("weights": ("regular": 400, "medium": 500, "bold": 700)))

    @debug map.get($fonts, "Helvetica", "weights", "regular") // 400
    @debug map.get($fonts, "Helvetica", "colors") // null
    ```

## has-key

```scss
map.has-key($map, $key, $keys...)
map-has-key($map, $key, $keys...) //=> булево значение
```

Если `$keys` пустой, возвращает, содержит ли `$map` значение, связанное с `$key`.

=== "SCSS"

    ```scss
    @use "sass:map";

    $font-weights: ("regular": 400, "medium": 500, "bold": 700);

    @debug map.has-key($font-weights, "regular"); // true
    @debug map.has-key($font-weights, "bolder"); // false
    ```

=== "SASS"

    ```sass
    @use "sass:map"

    $font-weights: ("regular": 400, "medium": 500, "bold": 700)

    @debug map.has-key($font-weights, "regular") // true
    @debug map.has-key($font-weights, "bolder") // false
    ```

Если `$keys` не пустой, следует по набору ключей, включая `$key` и исключая последний ключ в `$keys`, слева направо, чтобы найти вложенную карту для поиска.

Возвращает `true`, если целевая карта содержит значение, связанное с последним ключом в `$keys`.

Возвращает `false`, если не содержит, или если какой-либо ключ в `$keys` отсутствует в карте или ссылается на значение, которое не является картой.

=== "SCSS"

    ```scss
    @use "sass:map";

    $fonts: (
      "Helvetica": (
        "weights": (
          "regular": 400,
          "medium": 500,
          "bold": 700
        )
      )
    );

    @debug map.has-key($fonts, "Helvetica", "weights", "regular"); // true
    @debug map.has-key($fonts, "Helvetica", "colors"); // false
    ```

=== "SASS"

    ```sass
    @use "sass:map"

    $fonts: ("Helvetica": ("weights": ("regular": 400, "medium": 500, "bold": 700)))

    @debug map.has-key($fonts, "Helvetica", "weights", "regular") // true
    @debug map.has-key($fonts, "Helvetica", "colors") // false
    ```

## keys

```scss
map.keys($map)
map-keys($map) //=> список
```

Возвращает список всех ключей в `$map`, разделённых запятыми.

=== "SCSS"

    ```scss
    @use "sass:map";

    $font-weights: ("regular": 400, "medium": 500, "bold": 700);

    @debug map.keys($font-weights); // "regular", "medium", "bold"
    ```

=== "SASS"

    ```sass
    @use "sass:map"

    $font-weights: ("regular": 400, "medium": 500, "bold": 700)

    @debug map.keys($font-weights)  // "regular", "medium", "bold"
    ```

## merge

```scss
map.merge($map1, $map2)
map-merge($map1, $map2)
map.merge($map1, $keys..., $map2)
map-merge($map1, $keys..., $map2) //=> карта
```

!!! note "Примечание"

    На практике фактические аргументы для `map.merge($map1, $keys..., $map2)` передаются как `map.merge($map1, $args...)`. Они описаны здесь как `$map1, $keys..., $map2` только для целей объяснения.

Если `$keys` не переданы, возвращает новую карту со всеми ключами и значениями из `$map1` и `$map2`.

Если и `$map1`, и `$map2` имеют один и тот же ключ, значение из `$map2` имеет приоритет.

Все ключи в возвращаемой карте, которые также присутствуют в `$map1`, имеют тот же порядок, что и в `$map1`. Новые ключи из `$map2` появляются в конце карты.

=== "SCSS"

    ```scss
    @use "sass:map";

    $light-weights: ("lightest": 100, "light": 300);
    $heavy-weights: ("medium": 500, "bold": 700);

    @debug map.merge($light-weights, $heavy-weights);
    // ("lightest": 100, "light": 300, "medium": 500, "bold": 700)
    ```

=== "SASS"

    ```sass
    @use "sass:map"

    $light-weights: ("lightest": 100, "light": 300)
    $heavy-weights: ("medium": 500, "bold": 700)

    @debug map.merge($light-weights, $heavy-weights)
    // ("lightest": 100, "light": 300, "medium": 500, "bold": 700)
    ```

Если `$keys` не пустой, следует по `$keys`, чтобы найти вложенную карту, предназначенную для объединения. Если какой-либо ключ в `$keys` отсутствует в карте или ссылается на значение, которое не является картой, в этом месте устанавливается пустая карта.

Возвращает копию `$map1`, в которой целевая карта заменена новой картой, содержащей все ключи и значения как из целевой карты, так и из `$map2`.

=== "SCSS"

    ```scss
    @use "sass:map";

    $fonts: (
      "Helvetica": (
        "weights": (
          "lightest": 100,
          "light": 300
        )
      )
    );
    $heavy-weights: ("medium": 500, "bold": 700);

    @debug map.merge($fonts, "Helvetica", "weights", $heavy-weights);
    // (
    //   "Helvetica": (
    //     "weights": (
    //       "lightest": 100,
    //       "light": 300,
    //       "medium": 500,
    //       "bold": 700
    //     )
    //   )
    // )
    ```

=== "SASS"

    ```sass
    @use "sass:map"

    $fonts: ("Helvetica": ("weights": ("lightest": 100, "light": 300)))
    $heavy-weights: ("medium": 500, "bold": 700)

    @debug map.merge($fonts, "Helvetica", "weights", $heavy-weights)
    // (
    //   "Helvetica": (
    //     "weights": (
    //       "lightest": 100,
    //       "light": 300,
    //       "medium": 500,
    //       "bold": 700
    //     )
    //   )
    // )
    ```

## remove

```scss
map.remove($map, $keys...)
map-remove($map, $keys...) //=> карта
```

Возвращает копию `$map` без значений, связанных с `$keys`.

Если ключ в `$keys` не имеет связанного значения в `$map`, он игнорируется.

=== "SCSS"

    ```scss
    @use "sass:map";

    $font-weights: ("regular": 400, "medium": 500, "bold": 700);

    @debug map.remove($font-weights, "regular"); // ("medium": 500, "bold": 700)
    @debug map.remove($font-weights, "regular", "bold"); // ("medium": 500)
    @debug map.remove($font-weights, "bolder");
    // ("regular": 400, "medium": 500, "bold": 700)
    ```

=== "SASS"

    ```sass
    @use "sass:map"

    $font-weights: ("regular": 400, "medium": 500, "bold": 700)

    @debug map.remove($font-weights, "regular")  // ("medium": 500, "bold": 700)
    @debug map.remove($font-weights, "regular", "bold")  // ("medium": 500)
    @debug map.remove($font-weights, "bolder")
    // ("regular": 400, "medium": 500, "bold": 700)
    ```

## set

```scss
map.set($map, $key, $value)
map.set($map, $keys..., $key, $value) //=> карта
```

!!! note "Примечание"

    На практике фактические аргументы для `map.set($map, $keys..., $key, $value)` передаются как `map.set($map, $args...)`. Они описаны здесь как `$map, $keys..., $key, $value` только для целей объяснения.

Если `$keys` не переданы, возвращает копию `$map` со значением `$value` для ключа `$key`.

=== "SCSS"

    ```scss
    @use "sass:map";

    $font-weights: ("regular": 400, "medium": 500, "bold": 700);

    @debug map.set($font-weights, "regular", 300);
    // ("regular": 300, "medium": 500, "bold": 700)
    ```

=== "SASS"

    ```sass
    @use "sass:map"

    $font-weights: ("regular": 400, "medium": 500, "bold": 700)

    @debug map.set($font-weights, "regular", 300)
    // ("regular": 300, "medium": 500, "bold": 700)
    ```

Если `$keys` переданы, следует по `$keys`, чтобы найти вложенную карту для обновления. Если какой-либо ключ в `$keys` отсутствует в карте или ссылается на значение, которое не является картой, устанавливает значение в этом ключе в пустую карту.

Возвращает копию `$map` с установленным значением `$value` для ключа `$key` в целевой карте.

=== "SCSS"

    ```scss
    @use "sass:map";

    $fonts: (
      "Helvetica": (
        "weights": (
          "regular": 400,
          "medium": 500,
          "bold": 700
        )
      )
    );

    @debug map.set($fonts, "Helvetica", "weights", "regular", 300);
    // (
    //   "Helvetica": (
    //     "weights": (
    //       "regular": 300,
    //       "medium": 500,
    //       "bold": 700
    //     )
    //   )
    // )
    ```

=== "SASS"

    ```sass
    @use "sass:map"

    $fonts: ("Helvetica": ("weights": ("regular": 400, "medium": 500, "bold": 700)))

    @debug map.set($fonts, "Helvetica", "weights", "regular", 300)
    // (
    //   "Helvetica": (
    //     "weights": (
    //       "regular": 300,
    //       "medium": 500,
    //       "bold": 700
    //     )
    //   )
    // )
    ```

## values

```scss
map.values($map)
map-values($map) //=> список
```

Возвращает список значений из `$map`, разделённых запятыми.

=== "SCSS"

    ```scss
    @use "sass:map";

    $font-weights: ("regular": 400, "medium": 500, "bold": 700);

    @debug map.values($font-weights); // 400, 500, 700
    ```

=== "SASS"

    ```sass
    @use "sass:map"

    $font-weights: ("regular": 400, "medium": 500, "bold": 700)

    @debug map.values($font-weights)  // 400, 500, 700
    ```
