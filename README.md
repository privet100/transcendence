### do not forget
* remove убрать settings.py SECRET_KEY, frontend/etc/private.key
* close ports 800 and 6800 for outside

### links
* https://github.com/bakyt92/14_ft_transendence
* https://docs.google.com/document/d/14zC4f2D8vdh9cYKosDQxsjWYc9aax2hPGuh8Y7CoENI/edit?tab=t.0
* https://miro.com/app/board/uXjVLAphyh8=/
* https://docs.google.com/document/d/1O1r9jEdxISjMV29lZgLXWNh-bgPzSlnZ6Nr8QuyP_Jc/edit?pli=1
  + https://befitting-chili-056.notion.site/Branch-management-171a80978370805f8faedeadcb86e35d?pvs=4 
* http://127.0.0.1:4444/ basic HTTP connection
* https://127.0.0.1:4443/ HTTPS connection
* http://127.0.0.1:8000/
* design https://www.figma.com/design/aWDJYfDmaeCv2NKJ8bJ15n/FT_Transcendence?node-id=109-32&p=f
* устаревшие
  + http://95.217.129.132:8000/
  + http://95.217.129.132:8000/chat/1
  + http://127.0.0.1:8000/admin/login/?next=/admin/
* **https://tr.naurzalinov.me/users/**
* https://localhost
* https://developer.mozilla.org/en-US/docs/Web/API/WebSocket/WebSocket
* бэк, фронт, база данных, API https://www.youtube.com/watch?v=XBu54nfzxAQ
* REST API на DRF в Pycharm https://blog.jetbrains.com/pycharm/2023/09/building-apis-with-django-rest-framework/
* бэкенд
  + Django Tutorial https://developer.mozilla.org/en-US/docs/Learn/Server-side/Django/Tutorial_local_library_website
  + https://www.djangoproject.com/start/
  + https://www.django-rest-framework.org/tutorial/quickstart/
  + django https://youtube.com/playlist?list=PLA0M1Bcd0w8xZA3Kl1fYmOH_MfLpiYMRs&si=mza4MvfFeRMqgU_B
  + django https://youtube.com/playlist?list=PLA0M1Bcd0w8yU5h2vwZ4LO7h1xt8COUXl&si=ddHMMDnBVPiUuEXy
  + The Browsable API - Django REST frameworkhttps://www.django-rest-framework.org/topics/browsable-api/
* https://docs.djangoproject.com/en/5.1/ref/contrib/auth/

