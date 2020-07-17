# react-conspect
My own and awful conspect on react. Do not read this if you want to keep your mental health safe.

# Нельзя взаимодействовать с DOM напрямую

в React нельзя напрямую обращаться к DOM, вешать события и тд. Для этого есть VirtualDOM

![image](https://2.downloader.disk.yandex.ru/preview/5aa89dc6bf4443cb11dfe0c940a815231ba99698ec70551527bed1fbb8858455/inf/pr-T82kzDj9pNoKeEctMR8ErQIAKdH0k-e4wSMz3ZF26-AzBoUBHEVSGvx9puZj8XOxGlNz9LOaqkI4VmOJWww%3D%3D?uid=603034236&filename=2020-07-17_22-55-05.png&disposition=inline&hash=&limit=0&content_type=image%2Fpng&owner_uid=603034236&tknv=v2&size=1903x931)

```JSX
document.getElementById, addEventListener //нельзя
```
такие взаимодействия делать нельзя, т.к. мы не знаем текущее состояние DOM дерева, ведь оно динамеческое.

Можно обращаться к элементам по ссылке ```ref``` хотя и это не рекомендуется. Для этого существует метод 
```JSX
React.createRef()
```
Этот метод создает ссылку на элемент, по которой позже можно к нему обращаться.

## Как использовать
Помещаем метод в переменную
```JSX
let newDiv = React.createRef();
```


У элемента добавляем атрибут ```ref={}``` с переменной внутри

```JSX
<div ref={ newDiv }>
  Some text
</div>
```
теперь в ```newDiv``` хранится ссылка на элемент `<div></div>`

после метода добавляем ```current```, что бы считывать элемент который хранится по ссылке
```newDiv.current.innerHTML```

## Пример
```javascript
let newDiv = React.createRef();

<div ref={ newDiv }>
  Some text
</div>

let printText = () => {
  const text = newDiv.current.innerHTML;
  console.log(text);
}
```

## Но и так поступать не слудует

По этой ссылке мы все равно получаем доступ к DOM элементу(хоть только и для чтения), а т.к. мы используем React то нам нужно обращаться к VirtualDOM

![image](https://2.downloader.disk.yandex.ru/preview/5aa89dc6bf4443cb11dfe0c940a815231ba99698ec70551527bed1fbb8858455/inf/pr-T82kzDj9pNoKeEctMR8ErQIAKdH0k-e4wSMz3ZF26-AzBoUBHEVSGvx9puZj8XOxGlNz9LOaqkI4VmOJWww%3D%3D?uid=603034236&filename=2020-07-17_22-55-05.png&disposition=inline&hash=&limit=0&content_type=image%2Fpng&owner_uid=603034236&tknv=v2&size=1903x931)


