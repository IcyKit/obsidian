# Events

Создание и “прослушка” событий.

### Методы

`on` - создание прослушки события.

- **Первый аргумент** - название события.
- **Второй аргумент** - действие, которое будет происходить при вызове события. Аргументом сюда приходит то, что будет указано вторым аргументом в вызове события.

`emit` - срабатывание события.

```js
const EventEmmiter = require("events");

const emmiter = new EventEmmiter(); // Создание экземпляра класса

emmiter.on("anything", (data) => {
  console.log("On: anything", data);
}); // Создание прослушки события.

emmiter.emit("anything", { a: 1 }); // Срабатывание события.
```

Помимо этого, можно делать свои классы, с кастомными функциями создания и срабатывания прослушки.

```js
const EventEmmiter = require("events");

class Dispatcher extends EventEmmiter {
  subscribe(eventName, callback) {
    console.log("[Subscribe...]");
    this.on(eventName, callback);
  } // Кастомная функция прослушки, **будет функцией экземпляра класса**

  dispatch(eventName, data) {
    console.log("[Dispatching...]");
    this.emit(eventName, data);
  } // Срабатывание прослушки.
}

const dis = new Dispatcher();

dis.subscribe("aa", (data) => {
  console.log("ON: aa", data);
});

dis.dispatch("aa", { a: 15 });
```