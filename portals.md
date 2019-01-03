# Portals

Существуют ситуации, когда нужно отрендерить компонент не в текущем месте дерева компонента, а отдельно. Хорошим примером служат модальные окна, тултипы, попапы. Как правило, такого рода элементы находятся в самом конце тега body, чтобы быть выше остальных элементов. Для таких случаев служат порталы, новое api реакта, пришедшее с 16ой версии.

В данном примере показан вызов модального окна. По нажатию на кнопку "Открыть модальное окно" у компонента ```PortalsExample``` меняем состояние на противоположное и вызываем дочерний компонент ```Modal```. В котором создаем DOM элемент, присваиваем id и в методе ```render()``` созданный DOM элемент передаем ```ReactDOM.createPortal```. Таким образом у нас появилась еще одна точка монтирования.

```javascript
class Modal extends Component {
    constructor(props) {
        super(props);
        this.id = "modalPortal";
        this.div = document.createElement("div");
        this.div.id = this.id;

        document.body.insertAdjacentElement("beforeend", this.div);
    }

    componentWillUnmount() {
        this.div.parentNode.removeChild(this.div);
    }

    render() {
        const { show, children } = this.props;
        return show
        ? ReactDOM.createPortal(children, document.getElementById(this.id))
        : null;
    }
}

class PortalsExample extends Component {
    state = {
        isShowModal: false
    };

    toggleModal = () => {
        this.setState(state => ({
        isShowModal: !state.isShowModal
        }));
    };

    render() {
        const { isShowModal } = this.state;
        return (
        <div>
            <button onClick={this.toggleModal}>Открыть модальное окно</button>
            {isShowModal && (
            <Modal show={isShowModal}>
                <p>Я модальное окно</p>
                <button onClick={this.toggleModal}>Закрой меня</button>
            </Modal>
            )}
        </div>
        );
    }
}
```
[![Edit 3y632mx8oq](https://codesandbox.io/static/img/play-codesandbox.svg)](https://codesandbox.io/s/3y632mx8oq)