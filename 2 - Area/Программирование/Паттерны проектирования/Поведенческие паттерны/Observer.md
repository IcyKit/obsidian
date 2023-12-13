# Observer

Observer - паттерн проектирования, который создаёт механизм подписки, позволяющий одним объектам следить и реагировать на события, происходящие в других объектах.

## Пример

У нас есть новый лид (заявка) и когда он приходит, нужно, чтобы менеджер связался с отправителем заявки, а сервис уведомлений отправил сообщение о заявке.

![Observer](observer1.png)

Чтобы нам не пришлось время от времени ходить и проверять заявки, мы можем подписаться на их получение.

![Observer](observer2.png)

Сервисы имплементируются от наблюдателей и после подписки они будут получать уведомления.

```ts
interface Observer {
	update(subject: Subject): void;
}

// Тот, кто эмитит события
interface Subject {
	attach(observer: Observer): void;
	detach(observer: Observer): void;
	notify(): void;
}

class Lead {
	constructor(public name: string, public phone: string) {}
}

class NewLead implements Subject {
	private observers: Observer[] = [];
	public state: Lead;
	
	attach(observer: Observer): void {
		if(this.observers.includes(observer)) {
			return;
		}
		this.observers.push(observer);
	}
	detach(observer: Observer): void {
		const observerIndex = this.observers.indexOf(observer);
		if (observerIndex == -1) {
			return
		}
		this.observers.splice(observerIndex, 1);
	}
	notify(): void {
		for (const obs of this.observers) {
			obs.update(this);
		}
	}
}

class NotificationService implements Observer {
	update(subject: Subject): void {
		console.log('NotificationService получил уведомление');
		console.log(subject);
	}
}

class LeadService implements Observer {
	update(subject: Subject): void {
		console.log('LeadService получил уведомление');
		console.log(subject);
	}
}

const subject = new NewLead();
subject.state = new Lead('Никита', '000000');
const s1 = new NotificationService();
const s1 = new LeadService();

subject.attach(s1);
subject.attach(s2);
subject.notify();
subject.detach(s1);
subject.notify();
```