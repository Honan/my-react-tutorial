# High Order Components (HOC)

Компонент высшего порядка — аналог функций высшего порядка, которые принимают аргументами функции и возвращают их. В случае HOC аргументом служит компонент, и такая функция возвращает обёрнутый компонент.

Компоненты являются единицей функциональности в мире реакта, но существуют ситуации, когда нужно использовать **одинаковую функциональность** для компонент, показывающих разный UI.

У нас есть код, который выводит фото и имя пользователя в виде списка.

```javascript
const Contact = props => {
    const { id, thumbnail, name } = props.contact;
    return (
        <li id={id}>
            <img src={thumbnail} alt={name} />
            {name}
        </li>
    );
};

class ContactsList extends Component {
    render() {
        const { contacts } = this.props;
        return (
        <ul>
            {contacts.map(contact => (
            <Contact key={contact.id} contact={contact} />
            ))}
        </ul>
        );
    }
}

class LoaderHOCExample extends Component {
    state = {
        contacts: []
    };

    componentDidMount() {
        fetch("https://api.randomuser.me/?results=10&nat=us,gb")
        .then(response => response.json())
        .then(parseResponse =>
            parseResponse.results.map(user => ({
            name: `${user.name.first} ${user.name.last}`,
            thumbnail: user.picture.thumbnail,
            id: user.id.value
            }))
        )
        .then(contacts => {
            this.setState({ contacts });
        });
    }

    render() {
        return <ContactsList contacts={this.state.contacts} />;
    }
}
```
[![Edit zqww4kzjp](https://codesandbox.io/static/img/play-codesandbox.svg)](https://codesandbox.io/s/zqww4kzjp)

Мы ожидаем данные, но через какое время они придут мы не знаем. Поэтому лучше показать пользователю сообщение о загрузки данных. Т.к. сообщение о подгрузки данных может понадобиться не только для одного компонента. Воспользуемся HOC, с его помощью мы обернем наш компонент. Во время загрузки данных мы будем показывать соответствующие сообщение, а когда они доступны мы их покажем.

Создаем функцию `loaderHOC()` из нее мы будет возвращать компонент обертку. Который будет рендерерить сообщение о загрузке либо обернутый компонент. И не забываем передать все `props` нашему оборачиваемого компоненту для этого мы используем  `{...this.props}`.

```javascript
const Contact = props => {
    const { id, thumbnail, name } = props.contact;
    return (
        <li id={id}>
        <img src={thumbnail} alt={name} />
        {name}
        </li>
    );
};

class ContactsList extends Component {
    render() {
        const { contacts } = this.props;
        return (
        <ul>
            {contacts.map(contact => (
            <Contact key={contact.id} contact={contact} />
            ))}
        </ul>
        );
    }
}

function loaderHOC(WrappedComponent) {
    return class extends Component {
        render() {
        return this.props.loaded ? (
            <WrappedComponent {...this.props} /> // не забываем передавать props
        ) : (
            <div>Загрузка данных</div>
        );
        }
    };
}

ContactsList = loaderHOC(ContactsList); // оборачиваем

class LoaderHOCExample extends Component {
    state = {
        contacts: []
    };

    componentDidMount() {
        fetch("https://api.randomuser.me/?results=10&nat=us,gb")
        .then(response => response.json())
        .then(parseResponse =>
            parseResponse.results.map(user => ({
            name: `${user.name.first} ${user.name.last}`,
            thumbnail: user.picture.thumbnail,
            id: user.id.value
            }))
        )
        .then(contacts => {
            this.setState({ contacts });
        });
    }

    render() {
        return (
        <ContactsList
            loaded={this.state.contacts.length} // если есть данные, то загружаем их
            contacts={this.state.contacts}
        />
        );
    }
}
```
[![Edit 8nm8k6nzo2](https://codesandbox.io/static/img/play-codesandbox.svg)](https://codesandbox.io/s/8nm8k6nzo2)

Рассмотрим еще один пример. У нас есть компонент `PureHOCExample` он имеет свой `state`, в нем что-то происходит и `state` обновляется (для простоты задан таймер). И дальше мы передаем еmail в виде `props`. В родителе меняется `state`, значит ребенок у ребенка ребенка будет вызываться `render`.

```javascript
class Stateless extends Component {
    render() {
        const { email } = this.props;
        console.log("Component render");
        return <p>I'm simple Stateless component. {email}</p>;
    }
}

class PureHOCExample extends Component {
    state = {
        counter: 0
    };

    componentDidMount() {
        setInterval(() => {
            this.setState(state => ({
                counter: state.counter + 1
            }));
        }, 1000);
    }

    render() {
        return <Stateless email="example@test.ru" />;
    }
}
```
[![Edit j2yy6xmqz9](https://codesandbox.io/static/img/play-codesandbox.svg)](https://codesandbox.io/s/j2yy6xmqz9?expanddevtools=1)

Давайте попробуем это исправить с помощью HOC. У нас есть функция, которая оборачивает компонент в `PureComponent`. Так же мы будем использовать статическое поле `pureHocWrapper` тем самым мы добавим имя нашему HOC это сделана для удобства в отладке.

> PureComponent это специальный тип компонента, в котором реализован shouldComponentUpdate. Который выполняет сравнение старых `props` и новых. Если они не изменились, то и `render` не будет вызываться.

```javascript
class Stateless extends Component {
    render(){
        const {email} = this.props;
        console.log('Component render');
        return <p>I'm simple Stateless component. {email}</p>
    }
}

function pureHOC(WrappedComponent){
    return class extends PureComponent{
        static displayName = "pureHocWrapper"; // для удобства читаемости при отладке

        render(){
            return <WrappedComponent {...this.props} /> // и снова... не забываем передавать props
        }
    }
}

Stateless = pureHOC(Stateless);

class PureHOCExample extends Component{
    state = {
        counter: 0
    }

    componentDidMount(){
        setInterval(()=>{
            this.setState(state => ({
                counter: state.counter + 1
            }));
        }, 1000)
    }

    render(){
        return <Stateless email="example@test.ru" />
    }
}
```
[![Edit zn0l4p7lz4](https://codesandbox.io/static/img/play-codesandbox.svg)](https://codesandbox.io/s/zn0l4p7lz4?expanddevtools=1)