### TEST
* https://localhost:4443/
* https://localhost:4443/chat: HTTP-запрос для загрузки страницы (HTML, CSS, JavaScript)
* https://localhost:4443/staticfiles/admin/css/base.css
* https://localhost:4443/static/css/popUpChat.css
* http://127.0.0.1:8000/admin/
* http://127.0.0.1:8000/user/1/
* http://localhost:8000/chat/d/
* 127.0.0.1:8000/chat/room1/ в разных местах, оба видят все сообщения
* http://localhost:8000/staticfiles/admin/css/base.css
* ws://localhost:4443/ws/chat/<roomName>/ ws-запрос на /ws/chat/
* http://95.217.129.132:8000/
* https://tr.naurzalinov.me/users/
* `curl -I http://localhost:4444` должен вернуть статус 301 с заголовком Location: https://localhost:4443/...
* `curl -I --insecure https://localhost:4443` должен вернуть `HTTP/1.1 200 OK`
* Endpoints: просматривать `views.py` в каждом приложении: какие представления и какие URL ассоциированы с функциями или классами в разных частях проекта
* Postman
  + импортируйте коллекцию эндпоинтов, если она уже создана  
  + для изучения API, отправляя запросы на `/api/`, `/swagger/`, ... и исследуя доступные маршруты
  + endpoints HTTP (API или страницы): введите `http://localhost:8000/api/endpoint/`
  + метод (GET, POST, PUT, DELETE и т. д.)
  + если требуется авторизация, добавьте токен или данные пользователя (если используете `Token` или `JWT`)
  + отправьте запрос и проверьте статус ответа (200 OK, 401 Unauthorized и т.д.) и тело ответа
* Django предоставляет встроенные инструменты для тестирования HTTP
* WebSocket-тесты с Django Channels + `pytest`
* F12
* если подключены библиотеки для документирования API, то `http://localhost:8000/swagger/` или `http://localhost:8000/redoc/`
* `docker-compose logs`
* `docker logs backend`
* логи внутри контейнера


### LIVE CHAT
* **история в бд** (или в redis?)
* **чат сделать компонентом**
* the user sends direct messages to other users (subject)
* the user blocks other users, they see no more messages from the account they blocked (subject)
* the user invites other users to play through the chat interface (subject)
* the tournament warns users expected for the next game (subject)
* the user access other players profiles through the chat interface (subject)
* rest api строит и отдаёт html
* js получает ответ (get)
* с каждым пользователем у бэкенда 2 вебсовета: чат, положение ракетки
* websocket объект js
* websocket: кому пишем сообщение? в запросе
* prepMsg забирает инпут и делает ws запрос
  + взять список пользователей из базы
* @login_required только если залогинен, если нет токена или он неправильный - не пропустит
* fetch() запрос обычный (не websocket)
* **AbsTimeUser** (непралвьно написано) наш класс наследует
* johnResponse - временный
* view.py не html
  + jsonResponse или httpResponse
* createuser встроенная, т.к. наследуем от ... 
* **[2302]** в контейнере бэк: python manage.py make migrations
* если логин - присылает **session id token**
* первый логин - header CSRF токен
  + посмотреть это: F12 Applocation storage cookies
  + в js фенкция post - в header токен CSRF
  + session id приязывает сессия + юзер в приложении
* сначала выполняется header, потом подгружаются стили
* в django новая функция: python manage.py statup, migrations, INSTALLED_APS
* 1 эндпоинт слушает чат
  + в заголовке запроса - кому сообщение
* если сообщение не дошло, то пользователя нет
* в контейнере бэк: python manage.py superuser то ли python manage.py createsuperuser, с именем админ пародлем админ
  + после этого migrations
* pathname = часть адреса после корня
* login = new ws connexion
* ws system msgs
* ws user communications
* базовый функционал чата
  + Обновите шаблоны HTML
  + База данных: модель для хранения сообщений
  + сохранение сообщений в `ChatConsumer` при получении данных
  + сообщения передаются через каналы
  + Проверьте сохранение сообщений в базе данных


### GAME LOGIC
* a player should also be possible to propose a tournament (subject)
  + a tournament displaies who is playing against whom and the order of the players (subject)
• a matchmaking system: the tournament system organize the matchmaking of the participants, and announce the next fight
• a registration system
  + at the start of a tournament, each player must input their alias name
  + the aliases will be reset when a new tournament begins
  + this requirement can be modified using the Standard User Management module.
* user management, authentication, users across tournaments (subject)
  + Users can subscribe to the website in a secure way
  + Registered users can log in in a secure way
  + Users can select a unique display name to play the tournaments
  + Users can update their information
  + Users can upload an avatar, with a default option if none is provided
  + Users can add others as friends and view their online status
  + User profiles display stats, such as wins and losses
  + Each user has a Match History including 1v1 games, dates, and relevant details, accessible to logged-in users
  + the management of duplicate usernames/emails is at your discretion, you must provide a solution that makes sense
* Remote players (subject)
  + two distant players, each player is located on a separated computer, accessing the same website and playing the same Pong game
  + think about **network issues**, like unexpected disconnection or lag
  + you have to offer the best user experience possible
* Game Customization Options (subject) возможно будем
  + customization options for all available games on the platform
  + customization features, such as power-ups, attacks, or different maps, that enhance the gameplay experience
  + allow users to choose a default version of the game with basic features if they prefer a simpler experience
  + ensure that customization options are available and applicable to all games offered on the platform
  + implement user-friendly settings menus or interfaces for adjusting game parameters
  + maintain consistency in customization features across all games to provide a unified user experience
  + to give users the flexibility to tailor their gaming experience across all available games by providing a variety of customization options while also offering a default version for those who prefer a straightforward gameplay experience
* AI Opponent (subject)
  + to introduce data-driven elements to the project, with the major module introducing an AI opponent for enhanced gameplay, and the minor module focusing on user and game statistics dashboards, offering users a minimalistic yet insightful glimpse into their gaming experiences
  + an AI player into the game
  + the use of the A* algorithm is not permitted 
  + an AI opponent that provides a challenging and engaging gameplay experience for users
  + the AI replicates human behavior, meaning that in your AI implementation, you must simulate keyboard input
  + the AI refreshes its view of the game once per second, requiring it to anticipate bounces and other actions
  + the AI utilizes **power-ups** if you have chosen to implement the Game customization options module
  + AI logic and decision-making processes that enable the AI player to make intelligent and strategic moves
  + explore alternative algorithms and techniques to create an effective AI player without relying on A*
  + the AI adapts to different gameplay scenarios and user interactions
  + to explain in detail how your AI is working 
  + it must have the capability to win occasionally
  + an AI opponent that adds excitement and competitiveness without relying on the A* algorithm
* user and Game Stats Dashboards (subject) может быть будем делать
  + dashboards that display statistics for individual users and game sessions
  + user-friendly dashboards that provide users with insights into their own gaming statistics
  + a separate dashboard for game sessions, showing detailed statistics, outcomes, historical data for each match
  + the dashboards offer an intuitive and informative user interface for tracking and analyzing data
  + data visualization techniques, such as charts and graphs, to present statistics in a clear and visually appealing manner
  + users access and explore their own gaming history and performance metrics conveniently
  + add any metrics you deem useful
  + users monitor their gaming statistics and game session details through user-friendly dashboards
  + a comprehensive view of their gaming experience
* game logic, because we need it to do the multiplayer
* трекинг **когда пользователь был онлайн**
  + models.py: last online для индикатива
  + три решения 
    - через библиотеку channels - по вебсокетам следим пользователь онлайн или нет - НАМ ЭТОТ СПОСОБ
    - через Django sessions - как только юзер делает какое либо действие, Джанго сохраняет в базу данных дату этого действия 
    - через redis - но не понял как это работает
* pass reset будет ли?
* change username, email будет ли?
* myapp: логика пользовательских профилей, турниров, историй игр
* alexey: Tournaments – working 


### FRONTEND NGINX
* try using **bolt.new** it's better at frontend
  + the ui is fire here
* **game customization** it's just gonna be front
  + like custom colors custom map
* подписывается на **WebSocket-каналы**
* проверяет сертификаты SSL
* расшифровывает с использованием SSL-сертификата
  + внутренний трафик не шифруется
* если запрос к статическому файлу, отдает его из /usr/share/nginx/html/static/
* запросы на /api/ (JSON, HTML, WebSocket) идут на Django (аутентификаци, получения/отправки данных)
  + `proxy_set_header ...` передает заголовки (Host, IP-адрес клиента, протокол, ...), чтобы бэкенд понимал, откуда запрос
  + **сохраняет заголовки WebSocket**
* фронт самописный или с использованием минимальных библиотек (jQuery, Bootstrap, ...), не на SPA-фреймворке
* балансировщик нагрузки (если много сообщений, **что будет**?)
* в модели пользователя сохраняю аватарки
  + **у фронтэнда есть требования к аватаркам или они могут сами отрисовывать?** вдруг пользователь сохранит свою фотографию 1000 х 1000 пикселей, на фронте вы сможете отрисовать аватарку ?
* `frontend/static/app.js`
  + `fetch('http://backend:8000/api/user-data/')`: HTTP-запрос к эндпоинту `/api/user-data/`, получить данные JSON 
  + динамически обновляет DOM§ пользовательского интерфейса с использованием данных, полученных от бэка
  + это не делает приложение SPA-фреймворком — это обычная логика на чистом JS
  + `response => response.json()` конвертирует ответ JSON от сервера в объект JavaScript
  + после получения данных пользовательский интерфейс (UI) обновляется без перезагрузки страницы
  + `async/await` для упрощения чтения
*  a single-page application (subject)
  + один html
  + меняется с помощью js
  + код внутри {} исполняется в django, он выполняет и заново отправляет html
  + не надо: в завимисости от какого-то условия, показываем или нет какие-то части страницы
* проверить: в контейнере `nginx -t`
* **SSL Labs** проверить корректность настройки SSL
* четыре server{}-блока = один процесс
* Vanilla JS (без фреймворков)
* **Bootstrap toolkit**
* **Карточка Bootstrap**  
* pop-up windows : login, chat, profile
* страница comptetition, profile, настройки
* compatible with the latest stable up-to-date version of Google Chrome
* Amine: game front using javascript


### BACKEND DAPHNE 
* compatible with the latest stable up-to-date version of Google Chrome (subject)
* The user should be able to use the Back and Forward buttons of the browser (subject)
* класс router обрабатывает перемещения по сайту
  + popstate кнопки назад вперёд в брузере
* на место .app подставляется div
  + class Component базовый, абстрактный
  + фиксированная часть страницы доступна в js
  + div = homapage, profile, ... (наследуют от Component)
  + constructor() создаёт класс
  + state = переменные 
  + % body % кберем, т.к. у нас SPA
  + fetch (в js) = запрос к бэку
  + функция render своя в каждом компоненте
    - например нужен username - делаем запрос к бэку fetchuserprofile
  + событие DomContactLoaded = html полностью загрузился (у нас только 1 раз)
* proxy_http_version 1.1
  + ws‐подключения для **апгрейда соединения** используют HTTP/1.1
  + если оставить по умолчанию HTTP/1.0, Nginx не будет корректно передавать заголовки Upgrade и Connection: upgrade, и вебсокеты могут не работать
  

### BACKEND DAPHNE 
* `python manage.py runserver 0.0.0.0:8000`
  + запускает Django-приложение с использованием встроенного **разработческого сервера**
    - может работать как с WSGI, так и с ASGI, в зависимости от конфигурации проекта
     если настроно Channels и указано `ASGI_APPLICATION`, то запускает ASGI-сервер вместо стандартного WSGI
    - команда `runserver` удобна для разработки
    - не рекомендуется для использования в продакшен-среде
    - специализированный ASGI-сервер Daphne: более высокую производительность и стабильность.
