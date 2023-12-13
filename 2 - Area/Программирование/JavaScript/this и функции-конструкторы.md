# this и функции-конструкторы

_this_ - это, что окружает функцию и как она вызывается. По факту, когда вызывается this, на ее место подставляется имя экземпляра класса.

```jsx
function showThis() {
    console.log(this);
}

// Пункт 1 в тексте ниже

const obj = {
    a: 20,
    b: 30,
    sum: function() {
        console.log(this);
    }
};

obj.sum();

// Пункт 2 в тексте ниже

function User(name, age) {
    this.name = name;
    this.age = age;
    this.human = true;
    this.hello = function() {
        console.log(`Hi! My name is ${this.name}`);
    };
}

let Nikita = new User('Nikita', 18);

//Пункт 3 в тексте ниже

function sayName(surname) {
    console.log(this);
    console.log(this.name + surname);
}

const user = {
    name: 'John',
};

sayName.call(user, 'Smith'); // Передаем Surname. Ручное срабатывание функции на объекте
sayName.apply(user, ['Smith']); // Передаем Surname. Ручное срабатывание функции на объекте

// Пункт 4 в тексте ниже

function count(num) {
    return this*num;
}

const double = count.bind(2); // двойка переходит в this у count
console.log(double(3)); //сюда передаем аргумент который будет переходить в num у count, double - переменная с функцией
```

`this` (контекст) у методов объекта - выводит сам объект. Если добавить функцию в свойство, в котором уже есть функция, т.е вложенную функцию, то переходим к первой функции, но если сделать стрелочную функцию, она будет брать контекст (this) родителя

`this` в конструкторах - новый экземпляр объекта

Ручная привязка `this` :

- `call`
- `apply`
- `bind`

Мы не можем использовать this у стрелочной функции, у которой нет родителя.

```jsx
btn.addEventListener('click' () => {
    this.style.backgroundColor = 'red';
}); // такое не сработает, у стрелочной функции нет своего контекста this
```

Задача с собеседований про this

```jsx
function showThis(a, b) {
    console.log(this);
    function sum() {
        console.log(this); // Вопрос 1 - что выведет эта строчка? Ответ: window без use strict, undefined с ним
        return this.a + this.b; // Вопрос 2 - как надо изменить эту строку чтобы она заработала? Ответ: Убрать this, т.е. замкнуть функцию
    }
    console.log(sum());
}
showThis(4, 5)
```

Мы можем делать функции, создающие прототипы с наборами свойств.

Но это уже устарело, потом в новом стандарте пришли классы, сейчас используют только их.

```jsx
function User(name, age) {
    this.name = name,
    this.age = age,
    this.human = true,
    this.hello = function() {
        console.log(`Hi! My name is ${this.name}`);
    };
}

User.prototype.exit = function() {
    console.log(`User ${this.name}, ${this.age} has left`);
}; // создает новое свойство для User, в данном случае - функция

const Nikita = new User('Nikita', 18),
      Mariya = new User('Mariya', 19);

Nikita.hello(); // Hi! My name is Nikita
Nikita.exit(); // User Nikita, 18 has left
```

[https://youtu.be/UGapN-hrekw](https://youtu.be/UGapN-hrekw)

## Дополнительная литература

[О ключевом слове «this»](https://tproger.ru/translations/javascript-this-keyword/)