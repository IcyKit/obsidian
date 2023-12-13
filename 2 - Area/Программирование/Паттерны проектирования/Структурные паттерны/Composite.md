# Composite

Composite - паттерн проектирования, который позволяет сгруппировать множество объектов в древовидную структуру, а затем работать с ней так, как будто это единичный объект.

## Пример

У нас есть магазин, который поставляет товары от нескольких поставщиков. Клиент набирает товары на вебсайте и хочет понять суммарную стоимость доставки. Есть магазин, который берет комиссию за поставку. В этом магазине есть упаковки, в которые мы кладем однотипный товар, например, SSD накопитель. А может быть товар и без упаковки. По сути, это древовидная структура.

![Composite](composite1.png)

Есть решение, которое делает абстрактный класс, в этом абстрактном классе есть набор товаров (не важно в упаковке или нет). У этого абстрактного класса будет две функции - добавить товар или получить общую стоимость. Получение общей стоимость просто будет запрашивать эту стоимость у всех товаров. Сама логика рассчета цены будет внутри класса товара.

![Composite](composite2.png)

```ts
abstract class DeliveryItem {
	items: DeliveryItem[];

	addItem(item: DeliveryItem) {
		this.items.push(item);
	}

	getItemPrices(): number {
		return this.items.reduce((acc: number, i: DeliveryItem) => acc += i.getPrice(), 0);
	}

	abstract getPrice(): number
}

class DeliveryShop extends DeliveryItem {
	constructor(public deliveryFee: number) {
		super();
	}
	
	getPrice(): number {
		return this.getItemPrices() + this.deliveryFee;	
	}
}

class Package extends DeliveryItem {
	getPrice(): number {
		this.getItemPrices();
	}
}

class Product extends DeliveryItem {
	constructor (public price: number) {
		super();
	}

	getPrice(): number {
		return this.price;
	}
}
```