### схема
![1-1](https://github.com/user-attachments/assets/e6a157ea-b278-493e-9649-6a361614deac)
```
Схема Transcendence (с учётом физического размещения):

[Браузер пользователя (HTML, JS, HTTP/HTTPS)]
    |
    v
[SSL/TLS (шифрование для HTTPS)]
    |
    v
[Frontend (Nginx)]
    |
    v
[Backend Container (Daphne + Django)]
    |
    +--> [Django REST API ()]
    |
    +--> [CSRF Token Validation]
    |
    +--> [API Авторизация (École 42)]
```
* Браузер отправляет запросы через HTTP/HTTPS
  + если запрос через HTTPS, данные шифруются с помощью SSL/TLS
* nginx (или другой прокси-сервер)
  + принимает запросы:
    - Если запросы через HTTPS, расшифровывает их с использованием SSL-сертификата
    - Если запросы через HTTP, передаёт в открытом виде 
  + проксирует запросы на бэкенд (Daphne/ASGI) через **HTTP (внутренний трафик не шифруется)**
* HTTP (HyperText Transfer Protocol) — протокол для передачи данных между браузером пользователя и сервером
  + данные передаются в открытом виде, что делает их уязвимыми для перехвата
  + HTTPS (Secure) — с добавлением шифрования через протокол SSL/TLS
* **файл сессии,токен авторизации**
* `proxy_set_header ...` передает заголовки (Host, IP-адрес клиента, протокол, ...), чтобы **бэкенд понимал, откуда пришел запрос**

### протоколы, токены
*  TCP (Transmission Control Protocol)
  + сетевой протокол для передачи данных на порту
  + Протоколы HTTP, Redis, PostgreSQL — прикладные протоколы, определяют, как данные кодируются и интерпретируются
  + данные передаются с помощью транспортного протокола TCP
  + надежность соединения
  + гарантирует доставку пакетов без потерь, в нужном порядке, без дублирования
  + гарантирует управление потоком: отправка данных регулируется, чтобы не перегружать получателя
  + гарантирует контроль ошибок: если пакет теряется или повреждается, он будет переотправлен
  + альтернатива: UDP (User Datagram Protocol), менее надежен, но быстрее
  + Docker по умолчанию использует TCP, так как он подходит для большинства сетевых приложений, например:
    - HTTP, HTTPS, подключение к PostgreSQL, redis  используют TCP
* SSL/TLS 
  + шифровать соединение между браузером и nginx
  + аутентификация сервера (подключаетесь к правильному серверу)
  + защита от **подделки данных**
  + браузер запрашивает установление защищённого **соединения** (HTTPS)
    - используется публичный ключ сервера (SSL-сертификат)
  + **сессионный ключ**, согласуется в процессе установки соединения
  + браузер шифрует данные перед их отправкой на сервер
  + nginx использует сертификат и приватный ключ, чтобы:  
    - подтвердить подлинность **сервера** (**браузер проверяет**, подписан ли сертификат доверенным центром)
    - **установить канал связи** SSL/TLS между сервером и браузером, чтобы шифровать трафик
  + nginx расшифровывает данные с использованием приватного ключа, связанного с SSL-сертификатом
  + nginx передаёт данные на бэкенд без шифрования, по HTTP
  + ответ сервера проходит через Nginx
  + nginx отправляет ответ, зашифрованный для клиента
  + браузер расшифровывает с использованием **сессионного ключа**
  + `ssl_protocols TLSv1.2 TLSv1.3;` версии, TLS 1.1, SSLv3, ... не поддерживаются из соображений безопасности  
  + `ssl_ciphers 'HIGH:!aNULL:!MD5';` использовать надежные шифры, `!aNULL` и `!MD5` исключают небезопасные варианты  
  + `ssl_prefer_server_ciphers on;` браузер отдаёт предпочтение **списку шифров** сервера, а не клиента
  + `etc/nginx/ssl/` сертификаты для основного домена/сайта/приложения, где крутится Transcendence
  + генерируете `.csr` запрос на подпись сертификата (Certificate Signing Request) с приватным ключом локально, отправляете `.csr` в удостоверяющий центр (Let’s Encrypt, ...), получаете `.crt`  
  + `.key` приватный ключ, хранится на **сервере**, не разглашается, нужен для расшифровки соединения и подтверждения подлинности  
  + `.crt` сертификат, выданный CA (Certificate Authority) или сгенерированный самостоятельно (self-signed)  
  + настраивается на стороне Nginx
* CSRF (Cross-Site Request Forgery) token
  + токен **прописан в settings.py**
  + чтобы никто не зашёл в БД
  + отдать клиенту при первом запросе
  + клиент будет слать запросы к **api** и вставлять этот токен в заголовок запроса при post / get запросах
  + views.py набор проверок которые нужно добавить при запросе к api
  + https://docs.djangoproject.com/en/5.1/howto/csrf/
* JWT (JSON Web Token)
  + лучше **JWT**, тогда не будет необходимости в CSRF-токене
  + JWT в **серверной службе **
  + формат токена для передачи данных между сторонами
  + для аутентификации, авторизации, обмена информацией
  + использует подпись, чтобы предотвратить изменения данных
  + все данные для проверки токена содержатся в нём, нет необходимости хранить их на сервере
  + nginx хранит jwt не в cookie, а в **локальном хранилище** (в переменной в js, обновляем как только он истечет)
  + Для аутентификации пользователей Веб-приложения
  + безопасность доступа к API-ресурсам
  + Если токен украдён, злоумышленник может использовать его до истечения срока действия
    - Устанавливайте короткий срок действия токена (exp)
  + Отзыв токенов сложен, так как они самодостаточны (нет централизованного хранилища)
    - Реализуйте механизмы обновления и отзыва токенов
  + Используйте HTTPS
* Идентификация = Кто вы
  + Пользователь вводит логин и пароль
  + Сервер проверяет данные и создаёт **JWT**, содержащий информацию о пользователе
  + токены содержат имя пользователя, которого они идентифицируют
  + Токен отправляется клиенту
  + служба аутентификации хранит закрытый ключ и может выдавать токены
  + другие микросервисы могут проверять их с помощью открытого ключа
  + Самый простой и самый безопасный — **cookie**
    - Поскольку все микросервисы имеют одно и то же имя хоста, файл cookie будет автоматически отправляться браузером при каждом запросе
    - Со стороны  у вас есть расширение для JWT, но мне не удалось заставить его работать с RSA, было проще создать собственное промежуточное программное обеспечение для аутентификации
  + https://dzone.com/articles/using-jwt-in-a-microservice-architecture
* Аутентификация = докажите, что это вы
  + асимметричный алгоритм (например **RSA**)
  + вы заявили свою личность, система требует доказательства (пароль, одноразовый код (OTP) из SMS, отпечаток пальца, лицо
  + у нас: email + password или через 42
  + https://django-rest-framework-simplejwt.readthedocs.io/en/latest/getting_started.html
  + https://openclassrooms.com/fr/courses/7192416-mise-en-place-une-api-avec-django-rest-framework/7424720-give-access-with-the-tokens
* Авторизация = что вам разрешено делать?
  + проверяется, что вы можете делать и к каким ресурсам имеете доступ
  + после входа в систему (идентификации) вы пытаетесь открыть раздел "Администрирование". Система проверяет, есть ли у вас соответствующие права доступа
  + **сещуствующее приложение django?**

### frontend nginx server
* слушает и обрабатывает HTTPS-соединения  
* подписывается на WebSocket-каналы для чата
* обработка WebSocket-запросов
* try using **bolt.new** it's better at frontend
  + the ui is fire here
* папка `frontend/`: сервировка фронта: статика, конфиг Nginx
* proxy_set_header передаёт заголовки (IP клиента, протоколь Host, X-Forwarded-For, ...) для Django
* Пользователь открывает https://tr.naurzalinov.me/
  + приходит запрос к http://95.217.129.132 на контейнер Nginx
  + если запрос к статическому файлу (CSS/JS/картинк/HTML), nginx отдает его из /usr/share/nginx/html/static/
    - certif/ и etc/ для сертификатов, дополнительных конфигураций, скриптов, вспомогательных файлов
  + **Фронтенд вызывает методы API** (для аутентификации, получения/отправки данных)
    - запросы на /api/ идут через Nginx на Django
  + если данные из Django (JSON, HTML, WebSocket), проходят через прокси на порт 8000
  + views.py **проверяет токен/подпись**
  + Django обрабатывает API
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
  
### backend ASGI сервер Daphne
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
  + `CACHES` Django **по умолчанию может кэшировать в памяти, но вы используете Redis**
  + Преимущества Redis
    - Если ваше приложение масштабируется или будет работать в нескольких экземплярах, Redis обеспечит централизованное кэширование.
    - Если кэшированные данные должны пережить перезапуск приложения или контейнера (например, для сессий).
    - Если вам нужно хранить более сложные типы данных в кэше (например, списки, множества, хеши).
    - Если ваш проект работает в распределённой среде, где несколько серверов могут обращаться к одному и тому же кэшированию.
    - Скалируемость, Redis используется для кэширования в распределённых системах, так как данные сохраняются в отдельном процессе/сервисе (Redis-сервере), а не в памяти каждого отдельного экземпляра приложения. Это особенно полезно в многосерверных или контейнеризованных (например, Docker) средах
    - Устойчивость к сбоям: Redis позволяет сохранять кэшированные данные на диск (persistent storage), что делает его более надёжным при сбоях. В отличие от кэширования в памяти, данные в Redis могут быть восстановлены после перезапуска или сбоя.
    - Производительность, Redis быстрый и эффективный инструмент для кэширования, который может поддерживать сложные структуры данных (списки, множества, хеши и другие), что позволяет вам более гибко управлять данными.
    - Поддержка более сложных стратегий кэширования: Redis позволяет реализовывать различные стратегии кэширования, такие как TTL (время жизни), евикция (удаление старых или неиспользуемых данных) и другие.
    - В вашем случае Redis уже используется для channel layers, его использование для кэширования также может быть оправдано, чтобы централизовать все кэшированные данные и улучшить производительность
  + Преимущества кэширования в памяти** (`LocMemCache` в Django)
    - Если ваше приложение работает в пределах одного сервера и не требует масштабируемости.
    - Если вы не ожидаете сильной нагрузки или большого объёма данных.
    - Если вы хотите избежать зависимости от внешних сервисов (например, Redis).
    - Простота: Встроенное кэширование в памяти является самым простым вариантом, не требует дополнительной настройки и внешнего сервиса.
    - Быстродействие: Доступ к данным в памяти быстрее, чем при запросах к внешнему сервису. Это может быть полезно, если приложение работает в пределах одного сервера и вам не нужно хранить кэш между перезапусками.
    - Не требует внешних зависимостей, не требует настройки Redis-сервера или других внешних систем, что упрощает развертывание проекта
    - если вы уберёте настройку CACHES в settings.py, Django будет использовать кэширование в памяти по умолчанию, встроенную память для кэширования, кэш в процессе работы сервера
    - backend django.core.cache.backends.locmem.LocMemCache (кэш в локальной памяти)
    - кэш будет работать быстро, но его данные будут теряться при перезагрузке сервера
  + `INSTALLED_APPS` список приложений: стандартные (admin, auth, sessions), кастомные: `myapp`, `chat`, `api_42`, ...
  + `ROOT_URLCONF` главный файл роутов `myproject/urls.py`, роутинг для всего проекта
  + `TEMPLATES` настройки для системы шаблонов Django (**HTML-шаблоны**, какие контекстные процессоры использовать)
  + `REST_FRAMEWORK` **рендеры**, по умолчанию JSON и **Browsable API**
  + `AUTH_PASSWORD_VALIDATORS` валидаторы сложности паролейл (минимальная длина, отсутствие совпадений с логином, ...)
  + `STATIC_ROOT = staticfiles` Django или система сборки складывает стат. файлы при выполнении `collectstatic` (**не трекается гитом**)
  + `STATIC_URL = '/static/'` стат файлы, в связке с `STATIC_ROOT` это позволяет управлять раздачей CSS, JS, изображений, путь по которому доступна статика в вашем проекте. Обычно используется для указания публичного URL, через который статические файлы (CSS, JavaScript, изображения) будут доступны на сайте. В вашем случае это настроено как:

   ```python
   STATIC_URL = 'static/'
   ```

   Это означает, что статические файлы будут доступны по URL, например, `http://localhost:8000/static/`.

2. **`STATIC_ROOT`** — это физическая директория, в которой будут собираться все статические файлы при запуске команды `collectstatic`. В вашей конфигурации указано:

   ```python
   STATIC_ROOT = os.path.join(BASE_DIR, 'staticfiles')
   ```

   Это путь на сервере, куда Django будет собирать все статические файлы из приложений и приложений, настроенных для работы со статикой. Эта настройка используется, когда вы запускаете команду `python manage.py collectstatic`, чтобы собрать все статические файлы в одном месте для развертывания в продакшн-среде.

### Разница между `STATIC_URL` и `STATIC_ROOT`:
- **`STATIC_URL`** указывает, по какому URL пользователи смогут получить доступ к статическим файлам в браузере.
- **`STATIC_ROOT`** указывает директорию на сервере, куда Django будет собирать все статические файлы при деплое проекта.

### Пример использования:
- **`STATIC_URL = 'static/'`** позволяет пользователю получить доступ к файлам по адресу, например, `http://localhost:8000/static/css/style.css`.
- **`STATIC_ROOT = os.path.join(BASE_DIR, 'staticfiles')`** указывает Django собирать все статические файлы в папке `staticfiles` при запуске команды `collectstatic`.

### Когда это используется:
- **`STATIC_URL`** полезен при разработке, когда вы работаете с статикой в локальной среде и можете напрямую ссылаться на файлы через URL.
- **`STATIC_ROOT`** используется при подготовке проекта к продакшн-среде, чтобы собрать все статические файлы в одну папку, которую затем можно обслуживать через сервер.

В вашем случае оба пути — это настройки для работы с динамическими (в процессе разработки) и статичными (для продакшн-сервера) файлами.

* ./backend:/app локальная backend примонтирована в контейнер как /app: изменения в коде сразу видны внутри контейнера
* manage.py django-утилита для **миграций**, запуска сервера, создания **суперпользователя**, ...
* requirements.txt: **Python**-библиотеки для бэкенда
* трекинг когда пользователь был онлайн
  + models.py: last online для индикатива
  + три решения 
    - через библиотеку channels - по вебсокетам следим пользователь онлайн или нет - НАМ ЭТОТ СПОСОБ
    - через Django sessions - как только юзер делает какое либо действие, Джанго сохраняет в базу данных дату этого действия 
    - через redis - но не понял как это работает
* `CMD ["python", "manage.py", "runserver", "0.0.0.0:8000"]`
  + не запускает Daphne, в продакшн требуется Daphne
  + запускает **встроенный сервер разработки Django**
    - **не поддерживает ASGI или WebSocket**
    - для разработки и тестирования, не для продакшена
  + `runserver`
    - не сможет обрабатывать WebSocket-соединения или другие асинхронные протоколы, такие как **HTTP/2** или **Server-Sent Events**
    - не рассчитан на высокую нагрузку
* REST FRAMEWORK
  + PUT - поменять поле в бд
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
  + URLRouter, ProtocolTypeRouter, маршрутизатор, обработчик ASGI-приложения: какой протокол (HTTP, WebSocket) для обработки запроса
    - "http": django_asgi_app обрабатывает (обычный обработчик Django, **встроенный?**), Django выполняет HTTP-запросы через **встроенный обработчик**
    - "websocket": AllowedHostsOriginValidator(AuthMiddlewareStack(URLRouter(websocket_urlpatterns))) обрабатывает, `websocket_urlpatterns` из `chat/routing.py`: перенаправлять на `ChatConsumer`, обрабатывается через Django Channels
  + DJANGO_SETTINGS_MODULE = mysite.settings **настройки** для компонент Django (**ORM**, middleware, ...)
  + `django_asgi_app = get_asgi_application()` создаёт объект ASGI-приложения, exposes the ASGI callable as a **module-level variable** named `application`

### django в целом
* Бэкенд-фреймворк
* ключевые механики (аутентификация, управление базой данных, админка, API), функции и практики из коробки
* диктует архитектуру (приложения, модели, views, urls, ...)
* `models.py` структура данных, связь между ними (пользователи, профили, чаты, сообщения, статистика игры, ...)
* `url.py` какие функции обрабатывают какие пути, какие запросы обрабатывает по адресу endpoint
* `views.py` функции  для обработки endpoint, обрабатывают логику запросов (отправка сообщения, обновление профиля)
* `admin.py`
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
  + инструменты по защите от распространённых уязвимостей (CSRF, XSS, SQL Injection)
  + благодаря джанговским формам и сериализаторам (в связке с Django REST Framework, если вы его используете), упрощается валидация данных, приходящих от фронтенда
* на базе Python
* `docker exec -it 423c6474989f python manage.py help` список команд django
  + [auth]: changepassword createsuperuser
  + [contenttypes] remove_stale_contenttypes
  + [daphne] runserver
  + [django] check compilemessages createcachetable dbshell diffsettings dumpdata flush inspectdb loaddata makemessages makemigrations   migrate optimizemigration sendtestemail shell showmigrations sqlflush sqlmigrate sqlsequencereset squashmigrations startapp startproject test testserver
  + [rest_framework] **generateschema**
  + [sessions] clearsessions
  + [staticfiles] collectstatic findstatic

### приложения Django app - отдельные модульные приложения внутри проекта
* myapp
  + бизнес-логика пользовательских профилей, турниров, историй игр
  + модель `UserProfile`: инфо о пользователе, друзьях, аватаре, дате последней активности, ...
  + модель `Tournament`: список участников (many-to-many к `UserProfile`)
    - даёт возможность собирать игры в рамках конкретного соревнования, управлять списком участников
  + модель `Game` история матчей, кто участвовал, какой турнир, счёт, победитель
  + `friends = models.ManyToManyField("self")` пользователи могут быть друзьями 
  + расширить профили (добавить рейтинг, биографию, статистику), турнирную логику (сетка турнира, раунды), механику игр (разные типы игр):
     - добавлять поля
     - добавтиь **миграции** в соответствующие модели
* **встроенное `django.contrib.messages`**
  + API для работы с сообщениями
  + интеграция уведомлений в веб-приложения, чтобы взаимодействовать с пользователем
  + для временных сообщений (**flash messages**)
  + сообщения пользователю **между запросами** (об успешной регистрации, показать ошибку при неверном вводе данных, о сохранении изменений)
  + **хранятся в сессии**
    - но можете изменить бэкэнд для хранения сообщений: `settings.py`: `MESSAGE_STORAGE = 'django.contrib.messages.storage.cookie.CookieStorage'`
  + сообщения автоматически удаляются после отображения
  + уровни важности сообщений: `messages.debug`, `messages.info`, `messages.success`, `messages.warning`, `messages.error`
  + работает через HTTP-запросы, добавляя сообщения во время перенаправлений или в шаблоны  
    - В проекте с использованием WebSocket обмен данными происходит в реальном времени через WebSocket соединения, а не через традиционные HTTP-запросы  
  + хранит сообщения временно (в сессии или cookies)
    - это не удобно, если вам нужно отображать историю уведомлений и хранить сообщения для аналитики или повторного использования
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
  + `GET /chat/rooms/` — получить список комнат
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

### django app chat
* приложение с реальным временем на WebSocket
* dj channels библиотека для чата
* API создать через REST: endpoint -> func
* tuto https://channels.readthedocs.io/en/latest/index.html
* js обращается к rest api (post) endpoints /history, /users/, /send
* rest api строит и отдаёт html  
* js получает ответ (get)
* логика в views.py
* с каждым пользователем у бэкенда 2 вебсовета: чат, положение ракетки
* история хранится **в бд**
* ws login new WS connexion
* ws system msg
* ws user communications
* INSTALLED_APPS 'channels'
* CHANNEL_LAYERS 'BACKEND': 'channels.layers.InMemoryChannelLayer', **in-memory ?**
* `asgi.py`
* `python manage.py startapp chat`
* `chat/models.py` модели сообщений и комнат
* `chat/consumers.py` WebSocket consumer 
  ```python
  class ChatConsumer(AsyncWebsocketConsumer):
      async def connect(self):
          self.room_name = self.scope['url_route']['kwargs']['room_name']
          self.room_group_name = f'chat_{self.room_name}'
          await self.channel_layer.group_add(             # Присоединиться к группе комнаты
              self.room_group_name,
              self.channel_name
          )
          await self.accept()
      async def disconnect(self, close_code):
          await self.channel_layer.group_discard(                  # Отключиться от группы комнаты
              self.room_group_name,
              self.channel_name
          )
      async def receive(self, text_data):            # сообщение от WebSocket
          text_data_json = json.loads(text_data)
          message = text_data_json['message']
          await self.channel_layer.group_send(             # Отправить сообщение группе
              self.room_group_name, {
                  'type': 'chat_message',
                  'message': message
              }
          )
      async def chat_message(self, event):       # Получение сообщения от группы
          message = event['message']
          await self.send(text_data=json.dumps({            # Отправить сообщение в WebSocket
              'message': message
          }))
  ```
* `chat/routing.py маршруты WebSocket 
  ```python
    websocket_urlpatterns = [
        path('ws/chat/<str:room_name>/', consumers.ChatConsumer.as_asgi()),
    ]
  ```
* `chat/urls.py` настройте маршруты для комнаты чата
  ```python
  urlpatterns = [
      path('<str:room_name>/', views.chat_room, name='chat_room'),
  ]
  ```
* `chat/views.py представление
  ```python
  def chat_room(request, room_name):
      return render(request, 'chat/room.html', {'room_name': room_name})
  ```
* `python manage.py makemigrations`, `python manage.py migrate` создайте и примените миграции для моделей 
* можно ли делать live chat с библиотекой channels или надо целиком писать
  + Какие библиотеки можно использовать?
* Django Channels
  + библиотека, надстройка Django для вебсокетов
  + добавить поддержку протоколов, отличных от HTTP (WebSocket, ...)
  + для приложений с функциями реального времени, выходящих за рамки стандартного HTTP-протокола: онлайн-игра, онлайн-статус пользователей
  + WebSocket - постоянное соединение между клиентом и сервером
  + WebSocket - передавать данные между сервером и клиентом в режиме реального времени
  + подходит для чата
* **RabbitMQ**
  + брокер сообщений
  + для системных сообщений (уведомление о начале турнира, уведомление о добавлении в друзья)_
  + **нужны ли оба?** Channels library / RabbitMQ 
* чат в формате поп-ап окна сбоку
* to set up the database **для чего?**
  + `python manage.py makemigrations`, `python manage.py migrate`
  + create a superuser `python manage.py createsuperuser`
  + reload the server in Docker
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
* CSS-OM = дереао как DOM
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

### F12 concole
* лучше всего в chrome
* colsole.log для отладки
* bootstrap готовые стили
  - можно сосдавать кастомные на основе н их
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
* WSGI, Web Server Gateway Interface
  + интерфейс между веб-сервером и веб-приложениями/фреймворками, используемый Django-приложениями
  + стандарт для взаимодействия между веб-серверами и Python-приложениями
    - в Python существует большое количество фреймворков, тулкитов, библиотек, для каждого из них собственный метод установки и настройки, они не умеют взаимодействовать между собой
  + WSGI-сервер передаёт данные о запросе Python-приложению
  + Python-приложение обрабатывает запрос и возвращает ответ
  + WSGI-приложение должно:
    - callable объект (обычно это функция или метод)
    - два параметра: словарь переменных окружения и обработчик запроса (start_response)
    - вызывать обработчик запроса с кодом HTTP-ответа и HTTP-заголовками
    - возвращать итерируемый объект с телом ответа
  + поддерживает только синхронные запросы HTTP
  + не подходит для реального времени или протокола WebSocket
  + всё ещё нужен в некоторых случаях
    - если работаете с инструментами или средами, которые изначально поддерживают только синхронные HTTP-запросы - многие устоявшиеся инструменты и сервисы (Gunicorn, uWSGI) поддерживают только WSGI
    - некоторые старые библиотеки или middleware могут работать только в синхронном режиме
    - для стандартных Django-приложений без асинхронных функций WSGI стабильнее
    - уменьшает сложность, если вам не нужны возможности ASGI
    - если используете только HTTP-запросы (API, стандартные страницы, ...)
* ASGI, Asynchronous Server Gateway Interface
  + Daphne хорошо интегрируется с Django Channels и поддерживает WebSocket из коробки
  + продолжение WSGI для приложений, которым нужно обрабатывать асинхронные задачи
  + ASGI-сервер передаёт синхронные HTTP-запросы стандартным Django-приложением
  + ASGI-сервер обрабатывает асинхронные-запросы (WebSocket) через Django Channels
    - Django работает с библиотекой Django Channels, чтобы обрабатывать WebSocket
  + для приложений, работающих в реальном времени (чат, уведомления, игры)
  * Daphne ASGI-сервер, хорошо работающий с Django Channels
* `ASGI_APPLICATION = "myproject.asgi.application"`
  + модуль и переменная, содержащая точку входа для ASGI-сервера
  + `daphne myproject.asgi:application` = запускаете приложение с помощью ASGI-сервера
    - переменная `application` = ASGI-приложение, которое обрабатывает входящие запросы
    - Переменная `application` = точка входа для ASGI-сервера
    - ASGI-приложение объединяет Django и Channels
  + явно не используется в коде
    - ASGI-сервер загружает модуль и использует `application` для обработки запросов
    - сервер ищет  в `settings.py модуль `myproject.asgi` и внутри него переменную `application` 
  + если не настроить `ASGI_APPLICATION`
    - ASGI-серверу придётся указывать путь к точке входа (например, через параметры запуска)
    - некоторые функции (Channels), могут перестать работать, так как они используют эту настройку для маршрутизации WebSocket-запросов
  + asgi.py использует AllowedHostsOriginValidator, который проверяет допустимые хосты для WebSocket-соединений
    - Убедитесь, что ALLOWED_HOSTS включает все домены и IP-адреса, которые вы используете:
    - ALLOWED_HOSTS = ['95.217.129.132', 'localhost', '127.0.0.1', 'tr.naurzalinov.me']
* перехожу на asgi only:
  + стандартные Django-функции (middleware, views, models) продолжают работать, не меняет Django-кода для HTTP
  + удалила файл wsgi.py
  + удалила строку `Gunicorn_APPLICATION = 'myproject.wsgi.application'`
  + в chat/routing.py WebSocket уже настроен
  + nginx поддерживает WebSocket, /ws/ для подключения к WebSocket
    - ```
      location /ws/ {
          proxy_pass http://backend:8000;  # Путь до ASGI-сервера
          proxy_http_version 1.1;
          proxy_set_header Upgrade $http_upgrade;
          proxy_set_header Connection "upgrade";
          proxy_set_header Host $host;
          proxy_set_header X-Real-IP $remote_addr;
          proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
          proxy_set_header X-Forwarded-Proto $scheme;
      }
      ```
  + Тестирование
    - стандартные HTTP-запросы (загрузка HTML-страницы, отправка данных формы)
    - функционал, связанный с WebSocket (чат, ...)
    - клиент может подключиться к ws:// или wss://, сообщения проходят корректно
    - если WebSocket-запросы зависят от Redis (например, для хранения данных о сессиях), убедитесь, что Redis работает и настроен правильно: `redis-cli ping`, Ожидаемый ответ: `PONG`
    - логи Daphne или Redis (соединения WebSocket создаются и работают стабильно?)

### db (PostgreSQL)
* СУБД (база данных) для хранения пользователей, сообщений, данных о матчах в Pong, статистики и т.д.
* Создаёт базу данных mydatabase с логином/паролем myuser / mypassword
* том db_data, чтобы не терять БД при перезапуске
* доступен внутри сети Docker по адресу db:5432

### redis (Remote Dictionary Server) 
* инструмент управления данными
* для:
  + Репликация и кластеризация для масштабирования и высокой доступности
  + Управление пользовательскими сессиями в веб-приложениях, хранение данных сессий, потому что он работает в оперативной памяти и обеспечивает быструю запись и чтение (Хранение данных о текущих авторизованных пользователях. Управление сессиями при подключении пользователей к WebSocket.)
    - Хранение сессий (если Django настроен использовать Redis для session storage)
  + Использование механизма списков или Pub/Sub для реализации очередей сообщений
  + Лидеры и рейтинги: Упорядоченные множества используются для хранения рейтингов в реальном времени
  + Быстрый подсчёт статистики, временных данных
  + Реализация чата (Совместно с WebSocket для быстрой обработки сообщений. В Pub/Sub позволяет рассылать сообщения всем пользователям чата.)
  + очередь задач (отправки email, push-уведомления, логирование, обработка фоновых задач (обновление статистики матчей))
  + для матчей в реальном времени (Хранение текущего состояния игры. Обмен данными между игроками. Хранение лидербордов с помощью упорядоченных множеств (sorted sets))
  + база данных
  + брокер сообщений, быстрый обмен сообщениями
  + хранение Pub/Sub (для реального времени)
    - Pub/Sub для чата
    - если используете Django Channels, Redis выступает как **канал-сервер**
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
  + `channels_redis` управляет подключением и отправкой сообщений в Redis
  + `django-redis` обрабатывает сериализацию данных, их запись и извлечение
  + легко интегрируется с Django через библиотеки `channels_redis` и `django-redis`
* Redis сервер хранение и маршрутизация данных
* **in-memory key-value store**
* значения могут быть: string, list, hash, set, sorted sets, bitmap, HyperLogLogs, геопространственные индексы и др  
* поддерживает множество языков программирования
* Транзакции и скрипты на языке Lua
* работает в оперативной памяти => высокая скорость
  + поэтому важно следить за потреблением памяти
* может сохранять данные на **диск**, чтобы избежать потери при сбоях
* пддержка Pub/Sub (публикации/подписки) => идеальный для каналов WebSocket
* Убедитесь, что сервер Redis защищён (настройка пароля, ограничение доступа, ...)
* Настройте Redis для работы с высоким количеством соединений
* несколько инстансов приложения, они **могут одновременно использовать Redis как общий слой**
* настройки: время жизни кэша (`TIMEOUT`), ...
* Убедитесь, что Redis не перегружен `redis-cli info memory`
* очистить кэш `redis-cli -n 1 flushdb`
* один сервер Redis, в одной инстанции, кэширование и обслуживание channel layers
  + Кэширование использует Redis как хранилище данных в памяти с быстрым доступом
  + Channel Layers использует Redis для обмена сообщениями между экземплярами Django через Django Channels, обеспечивая синхронизацию сообщений между ними
  + два процесса на одном сервере Redis, с разными ключами и настройками
* конфиг `settings.py`
* **остаётся, когда остальные выключились**

#### Redis для кэширования
* bakyt: только кэш сообщений, чат и **системные**
* для хранения часто запрашиваемых данных (сессий, других временных данных, которые необходимо быстро извлекать), помогает повысить производительность
  + Кэширование, хранение часто запрашиваемых данных, временных данных для ускорения работы приложений (инфо о пользователях (имя, аватар, статус, результатов сложных SQL-запросов)
    + гибким управлением временем жизни данных (TTL) (**Django-кэш**)
    +  задавать время жизни (TTL) для хранимых данных. (Ограничение количества запросов от одного пользователя за определённое время (rate limiting). Хранение временного статуса пользователя: онлайн/офлайн.
* `CACHES`, которая настроена на использование **`django_redis`** в качестве backend для кэширования
*`BACKEND` = `django_redis.cache.RedisCache` говорит Django использовать Redis в качестве кэша
* Django сохраняет временные данные (кэш) в Redis вместо локальной памяти или файловой системы
* `OPTIONS` = `CLIENT_CLASS` настраивает использование `DefaultClient` из библиотеки `django-redis` для взаимодействия с Redis.
* сохранять результаты частых запросов (сложные вычисления, данные из базы) и извлекать их из Redis, что быстрее, чем заново выполнять запросы или расчёты
* хранение пользовательских сессий, что ускоряет работу сессий в распределённых системах
* Django может кэшировать рендеринг HTML-шаблонов, чтобы повторные запросы быстрее обрабатывались
* можно кэшировать результаты REST API-запросов, чтобы не нагружать базу данных повторными запросами
* Убедитесь, что кэширование настроено правильно, чтобы извлечь максимальную выгоду
* ускорить доступ к часто используемым данным
* снизить нагрузку на базу данных
* хранит кэшированные данные в оперативной памяти
* ускорения запросов: вместо того чтобы запрашивать данные из базы каждый раз, результаты можно кэшировать
* кэширование сеансов: хранилище сеансов для авторизованных пользователей
* кэширование часто используемых API-ответов (данных профиля пользователя, данных статистики)
* Django использует этот кэш для данных, которые часто запрашиваются, но редко изменяются
* настройка `CACHES` в `settings.py` 
  + настройка интегрируется благодаря библиотеке **`django-redis`**
    - она берёт на себя работу с Redis, предоставляя **Django-friendly API**
  + `LOCATION` = местоположение сервера Redis 
  + класс клиента определяет, как Django взаимодействует с Redis
  
#### у нас redis для `CHANNEL_LAYERS`
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
* для функций реального времени: чата, игровой механики
* Браузер пользователя (JavaScript **WebSocket API**) ?
* сервер и клиент обмениваются данными в реальном времени
* создание 
  + клиент загружает страницу чата или игровой комнаты
  + клиентский код js инициирует соединение через **WebSocket API**: `const socket = new WebSocket("ws://example.com/ws/chat/room1/");`
  + daphne устанавливает WebSocket-соединение, **передавая его в ASGI-приложение**
  + запрос от бразуреа -> nginx -> daphne -> через механизм Channels -> Django consummer (обработчик соединений, класс для обработки WebSocket-сообщений)
    ```json
    {
      "type": "chat.message",
      "message": "Hello, World!"
    }
    ```
    ```python
    class ChatConsumer(AsyncWebsocketConsumer):
      async def connect(self):
        self.room_name = self.scope["url_route"]["kwargs"]["room_name"]
        self.room_group_name = f"chat_{self.room_name}"
        await self.channel_layer.group_add(self.room_group_name, self.channel_name)
        await self.accept()
      async def disconnect(self, close_code):
        await self.channel_layer.group_discard(self.room_group_name, self.channel_name)
      async def receive(self, text_data):
        text_data_json = json.loads(text_data)
        message = text_data_json["message"]
        await self.channel_layer.group_send(
          self.room_group_name, {"type": "chat.message", "message": message}
        )
      async def chat_message(self, event):
        message = event["message"]
        await self.send(text_data=json.dumps({"message": message}))
     ```
  + от сервера к клиенту:
    ```json
    {
      "type": "chat.message",
      "message": "Hi there!"
    }
    ```
* Django Channels
  + не сервер, не принимает запросы от клиента 
  + Django настроено через Channels
  + расширение Django для обработки асинхронных протоколов (WebSocket), для управления асинхронными соединениями внутри Django
  + обработка соединений внутри приложения 
  + Consumers обрабатывают каждое соединение и сообщения, отправленные клиентом
  + `chat/routing.py`
    - для **подключения WebSocket-маршрутов к приложению**
    - ```
      from . import consumers
      websocket_urlpatterns = [
          re_path(r"ws/chat/(?P<room_name>\w+)/$", consumers.ChatConsumer.as_asgi()),
      ]
      ```
    - связывает URL, по которым поступают запросы, с обработчиками Consumers
    - запросы, начинающиеся с `ws/chat/`
    - `<room_name>` переменная, динамически извлекается из URL, в данном случае любое слово (`\w+`): `ws/chat/room1/`, `room_name = "room1"`
    - `consumers.ChatConsumer.as_asgi()` связывает запросы с Consumer-классом `ChatConsumer`, `as_asgi()` позволяет Consumer работать в асинхронной среде 
  + канальный слой (Channel Layers) для взаимодействия между обработчиками (Consumers) и для передачи сообщений между процессами
   - **Redis = backend для Channel Layers**

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
  INSTALLED_APPS = [ ...  'django.contrib.staticfiles', ... ]
  STATIC_URL = '/static/'
  
  # Если вы используете нестандартную структуру (например, папка статических файлов во фронтенде), пропишите её здесь. Например:
  import os
  BASE_DIR = os.path.dirname(os.path.dirname(os.path.abspath(__file__)))
  STATICFILES_DIRS = [
      os.path.join(BASE_DIR, '..', 'frontend', 'static'),  # путь до папки frontend/static
  ]
  # STATICFILES_DIRS = родительская директория, которая содержит статические файлы
  ```
* В режиме разработки (DEBUG = True), статика обслуживается самим Django
  + обычно достаточно просто запустить python manage.py runserver
* В продакшене (DEBUG = False)
  + настроить раздачу статики через collectstatic и веб-сервер Nginx
  + сделать collectstatic
  + настроить сервер для раздачи статических файлов
* Инструменты разработчика (DevTools) → вкладка Network
  + какой путь к CSS-файлу пытается загрузить браузер
  + какой код ответа (200 или 404)
    - если 404, то Django не может найти нужный файл
    - Убедитесь, что конечный URL, по которому файл запрашивается, совпадает с расположением файла
* Иногда, если CSS-файл уже кэшировался, браузер может «залипать» на устаревшей версии
  + проверьте, обновляется ли версия CSS (можно добавить некий ?v=123 в конце ссылки или очистить кэш браузера)
* бэкенд или фронтенд, зависит от архитектуры вашего проекта и того, как именно вы собираетесь отдавать статику (CSS, JS, картинки). нередко делают так:
  + Отдельный фронтенд (React/Vue/Angular) со своей сборкой:
    - логика, стили и ресурсы (в том числе CSS) хранятся и собираются во фронтенде
    - результат сборки (папка build или dist) может отдаёся отдельным сервером (Nginx), либо «прокидываться» через бэкенд, если вы используете Django для статики (но чаще это делают через Nginx)
    - фронтенд — отдельное SPA (React/Vue)
    - в бэкенде только делаете API (Django REST или Channels)
    - CSS лежат в папке фронта
    - после сборки CSS окажется в итоговых бандлах (или отдельных .css файлах)
    - Django будет их отдавать или (чаще) их будет отдавать Nginx
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
  + Monolith (Django + шаблоны):
    - не используете отдельный фреймворк (React/Vue и т.п.)
    - шаблоны на Django
    - CSS-файлы, картинки, .... в Django-приложении в папке static/
    - Django настроен, чтобы обслуживать файлы из директории static/ (или директорий, указанных в STATICFILES_DIRS)
    - в шаблонах подключаете статику через {% load static %} и href="{% static 'css/myfile.css' %}"
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
  + когда вы ставите Django, вместе с ним из коробки идёт Django Admin и стандартные CSS-файлы для админки
    - Django складывает их в staticfiles/admin/… (или admin/…)
    - при выполнении collectstatic они копируются туда автоматически
  + Django REST Framework (DRF) имеет свои статические файлы для **browsable API** или других админ-панелей
    - при установке и использовании DRF **стили (CSS, JS, изображения) могут оказаться в rest_framework/...**
    - это автоматически подтягивается по **`python manage.py collectstatic`**
  + Ваши собственные стили (frontend)
    - frontend/static/css
    - например, bootstrap.min.css или ваши собственные файлы
    - Если используете React/Vue/Angular — тогда вообще всё может лежать в папке фронтенда, и при сборке результирующие файлы уходят в папку билда (например, build/static/css/...)
  + `python manage.py collectstatic` Django собирает все статические файлы, включая админку и DRF, в общую директорию сборки
    - по умолчанию STATIC_ROOT
    - Эта директория может называться backend/staticfiles/ (в зависимости от настроек settings.py)
    - внутри этой директории подпапки для каждого приложения (admin, rest_framework, ...) и для ваших собственных стилей, если вы тоже настроили STATICFILES_DIRS
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
*  `STATIC_ROOT = "/var/www/example.com/static/"`where you’d like to serve these files
* `python manage.py collectstatic` copy all files from your static folders into the STATIC_ROOT
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
* **Карточка Bootstrap**  

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
* `sudo ufw status` убедитесь, что 80, 443, 8000 открыты
* https://localhost:8000 проверка бэкенда
  + 502 Bad Gateway = Nginx не смог подключиться к бэкенду или получил от него некорректный ответ
* `curl http://localhost:8000`
* `docker exec -it git-backend-1 ss -tuln` проверить порты бэкенда
  + `tcp LISTEN 0 511 0.0.0.0:8000 0.0.0.0:*`
* `docker exec -it git-frontend-1 curl http://git-backend-1:8000` подключиться к бэкенду из контейнера
* `docker logs git-backend-1`
* `docker logs git-frontend-1`
* через 42 на посте кластера, отличном от того, на котором он размещен
  + 1 способ: каждый раз менять URL-адрес перенаправления, чтобы он соответствовал IP-адресу хост-компьютера 
  + 2 саособ лучше:
    - каждый компьютер использует VM, в ней вы изменяете файл хостов, чтобы связать IP-адрес исходной станции с URL-адресами
    - подключиться к другому компьютеру в кластере через его внутреннее доменное имя школы, добавить это в nginx.conf, чтобы веб-сервер разрешал связываться с вами в этом домене, а также на стороне django

### разное
* Vanilla JS (без фреймворков)
  + фронт самописный или с использованием минимальных библиотек (jQuery, Bootstrap, ...), не на SPA-фреймворке
* DPR, Data Processing and Rendering (Обработка и отображение данных) - данные (JSON, ...) обрабатываются на серверной стороне с помощью Django и затем передаются в представление для рендеринга на клиентской стороне с использованием шаблонов и JavaScript
* в модели пользователя сохраняю аватарки
  + у фронт энда есть требования какие нибудь к аватаркам или они могут сами отрисовывать?
  + вдруг пользователь сохранит свою фотографию 1000 х 1000 пикселей, на фронте вы сможете сами отрисовать аватарку ?
* с точки зрения реализации api есть **class based views** (не сильно сложнее) / functions based views (проще)
* Используем **стандартные структуры юзера для авторизации и для моделей данных**
* предупредить клиента, что его ждет игра, даже если он в чате
* валидация данных
  + **js на фронте читает инпут, проверяет с помощью regex**
  + функция make password django шифрует на сервере ?
  + бэк ещё раз валидирует (проверяет пароль и почту, ...) **зачем два раза** 
* update_docker.sh обновление и пересборку Docker-контейнеров (docker-compose pull, docker-compose build, docker-compose up -d)
* docker-compose up --build (или update_docker.sh)
  + чтобы пересобрать образы -> Django подхватывает изменения (если настроен hot-reload), фронтенд тоже
* change the post requests to a websocket. The idea was to make the game and chat using websockets (native browser api), it’s beneficial in terms of continuous data streaming
* `frontend/static/app.js`
  + вызывает fetch к бэкенду и затем динамически обновляет DOM, это не делает приложение SPA-фреймворком — это обычная логика на чистом JS
  + связь между бекендом и фронтендом
  + получать информацию о пользователе (имя, статус, аватар, результаты игр)
  + динамическое обновления пользовательского интерфейса с использованием данных, полученных от бэка 
  + `fetch('http://backend:8000/api/user-data/')`: HTTP-запрос к эндпоинту бекенда `/api/user-data/`, получить данные JSON о пользователе
  + `response => response.json()` конвертирует ответ JSON от сервера в объект JavaScript
  + после получения данных пользовательский интерфейс (UI) (отображение имени пользователя, аватарка, настройки) обновляется без перезагрузки страницы
  + `http://backend:8000` лушче хранить в перменной окружения
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
  + Docker кэширует каждый слой (каждую команду). Если вы копируете только `requirements.txt` перед установкой зависимостей, то этот шаг (`RUN pip install ...`) будет повторно использовать кэш, если файл `requirements.txt` не изменился.
  - Если вы измените код приложения (например, Python-файлы), установка зависимостей не будет повторяться.
  + Если вы сначала копируете весь код (`COPY . .`), то любые изменения в файлах приложения (например, в коде Python) приведут к тому, что Docker заново выполнит `RUN pip install ...`, даже если `requirements.txt` не изменялся.
  + 1. Кэширование нарушается
   - Любые изменения в любом файле вашего приложения (даже в HTML, CSS или документации) приведут к повторной установке зависимостей.
   - Это замедлит сборку Docker-образа, особенно если ваш `requirements.txt` содержит много пакетов.
  + Увеличение времени сборки: При каждой сборке будут тратиться ресурсы на переустановку всех пакетов, даже если зависимости остались неизменными.
  + Ускоряет сборку благодаря кэшированию,
  + Избегает повторной установки зависимостей без необходимости
* `./context`
  + все файлы и папки из указанной директории будут доступны для Docker в процессе сборки (исходный код, конфигурационные файлы, ...)
  + только те файлы, которые явно указаны в Dockerfile (COPY, ADD, ...), будут скопированы в контейнер
  
### Organisation
* на всю линейку продуктов от JetBrains бесплатная студенческая лицензия, для фронта WebStorm
* the password for the django admin panel ...
* docker vscode extension 
  + launch bash inside a container from gui
* Django Debug Toolbar
  + интерфейс для отслеживания различных аспектов работы проекта, включая middleware

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

### test
* bakyt: Endpoint that are formed from views.py from different folders
  + `urls.py` связывает эндпоинты (URL-маршруты) с функциями/классами представлений из `views.py`
  + просматривать `views.py` в каждом приложении: какие представления и какие URL ассоциированы с функциями или классами в разных частях проекта
* Если используется DRF - автоматичесая документация эндпоинтов
  + Browsable API (встроенная документация): в `http://localhost:8000/api/` или `http://localhost:8000/` список эндпоинтов
* если подключены библиотеки для документирования API, то `http://localhost:8000/swagger/` или `http://localhost:8000/redoc/`
* Postman: импортируйте коллекцию эндпоинтов, если она уже создана  
* использовать Postman для изучения API, отправляя запросы на `/api/`, `/swagger/`, ... и исследуя доступные маршруты
* test endpoints HTTP (API или страницы) with Postman:
  + Введите адрес вашего сервера, например:
     - `http://localhost:8000/api/endpoint/`
     - `https://example.com/api/endpoint/`
  + метод (GET, POST, PUT, DELETE и т. д.).
  + если требуется авторизация, добавьте токен или данные пользователя (если используете `Token` или `JWT`).
  + отправьте запрос и проверьте статус ответа (200 OK, 401 Unauthorized и т.д.) и тело ответа
* test endpoints HTTP (API или страницы) with Curl:
  + `curl -X GET http://localhost:8000/api/endpoint/`
  + `curl -X POST http://localhost:8000/api/endpoint/ -H "Content-Type: application/json" -d '{"key": "value"}'`
* test endpoints HTTP (API или страницы) with browser:
  + Для эндпоинтов, которые возвращают HTML (главная страница, панель администратора), просто откройте браузер и введите URL
* test endpoints Websockets with Postman
  + меню `New Request` - `WebSocket`
  + Укажите URL WebSocket-соединения:
     - `ws://localhost:8000/ws/chat/room_name/`
     - `wss://example.com/ws/chat/room_name/`
  + Установите соединение и отправьте тестовые сообщения
  +  Посмотрите, возвращает ли сервер ответы.
* test endpoints Websockets with Chrome + расширения [Smart WebSocket Client](https://chrome.google.com/webstore/detail/smart-websocket-client/kzhddgcmkfiimcdlddieeoemkbdmgkag) 
  + Укажите URL WebSocket: `ws://localhost:8000/ws/chat/room_name/`
  + Нажмите «Connect».
  + Отправьте тестовые сообщения и проверьте, получает ли сервер их.
* test endpoints Websockets with Python + библиотека `websockets`
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
* test Redis integration
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
1. Connection from another computer is working (so local network is working) 
2. When Ivan tried to login with 42Auth from another computer (not server) - he got error 400; however basic sign up with email is working. 
3. My login with 42Auth from server computer worked.
