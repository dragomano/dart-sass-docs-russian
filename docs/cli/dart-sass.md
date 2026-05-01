---
title: Интерфейс командной строки Dart Sass
icon: lucide/square-chevron-right
---

## Использование {#usage}

Исполняемый файл Dart Sass может быть запущен в одном из двух режимов.

### Режим «один к одному» {#one-to-one-mode}

```sh
sass <input.scss> [output.css]
```

Режим «один к одному» компилирует один входной файл (`input.scss`) в одно выходное расположение (`output.css`). Если выходное расположение не указано, скомпилированный CSS выводится в терминал.

Входной файл парсится как [SCSS](../syntax#scss), если его расширение `.scss`, как [синтаксис с отступами](../syntax#the-indented-syntax), если его расширение `.sass`, или как [простой CSS](../at-rules/import/#importing-css), если его расширение `.css`. Если у него нет одного из этих трёх расширений или если он поступает из стандартного ввода, он по умолчанию парсится как SCSS. Это можно контролировать с помощью флага [`--indented`](#indented).

Специальная строка `-` может быть передана вместо входного файла, чтобы указать Sass читать входной файл из [стандартного ввода](https://en.wikipedia.org/wiki/Standard_streams#Standard_input_(stdin)). Sass будет по умолчанию парсить его как SCSS, если не передан флаг [`--indented`](#indented).

### Режим «многие ко многим» {#many-to-many-mode}

```sh
sass [<input.scss>:<output.css>] [<input/>:<output/>]...
```

Режим «многие ко многим» компилирует один или несколько входных файлов в один или несколько выходных файлов. Входные файлы отделяются от выходных двоеточиями. Он также может компилировать все файлы Sass в директории в файлы CSS с теми же именами в другой директории.

```sh
​# Compiles style.scss to style.css.
$ sass style.scss:style.css

​# Compiles light.scss and dark.scss to light.css and dark.css.
$ sass light.scss:light.css dark.scss:dark.css

​# Compiles all Sass files in themes/ to CSS files in public/css/.
$ sass themes:public/css
```

При компиляции целых директорий Sass игнорирует [фрагменты](../at-rules/import/#partials), имена которых начинаются с `_`. Вы можете использовать фрагменты для разделения таблиц стилей без создания множества ненужных выходных файлов.

## Опции {#options}

### Ввод и вывод {#input-and-output}

Эти опции контролируют, как Sass загружает свои входные файлы и как он создаёт выходные файлы.

#### `--stdin` {#stdin}

Этот флаг является альтернативным способом указать Sass, что он должен читать входной файл из [стандартного ввода](https://en.wikipedia.org/wiki/Standard_streams#Standard_input_(stdin)). Когда он передан, входной файл не может быть указан.

```sh
$ echo "h1 {font-size: 40px}" | sass --stdin h1.css
$ echo "h1 {font-size: 40px}" | sass --stdin
h1 {
  font-size: 40px;
}
```

Флаг `--stdin` не может использоваться с [режимом «многие ко многим»](#many-to-many-mode).

#### `--indented` {#indented}

Этот флаг указывает Sass парсить входной файл как [синтаксис с отступами](../syntax#the-indented-syntax). Если он используется в [режиме «многие ко многим»](#many-to-many-mode), все входные файлы парсятся как синтаксис с отступами, хотя синтаксис файлов, которые они [подключают](../at-rules/use), будет определяться как обычно. Обратный вариант, `--no-indented`, может использоваться для принудительного парсинга всех входных файлов как [SCSS](../syntax#scss).

Флаг `--indented` наиболее полезен, когда входной файл поступает из [стандартного ввода](https://en.wikipedia.org/wiki/Standard_streams#Standard_input_(stdin)), поэтому его синтаксис не может быть определён автоматически.

```sh
$ echo -e 'h1\n  font-size: 40px' | sass --indented -
h1 {
  font-size: 40px;
}
```

#### `--load-path` {#load-path}

Эта опция (сокращённо `-I`) добавляет дополнительный [путь загрузки](../at-rules/use#load-paths) для Sass для поиска таблиц стилей. Её можно передавать несколько раз, чтобы указать несколько путей загрузки. Более ранние пути загрузки имеют приоритет над более поздними.

```sh
$ sass --load-path=node_modules/bootstrap/dist/css style.scss style.css
```

#### `--pkg-importer=node` {#pkg-importer-node}

Эта опция (сокращённо `-p node`) добавляет [Node.js импортер `pkg:`](../at-rules/use#node-js-package-importer) в конец пути загрузки, чтобы таблицы стилей могли загружать зависимости, используя алгоритм разрешения модулей Node.js.

Поддержка дополнительных встроенных импортеров `pkg:` может быть добавлена в будущем.

```sh
$ sass --pkg-importer=node style.scss style.css
```

#### `--style` {#style}

Эта опция (сокращённо `-s`) управляет стилем вывода результирующего CSS. Dart Sass поддерживает два стиля вывода:

- `expanded` (по умолчанию) записывает каждый селектор и объявление на отдельной строке.
- `compressed` удаляет как можно больше лишних символов и записывает всю таблицу стилей в одну строку.

```sh
$ sass --style=expanded style.scss
h1 {
  font-size: 40px;
}

$ sass --style=compressed style.scss
h1{font-size:40px}
```

#### `--no-charset` {#no-charset}

Этот флаг указывает Sass никогда не выводить объявление `@charset` или UTF-8 [метку порядка байтов](https://en.wikipedia.org/wiki/Byte_order_mark#UTF-8). По умолчанию, или если передан `--charset`, Sass вставит либо объявление `@charset` (в режиме расширенного вывода), либо метку порядка байтов (в режиме сжатого вывода), если таблица стилей содержит символы, отличные от ASCII.

```sh
$ echo 'h1::before {content: "👭"}' | sass --no-charset
h1::before {
  content: "👭";
}

$ echo 'h1::before {content: "👭"}' | sass --charset
@charset "UTF-8";
h1::before {
  content: "👭";
}
```

#### `--error-css` {#error-css}

Этот флаг указывает Sass, выводить ли CSS-файл при возникновении ошибки во время компиляции. Этот CSS-файл описывает ошибку в комментарии _и_ в свойстве `content` элемента `body::before`, чтобы вы могли увидеть сообщение об ошибке в браузере, не переключаясь обратно в терминал.

По умолчанию вывод CSS с ошибками включён, если вы компилируете хотя бы в один файл на диске (в отличие от стандартного вывода). Вы можете явно передать `--error-css`, чтобы включить его даже при компиляции в стандартный вывод, или `--no-error-css`, чтобы отключить его везде. Когда он отключён, флаги [`--update`](#update) и [`--watch`](#watch) будут удалять CSS-файлы при возникновении ошибки.

```sh
$ sass --error-css style.scss style.css
/* Error: Incompatible units em and px.
 *   ,
 * 1 | $width: 15px + 2em;
 *   |         ^^^^^^^^^^
 *   '
 *   test.scss 1:9  root stylesheet */

body::before {
  font-family: "Source Code Pro", "SF Mono", Monaco, Inconsolata, "Fira Mono",
      "Droid Sans Mono", monospace, monospace;
  white-space: pre;
  display: block;
  padding: 1em;
  margin-bottom: 1em;
  border-bottom: 2px solid black;
  content: "Error: Incompatible units em and px.\a   \2577 \a 1 \2502  $width: 15px + 2em;\a   \2502          ^^^^^^^^^^\a   \2575 \a   test.scss 1:9  root stylesheet";
}
Error: Incompatible units em and px.
  ╷
1 │ $width: 15px + 2em;
  │         ^^^^^^^^^^
  ╵
  test.scss 1:9  root stylesheet
```

#### `--update` {#update}

Если передан флаг `--update`, Sass будет компилировать только те таблицы стилей, зависимости которых были изменены позже, чем был создан соответствующий CSS-файл. Он также будет выводить статусные сообщения при обновлении таблиц стилей.

```sh
$ sass --update themes:public/css
Compiled themes/light.scss to public/css/light.css.
```

### Карты исходного кода {#source-maps}

Карты исходного кода — это файлы, которые сообщают браузерам или другим инструментам, использующим CSS, как этот CSS соответствует файлам Sass, из которых он был сгенерирован. Они позволяют просматривать и даже редактировать файлы Sass в браузерах. Смотрите инструкции по использованию карт исходного кода в [Chrome](https://developers.google.com/web/tools/chrome-devtools/javascript/source-maps) и [Firefox](https://developer.mozilla.org/en-US/docs/Tools/Style_Editor#Source_map_support).

Dart Sass по умолчанию генерирует карты исходного кода для каждого создаваемого CSS-файла.

#### `--no-source-map` {#no-source-map}

Если передан флаг `--no-source-map`, Sass не будет генерировать карты исходного кода. Его нельзя передавать вместе с другими опциями карт исходного кода.

```sh
$ sass --no-source-map style.scss style.css
```

#### `--source-map-urls` {#source-map-urls}

Эта опция контролирует, как карты исходного кода, которые генерирует Sass, ссылаются обратно на файлы Sass, которые участвовали в создании CSS. Dart Sass поддерживает два типа URL:

- `relative` (по умолчанию) использует относительные URL от расположения файла карты исходного кода до расположения исходного файла Sass.
- `absolute` использует абсолютные [`file:` URL](https://en.wikipedia.org/wiki/File_URI_scheme) исходных файлов Sass. Обратите внимание, что абсолютные URL будут работать только на том же компьютере, на котором был скомпилирован CSS.

```sh
​# Generates a URL like "../sass/style.scss".
$ sass --source-map-urls=relative sass/style.scss css/style.css

​# Generates a URL like "file:///home/style-wiz/sassy-app/sass/style.scss".
$ sass --source-map-urls=absolute sass/style.scss css/style.css
```

#### `--embed-sources` {#embed-sources}

Этот флаг указывает Sass встроить всё содержимое файлов Sass, которые участвовали в создании CSS, в карту исходного кода. Это может привести к созданию очень больших карт исходного кода, но гарантирует, что исходный код будет доступен на любом компьютере независимо от того, как обслуживается CSS.

```sh
$ sass --embed-sources sass/style.scss css.style.css
```

#### `--embed-source-map` {#embed-source-map}

Этот флаг указывает Sass встроить содержимое файла карты исходного кода в сгенерированный CSS, вместо создания отдельного файла и ссылки на него из CSS.

```sh
$ sass --embed-source-map sass/style.scss css.style.css
```

### Другие опции {#other-options}

#### `--watch` {#watch}

Этот флаг (сокращённо `-w`) действует как флаг [`--update`](#update), но после первого раунда компиляции Sass остаётся запущенным и продолжает компилировать таблицы стилей всякий раз, когда они или их зависимости изменяются.

Sass отслеживает только те директории, которые вы передаёте как есть в командной строке, родительские директории имён файлов, которые вы передаёте в командной строке, и пути загрузки. Он не отслеживает дополнительные директории на основе правил `@import`/`@use`/`@forward` файла.

```sh
$ sass --watch themes:public/css
Compiled themes/light.scss to public/css/light.css.

​# Then when you edit themes/dark.scss...
Compiled themes/dark.scss to public/css/dark.css.
```

#### `--poll` {#poll}

Этот флаг, который может быть передан только вместе с `--watch`, указывает Sass вручную проверять изменения в исходных файлах время от времени, вместо того чтобы полагаться на систему уведомлений операционной системы о том, что что-то изменилось. Это может быть необходимо, если вы редактируете Sass на удалённом диске, где система уведомлений операционной системы не работает.

```sh
$ sass --watch --poll themes:public/css
Compiled themes/light.scss to public/css/light.css.

​# Then when you edit themes/dark.scss...
Compiled themes/dark.scss to public/css/dark.css.
```

#### `--stop-on-error` {#stop-on-error}

Этот флаг указывает Sass немедленно прекратить компиляцию при обнаружении ошибки, вместо того чтобы пытаться скомпилировать другие файлы Sass, которые могут не содержать ошибок. Он особенно полезен в [режиме «многие ко многим»](#many-to-many-mode).

```sh
$ sass --stop-on-error themes:public/css
Error: Expected expression.
   ╷
42 │ h1 {font-face: }
   │                ^
   ╵
  themes/light.scss 42:16  root stylesheet
```

#### `--interactive` {#interactive}

Этот флаг (сокращённо `-i`) указывает Sass запуститься в интерактивном режиме, где вы можете писать [SassScript-выражения](../syntax/structure#expressions) и видеть их результаты. Интерактивный режим также поддерживает [переменные](../variables) и [правила `@use`](../at-rules/use).

```sh
$ sass --interactive
>> 1px + 1in
97px
>> @use "sass:map"
>> $map: ("width": 100px, "height": 70px)
("width": 100px, "height": 70px)
>> map.get($map, "width")
100px
```

#### `--color` {#color}

Этот флаг (сокращённо `-c`) указывает Sass выводить [цвета терминала](https://en.wikipedia.org/wiki/ANSI_escape_code#Colors), а обратный флаг `--no-color` запрещает вывод цветов. По умолчанию Sass будет выводить цвета, если обнаружит, что запущен в терминале, поддерживающем их.

```sh
$ sass --color style.scss style.css
Error: Incompatible units em and px.
  ╷
1 │ $width: 15px + 2em
  │         ^^^^^^^^^^
  ╵
  style.scss 1:9  root stylesheet

$ sass --no-color style.scss style.css
Error: Incompatible units em and px.
  ╷
1 │ $width: 15px + 2em
  │         ^^^^^^^^^^
  ╵
  style.scss 1:9  root stylesheet
```

#### `--no-unicode` {#no-unicode}

Этот флаг указывает Sass выводить в терминал только ASCII-символы в составе сообщений об ошибках. По умолчанию, или если передан `--unicode`, Sass будет выводить символы, отличные от ASCII, для этих сообщений. Этот флаг не влияет на вывод CSS.

```sh
$ sass --no-unicode style.scss style.css
Error: Incompatible units em and px.
  ,
1 | $width: 15px + 2em;
  |         ^^^^^^^^^^
  '
  test.scss 1:9  root stylesheet

$ sass --unicode style.scss style.css
Error: Incompatible units em and px.
  ╷
1 │ $width: 15px + 2em;
  │         ^^^^^^^^^^
  ╵
  test.scss 1:9  root stylesheet
```

#### `--verbose` {#verbose}

Этот флаг указывает Sass выводить _все_ предупреждения об устаревании при компиляции. По умолчанию Sass выводит только пять предупреждений для данного типа устаревания при использовании устаревших функций и подавляет любые предупреждения сверх этого.

```sh
$ sass --verbose style.scss style.css
```

#### `--quiet` {#quiet}

Этот флаг (сокращённо `-q`) указывает Sass не выводить никаких предупреждений при компиляции. По умолчанию Sass выводит предупреждения при использовании устаревших функций или при встрече с [правилом `@warn`](../at-rules/warn). Он также подавляет [правило `@debug`](../at-rules/debug).

```sh
$ sass --quiet style.scss style.css
```

#### `--quiet-deps` {#quiet-deps}

Этот флаг указывает Sass не выводить предупреждения об устаревании, которые исходят из зависимостей. Он считает «зависимостью» любой файл, который транзитивно импортирован через [путь загрузки](../at-rules/use#load-paths). Этот флаг не влияет на [правило `@warn`](../at-rules/warn) или [правило `@debug`](../at-rules/debug).

```sh
$ sass --load-path=node_modules --quiet-deps style.scss style.css
```

#### `--fatal-deprecation` {#fatal-deprecation}

Эта опция указывает Sass воспринимать определённый тип предупреждений об устаревании как ошибку. Например, следующая команда заставит Sass рассматривать предупреждения об устаревании для использования `/` как операции деления как ошибки.

```sh
$ sass --fatal-deprecation=slash-div style.scss style.css
Error: Using / for division outside of calc() is deprecated and will be removed in Dart Sass 2.0.0.

Recommendation: math.div(4, 2) or calc(4 / 2)

More info and automated migrator: ../breaking-changes/slash-div

This is only an error because you've set the slash-div deprecation to be fatal.
Remove this setting if you need to keep using this feature.
  ╷
1 │ a { b: (4/2); }
  │         ^^^
  ╵
  style.scss 1:9  root stylesheet
```

Поддерживаются следующие идентификаторы устареваний:

| Идентификатор                   | Устарела с версии | Описание                                                                              |
|---------------------------------|-------------------|---------------------------------------------------------------------------------------|
| call-string                     | 0.0.0             | Передача строки напрямую в `meta.call()`                                              |
| elseif                          | 1.3.2             | Правило `@elseif`                                                                     |
| moz-document                    | 1.7.2             | Правило `@-moz-document`                                                              |
| relative-canonical              | 1.14.2            | Импорт с использованием относительных канонических URL                                |
| new-global                      | 1.17.2            | Объявление новых переменных с модификатором `!global`                                 |
| color-module-compat             | 1.23.0            | Использование функций модуля `color` вместо обычных CSS-функций                       |
| slash-div                       | 1.33.0            | Оператор `/` для деления                                                              |
| bogus-combinators               | 1.54.0            | Начальные, конечные и повторяющиеся комбинаторы                                       |
| strict-unary                    | 1.55.0            | Неоднозначные операторы `+` и `-`                                                     |
| function-units                  | 1.56.0            | Передача некорректных единиц измерения во встроенные функции                          |
| duplicate-var-flags             | 1.62.0            | Многократное использование `!default` или `!global` для одной переменной              |
| null-alpha                      | 1.62.3            | Передача `null` в качестве альфа-канала в JS API                                      |
| abs-percent                     | 1.65.0            | Передача процентов в функцию `abs()`                                                  |
| fs-importer-cwd                 | 1.73.0            | Использование текущей рабочей директории как неявного пути загрузки                   |
| feature-exists                  | 1.78.0            | Функция `meta.feature-exists()`                                                       |
| color-4-api                     | 1.79.0            | Некоторые способы использования встроенных функций модуля `sass:color`                |
| color-functions                 | 1.79.0            | Использование глобальных цветовых функций вместо модуля `sass:color`                  |
| legacy-js-api                   | 1.79.0            | Устаревший JavaScript API                                                             |
| import                          | 1.80.0            | Правило `@import`                                                                     |
| global-builtin                  | 1.80.0            | Глобальные встроенные функции, которые теперь доступны только через модули `sass:`    |
| compile-string-relative-url     | 1.88.0            | Передача относительного URL в функцию `compileString()`                               |
| misplaced-rest                  | 1.91.0            | Rest-параметр перед позиционным или именованным параметром                            |
| with-private                    | 1.92.0            | Настройка приватных переменных в `@use`, `@forward` или `load-css()`                  |
| if-function                     | 1.95.0            | Функция `if($condition, $if-true, $if-false)` в Sass                                  |

В качестве альтернативы можно передать версию Dart Sass, чтобы считать ошибками все предупреждения об устаревании, которые существовали в этой версии. Например, `--fatal-deprecation=1.33.0` будет рассматривать все устаревания из приведённой выше таблицы, вплоть до и включая `slash-div`, как ошибки, но оставит любые более новые устаревания предупреждениями.

Следующие идентификаторы устареваний устарели и больше не оказывают никакого эффекта:

| Идентификатор            | Устарела с версии | Устарела полностью с версии | Описание                                                   |
|--------------------------|-------------------|-----------------------------|------------------------------------------------------------|
| css-function-mixin       | 1.76.0            | 1.94.0                      | Имена функций и миксинов, начинающиеся с `--`              |
| mixed-decls              | 1.77.7            | 1.92.0                      | Объявления свойств после или между вложенными правилами    |
| type-function            | 1.86.0            | 1.92.0                      | Функции с именем `type`                                    |

#### `--future-deprecation` {#future-deprecation}

Эта опция указывает Sass заранее включить будущий тип предупреждений об устаревании, выводя предупреждения, даже если устаревание ещё не активно. Эту опцию можно комбинировать с `--fatal-deprecation`, чтобы вместо предупреждений выдавать ошибки для будущего устаревания.

```sh
$ sass --future-deprecation=import style.scss style.css
Deprecation Warning on line 1, column 9 of style.scss:
Sass @import rules will be deprecated in the future.
Remove the --future-deprecation=import flag to silence this warning for now.
  ╷
1 │ @import 'dependency';
  │         ^^^^^^^^^^^^
  ╵
```

В последней версии Dart Sass нет будущих устареваний.

#### `--silence-deprecation` {#silence-deprecation}

Эта опция указывает Sass подавить определённый тип предупреждений об устаревании, если вы хотите временно игнорировать устаревание. Сюда можно передать любое из устареваний, перечисленных в разделе `--fatal-deprecation` выше, однако передача версии не поддерживается.

```sh
$ sass --silence-deprecation=slash-div style.scss style.css
```

#### `--trace` {#trace}

Этот флаг указывает Sass печатать полную трассировку стека Dart или JavaScript при возникновении ошибки. Он используется командой Sass для отладки ошибок.

```sh
$ sass --trace style.scss style.css
Error: Expected expression.
   ╷
42 │ h1 {font-face: }
   │                ^
   ╵
  themes/light.scss 42:16  root stylesheet

package:sass/src/visitor/evaluate.dart 1846:7                        _EvaluateVisitor._addExceptionSpan
package:sass/src/visitor/evaluate.dart 1128:12                       _EvaluateVisitor.visitBinaryOperationExpression
package:sass/src/ast/sass/expression/binary_operation.dart 39:15     BinaryOperationExpression.accept
package:sass/src/visitor/evaluate.dart 1097:25                       _EvaluateVisitor.visitVariableDeclaration
package:sass/src/ast/sass/statement/variable_declaration.dart 50:15  VariableDeclaration.accept
package:sass/src/visitor/evaluate.dart 335:13                        _EvaluateVisitor.visitStylesheet
package:sass/src/visitor/evaluate.dart 323:5                         _EvaluateVisitor.run
package:sass/src/visitor/evaluate.dart 81:10                         evaluate
package:sass/src/executable/compile_stylesheet.dart 59:9             compileStylesheet
package:sass/src/executable.dart 62:15                               main
```

#### `--help` {#help}

Этот флаг (сокращённо `-h`) выводит краткое описание этой документации.

```sh
$ sass --help
Compile Sass to CSS.

Usage: sass <input.scss> [output.css]
       sass <input.scss>:<output.css> <input/>:<output/>

...
```

#### `--version` {#version}

Этот флаг выводит текущую версию Sass.

```sh
$ sass --version
1.97.2
```
