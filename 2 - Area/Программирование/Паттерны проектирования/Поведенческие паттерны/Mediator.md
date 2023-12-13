# Mediator

Mediator - паттерн проектирования, который позволяет уменьшить связанность множества классов между собой, благодаря перемещению этих связей в один класс-посредник.

## Пример

У нас есть класс `EventHandler`, который обрабатывает некое событие и говорит сервису уведомление, что нужно отправить уведомление, уведомление говорит сервису логирования, что нужно залогировать, а также, сервис уведомление говорит сервису кеширования, что нужно закешировать запрос, а если закешировать не получилось - отправить ошибку в сервис логирования.

![Mediator](mediator1.png)

Для разрешения такой сложной связанности, нужно добавить компонент посредник, который обычно называется медиатором.

![Mediator](mediator2.png)

```ts
interface IMediator {
	notify(sender: string, event: string): void;
}

abstract class Mediated {
	mediator: IMediator;
	setMediator(mediator: Mediator) {
		this.mediator = mediator;
	}
}

class Notifications {
	send() {
		console.log('Отправляю уведомление...');
	}
}

class Log {
	log(message: string) {
		console.log(message);
	}
}

class EventHandler extends Mediated {
	myEvent() {
		this.mediator.notify('EventHandler', 'myEvent');	
	}
}

class NotificationMediator implements IMediator {
	constructor(
		public notifications: Notifications, 
		public logger: Log, 
		public eventHandler: EventHandler) {
		}
	notify(sender: string, event: string): void {
		switch(event) {
			case 'myEvent':
				this.notifications.send();
				this.logger.log('Отправлено');
				break;
		}
	}
}

const handler = new EventHandler();
const logger = new Log();
const notifications = new Notifications();

const m = new NotificationMediator(notifications, logger, handler);
handler.setMediator(m);
handler.myEvent();
```