* слушает внутри контейнера на порту 8000 API-запросы, HTTP-запросы фронтенда
* обработка через DRF стандартным Django-приложением, передаёт данные о запросе Python-приложению
* Python-приложение обрабатывает запрос и возвращает ответ
* хорошо интегрируется с Django Channels
* поддерживает WebSocket из коробки
* ASGI сервер 
* интерфейсы asgi, wsgi
  + стандарт для взаимодействия между веб-сервером и веб-приложениями/фреймворками
  + фреймворки, тулкиты, библиотеки питон не умеют взаимодействовать между собой, у каждого свой метод установки, настройки
  + WSGI, Web Server Gateway Interface
    - поддерживает только синхронные запросы HTTP, не подходит для реального времени или протокола ws
    - бывает нужен: некоторые инструменты поддерживают только синхронные HTTP-запросы; Django-приложений без асинхронных функций с WSGI стабильнее
  + ASGI, Asynchronous Server Gateway Interface - продолжение WSGI, когда есть асинхронные задачи
* `ASGI_APPLICATION = "myproject.asgi.application"`
  + Переменная `application` = точка входа для ASGI-сервера, ASGI-приложение, обрабатывает входящие запросы
  + `django_asgi_app = get_asgi_application()` создаёт объект ASGI-приложения, exposes the ASGI callable as a **module-level variable**
    - Initialize Django ASGI application: загрузка приложений из `INSTALLED_APPS`, конфигурация бд, **среда `AppRegistry`**
    - to ensure the AppRegistry is populated before importing ORM models
    - приложение Django инициализирется до использования **ORM**-моделей или других компонентов Django
    - приложения готовы к работе _до_ конфигурации маршрутов WebSocket
    - если снчала импортировать модели или др компоненты Django, будут ошибки, связанные с незарегистрированными приложениями или моделями
* AllowedHostsOriginValidator проверяет допустимые хосты для WebSocket-соединений
* Amine: game backend using websockets (with 42 auth)


### ЯДРО DJANGO 
* бэкенд-фреймворк
* ключевые механики (аутентификация, управление базой данных, админка, API), функции и практики из коробки
* диктует архитектуру (приложения, модели, views, urls, ...)
* DJANGO_SETTINGS_MODULE настройки компонент Django (**ORM**, middleware, ...)
* `settings.py` конфиг на высоком уровне
  + `BASE_DIR` корневая папка проекта  
  + `SECRET_KEY` 
  + `ALLOWED_HOSTS` список доменов/IP, с которых разрешён доступ к Django-приложению (когда `DEBUG=False`)
  + `TEMPLATES` настройки шаблонов Django (**HTML-шаблоны**)
  + `REST_FRAMEWORK` **рендеры**, по умолчанию JSON и **Browsable API**
  + `AUTH_PASSWORD_VALIDATORS` валидаторы сложности паролей (минимальная длина, ...)
  + logging.basicConfig
    - настройки логирования в Python
    - влияет на все логгеры, включая те, которые используются Django и Channels
    - может игнорировать встроенные настройки логирования django
    - может конфликтовать с настройками логгеров Django и Channels
    - мало контроля над логгерами
  + LOGGING можете настроить разные обработчики, уровни логирования и форматы для отдельных логгеров
    - django для стандартных событий Django (запросы, ответы, ошибки)
    - channels для событий, связанных с WebSocket и канальным слоем
    - django.db
* myproject/ конфигурация Django, корневой маршрутизатор, запуск, управление проектом
* `models.py` структура данных, связи (пользователи, профили, чаты, сообщения, статистика игры, ...)
* **`migrations/`** для моделей
* встроенная система аутентификации и модель пользователей
* регистрация и авторизация (в том числе OAuth/SSO)
* разграничение доступа к страницам (приватные/публичные чаты, комнаты, ...)
* проверка **токена/сессии** при каждом запросе с фронтенда
* инструменты по защите от распространённых уязвимостей (**CSRF, XSS, SQL Injection**)
* благодаря джанговским формам и сериализаторам + Django REST Framework, упрощается валидация данных, приходящих от фронтенда
* DPR, Data Processing and Rendering: данные (JSON, ...) обрабатываются на серверной стороне с помощью Django и передаются в представление для рендеринга на клиентской стороне
* manage.py django-утилита
  + скрипт запуска Django-проекта
  + интерфейс для настройки, разработки, управлением проектом
  + `python manage.py runserver` запуск сервера разработки 
  + python manage.py migrate` применение **миграций** базы данных
  + `python manage.py createsuperuser` создание суперпользователя
  + `if __name__ == '__main__':` точка входа программы, если файл запущен напрямую (не импортирован как модуль), выполняется `main()`
  + можете создавать собственные команды (в папке `management/commands`) и запускать их через `manage.py`.
  + если миграции применяются автоматически при развёртывании, это делается через Docker, где команды `python manage.py migrate` уже интегрированы в скрипты, вызов `manage.py` встроен в процессы
  + daphne обращается к проекту напрямую через или `mysite.asgi`
  + команды django:
    - [auth]: changepassword createsuperuser
    - [contenttypes] remove_stale_contenttypes
    - [daphne] runserver
    - [django] check compilemessages createcachetable dbshell diffsettings dumpdata flush inspectdb loaddata makemessages makemigrations   migrate optimizemigration sendtestemail shell showmigrations sqlflush sqlmigrate sqlsequencereset squashmigrations startapp startproject test testserver
    - [rest_framework] **generateschema**
    - [sessions] clearsessions
    - [staticfiles] collectstatic findstatic
* /admin встроеный интерфейс для управления данными (пользователи, чаты, статистика, ...)
* суперпользователь Django Admin
  + все права в системе
  + доступ к системе управления данными, редактирование, добавление, удаление записей во всех моделях
  + можно в продакшене для настройки данных или исправления ошибок 
  + объект модели `User` в Django
  + если админка не используется, не нужен
  + != суперпользователь базы данных
* **Django Debug Toolbar** отслеживание работы проекта, включая middleware
* django app - модульные приложения внутри проекта
* втроенные django app
  + 'django.contrib.messages'
  + 'django.contrib.admin',
  + 'django.contrib.auth',
  + 'django.contrib.contenttypes',
  + 'django.contrib.sessions',
  + 'django.contrib.staticfiles'
  * **какие ещё**
* 'django.contrib.messages'
  + отвечает за систему flash‐messages
  + приложение? фреймворк?
  + предоставляет API для работы с сообщениями
  + flash-messages
    - временные уведомления
    - сообщения сохраняются между запросами
    - автоматически удаляются после первого отображения
    - `MessageMiddleware` сохраняет и извлекает сообщения из хранилища (cookie, сессии, ...)
    - без `MessageMiddleware` только для сообщений **внутри одного запроса**, сообщения не сохраняются между запросами
    - `MessageMiddleware` без `django.contrib.messages` невозможно использовать
    - сообщения пользователю **между запросами** (об регистрации, ошибка при неверном вводе данных, ...)
    - отображаются пользователю после выполнения действия
    - хранит сообщения временно (в сессии или cookies)
    - отображаются в верхней части страницы и исчезают через несколько секунд или при обновлении
    - исчезают после того, как пользователь обновит страницу или перейдет на другую
    - не используются для функций реального времени, как в чате
    - не асинхронны
    - одноразовые сообщения через **редиректы** (например, после POST-запросов)
    - работают благодаря связке `django.contrib.messages` + `MessageMiddleware`
    - результ действия: "Ваше сообщение отправлено", "Неверный пароль"
    - операции в чате: уведомления о добавлении нового участника в группу
    - управление игровыми процессами: Ваш запрос на матч отправлен
    - администрирование: Настройки сохранены, Ошибка при обновлении параметров"
    - передача статуса операций между страницами: После успешного сохранения формы пользователю показывается сообщение на новой странице
    - отображение успешных действий, предупреждений, ошибок (пользователь отправил форму, обновил профиль, завершил какое-то действие)
  + обмен данными
    - через ws соединения или HTTP-запросы, добавляя сообщения во время перенаправлений или в шаблоны
* middlware
  + набор классов
  + не являются серверами
  + фильтры, обработчики, модифицируюют запросы/ответы
  + **для обеспечения доступа к объекту пользователя (request.user) и другим данным авторизации**
  + SecurityMiddleware
    - добавляет заголовки безопасности (`Strict-Transport-Security` для HTTPS)
    - перенаправляет HTTP-запросы на HTTPS (если настроено) **куда именно? какой-то порт?**
    - убирает опасные элементы из запросов (например, от уязвимости **XSS**)
  + SessionMiddleware
    - загружает данные сессии из хранилища
    - если сессии хранятся в Redis или базе данных, сохраняет их после выполнения запроса
    - если пользователь **аутентифицирован**, сессия **сохраняет его идентификатор**
    - управляет сессиями через куки
  + CommonMiddleware
    - базовая обработка (перенаправление с /, ...)
    - перенаправление при добавлении/удалении `/` в конце URL
    - возвращает стандартные заголовки HTTP (`Content-Type`, ...)
  + CsrfViewMiddleware
    - проверяет наличие и правильность CSRF-токена
    - если токен неверный, возвращает ошибку `403 Forbidden`
    - защита от атак типа **Cross-Site Request Forgery**
  + AuthenticationMiddleware
    - проверяет **сессию или заголовки авторизации**
    - добавляет **объект `request.user`**, чтобы представления могли определить, аутентифицирован ли пользователь
  + MessageMiddleware
    - обрабатывает flash messages
    - сообщение помещается в **сессии**
    - сообщения остаются там только до следующего запроса, затем удаляются автоматически
    - не имеет отношения к чату (чат требуют постоянного обновления страницы или использования ws для обмена сообщениями)
    - может быть настроен для **обработки сообщений и добавления дополнительной информации или фильтрации на уровне WebSocket**
  + XFrameOptionsMiddleware
    - добавляет заголовок `X-Frame-Options` для защиты от атак **clickjacking** (запрещает отображение сайта в **iframe** на других доменах, ...)
* middleware на обратном пути
  + XFrameOptionsMiddleware обрабатывает ответ (добавляет заголовок X-Frame-Options)
    - XFrameOptionsMiddleware добавление заголовков безопасности
  + MessageMiddleware (сохраняет flash-сообщения)
  + AuthenticationMiddleware (завершает обработку данных авторизации)
    - добавление объекта `request.user` для авторизованных пользователей 
  + CsrfViewMiddleware (может добавлять CSRF-токен в шаблон)
    - проверка CSRF-токена **ещё раз?**
  + CommonMiddleware (может модифицировать заголовки)
  + SessionMiddleware сохраняет сессии, если они изменились
  + SecurityMiddleware
    - проверка заголовков
    - добавление заголовков безопасности
* сериализация
  + базовый serializers.ModelForm
* базовые механизмы аутентификации django.contrib.auth
* простая модель групп и разрешений django.contrib.auth.models.Permission
* Используем **стандартные структуры юзера для авторизации и для моделей данных**
* технические детали
  + на базе Python
    

### DJANGO REST FRAMEWORK DRF
* расширяет **стандартные возможности Django**
* строит над Django более мощный стек для работы с RESTful API
* каждый запрос обрабатывается отдельно, клиент получает ответ сразу
* регистрирует базовые модели, виджеты, фильтры, классы для работы с API
* **DefaultRouter** и маршрутизация
* PUT = поменять поле в бд (без DRF запрос sql)
* **среда `AppRegistry`** управляет регистрацией приложений и моделей
* модуль сериализации данных для работы с JSON или API #see
  + преобразование моделей Django и других объектов Python в форматы JSON/XML и обратно
  + создавать сложные структуры
  + валидация данных
* **модуль аутентификации** #see
  + расширяет аутентификацию
  + поддерживает Token Authentication
  + поддерживает Session Authentication
  + поддерживает JWT (с внешними библиотеками, например, Simple JWT), добавляет гибкие механизмы для работы с JWT
  + поддерживает Basic Authentication
  + поддерживает авторизацию по API-ключам
  + django без DFR предоставляет базовые механизмы аутентификации (модель пользователей, проверка паролей, сессии)
* модуль прав доступа Permissions #see
  + позволяет контролировать **доступ к ресурсам** API на основе стандартных классов прав доступа: `AllowAny`, `IsAuthenticated`, `IsAdminUser`, `IsAuthenticatedOrReadOnly` — аутентифицированные могут изменять, гости читать, для групп пользователей или конкретных ролей, спец условия (владелец ресурса, ...) и кастомных permissions
* Throttle Classes, APIView, ViewSets, Routers, Filters
  + вспомогательные компоненты DRF
  + работают в тандеме с модулями сериализации, аутентификации, прав доступа
* Throttles
  + контролируют частоту запросов пользователей
  + защита от DoS-атак и чрезмерной нагрузки на сервер (**nginx недостаточно?**)
* APIView
  + базовый класс представлений (views)
  + создавать собственные представления для обработки запросов
  + методы (`get`, `post`, `put`, `delete`, и т.д.), которые можно переопределять для обработки HTTP-запросов
  + может работать в связке с сериализаторами для обработки данных запросов и формирования ответов
  + встроенная поддержка систем аутентификации и контроля прав доступа (например, с использованием `IsAuthenticated`)
  + применять ограничения на количество запросов
  + больше контроля, чем ViewSet, определяете логику запросов и маршрутизацию
  + вручную определять маршруты и методы для каждого запроса
  + если проект небольшой или требует точной настройки, APIView отлично подходит
* ViewSets
  + автоматизирует маршрутизацию и типичные CRUD-операции с помощью Routers
  + объединяют логику CRUD-операций в одном классе
* Routers
  + помогает автоматизировать и упрощать маршрутизацию RESTful API
  + автоматически связывают URL-маршруты с ViewSets
* Filters  
  + фильтрация данных, которые возвращает API
  + базовые фильтры по параметрам запроса
  + мощные инструменты фильтрации через библиотеку `django-filter`
* рендеринг шаблонов (Server-Side Rendering) у нас нет


### DJANGO RESTFUL HTTP API (Application Programming Interface)
* интерфейс, набор правил
* обмен данных в **формате REST**
* обрабатывает HTTP-запросы (GET, POST, PUT, DELETE)
* обработка запросов и откликов между клиентом и сервером
* позволяет приложениям взаимодействовать друг с другом
* определяются в View-классах или функциях
  + классы, наследущие от `APIView`, `GenericViewSet`, `ViewSet`
* API = WebSocket-путь = группа маршрутов (эндпоинтов)
  + приложение `auth_app`, для каждого из них 5–10 эндпоинтов
  + для управления пользователями 
    - маршрут `GET /users/` список пользователей
    - маршрут API чата = группа всех маршрутов, связанных с чатом
* Endpoint = URL-маршрут = конкретный маршрут, связанный с API
  + выполняет определённое действие
  + Endpoints чата:
    - `GET /chat/rooms/` — список комнат
    - `POST /chat/rooms/` — создать комнату
    - `GET /chat/rooms/<room_id>/messages/` — получить сообщения из комнаты
    - `POST /chat/rooms/<room_id>/messages/` — отправить сообщение
  + список эндпоинтов
    - `python manage.py show_urls` 
    -  `grep -r "path(" backend/`, `grep -r "re_path(" backend/`
    - `curl -X GET http://localhost:8000/api/endpoint/` проверить эндпионт
    - `curl -X POST http://localhost:8000/api/endpoint/ -H "Content-Type: application/json" -d '{"key": "value"}'`
    - with Postman
    - Chrome + расширения
    - Python + библиотека `websockets`
    - автоматичесая документация эндпоинтов **Browsable API** `http://localhost:8000/api/`, `http://localhost:8000/`
