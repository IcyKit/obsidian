# Redux Toolkit

Redux Toolkit - улучшенная и упрощенная версия ванильного [[Redux]] для [[React]]. С ее помощью, можно намного проще создавать состояние и управлять им.

## Начало

Изначально, `Redux` - библиотека ванильного [[2 - Area/Программирование/JavaScript/JavaScript]] и чтобы использовать ее в реакте, нам нужно подружить `Redux` и `React`.  
Для этого, мы устанавливаем в наш проект две зависимости. Саму библиотеку `Redux Toolkit` и `React-Redux` для корректной работы `Redux` в `React`.

```bash
npm install @reduxjs/toolkit react-redux
```

## Создания store

После установки зависимостей - перейдем к созданию store.  
Store - общее хранилище, которое будет хранить множество других объектов состояний.  
Создадим папку `redux` и в ней создадим файл `store.js`

```jsx
// src/redux/store.js

import { configureStore } from '@reduxjs/toolkit'  
  
export const store = configureStore({  
reducer: {},  
})
```

В главный файл нашего приложения (`index.js`) импортируем созданный нами `store` и`Provider` в который нужно обернуть наше приложение и передать `store` как props.

```jsx
import { store } from './app/store'  
import { Provider } from 'react-redux'

ReactDOM.render(  
	<Provider store={store}>  
		<App />  
	</Provider>,  
document.getElementById('root')  
)
```

## Создание Slice

Slice - аналог `reducer` в ванильном `Redux`. С помощью `slice` - мы описываем наш объект состояния. Их может быть несколько, но в разных файлах. Все они попадают в один `store`.  
Создание `slice` показано на простом примере счетчика.

```jsx
import { createSlice } from '@reduxjs/toolkit'  
  
const initialState = {  
	value: 0,  
}  
  
export const counterSlice = createSlice({  
	name: 'counter',  
	initialState,  
	reducers: {  
		increment: (state) => {   
		state.value += 1  
		},  
		decrement: (state) => {  
		state.value -= 1  
		},  
		incrementByAmount: (state, action) => {  
		state.value += action.payload  
		},  
	},  
})  
  

export const { increment, decrement, incrementByAmount } = counterSlice.actions  
  
export default counterSlice.reducer
```

Для начала - мы импортируем функцию, с помощью которой мы будем создавать `slice`.  
После этого мы создаем объект `initialState`, это объект начального состояния, там может быть сколько угодно полей и эти поля могут называться как угодно. Это аналог того, что мы передаем в аргументы [[useState]]

Теперь мы готовы к созданию самого `slice`, для его создания мы используем функцию, которую мы импортировали (`createSlice`).  
Нам нужно передать сюда следующие поля:

- `name` - название `slice`. Нужно для корректного понимания Redux о том, какой `slice` нужно обработать. Имя используется в типе `action`. Тип `action` будет приходить в `reducer` функцию в виде: имяслайса/имяфункции.
- `initialState` - объект начального состояния, передаем просто имя переменной, так как и ключ и значение называются одинаково. Это аналог `initialState: initialState`.
- `reducers` - функции изменения состояния. Такие функции еще называют `action`. В отличии от ванильного Redux, нам не нужно передавать тип взаимодействия в аргумент. Мы просто пишем функцию, которая в себя принимает два аргумента:
    - `state` - приходит абсолютно всегда и содержит в себе объект состояния.
    - `action` - у которого есть поле `action.payload`. Это поле содержит в себе аргументы, если мы передавали их при вызове функции изменения состояния.

Нам осталось экспортировать функции изменения состояния и сам объект как экспорт по дефолту. Экспортируем функции мы не из поля `reducers`, а из поля `actions`, а экспортирование по дефолту бы делаем поле `reducer`. Все верно, таких полей у нас еще нет, но после инициализации переменной `counterSlice` будут. Это связано с тем, что описано ниже.

**Важно отметить**, что после создания объекта `slice`, он не имеет такую структуру, которую мы ему описали, `Redux Toolkit` под капотом изменяет ее, и наш объект имеет следующие поля:

- `actions` - содержит методы изменения состояния, которые мы прописали в объекте.
- `caseReducers`
- `getInitialState`
- `name` - имя `slice`, которое мы писали в объекте.
- `reducer` - логика, прописанная разработчиками `Redux Toolkit`, которая обрабатывает наши `action` функции.

Теперь нам нужно импортировать наш `slice` в `store`, хранилище для всех `slice`

```jsx
import { configureStore } from '@reduxjs/toolkit'  
import counterReducer from '../features/counter/counterSlice'  
  
export const store = configureStore({  
	reducer: { counter: counterReducer },  
})
```

## Использование состояния в приложении

Теперь мы можем полноценно использовать `Redux Toolkit store` в нашем приложении.  
Для начала, в файл, где мы хотим получить состояния, нужно импортировать хуки для взаимодействия с состоянием, а также `action` функции, которые мы экспортировали из файла `slice`.

```jsx
import React from 'react'  
import { useSelector, useDispatch } from 'react-redux'  
import { decrement, increment } from './counterSlice'  
  
export function Counter() {  
	const count = useSelector((state) => state.counter.value)  
	const dispatch = useDispatch()  
	  
	return (  
		<div>  
			<div>  
				<button  
				aria-label="Increment value"  
				onClick={() => dispatch(increment())}  
		  .     > 				 
				Increment  
				</button>  
				<span>{count}</span>  
				<button  
				aria-label="Decrement value"  
				onClick={() => dispatch(decrement())}  
		  .     > 		 
				Decrement  
				</button>  
			</div>  
		</div>  
	)  
}
```

Из библиотеки `react-redux`, с помощью деструктуризации, мы вытаскиваем два хука:

- `useSelector` - хук для получения информации о глобальном `store`, принимает аргументом callback, который внутри себя всегда несет `store`
- `useDispatch` - хук, для изменения состояния, этот хук будет посылать наши `action` функции.

После импорта, внутри функции проинициализируем две переменные:

- `count` - переменная, хранящая в себе состояние. В аргументе мы указываем, какое конкретно состояние мы хотим получить. Сначала, мы обращаемся к объекту `store`, в данном примере он назван `state` внутри callback функции, затем, указываем название `slice`, которое мы писали внутри `store` объекта, а затем - саму переменную состояния.
- `dispatch` - мы присваиваем ей функционал хука `useDispatch`, так как сам этот хук нельзя использовать внутри callback функций. Как аргумент мы должны передавать `action` функцию импортированную из `slice` объекта

Теперь мы можем использовать взаимодействие с состоянием, например, через кнопки и их свойство `onClick`.