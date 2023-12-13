# State

State - паттерн проектирования, который позволяет объектам менять поведение в зависимости от своего состояния. Извне создаётся впечатление, что изменился класс объекта.

## Пример

Мы публикуем статью в блог. Статья у нас сначала в стадии черновика, потом на модерации, и если все хорошо - опубликована. Но при этом если на модерации возникли проблемы, модерация возвращает статью обратно в черновик.

![State](state1.png)

В данном случае паттерн состояния призван решить проблему удобного управления состоянием.

![State](state2.png)

Вместо того, чтобы хранить просто состояние, а всю логику хранить внутри документа, мы говорим, что у документа, есть ссылка на текущее состояние, которое является подклассом `DocState`. И вся логика, которая связана с переходом между состояниями мы реализовываем внутри подклассов состояния.

```ts
class DocumentItem {
	public text: string;
	private state: DocumentItemState;

	constructor() {
		this.setStatae(new DraftDocumentItemState());
	}

	getState() {
		return this.state;
	}

	setState(state: DocuemntItemState) {
		this.state = state;
		this.state.setContext(this);
	}

	publishDoc() {
		this.state.publish();
	}

	deleteDoc() {
		this.state.delete();
	}
}

abstract class DocumentItemState {
	public name: string;
	public item: DocumentItem;

	public setContext(item: DocumentItem) {
		this.item = item;
	}

	public abstract publish(): void;
	public abstract delete(): void;
}

class DraftDocumentItemState extends DocumentItemState {
	constructor() {
		super();
		this.name = 'DraftDocument'
	}

	public publish(): void {
		console.log(`На сайт отправлен текст ${this.item.text}`)
		this.item.setState(new PublishDocumentItemState());
	}

	public delete(): void {
		console.log(`Документ удален`);
	}
}

class PublishDocumentItemState extends DocumentItemState {
	constructor() {
		super();
		this.name = 'PublishDocument'
	}

	public publish(): void {
		console.log('Документ уже опубликован')
	}

	public delete(): void {
		console.log('Снять с публикации');
		this.item.setState(new DraftDocumentItemState());
	}
}
```