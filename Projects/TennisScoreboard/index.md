# Проект "табло теннисного матча"

Веб-приложение, реализующее табло счёта теннисного матча.

Комментарии по проекту - [https://www.youtube.com/watch?v=zAOiNa24jpg](https://www.youtube.com/watch?v=zAOiNa24jpg) (видео записано для Java версии роадмапа).

## Что нужно знать

- [Python]({{ site.baseurl }}/Technologies/Python/) - коллекции, ООП
- [Паттерн MVC(S)]({{ site.baseurl }}/Technologies/Python/#mvc) 
- [Maven/Gradle]({{ site.baseurl }}/Technologies/BuildSystems/)
- [Backend]({{ site.baseurl }}/Technologies/Backend/)
  - uWSGI, Waitress
  - Веб - GET и POST запросы, формы
  - Jinja2
- [Базы данных]({{ site.baseurl }}/Technologies/Databases/) - MySQL, SQLAlchemy
- [Frontend]({{ site.baseurl }}/Technologies/Frontend/) - HTML/CSS, блочная вёрстка
- [Тесты]({{ site.baseurl }}/Technologies/Tests/) - юнит тестирование
- [Деплой]({{ site.baseurl }}/Technologies/DevOps/#деплой) - облачный хостинг, командная строка Linux, Tomcat

## Мотивация проекта

- Создать клиент-серверное приложение с веб-интерфейсом
- Получить практический опыт работы с ORM Hibernate
- Сверстать простой веб-интерфейс без сторонних библиотек
- Познакомиться с архитектурным паттерном MVC(S)

Комментарии:
- Проект не подразумевает фреймворки, ради практики с паттерном MVC, использование фреймворков начинается с проекта #5
- Не используем Bootstrap, для практики верстки вручную, Bootstrap используется в проекте #5
- Проект не многопользовательский, поэтому не используем сессии

## Функционал приложения

Работа с матчами:

- Создание нового матча
- Просмотр законченных матчей, поиск матчей по именам игроков
- Подсчёт очков в текущем матче

## Подсчёт очков в теннисном матче

В теннисе особая система подсчёта очков - https://www.gotennis.ru/read/world_of_tennis/pravila.html

Для упрощения, допустим что каждый матч играется по следующим правилам:
- Матч играется до двух сетов (best of 3)
- При счёте 6/6 в сете, играется тай-брейк до 7 очков

## Интерфейс приложения

### Главная страница

- Ссылки, ведущие на страницы нового матча и списка завершенных матчей

### Страница нового матча

Адрес - `/new-match`.

Интерфейс:
- HTML форма с полями “Имя игрока 1”, “Имя игрока 2” и кнопкой “начать”
- Нажатие кнопки “начать” приводить к POST запросу по адресу `/new-match`

Обработчик POST запроса:
- Проверяет существование игроков в таблице `Players`. Если игрока с таким именем не существует, создаём
- Создаём экземпляр класса `Match` (содержащий айди игроков и текущий счёт, уникальный айди матча - UUID) и сохраняем в таблицу `Matches`
- Редирект на страницу `/match-score?uuid=$match_id`

### Страница счёта матча - `/match-score`

Адрес - `/match-score?uuid=$match_id`. GET параметр `uuid` содержит UUID матча.

Интерфейс:
- Таблица с именами игроков, текущим счётом
- Формы и кнопки для действий - "игрок 1 выиграл текущее очко", "игрок 2 выиграл текущее очко"
- Нажатие кнопок приводит к POST запросу по адресу `/match-score?uuid=$match_id`, в полях отправленной формы содержится выигравший очко игрок

Обработчик POST запроса:
- Извлекает из коллекции экземпляр класса Match
- В соответствии с тем, какой игрок выиграл очко, обновляет счёт матча
- Если матч не закончился - рендерится таблица счёта матча с кнопками, описанными выше
- Если матч закончился:
    - Удаляем матч из коллекции текущих матчей
    - Записываем законченный матч в SQL базу данных
    - Рендерим финальный счёт

### Страница сыгранных матчей - `/matches`

Адрес - `/matches?page=$page_number&filter_by_player_name=$player_name`. GET параметры:
- `page` - номер страницы. Если параметр не задан, подразумевается первая страница
- `filter_by_player_name` - имя игрока, матчи которого ищем. Если параметр не задан, отображаются все матчи

Постранично отображает список сыгранных матчей. Позволяет искать матчи игрока по его имени. Для постраничного отображения потребуется реализация пагинации.

Интерфейс:
- Форма с фильтром по имени игрока. Поле ввода для имени и кнопка "искать". По нажатию формируется GET запрос вида `/matches?filter_by_player_name=${NAME}`
- Список найденных матчей
- Переключатель страниц, если матчей найдено больше, чем влезает на одну страницу

## База данных

В качестве базы данных предлагаю использовать MySQL.

#### Таблица `Players` - игроки

| Имя колонки | Тип     | Комментарий                   |
|-------------|---------|-------------------------------|
| ID          | Int     | Первичный ключ, автоинкремент |
| Name        | Varchar | Имя игрока                    |

Индексы:
- Индекс колонки `Name`, для эффективности поиска игроков по имени

### Таблица `Matches` - завершенные матчи

Для упрощения, в БД сохраняются только доигранные матчи в момент их завершения.

| Имя колонки | Тип    | Комментарий                                         |
|-------------|--------|-----------------------------------------------------|
| ID          | Int    | Первичный ключ, автоинкремент                       |
| UUID        | String | Уникальный айди матча                               |
| Player1     | Int    | Айди первого игрока, внешний ключ на Players.ID     |
| Player2     | Int    | Айди второго игрока, внешний ключ на Players.ID     |
| Winner      | Int    | Айди победителя, внешний ключ на Players.ID         |
| Score       | String | JSON представление объекта с текущим счётом в матче |

## MVCS

MVCS - архитектурный паттерн, особенно хорошо подходящий под реализацию подобных приложений. Я подразумеваю, что студент самостоятельно изучил что такое MVCS, и ниже приведу только пример того, как он может быть использован в данном проекте.

## Тесты

Покроем юнит тестами подсчёт очков в матче. Примеры кейсов:
- Если игрок 1 выигрывает очко при счёте 40-40, гейм не заканчивается
- Если игрок 1 выигрывает очко при счёте 40-0, то он выигрывает и гейм
- При счёте 6-6 начинается тайбрейк вместо обычного гейма

Предлагаю студентам самостоятельно придумать тест кейсы для покрытия всех вариантов изменения счёта в матче, особенно правила "больше-меньше" и тайбрейк. Для реализации тестов предлагаю использовать библиотеку unittest, pytest, или подобную.

## Деплой

Будем вручную деплоить war артефакт в Tomcat, установленный на удалённом сервере. При использовании базы данных H2, установка внешней SQL БД не требуется.

Шаги:
- В хостинг-провайдере по выбору арендовать облачный сервер на Linux
- Установить Python, MySQL
- Скопировать код приложения
- Установить зависимости
- Запустить приложение

Ожидаемый результат - приложение доступно по адресу `http://$server_ip:$port/$app_root_path`.

## План работы над приложением

- Классы-модели SQLAlchemy для таблиц БД
- Страница создания нового матча
- Классы для подсчета очков в матче, юнит тесты подсчёта очков
- Страница счёта матча
- Сервис для сохранения матча в БД
- Сервис поиска законченных матчей по имени игрока
- Страница отображения законченных матчей, поиска матчей по имени игрока
- Деплой на удалённый сервер

## Ресурсы для работы над ошибками

- Реализации проекта другими студентами и мои ревью этих реализаций - [https://zhukovsd.github.io/python-backend-learning-course/Projects/FinishedProjects](https://zhukovsd.github.io/python-backend-learning-course/Projects/FinishedProjects)
- Готовый проект можете отправить мне на ревью - [https://t.me/zhukovsd](https://t.me/zhukovsd)
