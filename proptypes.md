# Типизация данных

Когда проект сильно разрастается и над ним работает минимум два разработчика. Для того, чтобы отслеживать правильность работы компонент между собой потребуется типизация. Если компонент ожидает строку, а в него пришло число. То уже стоит призадуматься, а все ли правильно я делаю? Реакт предоставляет инструмент для описания типов props компонент, и выводит ошибки в консоли браузера если типы не сходятся.

Cтатическая реализация реализуется с помощью
- Flow
- TypeScript
> Эти два варианта подразумевают надстройку над синтаксисом js.

Помимо статической типизации есть и **динамическая** с помощью PropTypes. Раньше данная библиотека входила в реакт, сейчас же нам нужно ее импортировать.

Объявляем статическое поле класса PropTypes. В которое записываем объект где указываем тип переменной.

> Обратите внимание в defaultProps можно присвоить **только** значение объекта верхнего уровня

```javascript
class Child extends Component {

    static propTypes = {
        name: PropTypes.string.isRequired, // Обязательный параметр
        age: PropTypes.number,
        isPupil: PropTypes.bool.isRequired
    };

    static defaultProps = { // устанавливаем props по умолчанию
        isPupil: true
    };

    render() {
        const { name, age } = this.props;
        return (
        <div>
            <p>Hello, {name}!</p>
            <p>Age, {age}!</p>
        </div>
        );
    }
}

class PropTypesExample extends Component {
  render() {
    return (
      <div>
        <Child name="Денис" age={24} />
      </div>
    );
  }
}
```
[![Edit m4ox737ply](https://codesandbox.io/static/img/play-codesandbox.svg)](https://codesandbox.io/s/m4ox737ply)

А это список типов, которые доступны в PropTypes:

```
MyComponent.propTypes = {
 // Все PropTypes опциональны, и делают проверку только на соответствие типа
 optionalArray: PropTypes.array,
 optionalBool: PropTypes.bool,
 optionalFunc: PropTypes.func,
 optionalNumber: PropTypes.number,
 optionalObject: PropTypes.object,
 optionalString: PropTypes.string,
 optionalSymbol: PropTypes.symbol,
 // Этот тип описывает любой элемент, который реакт может отрендерить в jsx
 optionalNode: PropTypes.node,
 // Реакт компонент
 optionalElement: PropTypes.element,
 // Также можно проверить, является ли тип экземпляром конкретного класса
 optionalMessage: PropTypes.instanceOf(Message),
 // Позволяет проверить, что значение является одним из списка
 optionalEnum: PropTypes.oneOf(['News', 'Photos']),
 // Переменная может быть одной из перечисленных типов
 optionalUnion: PropTypes.oneOfType([
   PropTypes.string,
   PropTypes.instanceOf(Message)
 ]),
 // Переменная должна быть списком элементов с конкретным типом
 optionalArrayOf: PropTypes.arrayOf(PropTypes.number),
 // Объект должен содержать значения определенного типа
 optionalObjectOf: PropTypes.objectOf(PropTypes.number),
 // Проверка ключей объекта на соответствие типа
 optionalObjectWithShape: PropTypes.shape({
   color: PropTypes.string,
   fontSize: PropTypes.number
 }),
 // Пометить переменную, как обязательную
 requiredFunc: PropTypes.func.isRequired,
 // Переменная может быть любого типа
 requiredAny: PropTypes.any.isRequired
};
```