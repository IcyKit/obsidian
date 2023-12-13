# fs

Управление файловой системой на пк. К примеру - создание или удаление файлов и папок.

### Методы

`mkdir` - создание папки.

- **Первый аргумент** - путь до папки, которую хотим создать, учитывая название папки.

В аргумент поступает ошибка, если она есть. **Выдаст ошибку, если папка уже есть.**

- **Второй аргумент** - callback функция, которая исполняется, когда функция выполнится.

```js
const fs = require("fs");
const path = require("path");

fs.mkdir(path.join(__dirname, "test"), (err) => {
  if (err) {
    throw err;
  }

  console.log("Папка создана");
});
```

`writeFile` - создание файла с **перезаписыванием всего, что было внутри файла.**

**Первый аргумент** - путь до файла, который хотим создать.

**Второй аргумент** - текст внутри файла.

**Третий аргумент** - callback, который произойдет после выполнения.

```js
const fs = require("fs");
const path = require("path");

const filePath = path.join(__dirname, "test", "text.txt"); 
// Путь до файла, который хотим создать.

fs.writeFile(filePath, "Hello World!", (err) => {
  if (err) {
    throw err;
  }

  console.log("Файл создан");
});
```

`appendFile` - тоже самое, что и `writeFile`, но **добавление контента в файл без перезаписи**.

```js
const fs = require("fs");
const path = require("path");

const filePath = path.join(__dirname, "test", "text.txt"); 
// Путь до файла, который хотим создать.

fs.writeFile(filePath, "Hello World!", (err) => {
  if (err) {
    throw err;
  }

  console.log("Файл создан");
});
```

`readFile` - прочитать содержимое файла.

**Вторым аргументом** указывается кодировка, в которой написан файл. (опционально)

```js
const fs = require("fs");
const path = require("path");

const filePath = path.join(__dirname, "test", "text.txt"); 
// Путь до файла, который хотим создать.

fs.readFile(filePath, "utf-8", (err, content) => {
  if (err) {
    throw err;
  }
  console.log(content);
});
```

_Создавая сервер на чистом `http` модуле можно выдавать пользователю html страницы в зависимости от строки поиска с помощью **readFile**. Простой пример:_

```js
*const server = http.createServer((request, response) => {
  if (request.url === "/" && request.url === "/home") {
    fs.readFile(path.join(__dirname, "public", "index.html"), (err, data) => {
      if (err) {
        throw err;
      }
      
      response.writeHead(200, {
        "Content-Type": "text/html",
      });
      response.end(data);
    });
  }
});

server.listen(3000, () => {
  console.log("Сервер успешно запущен!");
});*
```

_Страницу передаем в `response.end()` с результатом чтения `readFile`. Но перед этим **ОБЯЗАТЕЛЬНО** нужно в `headers` вписать, что тип контента - html, чтобы браузер понимал, что он считывает html файл._