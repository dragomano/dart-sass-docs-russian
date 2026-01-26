---
title: Экспорты по умолчанию
icon: lucide/newspaper
---

> По умолчанию Node.js позволяет [загружать CommonJS-модули](https://nodejs.org/docs/latest/api/modules.html#modules-commonjs-modules) из ECMAScript-модулей с помощью синтаксиса `import sass from 'sass'`. Это теперь устарело; пользователи ESM должны использовать `import * as sass from 'sass'`.

Исторически Dart Sass был доступен только как CommonJS-модуль. Это означало, что любой, кто использовал его из проекта, использующего встроенную поддержку ECMAScript-модулей Node.js, мог загружать его так, как будто он предоставлял [экспорт по умолчанию](https://developer.mozilla.org/en-US/docs/web/javascript/reference/statements/export#using_the_default_export):

```js
import sass from 'sass'; // Не делай так больше
```

Это никогда не предполагалось командой Sass, и это не соответствовало объявлениям типов, предоставляемым с пакетом, но это _действительно_ работало. Мы решили удалить эту поддержку в Dart Sass 2.0.0 и требовать, чтобы пользователи ECMAScript-модулей использовали только именованные экспорты пакета:

```js
import * as sass from 'sass'; // Делайте так
```

## Переходный период {#transition-period}

До Dart Sass 2.0.0 мы продолжим поддерживать загрузку пользователями экспорта по умолчанию Sass. В первый раз, когда будет получен доступ к любым свойствам экспорта по умолчанию, будет выдано предупреждение об устаревании в `console.error()`. Чтобы избежать этой ошибки, используйте `import * as sass from 'sass'`.
