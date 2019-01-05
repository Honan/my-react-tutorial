# RenderProps

RenderProps это простая техника разделения кода между компонентами, используя props переменную как функцию, которая возвращает jsx.

Одним из способов иметь одинаковую функциональность у компонентов являются HOC. Но есть и другой способ, можно это делать с помощью паттерна RenderProps. Этот паттерн проще ;).

> HOC и RenderProps это два паттерна взаимозаменяемые

Основная идея: В props мы передаем функцию render. Которую используем внутри рендера компонента, куда собственно ее и передали.

```javascript
class WithWindowSize extends PureComponent {
    state = {
        width: window.innerWidth
    };

    componentDidMount() {
        window.addEventListener("resize", this.onResize);
    }

    componentWillUnmount() {
        window.removeEventListener("resize", this.onResize);
    }

    onResize = () => {
        this.setState({ width: window.innerWidth });
    };

    render() {
        const { width } = this.state;
        const { render } = this.props; // принимаем функцию render
        return render(width);
    }
}

const RenderPropsExample = () => (
  <WithWindowSize
    render={width => <pre>windowWidth: {width}</pre>} // передаем функцию render
  />
);
```
[![Edit 7ky5027w21](https://codesandbox.io/static/img/play-codesandbox.svg)](https://codesandbox.io/s/7ky5027w21)