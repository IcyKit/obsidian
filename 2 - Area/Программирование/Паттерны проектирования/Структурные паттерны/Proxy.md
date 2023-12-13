# Proxy

Proxy - паттерн проектирования, который позволяет подставлять вместо реальных объектов специальные объекты-заменители. Эти объекты перехватывают вызовы к оригинальному объекту, позволяя сделать что-то _до_ или _после_ передачи вызова оригиналу.

## Пример

У нас есть код, который работает с API взаимодействия с платежами. Но возникает вопрос, как управлять доступом к этому API

![Proxy](proxy1.png)

Один из вариантов решения - прокси.

![Proxy](proxy2.png)

Наш класс `PaymentAPI` имплементируется от интерфейса `IPaymentAPI`. Мы создаем новый класс `PaymentAPIProxy`, который так-же будет имплементироваться от интерфейса `IPaymentAPI`. Поэтому, мы не взаимодействуем напрямую с `PaymentAPI`, мы внедряем прокси класс, как зависимость, и уже в этом классе можем делать дополнительные действия, например, проверять кешировать запросы, проверять доступ и т.д.

```ts
interface IPaymentAPI {
	getPaymentDetail(id: number): IPaymentDetail | undefined;
}

interface IPaymentDetail {
	id: number;
	sum: number;
}

class PaymentAPI implements IPaymentAPI {
	private data = [{id: 1, sum: 10000}];
	
	getPaymentDetail(id: number): IPaymentDetail | undefined {
		return data.find(p => p.id === id);
	}
}

class PaymentAccessProxy implements IPaymentAPI {
	constructor(private api: PaymentAPI, private userId: number) {}

	getPaymentDetail(id: number): IPaymentDetail | undefined {
		if (this.userId === 1) {
			return this.api.getPaymentDetail(id);
		}
		console.log('Попытка получить данные платежа!')
		return undefined;
	}
}
```