* если настроена автоматическая документация (Swagger, Redoc), то список API http://localhost:8000/swagger/, http://localhost:8000/redoc/
* с точки зрения реализации api есть **class based views** (не сильно сложнее) / functions based views (проще)
* **ViewSet/APIView:** обработчики HTTP-запросов
* для APIView, ViewSet и связанных с ними классов
* APIView 
  + инструмент для создания API
  + класс в DRF
  + наследуется от класса View
  + добавляет поддержку сериализации, работы с JSON, вспомогательные методы для обработки HTTP, `GET` `POST` `PUT` `DELETE`
  + подходит для создания простых API с базовой логикой
* ViewSet 
  + более высокоуровневый класс
  + добавляет автоматическую обработку запросов для стандартных операций CRUD (Create, Read, Update, Delete)
  + генерирует маршруты для операций с ресурсами (создание нового объекта, получение списка объектов, обновление, удаление)
  + избежать написания методов для каждого HTTP-запроса, таких как `GET`, `POST`, `PUT`, `DELETE`
  + использует методы по умолчанию, предоставляемые DRF
  + автоматически предоставляет стандартные действия для модели
  + настраивать маршруты для ресурсов API с помощью router
  + для стандартных операций, связанных с моделями (создание, получение, обновление, удаление объектов)
  + подходит для ресурсо-ориентированных API
    - где операции с объектами ("пользователи", "продукты") 
  + настроить API для CRUD-операций над данными, которые уже есть в базе
* bakyt: API for Database - UserProfile works
* как устроен
  + протокол HTTP(S)
  + реализуются
    - через события
    - через статусы в ответах API
    - не через механизм сообщений Django


### DJANGO CHANNELS
* расширение Django, framework, библиотека, надстройка для вебсокетов
* добавляет поддержку асинхронных протоколов
* не сервер, не принимает запросы
* обработка http запросов через Django и WSGI-сервер
* как и DRF слушает запросы, связывает URL запроса с обработчиками
  + отличия от html: непрерывное соединение, стрим
  + другие типы запросов, другой протокол
  + создает ws
  + создает consumer
