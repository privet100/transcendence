* **server side rendering is not a good idea**
  + it's just gonna take a lot of time
  + I have no idea how to do it
  + if we do it in the front it's easy
* game customization it's just gonna be front
  + like custom colors custom map
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
* docker-compose up --build
  + пересобрать образы -> Django подхватывает изменения (если настроен **hot-reload**), фронтенд тоже
* https://github.com/bakyt92/14_ft_transendence
* https://docs.google.com/document/d/14zC4f2D8vdh9cYKosDQxsjWYc9aax2hPGuh8Y7CoENI/edit?tab=t.0
* https://docs.google.com/document/d/1O1r9jEdxISjMV29lZgLXWNh-bgPzSlnZ6Nr8QuyP_Jc/edit?pli=1
* наш сайт
  + http://localhost:4444/ 
  + https://localhost:4443/
  + https://localhost:4443/chat
    - HTTP-запрос для загрузки веб-страницы чата (HTML, CSS, JavaScript)
  + ws://localhost:4443/ws/chat/<roomName>/
    - ws-запрос для работы реального времени внутри чата
    - Ws-запрос отправляется на URL /ws/chat/ (настроен в routing.py)
  + http://localhost:8000/admin
  + https://tr.naurzalinov.me/users/
  + http://95.217.129.132:8000/
* бэк, фронт, база данных, API https://www.youtube.com/watch?v=XBu54nfzxAQ
* REST API на DRF в Pycharm https://blog.jetbrains.com/pycharm/2023/09/building-apis-with-django-rest-framework/
* https://developer.mozilla.org/en-US/docs/Web/API/WebSocket/WebSocket
* https://docs.djangoproject.com/en/5.1/ref/contrib/auth/
* Django Tutorial https://developer.mozilla.org/en-US/docs/Learn/Server-side/Django/Tutorial_local_library_website
* https://www.djangoproject.com/start/
* https://www.django-rest-framework.org/tutorial/quickstart/
* django https://youtube.com/playlist?list=PLA0M1Bcd0w8xZA3Kl1fYmOH_MfLpiYMRs&si=mza4MvfFeRMqgU_B
* django https://youtube.com/playlist?list=PLA0M1Bcd0w8yU5h2vwZ4LO7h1xt8COUXl&si=ddHMMDnBVPiUuEXy
* The Browsable API - Django REST frameworkhttps://www.django-rest-framework.org/topics/browsable-api/
* design https://www.figma.com/design/aWDJYfDmaeCv2NKJ8bJ15n/FT_Transcendence?node-id=109-32&p=f
* // https://miro.com/app/board/uXjVLAphyh8=/

