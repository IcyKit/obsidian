# Singleton

Singleton - паттерн, работающий на основе Dependency Injection. Основная цель - внезависимости от того, где используется класс, иметь всегда один экземпляр, к которому будут обращаться разные сервисы, и не давать создавать новый.

## Пример

У нас есть два сервиса, которые обращаются к одному и тому же словарю (Map) из разных мест. Нам нужно запретить создание нескольких экземпляров и заставить сервисы использовать один и тот-же экземпляр.

```ts
class MyMap {
	private static instance: MyMap;
	
	map: Map<string, string> = new Map();

	// Делаем конструктор приватным, 
	// чтобы нельзя было вызвать создание нового экземпляра класса. 
	private constructor() {}

	// Статическая функция (можно вызывать без создания экземпляра),
	// на получение словаря. Если его нет, создает новый словарь.
	public static get(): MyMap {
		if (!MyMap.instance) {
			MyMap.instance = new MyMap();
		}
		return MyMap.instance;
	}
}

class Service1 {
	addMap(key: number, value: string) {
		const myMap = MyMap.get();
		myMap.map.set(key, value)
	}
}

class Service2 {
	getKeys(key: number) {
		const myMap = MyMap.get();
		myMap.map.get(key);
	}
}
```