# Context

Context позволяет пробрасывать объекты, функции, какие-то данные по всему приложению. Если данные должны быть доступны из любого компонента, то используется Context.
> С помощью контекста работают библиотеки, связывающие react и redux, react-router, redux-form, все библиотеки которым необходимо интегрировать свою логику в ваши компоненты.

```React.createContext("")``` возвращает объект, в котором мы переопередили название свойств данного объекта в ```ThemeConsumer``` и ```ThemeProvider```. Затем в ```ThemeProvider``` мы закидываем данные, которые хотим передать. А в  ```ThemeConsumer``` эти данные мы можем получить, вызываем функцию, в параметрах которых приходят данные, обрабатываем их и возвращаем сформированный элемент.

> При каждом вызове ```React.createContext("")```, реакт будет выдавать новую пару Consumer и Provider.

## Provider

Провайдеру можно передать через props новые данные, с помощью поля value, таким образом, можно изменять данные, которые транслируются через context. При создании контекста указываются значения по умолчанию.

## Consumer

Консъюмер предоставляет данные с помощью renderProps, только в случае компоненты Consumer используется рендер функция, передачу которой компонент ожидает в children.

```javascript
const {
  Consumer: ThemeConsumer,
  Provider: ThemeProvider
} = React.createContext("");

const Button = () => (
    <ThemeConsumer>
        {({ theme, toggleTheme }) => (
            <button
                onClick={toggleTheme}
                style={{
                    backgroundColor: theme === "light" ? "#666" : "#eee"
                }}
            >
            Кнопка с темой
            </button>
        )}
    </ThemeConsumer>
);

class ContextApiProviderExample extends Component {
    state = {
        theme: "light"
    };

    toggleTheme = () => {
        this.setState(state => ({
            theme: state.theme === "light" ? "dark" : "light"
        }));
    };

    render() {
        const { theme } = this.state;
        return (
            <ThemeProvider value={{ theme, toggleTheme: this.toggleTheme }}>
                <Button />
            </ThemeProvider>
        );
    }
}
```

P.S Есть различные паттерны, для удобной работы с контекстом.