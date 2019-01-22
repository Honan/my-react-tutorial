# Store

Первым делом импортируем `createStore`.

Инициализируем первый storе. Создаем константу и присваиваем ей вызов функции `createStore`. В `createStore` в качестве аргумента мы передаем функцию reducer. Reducer это чистая функция которая принимает state и action. В данном примере state мы назначаем дефолтный. И давайте выведем наш текущий store с помощью метода `getState`.

> Таким образом state хранится в store

```javascript
import { createStore } from "redux";

const reducer = (state = { name: "Denis" }, action) => state;

const store = createStore(reducer);

console.log(store.getState());
```

[![Edit Store_1](https://codesandbox.io/static/img/play-codesandbox.svg)](https://codesandbox.io/s/yw6wn69qoj?expanddevtools=1)

Чтобы поменять store мы должны использовать action. Action представляет простой объект. Со свойствами type и payload. Мы можем использовать и другие названия, но так принято называть данный свойства.

```javascript
const action = {
  type: "SET_NAME",
  payload: "Denis"
};
```

С помощью метода `dispatch` мы попросим store изменить на action.

```javascript
store.dispatch(action);
```

Так как мы получили новый action, reducer может вернуть новое значение state. Обычно внутри reducer все action обрабатывается с помощью конструкции `switch case`.

```javascript
const reducer = (state = { name: "Denis" }, action) => {
  switch(action.type){
    case "SET_NAME":
      return {...state, name: action.payload}
    default:
      return state
  }
};
```

У store есть метод `subscribe`, который позволяет подписаться на его обновления. В `subscribe` мы передаем функцию, которая будет вызываться каждый раз, когда store будет получать action.

```javascript
store.subscribe(() => {
  console.log(store.getState());
});
```

Давайте соберем все по кусочкам и посмотрим на полноценный код. Сначала у нас в state хранилось имя Denis, а затем мы с помощью action изменили на пустую строчку.

```javascript
import { createStore } from "redux";

const reducer = (state = { name: "Denis" }, action) => {
  switch (action.type) {
    case "SET_NAME":
      return { ...state, name: action.payload };
    default:
      return state;
  }
};

const store = createStore(reducer);

console.log(store.getState());

const action = {
  type: "SET_NAME",
  payload: ""
};

store.dispatch(action);

console.log(store.getState());
```

[![Edit Store_2](https://codesandbox.io/static/img/play-codesandbox.svg)](https://codesandbox.io/s/zlw4r7plkm?expanddevtools=1)