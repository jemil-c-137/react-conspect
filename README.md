# react-conspect
My own and awful conspect on react. Do not read this if you want to keep your mental health safe.

# Нельзя взаимодействовать с DOM напрямую

в React нельзя напрямую обращаться к DOM, вешать события и тд. Для этого есть VirtualDOM

![image](https://yadi.sk/i/CC63YHO4NeoyrQ)

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

![image](https://yadi.sk/i/CC63YHO4NeoyrQ)


