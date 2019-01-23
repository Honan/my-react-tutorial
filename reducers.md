# Reducers

Reducers - это функция, которая принимает state и action. Reducer должен быть чистой функцией. И нельзя мутировать state.

> Чистая функция имеет два свойства: 
> 1. Функция является детерминированной (при одинаковых входных данных **всегда** будет возвращать один и тот же результат. Не зависит от глобальных переменных)
> 2. Не обладают побочными эффектами. Ничего не изменяет во внешней области видимости.

Если у нас state имеет вложенный вид:

```javascript
const initialState = {
    person: {
        name: "",
        surname: ""
    },
    work: ""
}
```

То reducer будет сильно разрастаться

```javascript
const reducer = (state = initialState, action) => {
  switch (action.type) {
    case SET_NAME:
      return { ...state, person: {...state.person, name: action.payload} };
    case RESET_NAME:
      return { ...state, name: "" };
    default:
      return state;
  }
};
```

Для работы с вложенными state существует метод combineReducers. CombineReducers позволяет работать с конкретным срезом, таким образом позволяет написать несколько простых reducer вместо одного сложного. У нас есть state:

```javascript
const initialState = {
    person: {
        name: "",
        surname: ""
    },
    course: {
        title: "",
        students: 0
    }
}
```

CombineReducers это функция, которая получает на вход объект, ключами которого является reducers, которые мы хотим скомбинировать.

```javascript
import { combineReducers, createStore } from "redux";

const SET_NAME = "SET_NAME";
const RESET_NAME = "RESET_NAME";

const SET_TITLE = "SET_TITLE";
const RESET_TITLE = "RESET_TITLE";

const initialPerson = {
  name: "",
  surname: ""
};

const personReducer = (state = initialPerson, action) => {
  switch (action.type) {
    case SET_NAME:
      return { ...state, name: action.payload };
    case RESET_NAME:
      return { ...state, name: "" };
    default:
      return state;
  }
};

const initialCourse = {
  title: "",
  students: 0
};

const courseReducer = (state = initialCourse, action) => {
  switch (action.type) {
    case SET_TITLE:
      return { ...state, title: action.payload };
    case RESET_TITLE:
      return { ...state, title: "" };
    default:
      return state;
  }
};

const rootReducer = combineReducers({
  person: personReducer,
  course: courseReducer
});

const store = createStore(rootReducer);

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

[![Edit Reducers](https://codesandbox.io/static/img/play-codesandbox.svg)](https://codesandbox.io/s/8xq93l4l48?expanddevtools=1)