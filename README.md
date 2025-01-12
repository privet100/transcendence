```
                            Internet
               +----------------+----------------+
          Browser/User                         GitHub 
      1) HTTPS |                                 | 2) Webhook (HTTPS)
               v                                 v
        +--------------+                 +-----------+
        | Nginx        |                 |  Nginx    |
        +------+-------+                 +-------+---+
               |                                 |
       location /static           location /webhook proxy_pass
               |      v                  v
               |  /usr/share/nginx/html/static/      
               |                                      
               |    ( location / ) proxy_pass http://backend:8000
               v
        +------+------------------------+
        | Django backend runserver 8000 |
        +------+--------+---------------+
Запросы к API/логике |        |  Веб-хуки ( /webhook ), чаты, API и т.д.
                     v        v
               +-----+--+   +-------+
               | Postgres|  |Redis  |
               +---------+  +-------+
```

### как это работает
* Пользователь открывает в браузере https://tr.naurzalinov.me/
* защищённое соединение с Nginx
* приходит запрос к http://<ваш IP>/ на фронтенд-контейнер Nginx, порт 443
  + если запрос к статическому файлу (CSS/JS/картинк/HTML), nginx отдает его из /usr/share/nginx/html/static/
  + если /, /api/…, nginx проксирует запрос на backend:8000
  + Django обрабатывает API
    - если это данные из Django (JSON, HTML, WebSocket), проходят через прокси на порт 8000
* Фронтенд при необходимости вызывает методы API (для аутентификации, получения/отправки данных)
  + эти запросы идут через Nginx на Django
* Ответ возвращается по цепочке Django → Nginx → Пользователь/Сервис
* Django-представление (views.py) проверяет токен/подпись, делает git pull, docker-compose up --build -d, и т.д

### как это работает - второй вариант
* GitHub посылает webhook на https://tr.naurzalinov.me/webhook
  + nginx проксирует запрос на backend:8000/webhook
  + Django обрабатывает пуши GitHub, может сделать git pull, пересобрать образы Docker и т.д

### backend
* серверные задачи: бизнес-логика (чат, авторизация, API, управление базой данных, игровая логика, ...), роутинг Django, модели, чаты, аутентификация
* runserver 0.0.0.0:8000 => запускает Django-приложение
* backend = Django-приложение (API, бизнесс-логика, аутентификация, чат)
* Django-сервер слушает внутри контейнера на порту 8000 внутри сети Docker
  + снаружи его можно вызвать на localhost:8000
* Django Бэкенд-фреймворк
* обрабатывает запросы от фронтенда: логика аутентификации, чат (через Channels, если настроено), API для фронтенда, взаимодействие с базой данных
* формирует ответы
  + рендеринг шаблонов (Server-Side Rendering) или отдача JSON-данных (REST API)
  + отдаёт JSON/HTML или websocket (Channels) — в зависимости от реализации
* Обращается к Postgres для чтения/записи данных (пользователи, чат, профили)
* Общается с Redis для кэша, сессий, Pub/Sub (чат, обновления статусов игроков в реальном времени)
* myproject/
  + основной проект
  + «оболочка», связывающая всё воедино: настройки, корневой маршрутизатор, запуск и управление всем проектом
  + ключевые файлы проекта Django
  + __init__.py, asgi.py, wsgi.py — стандартные файлы «точки входа» Django
  + settings.py Django (подключение к БД, приложения, Middleware, статика, и т.п.)
  + urls.py — общий роутинг (ссылки) для всего проекта
  + основная конфигурация проекта Django  
  + `settings.py` (главные настройки: базы данных, приложения, статика и пр.)
  + `urls.py` (корневой маршрут)
  + `wsgi.py` и `asgi.py` (точки входа для запуска приложения)
  + `__init__.py` (делает папку пакетом Python)
  + в этой папке вы «собираете» все отдельные приложения (apps) вместе, указываете их в `INSTALLED_APPS` и настраиваете проект в целом
* несколько подпроектов-«приложений»
  + Django app, отдельный модульный приложения внутри проекта, отвечают за конкретную часть функционала, бизнес-логика (модели, вьюхи, сериализаторы, формы), тесты, админ-конфигурация и т. п.  
  + api_42 отвечает за интеграцию с 42 API (авторизация через intra 42 и т.д.)
  + module_auth логика авторизации
  + auth_app логика аутентификации
  + chat (модели для сообщений, WebSocket-каналы, если используются Django Channels)
  + myapp  
  + `models.py`,  
  + `views.py`,  
  + `urls.py` (локальный роутинг для этого приложения) 
  + `admin.py`
  + `apps.py` (регистрация самого приложения в Django)
  + `migrations/` (миграции для моделей)
  + `tests.py` (тесты конкретно для этого приложения)
* staticfiles
  + Django или система сборки складывает статические файлы после команды collectstatic
  + обычно такая папка не трекается гитом
  + создана для удобства
* templates/
  + HTML-шаблоны
  + часть фронтенда рендерится на сервере
* requirements.txt
  + Python-библиотеки для работы бэкенда
  + Установка Python, зависимостей (pip install -r requirements.txt)
* manage.py
  + Django-утилита для миграций, запуска сервера, создания суперпользователя и др
* dockefile
  + Устанавливаете зависимости (Django, DRF, etc.) из requirements.txt
  + Копируете код в /app
  + CMD запуск дев-сервера Django: manage.py runserver 0.0.0.0:8000
    - В продакшене часто используют Gunicorn/Uvicorn + supervisor, но для учебных целей/простоты можно и так
  + При запуске внутри Docker Compose, сервис backend получает имя backend
    - в nginx.conf вы видите proxy_pass http://backend:8000
  + ./backend:/app — локальная backend примонтирована в контейнер как /app
    - изменения в коде сразу видны внутри контейнера (при корректной настройке)
* django:
  + бэкенд со всеми ключевыми механиками (аутентификация, управление базой данных, админка, API)
  + фронтенд работает через HTTP/WS-протоколы, используя эндпоинты Django для обмена данными и интерактивного взаимодействия (чат, игра, профили пользователей)
  + Django даёт множество готовых функций и лучших практик прямо из коробки
    - что накладывает свои рамки в архитектуре
  + Django диктует архитектуру (приложения, модели, views, urls и т.д.), что помогает поддерживать чистоту кода и масштабируемость
  + Модели Django (Models) описывают структуру данных и связи между ними (пользователи, профили, чаты, сообщения, статистика игры и пр.)
  + Представления (Views) обрабатывают логику запросов (создание комнаты, отправка сообщения, обновление профиля)
  + Маршрутизация (URLs): какие функции обрабатывают какие пути (например, api/chat/ или api/game/)
* Управление пользователями и аутентификацией
  + встроенная система аутентификации и гибкая модель пользователей
  + Регистрацию и авторизацию (в том числе OAuth/SSO)
  + Разграничение доступа к страницам (приватные/публичные чаты, комнаты и т.д.)
  + Проверку токена/сессии при каждом запросе с фронтенда
* Администрирование
  + Django Admin позволяет быстро «набросать» интерфейс для управления данными (пользователи, чаты, статистика и пр.)
  + полезно для мониторинга проекта во время разработки и тестирования
