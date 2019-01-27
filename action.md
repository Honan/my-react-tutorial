# Action

Как говорилось ранее, «единственный способ изменить состояние — передать «action»— объект, описывающий, что произошло».

Action - это объект, с двумя полями `type` и `payload` можно использовать и другие поля но чаще использует именно их. В прошлой статье в качестве значение свойства action мы использовали просто строку. Но есть некоторая опасность опечататься. Обычно место где объявляются action в первую очередь объявляются константы. И как правило они экспортируются для удобного доступа.

```javascript
export const SET_NAME = "SET_NAME";
```

В прошлой статье payload у нас был захаркоден.

```javascript
const action = {
  type: SET_NAME,
  payload: "Denis the best"
};
```

Обычно требуется передавать разные значения. На помощь приходят экшен криейторы (action creators)  - это функция, которая принимает значение, которое мы хотим использовать в нашем action и возвращает объект (новый action).

```javascript
export const setName = name => {
  return {
    type: SET_NAME,
    payload: name
  }
};
```

Соберем все пазлы вместе ;)

```javascript
import { createStore } from "redux";

const SET_NAME = "SET_NAME";
const RESET_NAME = "RESET_NAME";

const reducer = (state = { name: "Denis" }, action) => {
  switch (action.type) {
    case SET_NAME:
      return { ...state, name: action.payload };
    case RESET_NAME:
      return { ...state, name: "" };
    default:
      return state;
  }
};

const store = createStore(reducer);

const setName = name => {
  return {
    type: SET_NAME,
    payload: name
  };
};

const resetName = {
  type: RESET_NAME
};

store.subscribe(() => {
  console.log(store.getState());
});

store.dispatch(setName("Denis"));
store.dispatch(resetName); // затираем имя
store.dispatch(setName("Denis the best")); // выставляем имя обратно
```

[![Edit Action](https://codesandbox.io/static/img/play-codesandbox.svg)](https://codesandbox.io/s/8lr442q358?expanddevtools=1)