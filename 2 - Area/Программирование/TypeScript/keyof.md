# keyof

`keyof` - это оператор, позволяющий получить все свойства объекта, его ключи. Мы работаем с типами, а не с литералами, так что его можно применить к interface, type и class

```ts
interface ICompany {
	name: string;
	debts: number;
}

type CompanyKeys = keyof ICompany // 'debts' | 'name'
```

Результат работы - это `union type` из свойств. Часто такой прием используется с дженериками для того, чтобы создать связь между типом и его свойствами. В таком случае мы сможем использовать только существующие свойства и получим подсказки

```ts
function printDebts<T, K extends keyof T, S extends keyof T>(
	company: T,
	name: K,
	debts: S
) {
	console.log(`Company ${company[name]}, debts: ${company[debts]}`);
}

const google: ICompany = {
	name: 'Google',
	debts: 500000,
}

printDebts(google, 'name', 'debts'); // 2 и 3 аргумент строго ограничены тем, какие поля есть у google.
```

## Indexed Access Types

Для получения типа значения в определенном свойстве используется

прием Indexed Access Types (дословно: получение типа по индексу):

```ts
interface ICompany {
	name: string;
	debts: number;
}

type CompanyDebtsType = ICompany['debts'] // number
```

Можно применять на объектных типах, в том числе и для получения значений вложенных свойств

## Получение одного конкретного типа из массива типов

```ts
interface ICompany {
	name: string;
	debts: number;
	departments: Department[];
}

type CompanyDepartmentsType = ICompany['departments'][number] // Department
```