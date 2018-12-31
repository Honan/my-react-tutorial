# Состояние компоненты

Объект state описывает внутреннее состояние компонента, он похож на props за тем исключением, что состояние определяется внутри компонента и доступно только из компонента.

Состояние можно обновлять, и каждое обновление состояния вызывает новый render компоненты. Доступ к state происходит с помощью this.state. Состояние компоненты можно получить в любой функции компоненты.

Самое простое объявление

    class App extends Component {

    state = {
        text: "Привет"
    };

    render() {
        return (
            <button>{this.state.text}</button>
        );
        }
    }

[![Edit ll7mvrx3wl](https://codesandbox.io/static/img/play-codesandbox.svg)](https://codesandbox.io/s/ll7mvrx3wl)

Изменить состояние можно с помощью функции **setState**. В эту функцию мы передаем объект с изменениями, которые мы хотим внести в state.

## setState можно использовать двумя способами

* Передать объект — this.setState({a: 1})
* Передать функцию — this.setState(state => ({...state, a: 1}))

Первым способом при клике по кнопке в setState мы передаем объект. Вызывается функция render перерисовывает элемент.

    class App extends Component {
            state = {
            text: "Привет"
        };

        changeState = () =>{
            this.setState({
            text: "Пока"
            });
        }

        render() {
            return <button onClick={this.changeState}>{this.state.text}</button>;
            }
    }

[![Edit ll7mvrx3wl](https://codesandbox.io/static/img/play-codesandbox.svg)](https://codesandbox.io/s/ll7mvrx3wl)

Но если нам нужно поменять состояние, зависящее от предыдущего, то мы должны передавать функцию. Т.к. state может обновляться асинхронно. Мы должны точно знать в каком состоянии находится компонент.

В данном случае меняем текст кнопки в зависимости от ее названия.

    class App extends Component {
        state = {
            text: "Привет"
        };

        changeState = ()=>{
            this.setState( ({text}) => {
            return {
                text: (text === 'Привет' ? 'Пока' : 'Привет')
            }
            });
        }

        render() {
            return <button onClick={this.changeState}>{this.state.text}</button>;
        }
    }

[![Edit ll7mvrx3wl](https://codesandbox.io/static/img/play-codesandbox.svg)](https://codesandbox.io/s/ll7mvrx3wl)