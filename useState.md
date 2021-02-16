# useState

useState предназначен для взаимодействия со state. 

useState - это функция, которая возвращает массив. Где первый элемент массива значения состояние. Второй элемент - функция которая позволяет менять это состояние.

Давайте создадим счетчик использую useState. Хорошей практикой считается определение состояние в начале компонента.

```jsx harmony
export default function App() {

  const [counter, setCounter] = useState(0);

  function increment(){
    setCounter(counter + 1);
  }

  function decrement(){
    setCounter(counter - 1);
  }

  return (
    <div className="App">
      <h1>Счетчик: {counter} </h1>
      <button onClick={increment}>Добавить</button>
      <button onClick={decrement}>Убрать</button>
    </div>
  );
}
```

[![Edit mzlp9rz3zy](https://codesandbox.io/static/img/play-codesandbox.svg)](https://codesandbox.io/s/zen-grothendieck-1vsrx?file=/src/App.js)

Ограничение.
Задавать состояние в блоке `if`. React выдаст ошибку

```jsx harmony
export default function App() {
    if(true){
       const [counter, setCounter] = useState(0);
    }
}
```

## Особенности

### Данный хук работает **асинхронно**
Если мы попробуем изменить функцию `increment` и добавить еще `setCounter`

```javascript
function increment(){
    setCounter(counter + 1);
    setCounter(counter + 1);
}
```

При нажатии на кнопку добавить ничего не измениться. Сработает первый `setCounter` пойдет перерендер страницы и сработает второй `setCounter`, но при этом само значение counter еще не успеет измениться!

`setCounter` может принимать в себя колбэк функцию

```javascript
function increment(){
    setCounter((prevCounter) => {
        return prevCounter + 1;
    });
    // или более кратко
    setCounter(prev => prev + 1);
}
```

### Вычисляемое начальное состояние

Если мы хотим вычислить начальное состояние.

```jsx harmony
import { useState } from "react";
import "./styles.css";

function computeInitialCounter() {
  console.log("some calculations");
  return Math.trunc(Math.random() * 20);
}

export default function App() {
  const [counter, setCounter] = useState(computeInitialCounter());

  function increment() {
    setCounter(counter + 1);
  }

  function decrement() {
    setCounter(counter - 1);
  }

  return (
    <div className="App">
      <h1>Счетчик: {counter} </h1>
      <button onClick={increment}>Добавить</button>
      <button onClick={decrement}>Убрать</button>
    </div>
  );
}
```

Не случайно в функции computeInitialCounter прописан console.log(). Мы можем заметить что рендеринг происходит два раза.
И если я увеличиваю счетчик функция computeInitialCounter вызывается два раза. Т.е. на любое изменения стейта функция вызывается
два раза.

Для оптимизации этого процессе в useState мы можем передавать функцию. Которая один раз вычислит значение и не будет
повторно вызываться.

```jsx harmony
import { useState } from "react";
import "./styles.css";

function computeInitialCounter() {
  console.log("some calculations");
  return Math.trunc(Math.random() * 20);
}

export default function App() {
  const [counter, setCounter] = useState(() => {
    return computeInitialCounter();
  });

  function increment() {
    setCounter(counter + 1);
  }

  function decrement() {
    setCounter(counter - 1);
  }

  return (
    <div className="App">
      <h1>Счетчик: {counter} </h1>
      <button onClick={increment}>Добавить</button>
      <button onClick={decrement}>Убрать</button>
    </div>
  );
}
```

### Состояние в виде объекта

Если у нас состояние в виде объекта (*но обычно так его не используют*)

```javascript
const [counter, setCounter] = useState({
    title: "Счетчик",
    date: Date.now()
}); 
```

И мы хоти поменять только свойство `title`. Мы должны вернуть целиком объект с измененным состоянием. А не только то св-во,
которое хотим поменять. Отличие от классовых компонент.

**bad**
```javascript
setState({title: "Новый заголовок"})
```

**good**
```javascript
setState({
    title: "Новый заголовок",
    date: Date.now()
})
```