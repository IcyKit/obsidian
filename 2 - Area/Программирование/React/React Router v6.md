# React Router v6

Новая версия роутера привнесла огромные изменения, и вся библиотека теперь используется по другому.

Примеры будут показаны стандартном приложении `create-react-app`.

Для начала - создадим роутер в файле `index.js`

```tsx
import { createBrowserRouter, RouterProvider, Route } from "react-router-dom";

const router = createBrowserRouter([
  {
    path: "/",
    element: <App />,
    errorElement: <NotFound />childs: [
      {
	    path: '/',
	    element: <Home />},
	  {
	    path: "cart",
	    element: <Cart />}
    ]
  },
  
]);

const root = ReactDOM.createRoot(document.getElementById("root"));
root.render(
  <React.StrictMode>
    <RouterProvider router={router} />
  </React.StrictMode>);
```

Данный роутер говорит нам о том, что в корневом пути сайта, т.е. `/` у нас будет появляться компонент `<App />`.

При этом, мы хотим, чтобы в `App.js` обязательно был хедер и футер, а контент показывался разный, в зависимости от пути.

`App.js` содержит следующий код

```tsx
import { Outlet } from 'react-router-dom';

return (
	<div className="wrapper">
	      <Header />
	      <div>
	        <Outlet />
	      </div>
	      <Footer />
	</div>)
```

Компонент `Outlet` говорит роутеру о том, что вместо этого тега будет появляться контент, который находится в константе `index.js`.

В нашем случае, если путь равен `/`, то появляется стандартная верстка из `App.js`, а также верстка компонента `Home`. А если путь равен `/cart`, то вместо компонента `Home` появится компонент `Cart`.

Помимо этого, у нас есть ключ `errorElement`, он появляется, когда путь, который запрашивает пользователь не существует.

Чтобы мы могли использовать все преимущества **React Router**, то мы должны сменить все теги `a`, которые содержат ссылки на другие разделы сайта, на тег

```tsx
<Link to={путь}>Ссылка</Link>.
```

Этот тег позволяет переходить по сайту без перезагрузок страницы.

Пример использования:

```tsx
<nav>
  <ul>
	<li>
	  <Link to={`contacts/1`}>Your Name</Link>
	</li>
	<li>
	  <Link to={`contacts/2`}>Your Friend</Link>
	</li>
  </ul>
</nav>
```