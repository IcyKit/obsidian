# Facade

Фасад - паттерн проектирования, который предоставляет простой интерфейс к сложной системе классов, библиотеке или фреймворку.

## Пример

Нам нужно отправить уведомление. Есть класс, который отправляет уведомление, есть класс который пишет логи, есть класс, который делает e-mail шаблон. И если бы мы хотели отправить уведомление, нам бы нужно было поочередно понимать, как устроен каждый класс.

![Facade](facade.png)

Фасад позволяет скрыть эту сложность за некоторой "ширмой", чтобы внешний пользователь пользовался обычным АПИ (функцией `send()`).

```ts
class Notify {
	send(template: string, to: string) {
		console.log(`Отправляю ${template}: ${to}`)
	}
}

class Log {
	log(message: string) {
		console.log(message);
	}
}

class Template {
	private template = [
		{name: 'other', template: '<h1>Шаблон</h1>'}
	];
	
	getByName(name: string) {
		return this.template.find(t => t.name === name);
	}
}

class NotificationFacade {
	private notify: Notify;
	private log: Log;
	private template: Template;

	constructor() {
		this.notify = new Notify();
		this.template = new Template();
		this.log = new Log();
	}
	
	send(to: string, templateName: string) {
		const data = this.template.getByName(templateName);
		if (!data) {
			return this.log.log('Не найден шаблон!');
		}
		this.notify.send(data.template, to);
		this.log.log('Отправлено!');
	}
}
```