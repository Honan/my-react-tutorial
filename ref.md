# Ref

Ref — это способ получения ссылки на объект в реальном DOM или на экземпляр компонента.

Для начала объявим объект, который будет храниться ref. Вызываем ```React.createRef``` и присваиваем его в переменную ```video```. По сути говоря, мы создали переменную, в которой будет храниться ссылка на видео. Чтобы обратиться к элементу нужно вызвать ```this.video.current```. Таким образом DOM элемент хранится в свойстве ```current``` объекта ```video```.

```javascript
class RefExapmle extends Component {
    video = React.createRef(); // Создали пустой ref объект, в котором будет хранится ссылка на видео

    playVideo = () => {
        this.video.current.play();
    };

    stopVideo = () => {
        this.video.current.pause();
    };

    render() {
        const videoSource =
        "http://distribution.bbb3d.renderfarming.net/video/mp4/bbb_sunflower_1080p_60fps_normal.mp4";
        return (
        <div>
            <button onClick={this.playVideo}>Play</button>
            <button onClick={this.stopVideo}>Stop</button>
            <video ref={this.video} width="320" height="240">
                <source src={videoSource} type="video/mp4" />
            </video>
        </div>
        );
    }
}
```

[![Edit m5yo2pnw4x](https://codesandbox.io/static/img/play-codesandbox.svg)](https://codesandbox.io/s/m5yo2pnw4x)