# move-list

Выполненое тестовое задание. Сделано с помощью vue.cli.

Ссылка на результат: https://stasokulov.github.io/test-task_move-list/

Текст задания:

Написать небольшое приложение по написанию отзывов к фильмам Звездные войны.

1) Выбор фильма

Начальная страница состоит из 2х блоков: первый - список фильмов, второй - содержание фильма.

API для получения данных: https://swapi.dev/api/

Список фильмов: https://swapi.dev/api/films/

Конкретный фильм: https://swapi.dev/api/films/1/

2) Написание отзыва

При переходе на этот шаг, разметка и логика должны подгружаться динамически (лениво).

Форма на странице отзыва должна состоять из 3х полей:

"username" - input, required

"email" - input, required, email mask

"review" - textarea, required

Сэмулировать запрос отправки формы через:
new Promise((resolve) => { setTimeout(resolve, 1000) })

3) Сообщение о успешной отправке
Отобразить введенные пользователем данные в модальном окне.


## Project setup
```
yarn install
```

### Compiles and hot-reloads for development
```
yarn serve
```

### Compiles and minifies for production
```
yarn build
```

### Lints and fixes files
```
yarn lint
```

### Customize configuration
See [Configuration Reference](https://cli.vuejs.org/config/).
