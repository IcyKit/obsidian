# Chain of Command

Chain of Command - паттерн проектирования, который позволяет передавать запросы последовательно по цепочке обработчиков. Каждый последующий обработчик решает, может ли он обработать запрос сам и стоит ли передавать запрос дальше по цепи.

## Пример

У нас есть некоторый внешний запрос который приходит в обработчик, например http запрос в API. Но тут мы понимаем, что должны добавить авторизацию в обработчик, мы добавляем эту логику в обработчик, потом понимаем что нужно добавить еще несколько промежуточных функций в запрос, и делать это в обработчике не стоит, так как код разрастется и его сложно будет поддерживать, а также код нужно будет дублировать. По сути это и есть `Middleware`

![Chain of Command](coc1.png)

Нам поможет подход цепочки команд.

![Chain of command](coc2.png)

```ts
interface IMiddleware {
	next(mid: IMiddleware): IMiddleware;
	handle(req: any): any;
}

abstract class AbstractMiddleware implements IMiddleware {
	private nextMiddleware: IMiddleware;
	
	 next(mid: IMiddleware) {
		 this.nextMiddleware = mid;
		 return mid;
	 }

	handle(req: any) {
		if (this.nextMiddleware) {
			return this.nextMiddleware.handle(req);
		}
		return;
	}
}

class AuthMiddleware extends AbstractMiddleware {
	override handle(req: any) {
		console.log('AuthMiddleware');
		if (req.userId === 1) {
			return super.handle(request);
		}
		return {error: "Вы не авторизованы"};
	}
}

class ValidateMiddleware extends AbstractMiddleware {
	override handle(req: any) {
		console.log('ValidateMiddleware');
		if (req.body) {
			return super.handle(request);
		}
		return {error: "Нет body"};
	}
}

class Controller extends AbstractMiddleware {
	override handle(req: any) {
		console.log('Controller');
		return {success: request};
	}
}

const controller = new Controller();
const validate = new ValidateMiddleware();
const auth = new AuthMiddleware();

auth.next(validate).next(controller);
```