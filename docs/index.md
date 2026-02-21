---
title: Введение
icon: lucide/rocket
---

> Dart Sass — это основная реализация Sass, что означает, что она получает новые функции раньше любой другой реализации. Она быстрая, легко устанавливается и компилируется в чистый JavaScript, что упрощает интеграцию в современные рабочие процессы веб-разработки. Узнайте больше или помогите в её разработке на [GitHub](https://github.com/sass/dart-sass).

## Командная строка {#command-line}

Автономный исполняемый файл Dart Sass использует молниеносно быструю виртуальную машину Dart для компиляции ваших таблиц стилей. Чтобы установить Dart Sass через командную строку, ознакомьтесь с [инструкциями по установке](install). После запуска вы можете использовать её для компиляции файлов:

```shell
sass source/index.scss css/index.css
```

Для получения дополнительной информации об интерфейсе командной строки выполните

```shell
sass --help
```

### Библиотека Dart {#dart-library}

Вы также можете использовать Dart Sass как библиотеку Dart, чтобы получить скорость виртуальной машины Dart плюс возможность определять собственные функции и импортёры. Чтобы добавить её в существующий проект:

1. [Установите Dart SDK][install]. Убедитесь, что его каталог `bin` находится [в вашем `PATH`][path].

   [install]: https://dart.dev/get-dart
   [path]: https://katiek2.github.io/path-doc/

2. Создайте файл `pubspec.yaml` следующего вида:

```yaml
name: my_project

environment:
  sdk: ^3.6.0

dev_dependencies:
  sass: ^{{ releases['dart-sass'].version }}
```

3. Выполните `dart pub get`.

4. Создайте файл `compile-sass.dart` следующего вида:

```dart
import 'dart:io';
import 'package:sass/sass.dart' as sass;

void main(List<String> arguments) {
  var result = sass.compileToResult(arguments[0]);
  new File(arguments[1]).writeAsStringSync(result.css);
}
```

5. Теперь вы можете использовать это для компиляции файлов:

```shell
dart compile-sass.dart styles.scss styles.css
```

6. Узнайте больше о [написании кода на Dart][dart] (это легко!) и о [Dart API Sass][sass].

[dart]: https://www.dartlang.org/guides/language/language-tour
[sass]: https://pub.dev/documentation/sass/latest/sass/compileToResult.html

## Библиотека JavaScript {#java-script-library}

Dart Sass также распространяется в виде пакетов [`sass`](https://www.npmjs.com/package/sass) и [`sass-embedded`](https://www.npmjs.com/package/sass-embedded) на чистом JavaScript в npm. Версия на чистом JS медленнее, чем автономный исполняемый файл, но её легко интегрировать в существующие рабочие процессы, и она позволяет определять пользовательские функции и импортёры на JavaScript. Вы можете добавить её в свой проект с помощью `npm install --save-dev sass` и подключить как библиотеку через `require()`:

```js
const sass = require('sass');

const result = sass.compile('style.scss');
console.log(result.css);

// ИЛИ

const result = await sass.compileAsync('style.scss');
console.log(result.css);
```

При установке через npm Dart Sass поддерживает [совершенно новый JavaScript API](https://sass-lang.com/documentation/js-api/), а также [устаревший API](https://sass-lang.com/documentation/js-api/#md:legacy-api), полностью совместимый со старым API Node Sass. Обратите внимание, что при использовании пакета `sass` синхронные функции API более чем в два раза быстрее асинхронного API из-за накладных расходов асинхронных обратных вызовов.

## Embedded Dart Sass {#embedded-dart-sass}

Dart Sass также поддерживает [протокол Embedded Sass](https://github.com/sass/sass/blob/main/spec/embedded-protocol.md#the-embedded-sass-protocol), который позволяет любому языку программирования напрямую взаимодействовать с виртуальной машиной Dart для выполнения компиляции Sass, включая поддержку пользовательских функций и импортёров. Это имеет два основных преимущества:

1. Это упрощает создание библиотеки-обёртки для Dart Sass для любого языка программирования, который может запускать подпроцесс.

2. Виртуальная машина Dart очень быстрая, поэтому это обеспечивает существенный прирост производительности даже для JavaScript, где доступен нативный пакет `sass`.

Доступны следующие пакеты-обёртки Embedded Sass. Если у вас есть ещё один для добавления, пожалуйста, [отправьте пулреквест](https://github.com/sass/sass-site/edit/main/source/dart-sass.md)!

* **Node.js**: Пакет [`sass-embedded`](https://www.npmjs.com/package/sass-embedded) поддерживается командой Sass и поддерживает тот же [официальный JavaScript API Sass](https://sass-lang.com/documentation/js-api/), что и нативный JS-пакет `sass`.

* **Go**: Пакет [`github.com/bep/godartsass`](https://github.com/bep/godartsass) запускает Embedded Sass и поддерживает генератор статических сайтов [Hugo](https://gohugo.io/).

* **Java**: Пакет [`de.larsgrefer.sass`](https://mvnrepository.com/artifact/de.larsgrefer.sass) запускает Embedded Sass в Java.

* **Ruby**: Пакет [`sass-embedded`](https://rubygems.org/gems/sass-embedded) для `gem` поддерживается частым контрибьютором なつき.

* **Rust**: Пакет [`sass-embedded`](https://crates.io/crates/sass-embedded) для `crate` запускает Embedded Sass в Rust.

* **PHP**: Пакет [`bugo/sass-embedded-php`](https://github.com/dragomano/sass-embedded-php) добавляет PHP-обёртку для компиляции SCSS в CSS.
