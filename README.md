# Задание 1 — найди ошибки

В этом репозитории находятся материалы тестового задания "Найди ошибки" для [14-й Школы разработки интерфейсов](https://academy.yandex.ru/events/frontend/shri_msk-2018-2) (осень 2018, Москва, Санкт-Петербург, Симферополь).

Для работы тестового приложения нужен Node.JS v9. В проекте используются [Yandex Maps API](https://tech.yandex.ru/maps/doc/jsapi/2.1/quick-start/index-docpage/) и [ChartJS](http://www.chartjs.org).

## Задание

Код содержит ошибки разной степени критичности. Некоторые из них — стилистические, а другие — даже не позволят вам запустить приложение. Вам нужно найти все ошибки и исправить их.

Пункты для самопроверки:

1. Приложение должно успешно запускаться.
1. По адресу http://localhost:9000 должна открываться карта с метками.
1. Должна правильно работать вся функциональность, перечисленная в условиях задания.
1. Не должно быть лишнего кода.
1. Все должно быть в едином codestyle.

## Запуск

```
npm i
npm start
```

При каждом запуске тестовые данные генерируются заново случайным образом.

# Решение

## Ошибка 1

### Описание

На первую ошибку указал сборщик Webpack после запуска команды ```npm start```:

```
WARNING in ./src/index.js 4:2-9
"export 'default' (imported as 'initMap') was not found in './map'
 @ multi (webpack)-dev-server/client?http://localhost:9000 ./src/index.js
```

Из ворнинга следует, что в файле index.js присутствует импорт дефолтного экспорта, которого в импортируемом файле map.js нет.

### Решение

Исправить ошибку можно двумя способами:

1. Исправить экспорт в файле map.js на дефолтный
1. Оставить именованный экспорт в файле map.js и исправить импорт в файле index.js

Так как map.js - это внутренний модуль приложения, его следует привести в соответствие с другими внутренними модулями. В этом приложении используется именованный экспорт, значит следует выбрать второй вариант решения.

## Ошибка 2

### Описание.
После исправления первой ошибки Webpack отработал без ошибок, но приложение по-прежнему не появилось в окне браузера. Консоль в Chrome также не показала ошибок в коде, а сообщение ```inited``` из index.js благополучно отобразилось, из чего сдедует, что инициализация карты успешно завершилась. Это указало на то, что следующая ошибка таится где-то в логике, связанной с отображением. Открыв dev tools и взглянув на html я обнаружил, что в контейнере #map действительно появилась разметка карты, а не отображалась она из-за css-правила ```height: 0```, примененного inline к контейнеру карты. При изменении значения этого правила на положительное карта проявилась. Это означало, что где-то в ходе инициализации карты ей задавалась неправильная высота. В документации к API яндекс карт указано, что для инициализации карты нужен [контейнер ненулевого размера](https://tech.yandex.ru/maps/doc/jsapi/2.1/quick-start/index-docpage/#create_map_container), в нашем случае контейнер #map имеет нулевую высоту, это значение и используется в ходе инициализации.

### Решение.


