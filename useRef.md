# useRef

Допустим у нас есть задача подсчитать сколько раз мы рендерили компонент.
Функция useRef возвращает одно значение. Даже правильным будет сказать, что функция возвращает объект, у которого есть
свойство **current**. Значения, которые мы определяем через *useRef* сохраняются между рендерами компонентов. И при изменении
не вызывает рендер компоненты.

```jsx harmony
export default function App() {

  const [value, setValue] = useState("initial");
  const renderCount = useRef(1);

  useEffect(()=>{
    renderCount.current++
  });

  return (
    <div className="App">
      <h1>Количество рендеров: {renderCount.current}</h1>
      <input type="text" onChange={e=> setValue(e.target.value)} value={value}/>
    </div>
  );
}
```

[![Edit mzlp9rz3zy](https://codesandbox.io/static/img/play-codesandbox.svg)](https://codesandbox.io/s/compassionate-almeida-ibu7u?file=/src/App.js)

Также с помощью useRef можно получать ссылки на различные DOM элементы. К примеру, мы можешь посмотреть свойство value 
при каждом изменении значения input.

```jsx harmony
export default function App() {
  const [value, setValue] = useState("initial");
  const renderCount = useRef(1);
  const inputRef = useRef(null);

  useEffect(() => {
    renderCount.current++;
    console.log(inputRef.current.value);
  });

  return (
    <div className="App">
      <h1>Количество рендеров: {renderCount.current}</h1>
      <input
        ref={inputRef}
        type="text"
        onChange={(e) => setValue(e.target.value)}
        value={value}
      />
    </div>
  );
}
```

С помощью useRef можно получить значение предыдущего состояния.

```jsx harmony
export default function App() {
  const [value, setValue] = useState("initial");
  const prevValue = useRef("");

  useEffect(() => {
    prevValue.current = value
  }, [value]);

  return (
    <div className="App">
      <h1>Прошлое состояние: {prevValue.current}</h1>
      <input
        type="text"
        onChange={(e) => setValue(e.target.value)}
        value={value}
      />
    </div>
  );
}
```
 