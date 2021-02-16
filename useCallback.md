# useCallback

У нас есть две кнопки, первая увеличивает значение счетчика, вторая меняет цвет заголовка.

```jsx harmony
export default function App() {

  const [count, setCount] = useState(1);
  const [colored, setColored] = useState(false);

  const styles = {
    color: colored ? "darkred" : "black"
  };

  return (
    <div className="App">
      <h1 style={styles}>Количество элементов: {count}</h1>
      <button onClick={()=> setCount(prev => prev + 1)}>Добавить</button>
      <button onClick={()=> setColored(prev => !prev)}>Изменить</button>
    </div>
  );
}
```

Предположим, что у нас есть функция, которая позволяет на основе `count` генерировать кол-во элементов, которое мы хотим
вывести в другой компонент. 

```javascript
const generateItemsFromAPI = () => {
    return new Array(count).fill("").map((_, i) => `Элемент ${i}`)
}
```

Теперь эту функцию я хочу передать другому компоненту, где и будем ее использовать.

```jsx harmony
export default function ItemsList({ getItems }) {
  const [items, setItems] = useState([]);

  useEffect(() => {
    const newItems = getItems();
    setItems(newItems);
  }, [getItems]); // в зависимости функция!

  return (
    <ul>
      {items.map((i) => (
        <li key={i}>{i}</li>
      ))}
    </ul>
  );
}
```

[![Edit mzlp9rz3zy](https://codesandbox.io/static/img/play-codesandbox.svg)](https://codesandbox.io/s/objective-jackson-s0sbk?file=/src/App.js)

Но возникает проблемы с производительностью. Давайте проследим сколько раз вызывается `useEffect` в компоненте `ItemsList`
`useEffect` вызывается всякий раз, когда мы меняем состояние. Т.к. в зависимостях у нас функция, а не переменная!

Поэтому мы должны закэшировать функцию `generateItemsFromAPI` с помощью `useCallback`.

```javascript
const generateItemsFromAPI = useCallback(() => {
    return new Array(count).fill("").map((_, i) => `Элемент ${i}`);
  }, [count]);
```

А вот целиком компонент 

```jsx harmony
export default function App() {
  const [count, setCount] = useState(1);
  const [colored, setColored] = useState(false);

  const generateItemsFromAPI = useCallback(() => {
    return new Array(count).fill("").map((_, i) => `Элемент ${i}`);
  }, [count]);

  const styles = {
    color: colored ? "darkred" : "black"
  };

  return (
    <div className="App">
      <h1 style={styles}>Количество элементов: {count}</h1>
      <button onClick={() => setCount((prev) => prev + 1)}>Добавить</button>
      <button onClick={() => setColored((prev) => !prev)}>Изменить</button>
      <ItemsList getItems={generateItemsFromAPI} />
    </div>
  );
}
```