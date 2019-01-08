# Nested routing

`Route` в react-router-dom динамические. Это значит, что они рассчитываются на этапе рендера. Это позволяет создавать сложную структуру навигации и иметь вложенные `Route`. При работе с вложенными `Route` активно используется проп `match`. В нем нам интересно 3 поля:

- isExact. Данное поле показывает совпал ли `path` у `Route` c url.
- path. Показывает `path Route`.
- url. Описывает наше текущие положение с точки зрения url адресной строке

> Если нужно будет вставлять ссылки или изображения то мы используем поле url, а если нужно постепенно добавлять все новые уровни вложенности к нашему `Route` то мы будет использовать path

```javascript
const CatPhotoComponent = ({ match }) => {
  console.log(match);
  return <p>cat {match.params.id} Photo {match.params.photoId}</p>;
};

const CatComponent = ({ match }) => {
  const { id } = match.params;

  return (
    <div>
      <h3>Cat {id}</h3>
      <div>
        <Link to={`${match.url}/photo/1`}>Photo 1</Link>{' '}
        <Link to={`${match.url}/photo/2`}>Photo 2</Link>{' '}
        <Link to={`${match.url}/photo/3`}>Photo 3</Link>{' '}
        <Link to={`${match.url}/photo/4`}>Photo 4</Link>{' '}
        <Link to={`${match.url}/photo/5`}>Photo 5</Link>
      </div>

      <div>
        <Route
          path={`${match.path}/photo/:photoId`}
          component={CatPhotoComponent}
        />
      </div>
    </div>
  );
};

const Hobbies = ({ match }) => {
  console.log(match)
  return (
    <div>
      <p>Hobbies</p>
      <div>
        <Link to={`${match.url}/cat/1`}>Cat 1</Link>{' '}
        <Link to={`${match.url}/cat/2`}>Cat 2</Link>{' '}
        <Link to={`${match.url}/cat/3`}>Cat 3</Link>{' '}
        <Link to={`${match.url}/cat/4`}>Cat 4</Link>{' '}
        <Link to={`${match.url}/cat/5`}>Cat 5</Link>
      </div>

      <div>
        <Route
          path={`${match.path}/cat/:id`}
          component={CatComponent}
        />
      </div>
    </div>
  );
};

const App = () => (
  <div>
    <nav>
      <Link to="/">Home</Link>{' '}
      <Link to="/Hobbies">Hobbies</Link>
    </nav>

    <hr />

    <div>
      <Switch>
        <Route path="/" render={() => <p>Home</p>} exact />
        <Route path="/Hobbies" component={Hobbies} />
      </Switch>
    </div>

  </div>
);
```

[![Edit 04r30r7vw0](https://codesandbox.io/static/img/play-codesandbox.svg)](https://codesandbox.io/s/04r30r7vw0?expanddevtools=1)