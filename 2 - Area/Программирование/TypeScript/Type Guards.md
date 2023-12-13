# Type Guards

Type Guard - вспомогательная конструкция, которая помогает нам работать с типами.

Guard позволяет правильно работать с переменной, которая может хранить в себе несколько типов. И в зависимости от типа переменной нам нужно делать разный функционал. Правила, которые позволяют выводу типов определить суженый диапазон типов для значения называются защитниками типа.

Чаще всего, type guard прописывается в отдельной функции.

При указании типа возвращаемого функцией значения, нужно указывать структуру `имя_аргумента is литерал/интерфейс`

В самой же функции мы проверяем тип аргумента.

При работе с примитивными типами, такие как `number, string, boolean` можно использовать `typeof аргумент === тип`

При работе с интерфейсами мы должны проверить уникальное поле, которое есть только у этого типа на наличие которого мы проверяем: `уникальное_поле in интерфейс`.

В нашем случае это колеса у машины и парус у корабля. Если в объекте имеется тип, например колеса, значит это точно машина.

```ts
function isNumber(n: string | boolean | number): n is number {
	return typeof n === number;
}

// ---------------

interface Car {
	engine: string;
	wheels: number;
}

interface Ship {
	engine: string;
	sail: string;
}

function isCar(transport: Car | Ship): transport is Car {
	return 'wheels' in transport;
}

function isShip(transport: Car | Ship): transport is Ship {
	return 'sail' in transport
}

function repairTransport(transport: Car | Ship): void {
	if (isCar(transport)) {
		console.log(transport.wheels);
	} else {
		console.log(transport.sail);
	}
}
```

Более продвинутый вариант функции выше, позволяет работать со вложенными структурами

```ts
function isCar(transport: Car | Ship): car is Car {
	return (car as Car).wheels.number !== undefined;
}
```

Для работы с классами и объектами мы можем использовать `instanceof`

```ts
class MyResponse {
	header: 'response header'
	result: 'response result'
}

class MyError {
	header: 'error header'
	message: 'error message'
}

function handle(response: MyResponse | MyError) {
	if (response instanceof MyResponse) {
		return {
			info: response.header + response.result
		}
	} else {
		return {
			info: response.header + response.message
		}
	}
}
```

Мы проверяем экземпляром какого класса, к нам приходит аргумент в функцию.

Если он является экземпляром класса `MyResponse` мы делаем определенный блок кода, а если он является экземпляром класса `MyError`, то мы делаем другой блок кода.

Для работы с type. Помогает правильно обработать приходящий тип.

```ts
type AlertType = 'success' | 'danger' | 'warning'

function setAlertType(type: AlertType) {
 //...
}

setAlertType('success') // Ошибки нет, все в порядке.
setAlerType('default') // Ошибка!
```

Если мы указали, что как аргумент, должен приходить созданный нами тип, то функция будет работать корректно только в том случае, если значение аргумента будет равно одному из возможных значений переменной типа.

В нашем случае аргумент строго должен быть равен **либо** `success` **либо** `danger` **либо** `warning`.