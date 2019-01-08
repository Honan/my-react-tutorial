# Recompose

[Recompose](https://github.com/acdlite/recompose) - библиотека с HOCками.

Перейдем сразу к примеру. У нас будет группа из 3 кнопок, по нажатию на кнопку она будет выделяться красным. Все кнопки будут в одном компоненте. Нам нужно будет хранить какой-то state, обрабатывать события. Но в компоненте этого ничего не будет, а всю функциональность мы реализуем при помощи библиотеки Recompose.

### Импорты

  - **сompose**. Так как мы будем использовать несколько HOCов, то эта функция позволит сделать их композицию и записать в более компактном формате.
  - **withState**. Данный HOC позволяет хранить state. И принимает 3 аргумента. Название поля, функция для изменения этого поля, начальное значение.
  - **withHandlers**. Обрабатывает события. Он принимает в себя объект, у которого полями будут название событий (в нашем случае toggleButton), а значение этих полей будут функции, которые принимают объект props и возвращают хендлеры.
  - **mapProps**. С его помощью можно обрезать лишние props, который будет передаваться в компонент. Данные HOC получает функцию в виде атрибута, которая получает текущие props и возвращает новые.
  - **pure**. Благодаря этому HOC мы избежим лишних рирендоров.

```javascript
import {
    withState,
    withHandlers,
    mapProps,
    pure,
    compose
    } from "recompose";

const Buttons = ({ buttons, toggleButton, selectedIndex }) => (
  <div>
    {buttons.map(index => (
      <button
        onClick={toggleButton}
        data-key={index}
        key={index}
        style={Object.assign(
          {},
          selectedIndex.includes(index) && {
            backgroundColor: "red"
          }
        )}
      >
        {index}
      </button>
    ))}
  </div>
);

const enhance = compose( // позволяет записать HOC в более компактном формате
  withState("selectedIndex", "changeSelectedIndex", []), // state для хранение выделенной кнопки
  withState("buttons", "changeButtons", ["😀", "😅", "😔"]), // Название поля, функция для изменения этого поля, начальное значение
  withHandlers({
    toggleButton: props => event => {
      const index = event.target.dataset.key;
      props.changeSelectedIndex(() => [index]);
    }
  }),
  mapProps(({ changeSelectedIndex, changeButtons, ...rest }) => rest), // выделяем props, которые нам не нужны
  pure
);

export default enhance(Buttons);
```

[![Edit znvjkpmmzl](https://codesandbox.io/static/img/play-codesandbox.svg)](https://codesandbox.io/s/znvjkpmmzl)