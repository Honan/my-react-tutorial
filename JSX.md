# JSX - синтаксис для разметки в React

JSX — это способ описать html-вёрстку в js. JSX не поддерживается браузерами, но на помощь приходит babel.

Давайте напишем простой компонент.

![Из jsx в js](/screens/Babel&#32;·&#32;The&#32;compiler&#32;for&#32;next&#32;generation&#32;JavaScript&#32;-&#32;Google&#32;Chrome&#32;2018-12-30&#32;20.37.57.png)

[Перейти к коду](https://babeljs.io/repl#?babili=false&browsers=&build=&builtIns=false&spec=false&loose=false&code_lz=GYVwdgxgLglg9mABAQQE6wgGwKYAoCUA3gFCKKrZQipIA8ADgHwAS2mmciA6nKpgCa0A9EwDcxAL5A&debug=false&forceAllTransforms=false&shippedProposals=false&circleciRepo=&evaluate=false&fileSize=false&timeTravel=false&sourceType=module&lineWrap=true&presets=es2015%2Creact%2Cstage-2&prettier=false&targets=&version=6.26.0&envVersion=1.6.2)

Babel транспилировал jsx в js. По сути можно сказать, что jsx - это синтаксический сахар, который помогает описывать верстку в привычном для нас виде.

Для того чтобы вычислить что-нибудь прямо в jsx или использовать значение из переменной, используются фигурные скобки.

    function Article() {
        return <p>{1 + 1}</p>;
    }

## Ограничения в jsx

- Для присвоения класса используется **className**, т.к. class — зарезервированное слово в js для создания классов.
- Вместо атрибута for (для тега label) используется **htmlFor**.
- Тэги элементов пишем с **маленькой** буквы, а название компонентов с **заглавной**.
- Теги должны быть **закрытыми**.
- Jsx не может вернуть два элемента рядом*
  
\* Но на самом деле можно с помощью массива. При этом каждому элементу нужно проставить уникальный ключ.

    function Article() {
        return [
            <p key="a">Hello World</p>, 
            <p key="b">Another line</p>
        ];
    }
[Перейти к коду](https://babeljs.io/repl#?babili=false&browsers=&build=&builtIns=false&spec=false&loose=false&code_lz=GYVwdgxgLglg9mABAQQE6wgGwKYAoCUiA3gFCLmKrZQipIDaZFzAPAA6IDW2AngLwAiAIYCAfAAlsmTHEQB1OKkwATFgHo2ogDSImzcuy69BAIzHIwcKAAtsqRJhhhs6zXsQBdANwkAvkA&debug=false&forceAllTransforms=false&shippedProposals=false&circleciRepo=&evaluate=false&fileSize=false&timeTravel=false&sourceType=module&lineWrap=true&presets=es2015%2Creact%2Cstage-2&prettier=false&targets=&version=6.26.0&envVersion=1.6.2)