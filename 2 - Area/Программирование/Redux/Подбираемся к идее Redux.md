# Подбираемся к идее Redux

Этот конспект — продолжение конспекта [[Создание store]]

В данном конспекте описывается то, как на самом деле в Redux создавать store, классическим паттерном.

В папке src создадим два файла, `actions.js` и `reducer.js`

- `actions.js` - содержит в себе функции, которые возвращают объект с обязательным полем `type` и опциональным полем `payload`.

```js
	export const inc = () => ({ type: "INC" });
	export const dec = () => ({ type: "DEC" });
	export const rnd = (value) => ({ type: "RND", payload: value });
```

- `reducer.js` - содержит в себе объект с начальными состояниями и саму функцию `reducer`, которая меняет состояние, в зависимости от приходящего объекта и его поля `type`

```js
	const initialState = { value: 0 };
	
	const reducer = (state = initialState, action) => {
	  switch (action.type) {
	    case "INC":
	      return {
	        ...state,
	        value: state.value + 1,
	      };
	    case "DEC":
	      return {
	        ...state,
	        value: state.value - 1,
	      };
	    case "RND":
	      return {
	        ...state,
	        value: state.value * action.payload,
	      };
	    default:
	      return state;
	  }
	};
	
	export default reducer;
```

Теперь мы можем оптимизировать вызов `store.dispatch()` с помощью actionCreator

Для начала, вытащим из store три его функции:

```js
{ getState, subscribe, dispatch } = store; // Вытаскиваем из store,
// который уже объявлен
```

Теперь, там, где мы писали `store.dispatch()`, `store.getState()` и `store.subscribe()` - мы можем просто писать `dispatch()`, `getState()`, `subscribe()`.

Теперь сделаем actionCreator (промежуточную функцию):

```js
const incDispatch = () => dispatch(inc());
```

Таким образом, у нас изменяется код в блоке с отслеживанием нажатия, для увеличения счетчика.

```js
// БЫЛО
document.getElementById('inc').addEventListener('click', () => {
	dispatch(inc());
});

// СТАЛО
document.getElementById('inc').addEventListener('click', incDispatch)
```

Теперь мы можем еще больше оптимизировать наш код и сделать объект с функциями диспатча, для этого нам нужно импортировать bindActionCreators из Redux:

```js
const incDispatch = bindActionCreators(inc, dispatch); // Первым аргументом передаем функцию из actions.js, которая возвращает объект // Второй аргумент - функция вытащенная из store.
```

Но самое правильно определение функций, чтобы не инициализировать все функции много раз - сделать объект с функциями.

```js
const actions = bindActionCreators({
	incDispatch: inc,
	decDispatch: dec,
	rndDispatch: rnd,
}, dispatch)

// Это позволяет нам сразу сделать деструктуризацию:
const { incDispatch, decDispatch, rndDispatch } = bindActionCreators({
	incDispatch: inc,
	decDispatch: dec,
	rndDispatch: rnd,
}, dispatch)
```

Но мы можем пойти еще дальше и если импортировать все actions как один объект:

```js
import * as actions from './actions';
```

Это позволяет нам сократить код путем передачи этого объекта как первый аргумент, в таком случае все функции будут называться так-же как они названы в файле actions, потому что по сути, в объект bindActionCreators подставляется ключ и значение одинакового текста:

```js
bindActionCreators({
	inc: inc,
	dec: dec,
	rnd: rnd,
})
```

Поэтому наш итоговая инициализация объекта выглядит так:

```js
const {inc, dec, rnd} = bindActionCreators(actions, dispatch); 
```