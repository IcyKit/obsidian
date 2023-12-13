# Prototype

Prototype - паттерн, благодаря которому мы можем изменять прототип и каждый раз получать объект с разными свойствами.

## Пример

У нас есть история пользователя. Мы хотим посмотреть его имя, и в какой-то момент что-то поменялось и мы хотим взять все, что было одним методом и изменить один параметр.

```ts
interface Prototype<T> {
	clone(): T;
}

class UserHistory implements Prototype<UserHistory> {
	createdAt: Date;
	
	constructor(public email: string, public name: string) {
		this.createdAt = new Date();
	}

	clone(): UserHistory {
		let target = new UserHistory(this.email, this.name);
		target.createdAt = new Date();
		return target;
	}
}
```