![1-1](https://github.com/user-attachments/assets/e6a157ea-b278-493e-9649-6a361614deac)

### протоколы, токены
* **файл сессии**
* **токен авторизации**
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
* CSRF (Cross-Site Request Forgery) token
  + токен **в settings.py**
  + чтобы никто не зашёл в БД
  + отдать клиенту при первом запросе
  + клиент вставлять токен в заголовок запроса к **api** и  при post/get запросах
    - **а при вебсокетах?**
  + views.py **набор проверок** добавить при запросе к api
  + https://docs.djangoproject.com/en/5.1/howto/csrf/
* JWT (JSON Web Token)
  + лучше **JWT**, тогда не будет необходимости в CSRF-токене
  + JWT в **серверной службе**
  + формат токена для передачи данных между сторонами
  + для аутентификации пользователей Веб-приложения, авторизации, обмена информацией
  + использует подпись, чтобы предотвратить изменения данных
  + все данные для проверки токена содержатся в нём, нет необходимости хранить их на сервере
  + nginx хранит jwt не в cookie, а в **локальном хранилище** (в переменной в js, обновляем как только он истечет)
  + безопасность доступа к API-ресурсам
  + Если токен украдён, злоумышленник может использовать его до истечения срока действия
    - Устанавливайте короткий срок действия токена (exp)
  + Отзыв токенов сложен, так как они самодостаточны (нет централизованного хранилища)
    - Реализуйте механизмы обновления и отзыва токенов
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
  + например пытаетесь открыть /admin, система проверяет, есть ли у вас права доступа

### frontend nginx server
* слушает и обрабатывает HTTPS-соединения  
* подписывается на **WebSocket-каналы**
* обработка WebSocket-запросов
* try using **bolt.new** it's better at frontend
  + the ui is fire here
* proxy_set_header передаёт заголовки (IP клиента, протоколь Host, X-Forwarded-For, ...) для Django
* Пользователь открывает https://tr.naurzalinov.me/
  + приходит запрос к http://95.217.129.132 на контейнер Nginx
  + Если через HTTPS, расшифровывает их с использованием SSL-сертификата
  + если запрос к статическому файлу, nginx отдает его из /usr/share/nginx/html/static/
  + **Фронтенд вызывает методы API** (для аутентификации, получения/отправки данных)
    - запросы на /api/ идут через Nginx на Django
  + JSON, HTML, WebSocket проходят через прокси на 8000 через **HTTP (внутренний трафик не шифруется)**
  + views.py **проверяет токен/подпись**
* проверяет сертификаты SSL
* nginx.conf = default.conf внутри контейнера, переопределяет стандартные настройки Nginx
* прослушивает HTTP-запросы на 80 и перенаправляет на HTTPS с использованием кода 301 (постоянное перенаправление)
* четыре server{}-блока = один процесс
* nginx.conf рядом с Dockerfile, чтобы во время сборки копировался внутрь контейнера
* you should keep separate server blocks, if you are encountering errors after merging the two server blocks
  + server_name cannot match raw IP addresses: The server_name directive is designed for matching domain names
  NGINX does not use server_name for requests with an IP address
  + Conflicting rules for IP-based requests: When a request is made to http://95.217.129.132, NGINX does not use the server_name directive for matching. Instead, it matches the listen directive and uses the first server block that matches the port
  + By merging the two server_name values, requests to http://95.217.129.132 may not behave as intended because NGINX will not consider the IP address part of server_name
* работает через HTTP/WS-протоколы
* использует эндпоинты Django 
* Bootstrap toolkit
* assessibility
* multiple language
* `proxy_set_header ...` передает заголовки (Host, IP-адрес клиента, протокол, ...), чтобы **бэкенд понимал, откуда пришел запрос**
* Nginx не нужноlocation /wss/
  + wss:// — зашифрованный WebSocket, работает через тот же путь, что и ws://, используя тот же location-блок
  + Протоколы ws и wss используют один и тот же маршрут
  + В Nginx нет разделения маршрутов между ws и wss
  + если вы уже настроили location /ws/, он будет работать и для wss://
    - если правильно настроен HTTPS.
  + если соединение идёт через HTTPS, Nginx управляет SSL и передаёт расшифрованный запрос вашему бэкенду
  + клиент отправляет запрос на wss://localhost/ws/ -> nginx проксирует запрос на бэкенд, **сохраняя заголовки WebSocket**
  + location /wss/ понадобится, только если вы хотите явно выделить другой маршрут для зашифрованных WebSocket-запросов, если вы хотите отделить шифрованные соединения (например, для разных приложений)
* Vanilla JS (без фреймворков)
  + фронт самописный или с использованием минимальных библиотек (jQuery, Bootstrap, ...)
  + не на SPA-фреймворке

### backend Daphne (ASGI сервер) 
* обработка запросов и передача их в Django Framework для выполнения бизнес-логики
* управляет обработкой логики приложения
* Управление синхронными HTTP-запросами
* управление асинхронными WebSocket-соединениями
* слушает внутри контейнера на порту 8000 в сети Docker
* Django через ASGI
* папка `backend/`: код Django, настройки, requirements, миграции, ...
* обработка **API-запросов** через Django REST Framework (DRF)
* валидация CSRF-токенов для защиты от атак
* ваимодействие с базами данных и другими внешними сервисами (например, API École 42 для авторизации)
* game logic in backed, backend because we need it to do the multiplayer
* runserver 0.0.0.0:8000 => запустили Django-приложение
* обрабатывает запросы фронтенда: аутентификация, чат (через Channels), API для фронтенда, авторизация, API, игровая логика, аутентификация
* возвращает ответы
  + рендеринг **шаблонов** (Server-Side Rendering)
  + JSON  (**REST API**)
    - **JSON API** (используя **Django REST Framework** или WebSocket-соединения через Channels)
  + html
  + websocket (Channels)
* обращается к Redis для кэша, сессий, Pub/Sub (чат, обновления статусов игроков в реальном времени)
* Django Channels обрабатывает событийную логику для чатов/онлайн-игр
* myproject/
  + конфигурация проекта Django, настройки, корневой маршрутизатор, запуск, управление проектом
  + `asgi.py` точка входа Django для запуска приложения
  + `__init__.py` делает папку пакетом Python
* `settings.py` конфиг Django на высоком уровне
  + `BASE_DIR` корневая папка проекта  
  + `SECRET_KEY` 
  + `ALLOWED_HOSTS` список доменов/IP, с которых разрешён доступ к этому Django-приложению (когда `DEBUG=False`)
  + `INSTALLED_APPS` список приложений: стандартные (admin, auth, sessions), кастомные: `myapp`, `chat`, `api_42`, ...
  + `ROOT_URLCONF` главный файл роутов `myproject/urls.py`, роутинг для всего проекта
  + `TEMPLATES` настройки для системы шаблонов Django (**HTML-шаблоны**, какие контекстные процессоры использовать)
  + `REST_FRAMEWORK` **рендеры**, по умолчанию JSON и **Browsable API**
  + `AUTH_PASSWORD_VALIDATORS` валидаторы сложности паролейл (минимальная длина, отсутствие совпадений с логином, ...)
* локальная backend примонтирована в контейнер как /app: изменения в коде сразу видны внутри контейнера
* manage.py django-утилита для **миграций**, запуска сервера, создания **суперпользователя**, ...
* requirements.txt: **Python**-библиотеки для бэкенда
* трекинг **когда пользователь был онлайн**
  + models.py: last online для индикатива
  + три решения 
    - через библиотеку channels - по вебсокетам следим пользователь онлайн или нет - НАМ ЭТОТ СПОСОБ
    - через Django sessions - как только юзер делает какое либо действие, Джанго сохраняет в базу данных дату этого действия 
    - через redis - но не понял как это работает
* `CMD ["python", "manage.py", "runserver", "0.0.0.0:8000"]`
  + запускает **встроенный сервер разработки Django**
    - **не поддерживает ASGI или WebSocket**
    - для разработки и тестирования, не для продакшена
    - не сможет обрабатывать WebSocket-соединения или другие асинхронные протоколы
    - не рассчитан на высокую нагрузку
  + не запускает Daphne (в продакшн требуется Daphne)
* REST FRAMEWORK
  + PUT поменять поле в бд
    - без него запрос sql
  + http://127.0.0.1:8000/user/1 отладка
  + http://127.0.0.1:8000/users
  + http://127.0.0.1:8000/tour
  + к нему обращается js:
    - нопка "создать турнир"
    - вызывает 127.0.0.1/tour
    - запускает rest framework
* django channel создает websocket, слушает запросы
  + отличия от html: непрерывное соединение, стрим
  + new Websocket(127.0.0.1:8000/game, wss)
* `asgi.py`
  + DJANGO_SETTINGS_MODULE = mysite.settings **настройки** для компонент Django (**ORM**, middleware, ...)
  + `django_asgi_app = get_asgi_application()` создаёт объект ASGI-приложения, exposes the ASGI callable as a **module-level variable**
    - Initialize Django ASGI application: загрузка приложений из `INSTALLED_APPS`, конфигурация бд, **среда `AppRegistry`**
    - to ensure the AppRegistry is populated before importing ORM models
    - приложение Django инициализирется до использования **ORM**-моделей или других компонентов Django
    - приложения готовы к работе _до_ конфигурации маршрутов WebSocket
    - если снчала импортировать модели или др компоненты Django, будут ошибки, связанные с незарегистрированными приложениями или моделями
  + URLRouter, ProtocolTypeRouter, маршрутизатор/обработчик ASGI-приложения: протокол (HTTP, WebSocket) для обработки запроса
    - "http": django_asgi_app обрабатывает (обычный обработчик Django, **встроенный?**)
    - "websocket": AllowedHostsOriginValidator(AuthMiddlewareStack(URLRouter(websocket_urlpatterns))) обрабатывает, `websocket_urlpatterns` из `chat/routing.py`: перенаправлять на `ChatConsumer`, обрабатывается через Django Channels
* **среда `AppRegistry`** управляет регистрацией приложений и моделей
* **Django Debug Toolbar**: интерфейс для отслеживания различных аспектов работы проекта, включая middleware
* **включить HTTP/2 ?**
 
### django в целом
* Бэкенд-фреймворк
* ключевые механики (аутентификация, управление базой данных, админка, API), функции и практики из коробки
* диктует архитектуру (приложения, модели, views, urls, ...)
* `models.py` структура данных, связи (пользователи, профили, чаты, сообщения, статистика игры, ...)
* `url.py` какие функции обрабатывают какие пути, какие запросы обрабатывает по адресу endpoint
* `views.py` функции  для обработки endpoint, запросов
* `apps.py` (регистрация приложения в Django)
* **`migrations/`** (миграции для моделей)
* `tests.py`
* Управление пользователями и аутентификацией
  + встроенная система аутентификации и модель пользователей
  + Регистрацию и авторизацию (в том числе OAuth/SSO)
  + Разграничение доступа к страницам (приватные/публичные чаты, комнаты, ...)
  + Проверку токена/сессии при каждом запросе с фронтенда
* Администрирование
  + позволяет набросать интерфейс для управления данными (пользователи, чаты, статистика, ...)
  + /admin встроено в django
* Безопасность и валидация
  + инструменты по защите от распространённых уязвимостей (**CSRF, XSS, SQL Injection**)
  + благодаря джанговским формам и сериализаторам + Django REST Framework, упрощается валидация данных, приходящих от фронтенда
* на базе Python
* список команд django
  + `docker exec -it 423c6474989f python manage.py help` 
  + [auth]: changepassword createsuperuser
  + [contenttypes] remove_stale_contenttypes
  + [daphne] runserver
  + [django] check compilemessages createcachetable dbshell diffsettings dumpdata flush inspectdb loaddata makemessages makemigrations   migrate optimizemigration sendtestemail shell showmigrations sqlflush sqlmigrate sqlsequencereset squashmigrations startapp startproject test testserver
  + [rest_framework] **generateschema**
  + [sessions] clearsessions
  + [staticfiles] collectstatic findstatic
* DPR, Data Processing and Rendering (Обработка и отображение данных) - данные (JSON, ...) обрабатываются на серверной стороне с помощью Django и затем передаются в представление для рендеринга на клиентской стороне с использованием шаблонов и JavaScript

### приложения Django app - отдельные модульные приложения внутри проекта INSTALLED_APPS
* myapp
  + логика пользовательских профилей, турниров, историй игр
  + модель `UserProfile`: инфо о пользователе, друзьях, аватаре, дате последней активности, ...
  + модель `Tournament`: список участников (many-to-many к `UserProfile`)
  + модель `Game` история матчей, кто участвовал, какой турнир, счёт, победитель
  + `friends = models.ManyToManyField("self")` пользователи могут быть друзьями 
  + расширить профили (рейтинг, биографию, статистику), турнирную логику (сетка турнира, раунды), механику игр:
     - добавлять поля
     - добавтиь **миграции** в соответствующие модели
    + **API** для работы с сообщениями
* 'django.contrib.messages'
  + приложение, предоставляющее API для работы с flash-сообщениями
  + для временных сообщений
    - сообщения удаляются после отображения
  + **flash messages**
  + сообщения пользователю **между запросами** (об регистрации, ошибка при неверном вводе данных, ...)
  + хранит сообщения временно (в сессии или cookies)
    - бэкэнд для хранения сообщений `MESSAGE_STORAGE = 'django.contrib.messages.storage.cookie.CookieStorage'`
  + уровни важности сообщений: `messages.debug`, `messages.info`, `messages.success`, `messages.warning`, `messages.error`
  + обмен данными происходит через WebSocket соединения  
    - бывает и через HTTP-запросы, добавляя сообщения во время перенаправлений или в шаблоны
  + в большинстве случаев подключается и `MessageMiddleware`
    - `MessageMiddleware` автоматически сохраняет и извлекает сообщения из хранилища (cookie, сессии, ...)
    - сообщения сохраняются между запросами и автоматически удаляются после первого отображения
    - без `MessageMiddleware` только для сообщений **внутри одного запроса**, сообщения не сохраняются между запросами
    - `MessageMiddleware` без `django.contrib.messages` невозможно использовать
* 'django.contrib.admin',
* 'django.contrib.auth',
* 'django.contrib.contenttypes',
* 'django.contrib.sessions',
* 'django.contrib.staticfiles'
* Используем **стандартные структуры юзера для авторизации и для моделей данных**
* **какие есть встроенные**

### API (Application Programming Interface)
* интерфейс или набор правил, позволяет приложениям взаимодействовать друг с другом
* группа маршрутов (эндпоинтов), которые клиент может использовать для взаимодействия с сервером
* определяются в `urls.py`
* определяются в View-классах или функциях
  + классы, которые наследуют от `APIView`, `GenericViewSet`, `ViewSet`
* `routing.py` маршруты WebSocket
  + Каждый WebSocket-путь можно считать отдельным API
  + WebSocket - часть API
* могут быть
  + стандартными REST (GET, POST, PUT, DELETE)
  + асинхронными WebSocket (через Django Channels)
* несколько эндпоинтов реализуют функциональность API
  + приложение `api_42`
  + приложение `chat`
  + приложение `auth_app`, для каждого из них 5–10 эндпоинтов
  + для аутентификации/авторизации (École 42, ...)
  + для работы с чатом (отправка, получение сообщений)
  + для обработки пользовательских данных (профиль, друзья, статистика)
  + для игры (управление матчами, таблицы лидеров)
  + для управления пользователями 
    - маршрут `GET /users/` список пользователей
    - маршрут `POST /users/` создать нового пользователя
    - маршрут `GET /users/<id>/`
    - маршрут `DELETE /users/<id>/`
    - маршрут API чата = группа всех маршрутов, связанных с чатом
* **создаются с помощью Django REST Framework (DRF)**
* список API http://localhost:8000/swagger/, http://localhost:8000/redoc/
  + если настроена автоматическая документация (Swagger, Redoc)  

### Endpoint 
* конкретный маршрут, связанный с API
* выполняет определённое действие
* Endpoints чата:
  + `GET /chat/rooms/` — список комнат
  + `POST /chat/rooms/` — создать комнату
  + `GET /chat/rooms/<room_id>/messages/` — получить сообщения из комнаты
  + `POST /chat/rooms/<room_id>/messages/` — отправить сообщение
* endpoints:
  + Откройте каждый `urls.py`
  + callback/
    logout/
    login/
    auth/email/
    signup/
    auth/callback
    profile/
    ...
* команда Django `python manage.py show_urls` список эндпоинтов
  ```
  /       
  /admin/ 
  /admin/<app_label>/
  /admin/<url>    
  /admin/auth/group/
  /admin/auth/group/<path:object_id>/
  /admin/auth/group/<path:object_id>/change/
  /admin/auth/group/<path:object_id>/delete/
  /admin/auth/group/<path:object_id>/history/
  /admin/auth/group/add/
  /admin/autocomplete/
  /admin/jsi18n/
  /admin/login/
  /admin/logout/
  /admin/myapp/game/
  /admin/myapp/game/<path:object_id>/
  /admin/myapp/game/<path:object_id>/change/
  /admin/myapp/game/<path:object_id>/delete/
  /admin/myapp/game/<path:object_id>/history/
  /admin/myapp/game/add/
  /admin/myapp/tournament/
  /admin/myapp/tournament/<path:object_id>/
  /admin/myapp/tournament/<path:object_id>/change/
  /admin/myapp/tournament/<path:object_id>/delete/
  /admin/myapp/tournament/<path:object_id>/history/
  /admin/myapp/tournament/add/
  /admin/myapp/userprofile/
  /admin/myapp/userprofile/<path:object_id>/
  /admin/myapp/userprofile/<path:object_id>/change/
  /admin/myapp/userprofile/<path:object_id>/delete/
  /admin/myapp/userprofile/<path:object_id>/history/
  /admin/myapp/userprofile/add/
  /admin/password_change/
  /admin/password_change/done/
  /admin/r/<int:content_type_id>/<path:object_id>/
  /auth/auth/callback
  /auth/auth/email/
  /auth/callback
  /auth/callback/ 
  /auth/email/
  /auth/login/
  /auth/logout/
  /auth/profile/
  /auth/signup/
  /callback/
  /chat/
  /chat/<str:room_name>/
  /game/<int:id>/
  /login/
  /logout/
  /profile/
  /signup/
  /tour/<int:id>/
  /user/<int:id>/
  /user_42/<int:user_id>/
  /users/
  /users_42/
  ```
* `grep -r "path(" backend/`, `grep -r "re_path(" backend/`
  ```
  login/
  callback/
  logout/
  login/
  auth/email
  signup/
  auth/callback
  profile/
  ''
  ""
  ws/chat/(?P<room_name>\w+)/$"
  <str:room_name>/
  admin/
  auth/
  chat/
  user_42/<int:user_id>/
  users_42/
  users/
  user/<int:id> /
  tour/<int:id>/
  game/<int:id>/
  r"^api-auth/"
  r"ws/chat/(?P<room_name>\w+)/$
  ```
* Using the URLconf defined in myproject.urls, Django tried these URL patterns, in this order:
  ```
  admin/
  auth/
  chat/
  [name='index']
  callback/ [name='callback']
  logout/ [name='logout_view']
  login/ [name='loginemail']
  auth/email/ [name='authemail']
  signup/ [name='signup']
  auth/callback [name='oauth_callback']
  profile/ [name='profile']
  user_42/<int:user_id>/ [name='get_user_42']
  users_42/ [name='get_all_users_42']
  users/ [name='get_all_userprofiles']
  user/<int:id>/ [name='user-detail']
  tour/<int:id>/ [name='tournament-detail']
  game/<int:id>/ [name='game-detail']
  You’re seeing this error because you have DEBUG = True in your Django settings file. Change that to False, and Django will display a standard 404 page.
  ```

### django app chat
* приложение с реальным временем
* на WebSocket
* tuto https://channels.readthedocs.io/en/latest/index.html
* js обращается к rest api (post) endpoints /history, /users/, /send
* rest api строит и отдаёт html  
* js получает ответ (get)
* логика в views.py
* с каждым пользователем у бэкенда 2 вебсовета: чат, положение ракетки
* **история в бд**
* ws login new WS connexion
* ws system msg
* ws user communications
* INSTALLED_APPS 'channels'
* CHANNEL_LAYERS 'BACKEND': 'channels.layers.InMemoryChannelLayer', **in-memory ?**
* `asgi.py`
* `chat/models.py` модели сообщений и комнат
* `chat/consumers.py` WebSocket consumer 
* `chat/routing.py маршрут
* `chat/urls.py` маршрут для комнаты чата
* `chat/views.py представление
* `python manage.py makemigrations`, `python manage.py migrate` создайте и примените миграции для моделей 
* Django Channels
  + библиотека, надстройка Django для вебсокетов, добавить поддержку протокола WebSocket
  + полагается на стандартную инициализацию Django для HTTP-запросов
  + использует дополнительную логику для обработки WebSocket-соединений и других типов протоколов
  + для приложений с функциями реального времени, выходящих за рамки стандартного HTTP-протокола: онлайн-игра, онлайн-статус пользователей
  + WebSocket - постоянное соединение между клиентом и сервером
  + WebSocket - передавать данные в режиме реального времени
  + подходит для чата
* RabbitMQ брокер сообщений для системных сообщений (уведомление о начале турнира, уведомление о добавлении в друзья), не нужен
* класс router обрабатывает перемещения по сайту
  + popstate кнопки назад вперёд в брузере
* на место .app подставляется div
  + div = homapage, profile, ... (все они наследуют от Component)
  + constructor() создаёт класс
  + class Component базовый, абстрактный - **понять**
  + render = вернуть html
  + state = переменные 
  + событие DomContactLoaded = html полностью загрузился (у нас только 1 раз)
  + фиксированная часть страницы доступна в js
  + % body % кберем, т.к. у нас SPA
  + login pages.js - запрос post к бэку
    - в заголовке запроса каждый раз CSRF токен, чтобы знать, что это не юзер с третьего сайта
  + **чат надо сделать компонентом**
  + fetch (в js) = запрос к бэку
  + функция render своя в каждом компоненте
    - например нужен username - делаем запрос к бэку fetchuserprofile
* websocket объект js
* send.msg
* receive = render msg
* websocket: кому пишем сообщение? в запросе
* prepMsg забирает инпут и делает ws запрос
  + взять список пользователей из базы
* @login_required только если залогинен, если нет токена или он неправильный - не пропустит
* fetch() запрос обычный (не websocket)
* websocket один для всех пользователей для чата
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
* incognito
* в django новая функция: python manage.py statup, migrations, INSTALLED_APS ... 'chat'
* 1 эндпоинт слушает чат
  + в заголовке запроса - кому сообщение
* если сообщение не дошло, то такого пользователя нет
* в контейнере бэк: python manage.py superuser то ли python manage.py createsuperuser, с именем админ пародлем админ
  + после этого migrations
* pathname = часть адреса после корня
* login = new ws connexion
* ws system msgs
* ws user communications
* можно ли делать live chat с библиотекой channels или надо целиком писать? Какие библиотеки можно использовать?
* базовый функционал чата
  + `daphne` настроен для обработки вебсокетов, `asgi.py`, routing, WebSocket-соединения перенаправляюся в обработчики
    ```python
    application = ProtocolTypeRouter({
        "http": get_asgi_application(),
        "websocket": AuthMiddlewareStack(
            URLRouter(
                chat.routing.websocket_urlpatterns
            )
        ),
    })
    ```
  + `ChatConsumer` для обработки сообщений:
  + Настройте Redis как слой для канального взаимодействия: `CHANNEL_LAYERS` в `settings.py` указывает на Redis
  + фронтенд, соединение с WebSocket-сервером
    ```javascript
    const chatSocket = new WebSocket(
        'ws://' + window.location.host + '/ws/chat/room_name/'
    );
    chatSocket.onmessage = function(e) {
        const data = JSON.parse(e.data);
        document.querySelector('#chat-log').value += (data.message + '\n');
    };
    chatSocket.onclose = function(e) {
        console.error('Chat socket closed unexpectedly');
    };
    document.querySelector('#chat-message-submit').onclick = function(e) {
        const messageInputDom = document.querySelector('#chat-message-input');
        const message = messageInputDom.value;
        chatSocket.send(JSON.stringify({
            'message': message
        }));
        messageInputDom.value = '';
    };
    ```
  + Обновите шаблоны HTML
      ```html
      <textarea id="chat-log" cols="100" rows="20" readonly></textarea><br>
      <input id="chat-message-input" type="text" size="100"><br>
      <button id="chat-message-submit">Send</button>
      ```
  + База данных: модель для хранения сообщений
    ```python
    class Message(models.Model):
        user = models.ForeignKey(User, on_delete=models.CASCADE)
        room = models.CharField(max_length=255)
        content = models.TextField()
        timestamp = models.DateTimeField(auto_now_add=True)
    ```
  + сохранение сообщений в `ChatConsumer` при получении данных
    ```python
    from .models import Message
    async def receive(self, text_data):
        text_data_json = json.loads(text_data)
        message = text_data_json['message']
        user = self.scope['user']
        await database_sync_to_async(Message.objects.create)(         # Save to database
            user=user, room=self.room_name, content=message
        )
        await self.channel_layer.group_send(
            self.room_group_name,
            {
                'type': 'chat_message',
                'message': message,
            }
        )
    ```
  + Проверьте соединения через WebSocket (например, с помощью браузера)
  + Убедитесь, что Redis работает корректно и сообщения передаются через каналы
  + Проверьте сохранение сообщений в базе данных
  + Развертывание:
- WebSocket-роутинг в nginx:
    ```nginx
    location /ws/ {
        proxy_pass http://backend:8000;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection "upgrade";
    }
    ```

### F12 concole
* лучше всего в chrome
* colsole.log для отладки
* кнопка квадратик со стрелкой слево вверх - смотреть код элемента html
* js менять параметры html, class = стиль
* open chat
  + на странице login, profile, решгистрация нету
  + во время игры - статистика, другой user
  + приложение, отправлятть сообщение через js !!
  + django channel = **fr.w.** для сокетов
* прямо в консоли можно писать js и пробовать
  + ndetermined - то, что ыернула функция
  + можно создать переменные (let)
    - они сохраняются в обхекте window (window = браузер)
    - x или window.x досутп к этой переменной
  + api браузера - геолокация, звук, 
* Application - хранилище:
  + куки
  + local storage
  + ... storage
* Netrwork
  + запросы от фронта
  + document = html

### autorisation
* При каждом запросе клиент отправляет токен в заголовке Authorization: Bearer <token>
* Сервер декодирует токен, проверяет его подпись, использует данные для авторизации (например, проверяет роль пользователя)
* виды авторизаций: Ecole 42, гугл, логин и пароль с базы данных
* задача на бэкэнде
* внедрить 2FA через электронную почту
  + Мы выполнили первую часть, генерируя код и все такое
  + были проблемы с последней частью, где мы на самом деле отправляем электронное письмо
    - попробовали sendgrid life, было сложно добиться успеха в отправке электронных писем
  + мы использовали Gmail, самое сложное — найти путь через пользовательский интерфейс Google для создания «mdp приложения» на стороне Django, все легко делается с помощью функции send_mail
    - сначала пришло как спам, а потом появилось само по себе после нескольких писем
    - со стороны Django это происходит, Gmail изменил условия две недели назад, с тех пор это стало проблемой

### asgi, wsgi
* интерфейс между веб-сервером и веб-приложениями/фреймворками, используемый Django-приложениями
* стандарт для взаимодействия между веб-серверами и Python-приложениями
  + в Python фреймворки, тулкиты, библиоти, для каждого собственный метод установки и настройки, не умеют взаимодействовать между собой
* WSGI/ASGI-сервер передаёт данные о запросе Python-приложению - Python-приложение обрабатывает запрос и возвращает ответ
* WSGI, Web Server Gateway Interface
  + поддерживает только синхронные запросы HTTP
  + не подходит для реального времени или протокола WebSocket
  + нужен в некоторых случаях
    - некоторые инструменты, среды, библиотеки, middleware поддерживают только синхронные HTTP-запросы
    - Django-приложений без асинхронных функций с WSGI стабильнее
* ASGI, Asynchronous Server Gateway Interface
  + продолжение WSGI, когда есть асинхронные задачи (в реальном времени - чат, уведомления, игры)
  + Daphne хорошо интегрируется с Django Channels и поддерживает WebSocket из коробки
  + ASGI-сервер передаёт синхронные HTTP-запросы стандартным Django-приложением
  + ASGI-сервер обрабатывает асинхронные-запросы (WebSocket) через Django Channels
    - Django работает с библиотекой Django Channels, чтобы обрабатывать WebSocket
  + ASGI-приложение объединяет Django и Channels
* `ASGI_APPLICATION = "myproject.asgi.application"`: запускаете приложение с помощью ASGI-сервера
  + переменная `application` = ASGI-приложение, обрабатывает входящие запросы
  + Переменная `application` = точка входа для ASGI-сервера
  + ASGI-сервер загружает модуль и использует `application` для обработки запросов
  + сервер ищет  в `settings.py модуль `myproject.asgi` и внутри него переменную `application` 
  + не используется в коде
  + AllowedHostsOriginValidator проверяет допустимые хосты для WebSocket-соединений
    - ALLOWED_HOSTS включает все домены и IP-адреса, которые вы используете
* перехожу на asgi only:
  + стандартные Django-функции (middleware, views, models) продолжают работать, не меняет Django-кода для HTTP
  + удалила файл wsgi.py
  + удалила строку `Gunicorn_APPLICATION = 'myproject.wsgi.application'`
  + nginx location /ws/
  + Тестирование
    - HTTP-запросы (HTML-страница, отправка данных формы)
    - функционал, связанный с WebSocket (чат, ...)
    - клиент подключаетсся к ws:// или wss://

### db (PostgreSQL)
* СУБД (база данных) для хранения пользователей, сообщений, данных о матчах в Pong, статистики и т.д.
* Создаёт базу данных mydatabase с логином/паролем myuser / mypassword
* том db_data, чтобы не терять БД при перезапуске
* доступен внутри сети Docker по адресу db:5432

### redis (Remote Dictionary Server) 
* инструмент управления данными
* репликация и кластеризация для масштабирования и высокой доступности
* управление пользовательскими сессиями в веб-приложениях, хранение данных сессий
* работает в оперативной памяти => высокая скорость
  + важно следить за потреблением памяти
* быструю запись и чтение
* хранение данных о текущих авторизованных пользователях. Управление сессиями при подключении пользователей к WebSocket
* хранение сессий (если Django настроен)
* механизм списков или Pub/Sub для реализации очередей сообщений
* Быстрый подсчёт статистики, временных данных
* Реализация чата (Совместно с WebSocket для быстрой обработки сообщений. В **Pub/Sub** позволяет рассылать сообщения всем пользователям чата.)
* очередь задач (отправки email, push-уведомления, логирование, обработка фоновых задач (обновление статистики матчей))
* для матчей в реальном времени (текущее состояние игры, обмен данными между игроками,хранение лидербордов с помощью упорядоченных множеств (sorted sets))
  + база данных
  + брокер сообщений, быстрый обмен сообщениями
  + хранение Pub/Sub (для реального времени)
    - Pub/Sub для чата
    - Pub/Sub (механизм публикации/подписки) для обмена сообщениями
    - Pub/Sub в реальном времени (чат, игра Pong)
    - бекенд для Pub/Sub и WebSocket-сессий (чат, игра в реальном времени, ...)
  + Когда клиент отправляет сообщение через WebSocket, **оно сначала сохраняется в Redis**, после чего перенаправляется другим клиентам или серверу в зависимости от логики
  + канальный слой, для поддержки системы каналов через Django Channels
  + реализация реального времени через Django Channels (WebSocket)
  + Коммуникации между разными инстансами приложения
  + инструмент маршрутизации между подключёнными клиентами
  + Обработка событий WebSocket
  + Django Channels позволяет приложению обрабатывать протоколы реального времени (WebSocket, ...)
* Библиотеки абстрагируют низкоуровневую работу с Redis, скрывают сложность интеграции
  + `channels_redis` подключением и отправкой сообщений в Redis
  + `django-redis` сериализацию данных, запись, извлечение
  + легко интегрируется с Django через библиотеки `channels_redis` и `django-redis`
* хранение и маршрутизация данных
* **in-memory key-value store**
* значения могут быть: string, list, hash, set, sorted sets, bitmap, HyperLogLogs, геопространственные индексы и др  
* поддерживает множество языков программирования
* Транзакции и скрипты на языке Lua
* может сохранять данные на **диск**, чтобы избежать потери при сбоях
* пддержка Pub/Sub (публикации/подписки) => идеальный для каналов WebSocket
* Убедитесь, что сервер Redis защищён (настройка пароля, ограничение доступа, ...)
* Настройте Redis для работы с высоким количеством соединений
* настройки: время жизни кэша (`TIMEOUT`), ...
* Убедитесь, что Redis не перегружен `redis-cli info memory`
* очистить кэш `redis-cli -n 1 flushdb`
* один сервер Redis = кэширование + обслуживание channel layers
  + два процесса на одном сервере Redis, с разными ключами и настройками
* если WebSocket-запросы зависят от Redis (хранение данных о сессиях, ...), убедитесь, что Redis работает: `redis-cli ping`, Ожидаемый ответ: `PONG`
* логи Daphne или Redis - посмотреть соединения WebSocket создаются и работают стабильно?
* Redis WARNING Memory overcommit must be enabled!
  + `sudo sysctl vm.overcommit_memory=1` настройка системы, чтобы Redis мог эффективно сохранять данные в условиях низкой памяти
  + `vm.overcommit_memory = 1` в `/etc/sysctl.conf` сохранить настройку после перезагрузки
* используется механизм Pub/Sub (Publish/Subscribe) для реализации чата?
  + Поиск в коде `PubSub`, `publish`, `subscribe`, `event`, `broker`, `topic`
    - библиотеки  `Redis`, `RabbitMQ`, `Kafka`, WebSocket-библиотеки с поддержкой Pub/Sub
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

#### Redis для кэширования
* Кэширование использует Redis как хранилище данных в памяти с быстрым доступом
* Django кэширует данные, которые часто запрашиваются, но редко изменяются
  + временне данные для ускорения работы приложений (имя, аватар, статус
  + результатов сложных SQL-запросов
  + рендеринг HTML-шаблонов, чтобы повторные запросы быстрее обрабатывались
  + результаты REST API-запросов, чтобы не нагружать базу данных повторными запросами
  + API-ответов (данных профиля пользователя, данных статистики)
  + сеансы: **хранилище** сеансов для авторизованных пользователей
  + пользовательские сессии, что **ускоряет работу сессий в распределённых системах**
  + временный статус пользователя: **онлайн/офлайн**
  + результаты частых запросов (сложные вычисления, данные из базы)
* по умолчанию django кэширует в памяти `django.core.cache.backends.locmem.LocMemCache` 
  + кэш в процессе работы сервера
  + если приложение работает в пределах одного сервера и не требует масштабируемости
  + нет сильной нагрузки или большого объёма данных
  + не требует настройки и внешнего сервиса
  + доступ к данным в памяти быстрее
  + не нужно хранить кэш между перезапусками
* у нас Redis для кэширования
  + `CACHES`: `django_redis` = backend для кэширования
    - она берёт на себя работу с Redis, предоставляя **Django-friendly API**
    - **класс клиента определяет, как Django взаимодействует с Redis**
    - `OPTIONS` = `CLIENT_CLASS` настраивает использование `DefaultClient` из библиотеки `django-redis` для взаимодействия с Redis
  + если приложение масштабируется
  + если проект в распределённой среде, в многосерверных или контейнеризованных средах, несколько серверов обращаются к одному кэшированию, данные в процессе/сервисе (Redis-сервере), а не в памяти отдельного экземпляра приложения
  + несколько инстансов приложения могут одновременно использовать Redis как общий слой
  + сохранять кэшированные данные на диск (persistent storage), данные могут быть восстановлены после перезапуска или сбоя
  + кэшированные данные переживают перезапуск приложения или контейнера (например, для сессий)
  + сложные типы данных в кэше (списки, множества, хеши)
  + сложные стратегии кэширования: TTL (время жизни), евикция (удаление старых или неиспользуемых данных)
  + ограничение количества запросов от одного пользователя за определённое время (rate limiting)
  + если Redis для channel layers, то чтобы централизовать кэшированные данные и улучшить производительность
* bakyt: только кэш сообщений, чат и **системные**
  
#### у нас redis для `CHANNEL_LAYERS`
* Channel Layers использует Redis для обмена сообщениями между экземплярами Django через Django Channels, обеспечивая синхронизацию сообщений между ними
* если используете Django Channels, Redis выступает как **канал-сервер**
* встроенные механизмы кэширования Django без Redis
  + Простое в настройке
  + Ограничено по производительности и функциональности
  + Подходит для небольших и локальных проектов
  + 1 типа: Локальное кэширование в памяти (`LocMemCache`)
    - кэш в оперативной памяти локального сервера
    - для небольших проектов
    - данные не разделяются между несколькими процессами или серверами
    - кэш очищается при перезапуске приложения.
  + 2 типа: Файловое кэширование (`FileBasedCache`)
    - кэш в файловой системе
    - для простых сценариев, где не требуется высокая скорость, медленнее, чем кэш в памяти или Redis
    - требует места на диске
  + 3 типа: База данных (`DatabaseCache`)
    - кэш в базе данных
    - если проект уже активно использует базу данных
    - замедляет производительность, так как кэш хранится в той же базе, что и основные данные, не подходит для высоконагруженных систем
* Redis является более мощным инструментом для кэширования
  + работает в оперативной памяти, что делает его чрезвычайно быстрым
  + поддерживает сложные операции (например, Pub/Sub), что полезно для систем в реальном времени
  + легко масштабируется и может использоваться в распределённых системах
  + данные кэша разделяются между всеми процессами и серверами, что делает его идеальным для **кластеров**
  + **сохраняет** данные на диск для предотвращения потери данных после перезапуска, чего нет в `LocMemCache`
  + Поддержка TTL (времени жизни записей)
  + Подходит не только для кэширования, но **и для других задач (сессии, Pub/Sub, очереди)**
* управляет обменом сообщений между **WebSocket-клиентами**
  + взаимодействие между различными **экземплярами** приложения, передача сообщений в реальном времени между пользователями
* Django Channels использует Redis для управления состоянием WebSocket-соединений и маршрутизации сообщений между различными клиентами
* Redis = брокер для обмена сообщениями между разными инстансами Django (если у вас несколько серверов или контейнеров)
* Redis = для работы с асинхронными задачами через Django Channels
* настройка в CHANNEL_LAYERS
* промежуточный уровень (channel layer) для обработки событий в реальном времени
* Django Channels использует Redis через установленный backend (`channels_redis`)

### middlware
* набор классов
* для сервера middleware приложение
* для приложения middleware сервер
* обрабатывают HTTP-запросы и HTTP-ответы, **а сокеты?**
* запускаются внутри Django-приложения
* не являются серверами
* часть логики Django-приложения
* служат фильтрами или обработчиками, модифицирующими запросы/ответы
* помогают в аутентификации, защите от атак, обработке сессий, модификации ответов
* запрос поступает от Daphne в django
* каждый middleware по очереди обрабатывает запрос и передаёт запрос следующему middleware
* SecurityMiddleware обратывает запрос
  + добавляет заголовки безопасности (`Strict-Transport-Security` для HTTPS)
  + перенаправляет HTTP-запросы на HTTPS (если настроено) **куда именно? какой-то порт?**
  + убирает потенциально опасные элементы из запросов (например, если они могут привести к **уязвимости XSS**)
* SessionMiddleware
  + загружает данные сессии из хранилища
  + если **сессии хранятся в Redis или базе данных**, сохраняет данные сессии после выполнения запроса (пример: Если пользователь **аутентифицирован**, сессия **сохраняет его идентификатор**)
* CommonMiddleware
  + выполняет базовую обработку (например, перенаправление с /)
  + **Перенаправление при добавлении/удалении `/` в конце URL** (если включено)
  + Возвращает стандартные заголовки HTTP (`Content-Type`, ...)
* CsrfViewMiddleware
  + Проверяет наличие и правильность CSRF-токена
  + Если токен отсутствует или неверный, возвращает ошибку `403 Forbidden`
  + Обеспечивает защиту от атак типа **Cross-Site Request Forgery**
* AuthenticationMiddleware
  + Проверяет сессию или заголовки авторизации
  + Добавляет **объект `request.user`**, чтобы представления могли определить, аутентифицирован ли пользователь
* MessageMiddleware
  + обрабатывает систему сообщений (flash messages): временные уведомления или сообщений пользователю на веб-странице, которые исчезают после того, как пользователь обновит страницу или перейдет на другую
    - для отображения успешных действий, предупреждений или ошибок, например, после того как пользователь отправил форму, обновил профиль, или завершил какое-то действие в приложении
  + Когда пользователю нужно показать сообщение, это сообщение помещается в **сессии** с помощью системы сообщений.
  + сообщения остаются в сессии только до следующего запроса, а затем удаляются автоматически
  + не имеет отношения к чату
  + В отличие от чатов, которые требуют постоянного обновления страницы или использования WebSocket для обмена сообщениями, flash-сообщения отображаются в верхней части страницы и исчезают через несколько секунд или при обновлении
* XFrameOptionsMiddleware
  + добавляет заголовок `X-Frame-Options` для защиты от атак **clickjacking** (например запрещает отображение сайта в **iframe** на других доменах)
* запрос передаётся в view
* view обрабатывает его и формирует ответ
* XFrameOptionsMiddleware Ответ сначала обрабатывает (например, добавляет заголовок X-Frame-Options)
  + XFrameOptionsMiddleware добавление заголовков безопасности
* MessageMiddleware (сохраняет flash-сообщения)
* AuthenticationMiddleware (например, завершает обработку данных авторизации)
  + добавление объекта `request.user` для авторизованных пользователей 
* CsrfViewMiddleware (может добавлять CSRF-токен в шаблон)
  + CsrfViewMiddleware проверка CSRF-токена **ещё раз?**
* CommonMiddleware (может модифицировать заголовки)
* SessionMiddleware (сохраняет сессии, если они изменились)
  + SessionMiddleware Сохранение сессии
* SecurityMiddleware
  + SecurityMiddleware Проверка заголовков 
  + SecurityMiddleware добавление заголовков безопасности

### WebSockets
* сервер и клиент обмениваются данными в реальном времени: чата, игровой механики
* Браузер пользователя (JavaScript **WebSocket API**) ?
* создание 
  + клиент загружает страницу чата или игровой комнаты
  + клиентский код js инициирует соединение через **WebSocket API**: `const socket = new WebSocket("ws://example.com/ws/chat/room1/");`
  + daphne устанавливает WebSocket-соединение, **передавая его в ASGI-приложение**
  + `{ "type": "chat.message", "message": "Hello, World!" }` браузер -> nginx -> daphne -> механизм Channels -> Django consummer
  + `{ "type": "chat.message", "message": "Hi there!" }` от сервера к клиенту
* Django Channels
  + расширение Django
  + для обработки асинхронных протоколов (WebSocket), управления асинхронными соединениями внутри Django
  + не сервер, не принимает запросы от клиента 
  + обработка соединений внутри приложения 
  + `websocket_urlpatterns = [ re_path(r"ws/chat/(?P<room_name>\w+)/$", consumers.ChatConsumer.as_asgi()) ]` связывает URL, по которым поступают запросы, с обработчиками Consumers
    - `<room_name>` динамически извлекается из URL, тут любое слово (`\w+`) 
  + `consumers.ChatConsumer.as_asgi()` связывает запрос с Consumer-классом
    - `as_asgi()` позволяет Consumer работать в асинхронной среде 
  + Channel Layers для взаимодействия между Consumers и для передачи сообщений между **процессами**
    - **Redis = backend для Channel Layers**
* для работы с wss://
  + сервер использует HTTPS с действительным SSL-сертификатом
    - если сертификат отсутствует или самоподписан, браузер может блокировать подключение WebSocket

### подключить статические файлы
* CSS, JavaScript, изображения
* CSS-OM = дереао как DOM
* bootstrap готовые стили
  - можно создавать кастомные на основе н их
* могут располагаться в разных местах
  + некоторые стили автоматически приносят сторонние **пакеты** (Django Admin, Django REST Framework)
  + некоторые — ваши файлы (bootstrap.min.css во фронтенде)
  + вместе с django из коробки идёт Django Admin и стандартные CSS-файлы для админки
    - при `python manage.py collectstatic` они копируются в staticfiles/admin/… (или admin/…)
  + Django REST Framework (DRF) имеет свои стат. файлы для **browsable API** или других админ-панелей
    - при `python manage.py collectstatic` они копируются в rest_framework/...
  + Ваши собственные стили (frontend)
    - frontend/static/css
    - например, bootstrap.min.css или ваши собственные файлы
  + `python manage.py collectstatic` Django собирает все статические файлы, включая админку и DRF, в общую директорию сборки STATIC_ROOT
    - внутри для каждого приложения (admin, rest_framework, ...) и для ваших собственных стилей, если вы настроили STATICFILES_DIRS
  + Django складывает их либо в подпапках staticfiles/…, либо хранит их отдельно, пока вы не соберёте их в одну директорию
  + разные пакеты = разные CSS, каждый пакет содержит свои статические ресурсы, не мешаясь с чужими
  + Автоматическая сборка — Django находит все static/ директории в установленных приложениях и складывает их в папку staticfiles (или то, что указано в STATIC_ROOT)
  + можете хранить файлы фронтенда отдельно и отдавать их Nginx-ом без участия Django
  + самое главное — чтобы все CSS-файлы находились/отдавались там, где ожидает браузер
    - физически файлы могут лежать в разных местах (при правильных настройках статики Django/Nginx)
  + настроить раздачу этих файлов (через Django в DEV, через Nginx или другую службу в PRODUCTION)
* `STATIC_ROOT = os.path.join(BASE_DIR, 'staticfiles')`
  + директория на сервере
  + `python manage.py collectstatic` -> тут собираются все статические файлы приложений, настроенных для работы со статикой 
  + при подготовке проекта к продакшн
  + **не трекается гитом**
* INSTALLED_APPS = ...  'django.contrib.staticfiles', ... 
* `STATIC_URL = '/static/'`
  + `http://localhost:8000/static/` публичный 
  + `http://localhost:8000/static/css/style.css`
  + стат файлы доступны на сайте
  + при разработке можете напрямую ссылаться на файлы через URL
  ```
* STATICFILES_DIRS
  + Если используете нестандартную структуру (**папка статических файлов во фронтенде**), пропишите:
  BASE_DIR = os.path.dirname(os.path.dirname(os.path.abspath(__file__)))
  + STATICFILES_DIRS = os.path.join(BASE_DIR, '../frontend/static') Путь к статическим файлам
  + STATICFILES_DIRS = родительская директория, содержит статические файлы
  + STATICFILES_DIRS = os.path.join(BASE_DIR, '..', 'frontend', 'static')  # путь до frontend/static
* {% load static %}, чтобы Django обработал ссылку {% static ... %}
  + статические файлы Django (table.css) настроены для загрузки через {% static %}
* `python manage.py collectstatic` файлы будут скопированы в backend/staticfiles
* `python manage.py findstatic css/popUpChat.css`
* `python manage.py runserver`
* сервер Django может обслуживать статические файлы: `urls.py`:
  ```
  from django.conf import settings
  from django.conf.urls.static import static
  urlpatterns += static(settings.STATIC_URL, document_root=settings.STATIC_ROOT)
  if settings.DEBUG:
      urlpatterns += static(settings.STATIC_URL, document_root=settings.STATICFILES_DIRS[0])
  ```
* `frontend/static` доступна контейнеру:
  ```
  backend:
    volumes:
      - ./backend:/app
      - ./frontend/static:/app/frontend/static
  ```
* В разработке (DEBUG = True), статика обслуживается самим Django
  + достаточно просто запустить python manage.py runserver
* В продакшене (DEBUG = False)
  + настроить раздачу статики через collectstatic и веб-сервер Nginx
  + сделать collectstatic
  + настроить сервер для раздачи статических файлов
* DevTools → вкладка Network
  + какой путь к CSS-файлу пытается загрузить браузер
  + код ответа 200
  + код ответа 404: Django не может найти файл, конечный URL, по которому файл запрашивается = расположение файла?
* Иногда, если CSS-файл уже кэшировался, браузер может залипать на устаревшей версии
  + проверьте, обновляется ли версия CSS (можно добавить некий ?v=123 в конце ссылки или очистить кэш браузера)
* фронтенд 
  - хранятся и собираются логика, стили и ресурсы (CSS,...)
  - фронтенд — отдельное SPA
  - результат сборки (папка build или dist) может отдаёся отдельным сервером (Nginx), либо «прокидываться» через бэкенд, если вы используете Django для статики (чаще Nginx)
  - в бэкенде только делаете API (Django REST **или** Channels)
  - CSS лежат в папке фронта
  - после сборки CSS окажется в итоговых бандлах (или отдельных .css файлах)
  - Django будет их отдавать или (чаще) их будет отдавать Nginx
  - бэкенд отдает данные (API)
  - frontend/ (с package.json, src/, node_modules/ и т.п.)
  - CSS будет находиться внутри этой папки
  - а Django отдаёт или проксирует статику (или Nginx)
  - settings.py: STATIC_URL = '/static/', STATICFILES_DIRS = [ # ... ]: статика берётся из папки frontend/dist или build
  -Django подхватывает статические файлы из фронтенд
  - в Django-шаблоне либо нет упоминания о стилях, либо там просто <div id="root"></div> (для React)
  - стили приходят с фронтенда (собранный **бандл**)
  - Запустите приложение, DevTools, Network, запрос к файлу .css: http://localhost:3000/... = отдельный дев-сервер фронтенда  

### manage static files (e.g. images, JavaScript, CSS)
* https://docs.djangoproject.com/en/4.2/howto/static-files/
* Websites generally need to serve additional files such as images, JavaScript, CSS. In Django, we refer to these files as “static files”.
* Django provides django.contrib.staticfiles to help you manage the static files

#### Configuring static files
  + Make sure that django.contrib.staticfiles is included in your INSTALLED_APPS
  + settings file: `STATIC_URL = "static/"`
  + templates: use the static template tag to build the URL for the given relative path using the configured staticfiles STORAGES alias:
      ```
      {% load static %}
      <img src="{% static 'my_app/example.jpg' %}" alt="My image">
      ```
  + Store your static files in a folder called static in your app. For example my_app/static/my_app/example.jpg
  + You’ll also need to actually serve the static files.
    - During development, if you use django.contrib.staticfiles, this will be done automatically by runserver when DEBUG is set to True (see django.contrib.staticfiles.views.serve()). 
    - This method is inefficient and insecure, so it is unsuitable for production.
    - See How to deploy static files for proper strategies to serve static files in production environments.
* Your project will probably also have static assets that aren’t tied to a particular app
  + In addition to using a static/ directory inside your apps, you can define a list of directories (STATICFILES_DIRS) in your settings file where Django will also look for static files. For example:
  ```
  STATICFILES_DIRS = [
      BASE_DIR / "static",
      "/var/www/static/",
  ]
  ```
  + See the documentation for the STATICFILES_FINDERS setting for details on how staticfiles finds your files

#### Static file namespacing
  + Now we might be able to get away with putting our static files directly in my_app/static/ (rather than creating another my_app subdirectory), but it would actually be a bad idea. Django will use the first static file it finds whose name matches, and if you had a static file with the same name in a different application, Django would be unable to distinguish between them. We need to be able to point Django at the right one, and the best way to ensure this is by namespacing them. That is, by putting those static files inside another directory named for the application itself.
  + You can namespace static assets in STATICFILES_DIRS by specifying prefixes

#### Serving static files during development
* If you use django.contrib.staticfiles as explained above, runserver will do this automatically when DEBUG = True
* If you don’t have django.contrib.staticfiles in INSTALLED_APPS, you can manually serve static files using the django.views.static.serve() view
  + This is not suitable for production use
  + For some common deployment strategies, see How to deploy static files
  + if STATIC_URL = static/, you can do this by adding the following snippet to your urls.py:
    ```
    from django.conf import settings
    from django.conf.urls.static import static
    urlpatterns = [
        # ... the rest of your URLconf goes here ...
    ] + static(settings.STATIC_URL, document_root=settings.STATIC_ROOT)
    ```
  + This helper function works only in debug mode and only if the given prefix is local (e.g. static/) and not a URL (e.g. http://static.example.com/).
  + this helper function only serves the actual STATIC_ROOT folder; it doesn’t perform static files discovery like django.contrib.staticfiles
  + static files are served via a wrapper at the WSGI application layer. As a consequence, static files requests do not pass through the normal middleware chain

#### Serving files uploaded by a user during development
* During development, you can serve user-uploaded media files from MEDIA_ROOT using the django.views.static.serve() view
  + This is not suitable for production use
  + For some common deployment strategies, see How to deploy static files
  + For example, if MEDIA_URL = media/, you can do this by adding the following snippet to your ROOT_URLCONF:
    ```
    from django.conf import settings
    from django.conf.urls.static import static
    urlpatterns = [
        # ... the rest of your URLconf goes here ...
    ] + static(settings.MEDIA_URL, document_root=settings.MEDIA_ROOT)
    ```
  + This helper function works only in debug mode and only if the given prefix is local (e.g. media/) and not a URL (e.g. http://media.example.com/).

#### Testing
* When running tests that use actual HTTP requests instead of the built-in testing client (i.e. when using the built-in LiveServerTestCase) the static assets need to be served along the rest of the content so the test environment reproduces the real one as faithfully as possible, but LiveServerTestCase has only very basic static file-serving functionality: It doesn’t know about the finders feature of the staticfiles application and assumes the static content has already been collected under STATIC_ROOT.
* Because of this, staticfiles ships its own django.contrib.staticfiles.testing.StaticLiveServerTestCase, a subclass of the built-in one that has the ability to transparently serve all the assets during execution of these tests in a way very similar to what we get at development time with DEBUG = True, i.e. without having to collect them using collectstatic first.

#### Deployment
* django.contrib.staticfiles provides a convenience management command for gathering static files in a single directory
*  `STATIC_ROOT = "/var/www/example.com/static/"`where you’d like to serve these files
* `python manage.py collectstatic` copy all files from your static folders into the STATIC_ROOT
* Use a web server of your choice to serve the files
  + How to deploy static files covers some common deployment strategies for static files

### некоторые файлы
* .map (Source Map) для отладки кода в браузере
  + При минификации или транспиляции CSS/JS (например, при сборке проекта) ваш изначальный код (в данном случае CSS) превращается в более оптимизированную сжатую версию
  + Source Map хранит сопоставление (mapping) между сжатым (скомпилированным) кодом и исходным кодом
  + позволяет браузерным DevTools (Chrome, Firefox и др.) «понимать», какой изначальный CSS, SCSS или другой препроцессор соответствует конкретной строчке в минифицированном файле
  + bootstrap-grid.css — часть Bootstrap, отвечающая за сетку (Grid system)
  + соответствующий bootstrap-grid.css.map нужен, чтобы при отладке вы видели правильные номера строк и селекторы из исходных файлов Bootstrap, а не из минифицированной (или объединённой) версии
  + .map-файлы нужны при разработке или отладке, чтобы видеть исходный код
  + в продакшен версии многие отключают генерацию .map-файлов, чтобы уменьшить вес приложения и не раскрывать детали исходного кода
  + иногда их оставляют, чтобы упрощать диагностику проблем прямо на продакшен-сервере
  + если не хотите, чтобы пользователи имели доступ к отладочной информации, .map-файлы можно не заливать на сервер или закрывать их от внешнего доступа
  + в большинстве случаев там только информация о соответствии строк исходного и минифицированного кода

### запуск проекта
* For Ecole42 computers, I've updated settings of docker file in DEV branch 
  + порт, который нужен для django, занят
  + поменять номера портов в docker и в nginx
  + 6800  port for redis 
  + 4444 port frontend HTTP connections
  + 4443 port frontend for SSL connections over HTTPS
  + for local Ecole 42's network `ALLOWED_HOSTS = ['*']` in settings.py
* Секретный ключ от api раз в две недели примерно обновляется
* 'docker compose up --build' (BUILD нужно)
  - все скачается и распакуется
  - потом в терминале можно Ctrl + C (условное клавиатурное прерывание)
* `sudo ufw status` 80, 443, 8000 открыты
* `curl http://localhost:8000`
* `docker exec -it git-backend-1 ss -tuln` проверить порты бэкенда
  + `tcp LISTEN 0 511 0.0.0.0:8000 0.0.0.0:*`
* `docker exec -it git-frontend-1 curl http://git-backend-1:8000` подключиться к бэкенду из контейнера
* `docker logs git-backend-1`
* `docker logs git-frontend-1`
* через 42 на посте кластера, отличном от того, на котором он размещен
  + 1 способ: каждый раз менять URL-адрес перенаправления, чтобы он соответствовал IP-адресу хост-компьютера 
  + 2 саособ лучше:
    - каждый компьютер использует VM, в ней изменяете файл хостов, чтобы связать IP-адрес исходной станции с URL-адресами
    - подключиться к другому компьютеру в кластере через его внутреннее доменное имя школы, добавить это в nginx.conf, чтобы веб-сервер разрешал связываться с вами в этом домене, а также на стороне django

### Dockefiles
* `PYTHONPATH` переменная окружения
  + где Python ищет директории с модулями, кастомные библиотеки, пакеты при импорте
  + `PYTHONPATH` `export PYTHONPATH="/mnt/md0/42/14_ft_transendence/backend:$PYTHONPATH"`
  + удалили
+ PYTHONUNBUFFERED нужна
* не рекомендуется 
  ```dockerfile
  COPY requirements.txt .
  RUN pip install --no-cache-dir -r requirements.txt
  COPY . .
  ```
  + Docker кэширует каждый слой (каждую команду). Если вы копируете только `requirements.txt` перед установкой зависимостей, то (`RUN pip install ...`) будет повторно использовать кэш, если файл `requirements.txt` не изменился.
  - Если вы измените код приложения (например, Python-файлы), установка зависимостей не будет повторяться.
  + Если вы сначала копируете весь код (`COPY . .`), то любые изменения в файлах приложения (например, в коде Python) приведут к тому, что Docker заново выполнит `RUN pip install ...`, даже если `requirements.txt` не изменялся.
  + Кэширование нарушается
   - Любые изменения в любом файле вашего приложения (даже в HTML, CSS или документации) приведут к повторной установке зависимостей.
   - Это замедлит сборку Docker-образа
  + Увеличение времени сборки: При каждой сборке будут тратиться ресурсы на переустановку всех пакетов, даже если зависимости остались неизменными
  + Ускоряет сборку благодаря кэшированию,
  + Избегает повторной установки зависимостей без необходимости
* `./context`
  + все файлы доступны для Docker в процессе сборки
  + только те файлы, которые явно указаны в Dockerfile (COPY, ADD, ...), будут скопированы в контейнер
* the volumes directive is only applied at runtime and does not affect the build stage
* If you want to avoid using COPY, you can change the pip install command to point to the mounted volume
  + this assumes that the requirements.txt file exists in the mounted /app folder at runtime
  + you need to skip installing dependencies during the build process
  + install them at runtime instead
  
### разное
* **Карточка Bootstrap**  
* в модели пользователя сохраняю аватарки
  + у фронт энда есть требования какие нибудь к аватаркам или они могут сами отрисовывать?
  + вдруг пользователь сохранит свою фотографию 1000 х 1000 пикселей, на фронте вы сможете сами отрисовать аватарку ?
* с точки зрения реализации api есть **class based views** (не сильно сложнее) / functions based views (проще)
* предупредить клиента, что его ждет игра, даже если он в чате
* валидация данных
  + **js на фронте читает инпут, проверяет с помощью regex**
  + функция make password django шифрует на сервере ?
  + бэк ещё раз валидирует (проверяет пароль и почту, ...) **зачем два раза** 
* change the post requests to a websocket. The idea was to make the game and chat using websockets (native browser api), it’s beneficial in terms of continuous data streaming
* `frontend/static/app.js`
  + вызывает fetch к бэкенду и затем динамически обновляет DOM, это не делает приложение SPA-фреймворком — это обычная логика на чистом JS
  + связь между бекендом и фронтендом
  + получать информацию о пользователе (имя, статус, аватар, результаты игр)
  + динамическое обновления пользовательского интерфейса с использованием данных, полученных от бэка 
  + `fetch('http://backend:8000/api/user-data/')`: HTTP-запрос к эндпоинту бекенда `/api/user-data/`, получить данные JSON о пользователе
  + `response => response.json()` конвертирует ответ JSON от сервера в объект JavaScript
  + после получения данных пользовательский интерфейс (UI) (отображение имени пользователя, аватарка, настройки) обновляется без перезагрузки страницы
  + улушение: динамически изменять DOM:
    ```
    fetch('http://backend:8000/api/user-data/')
        .then(response => response.json())
        .then(data => {
            document.getElementById('username').innerText = data.username;
            document.getElementById('avatar').src = data.avatar_url;
        })
        .catch(error => console.error('Error:', error));
    ```
  + улучшене: `async/await` для упрощения чтения:
     ```
     async function fetchUserData() {
         try {
             const response = await fetch('http://backend:8000/api/user-data/');
             const data = await response.json();
             console.log('User Data:', data);
         } catch (error) {
             console.error('Error fetching user data:', error);
         }
     }
     fetchUserData();
* вы должны предупредить клиента о том, что его ждет игра, даже если он в чате
* single-page (SPA)
  + сайт одностраничный
  + один html файл
  + этот html меняется с помощью js
  + не надо: server side rendering подход
  + не надо: django html - server-side code
  + не надо: в завимисости от какого-то условия, показываем или нет какие-то части страницы
  + надо: js
  + надо: код внутри {} исполняется в django, если условие выполняется (например, есть юзер), он выполняет и заново отправляет html
* virtual environment не нужно, потому что у нас докер
* pass reset будет ли?
* change username, email будет ли?
* to set up the database **для чего?**
  + `python manage.py makemigrations`, `python manage.py migrate`
  + create a superuser `python manage.py createsuperuser`
* DRF - автоматичесая документация эндпоинтов
  + Browsable API (встроенная документация): в `http://localhost:8000/api/` или `http://localhost:8000/` список эндпоинтов

### Organisation
* на всю линейку продуктов от JetBrains бесплатная студенческая лицензия, для фронта WebStorm
* the password for the django admin panel ...
* docker vscode extension 
  + launch bash inside a container from gui

### to do
* pop-up windows : login, chat, profile
* страница comptetition, profile, настройки
* Б
  + структуры данных
  + Django REST Framework
  + модуль с сокетами и чатом (live chat, уведомления о турнирах, и проч)
  + API, эндпоинты и методы для API
  + live chat
  + **rabbitMQ модуль сообщение от сервера**
  + google doc: status which modules are chosen and which modules are done
  + to finish APIS
  + live chat, ws
* Л
  + авторизация
  + фронт-энд
  + шаблон для фронт энда
  + profile
  + game page
  + auth issue
* Ан
  + фронт-энд
  + накидать в Figma шаблоны страничек (часть есть в Миро) страницы: страница с логином, с самой игрой (пока без игры), профиль пользователя, страница с турниром
  + писать код, делать шаблончики  
  + структуры страничек
  + API с бэкэнда
* Амин
  + вебсокеты для модуля remote players
    - обмен информацией между игроками и сервером о локации ракетки и мяча
  + game logic
    - whether we want to follow basic ping pong rules?
    - the ball should speed up when paddle hits the ball ?
    - https://stackoverflow.com/questions/54796089/python-ping-pong-game-speeding-up-the-ball-after-paddle-hit 
  + some sort of **anticheat** to be sure that the users mouvement are normal
    - my code will be easy, it'd just gonne output two players position and the ball and then you can render it however you want
  + I'll update you soon on the game websocket
* basic requirements 20.01.2025 
  + All pong game part will be done by Amine? Do you need help with front-end (table, paddles, ball, some activity of JS or someone will do it?
  + Tournament, registration and matchmaking system by Alexey? Do you need help? 
  + Basic front-end will be done by Alexey? (profile page, other pages) or you need help?
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
* settings.py logging.basicConfig
  + глобальные настройки логирования в Python
  + влияет на все логгеры, включая те, которые используются Django и Channels
  + Django и Channels используют свои логгеры (настроенные через LOGGING), может конфликтовать с их внутренними настройками
  + не предоставляет тонкого контроля над логгерами (раздельное управление для django и channels)
  + не рекомендуется для Django, так как может игнорировать встроенные настройки логирования
* settings.py LOGGING
  + настраивает Django и все его зависимости через встроенную систему логирования
  + можете настроить разные обработчики, уровни логирования и форматы для отдельных логгеров:
    - django — для стандартных событий Django (запросы, ответы, ошибки).
    - channels — для событий, связанных с WebSocket и канальным слоем.
  + гибкий подход, позволяет разделять логи
  + Позволяет управлять логированием для разных компонентов (например, django, channels, django.db)
  + Совместимо с встроенной системой Django, которая использует LOGGING
  + Рекомендуется для Django-проектов, особенно если вы хотите гибко управлять логами и выводить разные сообщения для компонентов

### test
* bakyt: Endpoint that are formed from views.py from different folders
  + `urls.py` связывает эндпоинты (URL-маршруты) с функциями/классами представлений из `views.py`
  + просматривать `views.py` в каждом приложении: какие представления и какие URL ассоциированы с функциями или классами в разных частях проекта
* если подключены библиотеки для документирования API, то `http://localhost:8000/swagger/` или `http://localhost:8000/redoc/`
* Postman: импортируйте коллекцию эндпоинтов, если она уже создана  
* Postman для изучения API, отправляя запросы на `/api/`, `/swagger/`, ... и исследуя доступные маршруты
* endpoints HTTP (API или страницы) with Postman:
  + Введите адрес вашего сервера, например:
     - `http://localhost:8000/api/endpoint/`
     - `https://example.com/api/endpoint/`
  + метод (GET, POST, PUT, DELETE и т. д.).
  + если требуется авторизация, добавьте токен или данные пользователя (если используете `Token` или `JWT`).
  + отправьте запрос и проверьте статус ответа (200 OK, 401 Unauthorized и т.д.) и тело ответа
* endpoints HTTP (API или страницы) with Curl:
  + `curl -X GET http://localhost:8000/api/endpoint/`
  + `curl -X POST http://localhost:8000/api/endpoint/ -H "Content-Type: application/json" -d '{"key": "value"}'`
* endpoints HTTP (API или страницы) with browser:
  + Для эндпоинтов, которые возвращают HTML (главная страница, панель администратора), просто откройте браузер и введите URL
* endpoints Websockets with Postman
  + меню `New Request` - `WebSocket`
  + Укажите URL WebSocket-соединения:
     - `ws://localhost:8000/ws/chat/room_name/`
     - `wss://example.com/ws/chat/room_name/`
  + Установите соединение и отправьте тестовые сообщения
  +  Посмотрите, возвращает ли сервер ответы.
* endpoints Websockets with Chrome + расширения [Smart WebSocket Client](https://chrome.google.com/webstore/detail/smart-websocket-client/kzhddgcmkfiimcdlddieeoemkbdmgkag) 
  + Укажите URL WebSocket: `ws://localhost:8000/ws/chat/room_name/`
  + Нажмите «Connect».
  + Отправьте тестовые сообщения и проверьте, получает ли сервер их.
* endpoints Websockets with Python + библиотека `websockets`
  ```python
  import asyncio
  import websockets
  async def test_websocket():
      uri = "ws://localhost:8000/ws/chat/room_name/"
      async with websockets.connect(uri) as websocket:
          await websocket.send("Hello, WebSocket!")
          response = await websocket.recv()
          print(f"Response: {response}")
  asyncio.run(test_websocket())
  ```
* Redis integration
  + `redis-cli`
  + `PING`
    - Ожидаемый ответ: `PONG`.
  + Проверьте, публикуются ли сообщения: `SUBSCRIBE my_channel`
    - отправьте тестовые сообщения в канал `my_channel` и убедитесь, что они принимаются
* HTTP-тесты с autrotests Django
  + Django предоставляет встроенные инструменты для тестирования HTTP:
    ```python
    from django.test import TestCase
    from django.urls import reverse
    class APITest(TestCase):
        def test_endpoint(self):
            url = reverse("api_endpoint_name")  # Используйте имя вашего маршрута
            response = self.client.get(url)
            self.assertEqual(response.status_code, 200)
    ```
* WebSocket-тесты с Django Channels + `pytest`:
  ```python
  from channels.testing import WebsocketCommunicator
  from myproject.asgi import application
  import pytest
  @pytest.mark.asyncio
  async def test_websocket():
      communicator = WebsocketCommunicator(application, "/ws/chat/room_name/")
      connected, _ = await communicator.connect()
      assert connected
      await communicator.send_to(text_data="Hello!")
      response = await communicator.receive_from()
      assert response == "Hello, WebSocket!"      await communicator.disconnect()
  ```
* basic functions of website
* websockets in room page
* websockets in the game
* connection
  + Connection from another computer is working (so local network is working) 
  + When Ivan tried to login with 42Auth from another computer (not server) - he got error 400; however basic sign up with email is working. 
  + My login with 42Auth from server computer worked.
* открываю 127.0.0.1:8000/chat/room1/ в двух разных местах и они получаются объдинены в одну комнату, оба видят все сообщения.
На данный момент только это надо проверять, потому что другое пока не реализовано.
* Убедитесь, что WebSocket работает: проверьте консоль браузера (F12) на наличие ошибок
* Проверьте `docker-compose logs`

### DO NOT FORGET
* remove убрать settings.py SECRET_KEY, frontend/etc/private.key
* close ports 800 and 6800 for outside
* `http://backend:8000` хранить в перменной окружения
* close http://localhost/backend:8000/chat
* to justify your choices during the evaluation