* WebSockets
  + клиент загружает страницу
  + js инициирует соединение через **WebSocket API**: `new WebSocket`
  + daphne устанавливает ws-соединение
  + daphne передаёт ws-соединение в ASGI-приложение
  + AuthenticationMiddlewareStack
    - проверяет, что ws-соединение аутентифицировано
    - различные методы аутентификации (проверка токенов JWT, Cookies и др)
    - связывает пользователя с запросом, с подключением WebSocket
  + SessionMiddleware (если используется) работа с сессией
    - получение id сессии и связанного с ним пользователя
    - сохранение информации о сессии: id сессии и связанные данные (пользователь, ...)
    - обязателен, если используете сессии (авторизация через `django.contrib.auth`, хранение пользовательские данных между запросами, если работаете с `request.session`)
    - обязателен, если есть `django.contrib.auth` (он опирается на сессии для хранения данных о пользователе)
    - обязателен, если взаимодействие с сессиями на сервере (сессионное хранилище в бд, Redis, файловой системе,...)
    - можно без него, если JWT токены или др безсессионные методы аутентификации (например, в REST API (DRF) сессии не нужны, авторизация через токены)
    - можно без него, если данные о сессии хранятся полностью в зашифрованных cookie (без серверного хранилища)
  + consummer получает `{ "type": "chat.message", "message": "Hello, World!" } с заголовками
  + Backend Channel Layer
  + consumer отправляет через ws обратно пользователю `{ "type": "chat.message", "message": "Hi there!" }` 
  + предоставляет **объект пользователя (scope["user"]) в consumer'ах**, если пользователь аутентифицирован
  + если страница загружена по https://, то ws:// с другим или тем же доменом зачастую будет заблокирован браузером
  +
    | Method                | Real-Time | Message Persistence  | Use Case                           | Complexity |
    |-----------------------|-----------|----------------------|------------------------------------|------------|
    | WebSocket (chat)      | Yes       | No                   | Real-time chats, games             | High       |
    | Redis (Pub/Sub, ws)   | Yes       | No                   | Notifications, chats               | High       |
    | Django Messages       | No        | Yes                  | System notifications, confirmations| Low        |
    | REST API              | No        | Yes                  | Simple notifications, data requests| Low        |
    | Email                 | No        | Yes                  | Important notifications, confirmations| Medium  |
    | Push Notifications    | Yes       | Yes (by service)     | Mobile device notifications        | Medium     |
  + ограничение скорости
  + управление правами доступа
  + обработка ошибок
  + сессии передаются через cookie
  + если сертификат отсутствует, браузер может блокировать подключение WebSocket
  + **обновлений интерфейса** в режиме реального времени
* `consumers.ChatConsumer.as_asgi()` связывает запрос с Consumer-классом
* логика приёма и обработки ws-запросов и аутентификации/авторизации пишется в consumers.py и/или в кастомном middleware
  + но можно организовать WebSocket‐аутентификацию через Django session
    - Channels используют cookie-сессии Django, если пользователь уже аутентифицирован через views, то Channels видит сессию по cookie
   - логика аутентификации (включая проверку подписи на этапе логина) происходит в views
   - ws-протокол подхватывает готовую сессию
* alexey: Layout on the pages – working on it
  + расположение и структура элементов пользовательского интерфейса на веб-страницах
  + Работа с CSS-фреймворками (например, Tailwind CSS или Bootstrap)


### CHANNEL LAYERS
* не сервер
* абстракция, настройка Django Channels
* канал-сервер, канальный слой для Django Channels
* промежуточный уровень для обработки событий в реальном времени
* распределенные задачи (уведомления, обработка сообщений, др асинхронные операций)
  + сообщения от клиента передаются через Channel Layer
  + Channel Layer передаёт сообщение другим серверам/пользователям, подключенным к одной и той же группе
  + ответ от сервера передается обратно через WebSocket-соединение
* публикует в канал, доставляет подписанным
* channels_redis поддержка групповой рассылки
* асинхронный обмен сообщениямм, маршрутизация **между**
  + частям приложения
  + сервером и клиентом
  + клиентами
  + инстансами приложения
  + процессами
  + ws-соединениями и **фоновыми задачами**
  + consumers
  + consumers и группами
  + пользователями или группами 
  + управляет группами (чаты, комнаты)
* **регистрация и управление подключениями** (каналами) пользователей
* управляет **хранением состояния**
* очереди сообщений
  + операции в фоновом режиме (обработка запроса, отправка уведомлений)
  + избежать блокировки основного потока
* не обязателен, если
  + приложение запускается в одном экземпляре
  + все клиенты подключаются к одному процессу
  + обмен сообщениями только между клиентами и один общий чат, нет сложного обмена сообщениями
* без Channel Layer
  + обрабатывать сообщения напрямую внутри процесса, используя **функциональность Django Channels** и WebSocket-соединений
  + Сообщения от клиентов можно сохранять в памяти (словарь, ...) **или в базе данных**
  + для рассылки сообщений всем подключённым клиентам можно использовать **группы, которые предоставляет Django Channels**
  + Django Channels работает в памяти для управления группами и отправки сообщений между клиентами в рамках одного процесса
  + Отсутствие долговременного хранения сообщений (если вы не сохраняете их в базу данных)
  + Django Channels достаточно для управления WebSocket-соединениями и реализацией чата
  + Ручная маршрутизация сообщений: механизм для связи между клиентами (например, вручную управлять Redis Pub/Sub)
  + лишает преимуществ готовой интеграции с Django Channels
* Асинхронный обмен сообщениями может работать без Channel Layers
  + 1 вариант: реализовать вебсокеты самостоятельно с библиотеками `websockets` или `Django ASGI` 
  + 2 вариант: напрямую взаимодействовать с Redis/RabbitMQ/другим брокером сообщений
  + но с channel layers проще, масштабируемо
* масштабирование, легко работают в кластере (через Redis или другой бекенд)
* **websocket один для всех пользователей для чата (лёша)**
* **распределят нагрузку между несколькими процессами/серверами**
* может работать без внешнего channel layer
  + по умолчанию InMemoryChannelLayer
  + работает только внутри одного процесса (одного экземпляра приложения)  
  + ок если вам нужно всего лишь точечно отправлять и получать сообщения в рамках одного процесса
  + широковещательное (group_send) или мульти‐user‐чат‐шаблон если и нужен, то только в рамках одного процесса
  + реал-тайм обновления для одного пользователя (или небольшой группы в рамках одного процесса). Например, вы хотите уведомлять конкретного юзера о событиях (новые сообщения, новые заявки в друзья и т.п.), и ваш сервер – один.  
  + Мини-чат или прямое общение (point-to-point). Если все пользователи сидят в рамках одного сервера (и у вас не требуется распределённая горизонтальная архитектура), то и группы Channels будут работать внутри этого одного процесса 
  + Push-уведомления из кода в конкретный WebSocket‐консьюмер


### REDIS
* Remote Dictionary Server  
* * **репликация** и **кластеризация** для масштабирования и высокой доступности
* хранение данных
* маршрутизация данных
* подсчёт статистики, **временных данных**
* настройка Redis
  + для работы с высоким количеством соединений
  + **защищён паролем**
  + доступен только внутри сети Docker
  + время жизни кэша (`TIMEOUT`), ...
  + проверка: `redis-cli`: `PING`, ответ: `PONG`.
  + `redis-cli info memory` Redis не перегружен?
  + `redis-cli -n 1 flushdb` очистить кэш 
  + Redis WARNING Memory overcommit must be enabled!
    - `sudo sysctl vm.overcommit_memory=1` настройка системы, чтобы Redis мог эффективно сохранять данные в условиях низкой памяти
    - `vm.overcommit_memory = 1` в `/etc/sysctl.conf` сохранить настройку после перезагрузки
  + один сервер Redis = два процесса на одном сервере Redis: кэширование + обслуживание channel layers (c разными ключами и настройками)
* как устроен технически
  + работает в оперативной памяти => высокая скорость записи и чтения
  +* может сохранять данные на **диск**, чтобы избежать потери при сбоях
  + `channels_redis` библиотека для подключения и отправки сообщений в Redis
  + `django-redis` библиотека для **сериализацию** данных, записи, извлечения
  + поддерживает множество языков программирования
  + транзакции и скрипты на языке Lua


### REDIS = BACKEND ДЛЯ CHANNEL LAYERS  
* бэкэнд, брокер сообщений, посредник (если несколько серверов/контейнеров обрабатывают ws-запросы)
  + daphne каждого инстанса/контейнера подключается к Redis-серверу
  + сервер получает сообщение от клиента, отправляет его в канал через Channel Layer
  + Redis доставляет соообщение на другой сервер/инстанс, подписаный на канал
* брокер сообщений для передачи сообщений между различными экземплярами Django
* брокер сообщений для обмена данными между клиентом и сервером
* для работы с асинхронными задачами через Django Channels (**какие ещё задачи**)
* замной ему могут быть:
  + встроенный в Django Channels **in-memory channel layer** может заменить channel layer
    - не требует дополнительных зависимостей
    - подходит для небольших проектов или одиночных контейнеров
    - не нужно устанавливать и настраивать дополнительные сервисы
    - работает только внутри одного процесса
    - при перезапуске сервера данные в памяти будут потеряны, потеря сообщений
    - для чата с обменом сообщениями между клиентами и одним общим чатом 
    - не используйте в продакшен, так как не поддерживает многопроцессные или распределённые среды (**только эта принчина?**)
  + RabbitMQ брокер сообщений для системных сообщений (уведомление о начале турнира, уведомление о добавлении в друзья)
  + PostgreSQL
* Почему Redis:
  + упрощает процесс
  + позволяет использовать channel layer на нескольких процессах и контейнерах (Масштабируемость)
  + распределять нагрузку между несколькими контейнерами (Горизонтальное Масштабирование)
  + **устойчивость к сбоям**
  + хранение данных вне памяти приложения
  + быстрый для обработки большого количества сообщений в реальном времени
  + для долговременного хранения сообщений или очередей
  + обработка большого количества одновременных WebSocket соединений и сообщений
* поддержка Pub/Sub => идеальный для каналов WebSocket
* Pub/Sub публикация/подписка
  + рассылать сообщения всем пользователям чата
  + Pub/Sub в реальном времени (чат, игра Pong)
  + бекенд для Pub/Sub
  + бекенд для **ws-сессий**
  + механизм списков или Pub/Sub для реализации очередей сообщений
* используется механизм Pub/Sub (Publish/Subscribe) для реализации чата?
  + Поиск в коде `PubSub`, `publish`, `subscribe`, `event`, `broker`, `topic`
    - библиотеки  `Redis`, `RabbitMQ`, WebSocket-библиотеки с поддержкой Pub/Sub
  + Если чат реализован через WebSocket, Pub/Sub может быть встроен в логику
    . Например, отправка сообщения (`publish`) уведомляет подписчиков (`subscribe`)
  + Redis = брокер сообщений в режиме Pub/Sub
     - Подключение к Redis (`redis.createClient()` или аналог)
     - Вызовы методов `publish()` и `subscribe()`
  + Если вы используете NestJS, в нем есть встроенная поддержка Pub/Sub через библиотеки, такие как `@nestjs/microservices`.
   - Проверьте папку `services`, `chat`, или `events` на наличие Pub/Sub-логики.
  + Найдите модули или файлы, связанные с чатом (`chat.service`, `chat.controller`)
    - если сообщения отправляются на брокер или WebSocket, это может быть признаком Pub/Sub
  + `package.json` зависимости, связанные с Pub/Sub `cat package.json | grep -E "redis|socket.io|nestjs/microservices"`
  + Отправьте сообщение в чат и проверьте:
    - Логирование на сервере. Часто Pub/Sub системы логируют публикацию и доставку сообщений.
    - Задержки при передаче сообщения. Если сообщение доходит мгновенно, вероятно используется WebSocket с логикой Pub/Sub


### REDIS ДЛЯ ХРАНЕНЯ СЕССИЙ
* для управления **состоянием** ws-соединений
  + хранит **данные о подключениях**
  + очередь задач (отправки email, push-уведомления, логирование, обработка фоновых задач (обновление статистики матчей))
  + для создания очередей задач (для обмена задачами между различными частями приложения, такими как обработка сообщений или фоновые задачи)
* управление пользовательскими сессиями в веб-приложениях при подключении пользователей к ws
  + хранение данных о текущих авторизованных пользователях
 

### REDIS ДЛЯ КЭШИРОВАНИЯ
* django cash framework обращается к redis
  + инфраструктура для кэширования
  + хранение кэша (запросы, объекты, шаблоны, ...)
  + не требует добавления в INSTALLED_APPS
  + настраивается в CACHES
  + Django API для кэширования данных
* Redis = хранилище данных в памяти
* Django кэширует данные, которые часто запрашиваются, но редко изменяются
  + временне данные для ускорения работы приложений (имя, аватар, статус)
  + результатов сложных SQL-запросов, сложных вычислений
  + рендеринг HTML-шаблонов, чтобы повторные запросы быстрее обрабатывались
  + результаты REST API-запросов, чтобы не нагружать базу данных повторными запросами
  + API-ответов (данных профиля пользователя, данных статистики)
  + сеансы: **хранилище** сеансов для авторизованных пользователей
  + пользовательские сессии, что **ускоряет работу сессий в распределённых системах**
  + временный статус пользователя: **онлайн/офлайн**
* по умолчанию django кэширует в памяти `django.core.cache.backends.locmem.LocMemCache` 
  + ограничено по производительности и функциональности
  + кэш в процессе работы сервера
  + если приложение работает в пределах одного сервера и не требует масштабируемости
  + нет сильной нагрузки или большого объёма данных
  + не требует настройки и внешнего сервиса
  + доступ к данным в памяти быстрее
  + в оперативной памяти локального сервера (`LocMemCache`)
    - очищается при перезапуске приложения
  + файловое кэширование (`FileBasedCache`)
    - медленнее, чем кэш в памяти или Redis
    - требует места на диске
  + в базе данных (`DatabaseCache`)
    - замедляет производительность
* Redis для кэширования
  + `django_redis` берёт на себя работу с Redis, предоставляя **Django-friendly API**
  + **класс клиента определяет, как Django взаимодействует с Redis**
  + `OPTIONS` = `CLIENT_CLASS` настраивает использование `DefaultClient` из библиотеки `django-redis` для взаимодействия с Redis
  + работает в оперативной памяти, быстрый
  + сохранять кэшированные данные на диск (persistent storage) для предотвращения потери данных после перезапуска (например, для сессий)
  + сложные типы данных в кэше (списки, множества, хеши)
  + сложные стратегии кэширования: TTL (время жизни), евикция (удаление старых или неиспользуемых данных), ограничение количества запросов от одного пользователя за определённое время (rate limiting)
  + легко масштабируется
  + может использоваться в распределённых системах, в многосерверных / контейнеризованных средах
    - данные в процессе/сервисе Redis, а не в памяти отдельного экземпляра приложения
    - данные кэша разделяются между всеми процессами и серверами, что делает его идеальным для кластеров
    - несколько серверов обращаются к одному кэшированию
    - Redis как общий слой
* django REST Framework обращается к redis
  + кэшировать ответы API, особенно одинаковые для множества пользователей
  + кэширование для представлений API: если данные уже есть в кэше, они будут возвращены из Redis, если нет, будут получены из базы данных и затем кэшированы
  + кэширование на уровне вьюх или сериализаторов
* django channels обращается к redis
  + CHANNEL_LAYERS: позволяет Django Channels использовать Redis для обмена сообщениями через WebSocket или другие каналы
    - redis= очередь для сообщений, которые могут быть отправлены клиентам
    - reids = механизм синхронизации для разных процессов или серверов
  + redis для кэширования промежуточных результатов в асинхронных операциях (промежуточные результаты обработки WebSocket-соединений)
* bakyt: только кэш сообщений, чат и **системные**


### DATABASE PostgreSQL
* СУБД для хранения пользователей, сообщений, данных о матчах в Pong, статистики, ...
* to set up the database **для чего?**
  + `python manage.py makemigrations`, `python manage.py migrate`
  + create a superuser `python manage.py createsuperuser`
* `psql -U myuser -d mydatabase` `\dt`
  ```
   Schema |                Name                | Type  | Owner  
  --------+------------------------------------+-------+--------
   public | auth_group                         | table | myuser
   public | auth_group_permissions             | table | myuser
   public | auth_permission                    | table | myuser
   public | django_admin_log                   | table | myuser
   public | django_content_type                | table | myuser
   public | django_migrations                  | table | myuser
   public | django_session                     | table | myuser
   public | myapp_game                         | table | myuser
   public | myapp_tournament                   | table | myuser
   public | myapp_tournament_players           | table | myuser
   public | myapp_userprofile                  | table | myuser
   public | myapp_userprofile_friends          | table | myuser
   public | myapp_userprofile_groups           | table | myuser
   public | myapp_userprofile_user_permissions | table | myuser
  ```


### СТАТИЧЕСКИЕ ФАЙЛЫ html js CSS изображения шрифты
* в разработке:
  + **`python manage.py runserver` при DEBUG = True**
    - you use django.contrib.staticfiles
  + manually serve user-uploaded media files from MEDIA_ROOT
    - use django.views.static.serve() view
    - don’t have django.contrib.staticfiles in INSTALLED_APPS
    - if STATIC_URL = static/, addi urlpatterns = [ ] to your urls.py `static(settings.STATIC_URL, document_root=settings.STATIC_ROOT)`
      - works only if the prefix is local (static/) and not a URL (http://static.example.com/)
    - only serves STATIC_ROOT folder
    - doesn’t perform static files discovery like django.contrib.staticfiles
    - serves static files via a wrapper at the WSGI application layer => static files requests do not pass through the middleware chain
  + напрямую ссылаться на файлы через URL
  + всё, что идёт ниже, можно в production
* чтобы все файлы находились/отдавались там, где ожидает браузер
* 'django.contrib.staticfiles'
  + обслуживает стат файлы для /admin
  + позволяет добавлять версии, хеши в имена файлов на фронтенд, чтобы избежать кеширования старых версий
  + для простоты настройки оставляют
  + можно убрать, если cтат файлы обслуживаются  через `/static` Nginx, не нужен `collectstatic`, SPA и фронтенд отделен от бэкенда
* `collectstatic`
  + админские CSS (`base.css`, `dashboard.css`, ...), стат. файлы DRF для **browsable API** или других админ-панелей - в `django.contrib.admin`  
   - `ls $(python -c "import django; print(django.__path__[0])")/contrib/admin/static/admin/css/`
  + `python manage.py collectstatic` собирает статические файлы (включая админку, DRF) в STATIC_ROOT = app/staticfiles/
    - **STATIC_ROOT не трекается гитом**
  + **Frontend имеет только права на чтение статических файлов Backend**
* Nginx обслуживает ваши собственные стат фалйы и те, что созданы Django, по одному и тому же пути `/static/`
  + `location /staticfiles/ { alias /app/staticfiles/; }` # папка внутри контейнера Nginx
  + Daphne передает запросы к Django-приложению
  + Django: STATIC_URL для указания, где файлы
    - STATIC_URL = 'staticfiles/' 
    - **когда меняю на staic, почему-то статические файлы фронтенда перестают открываться**
  + Nginx обслуживает статические файлы по пути `STATIC_URL = '/staticfiles/'` (url для nginx, не название папки)
  + location /static/ { alias /usr/share/nginx/html/static/;} }: мои стат файлы в `/static` контейнер Nginx
    - файлы, запросы к которым начинаются с `/static/`
    - `/static/style.css` обслуживается из `/usr/share/nginx/html/static/style.css`
  + location /staticfiles/ { alias /app/staticfiles/; }
    - Nginx обслуживает файлы, запросы к которым начинаются с `/staticfiles/`
    - Например, запрос на `/staticfiles/admin/css/style.css` обслуживается из `/app/staticfiles/admin/css/style.css`
    - Nginx проксирует запросы к Django
    - стат файлы созданные Django собираются в `/app/staticfiles` в контейнере backend,
  + создать общий том, чтобы Nginx и backend контейнеры обменивались статическими файлами 
  + два разных пути для обслуживания разных типов статических файлов, Nginx направляет запросы к нужным папкам на основе их URL
* в Django-шаблоне либо нет упоминания о стилях, стили приходят с фронтенда (собранный **бандл**)
* `Daphne` не обслуживает статические файлы по умолчанию
  + может делать это с библиотекой WhiteNoise
* проверка 
  + static backend: https://localhost:4443/admin/
  + static backend: https://localhost:4443/staticfiles/admin/css/base.css доступны через Nginx, публичный, файлы доступны на сайте (`STATIC_URL = '/static/'`)
  + static frontend: http://localhost:4444/static/frontend/css/popUpChat.css
  + static frontend: https://localhost:4443/static/frontend/css/popUpChat.css
  + если CSS-файл кэшировался, браузер может залипать на устаревшей версии
    - добавить ?v=123 в конце ссылки или очистить кэш
  + `python manage.py findstatic css/popUpChat.css`
  + `docker-compose logs frontend`
  + `docker-compose logs backend`
  + docker exec -it frontend ls -la /usr/share/nginx/html/static/frontend/css/ убедиться что Nginx имеет права на чтение
  + файлы в `staticfiles` **только чтение** для Nginx
  + DevTools → Network
    - какой путь к CSS-файлу пытается загрузить браузер
    - код ответа 200 = OK
    - код ответа 404: Django не может найти файл, конечный URL, по которому файл запрашивается
  + **built-in testing client LiveServerTestCase**
    - only assumes the static content has been collected under STATIC_ROOT
    - staticfiles ships its own django.contrib.staticfiles.testing.StaticLiveServerTestCase, a subclass of the built-in one that has the ability to transparently serve all the assets during execution of these tests in a way very similar to what we get at development time with DEBUG = True, i.e. without having to collect them using collectstatic first
* put static files in my_app/static/my_app, not in my_app/static/ 
* CSS-OM = дереао как DOM
* bootstrap готовые стили, можно создавать кастомные на основе них
* .map (Source Map) 
  + При минификации или транспиляции CSS/JS (при сборке) код CSS превращается в сжатую скомпилированную версию
  + Source Map хранит сопоставление (mapping) между сжатым и исходным кодом
  + видеть исходный код для отладки кода в браузере
  + в продакшен отключают генерацию .map-файлов, чтобы уменьшить вес приложения и не раскрывать детали исходного кода


### ТОКЕНЫ БЕЗОПАСНОСТЬ
| Характеристика       | Токен авторизации            | CSRF-ключ                  | Сессионный ключ            |
|----------------------|------------------------------|----------------------------|----------------------------|
| Назначение           | Аутентификация               |                            | Идентификация сессии       |
| Где хранится?        | HTTP-заголовки, cookie, JS-хранилища | Cookie (обычно)    | Cookie (обычно)            |
| Пример использования | REST API, GraphQL, WebSockets| Формы, AJAX-запросы        | Веб-приложения с авторизацией|

| токен/ключ    | Назначение                          | Где хранится                         | Особенности | пример |
|---------------|-------------------------------------|--------------------------------------|-------------|--------|
| CSRF          | от подделки запросов (CSRF-атак)    | cookie csrftoken                     | проверяется сервером при POST/PUT запросах, передаётся в скрытом поле формы или заголовке            | Защита формы входа; обеспечение легитимности запросов от пользователя|
|ток авторизации| авторизация                         |HTTP-заг cookie locStorage sessStorage| Используется для REST API и WebSocket; может быть JWT | авторизация REST API (`Authorization: Bearer <token>`) или ws-соединения |
|сессионный ключ| управление пользов! сессией         | cookie sessionid                     | долговрем. связь пользователя с сервером; подходит для **веб-интерфейсов**; сохраняется на сервере | отслеживание состояния авторизации в веб-приложении; данные польз. (корзина)|
| ws-ток        | авторизация при установке ws-соедине| URL параметр / заголовок ws-запроса  | передаётся при установке соединения; проверка прав доступа | авторизация чата, потоковой системы `wss://example.com/ws/chat/?token=<...>`|
| Email-токен   | подтвержд email-адреса, восст пароля| URL отправляемое пользователю        | одноразовый токен| подтверждение email ссылкой `https://example.com/verify-email/?token=<>`|
| API-ключи     |идентификация, авторизация стор прилож| HTTP заголовок запроса              | передаётся с каждым запросом; ограниченный доступ к API | интеграция с внеш сервисом, предост. публ. API `Authorization: Api-Key <>`|
| щифров. ключи | шифрование сообщений                | На сервере в хранилище               | не передаётся клиенту; для шифрования | шифрование сообщений чата на стороне сервера|
| OAuth-токен   | аутентиф и авторизация через Google |  cookie или localStorage             | для OAuth 2.0. | авторизация Google OAuth API; доступ к данным польз. через API (email, ...)|

