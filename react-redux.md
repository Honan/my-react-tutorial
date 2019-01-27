# React-redux

Как мы уже обсудили, Redux — это независимый фреймворк.

Подтянем Redux и react-redux в наш проект:

```bash
npm i redux react-redux
```

Итак, первый компонент из мира react-redux - `<Provider>`.

Компонентам необходим доступ к Redux-хранилищу (store), для того, чтобы они могли подписаться на него. Как вариант — передать его как props в каждый контейнер. Однако это становится утомительным, так как вы должны подключать store, даже если представления просто рендерят контейнер глубоко в дереве компонентов.

Лучше использовать специальный React Redux компонент - `<Provider>`, вызов которого делает хранилище доступным всем контейнерам в приложении без его явной передачи. Нужно только воспользоваться им единожды, когда вы рендерите корневой компонент.

Обратимся к примеру.

У нас будет приложение, которое позволит добавлять в store пользователей с id и именем.

Посмотрим на структуру файлов. У нас есть App.js в котором три кнопки одна будет добавлять, другая удалять всех пользователей и последняя увеличивать счетчик на единицу.

Выделим отдельную папку для создания reducer, в индекс которой мы импортируем `combineReducer`, на случай того если состояние будет усложнятся и будет вложенным.

```javascript
export default combineReducers({
  users
});
```

И для каждого action выделем соответствующий файл, в данном случае user.js. В данном файле у нас описывается работа с юзерами и всего два action: ADD_USER и REMOVE_ALL_USERS.

```javascript
export default (state = [], action) => {
  switch (action.type) {
    case ADD_USER:
      return [...state, action.payload];

    case REMOVE_ALL_USERS:
      return [];

    default:
      return state;
  }
};
```

Так же создам папку для action creator, положим в нее файл users.js который будет возвращать action.

```javascript
export const addUser = (id, name) => ({
  type: ADD_USER,
  payload: {
    id,
    name
  }
});

export const removeAllUsers = () => ({
  type: REMOVE_ALL_USERS
});
```

Первым этапом для подключения redux к нашему приложению идет импорт провайдера. Провайдер позволит взять наш store и сделать его доступным для всех реакт компонентов.

```javascript
import { Provider } from "react-redux"
```

> Нужно помнить, что методы для работы с redux (Provider, Connect и др.) Они доступны не из redux, а из **react-redux**.

Создание store тоже вынесено в отдельный файл, в котором функция возвращает созданный store.

Оборачиваем наш компонент App в провайдер.

```javascript
ReactDOM.render(<Provider store={store}><App /></Provider>, document.getElementById("root"));
```

Теперь мы должны получить store в компоненте App. А для того, чтобы нам были доступные методы для работы со store, мы должны импортировать метод connect.

```javascript
import { connect } from 'react-redux';
```

Теперь нам нужно обернуть наш App в метод connect. При первом вызове этого метода он вернет функцию, в которую нам нужно передать две другие функции, а затем в него можно будет обернуть App.

Как говорилось ранее, connect ожидает получить два метода `mapStateToProps` и `mapDispatchToProps`. B оба метода нам нужно будет объявить.

```javascript
export default connect(mapStateToProps, mapDispatchToProps)(App);
```

Функция `mapStateToProps` нужна для того, чтобы мы могли получать данные из store. В качестве параметров эта функция получает два параметра:

1. **state**. Каждый раз когда state в нашем store будет обновляться в тех компонентах, которые мы подключены при помощи connect будет вызываться метод `mapStateToProps` и будет приходить обновленный state.
2. **ownProps**. Это те props который имеет наш компонент.

`mapStateToProps` возвращает объект с props, которые мы должны получить из state.

```javascript
const mapStateToProps = (state, ownProps) => ({
    users: state.users
})
```

Метод `mapDispatchToProps` позволяет получить `dispatch()` и возвращает колбек props, который вы можете вставить в компонент:

```javascript
const mapDispatchToProps = dispatch => ({
    addUser: (id, name) => dispatch(addUser(id, name)),
    removeAllUsers: () => dispatch(removeAllUsers());
});
```

Теперь в props нашего компонента из `mapStateToProps` прилетит props `users`, а из `mapDispatchToProps` прилетят функции `addUser` и `removeAllUsers`.

```javascript
const {users, addUser, removeAllUsers} = this.props;
```

Так же функцию `mapDispatchToProps` переписать в более коротком виде.

```javascript
const mapDispatchToProps = {
  addUser,
  removeAllUsers
};
```

Полный код можете посмотреть нажав на кнопку.

[![Edit react-redux](https://codesandbox.io/static/img/play-codesandbox.svg)](https://codesandbox.io/s/w6545kwm4k)