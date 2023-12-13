# Strategy

Strategy - паттерн проектирования, в котором мы выделяем схожие поведения и алгоритмы в нашей системе и выделяем их в отдельные классы, после чего можем использовать на лету взаимозаменяя их.

## Пример

Мы хотим авторизовать нашего пользователя в сервисе авторизации. Но у нас есть несколько способов авторизации.

![Strategy](strategy1.png)

Стратегия решает эту проблему, потому что алгоритмы авторизации схожи.

![Strategy](strategy2.png)

Выделяется интерфейс авторизации, при этом сама логика будет реализовываться в классе авторизации.

```ts
class User {
	githubtoken: string;
	jwttoken: string;
}

interface AuthStrategy {
	auth(user: User): boolean;
}

class Auth {
	constructor(private strategy: AuthStrategy) {}

	setStrategy(strategy: AuthStrategy) {
		this.strategy = strategy;
	}

	public authUser(user: User): boolean {
		return this.strategy.auth(user);
	}
}

class JWTStrategy implements AuthStrategy {
	auth(user: User): boolean {
		if (user.jwttoken) {
			return true
		}
		return false;
	}
}

class GitHubStrategy implements AuthStrategy {
	auth(user: User): boolean {
		if (user.githubtoken) {
			return true
		}
		return false;
	}
}
```