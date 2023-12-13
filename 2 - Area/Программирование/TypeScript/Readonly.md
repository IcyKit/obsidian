# Readonly

TypeScript позволяет на уровне синтаксиса сказать, что свойства объекта, массивы или кортежи являются неизменяемыми. Любая операция, направленная на это, будет воспринята как ошибка. Для этого используется модификатор `readonly`

```ts
interface User {
	readonly login: string;
}

const user: user = {
	login: 'qwerty',
}

user.login = 'Error!' // Ошибка!
```

Существуют альтернативные варианты записи при помощи дженериков.

```ts
const userFreeze: Readonly<User> = {
	login: 'qwerty',
	password: '12345'
};

userFreeze.password '123' // Ошибка!
```