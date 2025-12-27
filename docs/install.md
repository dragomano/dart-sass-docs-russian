---
title: Установка Sass
icon: lucide/hard-drive-download
---

## Приложения {#applications}

Существует множество приложений, которые помогут вам начать работу с Sass за несколько минут на Mac, Windows и Linux. Большинство приложений можно скачать бесплатно, но некоторые из них платные.

## Библиотеки {#libraries}

Команда Sass поддерживает два пакета Node.js для Sass, оба из которых поддерживают [стандартный JavaScript API](https://sass-lang.com/documentation/js-api/). Пакет [`sass`](https://www.npmjs.com/package/sass) написан на чистом JavaScript, он немного медленнее, но может быть установлен на всех платформах, которые поддерживает Node.js. Пакет [`sass-embedded`](https://www.npmjs.com/package/sass-embedded) оборачивает JS API вокруг виртуальной машины Dart, поэтому он быстрее, но поддерживает только Windows, Mac OS и Linux.

Также существуют поддерживаемые сообществом обёртки для следующих языков:

- [Ruby](https://github.com/sass-contrib/sass-embedded-host-ruby#readme)
- [Swift](https://github.com/johnfairh/swift-sass#readme)
- [Java](https://mvnrepository.com/artifact/de.larsgrefer.sass), включая:
  - [Плагин для Gradle](https://docs.freefair.io/gradle-plugins/current/reference/#_embedded_sass).
  - Легковесный [плагин Maven, оборачивающий Sass CLI](https://github.com/HebiRobotics/sass-cli-maven-plugin). Он указывает используемую версию Sass. Аргументы CLI передаются через список `<args>`.
  - Полнофункциональный [плагин Maven, оборачивающий Dart Sass](https://github.com/cleydyr/dart-sass-maven-plugin). Он включает фиксированную версию `dart-sass`. Аргументы CLI представлены как параметры Maven.

## Командная строка {#command-line}

После установки Sass в командной строке вы сможете запускать исполняемый файл `sass` для компиляции файлов `.sass` и `.scss` в файлы `.css`. Например:

```shell
sass source/stylesheets/index.scss build/stylesheets/index.css
```

Сначала установите Sass, используя один из вариантов ниже, затем выполните `sass --version`, чтобы убедиться в правильности установки. Если всё прошло успешно, это будет включать `{{ releases['dart-sass'].version }}`. Вы также можете выполнить `sass --help` для получения дополнительной информации об интерфейсе командной строки.

После завершения настройки **идите и экспериментируйте**. Если вы совсем новичок в Sass, мы подготовили некоторые ресурсы, которые помогут вам научиться довольно быстро.

[Узнать больше о Sass](tutorial){ .md-button .md-button--primary }

### Автономная установка {#install-standalone}

Вы можете установить Sass на Windows, Mac или Linux, скачав соответствующий пакет для вашей операционной системы и архитектуры процессора со [страницы релизов на GitHub](https://github.com/sass/dart-sass/releases/latest). Мы предоставляем ZIP-файл для Windows и файл `.tar.gz` для Mac и Linux.

После загрузки архива извлеките его содержимое. Извлечённые файлы содержат каталог с именем `dart-sass`, который содержит исполняемый скрипт `sass` (или `sass.bat` в Windows). Нет никаких внешних зависимостей и ничего другого, что нужно устанавливать.

Чтобы запускать Sass из любого места на вашем компьютере, вам нужно добавить извлечённый каталог `dart-sass` в системную переменную `PATH`. Для получения дополнительной информации о том, как это сделать, см. [это руководство](https://katiek2.github.io/path-doc/).

### Установка через менеджер пакетов для JavaScript {#install-npm}

Если вы используете Node.js, вы также можете установить Sass с помощью используемого менеджера пакетов, выполнив

=== ":simple-npm: npm"
    ```shell
    npm install -g sass
    ```

=== ":simple-pnpm: pnpm"
    ```shell
    pnpm add -g sass
    ```

=== ":simple-yarn: Yarn"
    ```shell
    yarn add -g sass
    ```

=== ":simple-bun: Bun"
    ```shell
    bun add -g sass
    ```

**Однако обратите внимание**, что это установит реализацию Sass на чистом JavaScript, которая работает несколько медленнее, чем другие варианты, перечисленные здесь. Но у неё тот же интерфейс, поэтому будет легко заменить её другой реализацией позже, если вам понадобится немного больше скорости!

### Установка на Windows (Chocolatey) {#install-choco}

Если вы используете [менеджер пакетов Chocolatey](https://chocolatey.org/) для Windows, вы можете установить Dart Sass, выполнив

```shell
choco install sass
```

### Установка на Mac OS X или Linux (Homebrew) {#install-brew}

Если вы используете [менеджер пакетов Homebrew](https://brew.sh/) для Mac OS X или Linux, вы можете установить Dart Sass, выполнив

```shell
brew install sass/sass/sass
```