* js обращается к rest api (post) endpoints /history, /users/, /send
  + login pages.js - запрос post к бэку
  + в заголовке запроса каждый раз CSRF токен, чтобы знать, что это не юзер с третьего сайта
* **csrf MW проверяет токен, а до этого не надо его проверять?**
* можно вообще без сессий: если вся информация хранится в токенах или сессии вообще не нужны (например, чисто API-проект)
**если используется исключительно токен-авторизация (JWT), можно отключить django.contrib.sessions.middleware.SessionMiddleware и  django.middleware.csrf.CsrfViewMiddleware**
* CSRF (CROSS-SITE REQUEST FORGERY) ключ, токен
  + для защиты формы на странице входа
  + необходимы в stateful-приложениях, где используется sessionid или cookie для отслеживания пользователя
  + В stateless-приложениях с JWT-авторизацией CSRF-токен обычно не используется
  + для защиты от CSRF-атак
    - убедиться, что запрос исходит от легитимного пользователя
  + защищает формы и AJAX-запросы от подделки запросов
  + для защиты UI-приложений
  + Django по умолчанию сохраняет в cookie `csrftoken`
    - доступно только для чтения с клиентской стороны (не `HttpOnly`)
    - JavaScript может получить доступ к токену (например, для AJAX-запросов)
  + при отправке POST-запроса CSRF-токен передаётся серверу в заголовке или скрытом поле формы
  + django сравнивает токен из запроса с токеном из cookie, если совпадают, запрос легитимен
  + cookie устанавливается с параметром `Secure`, если используется HTTPS
  + Не должен быть доступен для сторонних доменов (используйте `SameSite=Lax` или `SameSite=Strict`).
  + токен **в settings.py**
  + чтобы никто не зашёл в БД
  + отдать клиенту при первом запросе
  + клиент вставлять токен в заголовок запроса к **api** и  при post/get запросах
    - **а при вебсокетах?**
  + views.py **набор проверок** добавить при запросе к api
  + https://docs.djangoproject.com/en/5.1/howto/csrf/
* ТОКЕН АВТОРИЗАЦИИ
  + для подтверждения того, что пользователь авторизован
  + идентифицировать пользователя без необходимости хранения состояния на сервере
  + содержит информацию о пользователе (например, его ID) и подписан сервером
  + для аутентификации пользователей Веб-приложения, авторизации, обмена информацией
  + для аутентификации в REST API или WebSocket
  + для API-запросов (например, токен JWT для мобильного приложения)
  + Часто применяется в RESTful API и WebSocket-соединениях
  + используется для API (например, для общения с мобильным приложением или фронтендом)
    - удобно для аутентификации в stateless-приложениях
  + в API и системах с аутентификацией без сохранения состояния (stateless authentication)
  + может храниться:
    - в HTTP заголовках (например, `Authorization: Bearer <token>`)
    - в cookie (реже, обычно для обеспечения защиты от XSS/CSRF)
    - в localStorage или sessionStorage (в клиентском JavaScript)
  + Пользователь получает токен после входа в систему
    - передаёт его с каждым запросом
    - Сервер проверяет, чтобы убедиться, что пользователь авторизован
  + если проект использует токены для авторизации, чаще всего это JWT (JSON Web Token)
    - с ним не будет необходимости в CSRF-токене
    - формат токена для передачи данных между сторонами
    - использует подпись, чтобы предотвратить изменения данных
    - все данные для проверки токена содержатся в нём, нет необходимости хранить их на сервере
    - nginx хранит jwt не в cookie, а в **локальном хранилище** (в переменной в js, обновляем как только он истечет)
    - безопасность доступа к API-ресурсам
    - Если украдён, злоумышленник может использовать его до истечения срока действия: устанавливайте короткий срок действия токена
    - отзыв токенов сложен, они самодостаточны (нет централизованного хранилища): реализуйте механизмы обновления и отзыва токенов
    - информация о пользователе
    - подписаны сервером
    - являются самодостаточными
    - имеют риски в случае компрометации
* СЕССИИОННЫЙ КЛЮЧ = ФАЙЛ СЕССИИ
  + для управления сессией после входа через веб-интерфейс
  + идентифицирует пользователя и связывает его сессии с данными на сервере
  + подходит для stateful-приложений (например, с веб-интерфейсом)
  + идентификатор для отслеживания пользовательской сессии
  + для сопоставления пользователя с данными, хранящимися на сервере (состояние авторизации, данные корзины)
  + хранится в cookie `sessionid`
    - cookie `HttpOnly`, чтобы предотвратить доступ к нему из JavaScript
    - cookie `Secure`
    - Настройте время жизни сессии (`SESSION_COOKIE_AGE`)
  + При каждом запросе браузер отправляет `sessionid` серверу
    - Django использует его для извлечения данных из хранилища сессий (база данных, кэш, файл)
