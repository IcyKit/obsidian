# Template Method

Template Method - паттерн проектирования, который определяет скелет алгоритма, перекладывая ответственность за некоторые его шаги на подклассы. Паттерн позволяет подклассам переопределять шаги алгоритма, не меняя его общей структуры.

## Пример

У нас есть форма заявки, которая может работать с несколькими API. При этом мы понимаем, что процесс работы с этими API будет почти одинаковым.

![Template](template1.png)

В таком случае, логику пришлось бы дублировать для двух API. В этом случае нам на помощь приходит шаблонный паттерн.

![Template](template2.png)

У нас есть определенный набор последовательных шагов, который в результате приводит к результату.

Делаем абстрактный класс, который имеет набор методов. При этом некоторые методы будут абстрактными, а некоторые - нет.

После этого мы создаем от него два класса API, которые должны будут реализовать отправку (абстрактные методы).

```ts
class Form {
	constructor(public name: string) {}
}

abstract class SaveForm<T> {
	public save(form: Form) {
		const res = this.fill(form);
		this.log(res);
		this.send(res);
	}
	protected abstract fill(form: Form): T;
	protected log(data: T): void {
		console.log(data);
	};
	protected abstract send(data: T): void;
}

class FirstAPI extends SaveForm<string> {
	protected fill(form: Form): string {
		return form.name;
	};
	
	protected send(data: string): void {
		console.log(`Отправляю ${data}`);
	};
}

class SecondAPI extends SaveForm<{fio: string}> {
	protected fill(form: Form): {fio: string} {
		return {fio: form.name};
	};
	
	protected send(data: {fio: string}): void {
		console.log(`Отправляю ${data}`);
	};
}
```