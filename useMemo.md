# useMemo

У нас есть две кнопки, которые изменяют состояние `number`. Предположим что на основе нового состояния нам нужно вычислить
новое значение, с которым будет работать и это вычисление занимает какое-то время. В данном случае я сделал бесполезный цикл
для наглядности.

```jsx harmony
function complexCompute(num){
  // искусственно большая задержка
  let i = 0;
  while(i < 1000000000) i++;
  return num * 2;
}

export default function App() {

  const [number, setNumber] = useState(42);

  const computed = complexCompute(number);

  return (
    <div className="App">
      <h1>Вычисляемое свойство: {computed}</h1>
      <button onClick={()=> setNumber(prev => prev + 1)}>Добавить</button>
      <button onClick={()=> setNumber(prev => prev - 1)}>Убрать</button>
    </div>
  );
}
```

Давайте добавим еще одну кнопку, которая будет изменять цвет заголовка ("darkred" или "black")

```jsx harmony
function complexCompute(num){
  // искуственно большая задержка
  let i = 0;
  while(i < 1000000000) i++;
  return num * 2;
}

export default function App() {

  const [number, setNumber] = useState(42);
  const [colored, setColored] = useState(false);

  const computed = complexCompute(number);

  const styles = {
    color: colored ? "darkred" : "black"
  };

  return (
    <div className="App">
      <h1 style={styles}>Вычисляемое свойство: {computed}</h1>
      <button onClick={()=> setNumber(prev => prev + 1)}>Добавить</button>
      <button onClick={()=> setNumber(prev => prev - 1)}>Убрать</button>
      <button onClick={()=> setColored(prev => !prev)}>Изменить</button>
    </div>
  );
}
```

Когда мы пытаемся поменять цвет заголовка, все равно происходит задержка! Из-за того что мы меняем состояние цвета
мы вызываем рендер всего компонента, и нужно время для вычисления функции `complexCompute`. Как раз в помощью `useMemo`
можно оптимизировать данный процесс.

У нас есть значение `computed`, которое мы можем закэшировать. Если значение `number` не изменилось, но нам нет необходимости 
вызывать эту функцию. Поэтому `useMemo` первым параметром мы передаем функцию, которая возвращает значение `complexCompute(number)`,
а втором параметром значение зависимое от `complexCompute(number)` в нашем случае это `number`. Т.е. вычисления зависят
`number`.

```jsx harmony
function complexCompute(num){
  // искуственно большая задержка
  let i = 0;
  while(i < 1000000000) i++;
  return num * 2;
}

export default function App() {

  const [number, setNumber] = useState(42);
  const [colored, setColored] = useState(false);

  const computed = useMemo(() => {
    return complexCompute(number);
  }, [number]);

  const styles = {
    color: colored ? "darkred" : "black"
  };

  return (
    <div className="App">
      <h1 style={styles}>Вычисляемое свойство: {computed}</h1>
      <button onClick={()=> setNumber(prev => prev + 1)}>Добавить</button>
      <button onClick={()=> setNumber(prev => prev - 1)}>Убрать</button>
      <button onClick={()=> setColored(prev => !prev)}>Изменить</button>
    </div>
  );
}
```

Есть и другое применение данного хука. Если мы хотим при помощи `useEffect` отслеживать состояние объекта.
К примеру, `styles` из примера выше. То при каждом рендере этот объект будет разным! На помощью тоже может прийти `useMemo`
который может взять и сохранить этот объект на следующий рендер.

```jsx harmony
function complexCompute(num){
  // искуственно большая задержка
  let i = 0;
  while(i < 1000000000) i++;
  return num * 2;
}

export default function App() {

  const [number, setNumber] = useState(42);
  const [colored, setColored] = useState(false);

  const computed = useMemo(() => {
    return complexCompute(number);
  }, [number]);

  const styles = useMemo(()=> {
    return {
      color: colored ? "darkred" : "black"
    }
  }, [colored]);

  useEffect(() => {
    console.log("Styles changed");
  }, [styles]);

  return (
    <div className="App">
      <h1 style={styles}>Вычисляемое свойство: {computed}</h1>
      <button onClick={()=> setNumber(prev => prev + 1)}>Добавить</button>
      <button onClick={()=> setNumber(prev => prev - 1)}>Убрать</button>
      <button onClick={()=> setColored(prev => !prev)}>Изменить</button>
    </div>
  );
}
```