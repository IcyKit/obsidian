# Функции в onClick

Краткая заметка о том, почему мы должны вызывать функции в onClick через

```tsx
onClick={() => функция()}
```

Краткая верстка для примера

```tsx
function Categories() {
	const [activeIndex, setActiveIndex] = useState(0);

	const onClickCategory = (index) => {
		setActiveIndex(index);
	}

	return (
	<div className="categories">
	    <ul>
		    <li
	          onClick={onClickCategory(0)}className={activeIndex === 0 ? "active" : ""}>
	          Все
	        </li>
	        <li
	          onClick={onClickCategory(1)}className={activeIndex === 1 ? "active" : ""}>	
	          Мясные
	        </li>
	        <li
	          onClick={onClickCategory(2)}className={activeIndex === 2 ? "active" : ""}>	
	          Вегетарианская
	        </li>
		<ul>
	</div>
	);
}
```

Если мы будем вызывать функцию, без анонимной функции, т.е. без `() => функция()`, то у нас получится бесконечный рендер компонента и приложение просто сломается. Мы говорим компоненту, чтобы при рендере верстки, сразу же запускалась функция, которая будет менять состояние, а смена состояния в свою очередь будет заново перерисовывать компонент, заново перерисовавшийся компонент опять сразу будет вызывать функцию и так получится _бесконечный цикл перерисовки_.