* WEBSOCKET TOKEN
  + используемый для авторизации пользователя при установке WebSocket-соединения
  + при установке WebSocket-соединений клиент может передавать токен в URL или заголовках
    - `wss://example.com/ws/chat/?token=<your-token>`.
  + Проверка прав доступа и идентификация пользователя
  + могут быть основаны на JWT или другом формате, но их передача должна быть защищена (например, через HTTPS/WSS)
* EMAIL-ТОКЕН
  + для подтверждения email-адресов, восстановления пароля, ...
  + При отправке ссылок пользователю `https://example.com/verify-email/?token=<your-token>`
  + обеспечение одноразовой аутентификации для выполнения действий
* API-КЛЮЧИ
  + используемые для идентификации и авторизации сторонних приложений или интеграций
  + Для интеграции с внешними сервисами или предоставления публичного API
    - `Authorization: Api-Key <your-key>` в заголовке запроса
* ШИФРОВЛАЬНЫЕ КЛЮЧИ
  + для работы с зашифрованными данными (если шифруете сообщения чата на стороне сервера)
* OAUTH-ТОКЕНЫ
  + если хранится на стороне клиента в Local Storage
    - Простота реализации.
    - Долгосрочное хранение: данные остаются после перезагрузки страницы.
    - Уязвимость к XSS-атакам: злоумышленники могут украсть токен, если внедрят вредоносный JavaScript-код.
  + если хранится на стороне клиента в Session Storage
    - Простота реализации.
    - Повышенная безопасность: данные удаляются при закрытии вкладки.
    - Также уязвимо к XSS-атакам.
    - Данные недоступны между вкладками.
  + если хранитсяв памяти приложения (In-Memory), в оперативной памяти клиента (например, в переменной в JavaScript)
    - Лучшая защита от XSS, поскольку токен недоступен после перезагрузки страницы.
    - Токен исчезает при обновлении страницы, что может вызывать неудобства.
    - Требует дополнительной реализации для повторного получения токена.
  + если хранится на стороне сервера в базе данных (**Access Token, Refresh Token**)
    - Высокая безопасность: токен недоступен напрямую для клиента.
    - Удобное управление сроком действия и обновлением токенов.
    - Более высокая задержка: доступ к базе данных может быть медленнее, чем кэш.
    - Требует дополнительного места и обработки.
    - сохраняйте Access Token и Refresh Token в защищённой таблице, связанной с пользователем
    - шифруйте токены перед сохранением в базу данных
    - убедитесь, что доступ к таблице с токенами ограничен только необходимыми сервисами
  + если хранится на стороне сервера в Redis или другом кэше)
    - Быстрый доступ: Redis работает значительно быстрее, чем база данных.
    - Легко управлять истечением токенов (TTL).
    - Токены временные: данные в Redis очищаются при сбое или перезагрузке.
  + если хранится на стороне сервера в HTTP-only cookies
    - Токен (или сессионный идентификатор, связанный с токеном) передаётся клиенту в виде HTTP-only cookie.
    - Токен защищён от XSS-атак (недоступен через JavaScript).
    - браузер автоматически отправляет cookies на каждый запрос.
    - Требует правильной настройки (например, `SameSite`, `Secure`, и HTTPS).
    - Ограниченная длина cookies (токен с большими данными может не поместиться).
  + гибридный подход**
    - Access Token хранится в памяти клиента (`In-Memory`) для минимизации уязвимости к XSS.
    - Refresh Token хранится в HTTP-only cookie для долгосрочного хранения и обновления Access Token.
  +
    | **Место хранения**        | **Безопасность**      | **Удобство** | **Долговечность** |
    |---------------------------|-----------------------|--------------|-------------------|
    | Local Storage             | Уязвимо к XSS         | Высокое      | Высокая           |
    | Session Storage           | Уязвимо к XSS         | Среднее      | Средняя           |
    | In-Memory                 | Защищено от XSS       | Низкое       | Низкая            |
    | База данных (сервер)      | Очень высокая         | Низкое       | Высокая           |
    | Redis (сервер)            | Высокая               | Высокое      | Средняя           |
    | HTTP-only cookies         | Защищено от XSS/CSRF  | Высокое      | Средняя/высокая   |
  + Если требуется высокая безопасность
    - Храните Access и Refresh Tokens на сервере (в базе данных или Redis)
    - Используйте HTTP-only cookies для передачи сессионных данных
  + Если важно удобство разработки
    - Используйте Local Storage или Session Storage
    -  защищать приложение от XSS-атак
  + Гибридный подход
     - Access Token в памяти клиента (**In-Memory**) для временного использования
     - Refresh Token в HTTP-only cookies для долговременного хранения и обновления Access Token
  + Frontend отправляет запросы к Backend
    - backend использует токены для взаимодействия с внешними сервисами
    - токены никогда не покидают Backend
  + backend может использовать токены для взаимодействия с внешними API без необходимости передачи токенов на Frontend
  + получение Токенов
    - пользователь инициирует аутентификацию через Frontend
    - frontend **перенаправляет пользователя на OAuth-провайдера через Backend**
    - после аутентификации OAuth-провайдер перенаправляет пользователя обратно на Backend с кодом авторизации
    - backend **обменивает** код авторизации на Access Token и Refresh Token
  + кэшируйте Access Tokens в Redis для быстрого доступа, особенно если Backend часто обращается к внешним API
  + используйте Redis для хранения сессий пользователей, **связывая их с соответствующими OAuth-токенами**
  + как Backend взаимодействует с геолокационными сервисами
    - извлекает Access Token из базы данных или Redis.
    - Использует токен для аутентификации запросов к геолокационному API (например, Google Maps API).
    - При истечении срока действия Access Token, Backend использует Refresh Token для получения нового Access Token.
    - Создайте отдельные сервисы или модули на Backend, которые отвечают за взаимодействие с геолокационными API.
    - Эти сервисы используют Access Tokens для аутентификации запросов.
  + Убедитесь, что все внешние запросы (Frontend и OAuth-потоки) проходят через HTTPS.
  + Используйте валидные SSL-сертификаты для вашего домена.
  + Настройте автоматическое обновление Access Tokens с помощью Refresh Tokens.
  + Реализуйте механизмы для аннулирования токенов при необходимости.
  + Настройте правильные заголовки для обеспечения безопасности (например, **`Strict-Transport-Security`**)
  + **Backend отвечает за весь OAuth-поток**, включая получение, хранение и обновление токенов.
  + **Токены хранятся в защищённой базе данных** и кэшируются в Redis для быстрого доступа.
  + **Frontend не хранит токены**; вместо этого использует безопасные сессии через HTTP-only cookies для взаимодействия с Backend.
  + **Геолокационные компоненты на Backend** используют сохранённые токены для взаимодействия с внешними API.
  + **Обеспечивается безопасность токенов** через шифрование, ограничение доступа и использование защищённых методов передачи данных.
* PUSH NOTIFICATION ТОКЕНЫ
  + Если приложение поддерживает уведомления, эти токены используются для отправки сообщений на устройства пользователей
* SSL-сертификат решает другие задачи, связанные с безопасностью
  + SSL-сертификаты шифруют соединение, защищая передаваемые токены (CSRF, авторизации, WebSocket)
  + токены (JWT, sessionid) передаются внутри защищённого соединения, чтобы их не могли перехватить злоумышленники
* Идентификация = Кто вы
  + Пользователь вводит логин и пароль
  + Сервер проверяет данные и создаёт **JWT**, содержащий информацию о пользователе, имя пользователя
  + Токен отправляется клиенту
  + служба аутентификации хранит закрытый ключ и может выдавать токены
  + другие микросервисы могут проверять их с помощью открытого ключа
  + Самый простой и самый безопасный — **cookie**
    - все микросервисы имеют одно и то же имя хоста => файл cookie будет автоматически отправляться браузером при каждом запросе
    - Со стороны  у вас есть расширение для JWT, но мне не удалось заставить его работать с RSA, проще создать собственное промежуточное программное обеспечение для аутентификации
  + https://dzone.com/articles/using-jwt-in-a-microservice-architecture
