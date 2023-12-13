# useContext

**useContext** - позволяет передавать параметры в компоненты без промежуточных компонентов.

Т.е если вложенность компонентов больше двух, нам нужно передавать пропсы через каждый компонент. Хук useContext помогает избавиться от этого и не использовать прослойки.

Для начала использования данного хука его нужно импортировать из React.

```tsx
import {React, createContext, useContext} from 'react';
```

После импорта - создадим экземпляр контекста в родительском компоненте:

```tsx
const MyContext = createContext('Если мы забудем обернуть компонент в провайдер, мы увидим этот текст');
```

Для работы хука useContext, нам нужно обернуть дочерний элемент в провайдер, который будет видеть контексты.

В параметре `value` мы отправляем значение во вложенный компонент. Контекст из вложенного компонента будет видеть текст из `value`.

```tsx
function External() {
	return (
		<MyContext.Provider value="Hello, I am External!">;
			<Intermediate />
		</MyContext.Provider>)
}
```

Внутри самого вложенного тега, того, в котором мы хотим увидеть контекст, для начала нам нужно создать экземпляр уже имеющегося у нас контекста

```tsx
function Internal() {
	const context = useContext(MyContext);
	return `Я вложенный тег, я вижу значение контекста: {context}`;
}
```

Теперь, наш компонент видит текст, без передачи пропсов через несколько компонентов.

### Полная структура приложения для примера

```tsx
const MyContext = createContext('Если мы забудем обернуть компонент в провайдер, мы увидим этот текст');

function External() {
	return (
		<MyContext.Provider value="Hello, I am External!">;
			<Intermediate />
		</MyContext.Provider>)
}

function Intermediate() {
	return <Internal />}

function Internal() {
	const context = useContext(MyContext);
	return `Я вложенный тег, я вижу значение контекста: {context}`;
}

function App() {
	return <External />;
}
```