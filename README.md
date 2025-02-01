### TEST
* https://localhost:4443/
* https://localhost:4443/chat: HTTP-запрос для загрузки страницы (HTML, CSS, JavaScript)
* https://localhost:4443/staticfiles/admin/css/base.css
* https://localhost:4443/static/css/popUpChat.css
* wss://localhost:4443/ws/chat/
* wss://localhost:4443/ws/chat/dd/
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
* endpoints: `views.py` в каждом приложении: какие представления и какие URL ассоциированы с функциями или классами в разных частях проекта
* postman
  + импортируйте коллекцию эндпоинтов, если она уже создана  
  + отправляйте запросы на `/api/`, `/swagger/`, ... исследуя доступные маршруты
  + `http://localhost:8000/api/endpoint/` endpoints HTTP (API или страницы)
  + метод (GET, POST, PUT, DELETE и т. д.)
  + если требуется авторизация, добавьте токен или данные пользователя (если используете `Token` или `JWT`)
  + отправьте запрос и проверьте статус ответа (200 OK, 401 Unauthorized, ...) и тело ответа
* django предоставляет встроенные инструменты для тестирования HTTP
* ws-тесты с Django Channels + `pytest`
* F12
* если подключены библиотеки для документирования API, то `http://localhost:8000/swagger/` или `http://localhost:8000/redoc/`
* `docker logs backend`
  + Exception while resolving variable **'name'** in template 'unknown'.
    - File "/usr/local/lib/python3.10/site-packages/django/core/handlers/exception.py", line 55, in inner response = get_response(request)
    - File "/usr/local/lib/python3.10/site-packages/django/core/handlers/base.py", line 181, in _get_respons callback, callback_args, callback_kwargs = self.resolve_request(request)
    - File "/usr/local/lib/python3.10/site-packages/django/core/handlers/base.py", line 313, in resolve_request resolver_match = resolver.resolve(request.path_info)
    - File "/usr/local/lib/python3.10/site-packages/django/urls/resolvers.py", line 705, in resolve raise Resolver**404**({"tried": tried, "path": new_path})
  + django.urls.exceptions.Resolver**404**: {'tried': [[<URLResolver <URLPattern list> (admin:admin) 'admin/'>], [<URLResolver <module 'auth_app.urls' from '/app/auth_app/urls.py'> (None:None) 'auth/'>], [<URLResolver <module 'chat.urls' from '/app/chat/urls.py'> (None:None) 'chat/'>], [<URLResolver <module 'myapp.urls' from '/app/myapp/urls.py'> (None:None) ''>, <URLPattern '' [name='index']>], [<URLResolver <module 'auth_app.urls' from '/app/auth_app/urls.py'> (None:None) ''>, <URLPattern 'callback/' [name='callback']>], [<URLResolver <module 'auth_app.urls' from '/app/auth_app/urls.py'> (None:None) ''>, <URLPattern 'logout/' [name='logout_view']>], [<URLResolver <module 'auth_app.urls' from '/app/auth_app/urls.py'> (None:None) ''>, <URLPattern 'login/' [name='loginemail']>], [<URLResolver <module 'auth_app.urls' from '/app/auth_app/urls.py'> (None:None) ''>, <URLPattern 'auth/email/' [name='authemail']>], [<URLResolver <module 'auth_app.urls' from '/app/auth_app/urls.py'> (None:None) ''>, <URLPattern 'signup/' [name='signup']>], [<URLResolver <module 'auth_app.urls' from '/app/auth_app/urls.py'> (None:None) ''>, <URLPattern 'auth/callback' [name='oauth_callback']>], [<URLResolver <module 'auth_app.urls' from '/app/auth_app/urls.py'> (None:None) ''>, <URLPattern 'profile/' [name='profile']>], [<URLPattern 'user_42/<int:user_id>/' [name='get_user_42']>], [<URLPattern 'users_42/' [name='get_all_users_42']>], [<URLPattern 'users/' [name='get_all_userprofiles']>], [<URLPattern 'user/<int:id>/' [name='user-detail']>], [<URLPattern 'tour/<int:id>/' [name='tournament-detail']>], [<URLPattern 'game/<int:id>/' [name='game-detail']>]], 'path': 'ws/chat/rr/'}
  + During handling of the above exception, another exception occurred: File "/usr/local/lib/python3.10/site-packages/django/template/base.py", line 883, in _resolve_lookup  current = current[bit]
  + TypeError: **'URLResolver' object is not subscriptable**
  + During handling of the above exception, another exception occurred: File "/usr/local/lib/python3.10/site-packages/django/template/base.py", line 893, in _resolve_lookup current = getattr(current, bit)
  + AttributeError: 'URLResolver' object has no attribute **'name'**
  + During handling of the above exception, another exception occurred: File "/usr/local/lib/python3.10/site-packages/django/template/base.py", line 899, in _resolve_lookup current = current[int(bit)]
  + ValueError: invalid literal for int() with base 10: **'name'**
  + During handling of the above exception, another exception occurred: File "/usr/local/lib/python3.10/site-packages/django/template/base.py", line 906, in _resolve_lookup raise VariableDoesNotExist(
  + django.template.base.VariableDoesNotExist: Failed lookup for key [name] in <URLResolver <URLPattern list> (admin:admin) 'admin/'>
  + Exception while **resolving variable 'name' in template 'unknown'**
    - File "/usr/local/lib/python3.10/site-packages/django/core/handlers/exception.py", line 55, in inner response = get_response(request)
    - File "/usr/local/lib/python3.10/site-packages/django/core/handlers/base.py", line 181, in _get_response callback, callback_args, callback_kwargs = self.resolve_request(request)
    - File "/usr/local/lib/python3.10/site-packages/django/core/handlers/base.py", line 313, in resolve_request resolver_match = resolver.resolve(request.path_info)
    - File "/usr/local/lib/python3.10/site-packages/django/urls/resolvers.py", line 705, in resolve raise Resolver**404**({"tried": tried, "path": new_path})
  + django.urls.exceptions.Resolver**404**: {'tried': [[<URLResolver <URLPattern list> (admin:admin) 'admin/'>], [<URLResolver <module 'auth_app.urls' from '/app/auth_app/urls.py'> (None:None) 'auth/'>], [<URLResolver <module 'chat.urls' from '/app/chat/urls.py'> (None:None) 'chat/'>], [<URLResolver <module 'myapp.urls' from '/app/myapp/urls.py'> (None:None) ''>, <URLPattern '' [name='index']>], [<URLResolver <module 'auth_app.urls' from '/app/auth_app/urls.py'> (None:None) ''>, <URLPattern 'callback/' [name='callback']>], [<URLResolver <module 'auth_app.urls' from '/app/auth_app/urls.py'> (None:None) ''>, <URLPattern 'logout/' [name='logout_view']>], [<URLResolver <module 'auth_app.urls' from '/app/auth_app/urls.py'> (None:None) ''>, <URLPattern 'login/' [name='loginemail']>], [<URLResolver <module 'auth_app.urls' from '/app/auth_app/urls.py'> (None:None) ''>, <URLPattern 'auth/email/' [name='authemail']>], [<URLResolver <module 'auth_app.urls' from '/app/auth_app/urls.py'> (None:None) ''>, <URLPattern 'signup/' [name='signup']>], [<URLResolver <module 'auth_app.urls' from '/app/auth_app/urls.py'> (None:None) ''>, <URLPattern 'auth/callback' [name='oauth_callback']>], [<URLResolver <module 'auth_app.urls' from '/app/auth_app/urls.py'> (None:None) ''>, <URLPattern 'profile/' [name='profile']>], [<URLPattern 'user_42/<int:user_id>/' [name='get_user_42']>], [<URLPattern 'users_42/' [name='get_all_users_42']>], [<URLPattern 'users/' [name='get_all_userprofiles']>], [<URLPattern 'user/<int:id>/' [name='user-detail']>], [<URLPattern 'tour/<int:id>/' [name='tournament-detail']>], [<URLPattern 'game/<int:id>/' [name='game-detail']>]], 'path': 'ws/chat/rr/'}
  + During handling of the above exception, another exception occurred: File "/usr/local/lib/python3.10/site-packages/django/template/base.py", line 883, in _resolve_lookup current = current[bit]
  + TypeError: 'URLResolver' object is not subscriptable
  + During handling of the above exception, another exception occurred: File "/usr/local/lib/python3.10/site-packages/django/template/base.py", line 893, in _resolve_lookup current = getattr(current, bit)
  + AttributeError: 'URLResolver' object has no attribute 'name'
  + During handling of the above exception, another exception occurred: File "/usr/local/lib/python3.10/site-packages/django/template/base.py", line 899, in _resolve_lookup current = current[int(bit)]
  + ValueError: invalid literal for int() with base 10: 'name'
  + During handling of the above exception, another exception occurred: File "/usr/local/lib/python3.10/site-packages/django/template/base.py", line 906, in _resolve_lookup raise VariableDoesNotExist(
  + django.template.base.VariableDoesNotExist: Failed lookup for key [name] in <URLResolver <module 'auth_app.urls' from '/app/auth_app/urls.py'> (None:None) 'auth/'>
  + Exception while resolving variable **'name'** in template 'unknown'.
  + Traceback (most recent call last):
    - File "/usr/local/lib/python3.10/site-packages/django/core/handlers/exception.py", line 55, in inner response = get_response(request)
    - File "/usr/local/lib/python3.10/site-packages/django/core/handlers/base.py", line 181, in _get_response callback, callback_args, callback_kwargs = self.resolve_request(request)
   - File "/usr/local/lib/python3.10/site-packages/django/core/handlers/base.py", line 313, in resolve_request resolver_match = resolver.resolve(request.path_info)
    - File "/usr/local/lib/python3.10/site-packages/django/urls/resolvers.py", line 705, in resolve raise Resolver**404**({"tried": tried, "path": new_path})
  + django.urls.exceptions.Resolver**404**: {'tried': [[<URLResolver <URLPattern list> (admin:admin) 'admin/'>], [<URLResolver <module 'auth_app.urls' from '/app/auth_app/urls.py'> (None:None) 'auth/'>], [<URLResolver <module 'chat.urls' from '/app/chat/urls.py'> (None:None) 'chat/'>], [<URLResolver <module 'myapp.urls' from '/app/myapp/urls.py'> (None:None) ''>, <URLPattern '' [name='index']>], [<URLResolver <module 'auth_app.urls' from '/app/auth_app/urls.py'> (None:None) ''>, <URLPattern 'callback/' [name='callback']>], [<URLResolver <module 'auth_app.urls' from '/app/auth_app/urls.py'> (None:None) ''>, <URLPattern 'logout/' [name='logout_view']>], [<URLResolver <module 'auth_app.urls' from '/app/auth_app/urls.py'> (None:None) ''>, <URLPattern 'login/' [name='loginemail']>], [<URLResolver <module 'auth_app.urls' from '/app/auth_app/urls.py'> (None:None) ''>, <URLPattern 'auth/email/' [name='authemail']>], [<URLResolver <module 'auth_app.urls' from '/app/auth_app/urls.py'> (None:None) ''>, <URLPattern 'signup/' [name='signup']>], [<URLResolver <module 'auth_app.urls' from '/app/auth_app/urls.py'> (None:None) ''>, <URLPattern 'auth/callback' [name='oauth_callback']>], [<URLResolver <module 'auth_app.urls' from '/app/auth_app/urls.py'> (None:None) ''>, <URLPattern 'profile/' [name='profile']>], [<URLPattern 'user_42/<int:user_id>/' [name='get_user_42']>], [<URLPattern 'users_42/' [name='get_all_users_42']>], [<URLPattern 'users/' [name='get_all_userprofiles']>], [<URLPattern 'user/<int:id>/' [name='user-detail']>], [<URLPattern 'tour/<int:id>/' [name='tournament-detail']>], [<URLPattern 'game/<int:id>/' [name='game-detail']>]], 'path': 'ws/chat/rr/'}
  + During handling of the above exception, another exception occurred: File "/usr/local/lib/python3.10/site-packages/django/template/base.py", line 883, in _resolve_lookup current = current[bit]
  + TypeError: 'URLResolver' object is not subscriptable
  + During handling of the above exception, another exception occurred: File "/usr/local/lib/python3.10/site-packages/django/template/base.py", line 893, in _resolve_lookup current = getattr(current, bit)
  + AttributeError: 'URLResolver' object has no attribute **'name'**
  + During handling of the above exception, another exception occurred: File "/usr/local/lib/python3.10/site-packages/django/template/base.py", line 899, in _resolve_lookup current = current[int(bit)]
  + ValueError: invalid literal for int() with base 10: **'name'**
  + During handling of the above exception, another exception occurred: File "/usr/local/lib/python3.10/site-packages/django/template/base.py", line 906, in _resolve_lookup raise VariableDoesNotExist(
  + django.template.base.VariableDoesNotExist: Failed lookup for key [name] in <URLResolver <module 'chat.urls' from '/app/chat/urls.py'> (None:None) 'chat/'>
  + **Not Found: /ws/chat/rr/**
  + "GET /ws/chat/rr/ HTTP/1.1" **404** 5670
  + File /usr/local/lib/python3.10/site-packages/django/contrib/messages/storage/cookie.py first seen with mtime 1738426491.0360327
  + File /usr/local/lib/python3.10/site-packages/django/contrib/messages/storage/session.py first seen with mtime 1738426491.0400329
  + File /usr/local/lib/python3.10/site-packages/django/contrib/messages/storage/fallback.py first seen with mtime 1738426491.0400329
  + File /usr/local/lib/python3.10/site-packages/django/contrib/sessions/serializers.py first seen with mtime 1738426491.1720328
* `docker-compose logs frontend`
  + если нет упоминания GET /ws/chat/…, запрос не доходит
  + если есть, но возвращает **404**/400, значит либо Nginx, либо Channels отказывает
  + 172.21.0.1 - - [01/Feb/2025:16:26:40 +0000] "GET /favicon.ico HTTP/1.1" 404 5670 "https://localhost:4443/" "Chrome" "-"
    - основные заголовки (User-Agent, Referer) и метод/URL
    - 172.21.0.1 IP-адрес клиента (в контейнерной сети может быть внутренний адрес Docker)
    - `-` **идентификация (RFC 1413)**, по умолчанию отсутствует
    - `-` **имя пользователя (Basic Auth)**, по умолчанию отсутствует
    - `GET /favicon.ico HTTP/1.1` request Line: метод запроса, путь, версия протокола
    - 404 код ответа HTTP (Not Found)
    - 5670 объём переданных сервером байт в теле ответа
    - `https://localhost:4443/` URL, откуда пользователь перешёл
  + ```
    /docker-entrypoint.sh: /docker-entrypoint.d/ is not empty, will attempt to perform configuration
    /docker-entrypoint.sh: Looking for shell scripts in /docker-entrypoint.d/
    /docker-entrypoint.sh: Launching /docker-entrypoint.d/10-listen-on-ipv6-by-default.sh
    10-listen-on-ipv6-by-default.sh: info: Getting the checksum of /etc/nginx/conf.d/default.conf
    10-listen-on-ipv6-by-default.sh: info: /etc/nginx/conf.d/default.conf differs from the packaged version
    /docker-entrypoint.sh: Sourcing /docker-entrypoint.d/15-local-resolvers.envsh
    /docker-entrypoint.sh: Launching /docker-entrypoint.d/20-envsubst-on-templates.sh
    /docker-entrypoint.sh: Launching /docker-entrypoint.d/30-tune-worker-processes.sh
    /docker-entrypoint.sh: Configuration complete; ready for start up
    [notice] 1#1: using the "epoll" event method
    [notice] 1#1: nginx/1.27.3
    [notice] 1#1: built by gcc 12.2.0 (Debian 12.2.0-14) 
    [notice] 1#1: OS: Linux 5.15.0-131-generic
    [notice] 1#1: getrlimit(RLIMIT_NOFILE): 1048576:1048576
    [notice] 1#1: start worker processes
    [notice] 1#1: start worker process 28
    [notice] 1#1: start worker process 29
    [notice] 1#1: start worker process 30
    [notice] 1#1: start worker process 31
    172.21.0.1 "GET /                               HTTP/1.1" 200 4644  "Chrome"
    172.21.0.1 "GET /staticfiles/css/popUpChat.css  HTTP/1.1" 404 1882  "https://localhost:4443/" "Chrome"
    172.21.0.1 "GET /favicon.ico                    HTTP/1.1" 404 5670  "https://localhost:4443/" "Chrome"
    172.21.0.1 "GET /staticfiles/admin/css/base.css HTTP/1.1" 200 22092 "Chrome"
    172.21.0.1 "GET /static/css/chat.css            HTTP/1.1" 200 3189  "Chrome"
    172.21.0.1 "GET /static/chat.html               HTTP/1.1" 200 8204  "Chrome"
    172.21.0.1 "GET /static/css/chat.css            HTTP/1.1" 200 3189  "https://localhost:4443/static/chat.html" "Chrome"
    172.21.0.1 "GET /static/chat.html               HTTP/1.1" 200 8204  "Chrome"
    172.21.0.1 "GET /static/css/chat.css            HTTP/1.1" 200 3189  "https://localhost:4443/static/chat.html" "Chrome"
    172.21.0.1 "GET /ws/chat/r/                     HTTP/1.1" 404 5667  "Chrome"
    172.21.0.1 "GET /ws/chat/r                      HTTP/1.1" 404 5664  "Chrome"
    172.21.0.1 "GET /ws/chat                        HTTP/1.1" 404 5658  "Chrome"
  ```
* `docker-compose logs backend`
  + При успешном подключении Channels пишет WebSocket CONNECT /ws/chat/<room_name>/
* F12 → Network, найдите запрос, посмотрите Response Headers
  + если server: nginx, скорее всего это Nginx 404
  + если server: WSGIServer/… или Content-Type: text/html; charset=utf-8 и Traceback HTML от Django — это Django 404
* Nginx и Daphne добавляют заголовки Server: или X-Powered-By
  + если 404 вернул Nginx, в ответе может быть Server: nginx.
  + если 404 вернул Daphne, Server: Daphne или не указывать, нодобавит Content-Type: text/html; charset=utf-8
* дошёл ли ws-запрос до Channels ?
  + если в Nginx GET /ws/chat/... HTTP/1.1" 404…, но Channels молчит, значит Nginx не сделал proxy_pass или сделал как HTTP-запрос без Upgrade
  + channels при успешном ws handshake выводит в логе WebSocket CONNECT /ws/chat/....
* `docker-compose logs redis`
* запрос `GET /ws/chat/rr/` обрабатывается как обычный, не как WebSocket‐handshake - проверить:
  + `frontend "GET /ws/chat/rr/ HTTP/1.1" 404 ...`: js‐код формирует `ws://` или `wss://`, но браузер блокирует/не делает Upgrade
  + консоль `wss://localhost:4443/ws/chat/rr/` OK
  + location `/ws/`: `proxy_pass http://backend:8000;`, проксирует `/ws/chat/` OK
  + nginx передаёт Upgrade‐заголовки `Upgrade: websocket` и `Connection: upgrade` OK
  + браузер делает WebSocket‐handshake
  + F12 Network WS: `101 Switching Protocols` (но `GET /ws/chat/rr/ 404` значит браузер не отправляет Upgrade или Nginx вырезает заголовок)
  + `backend Not Found: /ws/chat/rr/` django не находит `/ws/chat/rr/` в `urlpatterns` => 404
  + log Daphne `WebSocket CONNECT /ws/chat/rr/` (но 404 = значит ws‐route не сработал, Channels ничего не получил)
  + нет WebSocket‐Upgrade. Решайте с помощью исправления JS (`wss://` при HTTPS) и location `/ws/` с Upgrade-заголовками
    - Channels получит `WebSocket CONNECT`
  + проверить, что Nginx видит заголовок Upgrade `curl -i -N -H "Connection: Upgrade" -H "Upgrade: websocket" -H "Host: localhost:4443" -H "Origin: https://localhost:4443" --insecure  https://localhost:4443/ws/chat/test/`
    - Если Nginx проксирует правильно, в ответ вы увидите что-то вроде:
      ```
      HTTP/1.1 101 Switching Protocols
      Upgrade: websocket
      Connection: Upgrade
      ```
    - Если сразу HTTP/1.1 404, значит Nginx не зашёл в нужный location


### LIVE CHAT
* **история в бд** (или в redis?)
* **чат сделать компонентом**
* **заменить textarea на div для лучшей стилизации и управления сообщениями**
* the user sends direct messages to other users (subject)
* the user blocks other users, they see no more messages from the account they blocked (subject)
* the user **invites** other users to play through the chat interface (subject)
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
* Аутентификация Пользователей: Если ваш чат требует аутентификации, убедитесь, что пользователи проходят проверку перед подключением к WebSocket. В ChatConsumer вы можете использовать self.scope['user'] для доступа к объекту пользователя.
* Обработка Ошибок: Добавьте обработку ошибок в JavaScript для улучшения устойчивости чата. Например, попытки переподключения при разрыве соединения.
* Улучшение UX: Вместо textarea для отображения сообщений рассмотрите использование более продвинутых компонентов, таких как div с динамически создаваемыми элементами <p> или <div> для сообщений. Это позволит лучше стилизовать сообщения и добавлять различные элементы (например, аватары, имена пользователей).
* Динамическое Выбор Комнаты: Если ваш SPA позволяет пользователям выбирать разные комнаты, интегрируйте этот функционал внутрь модального окна. Можно добавить поле ввода для имени комнаты или список доступных комнат
* управление WebSocket Соединением
  + Инициализация: WebSocket-соединение устанавливается после ввода имени комнаты и нажатия кнопки "Join Room"
  + Закрытие: Соединение закрывается при закрытии модального окна или при повторном подключении к другой комнате
  + Отправка Сообщений: Пользователь может отправлять сообщения через поле ввода. Сообщения отображаются в логах чата с пометкой "You"
  + Получение Сообщений: Полученные сообщения отображаются в логах чата с пометкой "Server" (можно изменить на имя пользователя, если реализована аутентификация)
* | Method                | Real-Time | Message Persistence  | Use Case                           | Complexity |
  |-----------------------|-----------|----------------------|------------------------------------|------------|
  | WebSocket (chat)      | Yes       | No                   | Real-time chats, games             | High       |
  | Redis (Pub/Sub, ws)   | Yes       | No                   | Notifications, chats               | High       |
  | Django Messages       | No        | Yes                  | System notifications, confirmations| Low        |
  | REST API              | No        | Yes                  | Simple notifications, data requests| Low        |
  | Email                 | No        | Yes                  | Important notifications, confirmations| Medium  |
  | Push Notifications    | Yes       | Yes (by service)     | Mobile device notifications        | Medium     |


### JAVASCRIPT
* js меняет параметры html
* class = стиль
* open chat
  + на странице login, profile, решгистрация нету
  + во время игры - статистика, другой user
  + приложение, отправлятть сообщение через js !!
  + django channel = framework для сокетов
* {% load static %} загрузка стат файлов Django (table.css), нам не надо


### FRONTEND NGINX
* try using **bolt.new** it's better at frontend
  + the ui is fire here
* **game customization** it's just gonna be front
  + like custom colors custom map
* проверяет сертификаты SSL, расшифровывает с использованием SSL-сертификата
  + внутренний трафик не шифруется
  + если сертификат самоподписанный, браузер может выдавать предупреждение «Not secure»
    - у вас: «Non sécurisé»
    - иногда это мешает wss://‐подключению
    - в большинстве случаев всё равно WebSocket должен подключиться
    - если браузер категорически отказывается, можно в тестовом режиме добавить исключение для самоподписанного сертификата
* запросы к статическим файлам отдает его из /usr/share/nginx/html/static/
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
* **Bootstrap toolkit**
* **Карточка Bootstrap**  
* pop-up windows : login, chat, profile
* страница comptetition, profile, настройки
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
* /var/log/nginx/access.log 
* /var/log/nginx/error.log 
* Amine: game front using javascript
* alexey: Layout on the pages – working on it
  + расположение и структура элементов пользовательского интерфейса на веб-страницах
  + Работа с CSS-фреймворками (например, Tailwind CSS или Bootstrap)
  

### BACKEND DAPHNE 
* ASGI сервер 
* хорошо интегрируется с Django Channels
* поддерживает ws из коробки
* не настраивается под SSL
* слушает внутри контейнера на порту 8000 API-запросы, HTTP-запросы фронтенда
* принимает wss:// соединения
  + страница загружается по https:// => браузер открывает wss://
* `ASGI_APPLICATION = "myproject.asgi.application"`
  + `django_asgi_app = get_asgi_application()` initialize Django ASGI appli, exposes the ASGI callable as a module-level variable (загрузка приложений `INSTALLED_APPS`, конфигурация бд, среда AppRegistry is populated 
    - before importing ORM models, до использования ORM-моделей и др компонентов Django, конфигурации маршрутов ws
  + переменная `application` = точка входа для ASGI-сервера, ASGI-приложение, обрабатывает запросы
* проверить ws:
  + nNginx проксирует `location /ws/` к `backend:8000`  
  + daphne запущен на `backend:8000`  
  + redis доступен по хосту `redis` на `6379`
  + `wss://localhost:4443/ws/chat/<room>/`
  + `F12` Network
    - js инициирует подключение к `wss://localhost:4443/ws/chat/room/`
    - в списке сетевых запросов элемент, отражающий WebSocket (101 Switching Protocols)
    - в столбце "Status" 101
    - соединение "pending" (открыто), Network pending или active
    - заголовки `Upgrade: websocket`, `Connection: upgrade`
    - Finished в колонке Status ws-подключения = загрузка ресурса ок (первоначальный HTTP‐запрос на handshake) != ws-соединение разорвано 
    - если соединение активно, то входящие/исходящие сообщения в ws → Messages, в Timing (Headers) статус 101 Switching Protocols (если закрылось, вы событие «Close» (code 1000 или другое))
  + WebSocket-тестер WebSocket King
    - https://websocketking.com/
    - URL `wss://localhost:4443/ws/chat/room/`
    - можно отправлять текстовые сообщенияет
  + WebSocket-тестер wscat (CLI-инструмент)
    - в консоли `wscat -c wss://localhost:4443/ws/chat/testroom/`
    - увидите `connected`
    - набрать любое сообщение
    - если самоподписанный сертификат, отключать проверку (`--no-check` / `NODE_TLS_REJECT_UNAUTHORIZED=0`)
  + `/var/log/nginx/error.log`, `/var/log/nginx/access.log`
    - при установке ws-соединения запрос пришёл с `Upgrade: websocket`
  + логи daphne
    - `wss://<domain>/ws/...` -> nginx проксирует на `http://backend:8000/ws/...` - daphne принимает ws по пути `/ws/chat/<room>/` 
  + views.py обрабатвает HTTP Django URLs /chat/<room> 
    - WebSocket Channels обрабатывает /ws/chat/<room> (ASGI routing)
    - если в urls.py есть в path("ws/chat/<room>", views...), Django перехватывает его как HTTP и выдаваёт 404, или ищет шаблон
    - маршруты Channels: websocket_urlpatterns = [re_path(r'^ws/chat/(?P<room_name>\w+)/$', ChatConsumer.as_asgi()),]
    - если есть что-то вроде path('ws/chat/<room>/', some_view), то Django перехватывает HTTP‐запрос (а не Channels)
  + убрать AllowedHostsOriginValidator ради теста
* AllowedHostsOriginValidator проверяет допустимые хосты для WebSocket-соединений (**зачем дублировать с nginx
  + проверяет заголовок Origin против ALLOWED_HOSTS
* **ORM**
* альтернатива: `python manage.py runserver 0.0.0.0:8000`
  + запускает Django-приложение с использованием встроенного **разработческого сервера**
    - может работать как с WSGI, так и с ASGI, в зависимости от конфигурации проекта
    - не рекомендуется для продакшен, daphne: более высокая производительность и стабильность
* альтернатива: wsgi
  + asgi, wsgi - интерфейсы, стандарты для взаимодействия между веб-сервером и веб-приложениями/фреймворками
    - иначе фреймворки, тулкиты и библиотеки питон не умеют взаимодействовать между собой, у каждого свой метод установки, настройки
  + WSGI, Web Server Gateway Interface
    - поддерживает только синхронные запросы HTTP, не подходит для реального времени или протокола ws
    - но бывает нужен: некоторые инструменты поддерживают только HTTP-запросы; Django-приложений без асинхронных функций стабильнее
  + ASGI, Asynchronous Server Gateway Interface - продолжение WSGI, когда есть асинхронные задачи
* Amine: game backend using websockets (with 42 auth)


### ЯДРО DJANGO 
* бэкенд-фреймворк
* диктует архитектуру (приложения, модели, views, urls, ...)
* DJANGO_SETTINGS_MODULE настройки компонент Django
* `settings.py` конфиг на высоком уровне
  + `ALLOWED_HOSTS` список доменов/IP, с которых разрешён доступ к Django-приложению (когда `DEBUG=False`)
  + `TEMPLATES` настройки шаблонов Django (**HTML-шаблоны**)
  + `REST_FRAMEWORK` **рендеры**, по умолчанию JSON и **Browsable API**
  + `AUTH_PASSWORD_VALIDATORS` валидаторы сложности паролей (минимальная длина, ...)
  + logging.basicConfig настройки логирования в Python, может конфликтовать с настройками логгеров Django и Channels
  + LOGGING можете настроить разные обработчики, уровни логирования и форматы для логгеров django, channels, django.db
* myproject/ конфигурация Django, корневой маршрутизатор, запуск, управление проектом
* `models.py` структура данных, связи (пользователи, профили, чаты, сообщения, статистика игры, ...)
  + **`migrations/`** для моделей
* manage.py django-утилита
  + интерфейс для настройки, разработки, управлением проектом
  + python manage.py runserver запуск сервера разработки, daphne обращается к проекту напрямую или через `mysite.asgi`
  + python manage.py migrate применение **миграций** базы данных
  + python manage.py createsuperuser создание суперпользователя
  + `if __name__ == '__main__':` точка входа программы, если файл запущен напрямую (не импортирован как модуль), выполняется `main()`
  + [auth]: changepassword, [contenttypes] remove_stale_contenttypes, [django] check compilemessages createcachetable dbshell diffsettings dumpdata flush inspectdb loaddata makemessages makemigrations optimizemigration sendtestemail shell showmigrations sqlflush sqlmigrate sqlsequencereset squashmigrations startapp startproject test testserverт, [rest_framework] generateschema, [sessions] clearsessions, [staticfiles] collectstatic findstatic, можнго создавать собственные команды
* /admin встроеный интерфейс для управления данными (пользователи, чаты, статистика, ...)
* суперпользователь Django Admin: все права в системе, полный доступ к системе управления данными (!= суперпользователь базы данных)
* вcтроенные django app модульные приложения
  + 'django.contrib.messages'
  + 'django.contrib.admin',
  + 'django.contrib.auth',
  + 'django.contrib.contenttypes',
  + 'django.contrib.sessions',
  + 'django.contrib.staticfiles'
  * **какие ещё**
* 'django.contrib.messages'  API для работы с flash‐messages
    - уведомления пользователю **между запросами** (об регистрации, ошибка при неверном вводе данных, ... в верхней части страницы
    - исчезают через несколько секунд или после того, как пользователь обновит страницу или перейдет на другую
    - **сообщение добавляется во время перенаправлений (после POST-запросов, ...) или в шаблоны**
    - **передача статуса операций между страницами**: после сохранения формы пользователю показывается сообщение на новой странице
    - не используются для функций реального времени, не асинхронны
    - работают благодаря  `django.contrib.messages` + `MessageMiddleware`
    - `MessageMiddleware` **сохраняет между запросами** и извлекает из хранилища (cookie, сессии, ...)
    - сообщения остаются там только до следующего запроса, затем удаляются автоматически
    - без `MessageMiddleware` только для сообщений **внутри одного запроса**, сообщения не сохраняются между запросами
    - через ws соединения или HTTP-запросы
    - не имеет отношения к чату (чат требуют постоянного обновления страницы или использования ws для обмена сообщениями)
* middlware
  + набор классов
  + фильтры, обработчики, модифицируюют запросы/ответы
  + **для доступа к объекту пользователя (request.user) и другим данным авторизации**
* middlware на пути от клиента
  + SecurityMiddleware
    - добавляет заголовки безопасности (`Strict-Transport-Security` для HTTPS)
    - перенаправляет HTTP-запросы на HTTPS **куда именно? какой-то порт?**
    - убирает опасные элементы из запросов (от уязвимости **XSS**, ...)
  + SessionMiddleware
    - загружает данные сессии из **хранилища**, управляет сессиями **через куки?**
    - если пользователь **аутентифицирован**, сессия **сохраняет его идентификатор** в Redis или базе данных после выполнения запроса
  + CommonMiddleware
    - перенаправление при добавлении/удалении `/` в конце URL
    - возвращает стандартные заголовки HTTP (`Content-Type`, ...)
  + CsrfViewMiddleware
    - проверяет наличие и правильность CSRF-токена
    - если токен неверный, возвращает ошибку `403 Forbidden`
    - защита от **Cross-Site Request Forgery**
  + AuthenticationMiddleware
    - проверяет **сессию или заголовки авторизации**
    - добавляет **объект `request.user`**, чтобы представления могли определить, аутентифицирован ли пользователь
  + MessageMiddleware
    - обрабатывает flash messages
    - может быть настроен для **обработки сообщений и добавления дополнительной информации или фильтрации на уровне WebSocket**
    - без `django.contrib.messages` невозможно использовать
  + XFrameOptionsMiddleware
    - добавляет заголовок `X-Frame-Options` для защиты **clickjacking** (запрещает отображение сайта в **iframe** на других доменах, ...)
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
* базовый serializers.ModelForm
* базовые механизмы аутентификации django.contrib.auth (**как связано с AuthenticationMiddleware**)
  + модель пользователей, проверка паролей, сессии
* проверка **токена/сессии** при каждом запросе с фронтенда
* регистрация и авторизация (в том числе OAuth/SSO)
* модель пользователей
* простая модель групп и разрешений django.contrib.auth.models.Permission
  + разграничение доступа к страницам (приватные/публичные чаты, комнаты, ...)
* инструменты по защите от распространённых уязвимостей (**CSRF, XSS, SQL Injection**)
* **среда `AppRegistry`** управляет регистрацией приложений и моделей
* **Django Debug Toolbar** отслеживание работы проекта
* DPR Data Processing and Rendering: данные (JSON, ...) обрабатываются на сервере с помощью Django, передаются в представление для рендеринга на клиентской стороне
* используем **стандартные структуры юзера для авторизации и для моделей данных**
* технические детали
  + на базе Python
* ORM генерирует SQL-запросы  


### DJANGO REST FRAMEWORK DRF
* расширяет Django, строит над Django более мощный стек для работы с RESTful API
* базовые модели, виджеты, фильтры, классы для работы с API
* модуль сериализации данных для работы с JSON или API #see
  + преобразование моделей Django и других объектов Python в форматы JSON/XML и обратно
  + создавать сложные структуры
  + валидация данных
* модуль аутентификации #see
  + Token Authentication
  + Session Authentication
  + JWT (с внешними библиотеками, например, Simple JWT), добавляет гибкие механизмы для работы с JWT
  + Basic Authentication
  + авторизация по API-ключам
* модуль прав доступа Permissions #see
  + контролировать доступ к API на основе классов прав доступа (`AllowAny`, `IsAuthenticated`, `IsAdminUser`, `IsAuthenticatedOrReadOnly`) или кастомных permissions
* Throttles
  + вспомогательный компонент DRF, работает в тандеме с модулями сериализации, аутентификации, прав доступа
  + контролируют частоту запросов пользователей
  + защита от DoS-атак и чрезмерной нагрузки на сервер (**nginx недостаточно?**)
* APIView
  + вспомогательный компонент DRF, работает в тандеме с модулями сериализации, аутентификации, прав доступа
  + может работать в связке с сериализаторами для обработки данных запросов и формирования ответов
  + базовый класс представлений (views)
  + создавать собственные представления для обработки запросов
  + методы (`get`, `post`, `put`, `delete`, и т.д.), которые можно переопределять для обработки HTTP-запросов
  + встроенная поддержка систем аутентификации и контроля прав доступа (например, с использованием `IsAuthenticated`)
  + применять ограничения на количество запросов
  + больше контроля, чем ViewSet, определяете логику запросов и маршрутизацию
  + вручную определять маршруты и методы для каждого запроса
  + подходит, если проект небольшой или требует точной настройки
* ViewSets
  + вспомогательный компонент DRF, работает в тандеме с модулями сериализации, аутентификации, прав доступа
  + автоматизирует маршрутизацию и типичные CRUD-операции с помощью Routers
  + объединяют логику CRUD-операций в одном классе
* Routers
  + вспомогательный компонент DRF, работает в тандеме с модулями сериализации, аутентификации, прав доступа
  + помогает автоматизировать и упрощать маршрутизацию RESTful API
  + автоматически связывают URL-маршруты с ViewSets
  + роутеры наследуются от базового класса BaseRouter
  + DefaultRouter один из предопределённых роутеров
    - реализациея Router
    - расширяет функциональность SimpleRouter, добавляет поддержку автоматической маршрутизации API root (/) и обработку схемы API
    - автоматически создаёт URL-маршруты (эндпоинты) для стандартных действий представления (viewset)
* Filters  
  + фильтрация данных, которые возвращает API, по параметрам запроса
+ базовые подходы django
  + FBV function based views
    - представления, основанные на функциях
    - гибкий подход
    - ручная обработкя запросов (`GET`, `POST`, ...)
    - проще
    - def my_view(request): return JsonResponse({"message": "Hello, world!"})
  + class based views CBV
    - представления как классы, ООП-подход 
    - встроенная логика обработки запросов (`get()`, `post()`, ...)
    - позволяют переопределять только нужные методы (`get`, `post`), а также использовать миксины
    - def get(self, request): return JsonResponse({"message": "Hello, world!"})
* каждый запрос обрабатывается отдельно, клиент получает ответ сразу
* рендеринг шаблонов (Server-Side Rendering) у нас нет
* PUT = поменять поле в бд (без DRF запрос sql)


### DJANGO RESTFUL HTTP API 
* API Application Programming Interface = интерфейс = набор правил = WebSocket-путь = группа маршрутов (эндпоинтов)
* обрабатывает HTTP-запросы (GET, POST, PUT, DELETE) и отклики между клиентом и сервером
* REST Representational State Transfer стиль взаимодействия между клиентом и сервером
  + Request Body (json, xml, yaml, MessagePac, CSV)
  + Query Parameters `GET /users?name=Иван`
  + Path Parameters `GET /users/1/`
  + Headers `Content-Type: application/json`  
* API = 5–10 эндпоинтов, например, группа всех маршрутов, связанных с чатом
* endpoint = URL-маршрут = конкретный маршрут, связанный с API
  + выполняет определённое действие
  + endpoints чата `GET /chat/rooms/`, `POST /chat/rooms/`, `GET /chat/rooms/<room_id>/messages/`, `POST /chat/rooms/<room_id>/messages/`
* APIView ViewSet специализированные CBV из DRF
  + обработчики HTTP-запросов  для работы с API
  + определяются в View-классах или функциях
  + классы, наследущие от `APIView`, `GenericViewSet`, `ViewSet`
  + APIView`
    - класс, наследуется от класса View
    - CBV предназначенный для создания REST API
    - добавляет поддержку сериализации,  аутентификации, работы с JSON, вспомогательные методы для обработки HTTP `GET` `POST` `PUT` `DELETE`, др API-функций
    - для простых API с базовой логикой
  + ViewSet
    - расширенный вариант APIView
    - использует методы по умолчанию, предоставляемые DRF (избежать написания методов для HTTP-запросов `GET`, `POST`, `PUT`, `DELETE`)
    - добавляет автоматическую обработку запросов для стандартных операций CRUD (Create, Read, Update, Delete) над данными, которые уже есть в базе
    - работает в связке с Router, что упрощает маршрутизацию API
    - генерирует маршруты для операций с ресурсами (создание нового объекта, получение списка объектов, обновление, удаление)
    - предоставляет стандартные действия для модели
    - подходит для ресурсо-ориентированных API, где операции с объектами ("пользователи", "продукты") 
  + | Вариант     | Для чего?                          | Маршрутизация |
    |-------------|------------------------------------|----------------|
    | **FBV**     | Обычные представления              | Ручная (`path()` или `re_path()`) |
    | **CBV**     | ООП-подход к представлениям        | Ручная (`as_view()`) |
    | **APIView** | API-представления                  | Ручная (`as_view()`) |
    | **ViewSet** | API + автоматическая маршрутизация | Через `Router` |
* позволяет **приложениям** взаимодействовать друг с другом
* сессии не нужны, авторизация через токены
* реализуются
  + через **события**
  + через **статусы в ответах API**
  + не через механизм сообщений Django
* bakyt: API for Database - UserProfile works


### DJANGO CHANNELS
* расширение, framework, библиотека, надстройка для ws поверх стандартного стека Django: использовать привычные инструменты и структуры
* добавляет поддержку асинхронных протоколов, асинхронного взаимодействия, **фоновых задач**, обновление интерфейса в режиме реального времени
* слушает ws-запросы
  + непрерывное соединение, стрим
  + js инициирует соединение через **WebSocket API**: `new WebSocket`
  + daphne устанавливает ws-соединение
  + daphne передаёт ws-соединение в ASGI-приложение
  + связывает URL запроса с обработчиками `consumers.ChatConsumer.as_asgi()`, URLRouter маршрутизирует запросы на consumers
    - создает consumer
  + закрытие соединений
* обслуживание ws
  + AuthenticationMiddlewareStack
    - проверяет, что соединение аутентифицировано (проверка токенов JWT, Cookies и др)
    - связывает пользователя с запросом, с подключением
    - оборачивает маршруты ws
    - обеспечивает аутентификацию пользователей
    - добавляет объект user в scope["user"] => в consumers `self.scope["user"]` для информации об аутентифицированном пользователе и выполнять действия на основе его прав и ролей
  + SessionMiddleware (если используется) работа с сессией
    - получение id сессии и связанного с ним пользователя
    - сохранение информации о сессии: id сессии и связанные данные (пользователь, ...)
    - обязателен, если используете сессии (хранение пользовательские данных между запросами)
    - обязателен, если есть `django.contrib.auth` (он опирается на сессии для хранения данных о пользователе)
    - обязателен, если взаимодействие с сессиями на сервере (сессионное хранилище в бд, Redis, файловой системе,...)
    - можно без него, если JWT токены или др безсессионные методы аутентификации 
    - можно без него, если данные о сессии хранятся в зашифрованных cookie (без серверного хранилища)
  + другие middleware: ограничение скорости, управление правами доступа, обработка ошибок
  + consummer получает `{ "type": "chat.message", "message": "Hello, World!" } с заголовками
    - логика приёма и обработки ws-запросов и аутентификации/авторизации может быть вместо этого в кастомном middleware
  + redis Backend Channel Layer
  + consumer отправляет через ws обратно пользователю `{ "type": "chat.message", "message": "Hi there!" }` 
  + сессии передаются через cookie
  + если сертификат отсутствует, браузер может блокировать подключение WebSocket
  + если страница загружена по https://, то используйте wss:// 
* назначение пользователей к группам каналов
* в браузере https://localhost:4443/static/chat.html
  + подключается к ws://localhost:4443/ws/chat/r/
  + надо:         wss://localhost:4443/ws/chat/r/
  + ошибка Mixed Content или блокировка
  + в консоли `Connecting to WebSocket at: ws://localhost:4443/ws/chat/r/`
* в Django логе 404 при обращении к /ws/chat/d/ как к обычному HTTP‐endpoint, а не WebSocket
  + запрос ws не доходит в ASGI
  + вместо этого идёт GET /ws/chat/d/, django видит /ws/chat/d/ как обычный GET
    - либо в браузере «GET ws://… failed» => браузер не делает **Upgrade** до WebSocket, отправляет обычный HTTP GET => Django ищет URL /ws/chat/d/ в urls.py — не находит → 404
    - либо ws:// заблокирован (Mixed Content), браузер не даёт WebSocket начаться, Django не видит WebSocket handshake
* проверить, что Nginx принимает ws
  + F12 → Network → ws
  + если соединение удаётся, в списке появится WebSocket‐запрос, обычно со статусом 101 (Switching Protocols)
  + если статус 404/400/500, смотрите вкладку «Headers» — там может быть ответ от Nginx
* console после нажатия Join Room, какой URL логируется?
  + Connecting to WebSocket at: wss://localhost:4443/ws/chat/r/
  + если wss://…, но статус = (failed), зайдите в Network → WS и гляньте причину
  + если всё ещё ws://, значит старый JS код или кэш
  + docker-compose logs frontend: 400 Bad Request, Request: GET /ws/chat/... HTTP/1.1", 101 Switching Protocols при успехе
* если на старте Daphne (Channels) не видите «WebSocket CONNECT …», то запрос не доходит как WebSocket
  + если всё ок, вы бы увидели WebSocket CONNECT /ws/chat/r/ …


### CHANNEL LAYERS
* абстракция, настройка, промежуточный уровень, канал-сервер, канальный слой для Django Channels
* абстрактный интерфейс/протокол
* публикует в канал, доставляет подписанным
* передача сообщений между частями приложения
  + ```
    [Клиент] 
        ▼
    [Consumers]
        ├──► [Django Models] ↔ [База данных]
        ├──► [Channel Layers (Redis)]
        │       ├──► [Другие Инстансы Приложения]
        │       └──► [Процессы]
        ├──► [Асинхронные задачи (Celery)]
        │       └──► [Фоновые Рабочие Процессы]
        └──► [Аутентификация и Авторизация]
      ```
  + инстансами приложения
  + клиент ↔ consumers: Установление соединения: Клиент инициирует WebSocket-соединение с сервером через потребителя
  + клиент ↔ consumers: Обмен сообщениями: Клиент отправляет данные (например, действия в игре), а сервер обрабатывает и рассылает обновления другим клиентам.
  + consumers ↔ channel layers: consumers присоединяются к группам (комнатам, игровым сессиям) через Channel Layers
  + consumers ↔ channel layers: отправка сообщений в группу
  + Consumers ↔ Django Models и База данных: чтение/Запись данных
  + Consumers ↔ Django Models и База данных: Транзакции: Обеспечение согласованности данных при одновременных изменениях.
  + Consumers ↔ Асинхронные задачи (**Celery**): Запуск задач: При определённых событиях (окончание игровой сессии, ...) Consumers инициируют выполнение фоновых задач.
  + Consumers ↔ Асинхронные задачи (Celery): Обработка результатов: Фоновые задачи могут обновлять данные или отправлять уведомления после выполнения.
  + Consumers ↔ Аутентификация: Проверка пользователя: При установлении соединения Consumers проверяют аутентификацию пользователя через scope["user"].
  + Consumers ↔ Аутентификация: Управление доступом: Ограничение доступа к определённым действиям или комнатам на основе прав пользователя.
  + Backend ↔ Внешние API и Сервисы: Интеграция: Django взаимодействует с внешними сервисами (обработка платежей, отправка уведомлений, получение данных)
  + Backend ↔ Внешние API и Сервисы: Маршрутизация: Обработка запросов от внешних источников и передача данных обратно клиентам через Consumers.
  + Клиент ↔ REST/GraphQL API: Дополнительные запросы: Помимо WebSocket-соединений, клиент может использовать традиционные HTTP-запросы для получения или отправки данных.
  + Клиент ↔ REST/GraphQL API: Синхронизация данных: Обеспечение согласованности состояния между различными частями приложения.
  + Consumers ↔ Другие Consumers: Взаимодействие: В некоторых случаях один Consumer может взаимодействовать с другим через Channel Layers для координации действий или обмена данными.
* очереди сообщений
  + операции в фоновом режиме (обработка запроса, отправка уведомлений)
  + избежать блокировки основного потока
* управляет **хранением состояния**
* не обязателен, если
  + приложение запускается в одном экземпляре
  + все клиенты подключаются к одному процессу
  + обмен сообщениями между клиентами и один общий чат, нет сложного обмена сообщениями
* без Channel Layer
  + обрабатывать сообщения напрямую внутри процесса, используя функциональность Django Channels и ws-соединений
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
* масштабирование, легко работают в кластере
* может работать без внешнего channel layer
  + по умолчанию InMemoryChannelLayer
  + работает только внутри одного процесса (одного экземпляра приложения)  
  + ок если вам нужно всего лишь точечно отправлять и получать сообщения в рамках одного процесса
  + широковещательное (group_send) или мульти‐user‐чат‐шаблон если и нужен, то только в рамках одного процесса
  + реал-тайм обновления для одного пользователя (или небольшой группы в рамках одного процесса). Например, вы хотите уведомлять конкретного юзера о событиях (новые сообщения, новые заявки в друзья и т.п.), и ваш сервер – один.  
  + Мини-чат или прямое общение (point-to-point). Если все пользователи сидят в рамках одного сервера (и у вас не требуется распределённая горизонтальная архитектура), то и группы Channels будут работать внутри этого одного процесса 
  + Push-уведомления из кода в конкретный WebSocket‐консьюмер
* **websocket один для всех пользователей для чата (лёша)**
* channels_redis поддержка групповой рассылки


### CELERY
* систем управления задачами
* взаимодействие между основным приложением и фоновыми процессами
* consumers инициируют фоновые задачи, которые обрабатываются отдельными рабочими процессами


### REDIS
* Remote Dictionary Server  
* два процесса на одном сервере (c разными ключами и настройками): кэширование + обслуживание channel layers
* **хранение данных**
* маршрутизация данных
* **подсчёт статистики, временных данных**
* `redis-cli -n 1 flushdb` очистить кэш 
* настройка
  + доступен только внутри сети Docker
  + время жизни кэша (`TIMEOUT`), ...
  + `redis-cli info memory` не перегружен?
* `vm.overcommit_memory = 1` #see
  + параметр ядра (kernel)
  + как система распределяет виртуальную память
  + сохранять данные в условиях низкой памяти
  + избежать ошибки `OOM` при больших `maxmemory`
  + в `/etc/sysctl.conf` сохранить после перезагрузки
  + sysctls: vm.overcommit_memory: "1" в docker-compose.yml
    - может не сработать, если контейнер не имеет прав (Docker не даёт контейнеру доступ к глобальному kernel-параметру, кроме безопасных сетевых)
    - иногда Docker игнорирует «небезопасные» параметры, если контейнер не в privileged-режиме
  +  Dockerfile: RUN echo "vm.overcommit_memory=1" >> /etc/sysctl.conf
    - не будет работать, так как в контейнере нет полноценного ядра, Docker запрещает менять ряд параметров
    - запускать контейнер с `--privileged`
   + `cat /proc/sys/net/core/somaxconn` 4096 #see
    - макс длина очереди соединений (backlog) для сокета в режиме прослушивания (listen), сколько входящих соединений могут быть поставлены в очередь
    - внутри контейнера listen(fd, 10000) - очередь будет ограничена 4096 (если не переопределяется другими настройками)
    - значение берётся у хостового ядра, ведь контейнеры используют общее ядро с хостом
  + **защищён паролем** #see
* проверка 
  + `docker-compose exec redis redis-cli PING` - PONG
  + добавить `redis-cli` в Dockerfile или docker-compose
* как устроен технически
  + работает в оперативной памяти => высокая скорость записи и чтения
  + может сохранять данные на **диск**, чтобы избежать потери при сбоях #see
  + `channels_redis` библиотека для подключения и отправки сообщений
  + `django-redis` библиотека для сериализации* данных, записи, извлечения
  + поддерживает множество языков программирования
  + транзакции и скрипты на языке Lua


### REDIS = BACKEND ДЛЯ CHANNEL LAYERS = реализация Channel Layer
* бэкэнд, брокер сообщений, посредник
* Django Channels описывает channel layer как интерфейс (набор методов `send()`, `receive()`, `group_add()`, ...)
* конкретный класс, который реализует протокол Channel Layer
* если несколько серверов/контейнеров обрабатывают ws-запросы
  + daphne каждого инстанса/контейнера подключается к Redis-серверу
  + сервер получает сообщение от клиента, отправляет его в канал через Channel Layer
  + Redis доставляет соообщение на другой сервер/инстанс, подписаный на канал
* для работы с асинхронными задачами через Django Channels (**какие ещё задачи**)
* распределять нагрузку между несколькими контейнерами (Горизонтальное Масштабирование)
+ **устойчивость к сбоям**
* для долговременного хранения сообщений или очередей вне памяти приложения
* обработка большого количества одновременных WebSocket соединений и сообщений в реальном времени
* используется механизм Pub/Sub (Publish/Subscribe) для реализации чата
  + Redis = брокер сообщений в режиме Pub/Sub
     - Подключение к Redis (`redis.createClient()` или аналог)
     - Вызовы методов `publish()` и `subscribe()`
  + Pub/Sub => идеальный для каналов WebSocket
  + рассылать сообщения всем пользователям чата
  + бекенд для Pub/Sub
  + механизм списков или Pub/Sub для реализации очередей сообщений
  + Поиск в коде `PubSub`, `publish`, `subscribe`, `event`, `broker`, `topic`
    - библиотеки  `Redis`, `RabbitMQ`, WebSocket-библиотеки с поддержкой Pub/Sub
  + Если чат реализован через WebSocket, Pub/Sub может быть встроен в логику
    . Например, отправка сообщения (`publish`) уведомляет подписчиков (`subscribe`)
  + Если вы используете NestJS, в нем есть встроенная поддержка Pub/Sub через библиотеки, такие как `@nestjs/microservices`.
   - Проверьте папку `services`, `chat`, или `events` на наличие Pub/Sub-логики.
  + Найдите модули или файлы, связанные с чатом (`chat.service`, `chat.controller`)
    - если сообщения отправляются на брокер или WebSocket, это может быть признаком Pub/Sub
  + `package.json` зависимости, связанные с Pub/Sub `cat package.json | grep -E "redis|socket.io|nestjs/microservices"`
  + отправьте сообщение в чат и проверьте:
    - логирование на сервере
    - Задержки при передаче сообщения. Если сообщение доходит мгновенно, вероятно используется WebSocket с логикой Pub/Sub
* альтернатива ему - различные реализации того же абстрактного интерфейса, что и `channels_redis.core.RedisChannelLayer`
  + встроенный в Django Channels **InMemoryChannelLayer**
    - класс из пакета `channels.layers`  
    - хранит сообщения в памяти процесса Python  
    - не годится для продакшн: поддерживается единственным процессом, данные исчезнут при перезапуске
  + RabbitMQ брокер сообщений системных сообщений (уведомление о начале турнира, уведомление о добавлении в друзья)
  + PostgreSQL
  + IPC  


### REDIS ДЛЯ ХРАНЕНЯ СЕССИЙ
* для управления **состоянием** ws-соединений
  + хранит **данные о подключениях**
  + очередь задач (отправки email, push-уведомления, логирование, обработка фоновых задач (обновление статистики матчей))
  + для создания очередей задач (для обмена задачами между различными частями приложения, такими как обработка сообщений или фоновые задачи)
* управление пользовательскими сессиями в веб-приложениях при подключении пользователей к ws
  + хранение данных о текущих авторизованных пользователях
* бекенд для **ws-сессий**
 

### REDIS ДЛЯ КЭШИРОВАНИЯ
* django cash framework обращается к redis
  + инфраструктура для кэширования
  + хранение кэша (запросы, объекты, шаблоны, ...)
  + не требует добавления в INSTALLED_APPS
  + настраивается в CACHES
  + Django API для кэширования данных
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
    - redis = очередь сообщений, которые могут быть отправлены клиентам
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


### СТАТИЧЕСКИЕ ФАЙЛЫ html js CSS изображения шрифты
* чтобы все файлы находились/отдавались там, где ожидает браузер
* в разработке три варианта (всё ниже этого пункта, можно в production):
  + **`python manage.py runserver` при DEBUG = True**
  + manually serve user-uploaded media files from MEDIA_ROOT
    - use django.views.static.serve() view
    - don’t have django.contrib.staticfiles in INSTALLED_APPS
    - if STATIC_URL = static/, addi urlpatterns = [ ] to your urls.py `static(settings.STATIC_URL, document_root=settings.STATIC_ROOT)`
      - works only if the prefix is local (static/) and not a URL (http://static.example.com/)
    - only serves STATIC_ROOT folder
    - doesn’t perform static files discovery like django.contrib.staticfiles
    - serves static files via a wrapper at the WSGI application layer => static files requests do not pass through the middleware chain
  + напрямую ссылаться на файлы через URL
* 'django.contrib.staticfiles'
  + обслуживает стат файлы для /admin
  + позволяет добавлять версии, хеши в имена файлов на фронтенд, чтобы избежать кеширования старых версий
* `collectstatic`
  + админские CSS (`base.css`, `dashboard.css`, ...), стат. файлы DRF для browsable API или других админ-панелей - в `django.contrib.admin`  
  + `python manage.py collectstatic` собирает статические файлы (включая админку, DRF) в STATIC_ROOT = app/staticfiles/
    - **STATIC_ROOT не трекается гитом**
  + **Frontend имеет только права на чтение статических файлов Backend**
* Nginx обслуживает ваши собственные стат фалйы и те, что созданы Django, по одному и тому же пути `/static/`
  + `location /staticfiles/ { alias /app/staticfiles/; }` # папка внутри контейнера Nginx
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
* ошибка с “MIME type ('text/html') is not a supported stylesheet” #see
  + в логах «Refused to apply style from '/static/css/chat.css' because its MIME type ('text/html') is not a supported stylesheet MIME type» => nginx отдает chat.css с заголовком Content-Type: text/html вместо text/css
  + убедитесь, что файл существует по пути /static/css/chat.css (или /app/static/css/chat.css) и настроены правильные заголовки MIME
  + как минимум, в Nginx можно прописать:
    ```
    location /static/ {
        alias /path/to/collected/static/;
        try_files $uri =404;
        # И MIME-тип автоматически определит, что *.css -> text/css
    }
    или включить include /etc/nginx/mime.types;
```


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
* валидация данных
  + **js на фронте читает инпут, проверяет с помощью regex**
  + функция make password django шифрует на сервере ?
  + бэк ещё раз валидирует (проверяет пароль и почту, ...) **зачем два раза** 
* DFR использует сессии или JWT для аутентификации пользователей
  + эти данные доступны в middleware или обработчиках запросов
  + Session storage: Если включён SessionMiddleware, пользовательские данные будут сохраняться в сессиях и доступны в обработчиках.
  + JWT токен передаётся в заголовке авторизации (Authorization: Bearer <token>) и обрабатывается специальным классом аутентификации


### ПРОТОКОЛЫ
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
* настроить `sysctl`-параметры для контейнера
  + минимальные образы не содержат `/etc/sysctl.conf`, системные параметры (sysctl) ориентированы на хостовый kernel, контейнеры используют ядро хоста, не своё собственное
  + `docker run --name redis --sysctl net.core.somaxconn=1024 -d your_image`
  + или docker-compose
    ```yaml
    services:
      redis:
        image: ...
        sysctls:
          net.core.somaxconn: 1024
          net.ipv4.tcp_syncookies: 1
    ```
  + проверить значения sysctl в рантайме `cat /proc/sys/net/core/somaxconn`


### ЗАПУСК
* ASGI_APPLICATION = "myproject.asgi.application" есть, **зачем CMD ["daphne", "-b", "0.0.0.0", "-p", "8000", "myproject.asgi:application"]**
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
* список эндпоинтов
  - `python manage.py show_urls` 
  -  `grep -r "path(" backend/`, `grep -r "re_path(" backend/`
  - `curl -X GET http://localhost:8000/api/endpoint/`
  - `curl -X POST http://localhost:8000/api/endpoint/ -H "Content-Type: application/json" -d '{"key": "value"}'`
  - Postman
  - Chrome + расширения
  - Python + библиотека `websockets`
  - http://localhost:8000/api/, http://localhost:8000/ автоматичесая документация эндпоинтов **Browsable API** 
  - http://localhost:8000/swagger/, http://localhost:8000/redoc/, если настроена автоматическая документация

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
