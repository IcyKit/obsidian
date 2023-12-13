# Props

Пропсы задаются когда мы вставляем компонент в приложение.

Пропсами могут быть строки, объекты, функции

```tsx
<h1>My name is {name()}, surname - {surname}</h1> // Передача name стоит в виде функции
<a href={link}>My profile</a>

<WhoAmI name='Nikita' surname='Efimov' link='<https://github.com/IcyKit228>' />

<WhoAmI name={{ firstName: 'Nikita' }} surname='Efimov' link='<https://github.com/IcyKit228>' />

<WhoAmI name={() => { return 'Niktia' }} surname='Efimov' link='<https://github.com/IcyKit228>' />
```

Помимо этого, мы можем передавать пропсы через spread оператор от полученного объекта запроса, если мы знаем, что ключи в полученном объекте, будут совпадать со значениями, которые мы деструктуризируем.

```tsx
<Компонент {...полученный_объект} />
```