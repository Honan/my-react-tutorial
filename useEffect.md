# useEffect

useEffect вызывается каждый раз при рендеринге компонента.

```jsx harmony
export default function App() {
  const [type, setType] = useState("users");

  useEffect(() => {
    console.log("render");
  });

  return (
    <div className="App">
      <h1>Ресурс: {type}</h1>
      <button onClick={()=> setType("users")}>Пользователи</button>
      <button onClick={()=> setType("todo")}>Todo</button>
      <button onClick={()=> setType("posts")}>Посты</button>
    </div>
  );
}
```

[![Edit mzlp9rz3zy](https://codesandbox.io/static/img/play-codesandbox.svg)](https://codesandbox.io/s/silent-forest-7rliv?file=/src/App.js)

Так же useEffect может принимать в себя второй параметр, где в массиве мы указываем от какой переменной должен зависит
useEffect. В данном случаем мы говорим, если изменилось значение type, вызывать колбэк useEffect.

```jsx harmony
export default function App() {
  const [type, setType] = useState("users");

  useEffect(() => {
    console.log("type change", type);
  }, [type]);

  return (
    <div className="App">
      <h1>Ресурс: {type}</h1>
      <button onClick={()=> setType("users")}>Пользователи</button>
      <button onClick={()=> setType("todo")}>Todo</button>
      <button onClick={()=> setType("posts")}>Посты</button>
    </div>
  );
}
```

С помощью useEffect мы можем эмулировать `componentDidMount`, передав в список зависимостей пустой массив.

```javascript
useEffect(() => {
    console.log("componentDidMount");
  }, []);
```

### Очистка

Каждый эффект может возвращать функцию, которая после него выполнит очистку. React производит очистку, когда компонент 
демонтируется. Однако, как мы уже знаем, эффекты запускаются для каждой отрисовки, а не единожды. Вот почему React также 
очищает эффекты предыдущей отрисовки, прежде чем запускать эффекты снова. 

В данном примере useEffect возвращает функцию, которая будет каждый раз вызываться перед выполнением useEffect!

```jsx harmony
export default function App() {
  const [type, setType] = useState("users");

  useEffect(() => {
    console.log("render");
    return () => {
      console.log("clean type")
    }
  });

  return (
    <div className="App">
      <h1>Ресурс: {type}</h1>
      <button onClick={() => setType("users")}>Пользователи</button>
      <button onClick={() => setType("todo")}>Todo</button>
      <button onClick={() => setType("posts")}>Посты</button>
    </div>
  );
}
```