* Аутентификация = докажите, что это вы
  + система требует доказательства (пароль, одноразовый код (OTP) из SMS, отпечаток пальца, лицо
    - email + password или через 42
  + асимметричный алгоритм (например **RSA**)
  + https://django-rest-framework-simplejwt.readthedocs.io/en/latest/getting_started.html
  + https://openclassrooms.com/fr/courses/7192416-mise-en-place-une-api-avec-django-rest-framework/7424720-give-access-with-the-tokens
* Авторизация = что вам разрешено делать?
  + что вы можете делать, к каким ресурсам имеете доступ
  + пытаетесь открыть /admin - система проверяет, есть ли у вас права
* SSL session ID
  + низкоуровневая часть протокола SSL/TLS
  + для ускорения установления безопасного соединения
  + сохраняется на сервере
  + может быть сохранена в cookie, но это не обязательно
* sessionId cookies
  + веб-ориентированный идентификатор сессии
  + для отслеживания состояния пользователя на сервере
    - например, для аутентификации или сохранения данных между запросами
* **Stateful/Stateless**
* где backend хранит сессии?
  + настройки бекенда**
* хранятся (`settings.py` `SESSION_ENGINE`)
  + База данных в таблице django_session (по умолчанию)
    - `session_key`: Уникальный идентификатор сессии.
    - `session_data`: Зашифрованные данные сессии.
    - `expire_date`: Дата истечения срока действия сессии.
  + Файловая система (в виде файлов)
  + в кеше, настроенном в `CACHES`
  + кеш с fallback на базу данных: сначала пишутся в кеш, а при его недоступности — в базу данных
  + на стороне клиента в зашифрованных cookie
* **http2**
* `SECRET_KEY = 'django-insecure-5equ&8dpiahftu52n^a5aq=52s8a_d8ty6(smkzjpsmvyk8q(#'`
  + для шифрования сессионных данных (cookies) и других временных данных, создаваемых Django
  + для генерации подписи токенов CSRF и других значений, требующих криптографической защиты
  + для генерации случайных токенов, например, в механизмах сброса пароля
  + Если злоумышленники узнают ваш `SECRET_KEY`, они могут:
    - Создавать фальшивые cookies, сессии или токены.
    - Подделывать запросы или манипулировать приложением.
* Не храните секретные переменные и файлы (например, `.env`, SSL-сертификаты) в системе контроля версий. Используйте инструменты, такие как [Docker Secrets](https://docs.docker.com/engine/swarm/secrets/) или специализированные менеджеры секретов (например, [HashiCorp Vault](https://www.vaultproject.io/)) для управления конфиденциальными данными.
* autorisation
  + При каждом запросе клиент отправляет токен в заголовке Authorization: Bearer <token>
  + Сервер декодирует токен, проверяет его подпись, использует данные для авторизации (например, проверяет роль пользователя)
  + виды авторизаций: Ecole 42, гугл, логин и пароль с базы данных
  + задача на бэкэнде
  + внедрить 2FA через электронную почту
    - Мы выполнили первую часть, генерируя код и все такое
    - были проблемы с последней частью, где мы на самом деле отправляем электронное письмо
      - попробовали sendgrid life, было сложно добиться успеха в отправке электронных писем
    - мы использовали Gmail, найти путь через пользовательский интерфейс Google для создания «mdp приложения» на стороне Django, функция send_mail
* можно без Session MiddleWareStack, если JWT токены или др безсессионные методы аутентификации (например, в REST API (DRF) сессии не нужны, авторизация через токены)
* можно без Session MiddleWareStack, если данные о сессии хранятся полностью в зашифрованных cookie (без серверного хранилища)
* JWT тоже могут использоваться для аутентификации пользователей
  + их доступность обеспечивается через middleware Channels
  + JWTможет быть передан через заголовки WebSocket-сообщений или параметры URL
  + нужно вручную обрабатывать его в consumer
* DRF: проверка пользователя выполняется при каждом запросе через аутентификационные классы (TokenAuthentication, SessionAuthentication, JWTAuthentication)
  + каждый запрос должен содержать соответствующий токен или cookie для подтверждения личности пользователя
  + Контроль доступа Обрабатывается через permissions (например, `IsAuthenticated`)
* Django Channels: Аутентификация один раз при установлении соединения
  + Использует middleware (`AuthMiddlewareStack`) для сопоставления пользователя.
  + Поддерживает передачу аутентификационных данных через Cookie (например, `sessionid`) или токены
* если хотите объединить аутентификацию для DRF и Django Channels, рекомендуется использовать JWT или SessionAuthentication и настроить  middleware
* any password stored in your database, if applicable, must be hashed; a strong password hashing algorithm (subject)
* your website must be protected against **SQL injections/XSS** (subject)
* if you have a backend or any other features, it is mandatory to enable an HTTPS connection for all aspects (Utilize wss instead of ws...)
* some form of validation for forms and any user input, either within the base page if no backend is used or on the server side if a backend is employed (subject)
* the security of your website (subject)
  + regardless of whether you choose to implement the JWT Security module with 2FA
  + even if you decide not to use JWT tokens
  + for instance, if you opt to create an API, ensure your routes are protected
* views.py проверяет токен/подпись
* alexey: Authorization and authorization logic on frontend — almost works
* Implementing a remote authentication OAuth 2.0 authentication with 42 (subject)
  + obtain the credentials and permissions from the authority to enable a secure login
  + user-friendly login and authorization flows that adhere to best practices and security standards
  + the secure exchange of authentication tokens and user information between the web application and the authentication provider
* аутентификация реализуется через middleware, через стандартный механизм HTTP-запросов, например, с использованием **JWT или сессий**
* сессии управляются с помощью cookie-сессионных данных или токенов для аутентификации API


### COOKIE LOCALSTORAGE SESSIONSTORAGE
| **Свойство**        | **Cookie**                            | **LocalStorage**      | **SessionStorage**             |
|---------------------|----------------------------------------|----------------------|-----------------------------------|
| **Хранение данных** | В браузере, отправляются серверу      | Только в браузере     | Только в браузере                    |
| **Срок действия**   | До истечения срока или закрытия браузера | Постоянное         | До закрытия вкладки/окна             |
| **Объём**           | ~4 KB                                 | 5-10 MB               | 5-10 MB                              |
| **Доступ к данным** | JavaScript и сервер (если `HttpOnly` нет) | Только JavaScript | Только JavaScript                   |
| **Область действия**| Общая для всего домена                | Общая для домена      | Изолирована для вкладки              |

* Cookie
  + текстовые данные
  + автоматически отправляются серверу при каждом HTTP-запросе на соответствующий домен.
  + использование:
    - хранение сессионных идентификаторов (`sessionid`)
    - настройки сайта (язык)
    - маркеры аутентификации
    - Используйте для передачи данных между клиентом и сервером (например, для аутентификации)
    - Подходит для сессионных данных, которые требуют взаимодействия с сервером
* localStorage
  + хранилище данных ключ-значение
  + для долговременного хранения данных
  + на устройстве пользователя, изолированно для каждого домена
  + сохраняются даже после закрытия браузера или перезагрузки устройства
  + не передаётся серверу
  + доступны только для того домена, который их создал.
  + доступны только через JavaScript
  + использование: 
    - хранение пользовательских предпочтений (тема сайта)
    - кеширование данных (JSON-ответы от API)
    - для долговременного хранения данных, доступных только клиенту (например, настройки интерфейса)
* SessionStorage**
  + Похож на localStorage
  + данные хранятся только на время текущей сессии браузера
  + изолированно для текущей вкладки или окна
  + данные удаляются, как только вкладка или окно браузера закрываются
  + доступны только через JavaScript
  + доступны только для вкладки или окна, где они были созданы
  + использование
    - хранение временных данных для одной сессии пользователя
    - статус пользователя на текущей странице (выбранные фильтры)
    - для временных данных, которые нужны только на время одной сессии пользователя

### протоколы
* **прикладные** протоколы: определяют, как данные кодируются и интерпретируются
  + HTTP (HyperText Transfer Protocol) для передачи данных между браузером и сервером
    - данные передаются в открытом виде
  + HTTPS (Secure)
    - шифрование через протокол SSL/TLS
  + Redis
  + PostgreSQL
* **транспортные**
  + TCP (Transmission Control Protocol)
    - сетевой протокол для передачи данных на порту
    - надежность соединения
    - без потерь, в нужном порядке, без дублирования
    - управление потоком: отправка данных регулируется, чтобы не перегружать получателя
    - контроль ошибок: если пакет теряется или повреждается, он будет переотправлен
    - Docker по умолчанию использует TCP, так как он подходит для большинства сетевых приложений
    - HTTP, HTTPS, подключение к PostgreSQL, redis  используют TCP
  + UDP (User Datagram Protocol)
    - менее надежен, но быстрее
  * SSL/TLS 
    + 1. шифровать соединение между браузером и nginx
      - браузер шифрует данные перед их отправкой на сервер
      - nginx отправляет ответ, зашифрованный для клиента
      - браузер расшифровывает с использованием **сессионного ключа**
    + 2. аутентификация сервера (подключаетесь к правильному серверу)
      - nginx подтвердает подлинность **сервера** (**браузер** проверяет, подписан ли сертификат доверенным центром)
    + 3. защита от **подделки данных**
    + браузер запрашивает установление защищённого **соединения** (HTTPS)
    + используется публичный ключ сервера (SSL-сертификат)
    + **сессионный ключ** согласуется в процессе установки соединения
    + nginx установливет **канал связи** SSL/TLS между сервером и браузером, чтобы шифровать трафик, использует сертификат и приватный ключ
    + nginx расшифровывает данные с использованием приватного ключа, связанного с SSL-сертификатом, использует сертификат и приватный ключ
    + `ssl_protocols TLSv1.2 TLSv1.3;` версии, TLS 1.1, SSLv3, ... не поддерживаются из соображений безопасности  
    + `ssl_ciphers 'HIGH:!aNULL:!MD5';` использовать надежные шифры, `!aNULL` и `!MD5` исключают небезопасные варианты  
    + `ssl_prefer_server_ciphers on;` браузер отдаёт предпочтение **списку шифров** сервера, а не клиента
    + генерируете `.csr` запрос на подпись сертификата (Certificate Signing Request) с приватным ключом локально, отправляете `.csr` в удостоверяющий центр (Let’s Encrypt, ...), получаете `.crt`  
    + `.key` приватный ключ для расшифровки соединения и подтверждения подлинности, хранится на **сервере**
    + `.crt` сертификат, выданный CA (Certificate Authority) или сгенерированный самостоятельно (self-signed)  
    + настраивается на стороне Nginx
    + SSL-сертификат (TLS-сертификат) для обеспечения защищённого соединения между клиентом и сервером
      - не является токенои или ключои в традиционном понимании
      - Шифрование данных, предотвращает перехват данных (паролей, сообщений) при их передаче
      - аутентификация сервера: подтверждает, что клиент общается с доверенным сервером
      - защита от MITM (Man-in-the-Middle): Исключает вмешательство третьих лиц в соединение
      - защиты HTTPS-запросов
      - шифрование WebSocket-соединений (wss://)
      - защищают транспортный уровень
      - не связаны с аутентификацией или авторизацией пользователя
* валидация данных
  + **js на фронте читает инпут, проверяет с помощью regex**
  + функция make password django шифрует на сервере ?
  + бэк ещё раз валидирует (проверяет пароль и почту, ...) **зачем два раза** 
* DFR использует сессии или JWT для аутентификации пользователей
  + эти данные доступны в middleware или обработчиках запросов
  + Session storage: Если включён SessionMiddleware, пользовательские данные будут сохраняться в сессиях и доступны в обработчиках.
  + JWT токен передаётся в заголовке авторизации (Authorization: Bearer <token>) и обрабатывается специальным классом аутентификации
   
### JAVASCRIPT
* js менять параметры html, class = стиль
* open chat
  + на странице login, profile, решгистрация нету
  + во время игры - статистика, другой user
  + приложение, отправлятть сообщение через js !!
  + django channel = framework для сокетов
* {% load static %}
  + загрузка стат файлов Django (table.css)
  + у нас кажется не будет этого


### DOCKER
* **docker volume ls** лишний том
* Чтобы файлы нового приложения, созданные внутри контейнера, появились на хосте
  + директория проекта внутри контейнера и на хосте связаны с помощью volumes (точек монтирования) `./backend:/app `
* `context ./backend`
  + файлы в `./backend` доступны для процесса сборки
  + только файлы COPY, ADD, ... скопируются в контейнер
  + `./backend` рабочая область команд в Dockerfile
* `WORKDIR /app`
  + используйте, даже если копируете все файлы в корень (`/`)
  + файлы будут копироваться `/app`, а не в `/`
  + все команды после этой строки будут выполняться в этой директории
    - без неё `CMD ["python", "/app/manage.py", "runserver", "0.0.0.0:8000"]`
* `volumes` is applied at runtime, does not affect the build stage
* ```dockerfile
  COPY requirements.txt .
  RUN pip install --no-cache-dir -r requirements.txt
  COPY . .
  ```
  + если файл `requirements.txt` не менялся, то `RUN pip install ...` будет  использовать кэш
    - Docker кэширует каждый слой (команду)
* ```dockerfile
  COPY . .
  RUN pip install --no-cache-dir -r requirements.txt
  ```
  + если `requirements.txt` не менялся, то любые изменения (python, HTML, CSS, документации) => повторня установка зависимостей
* **`PYTHONPATH`** переменная окружения - удалили
  + где Python ищет директории с модулями, кастомные библиотеки, пакеты при импорте
  + `PYTHONPATH` `export PYTHONPATH="/mnt/md0/42/14_ft_transendence/backend:$PYTHONPATH"`
* PYTHONUNBUFFERED нужна
* `COPY . .` + `volumes` ?
  + `COPY . .` включение файлов в образ на этапе сборки
  + `volumes` доступ к файлам хоста внутри контейнера
    - маскирует содержимое /app, которое было скопировано в образе
  + в разработке
    - frontend монтирует volume для статических файлов Frontend
    - используйте volume и не полагайтесь на файлы, скопированные в образе
    - `COPY . .` не нужно
    - `volumes` позволяет вносить изменения и видеть их в работающем контейнере
  + в производстве
    - backend (django + Daphne) Копирует все файлы приложения в образ Docker, собирает статические файлы и запускает Daphne
    - frrontend обслуживает статические файлы Frontend из `/static/` и статические файлы Backend из `/staticfiles/` через общий volume
    - не монтируйте volume, используйте только скопированные файлы в образе
    - монтируйте `static_backend` только для Статических Файлов 
    - `COPY . .` нужно
    - `volumes` Не требуется (код уже включен в образ с пломощью `COPY . .`)
* почему в производстве не монтировать код через volume и использовать скопированные файлы в образе Docker
  + в производственных средах придерживаются принципа **immutable infrastructure**
    - Образы являются неизменяемыми: После сборки образа он остаётся постоянным и не изменяется
    - Контейнеры запускаются на основе этих образов: Каждый запуск контейнера гарантирует, что он идентичен предыдущим
  + контейнеры, запущенные из одного образа, имеют одинаковый код, что упрощает отладку и снижает вероятность появления неожиданных ошибок
  + код внутри контейнера не зависит от файлов на хост-машине, что предотвращает несоответствия между средами разработки и производства
  + код, включённый в образ, защищён от несанкционированного доступа извне
  + меньше точек входа для потенциальных злоумышленников, так как код не доступен через volume
  + образы, содержащие весь код, самодостаточны и могут быть развернуты в любой среде без дополнительной настройки
  + Упрощённые пайплайны CI/CD: сборка, тестирование и развертываник более прямолинейны, так как весь код включён в образ
  + Docker может эффективно кэшировать слои, что ускоряет сборку и развертывание
  + монтирование volume может вносить дополнительные накладные расходы на файловую систему
  + облегчает управление версиями и откат к предыдущим стабильным версиям при необходимости
  + самодостаточные образы, не зависящие от внешних volume, развертываются быстрее и надёжнее, так как не требуют дополнительных настроек
* ./static — это папка на хосте, относительно того места, где находится Dockerfile.
* /usr/share/nginx/html/static — путь внутри контейнера
* volume staticfiles
  + Docker Volume
  + именованный том
  + могут быть использованы всеми контейнерами, которые подключены к этому тому
  + содержимое не связано с файловой системой хоста
  + volumes: - staticfiles:/app/staticfiles
    - Docker подключает том `staticfiles` к папке /app/staticfilesтом внутри контейнера
    - `/app/staticfiles` связан с томом `staticfiles
    - когда Django выполняет `collectstatic`, файлы помещаются в эту папку внутри контейнера, изменения синхронизируются с томом
  + volumes: - staticfiles:/usr/share/nginx/html/static
    - в контейнере frontend путь `/usr/share/nginx/html/static` также связан с тем же томом `staticfiles`
    - файлы, помещенные в том в одном контейнере, доступны для использования в другом 
  + `docker volume inspect staticfiles`
  + если вы хотите, чтобы том был доступен и через файловую систему хоста, вы можете использовать bind mount, а не именованный том
* Docker runtime files must be located in /goinfre or /sgoinfre (subject)
* You can’t use so called “bind-mount volumes” between the host and the container if non-root UIDs are used in the container (subject), but several fallbacks exist:
  + Docker in a VM
  + rebuild you container after your changes
  + craft **your own docker image** with root as unique UID
* virtual environment не нужно, потому что у нас докер


### ЗАПУСК
* **ASGI_APPLICATION = "myproject.asgi.application" установили, а зачем CMD ["daphne", "-b", "0.0.0.0", "-p", "8000", "myproject.asgi:application"]**
* For Ecole42 computers, I've updated settings of docker file in DEV branch 
  + порт, который нужен для django, занят
  + поменять номера портов в docker и в nginx
  + 6800  port for redis 
  + 4444 port frontend HTTP connections
  + 4443 port frontend for SSL connections over HTTPS
  + for local Ecole 42's network `ALLOWED_HOSTS = ['*']` in settings.py
* **Секретный ключ от api раз в две недели примерно обновляется**
* 'docker compose up --build'
  - все скачается и распакуется
  - потом в терминале можно Ctrl + C (условное клавиатурное прерывание)
  + пересобрать образы -> Django подхватывает изменения (если настроен **hot-reload**), фронтенд тоже
* `sudo ufw status` 80, 443, 8000 открыты
* через 42 на посте кластера, отличном от того, на котором он размещен
  + 1 способ: каждый раз менять URL-адрес перенаправления, чтобы он соответствовал IP-адресу хост-компьютера 
  + 2 саособ лучше:
    - каждый компьютер использует VM, в ней изменяете файл хостов, чтобы связать IP-адрес исходной станции с URL-адресами
    - подключиться к другому компьютеру в кластере через его внутреннее доменное имя школы, добавить это в nginx.conf, чтобы веб-сервер разрешал связываться с вами в этом домене, а также на стороне django
* from another computer (so local network is working) 
  + When Ivan tried to login with 42Auth from another computer (not server) - he got error 400; however basic sign up with email is working 
  + My login with 42Auth from server computer worked


### РАЗНОЕ
* CSR client-side rendering / SSR server-side rendering, данные загружаются на клиентскую сторону, HTML генерируется динамически с помощью JavaScript
  + SSR сервер генерирует и отправляет готовый HTML на клиентскую сторону, каждый запрос требует пересоздания всей страницы на сервере, может быть медленным, сервер должен выполнить обработку данных и сгенерировать страницу каждый раз, когда поступает запрос 
  + сервер отправляет данные (JSON, ...)
  + клиент с помощью js генерирует HTML на основе полученных данных
  + не требуется повторная генерация страниц на сервере для каждого запроса
  + it is almost front-end
  + if we do module - power-ups for AI are obligatory (so it is back end part): для модуля AI требуется бэкенд (сложная обработка данных, вычисления, доступ к серверным ресурсам)
* rendering
  + фронтенд: рендеринг HTML-шаблонов или динамически обновляемых данных через JavaScript, процесс генерации и отображения контента на веб-странице
  + Views: обработка запросов и отправка ответов в разных форматах (JSON, ...) через API
  + DRF: процесс обработки запросов и создания ответов в виде JSON, XML или других форматов
  + Django Channels: передача данных пользователю в реальном времени через протокол WebSocket
* the use of libraries or tools that provide an immediate and complete solution for a global feature or a module is rohibited (subject)
• the use of a small library or tool that solves a simple and unique task, representing a subcomponent of a global feature or module, is allowed (subject)

### Organisation
* https://github.com/bakyt92/14_ft_transendence
* https://docs.google.com/document/d/14zC4f2D8vdh9cYKosDQxsjWYc9aax2hPGuh8Y7CoENI/edit?tab=t.0
* https://docs.google.com/document/d/1O1r9jEdxISjMV29lZgLXWNh-bgPzSlnZ6Nr8QuyP_Jc/edit?pli=1
* F12 concole
  + лучше всего в chrome
  + colsole.log отладка
  + кнопка квадратик со стрелкой слево вверх - код элемента html
* прямо в консоли можно писать js и пробовать
  + гndetermined - что вернула функция
  + можно создать переменные (let)
    - они сохраняются в объекте window (window = браузер)
    - x или window.x досутп к этой переменной
  + api браузера - геолокация, звук
* Application - хранилище:
  + куки
  + local storage
  + ... storage
* the password for the django admin panel ...
* Б
  + структуры данных
  + DRF
  + модуль с сокетами и чатом (live chat, уведомления о турнирах, и проч)
  + эндпоинты и методы для API
  + chat
  + **rabbitMQ модуль сообщение от сервера**
  + ws
* Л
  + авторизация
  + фронт-энд
  + шаблон для фронт энда
  + profile
  + game page
  + auth issue
* Ан
  + накидать в Figma шаблоны страничек (часть есть в Миро) страницы: страница с логином, с самой игрой (пока без игры), профиль пользователя, страница с турниром
  + делать шаблончики  
  + структуры страничек
  + API с бэкэнда
* Амин
  + вебсокеты для модуля remote players: обмен информацией между игроками и сервером о локации ракетки и мяча
  + game logic
    - whether we want to follow basic ping pong rules?
    - the ball should speed up when paddle hits the ball ?
    - https://stackoverflow.com/questions/54796089/python-ping-pong-game-speeding-up-the-ball-after-paddle-hit 
  + some sort of **anticheat** to be sure that the users mouvement are normal
    - my code will be easy, it'd just gonne output two players position and the ball and then you can render it however you want
  + I'll update you soon on the game websocket
* basic requirements 20.01.2025 
  + All pong game part will be done by Amine? Do you need help with front-end (table, paddles, ball, some activity of JS or someone will do it?
  + Tournament, registration and matchmaking system by Alexey
  + Basic front-end will be done by Alexey? (profile page, other pages)
  + Security - probably we meet requirements by we need to validate input and follow some basic security rules on the front-end part.
* Modules (only that needs some response / comment)
  + User management - I did back-end (almost); but we need profile page with history of games, possibility to change profile data; see statistics of wins and loses - who will be responsible for this part? I can do it but I need template of javascript page (single page structure should be already applied) 
  + OAuth42 - almost finished by Alexey, please check that all requirements of this module are met/
  + Remote Players - will do Amine
  + LiveChat - is the proccess by me and Anna 
  + AI Opponent - will we do this module? Amine, can you implement it or you need help?
  + Game Customization Option - do we need this module? Who will do it? 
  + Multiple language supports - who will implement it? 
  + Server-side pong  - do we need this module? Is it implemented by Amine? For this module we need API for paddle, ball and other features
  + User and Game Stats Dashboards - do we need this module? Who will do it?
* Live pong game on website
  + users must have the ability to participate in a live Pong game against another player directly on the website
  + Both players will use the same keyboard
  + The Remote players module can enhance this functionality with remote players.
* Rules of Pong
  + All players must adhere to the same rules, which includes having identical paddle speed
  + This requirement also applies when using AI; the AI must exhibit the same speed as a regular player
* Tournament
  + A player must be able to play against another player
  + it should also be possible to propose a tournament
  + This tournament will consist of multiple players who can take turns playing against each other
  + it must display who is playing against whom and the order of the players
* A registration system
  + at the start of a tournament, each player must input their alias name
  + The aliases will be reset when a new tournament begins
  + this requirement can be modified using the Standard User Management module
* Matchmaking system for Tournament
  + the tournament system organize the matchmaking of the participants, and announce the next fight
* tutorials:
  + чат tuto https://channels.readthedocs.io/en/latest/index.html
  + бэк, фронт, база данных, API https://www.youtube.com/watch?v=XBu54nfzxAQ
  + REST API на DRF в Pycharm https://blog.jetbrains.com/pycharm/2023/09/building-apis-with-django-rest-framework/
  + https://developer.mozilla.org/en-US/docs/Web/API/WebSocket/WebSocket
  + https://docs.djangoproject.com/en/5.1/ref/contrib/auth/
  + Django Tutorial https://developer.mozilla.org/en-US/docs/Learn/Server-side/Django/Tutorial_local_library_website
  + https://www.djangoproject.com/start/
  + https://www.django-rest-framework.org/tutorial/quickstart/
  + django https://youtube.com/playlist?list=PLA0M1Bcd0w8xZA3Kl1fYmOH_MfLpiYMRs&si=mza4MvfFeRMqgU_B
  + django https://youtube.com/playlist?list=PLA0M1Bcd0w8yU5h2vwZ4LO7h1xt8COUXl&si=ddHMMDnBVPiUuEXy
  + The Browsable API - Django REST frameworkhttps://www.django-rest-framework.org/topics/browsable-api/
  + design https://www.figma.com/design/aWDJYfDmaeCv2NKJ8bJ15n/FT_Transcendence?node-id=109-32&p=f
  + // https://miro.com/app/board/uXjVLAphyh8=/
* **DO NOT FORGET**
  + remove убрать settings.py SECRET_KEY, frontend/etc/private.key
  + close ports 8000 and 6800 
  + `http://backend:8000` хранить в перменной окружения
  + We set DJANGO_SETTINGS_MODULE in the .env, docker-compose and Dockerfile. Then, we set it again: os.environ.setdefault('DJANGO_SETTINGS_MODULE', 'mysite.settings'). This appears to protect us from forgetting to set this variable in the .env, but it seems redundant in our case. May I remove os.environ.setdefault('DJANGO_SETTINGS_MODULE', 'mysite.settings')?
  + DEBUG = False
  + в продакшен отключают генерацию .map-файлов
  + GOOGLE_REDIRECT_URI=http://localhost:8000/auth/callback убрать
  + 42_REDIRECT_URI=http://127.0.0.1:8000/auth/callback убртаь
  + 0.0.0.0:8000 поменять
  + to justify your choices during the evaluation
