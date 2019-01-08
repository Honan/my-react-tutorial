# Simple routing

[React Router](https://github.com/ReactTraining/react-router) - это библиотека, позволяющая описывать роутинг.

Первое что мы делаем, это оборачиваем наше приложение в `BrowserRouter`, который реализует навигацию в браузере. Это HOC, который пробрасывает проперти в контекст и дальше мы потребляем при помощи специальных компонентов в данном примере это компоненты `link` и `Route`.

При помощь компонента `link` создаем ссылки. `Route` позволяет организовать сам роутинг. Если посмотреть в дерево компонентов, то ссылки рендеряться как обычные браузерные ссылки, по сути обертка в виде `Link` нужна для того чтобы перехватить event нажатие на ссылку и предотвратить обычное поведение при котором страница перезагружается.

После того, как мы нажали на ссылку, меняется url в строке браузера и об этом узнает `BrowserRouter` он смотрит соответствие url c `Route`. У каждого `Route` есть несколько атрибутов:

- `path` используется для того, чтобы определить за какой url отвечает конкретный `Route`.
- `compenent` куда мы передаем собственно сам компонент, который хотим отрендерить.
- `exact`. Если есть несколько соответствие между url и `Route`. С помощью `exact` мы требуем точного совпадения url и `Route`.
- Так же может быть и renderProps, куда мы передаем функцию, которую получит props и на основании этой функции отрендерит компонент.

> Лучше всего избегать использования renderProps

Для наглядности с помощью `console.log()` можно посмотреть какие проперти `Route` передает в дочерний компонент. Он передает объект с полями:

- history. Это объект history API, предоставляется браузером. Можем управлять историей браузера, переходить на конкретное кол-во записей вперед, может записать в стек новую запись, перезаписать последнюю и т.д.
- location. Показывает нам разбитую информацию из url.
- match

```javascript
let Home = props => {
  console.log(props);
  return (
    <>
      <Link to="/about">About</Link>
      <div>Home</div>
    </>
  );
};

Home = withRouter(Home);

const About = props => {
  console.log(props);
  return <div>About</div>;
};

const Topics = props => {
  console.log(props);
  return <div>Topics</div>;
};

class SimpleRouter extends Component {
  state = {
    userName: 'Denis',
  };

  render() {
    const { userName } = this.state;
    return (
      <div className="App">
        <Link to="/">Home</Link>
        <Link to="/about">About</Link>
        <Link to="/topics">Topics</Link>

        <hr />
        <Route path="/" component={Home} exact />
        <Route path="/home/:id" component={Home} />
        <Route path="/about*" component={About} />
        <Route
          path="/topics"
          render={props => (
            <Topics {...props} userName={userName} />
          )}
        />
      </div>
    );
  }
}
```
[![Edit mz24qwz3v8](https://codesandbox.io/static/img/play-codesandbox.svg)](https://codesandbox.io/s/mz24qwz3v8?expanddevtools=1)