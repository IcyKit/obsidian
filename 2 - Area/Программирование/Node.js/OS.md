# OS 

Получение информации о данной операционной системе.

## Методы

`platform` - получить операционную систему.

```js
const os = require("os");

console.log(os.platform()); // macOS = darwin;
```

`arch` - архитектура процессора.

```js
const os = require("os");

console.log(os.arch());
```

`cpus` - информация о всех процессорах.

```js
const os = require("os");

console.log(os.cpus());
```

`freemem` - получить количество свободной памяти.

```js
const os = require("os");

console.log(os.freemem());
```

`totalmem` - получить тотальное количество памяти.

```js
const os = require("os");

console.log(os.totalmem());
```

`homedir` - получить домашнюю директорию.

```js
const os = require("os");

console.log(os.homedir());
```

`uptime` - время действия системы.

```js
const os = require("os");

console.log(os.uptime());
```