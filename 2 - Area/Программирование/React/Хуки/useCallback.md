# useCallback

**useCallback** - Возвращает мемоизированный callback.

`useCallback(fn, deps)` — это эквивалент `useMemo(() => fn, deps)`.

`useCallback` - не результат выполнения функции, а сама функция.

Логика работы очень похожа на useEffect. Только `useEffect` выполняет указанный код, а `useCallback` возвращает функцию, которая указана первым аргументом, и мы можем в дальнейшем ее использовать где хотим.

Так как мы знаем, что при обновлении компонента, все функции внутри него пересоздаются, мы говорим этому хуку, чтобы он сохранил внутри себя ссылку на функцию, и если компонент, внутри которого находится этот хук обновится, функция не будет пересоздана, или будет пересоздана, если мы укажем зависимости в массиве.

Для начала использования данного хука его нужно импортировать из React.

```tsx
import {React, useCallback} from 'react';
```

Полность хук выглядит следующим образом:

```tsx
const memoize = useCallback(() => {
	return someFunction();
}, [state1]);
```

### Полная структура приложения для примера

```tsx
function App() {
	const [count, setCount] = useState(1);
	const [colored, setColored] = useState(false);

	const styles = {
		color: colored ? 'darkred' : 'black';
	}

	const fetchAPI = useCallback(() => {
		return new Array(count).fill('').map((i, index) => `Элемент: ${index}`);
	}, [count]) // Симулируем запрос к серверу

	return (
		<div>
			<h1 style={styles}>Количество элементов: {count}</h1>
			<button onClick={() => setCount(prev => prev + 1)}>Добавить</button>
			<button onClick={() => setColored(prev => !prev)}>Изменить</button>
			
			<ItemsList getItems={fetchAPI} />
		</div>)
}
```

```tsx
function ItemsList({fetchAPI}) {
	const [items, setItems] = useState([]);

	useEffect(() => {
		const newItems = fetchAPI();
		setItems(newItems);
		console.log('render');
	}, [fetchAPI])

	return (
		<ul>
			{items.map(i => <li key={i}>{i}</li>)}
		</ul>)
}
```

**Пояснение к коду**

При нажатии на кнопку "Добавить" - наш счетчик увеличивается на 1, соответственно и длинна массива, которая зависит от счетчика - увеличивается на 1.

Все работает отлично, рендер выполняется правильно.

Но если мы захотим сменить цвет заголовка, в консоли опять будет выводиться сообщение о рендере, хотя useEffect внутри компонента `ItemsList` зависит только от функции `fetchAPI`.

Если представить, что внутри `fetchAPI` находится запрос к серверу, то запрос будет выполняться много лишних раз.

Поэтому мы оборачиваем функцию `fetchAPI` в `useCallback`. Теперь запрос к серверу будет делаться только тогда, когда меняется состояние в массиве зависимостей, в нашем случае это счетчик.