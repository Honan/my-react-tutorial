# Switch routing

В `Switch` мы оборачиваем `Route`. `Switch` проверяет соответствие url с `path` и рендерит только тот `Route` у которого первым встретит соответствие. Таким образом он отрендерит только один `Route`.

В `Switch` мы можем использовать компонент `Redirect`. У него нет `path` и если не один из `Route` не подошел, то он перейдет на страницу, которую мы укажем.

Так же мы можешь сделать страницу 404 и в `path` прописать звездочку. Если не один путь не подошел, то перенаправляем пользователя на страницу с ошибкой.

> Библиотека React Router практически всегда используется с компонентом `Switch`

```javascript
const About = () => <div>About</div>;
const Home = () => <div>Home</div>;
const Topics = () => <div>Topics</div>;
// const PageNotFound = () => <div>404 Not found</div>;

class App extends Component {
  render() {
    return (
      <div className="App">
        <Link to="/">Home</Link>
        <Link to="/about">About</Link>
        <Link to="/topics">Topics</Link>

        <hr />

        <Switch>
          <Route path="/" component={Home} exact />
          <Route path="/about" component={About} />
          <Route path="/topics" component={Topics} />
          {/* <Route path="*" component={PageNotFound} /> */}
          <Redirect from="/about2" to="/about" />
          <Redirect to="/" />
        </Switch>
      </div>
    );
  }
}
```

[![Edit zz627my4x](https://codesandbox.io/static/img/play-codesandbox.svg)](https://codesandbox.io/s/zz627my4x)