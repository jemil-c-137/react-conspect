# react-conspect

My own and awful conspect on react. Do not read this if you want to keep your mental health safe.

# Нельзя взаимодействовать с DOM напрямую

в React нельзя напрямую обращаться к DOM, вешать события и тд. Для этого есть VirtualDOM

![image](./imgs/dom.png)

```JSX
document.getElementById, addEventListener //нельзя
```

такие взаимодействия делать нельзя, т.к. мы не знаем текущее состояние DOM дерева, ведь оно динамеческое.

Можно обращаться к элементам по ссылке `ref` хотя и это не рекомендуется. Для этого существует метод

```JSX
React.createRef()
```

Этот метод создает ссылку на элемент, по которой позже можно к нему обращаться.

## Как использовать

Помещаем метод в переменную

```JSX
let newDiv = React.createRef();
```

У элемента добавляем атрибут `ref={}` с переменной внутри

```JSX
<div ref={ newDiv }>
  Some text
</div>
```

теперь в `newDiv` хранится ссылка на элемент `<div></div>`

после метода добавляем `current`, что бы считывать элемент который хранится по ссылке
`newDiv.current.innerHTML`

## Пример

```javascript
let newDiv = React.createRef();

<div ref={newDiv}>Some text</div>;

let printText = () => {
  const text = newDiv.current.innerHTML;
  console.log(text);
};
```

## Но и так поступать не слудует

По этой ссылке мы все равно получаем доступ к DOM элементу(хоть только и для чтения), а т.к. мы используем React то нам нужно обращаться к VirtualDOM

![image](./imgs/dom.png)

## Изменения и ререндер

Что бы добавлять изменения произошедшие на странице в разметку(например добавление нового поста, или сообщения) нам нужно сначала добавить эти изменения в место хранения наших данных, BLL(Bussines logic layaer)

Для этого там нужно хранить функцию которая будет добавлять поступившие данные в уже имющиеся.

```javascript
export let addPost = (postMessage) => {
  let post = {
    id: 5,
    text: postMessage,
    value: 2,
  };

  state.profilePage.postsData.push(post); // добавляем объект в state
};
```

Создаем переменную для хранения ссылки на элемент, данные которого будем добавлять. Объявляем колбэк функцию, которая будет вызывать нашу функцию добавляющую объект в state.

```javascript
let newPost = React.createRef();

let printPost = () => {
  let post = newPost.current.value;
  props.addPost(post);
};
```

Вызываем колбэк по клику на кнопку.

```html
<textarea ref="{newPost}"></textarea>
<button onClick="{" printPost } className="{classes.button}">Add post</button>
```

Но тут у нас может возникать проблема
В state уже будут новые данные, но в разметке они не отобразятся.
Все дело в рендере

```JSX
ReactDOM.render(
  <React.StrictMode>
    <App state={state} addPost={addPost}/>
  </React.StrictMode>,
  document.getElementById('root')
);
```

Приложение у нас рендерится единожды, с имеющимеся на этот момент данными в `state`, что бы передать внутрь приложения через `props` новый `state` нам нужно заного вызвать рендер всего приложения.

Так же данные могут обновляться при перекличение по `<NavLink>`.

У `<Route />` есть атрибут: `render={() => <Component />}`

Когда route видит совпадение путей в адресной строке, он заного рендерит компонент с новым `state`.

## 33) Взаимодействие BLL(state/Redux) и UI(React)

Важно понимать, что данные нужно хранить в state и новые данные так же.

UI должен только рендерить данные которые ему прилетают.

Например, у нас есть кнопка добавляющая новый пост. По клику на кнопку, не UI должен пушить новые данные в `state`, а BLL.

После клика, UI вызывает колбек, который вызывают функцию находящуюся в `state`,
эта функция добавляет новый пост в `state`, и далее `state` заного вызывает рендер, с новыми данными

Схематично:

```javascript
BLL => Render => Click(event) => UI func(new data) => BLL func(push data) => BLL (new state) => Render
```
