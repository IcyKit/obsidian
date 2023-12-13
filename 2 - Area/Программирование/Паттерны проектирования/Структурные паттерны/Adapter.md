# Adapter

Adapter - паттерн проектирования, который позволяет объектам с несовместимыми интерфейсами работать вместе.

## Пример

У нас есть код, который работает с БД `KVDatabase` в виде ключ - значение, и мы хотим добавить возможность персистентного хранилища, которую мы могли бы использовать в нашем коде где угодно.

![Adapter](adapter1.png)

Поэтому нам нужно сделать адаптер, который адаптировал бы все вызовы, которые идут к KVDatabase, к нашей несовместимой БД.

![Adapter](adapter2.png)

Адаптер должен наследоваться от изначальной, совместимой БД. Но в адаптер мы передаем то, что мы должны адаптировать для БД другого типа.

```ts
class KVDatabase {
	private db: Map<string, string> = new Map();

	save(key: string, value: string) {
		this.db.set(key, value);
	}
}

class PersistentDB {
	savePersistent(data: Object) {
		console.log(data);
	}
}

class PersistentDBAdapter extends KVDatabase {
	constructor(public database: PersistentDB) {
		super();
	}

	// Переназначем метод, 
	// чтобы было возможным работать с объектом.
	override save(key: string, value: string): void {
		this.database.savePersistent({key, value});
	}
}

function run(base: KVDatabase) {
	base.save('key', 'myValue');
}
```