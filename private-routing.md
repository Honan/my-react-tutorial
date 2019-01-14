# Private routing

Иногда нужно организовывать приватные `Route`. Создаем контекст через провайдер прокидываем состояние авторизованы ли мы и страницу куда мы перенаправим, если мы не авторизованы. Через HOC мы получили эти переменные и передали в виде props оборачиваемого компонента. Применили наш HOC на нашем `PrivateRoute` передав ему состояние, авторизованы ли мы. Если мы авторизованы, то автоматически редиректим на корневую страницу.

```javascript
const { Provider: AuthProvider, Consumer: AuthConsumer } = React.createContext({
  isAuthorized: false
});

export class PrivateExample extends Component {
  loginPath = "/login";
  state = {
    isAuthorized: false
  };

  authorize = () => {
    this.setState({ isAuthorized: true });
  };

  render() {
    const { isAuthorized } = this.state;
    return (
      <AuthProvider
        value={
          {
            isAuthorized,
            authorize: this.authorize,
            loginPath: this.loginPath
          }
        }
      >
        <Link to="/public">Public</Link> <Link to="/private">Private</Link>{" "}
        <Link to="/login">Login</Link>
        <hr />
        <Switch>
          <Route path="/public" render={() => <p>Public</p>} />
          <Route path="/login" component={LoginPage} />
          <PrivateRoute path="/private" component={() => <p>Private</p>} />
          <Redirect to="/public" />
        </Switch>
      </AuthProvider>
    );
  }
}

let LoginPage = ({ isAuthorized, authorize }) =>
  isAuthorized ? (
    <Redirect to="/" />
  ) : (
    <button onClick={authorize}>Authorize</button>
  );

LoginPage = withAuth(LoginPage);

function withAuth(WrappedComponent) {
  return class AuthHOC extends Component {
    render() {
      const { ...rest } = this.props;
      return (
        <AuthConsumer>
          {contextProps => <WrappedComponent {...contextProps} {...rest} />}
        </AuthConsumer>
      );
    }
  };
}

let PrivateRoute = ({
  component: RouteComponent,
  isAuthorized,
  loginPath,
  ...rest
}) => (
  <Route
    {...rest}
    render={routeProps =>
      isAuthorized ? (
        <RouteComponent {...routeProps} />
      ) : (
        <Redirect to={loginPath} />
      )
    }
  />
);

PrivateRoute = withAuth(PrivateRoute);
```

[![Edit 6w3xjp6v1r](https://codesandbox.io/static/img/play-codesandbox.svg)](https://codesandbox.io/s/6w3xjp6v1r)