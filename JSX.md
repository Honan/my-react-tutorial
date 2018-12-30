# JSX - синтаксис для разметки в React

JSX — это способ описать html-вёрстку в js. JSX не поддерживается браузерами, но на помощь приходит babel.

Давайте напишем простой компонент.

![Из jsx в js](/screens/Babel&#32;·&#32;The&#32;compiler&#32;for&#32;next&#32;generation&#32;JavaScript&#32;-&#32;Google&#32;Chrome&#32;2018-12-30&#32;20.37.57.png)

[Перейти к коду](https://babeljs.io/repl#?babili=false&browsers=&build=&builtIns=false&spec=false&loose=false&code_lz=GYVwdgxgLglg9mABAQQE6wgGwKYAoCUA3gFCKKrZQipIA8ADgHwAS2mmciA6nKpgCa0A9EwDcxAL5A&debug=false&forceAllTransforms=false&shippedProposals=false&circleciRepo=&evaluate=false&fileSize=false&timeTravel=false&sourceType=module&lineWrap=true&presets=es2015%2Creact%2Cstage-2&prettier=false&targets=&version=6.26.0&envVersion=1.6.2)

Babel транспилировал jsx в js. По сути можно сказать, что jsx - это синтаксический сахар, который помогает описывать верстку в привычном для нас виде.

## Ограничения для jsx

- Jsx не может вернуть два элемента рядом