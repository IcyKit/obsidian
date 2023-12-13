# useRef

**useRef** - Хранит состояние, но не вызывает рендер компонента при изменении.

Очень похож на useState.

Данный хук отличается тем, что вместо кортежа мы получаем объект с единственным свойством `current`.

Обращаться к состоянию для его изменения можно напрямую.

_Мутирование свойства `current` не вызывает повторный рендер._

Для начала использования данного хука его нужно импортировать из React.

```tsx
import {React, useRef} from 'react';
```

Полность хук выглядит следующим образом:

```tsx
const state = useRef(начальное значение);
```

### Хранение DOM элемента

Помимо простого сохранения состояния, мы можем хранить DOM элементы внутри состояния.

```tsx
const inputRef = useRef(null);

return (
	<input ref={useRef} value="Value example"/>)
```

И теперь мы можем взаимодействовать с input напрямую через переменную.

```tsx
console.log(inputRef.current.value);
// -------
inputRef.current.focus();
```

### Хранение предыдущего состояния

Данный хук часто используется для того, чтобы хранить в нем предыдущее состояние от useState

```tsx
const [value, setValue] = useState('initial');
const prevValue = useRef('');

useEffect(() => {
	prevValue.current = value;
}, [value])
```

### Полная структура приложения для примера

```tsx
function App() {
	const [value, setValue] = useState('initial');
	const renderCount = useRef(1);
	const inputRef = useRef(null);
	const prevValue = useRef('');

	useEffect(() => {
		renderCount.current++;
	});

	useEffect(() => {
		prevValue.current = value;
	}, [value]);
	
	return (
		<div>
			<h1>Количество рендереов: {renderCount.current}</h1>
			<h2>Прошлое состояние: {prevValue.current}</h2>
			<input ref={inputRef} type="text" onChange={e => setValue(e.target.value) value={value}}/>
		</div>)
}
```

**Пояснения к коду**

Если бы сделали состояние счетчика рендеров через useState, то у нас бы запустился бесконечный цикл обновления компонентов, так как useEffect без массива зависимостей выполняет блок кода каждый раз, когда обновляется компонент. А из-за перерисовки `value`, перерисовка всего компонента будет выполняться бесконечно