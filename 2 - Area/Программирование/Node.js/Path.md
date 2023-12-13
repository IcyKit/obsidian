# Path

Встроенный модуль в Node.JS. Позволяет удобно работать с путями.

### Методы

`basename` - содержит название файла.

```js
const path = require("path");

console.log(path.basename(__filename));
```

`dirname` - Содержит полный путь до папки, где лежит файл, в котором мы на данный момент работаем.

```js
const path = require("path");

console.log(path.dirname(__filename));
```

`extname` - содержит расширение файла.

```js
const path = require("path");

console.log(path.extname(__filename));
```

`parse` - Возвращает объект с информацией о файле. Содержит информацию из методов выше.

```js
const path = require("path");

console.log(path.parse(__filename));
```

`join` - Получим строку с корректным путем в виде `__dirname/server/index.html` .

По факту соединим аргументы вместе.

```js
const path = require("path");

console.log(path.join(__dirname, 'server', 'index.html'));
```