# Bridge

Bridge - паттерн проектирования, который разделяет один или несколько классов на две отдельные иерархии — абстракцию и реализацию, позволяя изменять их независимо друг от друга.

## Пример

У нас есть приложение, которое доставляет уведомление в Telegram и WhatsApp. Кроме того, у нас есть два типа уведомления: отложенное и обычное.

![Bridge](bridge1.png)

Если бы мы делали это на классах, у нас было бы два класса мессенджера и два класса типа, это означает, что нам нужно расширять классы горизонтально. Другими словами, нам нужно было бы иметь классы: провайдер телеграма отложенных сообщений, провайдер телеграма обычных сообщений, провайдер ватсаппа отложенных сообщений, провайдер ватсаппа обычных сообщений. А если бы мы добавили новый мессенджер или новый тип, нам бы опять пришлось накидывать новые классы.

Паттерн _Bridge_ избавляет нас от этого.

![Bridge](bridge2.png)

У нас есть базовый класс `NotificationSender` который доставляет обычные уведомления.

В рамках работы этого класса, у нас есть провайдер класса, через который мы доставляем сообщений. При этом провайдер приходит в класс через конструктор, который должен соответствовать интерфейсу провайдера `IProvider`

с определенным набором методов. Этот интерфейс является неким мостом между конкретными провайдерами и функцией отправки в классе `NotificationSender`. А для отложенной отправки сообщений, нам просто нужно унаследовать базовый класс `NotificationSender` к новому классу `DelayedNotificationSender`.

```ts
interface IProvider {
	sendMessage(msg: string): void;
	connect(config: unknown): void;
	disconnect(): void;
}

class TelegramProvider implements IProvider {
	sendMessage(msg: string): void {
		console.log(msg)
	}

	connect(config: string): void {
		console.log(config);
	}

	disconnect(): void {
		console.log('Telegram has been disconnected');
	}
}

class WhatsAppProvider implements IProvider {
	sendMessage(msg: string): void {
		console.log(msg)
	}

	connect(config: string): void {
		console.log(config);
	}

	disconnect(): void {
		console.log('WhatsApp has been disconnected');
	}
}

class NotificationSender {
	constructor(private provider: IProvider) {}

	send() {
		this.provider.connect('connect');
		this.provider.sendMessage('message');
		this.provider.disconnect();
	}
}

class DelayNotificationSender extends NotificationSender {
	constructor(private provider: IProvider) {
		super(provider);
	}
	
	sendDelayed() {
		this.provider.connect('connect');
		this.provider.sendMessage('message');
		this.provider.disconnect();
	}
}
```