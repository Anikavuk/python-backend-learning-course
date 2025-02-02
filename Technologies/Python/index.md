# Python

Python - один из самых популярных языков. Относительно прост, хороша в качестве первого языка и для старта карьеры. Применим в бэкенд и веб разработке, Data Science, Machine Learning, наука.

## Базовый синтаксис

Под базовым синтаксисом понимаются основы - переменные и типы данных, ветвления, циклы for/while.

По этому материалу существует огромное количество ресурсов, книг, курсов. Я советую совмещать теорию с практикой. В качестве теории - книгу или курс, в качестве практики по закреплению синтаксиса - задачи.

#### Избранные курсы и учебные ресурсы

TODO

Предлагаемый порядок изучения материалов:
- Выбрать 1 курс или книгу, точечно изучить те главы, которые я озвучил
- Параллельно изучению теории решать задачи
- Написать проект "Виселица"

## Объектно-ориентированное программирование

ООП является одной из основных парадигм современного программирования. Потенциальная "глубина" владения ООП велика, и этот навык растет годами.

Что нужно знать и уметь:
- Синтаксис описания классов, работы с объектами
- Основные идеи ООП - инкапсуляция, наследование, полиморфизм
- Писать свои классы и иерархии
- Понимать чем плохой ООП код отличается от хорошего

#### Избранные курсы и учебные ресурсы

- TODO
- Практика - [Проект "Симуляция"](../../Projects/Simulation/index.md)

Предлагаемый порядок изучения материалов:
- Выбрать 1 курс или книгу, точечно изучить те главы, которые я озвучил
- Написать проект "Симуляция"

## Паттерны проектирования

Паттерны проектирования,  общепринятые "рецепты" классов для решения повторяющихся задач. Большинство задач являются типовыми, или их вариациями, и паттерны проектирования предлагают набор решения для этих задач.

Каждый паттерн - класс или семейство классов с определённым стилем именования и набором методов.

Что нужно знать:
- Постепенно осваивать паттерны, начиная с самых популярных - Singleton, Factory

#### Избранные курсы и учебные ресурсы

- TODO
- Практика - проекты курса, начиная со 2, содержат проблемы, которые можно решить с помощью паттернов

## MVC

MVC - самый актуальный для бэкенд приложений паттерн.

Суть MVC - разбиение кода на "слои", каждый из которых занимается одной задачей:
- Модели - описывают сущности приложения
- View - отображение состояния, в виде веб-страниц, JSON ответов
- Контроллер - принятие запросов от пользователя, создание модели и view для генерации ответа

Зачастую, MVC расширяется ещё одним слоем - Service, такой паттерн называется MVCS. Классы-сервисы описывают [бизнес-логику](https://ru.wikipedia.org/wiki/%D0%91%D0%B8%D0%B7%D0%BD%D0%B5%D1%81-%D0%BB%D0%BE%D0%B3%D0%B8%D0%BA%D0%B0) приложения.

#### Избранные курсы и учебные ресурсы

- TODO
- Практика - проекты курса, начиная с 4

## Чистый код

Чистый код - это код, понятный и податливый к изменениям. Писать такой код - навык, развивающийся на протяжении всей карьеры.

Практические проекты в рамках этого курса подразумевают не только использование фреймворков и библиотек, но и реализацию логики своими силами. Написание кода с нуля и работа над ним является главным элементом в повышении чистоты своего кода.

#### Избранные курсы и учебные ресурсы

- Книги - "Чистый код" Мартина, "Рефакторинг" Фаулера, "Совершенный код" Макконнелла
- Плейлист с моими live coding стримами, где я пишу код, озвучивая свои мысли и комментируя принятые решения - [https://www.youtube.com/playlist?list=PLOVOZrcS3XMZ-QJDHowJQ3abxNHoW8pV3](https://www.youtube.com/playlist?list=PLOVOZrcS3XMZ-QJDHowJQ3abxNHoW8pV3)
- Плейлист с избранными ревью реализаций проектов курса студентами - [https://www.youtube.com/playlist?list=PLOVOZrcS3XMbS4iInU-7p6TbIQW-kATfz](https://www.youtube.com/playlist?list=PLOVOZrcS3XMbS4iInU-7p6TbIQW-kATfz)
- Практика:
  - Писать проекты курса, отправлять их мне на ревью. Примеры [готовых проектов с ревью](../../Projects/FinishedProjects/index.md)
  - Читать чужой код и оценивать его качество