* Безопасность и валидация
  + ряд инструментов по защите от распространённых уязвимостей (CSRF, XSS, SQL Injection)
  + благодаря джанговским формам и сериализаторам (в связке с Django REST Framework, если вы его используете), упрощается валидация данных, приходящих от фронтенда.
* Связь с фронтендом
  + Django выступает как JSON API (используя Django REST Framework или WebSocket-соединения через Channels)
  + Фронтенд отправляет запросы к бэкенду (GET/POST и пр.) или подписывается на WebSocket-каналы для чата
  + Django обрабатывает данные, сохраняет в БД, возвращает ответы (JSON/HTTP)
  + При необходимости Django Channels обрабатывает событийную логику для чатов/онлайн-игр
* Django может быть связующим звеном для таких сервисов, как:
  + Файловое хранилище (при загрузке аватарок или результатов матчей)
  + Отправка email (подтверждение регистрации, уведомления)
  + Логирование и сбор метрик (для отладки и мониторинга)
* на базе Python

### frontend
* контейнер frontend (Nginx + собранный фронт)
* «точка входа» для пользователей (SSL, статика, проксирование)
* приём HTTPS/HTTP-запросы извне на порты 80/443
* отвечает на порты 80/443
* проверяет сертификаты SSL
* обеспечивает HTTPS (SSL/TLS)
* выступает как обратный прокси (reverse proxy) для бэкенда
* проксировать запросы на бэкенд (Django) и/или выдавать статический фронтенд
* Запросы на /api/ уходят на Django-контейнер
* остальные запросы отдаются статическими файлами фронтенда
* перенаправляет запросы к backend, проксирует API-запросы (например, http://localhost/api/...) на бэкенд
* Проксирует основные запросы к бэкенду (Django) по внутреннему адресу http://backend:8000
* Раздаёт статические файлы (CSS, JS, картинки) мз /usr/share/nginx/html/static
* клиентский код (CSS, JS, либо отдельный React/ Vue-сборщик)
* certif/ и etc/ для сертификатов, дополнительных конфигураций, скриптов, вспомогательных файлов
* static/ статические файлы фронтенда (CSS, JS, картинки и т.п.)
  + Иногда при сборке фронтенда (React/Vue/Angular) готовый билд кладётся сюда или в подпапку build/ (в зависимости от настроек).
* SSL/HTTPS, проксирование, статика, редиректы, вебхуки
* nginx.conf называется default.conf внутри контейнера
  + переопределяет стандартные настройки Nginx
* Копируем статические файлы (из локальной ./static) в /usr/share/nginx/html/static внутри контейнера
  + так Nginx может их раздавать напрямую по пути /static/
* копируем SSL-сертификаты в нужные места
* внутри контейнера Nginx слушает 80 и 443, а снаружи к frontend привязаны "80:80" и "443:443", это позволяет доступ извне по HTTP(80) и HTTPS(443)
* отдельный endpoint для вебхука
* строит или берёт ваши статические файлы фронтенда
* на хост-машине порт 80 (HTTP) и порт 443 (HTTPS) проброшены в контейнер
* слушает и проксирует или статику фронта (SPA), или перенаправляет /api запросы на backend:8000
* отдаёт фронтенд на http://localhost/
* в роли прокси/сервера статики
* `nginx.conf` в папке `frontend` по следующим причинам:
  + рядом с Dockerfile, чтобы во время сборки он копировался внутрь контейнера
  + Обслуживание статических файлов  
   - Файлы, относящиеся к клиентской части (CSS, JS, изображения), физически лежат в `frontend/static/`, и Nginx, запущенный в контейнере фронтенда, их раздаёт
   - `location /static/ { alias /usr/share/nginx/html/static/; }` - логика фронта, поэтому конфигурация там же
  + Проксирование бэкенда  
   - Тот же «фронтовой» Nginx может осуществлять proxy_pass на ваш бэкенд — по сути, становится точкой входа для всех запросов (HTTPS/HTTP)
   - Nginx выполняет серверную задачу проксирования, он всё равно обслуживает именно фронтенд-контейнер. `nginx.conf` часть «внешнего (front)» эшелона приложения
  + Условное деление директорий  
   - `frontend/`: всё, что касается сервировки фронта: статику, конфиги веб-сервера (Nginx)
   - `backend/`: код Django, его настройки, requirements, миграции и т.д
   - Разделение на frontend и backend — логическая структура, а не строгая привязка к тому, что фронтенд = работает в браузере, а конфиги = сервер
  + Удобство разработки  
   - В Dockerfile для frontend вся «сборка» фронта происходит из одной папки:
     ```
     COPY nginx.conf /etc/nginx/conf.d/default.conf
     COPY ./static /usr/share/nginx/html/static
     ```
  + `frontend` отвечает за раздачу статики и проксирование запросов к бэкенду 

### frontend/nginx.conf
* несколько server-блоков, которые слушают на 443 (HTTPS) и используют разные сертификаты
* есть блоки, слушающие на 80 (HTTP), которые перенаправляют (return 301) на HTTPS
* веб-приложение с поддержкой статических файлов, защищённым соединением и обработкой вебхуков
* Сервер для tr.naurzalinov.me
  + прослушивает HTTPS-трафик на порту 443
  + location /static/: Запросы к /static/ перенаправляются на локальную папку /usr/share/nginx/html/static/
  + location /: Все остальные запросы проксируются на бэкенд (http://backend:8000), при этом передаются заголовки, включая IP клиента и протокол
  + Серверы на порту 443 обрабатывает HTTPS-запросы, делая проксирование на бэкенд
  + Слушает HTTPS на 443 порту для домена tr.naurzalinov.me
  + В location /static/ сервер просто отдаёт файлы из /usr/share/nginx/html/static/
  + В location / все остальные запросы проксируются на http://backend:8000
  + proxy_set_header передаёт нужные заголовки (Host, X-Forwarded-For и т.п.), что важно для Django
* Сервер для IP-адреса 95.217.129.132
  + практически идентичен первому
  + работает для запросов, направленных на IP-адрес 95.217.129.132
* Сервер для вебхуков
  + Обрабатывает запросы к tr.naurzalinov.me и 95.217.129.132, только для пути /webhook
  + Используется отдельный сертификат для вебхуков (/certif/webhook.crt, /certif/webhook.key)
  + Запросы на /webhook проксируются на http://backend:8000, передавая все важные заголовки
  + Специальный блок для вебхуков (GitHub или других).
  + При запросе к https://tr.naurzalinov.me/webhook (или на IP с /webhook), Nginx шлёт запрос на http://backend:8000/webhook
  + Django (в webhook/views.py) уже обрабатывает пуши, git pull и т.д
* Перенаправление HTTP на HTTPS
  + Прослушивает HTTP-запросы на порту 80
  + Перенаправляет все запросы на HTTPS с использованием кода 301 (постоянное перенаправление)
  + HTTP (80 порт), редиректит все запросы на HTTPS (443).
  + Для обоих доменных имён (tr.naurzalinov.me и 95.217.129.132).
  + любой запрос на 80 порту перенаправляется на 443 (HTTPS)
  + на HTTPS-запросах Nginx либо раздаёт статику, либо пересылает запрос в бэкенд
* SSL (HTTPS)
  + чтобы шифровать соединение между клиентом (браузером пользователя) и сервером
  + `listen 443 ssl;` сервер слушает порт 443 и обрабатывает SSL/HTTPS-соединения на данном порту  
  + `ssl_certificate /etc/nginx/ssl/certificate.crt;`, `ssl_certificate_key /etc/nginx/ssl/private.key;` файлы сертификата и приватного ключа для данного домена
  + Когда клиент (браузер) устанавливает HTTPS-соединение, Nginx использует этот сертификат и ключ, чтобы:  
     - Подтвердить подлинность сервера (браузер проверяет, подписан ли сертификат доверенным центром)
     - Установить зашифрованный канал связи (SSL/TLS) между сервером и браузером, чтобы шифровать трафик
  + `ssl_protocols TLSv1.2 TLSv1.3;` ограничивает список версий протокола шифрования
    - сервер поддерживает TLS 1.2 и TLS 1.3
    - более старые протоколы (например, TLS 1.1, SSLv3) не поддерживаются из соображений безопасности  
  + `ssl_ciphers 'HIGH:!aNULL:!MD5';` определяет, какие шифры можно использовать
    - «HIGH» означает сильные (надежные) шифры
    - `!aNULL` и `!MD5` исключают небезопасные варианты  
  + `ssl_prefer_server_ciphers on;` указывает браузерам отдавать предпочтение списку шифров сервера, а не списка клиента
  + Настройка `proxy_set_header ...` передает важные заголовки (Host, IP-адрес клиента, протокол и т.д.), чтобы бэкенд понимал, откуда пришел запрос и мог корректно формировать ответы

### контейнер Redis (Remote Dictionary Server)
* Кэш, очереди, Pub/Sub (для реального времени)
* кэш и быстрый обмен сообщениями
* Используется для кэша, Pub/Sub в реальном времени (чат, игра Pong)
* restart: always — при сбое или перезапуске Docker сервис перезапускается автоматически
* бэкенд может подключаться к Redis (по адресу redis:6379)
* Хранение сессий (если Django настроен использовать Redis для session storage)
* Кэш (Django-кэш / Celery кэш)
* Pub/Sub для чата (если вы используете Django Channels, Redis часто выступает как канал-сервер)
* Кэширование, хранение сессий, Pub/Sub для чата (если используете Django Channels).
* Доступен по адресу redis:6379 в сети
* Webhook (часть Nginx + Django)
* хранение кэш/Pub-Sub
* инструмент управления данными
* in-memory key-value store
* для реализации временных и динамических данных
* может использоваться как база данных, кэш или брокер сообщений
* значения могут быть: string, list, hash, set, sorted sets, bitmap, HyperLogLogs, геопространственные индексы и др  
* высокая скорость, низкие задержки, простой интерфейс
* полностью работает в оперативной памяти, что обеспечивает высокую скорость
  + может сохранять данные на диск, чтобы избежать потери при сбоях
* Поддержка Pub/Sub (механизм публикации/подписки) для обмена сообщениями
* Возможности кэширования с гибким управлением временем жизни данных (TTL)
* Транзакции и скрипты на языке Lua
* Репликация и кластеризация для масштабирования и высокой доступности
* поддерживает множество языков программирования
* Примеры использования:
  + Кэширование данных: Хранение временных данных для ускорения работы приложений
  + Сессии: Управление пользовательскими сессиями в веб-приложениях
  + Очереди сообщений: Использование механизма списков или Pub/Sub для реализации очередей
  + Лидеры и рейтинги: Упорядоченные множества используются для хранения рейтингов в реальном времени
  + Анализ данных: Быстрый подсчёт статистики, временных данных
  + для хранения часто запрашиваемых данных, чтобы не обращаться каждый раз к основной базе (Хранение информации о пользователях (имя, аватар, статус) для быстрого доступа. Кэширование результатов сложных SQL-запросов.)
  + Реализация чата (Совместно с WebSocket для быстрой обработки сообщений. В Pub/Sub позволяет рассылать сообщения всем пользователям чата.)
  + для хранения данных сессий, потому что он работает в оперативной памяти и обеспечивает быструю запись и чтение. (Хранение данных о текущих авторизованных пользователях. Управление сессиями при подключении пользователей к WebSocket.)
  + может работать как очередь задач (Очередь отправки email, push-уведомления. Логирование или обработка фоновых задач (например, обновление статистики матчей))
  + позволяет задавать время жизни (TTL) для хранимых данных, что делает его полезным для временных данных. (Ограничение количества запросов от одного пользователя за определённое время (rate limiting). Хранение временного статуса пользователя: онлайн/офлайн.)
  + для матчей в реальном времени (Хранение текущего состояния игры. Обмен данными между игроками. Хранение лидербордов с помощью упорядоченных множеств (sorted sets))

### db (PostgreSQL)
* Хранение основной информации (профили, чаты, статистика).
* данные (пользователи, чаты, профили и т.п.)
* Официальный образ PostgreSQL
* Создаёт базу данных mydatabase с логином/паролем myuser / mypassword
* СУБД (база данных) для хранения пользователей, сообщений, данных о матчах в Pong, статистики и т.д.
* том db_data, чтобы не терять БД при перезапуске
* Хранит данные приложения (пользователи, чаты, профили, результаты игры и т.д.).
* Доступен внутри сети Docker по адресу db:5432.

### webhook
* webhook/ для автоматического деплоя
  + views.py может принимать сигнал от GitHub (push, pull request), проверять сигнатуру, делать git pull и, например, перезапускать Docker-контейнеры
* позволяет сторонним сервисам или вашим собственным вспомогательным сервисам отправлять уведомления (HTTP-запросы) на ваш сервер, когда у них что-то произошло
* Как только возникает событие, «инициатор» посылает запрос на ваш endpoint
* при определённом событии (подтвердился платёж, пришло важное сообщение, пользователь подтвердил почту и т.п.), сервис (OAuth-сервис, платёжная система, почтовая рассылка, Slack, ...) стучится на заранее заданный URL (webhook) вашего приложения, приложение  узнаёт о событии и может обработать его в реальном времени (обновить базу данных, отправить уведомление пользователю и т.д.)
* используется для микросервисов внутри вашего собственного проекта. Если архитектура Transcendence разбита на несколько сервисов, один сервис может вызвать webhook другого, чтобы сообщить об изменении статуса игрока, старте матча, сообщении в чате и т.д.
* если приложение не может напрямую (или не хочет) постоянно подключаться к внешнему API и проверять статус. Вместо этого внешний API «сам придёт» к вам, когда что-то произойдёт (например, событие в социальной сети, оплата, результат матча в реальном времени)
* При авторизации через 42-интра или GitHub/Google, некоторые из этих сервисов могут предоставлять webhooks или callback, куда они шлют подтверждения
* Если используете микросервисную архитектуру (к примеру, чат на отдельном процессе с WebSocket), webhook может сигнализировать основному приложению о сообщении, которое пришло в чат, или об изменении статуса пользователя
* проект отправляет уведомления о событиях (кто-то подключился к матчу и т.д.) в командный чат в Slack/Discord/Telegram
* если есть внутренняя система подсчёта очков и ведение таблицы лидеров, а внешняя система должна «знать», когда кто-то выиграл матч, можно отправлять соответствующий webhook-запрос
* архитектура по принципу «push-уведомления», а не постоянный опроса
* у нас backend/webhook содержит механизм приёма и обработки webhook-сигналов от GitHub, чтобы автоматизировать процесс деплоя и обновления кода. Происходит автоматическое обновление кода из репозитория. Запускается или пересобирается Docker-контейнер (или несколько контейнеров), чтобы задеплоить новую версию приложения.
  + urls.py Определяет путь webhook/, который будет слушать входящие HTTP-запросы от GitHub (или другого сервиса, который вы настроили на отправку webhook). Все запросы на your-domain.com/webhook/ будут передаваться во views.github_webhook
  + views.py (функция github_webhook), при получении POST-запроса (когда GitHub шлёт уведомление о push-событии или о «ping»):
    - Проверка подписи (X-Hub-Signature-256): GitHub отправляет заголовок с HMAC (подписью), чтобы гарантировать, что запрос действительно от GitHub, а не от злоумышленника. В коде идёт сравнение сигнатуры из запроса и вычисленной сигнатуры на основе SECRET_TOKEN.
    - Определение события (X-GitHub-Event). Если событие — ping, мы отвечаем Pong!, что нужно при первичной настройке вебхука (GitHub проверяет, жив ли endpoint).
    - Если событие — push и ветка — main, запускаются команды git pull, а затем docker-compose up --build -d
* Дополнительный server { ... } с location /webhook перенаправляет запрос на http://backend:8000/webhook.
При этом использует другие сертификаты (/certif/webhook.crt, /certif/webhook.key).
Это нужно, чтобы Github (или иной сервис) мог безопасно слать POST-запросы на https://tr.naurzalinov.me/webhook, а Nginx внутри пробросит его в Django.
* Webhook-запросы также приходят на frontend (на порт 443), который просто проксирует всё на backend
 
### подключить css
* статические файлы Django (table.css) должны быть настроены для загрузки через тег {% static %}
* в начале шаблона (base.html) {% load static %}
  + Без этой строки Django не сможет корректно обработать ссылку {% static ... %}
* backend/settings.py
  ```
  STATIC_URL = '/static/'
  STATICFILES_DIRS = [
      os.path.join(BASE_DIR, '../frontend/static'),  # Путь к статическим файлам
  ]
  ```
* `python manage.py collectstatic` файлы будут скопированы в backend/staticfiles
* `python manage.py findstatic css/popUpChat.css`
* `python manage.py runserver`
* В urls.py добавьте:
  ```
  from django.conf import settings
  from django.conf.urls.static import static
  urlpatterns += static(settings.STATIC_URL, document_root=settings.STATIC_ROOT)
  ```
* Убедитесь, что сервер Django может обслуживать статические файлы. Для локальной разработки добавьте в urls.py обработку статических файлов:
   ```
  from django.conf import settings
  from django.conf.urls.static import static
  if settings.DEBUG:
      urlpatterns += static(settings.STATIC_URL, document_root=settings.STATICFILES_DIRS[0])
  ```
* Если вы используете Docker, убедитесь, что папка frontend/static доступна контейнеру Django. Для этого в docker-compose.yml добавьте frontend/static как volume:
  ```
  backend:
    volumes:
      - ./backend:/app
      - ./frontend/static:/app/frontend/static
  ```
* to activate a virtual environment
* http://localhost:8000/static/css/table.css
* настройки Django
  ```
  INSTALLED_APPS = [
      # ...
      'django.contrib.staticfiles',
      # ...
  ]
  
  STATIC_URL = '/static/'
  
  # Если вы используете нестандартную структуру (например, папка статических файлов во фронтенде),
  # пропишите её здесь. Например:
  import os
  BASE_DIR = os.path.dirname(os.path.dirname(os.path.abspath(__file__)))
  
  STATICFILES_DIRS = [
      os.path.join(BASE_DIR, '..', 'frontend', 'static'),  # путь до папки frontend/static
  ]
  # STATICFILES_DIRS должен указывать на родительскую директорию, которая содержит все ваши статические файлы
  ```
* В режиме разработки (когда DEBUG = True), статика обслуживается самим Django, и обычно достаточно просто запустить python manage.py runserver.
* В продакшене (когда DEBUG = False) вам нужно настроить раздачу статики (например, через collectstatic и веб-сервер Nginx/Apache). Если у вас режим продакшена и вы при этом не сделали collectstatic и не настроили сервер для раздачи статических файлов, то CSS грузиться не будет.
* Откройте Инструменты разработчика (DevTools) → вкладка Network (Сеть).
  + какой путь к CSS-файлу пытается загрузить браузер и какой код ответа (200 или 404)
  + Если 404, значит Django не может найти нужный файл. Убедитесь, что конечный URL, по которому файл запрашивается, совпадает с реальным расположением вашего файла.
* Иногда, если CSS-файл уже кэшировался, браузер может «залипать» на устаревшей версии. Проверьте, обновляется ли версия CSS (можно добавить некий ?v=123 в конце ссылки или просто очистить кэш браузера).
* бэкенд или фронтенд,зависит от архитектуры вашего проекта и того, как именно вы собираетесь отдавать статику (CSS, JS, картинки). нередко делают так:
  + Отдельный фронтенд (React/Vue/Angular) со своей сборкой:
    - логика, стили и ресурсы (в том числе CSS) хранятся и собираются во фронтенде
    - результат сборки (папка build или dist) может отдаёся отдельным сервером (Nginx), либо «прокидываться» через бэкенд, если вы используете Django для статики (но чаще это делают через Nginx)
    - фронтенд — отдельное SPA (React/Vue)
    - в бэкенде только делаете API (Django REST или Channels)
    - CSS лежат в папке фронта
    - после сборки весь CSS окажется в итоговых бандлах (или отдельных .css файлах), и Django будет их отдавать или (чаще) их будет отдавать Nginx
    - бэкенд отдает данные (API)
    - в корне репозитория есть frontend/ (с package.json, src/, node_modules/ и т.п.)
    - отдельное фронтенд-приложение (React, Vue и т.д.)
    - CSS будет находиться внутри этой папки
    - а Django лишь отдаёт или проксирует статику (или вы используете Nginx)
    - settings.py: STATIC_URL = '/static/', STATICFILES_DIRS = [ # ... ]: статика берётся из папки frontend/dist или build
    - фронтенд собирается отдельным билдом, Django подхватывает статические файлы оттуда
    - в Django-шаблоне либо нет упоминания о стилях, либо там просто <div id="root"></div> (для React)
    - стили «приходят» с фронтенда (собранный бандл)
    - Запустите приложение, откройте DevTools в браузере, Network, запрос к файлу .css: http://localhost:3000/... = отдельный дев-сервер фронтенда  
  + Monolith (только Django + шаблоны):
    - не используете отдельный фреймворк (React/Vue и т.п.)
    - шаблоны на Django
    - CSS-файлы, картинки и т. д. в Django-приложении в папке static/
    - Django настроен, чтобы обслуживать файлы из директории static/ (или директорий, указанных в STATICFILES_DIRS)
    - в шаблонах подключаете статику через {% load static %} и href="{% static 'css/myfile.css' %}".
    - Django рендерит HTML-шаблоны
    - нет отдельного SPA
    - CSS проще хранить в backend/static/css/, backend/webapp/static/webapp/css/, ... и подключать через {% static %}
    - не делаете отдельный фронтенд-фреймворк
    - храните CSS в static/ внутри Django-приложения
    - вся верстка и стили лежат прямо в Django-приложении
    - только папка backend/, внутри которой есть папка templates/ и static/
    - settings.py: пути типа os.path.join(BASE_DIR, 'static')
    - статика, включая CSS, лежит на стороне Django
    - в backend/templates/ есть файлы с конструкциями вроде {% load static %} и <link rel="stylesheet" href="{% static 'css/style.css' %}">
    - стили находятся в Django-статике (в папке static/)
    - Запустите приложение, откройте DevTools в браузере, Network, запрос к файлу .css: http://localhost:8000/static/css/... =  раздачей занимается Django
* CSS-файлы и другая статика могут располагаться в разных местах
  + некоторые стили автоматически «приносят» сторонние пакеты (например, Django Admin, Django REST Framework)
  + некоторые — это ваши собственные файлы (например, bootstrap.min.css во фронтенде)
  + когда вы ставите Django, вместе с ним «из коробки» идёт Django Admin и стандартные CSS-файлы для админки
    - Django складывает их в staticfiles/admin/… (или admin/…), либо при выполнении collectstatic они копируются туда автоматически
  + Django REST Framework (DRF) имеет свои статические файлы для browsable API или других админ-панелей.
    - при установке и использовании DRF стили (CSS, JS, изображения) могут оказаться в rest_framework/....
    - Опять же, это автоматически подтягивается, когда вы делаете collectstatic.
  + Ваши собственные стили (frontend)
    - frontend/static/css
    - например, bootstrap.min.css или ваши собственные файлы
    - Если используете React/Vue/Angular — тогда вообще всё может лежать в папке фронтенда, и при сборке результирующие файлы уходят в папку билда (например, build/static/css/...)
  + Если запускаете python manage.py collectstatic, то Django собирает все статические файлы, включая админку и DRF, в одну общую директорию сборки
    - по умолчанию STATIC_ROOT
    - Эта директория может называться backend/staticfiles/ (в зависимости от настроек settings.py)
    - внутри этой единой директории вы увидите подпапки для каждого приложения (admin, rest_framework, etc.) и для ваших собственных стилей, если вы тоже настроили STATICFILES_DIRS
  + Django складывает их либо в подпапках staticfiles/…, либо хранит их отдельно, пока вы не соберёте их в одну директорию
  + разные пакеты = разные CSS
    - каждый пакет мог содержать свои статические ресурсы, не мешаясь с чужими
  + Автоматическая сборка — Django умеет автоматически находить все static/ директории в установленных приложениях и «складывать» их в финальную папку (часто называемую staticfiles или то, что указано в STATIC_ROOT)
  + Архитектурное разделение — если у вас, к примеру, есть ещё и отдельный фронтенд (React), вы можете хранить файлы фронтенда отдельно и вообще отдавать их Nginx-ом без участия Django
  + когда приложение запускается, самое главное — чтобы все нужные CSS-файлы находились/отдавались там, где ожидает браузер
  + физически эти файлы могут лежать хоть в десяти разных местах — при правильных настройках статики Django/Nginx это не проблема
  + Django/collectstatic работает так, чтобы все статические файлы из разных приложений можно было собрать в одном месте или отдать по правильным путям
  + настроить раздачу этих файлов (через Django в DEV, через Nginx или другую службу в PRODUCTION)

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
* Set STATIC_ROOT = the directory from which you’d like to serve these files
  + for example `STATIC_ROOT = "/var/www/example.com/static/"`
* `python manage.py collectstatic` This will copy all files from your static folders into the STATIC_ROOT directory
* Use a web server of your choice to serve the files
  + How to deploy static files covers some common deployment strategies for static files

### index.html
* `{% extends "base.html" %}` расширение базового шаблона
  + Шаблон наследует структуру из базового файла base.html
  + всё из ({% block body %}) будет вставлено в base.html на место тега `{% block body %}`
*  ```
   <div class="container mt-5">                              // контейнер с отступом сверху
    <h1 class="text-center">Привет из Django!</h1>
    <p class="text-center">База данных: {{ db_version }}</p> // Шаблонная переменная {{ db_version }}, значение передаётся в шаблон из представления Django (view)
   </div>
   ```
* `<a href="#" class="btn btn-primary">Нажми меня</a>`  Класс btn btn-primary задаёт стиль кнопки
* ```
  {% if user.is_authenticated %}                             // Шаблонное условие
    <p>Пользователь: <strong>{{ user.username }}</strong> (авторизован)</p> // user - Стандартная переменная, доступная в шаблоне, содержащая информацию о текущем пользователе
  {% else %}
    <p>Вы не авторизованы. Пожалуйста, <a href="/login/">войдите</a> в систему.</p>
  {% endif %}
  ```
* Карточка Bootstrap  

### некоторые файлы
* backend/wenhook/views.py
  + реализация вебхука GitHub в Django, для автоматического обновления кода и перестройки контейнеров при определённых событиях, таких как push в ветку main
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

### Chat
* можно ли делать live chat (с библиотекой channels) или надо целиком писать
  + Какие библиотеки можно использовать?
* Django Channels - библиотека для чата
  + библиотека для вебсокетов
  + подходит для создания чатов, так как WebSocket позволяет передавать данные между сервером и клиентом в режиме реального времени (обмен сообщениями, уведомления)
  + для реализации приложений с функциями реального времени, выходящих за рамки стандартного HTTP-протокола  
  + надстройка для Django 
    - добавляет поддержку протоколов, отличных от HTTP, таких как WebSocket
  + Channel в принципе за WS. На нем же и онлайн игру сделать можно
  + может быть использован для других приложений в реальном времени
    - Онлайн-игры: WebSocket обеспечивает постоянное соединение между клиентом и сервером, что важно для передачи игровых данных в реальном времени
    - Трекинг пользователей в режиме реального времени (отображение онлайн-статуса пользователей)
* RabbitMQ будет использоваться как брокер сообщений, но это больше системные сообщения
  + уведомление о начале турнира, уведомление о добавлении в друзья
* Channels library / RabbitMQ - обсудить водораздел между ними, нужны ли оба
* чат в формате поп-ап окна сбоку
* Do you have a database?
  + If not, you need to set up the database: `python manage.py makemigrations`, `python manage.py migrate`
  + create a superuser `python manage.py createsuperuser`
  + reload the server in Docker
* we implement game logic in backed, backend because we need it to do the multiplayer

### Autorisation
* виды авторизаций: Ecole 42, гугл, логин и пароль с базы данных пользователя
* задача на бэкэнде
* Л. придется разбираться с нюансами DRF и с фронтом
* Для аутентификации вы можно асимметричный алгоритм (например RSA)
  + служба аутентификации — единственная, которая хранит закрытый ключ и может выдавать токены
  + Токены содержат имя пользователя, которого они идентифицируют
  + другие микросервисы могут проверять их с помощью открытого ключа и могут доверять данному имени пользователя, если подпись проверена
  + Самый простой и самый безопасный — использовать файлы cookie
    - Поскольку все микросервисы имеют одно и то же имя хоста, файл cookie будет автоматически отправляться браузером при каждом запросе
    - Со стороны DRF у вас есть расширение для JWT, но мне не удалось заставить его работать с RSA, было проще создать собственное промежуточное программное обеспечение для аутентификации
  + https://dzone.com/articles/using-jwt-in-a-microservice-architecture
* настройка микросервисной архитектуры - планировали использовать JWT в серверной службе DRF
  + удалось добавить несколько токенов, следуя официальной документации
  + возникли трудности с вложением их в nginx сервер, который служит API-шлюзом/обратным прокси-сервером
  + есть ли документация по реализации службы аутентификации как части микросервисной архитектуры?
  + https://django-rest-framework-simplejwt.readthedocs.io/en/latest/getting_started.html
  + https://openclassrooms.com/fr/courses/7192416-mise-en-place-une-api-avec-django-rest-framework/7424720-give-access-with-the-tokens
 + nginx должен быть прозрачным в управлении jtwt
   - он ничего не делает, кроме пересылки запросов
   - хранить jwt: наиболее безопасным способом, не сохраняя его в файле cookie, а в локальном хранилище (только в переменной в js, и мы обновляем как как только он истечет)
* внедрить 2FA через электронную почту
  + Мы выполнили первую часть, генерируя код и все такое
  + были проблемы с последней частью, где мы на самом деле отправляем электронное письмо
    - попробовали sendgrid life, но, не тратя никаких денег, было сложно добиться успеха в отправке электронных писем
  + мы использовали Gmail, самое сложное — найти путь через пользовательский интерфейс Google для создания «mdp приложения» на стороне Django, все легко делается с помощью функции send_mail, документированной по памяти
    - Сначала это пришло как спам, а потом появилось само по себе после нескольких писем
  + Да, со стороны Django это происходит, Gmail они изменили свои условия примерно две недели назад, и с тех пор, насколько я понял, это стало проблемой
* CSRF (Cross-Site Request Forgery) token
  + в рамках авторизации делать, и видимо при post / get запросах?
  + лучше использовать JWT и тогда не будет необходимости в поименении CSRF-токена
  + CSRF token нужно будет реализовывать Л.
  + Его не нужно «реализовывать», он прописан в settings.py проекта. Его нужно отдать клиенту при первом запросе, а потом клиент будет слать запросы к api и вставлять этот токен в запрос. Я бы таки смотрел в сторону JWT.
  + во views.py есть набор проверок которые нужно добавить при запросе к api
  + со стороны Л. - остальная часть в части обмена этим токеном при авторизации
* у меня ошибка 403 на REST API. Я видел в Интернете, что вам нужно использовать токен csrft
  + https://docs.djangoproject.com/en/5.1/howto/csrf/ использование CSRF с Django, Документ Django по всем вопросам аутентификации и глобального использования
* JWT (JSON Web Token) — формат токена для передачи данных между сторонами в безопасном и компактном виде
  + используется для аутентификации, авторизации и обмена информацией в веб-приложениях
  + 1 часть - Header (заголовок)
    - Содержит метаинформацию о токене, например, тип токена и алгоритм подписи
    - ```
      { "alg": "HS256",
        "typ": "JWT" }
      ``` 
  + 2часть: Payload (полезная нагрузка), данные, которые нужно передать (user ID, роль пользователя (admin, user и т.д.), срок действия токена, ...)
    - ```
      { "sub": "1234567890",
        "name": "John Doe",
        "admin": true,
        "exp": 1672531200 }
      ```
  + 3 часть: Signature
    - подписывается с использованием алгоритма из заголовка (например, HS256) и секретного ключа. Это обеспечивает целостность и защиту от изменения токена.
    - ```
      HMACSHA256(
        base64UrlEncode(header) + "." + base64UrlEncode(payload),
        secret
      )
      ```
  + Пример
    ```
    eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9
    .eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkpvaG4gRG9lIiwiaWF0IjoxNTE2MjM5MDIyfQ
    .SflKxwRJSMeKKF2QT4fwpMeJf36POk6yJV_adQssw5c
    ```
  + Аутентификация:
    - Пользователь вводит логин и пароль.
    - Сервер проверяет данные и создаёт JWT, содержащий информацию о пользователе.
    - Токен отправляется клиенту.
  + Авторизация:
    - При каждом запросе клиент отправляет токен (обычно в заголовке Authorization: Bearer <token>)
    - Сервер декодирует токен, проверяет его подпись и использует данные для авторизации (например, проверяет роль пользователя)
  + Токен состоит из Base64-строки, что делает его лёгким для передачи через URL, заголовки и т.д.
  + Безопасность: Использует подпись, чтобы предотвратить изменения данных.
  + Самодостаточность: Все данные для проверки токена уже содержатся в нём, нет необходимости хранить их на сервере.
  + Масштабируемость: Подходит для микросервисов, так как не требует централизованного хранилища сессий.
  + Использование
    - Веб-приложения: Для аутентификации пользователей.
    - API: Для обеспечения безопасного доступа к API-ресурсам.
    - Мобильные приложения: Вместо сессий или cookie.
  + Недостатки
    - Если токен украдён, злоумышленник может использовать его до истечения срока действия.
    - Отзыв токенов сложен, так как они самодостаточны (нет централизованного хранилища).
  + Для снижения рисков:
    - Используйте HTTPS.
    - Устанавливайте короткий срок действия токена (exp).
    - Реализуйте механизмы обновления и отзыва токенов.

###  Organisation
* https://miro.com/app/board/uXjVLAphyh8=/
* https://docs.google.com/document/d/1O1r9jEdxISjMV29lZgLXWNh-bgPzSlnZ6Nr8QuyP_Jc/edit?pli=1
* У нас на всю линейку продуктов от JetBrains бесплатная студенческая лицензия
  + для фронта WebStorm
* git
  + создать dev ветку и работать с ней, потом смерджить в main
  + в dev-ветке создавать суб-ветки под каждую фичу или фикс
    - git checkput -b feature/backend/add_user_model
    - в этой ветке работать именно над конкретной задачей и оформлять в пул реквест.
  + не пушить код большими и хаотичными кусками
  + we do not "git push" directly to main branch - only push to your own branch and after check (we have no tests so it is just double check from your side) - merge your branch with main branch
  + https://befitting-chili-056.notion.site/Branch-management-171a80978370805f8faedeadcb86e35d?pvs=4 
* git
  + клонировать dev
  + `git checkout dev` переключитесь в dev
  + `git checkout -b an` создайте новую ветку для ваших изменений
  + Теперь вы можете работать над своим кодом, добавлять, изменять и удалять файлы в ветке an
  + `git add .`
  + `git commit -m "Добавляет функционал ..."`
  + `git push origin an`
  + Перейдите на GitHub и найдите вашу ветку an
  + Создайте Pull Request для слияния ветки an с dev
    - Добавьте описание изменений в PR, чтобы команда могла понять, над чем вы работали
    - Дождитесь проверки и одобрения от других участников команды
  + `git branch -d an` можете удалить локальную ветку
* the password for the django admin panel ...
* the docker extension in vscode
  + you can launch bash inside a container from gui
* web sockets
  + Django Channels стандартное средство реализации веб-сокетов на базе Django
  + не запрещено в сабжекте
  + реализация собственного асинхронного слоя может быть проблемой
* frontend:
  + Bootstrap toolkit
  + игра сама, картинка
  + assessibility
  + multiple language

### запуск проекта
* в гитигнор добавить 'backend/.env', '.env42'
* https://tr.naurzalinov.me/users/
* http://95.217.129.132:8000/chat/1
  + Секретный ключ от api раз в две недели примерно обновляется
* https://localhost
* 'docker compose up --build'
  - все скачается и распакуется
  - потом в терминале можно Ctrl + C (условное клавиатурное прерывание)
* `sudo ufw status` убедитесь, что 80, 443, и 8000 открыты
* https://localhost:8000 проверка бэкенда
  + 502 Bad Gateway = Nginx не смог подключиться к бэкенду или получил от него некорректный ответ
* `curl http://localhost:8000`
* `docker exec -it git-backend-1 ss -tuln` проверить порты бэкенда
  + `tcp LISTEN 0 511 0.0.0.0:8000 0.0.0.0:*`
* `docker exec -it git-frontend-1 curl http://git-backend-1:8000` подключиться к бэкенду из контейнера
* `docker logs git-backend-1`
* `docker logs git-frontend-1`
* конфигурация Nginx
  + proxy_pass указывает на git-backend-1 и 8000
* заставить работать соединение через 42 на посте кластера, отличном от того, на котором он размещен
  + один способ: каждый раз менять URL-адрес перенаправления, чтобы он соответствовал IP-адресу хост-компьютера 
  + лучшее решение, позволяющее избежать хлопот, особенно с учетом ограничений школьных компьютеров: пройти через виртуальную машину и каждый компьютер использует виртуальную машину, и в ней вы изменяете файл хостов, чтобы связать IP-адрес исходной станции с URL-адресами в бизнесе/проде у вас есть доменные имена, так или иначе связанные, так что это не проблема в вашей голове но я думаю, что вы можете подключиться к другому компьютеру в кластере через его внутреннее доменное имя школы соответствующее тому же названию должности, которую вы используете в штатном расписании но вам придется добавить это в свою конфигурацию nginx, чтобы ваш веб-сервер разрешал связываться с вами в этом домене, а также на стороне django
* в школе
  + порт, который нужен для django, занят
  + поменять номера портов в docker и в nginx

### разное
* Vanilla JS (без фреймворков)
  + фронт реализован «самописно» или с использованием минимальных библиотек (jQuery, Bootstrap и т.д.), но не на SPA-фреймворке
* DPR — Data Processing and Rendering (Обработка и отображение данных) - данные (например, JSON) обрабатываются на серверной стороне (например, с помощью Django) и затем передаются в представление для рендеринга на клиентской стороне с использованием шаблонов и JavaScript
* нет ясности, как между собой взаимодействовать частям приложения
  + нет четкого контракта между фронт/бэк частями
  + Отсюда неясности и с моделями в джанге: какие данные в ней нужны и как их обрабатывать и отдавать
* в модели пользователя я сохраняю аватарки
  + у фронт энда есть требования какие нибудь к аватаркам или они могут сами отрисовывать?
  + вдруг пользователь сохранит свою фотографию 1000 х 1000 пикселей, на фронте вы сможете сами отрисовать аватарку ?
* трекинг когда пользователь был онлайн
  + в models.py добавлю last online для индикатива
  + есть три ключевых решения 
    - через Django sessions - как только юзер делает какое либо действие, Джанго сохраняет в базу данных дату этого действия 
    - через библиотеку channels - по вебсокетам следим пользователь онлайн или нет 
    - через redis - но не понял как это работает. 
* с точки зрения реализации api есть class based views (не сильно сложнее) / functions based views (проще)
* используем ли мы стандартную модель пользователя с кастомизацией или используем свою модель
  + от этого зависит будущая авторизация и как авторизация / permissions будут работать
  + Если стандартную модель - надо перезагрузить все примененные миграции и таблички
* Используем стандартные структуры юзера для авторизации и для моделей данных
* Шаблон = супер базовый html файл с полями кнопками, без разметки
* вы должны предупредить клиента, что его ждет игра, даже если он в чате
* валидация данных
  + js на фронте читает инпут, проверяет с помощью regex
  + функция make password django шифрует на сервере ?
  + бэк ещё раз валидирует (например, проверяет пароль и почту) 
* вы не используете React/Vue/Angular в классическом виде
  + frontend копирует статические файлы в nginx/html/static
  + отсутствию каких-либо билд-шагов Node/npm — 
  + у вас просто набор статических файлов (HTML/CSS/JS), которые Nginx раздаёт напрямую
  + взаимодействие с бэкендом идёт через Django
  + фронтенд написан на чистом JavaScript (Vanilla JS) с использованием Bootstrap для верстки и стилей
  + app.js вызывает fetch к бэкенду (http://backend:8000/api/user-data/) и затем динамически обновляет DOM. Но это ещё не делает приложение SPA-фреймворком — это обычная логика на чистом JS
  + ваша архитектура фронтенда — это HTML/JS/Bootstrap-файлы, которые обслуживает Nginx
  + Django (бэкенд) обрабатывает запросы и отдаёт данные (например, в JSON)
  + подход без какого-либо сборщика или фреймворка для фронта
* update_docker.sh автоматизирует обновление и пересборку Docker-контейнеров (например, docker-compose pull, docker-compose build, docker-compose up -d)
* docker-compose up --build (или update_docker.sh)
  + чтобы пересобрать образы -> Django подхватывает изменения (если настроен hot-reload), фронтенд тоже
* change the post requests to a websocket. The idea was to make the game and chat using websockets (native browser api), it’s beneficial in terms of continuous data streaming
* Сертификаты
  + `.crt`, `.key`, `.csr` необходимы для **обеспечения HTTPS** и шифрования соединения.
  + `etc/nginx/ssl/` Основные сертификаты для сайта (или сайта и API) 
     - `certificate.crt`  
     - `certificate.csr`  
     - `private.key`
  2. `certif/` Отдельные сертификаты для webhook
     - `webhook.crt`  
     - `webhook.csr`  
     - `webhook.key`
  3. `openssl.cnf` конфиг OpenSSL, который используют при создании или подписывании сертификатов (например, когда генерируют CSR — Certificate Signing Request)

### Почему два набора?
В **`nginx.conf`** вы видите несколько `server { ... }` блоков:
- Один (или несколько) для основного сайта/домена (`tr.naurzalinov.me`, `95.217.129.132`) с `ssl_certificate /etc/nginx/ssl/certificate.crt;` и `ssl_certificate_key /etc/nginx/ssl/private.key;`.
- Отдельный блок для **`/webhook`**, у которого свои строки `ssl_certificate /certif/webhook.crt;` и `ssl_certificate_key /certif/webhook.key;`.

Значит, **для webhook-конфигурации** используется **другой** сертификат, лежащий в `certif/`. Часто это делают, если:
1. Вы хотите разделить сертификаты для основного домена и для «поддомена» или отдельного сервиса (webhook).
2. Или если webhook обрабатывается по **другому** доменному имени / другому SSL-сертификату (возможно, самоподписанному).
3. Или если это просто учебная/экспериментальная конфигурация, где решили хранить сертификаты для вебхука отдельно.

---

## 2. Что лежит в `.crt`, `.key` и `.csr`

- **`.crt` (Certificate)** – это **сам** сертификат, выданный CA (Certificate Authority) или сгенерированный самостоятельно (self-signed).  
- **`.key` (Private Key)** – это **приватный ключ**, который хранится только на сервере и никогда не разглашается. Он нужен для расшифровки соединения и подтверждения подлинности.  
- **`.csr` (Certificate Signing Request)** – это **запрос на подпись** сертификата. Обычно вы генерируете `.csr` вместе с приватным ключом локально, затем отправляете `.csr` в удостоверяющий центр (например, Let’s Encrypt, ZeroSSL, GeoTrust и т.д.), а получаете обратно `.crt`.  

Если это «боевые» сертификаты, `.csr` мог остаться как «исторический» файл, показывающий, с помощью чего был запрошен сертификат.

---

## 3. Зачем могут быть *разные* сертификаты в проекте

1. **Разные домены или поддомены**  
   - Основной сайт может обслуживаться, к примеру, на `tr.naurzalinov.me`.  
   - Webhook может идти либо на этот же домен, но другой порт/другую конфигурацию, либо на иной поддомен (например, `webhook.tr.naurzalinov.me`).  
   - Иногда webhook-сертификат самоподписан (особенно если это внутренняя/тестовая штука).

2. **Разделение безопасности**  
   - В некоторых случаях хотят чётко разделять главный TLS-сертификат и сертификат, который используется только для определённой сервисной функциональности (webhook).  
   - Это бывает полезно, если внешний сервис (GitHub) требует отдельного сертификата или политики компании обязывают сегментировать доступ.

3. **Учебная или тестовая настройка**  
   - В учебных проектах (например, Transcendence в Школе 42) студенты могут экспериментировать: один сертификат «боевой», другой — самоподписанный, чтобы отрабатывать механизм webhook’ов, проверки подписи и т.д.

---

## 4. Есть ли ещё сертификаты в проекте?

- Судя по структуре, вы уже видите в `etc/nginx/ssl/` и `certif/` все ключевые файлы.  
- **Возможно**, у вас может быть ещё какой-то self-signed сертификат (например, для локального тестирования) или сертификат для 42 OAuth, если требуется. Но обычно они либо не хранятся вместе с Nginx-конфигурацией, либо вы этого не упоминали.  
- Если у вас есть Docker-окружение, то сертификаты иногда копируются внутрь контейнеров (например, в `/etc/ssl/certs` или `/etc/nginx/ssl`) через Dockerfile или volume.  
- Можно заглянуть в `.env`, `docker-compose.yml` или сами Dockerfile, чтобы увидеть, не подключаются ли какие-то дополнительные сертификаты при сборке.

---

## 5. Итоговое резюме

- **`etc/nginx/ssl/`** – папка, где лежит **основной SSL-сертификат** (и приватный ключ) для основного домена/сайта, который отрабатывает в большинстве ваших `server { listen 443 ssl; }` блоков.  
- **`certif/`** – папка, где лежат **другие сертификаты** (`webhook.crt`, `webhook.key`, `webhook.csr`) для **конкретного server-блока** с `/webhook` или иного поддомена.  
- Такое разделение сертификатов **нормально** и встречается, когда нужно явно разделять несколько доменных имен/служб или когда webhook обслуживается по отдельному каналу.  
- **Дополнительных сертификатов** может и не быть, если вы не настраивали ничего сверх того, что видно на скриншоте. Всё, что нужно для HTTPS и webhook, уже хранится в этих двух папках.
- 
### Tutorials
* как работает бэк, фронт-энд, база данных, зачем API https://www.youtube.com/watch?v=XBu54nfzxAQ
* созданиe REST API на DRF в Pycharm https://blog.jetbrains.com/pycharm/2023/09/building-apis-with-django-rest-framework/
* как работает бэк, фронт, база данных, зачем API https://www.youtube.com/watch?v=XBu54nfzxAQ 
* бэкенд
  + Django Tutorial https://developer.mozilla.org/en-US/docs/Learn/Server-side/Django/Tutorial_local_library_website
  + https://www.djangoproject.com/start/
  + https://www.django-rest-framework.org/tutorial/quickstart/
  + django https://youtube.com/playlist?list=PLA0M1Bcd0w8xZA3Kl1fYmOH_MfLpiYMRs&si=mza4MvfFeRMqgU_B
  + django https://youtube.com/playlist?list=PLA0M1Bcd0w8yU5h2vwZ4LO7h1xt8COUXl&si=ddHMMDnBVPiUuEXy
  + The Browsable API - Django REST frameworkhttps://www.django-rest-framework.org/topics/browsable-api/
* https://docs.djangoproject.com/en/5.1/ref/contrib/auth/

### to do
* pop-up windows : login, chat, profile
* страница comptetition, profile, найтройки
* single-page (SPA): один html
  + этот html меняется с помощью js
  + не надо: server side rendering подход
  + не надо: django html - server-side code
  + не надо: в завимисости от какого-то условия, показываем или нет какие-то части страницы
  + надо: js
  + надо: код внутри {} исполняется в django, если условие выполняется (например, есть есть юзер), он выполняет и заново отправляет html
* написовать стол, чтобы реагировал на клавиатуру и мышку
  + потом туда подсоединим вебсокеты, базовый дизайн
* Б
  + структуры данных
  + Django REST Framework
  + модуль с сокетами и чатом (live chat, уведомления о турнирах, и проч)
  + API, эндпоинты и методы для API
  + live chat
  + rabbitMQ модуль сообщение от сервера
* Л
  + авторизация
  + фронт-энд
  + шаблон для фронт энда
* Ан
  + фронт-энд
  + накидать в Figma шаблоны страничек (часть есть в Миро) страницы следующие - страница с логином, страница с самой игрой (пока без игры), страница с профилем самого пользователя, страница с турниром
  + писать код, делать шаблончики  
  + сайт должен быть одностраничный, один html файл
  + обсудить с Л. структуры страничек, API с бэкэнда
  + помощь на фронт энд, точечные задания 
* К
  + помощь на фронт энд, точечные задания 
* Амин
  + вебсокеты для модуля remote players
    - обмен информацией между игроками и сервером о локации ракетки и мяча
  + логика игры 

### My questions
* settings.py SECRET_KEY
* frontend/etc/private.key
* backend/webhook/migrations/__init__.py пустой
