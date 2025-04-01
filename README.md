### git
* создаст репозиторий со всеми ветками
  + git clone --origin origin git@github.com:IOKXOI/ft_transcendence.git
  + cd ft_transcendence
  + git fetch --all --prune
* pull чужие ветки, моя не меняется
  + git fetch --all --prune
* pull мою ветку
  + git pull origin anna --rebase
* push мои изменнеия
  + git add . && git commit -m "comment" && git push origin anna

### Перевод бэкенда турниров с Django + PostgreSQL на Node.js + Fastify + Prisma + SQLite  
1. Перенос моделей данных (Django ORM → Prisma ORM)
   - Django использует классы моделей (`models.Model`), а Prisma — схему в `schema.prisma`  
   - переписать модели под Prisma
   - перенести миграции  
   - PostgreSQL поддерживает много мощных функций (JSONB, транзакции, индексы), а SQLite — намного проще
2. Переписывание бизнес-логики  
   - Django имеет готовую админку, аутентификацию, работу с пользователями
   - Fastify более минималистичный  
   - вручную писать обработку запросов, middlewares, валидацию
3. Переписывание API (Django REST Framework → Fastify)  
   - Django REST Framework (DRF) даёт много "из коробки", например, сериализацию данных, пагинацию.  
   - В Fastify нужно будет использовать Zod или TypeBox для схем API
4. Аутентификация  
   - Django использует Django Auth + JWT или сессии  
   - В Fastify можно использовать `fastify-jwt` или OAuth2  
5. Миграция базы данных  
   - PostgreSQL → SQLite может быть проблемой, если есть сложные связи, триггеры, JSONB-поля  
* изменится  
  - Fastify быстрее Django (меньше overhead)
  - Гибкость** → Prisma удобен для работы с БД, можно легко переключаться между SQLite и PostgreSQL
  - Придётся вручную писать логику**, которую Django делает автоматически
  - Fastify требует больше конфигурации**, чем Django (middleware, плагины, сериализация)

### checklist
// website with login, 2fa and 42 login where we can play local pong against ai or guest players. Frontend in vannila js, html, css and bootstrap. Backend django, database postgresql. used docker for deployment.

# Project Evaluation Checklist

## Preliminary Setup

### .env File Configuration
1. **Verify .env file presence and configuration:**
   - Open the repository and check for the `.env` file at the root.
   - Open the `.env` file and confirm it contains all necessary credentials, API keys, and environment variables.
   - Ensure no credentials are hardcoded in the codebase by searching for keywords like "password", "apikey", "secret", etc.

### Docker Configuration
2. **Check Docker setup:**
   - Ensure the `docker-compose.yml` file is at the root of the repository.
   - Run the command `docker-compose up --build` in the terminal.
   - Observe the build process for any errors or warnings.
   - Ensure the application starts successfully without crashing.

## Basic Checks

### Website Availability
3. **Check website access:**
   Test cases 1:
   - Open a web browser and navigate to the website URL.
   - Verify the website loads correctly without any errors.
   
   Test cases 2:
   - Open the website in a private or incognito window to check for any caching issues.
   - Ensure the website loads correctly in the private window as well.

### User Registration
4. **Test user registration:**
   Test cases 1:
   - Navigate to the registration page.
   - Fill out the registration form with valid user details.
   - Submit the form and check for successful registration.

   Test cases 2:
   - Attempt to register with an existing email address or username.
   - Verify the application displays an appropriate error message.

   Test cases 3:
   - Submit the registration form with invalid data (e.g., missing fields, incorrect email format).
   - Ensure the application validates the inputs and displays relevant error messages.

   Test cases 4:
   - Submit the registration with js or sql injection.
   - Ex: `username: <script>alert('XSS')</script>`
   - Ensure the application sanitizes the inputs and displays relevant error messages.

### User Login
5. **Test user login:**
   Test cases 1:
   - Navigate to the login page.
   - Enter valid credentials and log in.
   - Verify the application redirects to the user dashboard or home page.

   Test cases 2:
   - Attempt to log in with invalid credentials.
   - Ensure the application displays an appropriate error message.

   Test cases 3:
   - Attempt to log in with a valid username and incorrect password.
   - Verify the application displays an appropriate error message.

   Test cases 4:
   - Attempt to log in with a valid username and sql injection in password.
   - Ex: `password: ' OR 1=1 --`
   - Ensure the application sanitizes the inputs and displays relevant error messages.

### Single Page Application (SPA)
6. **Verify SPA behavior:**
   Test cases 1:
   - Navigate through different pages and sections of the website.
   - Check if the website behaves like a single-page application (SPA) with smooth transitions and dynamic content loading.

   Test cases 2:
   - Refresh the page or navigate directly to a specific URL.
   - Ensure the application loads the correct content and maintains the SPA behavior.

   Test cases 3:
   - Use the browser's back and forward buttons to navigate through the website.
   - Verify the application updates the content dynamically without full page reloads.

## User Interface and Design

### Cross-Browser Compatibility
7. **Check compatibility with Chrome:**
   Test cases 1:
   - Open the website in the Google Chrome browser.
   - Ensure all elements are displayed correctly and the website functions as expected.

   Test cases 2:
   - Open the website in other popular browsers (e.g., Firefox, Safari, Edge).
   - Verify the website's compatibility and functionality across different browsers.


## Security Concerns

### TLS/HTTPS
8. **Verify HTTPS setup:**
   Test cases 1:
   - Open the website in a web browser and check for a secure connection (https://).
   - Verify the presence of a valid SSL certificate.

   Test cases 2:
   - Attempt to access the website using an insecure connection (http://).
   - Ensure the application redirects to the secure (https://) version of the website.

   Test cases 3:
   - Use online tools like SSL Labs to scan the website for SSL/TLS configuration issues.
   - Ensure the website receives a good rating for its SSL/TLS setup.


### Password Security
9. **Check password hashing:**
   Test cases 1:
   - Register a new user and check the database for the stored password.
   - Verify that the password is hashed and not stored in plain text by searching for keywords like "password" in the database. Ex in django look for: User.objects.create_user(username='john', 'john@example.com`, 'password')


### Input Validation and Sanitization
10. **Test input validation:**
   Test cases 1:
   - Submit forms with valid inputs and check if the application accepts them.
   - Verify that the application validates inputs correctly and does not allow invalid data to be submitted.

   Test cases 2:
   - Submit forms with invalid inputs (e.g., incorrect formats, special characters).
   - Ensure the application displays relevant error messages and prevents submission of invalid data.
   

### Error Handling
11. **Check for unhandled errors:**
    - Navigate through the website and intentionally trigger potential error scenarios (e.g., invalid form submissions, accessing non-existent pages).
    - Ensure the application handles these errors gracefully and does not crash.

## Game Functionality

### Local Game Playability
12. **Test local game controls:**
    - Start a local game and check if both players can control their paddles using different sections of the keyboard.
    - Verify that player movements are responsive and match key presses.

### Tournament Matchmaking
13. **Test tournament initiation and matchmaking:**
    - Start a new tournament and enter aliases for multiple players.
    - Verify the matchmaking system correctly pairs players for matches.
    - Check that the tournament progresses through rounds as players win or lose.

### Game Rules and Controls
14. **Verify game replication and controls:**
    - Play a game to ensure it replicates the original Pong experience (e.g., paddle movement, ball physics).
    - Check for an end-game screen or message when a game concludes.
    - Ensure any game rules or controls are clearly explained or documented.

### Lag and Disconnection Handling
15. **Test lag and disconnection scenarios:**
    - Simulate a lag by intentionally slowing down the network connection.
    - Ensure the game handles lag without crashing (e.g., pauses the game, allows players to catch up).
    - Disconnect and reconnect a player to ensure the game resumes correctly without crashing.

## Module-Specific Tests

### Major Module 01 (example: AI Opponent)
16. **Verify AI opponent functionality:**
    - Play a game against the AI opponent to ensure it provides a challenging experience.
    - Check that the AI simulates human behavior and anticipates moves as required.

17. **Explain AI module:**
    - Prepare a detailed explanation of the AI opponent's implementation, including its logic and decision-making processes.

### Additional Modules (if implemented)
18. **Verify additional minor modules:**
    - Test the functionality of any additional minor modules, such as User and Game Stats Dashboards, by navigating to the relevant sections and checking for expected behavior.
    - Ensure dashboards display accurate and relevant st

### 403
* view отправляет 403
* django отправляет `403 Forbidden: CSRF verification failed`, при условии `SessionAuthentication`, CSRF-защита включена
* django отправляет `403 Forbidden: Authentication credentials were not provided`, при условии: `SessionAuthentication` в DRF или др
  + Нет `sessionid` в куках  
  + Пользователь не залогинен, запрашивает ресурс  
  + используешь `@login_required`, а юзер не авторизован
  + Сессия истекла или удалена  
* django отправляет 403
  + в коде raise PermissionDenied
  + `permission_classes = [IsAuthenticated]`

### БЕЗОПАСНОСТЬ
* use auth.js to check whether the user is authenticated. You can add it to your component and add data-auth-required tag to the html elements that require authentication. That’s how I do.
* identification = кто вы
  + определение личности пользователя (предоставление username, email)  
  + без проверки пароля
  + не вход в систему  
* authentification = докажите, что это вы
  + проверка личности, логин, система требует пароль, одноразовый код из SMS, отпечаток пальца, лицо, вход через соцсети, ...
  + stateless authentication
    - без сохранения состояния, сервер не хранит сессию
    - сервер проверяет подпись токена от OAuth-провайдера
    - JWT, OAuth 42 API, ...
  + stateful authentication
    - сервер хранит информацию о пользоватеях в памяти или бд
    - не подходит для масштабируемых приложений
  + DRF: проверка пользователя при каждом запросе через TokenAuthentication, SessionAuthentication, JWTAuthentication
    - каждый запрос содержит токен или cookie для подтверждения личности пользователя
    - контроль доступа через permissions (**`IsAuthenticated`**, ...)
    - сессии или JWT доступны в middleware или обработчиках запросов
    - Session storage: Если включён SessionMiddleware, то пользовательские данные сохраняюися в сессиях и доступны в обработчиках
    - можно без Session MiddleWareStack, если данные о сессии хранятся в зашифрованных cookie без серверного хранилища
    - views.py проверяет токен/подпись
* autorisation = что разрешено, к каким ресурсам имеете доступ 
  + basic auth, 42, гугл
  + при проверке ролей и прав через сессию система также участвует в авторизации (проверяет, имеет ли пользователь доступ к ресурсам, ...)
* объединить аутентификацию для DRF и Channels
  + JWT или SessionAuthentication
  + настроить middleware
* аутентификация через middleware, через стандартный механизм HTTP-запросов, например, с использованием **JWT или сессий**
* какой у нас тип
  - посмотреть auth, authentication, REST_FRAMEWORK, session, cookies, bearer, authorization, middleware, authentication classes   
  - F12 - network - headers, выполните login, посмотрите, что отправляется с клиентской стороны
  - заголовок `Authorization: Bearer <токен>` в последующих запросах => JWT (или др токен)
  - `Set-Cookie: sessionid=...` => сессионная аутентификация (Django Sessions, ...)
  - комбинированный вариант: JWT в куках, ...
* **checkAuthState()** ?
* **from rest_framework import authentication** ?
* функция **make password** django ?
* F12 network сокет: accept-encoding: **sec-websocket-key**: BNMxSoHDbO66J+298LsweQ== ?

#### 1. SECRET_KEY ключ django-проекта
* главный секретный ключ Django
* не доступен клиенту, скрыт на сервере 
  + нельзя публиковать, злоумышленник создаст фальшивые cookies, сессии, токены, подделает запросы, будет манипулировать приложением
* если сбрасываете все сессии и подписи, можно менять 
* используется 
  + для встроенных механизмов безопасности: **`django.contrib.auth`**, **`django.middleware.csrf`**  
  + при создании хэшей паролей
  + при создании CSRF-токенов,генерация подписи токенов CSRF
  + генерация подписи cookies
  + генерация подписи сессии
  + генерация подписи JWT
  + генерация случайных токенов (в механизмах сброса пароля, ...)

#### 2. SSL/TLS 
* не токен или ключ в традиционном понимании
* не связан с аутентификацией или авторизацией пользователя
* certBox сгенерировать свой сертификат
* шифрование ws-соединений (wss://)
* 1) Подготовка сертификата и настройка Nginx
  + генерируем локально **.csr** (запрос на подпись сертификата, Certificate Signing Request) и секретный ключ **.key**
    - .key хранится на сервере
  + отправляем .csr в удостоверяющий центр
  + получаем **.crt** (публичный ключ сервера) + подпись CA
* 2) Установка защищённого соединения (TLS-рукопожатие)
  + браузер отправляет запрос для установки HTTPS-соединения
  + nginx генерирует идентификатор сессии TLS **session id**
    - сохраняет у себя
    - отправляет браузеру, браузер сохраняет session id в оперативной памяти
  + nginx отправляет браузеру .crt
    - браузер сохраняет .crt  в кэше сессии
    - браузер сверяет с CA, подтверждает подлинность сервера (1-я функция SSL – Аутентификация сервера)
  + браузер генерирует **pre-master secret** (предварительный ключ для симметричного шифрования)  
    - шифрует с использованием .crt, отправляет серверу, не сохраняет
  + браузер генерирует **сессионный ключ** (симметричный ключ, symmetric keys, derived key) на основе pre-master secret (для шифрования трафика)
  + nginx расшифровывает pre-master secret с помощью`.key и .crt     
    - генерирует **сессионный ключ** на основе pre-master secret
    - не сохраняет
* 3) шифрование и проверка данных
  + TLS-слой браузера шифрует данные, используя симметричный ключ
  + TLS-слой браузера добавляет MAC (Message Authentication Code) для обеспечения целостности данных
  + браузер передаёт данные серверу по защищённому каналу
    - защищено от перехвата (XSS, CSRF, токены для ws, ...) — (2-я функция SSL – Шифрование данных)
  + TLS-слой nginx (ngx_http_ssl_module, библиотека OpenSSL) расшифровывает данные, используя session id
    - это ключ для дальнейшего симметричного шифрования данных
    - session id нужен для идентификации сессии при возобновлении соединения
  + TLS-слой nginx проверяет MAC (3-я функция SSL – защита от подделки данных, убедиться в целостности данных)
* 4) ответ сервера и завершение обмена
  + TLS-слой nginx шифрует ответ с использованием session id
    - CSRF-токены и их хеширование относятся к уровню приложения (Django), а не TLS
  + TLS-слой nginx добавляет MAC к ответу
  + Nginx отправляет зашифрованный ответ по защищённому каналу
  + TLS-слой браузера расшифровывает ответ, используя session ID
  + TLS-слой браузера проверяет MAC

#### 3. csrf (cross-site request forgery) ключ, токен
* криптографически защищённая случайная строка, уникальная для каждой сессии **или формы**
  - подписан с помощью SECRET_KEY  
* нужен:
  + validation for forms and any user input
  + validation for forms and any user input для формы на странице входа, если логин-запрос меняет сост. сервера (авто регистрация, ...) - можно для доп защиты
  + защита от подделки межсайтовых запросов через браузер (CSRF-атак): user вошел в аккаунт -> перешел на вредоносный сайт, вред. сайт отправляет запрос от user
  + когда браузер автоматически отправляет cookies с сессионным идентификатором
  + в stateful-приложениях (sessionid/ cookie автоматически отправляемые браузером для отслеживания пользователя)
  + если SPA обращается к API, использующему сессионные куки
  + для HTTP-запросов POST/PUT/DELETE: эти методы меняют состояние сервера
  + в stateless-приложениях с JWT-авторизацией, если JWT хранится в cookie не `SameSite=Strict`, то может понадобиться
* не нужен:
  + validation for forms and any user input в форме на странице входа (пользователь не аутентифицирован, CSRF-атака не имеет смысла)
  + с ws
  + для защиты между поддоменами - лучше CORS (POST request from subdomain.example.com from succeeding against api.example.com)
  + в stateless-приложениях с JWT-авторизацией не используется
    - если JWT хранится в `localStorage` или `sessionStorage` и отправляется в заголовке `Authorization`
  + GET-запросы CSRF-токена не требуют, так как они не изменяют данные
  + Не решает проблему авторизации
* CSRF_TRUSTED_ORIGINS = trusted origins for unsafe requests (e.g. POST)
  + a request includes the Origin header => header must match the origin present in the **Host header**
  + a **secure unsafe** request doesn’t include the Origin header => the request must have a **Referer header** that matches the origin present in the Host header
  + вхожу в /admin -> CSRF verification failed, https://localhost:4443 does not match any trusted origins
* https://docs.djangoproject.com/en/5.1/howto/csrf/
* 1. JS на фронте читает инпут, проверяет с помощью `regex` - валидация данных (проверка e-mail, пароля, ...), не имеет отношения к CSRF
* 2. запрос CSRF-токена:
  + либо django.middleware.csrf.get_token запрашивает  с `api/csrf-token/` или **при рендере HTML-страницы**
  + либо методы POST/PUT/DELETE при включённой CsrfViewMiddleware 
  + либо @ensure_csrf_cookie
* 3. django генерирует токен либо берёт существущий в cookie/сессии
  + отдаёт клиенту токен: устанавливает cookie `csrftoken` через **`@ensure_csrf_cookie`** (предпочтительнее)
  + только для чтения клиентом
  + **`SameSite=Lax` или `Strict`** — предотвращает межсайтовые запросы
  + `Secure` — передаётся только по HTTPS
  + токен остаётся в cookie до ротации (логин в другой вкладке, нажатие "Назад", обновление страницы)
  + Django не хранит токен на сервере
  + есть второй вариант - отдать токен не в куки, в явно в формате JSON, но нам кажется это не нужно
* 4. второй запрос клиента: js обращается к `/profile`, `/chat/`, ...
  + CSRF-токен в заголовке `X-CSRFToken` для `POST`, `PUT`, `PATCH`, `DELETE` запросов
  + злоумышленник не может получить доступ к cookie, Cookie отправляется автоматически браузером только для исходного домена
  + другие сайты не могут прочитать `csrftoken` из браузера, если настроен `SameSite=Lax` или `Strict`
  + другой сайт не может прочитать cookie из браузера пользователя из-за политики одного источника (Same-Origin Policy) 
  + чтобы создать CSRF-токен, злоумышленнику нужен SECRET_KEY, без него токен не пройдет валидацию, злоумышленник не знает токена, соответствующего подписи
5. `CsrfViewMiddleware` глобальная CSRF-защита
  + не читает токен из файлов, ищет его:  
    - в заголовке (request.headers['X-CSRFToken']) или скрытом поле формы
    - из памяти текущего запроса (`request.COOKIES['csrftoken']`/`request.session`)
    - если session-based CSRF, то **в памяти или бд, которую Django использует для хранения сессий** (`django_session`, ...)
    - ✅ **Если используется сессия (`SessionMiddleware`)**, CSRF-токен хранится на стороне сервера — в данных сессии (в базе данных или Redis).
    - ✅ **Если используется cookie-only защита** (по умолчанию в Django с `CsrfViewMiddleware`), защита основана на том, что:  
  + проверяет совпадение токена из запроса и из cookie/сессии
    - `django.middleware.csrf._does_token_match` сравнение за константное время, предотвратить атаки по времени
    - сравнение происходит не просто по значению токена, а с криптографической проверкой, используя `SECRET_KEY`
  + сервер пересчитывает хеш и сравнивает его с пришедшим токеном
  + принимает только запрос от авторизованного пользователя, с тем же Origin т.е. с того же сайта
  + // эту же работу может делать декоратор `@csrf_protect`
  + **Referer** проверяется для запросов по HTTPS, чтобы избежать **атак через смешанный контент**
* 6. `views.py` дополнительные проверки (авторизация, валидацию данных, др), не имеет отношения к CSRF

#### 3. oauth
* протокол авторизации
* авторизация пользователя через сторонний сервис, не передавая логин и пароль приложению  
* не надо хранить пароли
* разновидность токен-ориентированной аутентификации
* OAuth и CSRF решают разные задачи
* OAuth и CSRF имеют некоторые схожие механизмы
  + особенно при использовании OAuth 2.0 в веб-приложениях
  + В OAuth 2.0 часто используется параметр `state`, который выполняет роль CSRF-токена:  
    - Когда приложение перенаправляет пользователя на страницу авторизации, оно генерирует случайную строку (`state`) и сохраняет её в сессии.  
    - После авторизации сторонний сервис возвращает `state` вместе с кодом авторизации.  
    - Приложение проверяет, совпадает ли вернувшийся `state` с сохранённым значением.  
    - Если `state` совпадает, это подтверждает, что запрос на авторизацию был инициирован именно этим приложением, а не злоумышленником.
* CSRF Основан на проверке CSRF-токена между клиентом и сервером
* OAuth 2.0 Использует параметр `state` для предотвращения CSRF-атак во время авторизации
* CSRF Важен при любом изменяющем запросе (`POST`, `PUT`, `DELETE`)
* OAuth 2.0 Применяется в момент обмена кодом авторизации на access token
* Основные шаги:  
  1. Клиент перенаправляет пользователя на страницу авторизации стороннего сервиса.  
  2. Пользователь авторизуется и подтверждает доступ.  
  3. Сторонний сервис перенаправляет пользователя обратно в приложение с временным кодом.  
  4. Клиентское приложение обменивает код на **access token** для выполнения запросов от имени пользователя.  
* защита от CSRF встроена через использование параметра `state`
* реализуй проверку `state`
  + **django библиотеки для OAuth (`django-allauth`) делают это автоматически** 
* 1) аутентификация через OAuth2, пользователь логинится через 42
* 2) данные пользователя (логин, email, имя, аватар) извлекаются и сохраняются в базе данных
* 3) OAuth-сервер выдаёт токен
* 3) клиент получает токен доступа Bearer Token Authentication
  + получение Токенов
    - frontend перенаправляет пользователя на OAuth-провайдера **через Backend**
    - OAuth-провайдер перенаправляет пользователя обратно на Backend с кодом авторизации
    - backend **обменивает** **код авторизации** на Access Token и Refresh Token
* 4) токен сохраняется в сессии пользователя `request.session.get('access_token')`
* 5) для запроса к защищенному маршруту `/v2/me` токен передается в заголовке `Authorization: Bearer <access_token>`
* 5) Frontend отправляет запросы к Backend
  + backend использует токены для взаимодействия с внешними сервисами без передачи токенов на Frontend
  + токены не покидают Backend
+ если хранится в Local Storage
  - простота реализации
  - данные остаются после перезагрузки страницы
  - злоумышленник может украсть токен через js-код (**XSS-атака**)
+ если хранитсяв Session Storage
  - простота реализации
  - данные удаляются при закрытии вкладки, недоступны между вкладками
  - уязвимо к XSS-атакам
+ если хранится в памяти приложения (In-Memory), в оперативной памяти клиента (в переменной в js, ...)
  - токен исчезает при обновлении страницы
  - требует реализации для повторного получения токена
  - защита от XSS, поскольку токен недоступен после перезагрузки страницы
+ если хранится на стороне сервера в базе данных (**Access Token, Refresh Token**)
  - безопасность: токен недоступен для клиента
  - удобное управление сроком действия и обновлением токенов
  - медленнее
  - дополнительная обработка
  - сохраняйте **Access Token** и **Refresh Token** в **защищённой таблице**, связанной с пользователем
  - шифруйте токены перед сохранением в базу данных
  - ограничить доступ к таблице с токенами необходимыми сервисами
+ если хранится на стороне сервера в кэше Redis 
  - быстрый доступ
  - легко управлять истечением токенов (TTL)
  - данные очищаются при сбое или перезагрузке
+ если хранится на стороне сервера в HTTP-only cookies
  - Токен (или **сессионный идентификатор, связанный с токеном**) передаётся клиенту в виде HTTP-only cookie
  - браузер отправляет cookies на каждый запрос
  - Требует настройки `SameSite`, `Secure`, HTTPS
  - защищён от XSS-атак (недоступен через js)
+ гибридный подход
  - Access Token в памяти клиента (`In-Memory`) для минимизации уязвимости к XSS
  - Refresh Token в HTTP-only cookie для долгосрочного хранения и обновления Access Token
+ **где у нас?**
    | **Место хранения**        | **Безопасность**      | **Удобство** | **Долговечность** |
    |---------------------------|-----------------------|--------------|-------------------|
    | Local Storage             | Уязвимо к XSS         | Высокое      | Высокая           |
    | Session Storage           | Уязвимо к XSS         | Среднее      | Средняя           |
    | In-Memory                 | Защищено от XSS       | Низкое       | Низкая            |
    | База данных (сервер)      | Очень высокая         | Низкое       | Высокая           |
    | Redis (сервер)            | Высокая               | Высокое      | Средняя           |
    | HTTP-only cookies         | Защищено от XSS/CSRF  | Высокое      | Средняя/высокая   |
  + если важно удобство разработки: Local Storage или Session Storage
  + если требуется безопасность: храните Access и Refresh Tokens на сервере (в бд или Redis), используйте HTTP-only cookies для передачи сессионных данных
  + **Frontend не хранит токены**
    - frontend использует безопасные сессии через HTTP-only cookies для взаимодействия с Backend
  + кэшируйте Access Tokens в Redis для быстрого доступа, особенно если Backend часто обращается к внешним API
  + используйте Redis для хранения сессий пользователей, **связывая их с соответствующими OAuth-токенами**
  + как Backend взаимодействует с геолокационными сервисами
    - извлекает Access Token из базы данных или Redis
    - сервисы или модули на Backend, которые отвечают за взаимодействие с Google Maps API, используют Access Tokens для аутентификации запросов
    - Геолокационные компоненты на Backend используют сохранённые токены для взаимодействия с внешними API
  + **backend отвечает за OAuth-поток**, включая получение, хранение и обновление токенов
  + **токены хранятся в защищённой бд** и кэшируются в Redis для быстрого доступа
  + настройте заголовки для безопасности (**`Strict-Transport-Security`**, ...)
  + безопасность токенов через **шифрование, ограничение доступа, использование защищённых методов передачи данных**
* хранится: cookie или localStorage
* API-ключи
  + OAuth: пользователи логинятся в твой проект, используя учетные данные 42
    - зарегистрировать приложение в 42 и получить данные для подключения: (API-ключ (как токен)) или (клиентский ID и секрет)
  + клиент хранит ключ, передаётся в загол, сервер проверяет его в бд  
  + удобно для сервисов, API
  + можно украсть ключ
  + Назначение: идентификация, авторизация стор прилож
  + Где хранится: HTTP заголовок запроса
  + передаётся с каждым запросом
  + ограниченный доступ к API
* Implementing a remote authentication OAuth 2.0 authentication with 42 (subject)
  + obtain the credentials and permissions from the authority to enable a secure login
  + user-friendly login and authorization flows that adhere to best practices and security standards
  + the secure exchange of authentication tokens and user information between the web application and the authentication provider
* токен имеет ограниченный срок действия
  + автоматическое обновление Access Tokens с помощью **Refresh Tokens**
  + **секретный ключ от api раз в две недели примерно обновляется**
  + реализуйте механизмы для **аннулирования токенов**

#### 3) protected against **SQL injections/XSS**

#### 5) ensure your API-routes are protected
* even if you decide not to use JWT tokens
 
#### 8) сессионный ключ = сессионный идентификатор = файл сессии
1) после входа сервер создает сессию, привязнную к пользователю
1) сервер выдаёт клиенту  `session_id` 
2) сервер хранит сессию - см. SESSION_ENGINE
  + бд в таблице django_session (по умолчанию)
  + файловая система в виде файлов
  + в кеше `CACHES`
  + кеш с fallback на бд: в кеш, при его недоступности — в бд
3) в браузере защифрованная куки sessionId
  + идентификатор сессии
  + отслеживание состояния пользователя на сервере
    - например, для аутентификации или сохранения данных между запросами
  - `HttpOnly`, чтобы предотвратить доступ из js
  - `Secure`
  - настройте `SESSION_COOKIE_AGE`
4) при каждом запросе браузер отправляет `sessionid`
5) django использует `sessionid` для извлечения данных из хранилища сессий 
* долговременная связь пользователя с сервером
* сессии управляются с помощью cookie-сессионных данных или токенов для аутентификации API
* подходит для stateful-приложений (например, с веб-интерфейсом), подходит для **веб-интерфейсов**, Веб-приложения с авторизацией
* **неудобно для API**
* например: отслеживание состояния авторизации в веб-приложении; корзина

#### ws token
* проверка прав доступа и идентификация пользователя
* авторизация при установке ws-соедине
* аутентификация один раз при установлении ws-соединения
  + middleware (`AuthMiddlewareStack`) для сопоставления пользователя
  + клиент передаёт токен в URL или заголовках `wss://example.com/ws/chat/?token=<your-token>`
  + поддерживает передачу аутентификационных данных через Cookie (`sessionid`, ...) или токены
* передача защищена (через HTTPS/WSS, ...)
* могут быть основаны на JWT или другом формате
* Где хранится: URL параметр / заголовок ws-запроса
* передаётся при установке соединения
* проверка прав доступа
* пример: авторизация чата, потоковой системы `wss://example.com/ws/chat/?token=<...>`
* ws соединения устанавливаются через начальный HTTP-запрос (`GET`)
  + после рукопожатия соединение переходит на другой протокол
  + там CSRF-атаки не применимы, так как WebSocket:  
    - не поддерживает автоматическую отправку данных браузером (в отличие от cookies с HTTP-запросами)
    - требует явного открытия соединения клиентским кодом
* вместо CSRF:
  + Проверка токена аутентификации** (например, JWT в URL или в заголовках при `handshake`)
  + Использование `Origin` заголовка** — проверяйте, что соединение устанавливается с доверенного источника
  + Сессии Django** — если используете `channels`, можно проверять сессию пользователя в `consumers.py`
* передавать токен через параметры URL или при инициализации WebSocket-соединения

#### 2) any password stored in your database, if applicable, must be hashed
* a strong password hashing algorithm (subject)
  + асимметричный алгоритм (**RSA**, ...) - ?
* пароли пользователей  автоматически хешируются, если вы используете стандартную модель пользователя или наследуетесь от AbstractUser
* console: {id: 5, password: 'qsqsqsqs' **пользователь добавлен через админ**

#### базовая аутентификация (Basic Auth) - у нас нету
* user вводит логин/пароль, сервер создаёт сессию, логин/пароль передаются в заголовке при каждом запросе, слабо защищён
 
#### идентификация RFC 1413 - у нас нету
* устаревший сетевой протокол, определение имени владельца запущенного на клиентской машине процесса

#### токен авторизации (чаще всего JWT) - у нас нету
* jwt = формат токена
* самодостаточен, данные для проверки токена содержатся в нём
  - не требует хранения состояния на сервере (кроме валидации)
  - подписан сервером, использует подпись, чтобы предотвратить изменения данных
* для Веб-приложений, API, ws, SPA, мобильных приложений, микросервисов
1) **служба аутентификации** хранит закрытый ключ и может выдавать токены
1) сервер проверяет данные и создаёт JWT, содержащий информацию о пользователе (id, имя, ...)
1) хранится в HTTP заголовках, cookie, localStorage, sessionStorage
  - хранится в браузере => безопасно **почему у нас не так?**
  - самый простой и безопасный — **cookie**, все микросервисы имеют одно и то же имя хоста => cookie отправляется браузером при каждом запросе
1) хранится аутентификационная информация в localStorage`/`sessionStorage`, куки
2) клиент получает токен
3) клиент отправляет токен в заголовке `Authorization: Bearer <token>`, в заголовках ws-сообщений, в параметрах URL
4) сервер декодирует токен, проверяет подпись, использует данные для авторизации (проверяет роль пользователя, ...)
* другие микросервисы проверяют с помощью **открытого ключа**
* обеспечивается через middleware Channels
* обрабатывается классом аутентификации
* обрабатывать его в consumer
* можно без Session MiddleWareStack, если JWT токены или др безсессионные методы аутентификации (например, в **DRF сессии не нужны**, авторизация через токены)
* можно **отключить SessionMiddleware и CsrfViewMiddleware**, если только JWT
* не нужен CSRF-токен
* не нужны сессии
* если украдён, злоумышленник может использовать его до истечения срока действия: устанавливайте короткий срок действия токена
  + реализуйте механизмы обновления и отзыва токенов
    - отзыв сложен
* https://django-rest-framework-simplejwt.readthedocs.io/en/latest/getting_started.html
* https://dzone.com/articles/using-jwt-in-a-microservice-architecture
* https://openclassrooms.com/fr/courses/7192416-mise-en-place-une-api-avec-django-rest-framework/7424720-give-access-with-the-tokens
Назначение                          | Где хранится                         | Особенности | пример |
авторизация? аутентификация?        |HTTP-заг cookie locStorage sessStorage| Используется для REST API и WebSocket; может быть JWT | авторизация REST API (`Authorization: Bearer <token>`) или ws-соединения; REST API, GraphQL, WebSockets |
### Как понять, что JWT-токен проверяется через **middleware** в Django Channels?  
В `ASGI_APPLICATION` должно быть указано `channels.routing.application`, например:  

```python
ASGI_APPLICATION = "your_project.routing.application"
```

Также в `INSTALLED_APPS` должен быть `channels`:  

```python
INSTALLED_APPS = [
    "channels",
    "rest_framework",
    "your_app",
]
```

Если в `settings.py` нет `ASGI_APPLICATION`, значит, Channels не используются.  


#### 2️⃣ Проверьте **`middleware` в Channels**  
В **`routing.py`** или `asgi.py` может быть что-то вроде:  
```python
from channels.auth import AuthMiddlewareStack
from channels.routing import ProtocolTypeRouter, URLRouter
from your_app.middleware import JWTAuthMiddleware
import your_app.routing

application = ProtocolTypeRouter({
    "http": get_asgi_application(),
    "websocket": JWTAuthMiddleware(
        AuthMiddlewareStack(
            URLRouter(your_app.routing.websocket_urlpatterns)
        )
    ),
})
```

Обратите внимание на **`JWTAuthMiddleware`** – это кастомное middleware, которое обрабатывает JWT.

---

#### 3️⃣ Проверьте **реализацию `JWTAuthMiddleware`**  
Оно может находиться, например, в `your_app/middleware.py`:  

```python
from urllib.parse import parse_qs
from channels.middleware.base import BaseMiddleware
from django.contrib.auth.models import AnonymousUser
import jwt
from django.conf import settings
from asgiref.sync import sync_to_async
from your_app.models import UserProfile  # Важно: использовать get_user_model(), если User кастомный

class JWTAuthMiddleware(BaseMiddleware):
    async def __call__(self, scope, receive, send):
        query_string = scope.get("query_string").decode()
        token = parse_qs(query_string).get("token", [None])[0]

        if token:
            try:
                decoded_data = jwt.decode(token, settings.SECRET_KEY, algorithms=["HS256"])
                user = await sync_to_async(UserProfile.objects.get)(id=decoded_data["user_id"])
                scope["user"] = user
            except (jwt.ExpiredSignatureError, jwt.InvalidTokenError, UserProfile.DoesNotExist):
                scope["user"] = AnonymousUser()
        else:
            scope["user"] = AnonymousUser()

        return await super().__call__(scope, receive, send)
```

Если в проекте есть подобный **middleware**, значит, **JWT-токен проверяется в Channels через middleware**.  

---

### 🔥 Как протестировать?  
Попробуйте подключиться к WebSocket:  
```javascript
const socket = new WebSocket("ws://localhost:8000/ws/chat/?token=your_jwt_token");
```
Если токен валидный – соединение установится.  
Если токен не передан или неверный – сервер может разорвать соединение или вернуть ошибку.  

---

### **Вывод:**  
Если в `routing.py` используется `AuthMiddlewareStack` с кастомным `JWTAuthMiddleware`, а в `middleware.py` есть логика декодирования JWT, то **аутентификация через JWT обеспечивается middleware в Django Channels**.


#### ограничить доступ к страницам в Transcendence  
* Alexey:
  + You could use auth.js to check whether the user is authenticated
  + You can add it to your component and add data-auth-required tag to the html elements that require authentication
  + That’s how I do
* Alexey:
  + На беке чтобы ограничить доступ нужно просто использовать декоратор login_required
  ° На фронте я добавляю в html элемент тэг data-auth-required, потом проверяем на фронте с помощью AuthSevice залогинены мы или нет и зависимости от этого рендерим или нет
  + Еще сейчас понял что если мы захотим перейти не по ссылке, а через адрес вручную, то нужно добавить проверку на авторизованность в конструктор компонента
* API routes protected
  + проверяется, что пользователь имеет право доступа к этим данным или ресурсам (администратор, сам пользователь, ...)
  + для доступа к защищенным маршрутам нужно передавать валидный токен (например, JWT), который подтверждает, что пользователь авторизован
  + в Django - с помощью middleware или декораторов
    - @login_required
    - или через настройку JWT для API с использованием таких пакетов, как djangorestframework-simplejwt
* 'DEFAULT_AUTHENTICATION_CLASSES': ['rest_framework.authentication.SessionAuthentication']
  + без этой настройки              ['rest_framework.authentication.SessionAuthentication', 'rest_framework.authentication.BasicAuthentication']
* 'DEFAULT_PERMISSION_CLASSES':     ['rest_framework.permissions.IsAuthenticated'] 
  + без этой настройки              ['rest_framework.permissions.AllowAny']
* с 'DEFAULT_RENDERER_CLASSES':     ['rest_framework.renderers.JSONRenderer', 'rest_framework.renderers.BrowsableAPIRenderer']
  + без этой настройки то же самое
* `LoginRequiredMixin` для CBV
* LOGIN_URL = "/login/" # eliminates the need to pass login_url in every @login_required and LoginRequiredMixin
* @login_required(login_url="/login/") для FBV  
  - используется для функций
* `index` открыта:
+ urls.py
  path("", index, name="index"),  # Главная страница (открытая)
  path("protected/", protected_page, name="protected"),  # Доступ только залогиненным
  path("protected-cbv/", ProtectedPageView.as_view(), name="protected_cbv"),  # CBV-версия
* db        | initdb: warning: enabling "trust" authentication for local connections
  db        | initdb: hint: You can change this by editing pg_hba.conf or using the option -A, or --auth-local and --auth-host, the next time you run initdb.
* только залогиненные могут просматривать `StatsPage` - три инструмента на разных уровнях
  + 1) проверять аутентификацию перед рендерингом в `StatsPage`  
    - добавь `checkAuthentication()` в конструкторе, запрашивать данные текущего пользователя
      ```
      async checkAuthentication() {
          try {
              const response = await fetch('/auth/me', { method: 'GET' }); // Предполагается, что есть API /auth/me
              if (!response.ok) throw new Error('Not authenticated');
              const user = await response.json();
              this.setState({ currentUser: user });
              this.fetchGames();
          } catch (error) {
              console.error('Authentication error:', error);
              window.location.href = '/login'; // Перенаправление на страницу входа
          }
      }
      ```
  + 2) скрывать контент, если пользователь не залогинен
    ```javascript
    render() {
        if (!this.state.currentUser) {
            return '<p>Loading...</p>'; // Или ничего не рендерить
        }
        // код рендера
    }
    ```
  + 3) защитить API на сервере
    - если кто-то вручную запрашивает `/games/`, `/users/`, сервер не отдаст незалогиненному
* токены (JWT, sessionid) передаются внутри защищённого соединения, чтобы их не могли перехватить злоумышленники

#### комбинированные методы  
+ session + CSRF токен, или JWT в куке
+ для повышения безопасности


### COOKIE LOCALSTORAGE SESSIONSTORAGE
|                 | **Cookie**                           | **LocalStorage**     | **SessionStorage**           |
|---------------|------------------------------------|---------------------|------------------------------|
| **Хранение данных** | В браузере, отправляются серверу | Только в браузере   | Только в браузере            |
| **Срок действия**   | До истечения срока или закрытия браузера | Постоянное         | До закрытия вкладки/окна     |
| **Доступ к данным** | JS и сервер (если `HttpOnly` нет) | Только JS          | Только JS                    |
| **Область действия** | Общая для всего домена         | Общая для домена    | Изолирована для вкладки       |

* Cookie
  + отправляются серверу при каждом HTTP-запросе на соответствующий домен
  + использование:
    - хранение сессионных идентификаторов (`sessionid`)
    - для сессионных данных, которые требуют взаимодействия с сервером
    - **маркеры аутентификации**
    - настройки сайта (язык)
    - передача данных между клиентом и сервером (аутентификация, ...)
* localStorage
  + долговременное хранение
  + на устройстве пользователя, изолированно для каждого домена
  + сохраняются после закрытия браузера или перезагрузки устройства
  + не передаётся серверу
  + доступны только для того домена, который их создал
  + доступны только через js
  + использование: 
    - хранение пользовательских предпочтений (тема сайта)
    - кеширование данных (JSON-ответы от API)
    - для долговременного хранения данных, доступных только клиенту (например, настройки интерфейса)
* SessionStorage
  + похож на localStorage
  + данные хранятся только на время текущей сессии браузера
  + изолированно для текущей вкладки или окна
  + данные удаляются, как только вкладка или окно браузера закрываются
  + доступны только через js
  + доступны только для вкладки или окна, где они были созданы
  + использование
    - хранение временных данных для одной сессии пользователя
    - **статус пользователя** на текущей странице (выбранные фильтры)
    - для временных данных, которые нужны только на время одной сессии пользователя

#### ПРОТОКОЛЫ
* прикладные = как данные кодируются и интерпретируются
  + HTTP между браузером и сервером в открытом виде
  + HTTPS шифрование через протокол SSL/TLS
    - HTTPS connection for all aspects (subject)
    - wss instead of ws (subject)
    - DONE excl livechat
    - **http2 ?** 
  + Redis
  + PostgreSQL
* транспортные, сетевые протоколы для передачи данных на порту
  + TCP Transmission Control Protocol: надежность соединения, без потерь, в нужном порядке, без дублирования
    - используется в HTTP, HTTPS, подключение к PostgreSQL, redis
  + UDP User Datagram Protocol: менее надежен, но быстрее

### modules
0.5 Django 
0.5 bootstrap
1.0 database
1.0 User management
1.0 OAuth
1.0 Remote players
1.0 LiveChat
1.0 AI Opponent 

module                        | front   | back 
------------------------------|---------|------   
user management 1             | A       | L  
matchmaking system Tournament | L       | L      
live chat 1                   | B       | B
------------------------------|---------|------   
live pong game on website     | L       | Amine
rules of Pong                 | Amine   | Amine     
remote players 1              | ---     | Amine  
AI Opponent 1                 | ---     | Amine  
------------------------------|---------|------   
security                      |         | 
registration                  | +       | +       
basic front-end               | +       | ---
OAuth 1                       | ---     | +
django 1                      | ---     | +
bootstrap 0.5                 | L       | ---
database 0.5                  | ---     | +


### TEST
  + https://localhost:4443/` откуда пользователь перешёл
* endpoints
  + `views.py`: какие представления и какие URL ассоциированы с функциями или классами в разных частях проекта
  + postman
    - импортируйте коллекцию эндпоинтов, если она уже создана  
    - отправляйте запросы на `/api/`, `/swagger/`, ... исследуя доступные маршруты
    - `http://localhost:8000/api/endpoint/` endpoints HTTP (API или страницы)
    - метод (GET, POST, PUT, DELETE и т. д.)
    - если требуется авторизация, добавьте токен или данные пользователя (если используете `Token` или `JWT`)
* Django Channels + `pytest` : ws-тесты
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
  + проверить, что Nginx видит заголовок Upgrade
    - `curl -i -N -H "Connection: Upgrade" -H "Upgrade: websocket" -H "Host: localhost:4443" -H "Origin: https://localhost:4443" --insecure  https://localhost:4443/ws/chat/test/`
    - Если Nginx проксирует правильно, то `HTTP/1.1 101 Switching Protocols, Upgrade: websocket, Connection: Upgrade`
    - Если сразу HTTP/1.1 404: nginx не зашёл в нужный location


### LIVE CHAT
* зачем общий чат
* чат будет отдельным компонентом, не страницей
  +  должен быть на каждой странице, либо на странице с игрой
* the user sends direct messages to other users (subject)
* the user blocks other users, they see no more messages from the account they blocked (subject)
* the user invites other users to play through the chat interface (subject)
* the tournament warns users expected for the next game (subject)
* the user access other players profiles through the chat interface (subject)
* с каждым пользователем у бэкенда 2 вебсовета: чат, положение ракетки
* 1 эндпоинт слушает чат
  + в заголовке запроса - кому сообщение
* если сообщение не дошло, пользователя нет
* сделать базовый функционал чата
  + Обновите шаблоны HTML
  + База данных: модель для хранения сообщений
  + сохранение сообщений в `ChatConsumer` при получении данных
  + сообщения передаются через каналы
  + проверьте сохранение сообщений в базе данных
* аутентификация: проверка перед подключением к ws, в ChatConsumer self.scope['user'] для доступа к объекту пользователя
* | Method                | Real-Time | Message Persistence  | Use Case                           | Complexity |
  |-----------------------|-----------|----------------------|------------------------------------|------------|
  | WebSocket (chat)      | Yes       | No                   | Real-time chats, games             | High       |
  | Redis (Pub/Sub, ws)   | Yes       | No                   | Notifications, chats               | High       |
  | Django Messages       | No        | Yes                  | System notifications, confirmations| Low        |
  | REST API              | No        | Yes                  | Simple notifications, data requests| Low        |
  | Email                 | No        | Yes                  | Important notifications, confirmations| Medium  |
  | Push Notifications    | Yes       | Yes (by service)     | Mobile device notifications         | Medium     |
* варианты хранения сообщений чата  
  + Оптимальный вариант для большинства проектов: PostgreSQL (основное хранилище) + Redis (для кеша и ускорения)
  + База данных: надёжное долговременное хранение, полный контроль и история сообщений
  + Redis: быстрое временное хранение в памяти, можно использовать как кэш.  
    - Если чат временный (например, анонимный чат, данные стираются со временем).  
  + Файл на диске (лог-файлы, JSON, CSV): простое хранение, сложно организовать быстрый поиск, редко удобно  
  + Elasticsearch (если нужен поиск по сообщениям) Хорош для чатов с большим объёмом данных.
    - Если требуется поиск по сообщениям → Elasticsearch + база данных  
  + Kafka (или RabbitMQ) Если нужно передавать сообщения между сервисами, но не для долговременного хранения.  
 

### AVATARS
+ picture_url = models.URLField(max_length=300, blank=True, null=True) #if avatar is not available - we can use picture URL
+ avatar = models.ImageField(**upload_to='avatars/'**, blank=True, null=True) #if avatar is not available - we can use picture URL
+ в отдельном томе `media_django_volume`
  - статические файлы не изменяются после деплоя  
  - загружаемые пользователями аватарки меняются и не должны попадать в систему контроля версий
  - статика кешируется и может раздаваться напрямую через Nginx  
  - аватарки требуют авторизации, что удобнее обрабатывать через Django
  - `volumes: media_django_volume`
  - backend: volumes: media_django_volume:/app/media, nginx: volumes: media_django_volume:/usr/share/nginx/media
  - MEDIA_URL = '/media/', MEDIA_ROOT = os.path.join(BASE_DIR, 'media')
  - location /media/ { alias /usr/share/nginx/media/; }
+ локальное хранение (Media Storage)
  - для Малых и средних проектов  
  - простота настройки и отладки  
  - `settings.py`: MEDIA_URL MEDIA_ROOT 
  - **чистить старые файлы, если аватарка обновляется**
+ в базе данных (Base64, BLOB) НЕ рекомендуется
  - Долгая загрузка аватарок
  - Сложно кэшировать
+ S3-совместимое хранилище (AWS S3, DigitalOcean Spaces, MinIO)
  - Для продакшена лучший вариант
  - Подходит для Масштабируемых проектов  
  - pip install django-storages[boto3]
  - `settings.py`: DEFAULT_FILE_STORAGE, AWS_ACCESS_KEY_ID, AWS_SECRET_ACCESS_KEY, AWS_STORAGE_BUCKET_NAME, AWS_S3_REGION_NAME, AWS_S3_CUSTOM_DOMAIN, MEDIA_URL
  - Не нагружает сервер
  - Простая интеграция с CDN
+ внешних сервисов (Gravatar, Cloudinary)
  - Быстрого решения     
  - Минимального кода  
  - Не нагружает сервер
  - Автоматическая оптимизация изображений
  - Данные хранятся у сторонних сервисов

| Вариант | Простота | Скорость | Масштабируемость | Рекомендуется? |
|---------|---------|----------|-----------------|---------------|
| Локальное хранение | ✅ Простое | ⚡ Быстрое | ❌ Ограничено сервером | 👍 Подходит для небольших проектов |
| S3 (AWS, DigitalOcean) | ❌ Сложнее | ⚡ Быстрое | ✅ Хорошо масштабируется | 🔥 Лучший вариант для продакшена |
| База данных | ❌ Плохо | ❌ Медленно | ❌ Плохо масштабируется | 🚫 Не рекомендуется |
| Gravatar / Cloudinary | ✅ Очень просто | ⚡ Быстрое | ✅ Хорошо масштабируется | 👍 Отлично, если вас устраивает внешний сервис |


### BROWSER
* внутри браузера нет отдельного сервера, который перехватывает и обрабатывает запросы от js
* браузер сам инициирует запросы и обрабатывает их через HTTP(S) протокол, направляя их на сервер
* использует встроенные механизмы и API:
  + XMLHttpRequest или Fetch API: Эти технологии позволяют js-ом на странице браузера отправлять запросы к серверу
  + WebSocket: если приложение использует WebSocket для постоянного соединения, то браузер также инициирует это соединение с сервером, используя встроенный **WebSocket API**, который поддерживается всеми современными браузерами
* последовательность:
  + js на странице делает запрос (например, к Daphne через HTTP или WebSocket)
  + браузер инициирует этот запрос.
  + Запрос отправляется на сервер Django или Daphne
  + сервер обрабатывает запрос (маршрутизирует его в соответствующий обработчик или API, ...)
  + ответ от сервера возвращается в браузер
  + js обрабатывает ответ в браузере


### FRONTEND NGINX
* compatible with the latest stable up-to-date version of Google Chrome (subject)
* The user should be able to use the Back and Forward buttons of the browser (subject)
* try using **bolt.new** it's better at frontend
  + Bolt.new – инструмент или библиотеку для фронтенда
  + the ui is fire here  Дизайн пользовательского интерфейса выглядит очень круто
* **game customization** it's just gonna be front
  + like custom colors custom map
* проверяет сертификаты SSL, расшифровывает с использованием SSL-сертификата
  + внутренний трафик не шифруется
  + если сертификат самоподписанный, браузер может выдавать предупреждение «Not secure»
    - у вас: «Non sécurisé»
    - иногда это мешает wss://‐подключению
    - в большинстве случаев всё равно WebSocket должен подключиться
    - если браузер категорически отказывается, добавить исключение для самоподписанного сертификата
* запросы к статическим файлам: отдает из /usr/share/nginx/html/static/
* запросы на /api/ (JSON, HTML, WebSocket) идут на Django (аутентификаци, получения/отправки данных)
  + `proxy_set_header ...` передает заголовки (Host, IP-адрес клиента, протокол, ...), чтобы бэкенд понимал, откуда запрос
  + **сохраняет заголовки ws**
* фронт самописный или с использованием минимальных библиотек (jQuery, Bootstrap, ...), не на SPA-фреймворке
* балансировщик нагрузки (если много сообщений, **что будет**?)
* в модели пользователя аватарки: **у фронтэнда есть требования к аватаркам или они могут сами отрисовывать?** вдруг пользователь сохранит свою фотографию 1000 х 1000 пикселей, на фронте вы сможете отрисовать аватарку ?
* четыре server{}-блока = один процесс
* proxy_http_version 1.1: ws‐подключения для апгрейда используют HTTP/1.1, если оставить по умолчанию HTTP/1.0, Nginx не передаcn заголовки Upgrade и Connection: upgrade
* locaiton /ws/
  proxy_http_version 1.1; позволяет Upgrade c WebSocket (HTTP/1.0 не умеет)
  proxy_set_header Upgrade $http_upgrade; Передаём клиентский заголовок Upgrade: websocket дальше на backend.
  proxy_set_header Connection "upgrade"; То же для Connection: upgrade. 
  proxy_set_header Host $host; Пробрасываем заголовок Host, чтобы backend знал исходный хост (например, для ALLOWED_HOSTS или логики).
  proxy_set_header X-Real-IP $remote_addr; Передаём реальный IP клиента.
  proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for; Цепочка IP-адресов: чтобы приложение знало, кто настоящий клиент, если есть несколько прокси.
  proxy_set_header X-Forwarded-Proto $scheme; Показывает, что изначальный протокол был https (или http).
  proxy_read_timeout 3600s; proxy_send_timeout 3600s; Увеличиваем таймауты для WebSocket: чтобы не обрывать длинные соединения.
* location /
  proxy_http_version 1.1; позволяет использовать некоторые фичи: chunked transfer encoding, keep‐alive соединения, ..., рекомендуют, не мешает простым запросам, может улучшить поведение прокси (поддержка chunked‐ответов от бэкенда, ...)
  proxy_set_header Host $host; Сохраняем оригинальный хост в заголовке Host
  proxy_set_header X-Real-IP $remote_addr;
  proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for; 
  proxy_set_header X-Forwarded-Proto $scheme; 
  В proxy_set_header X-... и Host нужно, чтобы Django видел правильные заголовки и знал, что мы за SSL/TLS, реальный IP, ...
* Amine: game front using js
* alexey: Layout on the pages – working on it
  + расположение и структура элементов пользовательского интерфейса на веб-страницах
  + Работа с CSS-фреймворками (например, Tailwind CSS или Bootstrap)
* SSL Labs проверить корректность настройки SSL
* **Карточка Bootstrap**  
* "Failed to load module script: Expected a JavaScript module script but the server responded with a MIME type of 'text/html'"
  + 1. Файл js не найден на сервере, сервер возвращает HTML-страницу с ошибкой (404, 500, ...)
  + 2. Network → Headers, Content-Type для скрипта должно быть `Content-Type: application/javascript`
    - `nginx.conf`: `types { application/javascript js; }`
  + 3. загружаешь `.js` как **ES-модуль (`type="module"`)**, но сервер не поддерживает это
    - Проверь `type="module"` в `<script>`
    - `<script src="/static/js/main.js"></script> <!-- вместо type="module" -->`
    - убедись, что сервер поддерживает корректный MIME-тип
* CORS (Cross-Origin Resource Sharing)
  + механизм, который позволяет веб-браузерам делать запросы к ресурсам на другом домене
  + по умолчанию браузеры запрещают такие запросы из соображений безопасности


### JAVASCRIPT
* a single-page application (subject)
  + один html, меняется с помощью js, js меняет параметры html
  + код внутри {} исполняется в django, он выполняет и заново отправляет html
  + не надо: в завимисости от какого-то условия показываем или нет какие-то части страницы
* js/app.js
  + центральный скрипт, точка входа в приложение, основной код, запускается при загрузке страницы
  + управление авторизацией
  + динамически обновляет DOM пользовательского интерфейса с использованием данных, полученных от бэка
  + '.nav-link' кроме 'loginLink' 'logoutLink'
    - обработка событий интерфейса, кликов на ссылки навигации
    - инициализация маршрутизации router 
    - `e.preventDefault()` запрещает браузеру перезагружать страницу 
    - `router.navigate()` для смены страницы
  + authService.logout()
    - запрос на разлогинивание (`POST /logout/`)
    - удаляет данные профиля из `sessionStorage`
    - перенаправляет на главную страницу (`/`)
  + `fetch('http://backend:8000/api/user-data/')`: HTTP-запрос к эндпоинту `/api/user-data/`, получить данные JSON 
  + `response => response.json()` конвертирует ответ JSON от сервера в объект js
  + после получения данных пользовательский интерфейс (UI) обновляется без перезагрузки страницы
  + `async/await` для упрощения чтения
* на место app подставляется div
  + class Component базовый, абстрактный
  + фиксированная часть страницы доступна в js
  + div = homapage, profile, ... (наследуют от Component)
  + позволяет добавлять аватары, имена пользователя, ...
  + state = переменные 
  + fetch = запрос к бэку
  + функция render своя в каждом компоненте
    - например нужен username - делаем запрос к бэку fetchuserprofile
  + событие DomContactLoaded = html полностью загрузился (у нас только 1 раз)
  + % body % уберем, т.к. у нас SPA
* open chat
  + на странице login, profile, регистрация нету
  + во время игры - статистика, другой user
  + отправлять сообщение через js
* prepMsg забирает инпут и делает ws запрос
  + список пользователей из базы
* login = new ws connexion
* если логин - присылает **session id token**
* первый логин - header CSRF токен
  + в js функция post - в header токен CSRF
  + session id приязывает сессия + юзер в приложении
* обработка ошибок в js для устойчивости чата (попытки переподключения при разрыве соединения, ...)
* pop-up windows : login, chat, profile
* F12 concole
  + лучше всего в chrome
  + colsole.log отладка
  + кнопка квадратик со стрелкой слево вверх - код элемента html
* прямо в консоли писать js и пробовать
  + undetermined - что вернула функция
  + можно создать переменные (let)
    - они сохраняются в объекте window (window = браузер)
    - x или window.x досnуп к этой переменной
  + api браузера - геолокация, звук
* у нас 1 компонент = 1 станица (хотя обычно делают более гибко)
* vscode расширение Go Live - сразу смотреть, что получается на странице
* Router.js
  + определены фиксированные пути (/ → HomePage, /login → LoginPage, ...)
  + нет маршрутов /game/1 или /user/1 => они не обрабатываются Router.js => браузер отправляет запрос на сервер, запрос к localhost:4443/game/1, минуя Router.js
    - если django отдает HTML, то загружается шаблон с бэка
    - для /user/1 сервер отвечает 403 Forbidden, поэтому страница не рендерится
* {% load static %} загрузка стат файлов Django (table.css), нам не надо
* class = стиль
* ws объект js
* сначала выполняется header, потом подгружаются стили
* fetch() запрос обычный (не ws)
* **AbsTimeUser** (непралвьно написано) наш класс наследует
* johnResponse - временный
* view.py jsonResponse или httpResponse
* createuser встроенная, т.к. наследуем от ... 


### BACKEND DAPHNE 
* ASGI сервер 
  + asgi, wsgi - интерфейсы, стандарты для взаимодействия между веб-сервером и веб-приложениями/фреймворками
    - без них фреймворки, тулкиты, библиотеки питон не взаимодействуют между собой, у каждого свой метод установки, настройки
* хорошо интегрируется с Django Channels, поддерживает ws из коробки
* не настраивается под SSL
* слушает внутри контейнера на порту 8000 wss:// соединенияь API-запросы, HTTP-запросы фронтенда
  + страница загружается по https:// => браузер открывает wss://
* переменная `application` = точка входа для ASGI-сервера, ASGI-приложение, обрабатывает запросы
* `ASGI_APPLICATION = "myproject.asgi.application"`
  + `get_asgi_application()` initialize ASGI appli, exposes the ASGI callable as a module-level variable (загрузка приложений `INSTALLED_APPS`, конфигурация бд, среда AppRegistry is populated)
    - before importing ORM models, до использования ORM-моделей и др компонентов Django, конфигурации маршрутов ws
* AllowedHostsOriginValidator проверяет допустимые хосты для ws-соединений (**зачем дублировать с nginx**)
  + проверяет заголовок Origin против ALLOWED_HOSTS
* ws-маршруты Channels работают через ASGI, а не через файлы 
* альтернатива: WSGI, Web Server Gateway Interface
  + поддерживает только синхронные запросы HTTP, не подходит для реального времени или протокола ws
  + но бывает нужен: некоторые инструменты поддерживают только HTTP-запросы; Django-приложений без асинхронных функций стабильнее
  + ASGI, Asynchronous Server Gateway Interface - продолжение WSGI, когда есть асинхронные задачи
* альтернатива: `python manage.py runserver 0.0.0.0:8000`
  + запускает Django-приложение с использованием встроенного разработческого сервера
    - может работать как с WSGI, так и с ASGI, в зависимости от конфигурации проекта
    - не рекомендуется для продакшен, daphne: более высокая производительность и стабильность


### ЯДРО DJANGO 
* бэкенд-фреймворк
* диктует архитектуру (приложения, модели, views, urls, ...)
* DJANGO_SETTINGS_MODULE настройки, `settings.py`
  + `REST_FRAMEWORK` **рендеры**, по умолчанию JSON и **Browsable API**
  + myproject.settings обращается к `backend/myproject/settings.py`
* myproject/ = конфигурация Django, корневой маршрутизатор, запуск, управление проектом
* manage.py
  + django-утилита, интерфейс для настройки, разработки, управлением проектом
  + точка запуска
  + `if __name__ == '__main__':` если файл запущен напрямую (не импортирован как модуль), выполняется `main()`
  + python manage.py runserver сервер разработки
    - daphne обращается к проекту **напрямую** или через `mysite.asgi`
  + остальные команды: [contenttypes] remove_stale_contenttypes, [django] check compilemessages createcachetable dbshell diffsettings dumpdata flush inspectdb loaddata makemessages makemigrations optimizemigration sendtestemail shell showmigrations sqlflush sqlmigrate sqlsequencereset squashmigrations startapp startproject test testserverт, [rest_framework] generateschema, [sessions] clearsessions, [staticfiles] collectstatic findstatic, можно создавать команды
* вcтроенные модульные приложения
  + 'django.contrib.messages'
  + 'django.contrib.admin',
    - /admin встроеный интерфейс
    - суперпользователь Django Admin: все права, полный доступ к системе управления данными (!= суперпользователь бд)
  + 'django.contrib.auth',
  + 'django.contrib.contenttypes',
  + 'django.contrib.sessions',
  + 'django.contrib.staticfiles'
* 'django.contrib.messages' API для работы с flash‐messages
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
    - добавляет заголовки безопасности (`Strict-Transport-Security` для HTTPS) **зачем**
    - перенаправляет HTTP-запросы **на HTTPS**
    - убирает опасные элементы из запросов (от уязвимости **XSS**, ...)
  + SessionMiddleware
    - загружает данные сессии из **хранилища**, управляет сессиями **через куки?**
    - если пользователь аутентифицирован, сессия **сохраняет идентификатор** в Redis/бд после выполнения запроса
  + CommonMiddleware
    - перенаправление при добавлении/удалении `/` в конце URL (**параметр в settings.py ?**)
    - возвращает стандартные заголовки HTTP (`Content-Type`, ...)
  + CsrfViewMiddleware
    - use csrf_protect on any views that use the csrf_token template tag, as well as those that accept the POST data
  + AuthenticationMiddleware 'django.contrib.auth.middleware.AuthenticationMiddleware'
    - to use Django’s default user authentication system
    - проверяет сессию **или заголовки авторизации**
    - checks if the user is authenticated in each request by looking at the session data, and it attaches that information to the request object
    - handles session-based user authentication
    - ensures session-based authentication: This is critical for apps that use session-based authentication (like logging in via the web interface)
    - добавляет **объект `request.user`**, чтобы представления могли определить, аутентифицирован ли пользователь
    - attaches the user object to the request in your views and templates
    - adds the `user` object to the request: Every time a request comes in, the AuthenticationMiddleware populates the request.user attribute with the currently authenticated user based on the session data
    - a part of **Django’s authentication framework** #see
    - 'django.contrib.auth' = the app config and not middleware that manages authentication logic
    - you Can’t Just Replace It with `'django.contrib.auth'
    - the request.user is automatically set => the app knows who the current user is, and is able to rely on request.user to check for authentication or permissions
    - is involved in maintaining and validating the session-based user login (users are authenticated in Django’s session-based system)
    - альтернатива: middleware for token authentication instead of session-based authentication, replace it with middleware like rest_framework.authentication.JWTAuthentication
    - альтернатива: Session Authentication = the built-in Django authentication system but with a different mechanism (like for non-session-based login), you could use Django’s Custom Authentication Backends along with custom middleware to modify how the user is authenticated
  + MessageMiddleware
    - обрабатывает flash messages
    - **обработка сообщений** и добавление **дополнительной информации или фильтрации на уровне WebSocket**
    - без `django.contrib.messages` невозможно использовать
  + XFrameOptionsMiddleware
    - добавляет заголовок `X-Frame-Options` для защиты **clickjacking** (запрещает отображение сайта в **iframe** на других доменах, ...)
* middleware на обратном пути
  + XFrameOptionsMiddleware обрабатывает ответ (добавляет заголовок X-Frame-Options)
    - добавление заголовков безопасности
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
* Django Middleware DRF Middleware — не одно и то же
  + оба работают с HTTP-запросами
  + Django Middleware  
    - обрабатывает ВСЕ запросы в Django (и API, и HTML-страницы)
    - работает на уровне всего приложения перед обработкой представлений (views)
    - **аутентификация**  
    - обработка CORS  
    - сжатие запросов  
    - MIDDLEWARE = ['django.middleware.security.SecurityMiddleware',django.contrib.sessions.middleware.SessionMiddleware','django.middleware.common.CommonMiddleware',]
    - | **Функция** | **Django Middleware** | **DRF (Django REST Framework)** |
      |------------|--------------------|------------------|
      | Где работает? | ВО ВСЕМ Django | Только в API |
      | Уровень | Обработчик запросов (request/response) | DRF Views, Permissions, Authentication |
      | Используется для | Логирования, CORS, кеширования, аутентификации | API Permissions, Serializers, Views |
      | Определяется в | `MIDDLEWARE` в `settings.py` | `REST_FRAMEWORK` в `settings.py` |
* базовый serializers.ModelForm
* базовые механизмы аутентификации django.contrib.auth (**как связано с AuthenticationMiddleware**)
* регистрация и авторизация (в том числе OAuth/SSO)
* модель пользователей, проверка паролей, сессии
* модель групп и разрешений django.contrib.auth.models.Permission
  + разграничение доступа к страницам (приватные/публичные чаты, комнаты, ...)
* проверка **токена/сессии** при каждом запросе с фронтенда
* инструменты по защите от распространённых уязвимостей (**CSRF, XSS, SQL Injection**)
* DPR Data Processing and Rendering: данные (JSON, ...) обрабатываются на сервере с помощью Django, передаются в представление для рендеринга на клиентской стороне
* используем **стандартные структуры юзера для авторизации и для моделей данных**
* middleware работает **отдельно для каждого запроса**
  + создаётся заново ???
  + обычный middleware Django обрабатывает каждый HTTP-запрос
  + **DRF Middleware** (например, аутентификация) вызывается **отдельно для каждого запроса** от каждого клиента.
  + Middleware создаётся и выполняется каждый раз, когда клиент отправляет запрос
    - Когда первый клиент делает запрос → middleware создаётся и выполняется для него
    - Когда второй клиент делает запрос → создаётся новый экземпляр middleware, который обрабатывает этот запрос
  + В DRF аутентификация реализована через **Authentication Classes**. Они вызываются **отдельно для каждого запроса**:
    - Если клиент **отправляет запрос с токеном**, `TokenAuthentication` проверит его **только для этого запроса**.
    - Другой клиент отправляет запрос — DRF снова **запускает новую проверку** через authentication classes.
* среда `AppRegistry` управляет регистрацией приложений и моделей
* типы views
  + `View` базовый класс
    - можно переопределить методы `get()`, `post()`, `put()`, ...
  + `TemplateView для рендеринга шаблонов, в основном для статических страниц, когда не нужно взаимодействовать с базой данных
  + `RedirectView` для редиректов с одного URL на другой
  + `ListView`, `DetailView` классы для работы с базой данных, предоставляющие автоматическое отображение списка объектов или одного объекта
  + `CreateView`, `UpdateView`, `DeleteView` классы для создания, обновления и удаления объектов в базе данных
  + `APIView`, `ViewSet` (для DRF)
* контроль:
  + Django Debug Toolbar `pip install django-debug-toolbar`
  + LOGGING инфо о всех запросах и их обработке в файл `debug.log`
  + `django.db.connection.queries` SQL-запросы, выполненные Django

  
### DJANGO REST FRAMEWORK DRF
* расширяет Django, строит над Django стек для работы с RESTful API (модели, виджеты, фильтры, классы)
* аутентификация, пермиссии и сериализация — модули (слои), содержащие классы
  + в каждом из этих модулей находятся классы, которые можно переопределять
  + не только для middleware
  + аутентификация и разрешения подключаются как классы (Authentication Classes, Permission Classes) на уровне вью
  + сериализация — отдельный слой
  + бизнес-логика аутентификации/разрешений/сериализации чаще всего вне middleware
  + Middleware может использоваться для сессий или базовых вещей
  + `from rest_framework.authentication import TokenAuthentication` (`TokenAuthentication` класс)
  + экземпляры создаются заново для каждого запроса, DRF делает это для изоляции данных между запросами и безопасности  
  + DRF создает новый объект аутентификации на каждый запрос
  + DRF создает новый экземпляр класса разрешений и вызывает его метод `has_permission()`
  + Serializers создаются отдельно для каждого запроса, когда данные сериализуются или десериализуются, `serializer` будет уникальным объектом для текущего запроса
* модуль аутентификации #see
  + классы для проверки пользователя (`SessionAuthentication`, `TokenAuthentication`, ...)
  + проверка сеансовой куки, сессий, csrf, JWT (с библиотекой Simple JWT), Basic Authentication, авторизация по API-ключам
  + гибкие механизмы для работы с JWT
  + 1) во многом реализована через AuthenticationMiddleware
    - Может частично использовать стандартное Django middleware (`AuthenticationMiddleware`), когда речь о сессиях
    - привязать пользователя (request.user) до попадания запроса во вью
    - если JWT или токены, то не чистое middleware, а authentication classes
  + 2) на уровне Views / Viewsets  
    - определяете `authentication_classes = ` (`SessionAuthentication`, `JWTAuthentication` из внешних пакетов)  
    - эти классы вызываются при каждом запросе
    - технически не являются классическим Django middleware  
    - проверяют заголовки/токены/сессии
    - устанавливают `request.user`
    - тут логика
    - если вы говорите о токенах, JWT, Basic/Auth, обычно это **DRF Authentication Classes** на уровне вью.  
  + Считывает токен (Bearer, JWT), куку сессии или другие данные из запроса
  + Определяет, какой пользователь (или аноним) делает запрос, и выставляет `request.user`/`request.auth`
  + работает не напрямую с JSON, а с объектом запроса (request), где смотрят заголовки, куки, сессии, токены, ...
* модуль Permissions
  + имеет ли этот `request.user` право на доступ к данному ресурсу (вью), методу (GET, POST…), конкретным операциям (создание, редактирование, ...)   
  + работает не напрямую с JSON, а с объектом запроса (request), где смотрят заголовки, куки, сессии, токены, ...
* модуль сериализации данных для работы с JSON или API #see
  + request.body json парсится `JSONParser`, ответ сериализуется `JSONRenderer`, преобразование моделей Django и других объектов Python в JSON/XML и обратно
  + `serializer.is_valid()` для проверки пришедших данных
  + вычитанные поля доступны в `serializer.validated_data`
  + DRF Serializers (`rest_framework.serializers.Serializer` или `ModelSerializer`)  
  + реализуется внутри вью/serializer классов DRF
  + валидация данных *?
* модуль прав доступа Permissions
  + `AllowAny`, `IsAuthenticated`, `IsAdminUser`, `IsAuthenticatedOrReadOnly`, кастомные
  + указываются либо глобально в `settings.py` (`REST_FRAMEWORK['DEFAULT_PERMISSION_CLASSES']`), либо на уровне вью
  + DRF вызывает методы `has_permission()` и `has_object_permission()` для permission classes  
* APIView/ViewSet вспомогательные компонены, работают в тандеме с тремя модулями
  + View из обычного Django не применяется для создания API
  + View не используется в DRF напрямую для обработки API-запросов, поскольку DRF специально предоставляет улучшенную функциональность с APIView и ViewSet для работы с RESTful API
  + в связке с сериализаторами для обработки данных запросов и формирования ответов
* APIView 
  + базовый класс представлений (views)
  + предоставляет функциональность для обработки запросов (например, `GET`, `POST`, `PUT`, ...)
  + класс для создания API, инструмент для создания API
  + позволит управлять данными через HTTP-запросы
  + можешь явно определять методы обработки HTTP-запросов, такие как get(), post(), put(), ...
  + встроенная поддержка систем аутентификации и контроля прав доступа (например, с использованием `IsAuthenticated`)
  + создавать собственные представления для обработки запросов
  + методы (`get`, `post`, `put`, `delete`, и т.д.), их можно переопределять для обработки HTTP-запросов
  + ограничения на количество запросов
  + вручную определять маршруты и методы для каждого запроса **против этого ViewSet или router?**
  + больше контроля, чем ViewSet, определяете логику запросов и маршрутизацию
  + подходит, если проект небольшой или требует точной настройки
  + class UserProfileView(APIView)
    - другие сервисы или фронтенд получают информацию о пользователе через API
    - `GET /user-profile/{id}/` вернет данные профиля в формате JSON
    - `get()` получать данные профиля пользователя по его `id`
    - API позволяет _дистанционно_ взаимодействовать с данными через HTTP-запросы, в отличие от Django Admin
    - Django Admin, интерфейс администрирования, не предназначен для работы с API, можно просматривать/редактировать данные профиля
  + class GameView(APIView)
  + class TournamentView(APIView)
* ViewSets вспомогательный компонент
  + обертка над APIView
  + более абстрактная форма
  + упрощает работу с CRUD, объединяют логику CRUD-операций в одном классе
  + автоматизирует маршрутизацию и типичные CRUD-операции с помощью Routers
  + генерирует действия для стандартных операций CRUD, таких как `list`, `create`, `retrieve`, `update`, `destroy`
  + полезен для упрощения кода и автоматизации создания стандартных операций CRUD
* routers вспомогательные компонены, работают в тандеме с тремя модулями
  + маршрутизация RESTful API, URL-маршруты (эндпоинты) для действий представления (viewset)
  + не нужно вручную добавлять каждый HTTP-метод в `urls.py`, писать `urlpatterns` для каждого метода (GET, POST, PATCH, DELETE, ...)
  + DRF автоматически формирует для него набор URL-маршрутов, настроит URL-адреса для всех действий из ViewSet
  + `BaseRouter`- абстрактный класс, от которого наследуются все остальные роутеры, не используется напрямую, определяет базовую логику для работы с маршрутами.  
  + `SimpleRouter` Минимальный роутер с базовым функционалом, не добавляет `root-view` (список всех маршрутов API)
  + `DefaultRouter`расширяет `SimpleRouter`**, добавляя **автоматический `root-view`** (`/api/`), генерация путей к методам ViewSet  
  + `re_path(r'^.*', TemplateView.as_view(template_name='index.html'))` перехватывает URL, не обработанные выше => несуществующие пути перенаправляются на `index.html`, что неправильно для API
  + альтернатива: метод `get_absolute_url()` в модели `UserProfile` может **автоматически создавать URL для объекта**
  + альтернатива: декоратор `@action`
* Filters вспомогательный компонент, фильтрация данных, которые возвращает API, по параметрам запроса
* Throttles вспомогательный компонент, контролируют частоту запросов пользователей, защита от DoS-атак и чрезмерной нагрузки на сервер (nginx у нас)
+ базовые подходы django
  + FBV function based views
    - представления, основанные на функциях
    - ручная обработкя запросов (`GET`, `POST`, ...)
    - проще
    - def my_view(request): return JsonResponse({"message": "Hello, world!"})
  + class based views CBV
    - представления как классы, ООП-подход 
    - встроенная логика обработки запросов (`get()`, `post()`, ...)
    - переопределять только нужные методы (`get`, `post`), а также использовать миксины
    - def get(self, request): return JsonResponse({"message": "Hello, world!"})


### DJANGO RESTFUL HTTP API 
* API Application Programming Interface = интерфейс = набор правил = WebSocket-путь = группа маршрутов (эндпоинтов)
* обрабатывает HTTP-запросы (GET, POST, PUT, DELETE) и отклики между клиентом и сервером
* REST Representational State Transfer стиль взаимодействия между клиентом и сервером
  + Request Body (json, xml, yaml, MessagePac, CSV)
  + Query Parameters `GET /users?name=Иван`
  + Path Parameters `GET /users/1/`
  + Headers `Content-Type: application/json`  
* API = 5–10 эндпоинтов (группа маршрутов, связанных с чатом)
* endpoint = URL-маршрут = конкретный маршрут, связанный с API
  + выполняет определённое действие
  + endpoints чата `GET /chat/rooms/`, `POST /chat/rooms/`, `GET /chat/rooms/<room_id>/messages/`, `POST /chat/rooms/<room_id>/messages/`
* APIView ViewSet специализированные CBV из DRF
  + обработчики HTTP-запросов для работы с API
  + определяются в View-классах или функциях
  + классы, наследующие от `APIView`, `GenericViewSet`, `ViewSet`
  + APIView (у нас)
    - класс, наследуется от класса View
    - CBV предназначенный для создания REST API
    - добавляет сериализацию, аутентификацию, работу с JSON, вспом. методы для обработки HTTP `GET` `POST` `PUT` `DELETE`, др API-функций
    - для простых API с базовой логикой
    - `views.py`: метод `game_detail` обрабатывает запрос
  + ViewSet
    - расширенный вариант APIView
    - использует методы по умолчанию, предоставляемые DRF (избежать написания методов для HTTP-запросов `GET`, `POST`, `PUT`, `DELETE`)
    - добавляет автом. обработку запросов для стандартных операций CRUD (Create, Read, Update, Delete) над данными, которые уже есть в базе
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
* строит и отдаёт html
* реализуются
  + через **события**
  + через **статусы в ответах API**
  + не через механизм сообщений Django
* View (или APIView, если это Django REST Framework)
  + класс‐вью создаётся (инстанцируется) на каждый входящий запрос
  + при каждом запросе к этому урлу Django создаёт новый объект класса, вызывает соответствующие методы (get, post, ...) и затем уничтожает объект
  + хранить в себе контекст обработки (запрос, параметры, данные)
  + в объекте View можно было безопасно хранить данные, относящиеся к конкретному запросу, не опасаясь утечек между разными запросами
* один запрос — одна вьюшка
  + Маршрутизатор URLconf сопоставляет URL‑адрес вьюшке
  + исключения:  
    - middleware или декораторы вокруг вью, которые дополнительно модифицируют запрос/ответ
    - ручной вызов «другой» вью внутри кода (специфичный сценарий)
* AUTH_PASSWORD_VALIDATORS при регистрации нового пользователя или при смене пароля через форму
  + или в Django Admin (если используется для изменения пароля).
  + или в Custom views или views, работающие с DRF, если используется функциональность для смены пароля через API.
  + пользователь регистрируется/ли меняет пароль через Django Forms или DRF serializers => перед сохранением пароля Django проверяет его с помощью валидаторов
  + валидация происходит в момент вызова метода **set_password()**
* bakyt: API for Database - UserProfile works
* DEFAULT_AUTO_FIELD #see


### DJANGO CHANNELS
* расширение, framework, библиотека, надстройка для ws поверх стандартного стека django: привычные инструменты, структуры + асинхронное взаимодействие, **фоновые задачи**, обновление интерфейса в режиме реального времени
* слушает ws-запросы, обслуживание ws
  + непрерывное соединение, стрим
  + js инициирует соединение через **WebSocket API**: `new WebSocket`
  + daphne устанавливает ws-соединение
  + daphne передаёт ws-соединение в ASGI-приложение
  + создает consumer
  + связывает URL с обработчиками `consumers.ChatConsumer.as_asgi()`, URLRouter маршрутизирует запросы на consumers
  + **если сертификат отсутствует, браузер может блокировать подключение ws**
  + если страница загружена по https://, то используйте wss:// 
  + AuthenticationMiddlewareStack
    - соединение аутентифицировано (токены JWT, Cookies, ...)
    - связывает пользователя с запросом, с подключением
    - **оборачивает** маршруты ws
    - доб. объект user в scope["user"] => consumer self.scope["user"] user инфо, действия на основе его прав, ролей
  + SessionMiddleware
    - получение и сохранение id сессии и связанного с ним пользователя
    - обязателен, если используете сессии (хранение пользовательские данных между запросами)
    - обязателен, если есть `django.contrib.auth` (он опирается на сессии для хранения данных о пользователе)
    - обязателен, если взаимодействие с сессиями на сервере (сессионное хранилище в бд, Redis, файловой системе,...)
    - можно без него, если JWT токены или др безсессионные методы аутентификации 
    - можно без него, если данные о сессии хранятся в зашифрованных cookie (без серверного хранилища)
  + свои middleware: ограничение скорости, управление правами доступа, обработка ошибок
  + сессии передаются через cookie
* ws‑запрос (подключение) привязывается к одному consumer
  + Маршрутизация (routing): какой consumer обрабатывает точку подключения (по URL, пути, протоколу, ...)  
* назначение пользователей к группам каналов
* ws
  + как работает утсновка соединения:
    - js инициирует подключение к `wss://localhost:4443/ws/chat/room/`
    - F12 в списке сетевых запросов элемент, отражающий WebSocket (101 Switching Protocols), "Status" 101, network pending или active
    - nginx видит запрос с заголовками Upgrade: websocket
    - nginx Найдёт location /ws/ { ... }
    - nginx проксирует запрос на backend
    - daphne отвечает HTTP/1.1 101 Switching Protocols
    - при установке ws-соединения запрос пришёл с `Upgrade: websocket`
    - nginx вернёт 101
    - соединение переходит в ws‐режим
  + как работает обмен сообщениями:
    - `wss://<domain>/ws/...` -> `http://backend:8000/ws/...` - daphne принимает ws по пути `/ws/chat/<room>/` 
  * wss://localhost:4443/ws/chat/
  * ws://localhost:4443/ws/chat/room/ ws-запрос на /ws/chat/
  * wss://localhost:4443/ws/chat/room/
  + `F12` Network - ws - messages: входящие/исходящие сообщения, в Timing (Headers) статус 101 Switching Protocols
  + views.py обрабатвает HTTP Django URLs /chat/<room> 
  + channels обрабатывает /ws/chat/<room>
  + маршруты Channels: websocket_urlpatterns = [re_path(r'^ws/chat/(?P<room_name>\w+)/$', ChatConsumer.as_asgi()),]


### CHANNEL LAYERS
* абстракция, интерфейс/протокол, настройка, промежуточный уровень, канал-сервер, канальный слой для Django Channels
* публикует в канал, доставляет подписанным
* передача сообщений между частями приложения
  + ```
    [Consumers]
        ├──► [Django Models] ↔ [База данных]
        ├──► [Channel Layers (Redis)]
        │       └──► [Процессы]
        ├──► [Асинхронные задачи (Celery)]
        │       └──► [Фоновые Рабочие Процессы]
        └──► [Аутентификация и Авторизация]
      ```
  + инстансами приложения
  + клиент ↔ consumers: установление соединения
  + клиент ↔ consumers: обмен сообщениями
  + consumers ↔ channel layers: consumers присоединяются к группам (комнатам, игровым сессиям) через Channel Layers
  + consumers ↔ channel layers: отправка сообщений в группу
  + Consumers ↔ Django Models и База данных: чтение/Запись данных
  + Consumers ↔ Django Models и База данных: Транзакции: согласованности данных при одновременных изменениях
  + Consumers ↔ Асинхронные задачи (Celery): Запуск задач: При определённых событиях (окончание игровой сессии, ...) Consumers инициируют выполнение фоновых задач
  + Consumers ↔ Асинхронные задачи (Celery): Обработка результатов: Фоновые задачи могут обновлять данные или отправлять уведомления после выполнения
  + Consumers ↔ Аутентификация: проверяют аутентификацию пользователя через scope["user"]
  + Consumers ↔ Аутентификация: ограничение доступа к определённым действиям или комнатам на основе прав пользователя
  + Backend ↔ Внешние API и Сервисы (обработка платежей, отправка уведомлений)
  + Backend ↔ Внешние API и Сервисы: Маршрутизация: Обработка запросов от внешних источников и передача данных обратно клиентам через Consumers
  + Клиент ↔ REST API: клиент может использовать традиционные HTTP-запросы для получения или отправки данных
  + Клиент ↔ REST API: Синхронизация данных: Обеспечение **согласованности состояния** между частями приложения
  + Consumers ↔ Другие Consumers: Consumer может взаимодействовать с другим для координации действий или обмена данными
* **очереди сообщений**
  + операции в фоновом режиме (обработка запроса, отправка уведомлений)
  + избежать блокировки основного потока
* управляет **хранением состояния**
* масштабирование, легко работают в кластере
* альтернатива: асинхронный обмен сообщениями без Channel Layers
  + обрабатывать сообщения напрямую внутри процесса, используя функциональность Django Channels и ws-соединений
  + между клиентами в рамках одного процесса
  + для рассылки сообщений можно использовать **группы, которые предоставляет Django Channels**
  + отсутствие долговременного хранения сообщений (если не сохраняете их в базу данных)
  + channel layers проще, масштабируемо
* альтернатива: без внешнего channel layer
  + по умолчанию InMemoryChannelLayer
  + отправлять и получать сообщения в рамках одного процесса (одного экземпляра приложения)  
  + group_send или мульти‐user‐чат‐шаблон только в рамках **одного процесса** #question
  + реал-тайм обновления для одного пользователя (или небольшой группы в рамках одного процесса). Например, вы хотите уведомлять конкретного юзера о событиях (новые сообщения, новые заявки в друзья и т.п.), и ваш сервер – один.  
  + Мини-чат или прямое общение (point-to-point). Если все пользователи сидят в рамках одного сервера (и у вас не требуется распределённая горизонтальная архитектура), то и группы Channels будут работать внутри этого одного процесса 
  + Push-уведомления из кода в конкретный WebSocket‐консьюмер
* **ws один для всех пользователей для чата (лёша)** #question
* channels_redis поддержка групповой рассылки


### REDIS
* Remote Dictionary Server  
* **хранение данных**
* **маршрутизация данных**
* **подсчёт статистики, временных данных**
* Channel Layers и кэш — два независимых сервиса логически
  + могут использовать один и тот же Redis-сервер с разными бд или настройками, c разными ключами и настройками
  + 1. для проектов с ~100 пользователей и без высоких требований к масштабированию достаточно 1 Redis-инстанса и 1 базы, separating Redis instances can be overkill. The arguments (isolation, scalability, etc.) are still valid in theory, but one Redis instance is usually enough at this size. If you expect significant growth or want a clean separation for reliability and maintenance, you can still split them. 
  + 2. один Redis с двумя бд
    - для небольших проектов, если нет больших требований по нагрузке и безопасности
    - Redis поддерживает 16 баз данных (`db=0`, `db=1` ...)
    - память, процессы ввода-вывода, др. ресурсы общие, если одна база нагрузит Redis, другая пострадает
    - базы работают под общими параметрами (политика сброса ключей по LRU, максимальный объём памяти, ...)
    - ограниченные возможности мониторинга, метрики будут по одному инстансу в целом (использование памяти, CPU, ...)
  + 3. два Redis-инстанса или даже два сервера
    - Isolation: Flushing the cache won’t wipe out channel data, flush-операции, производительность
    - Scalability: Each instance can be tuned or scaled independently
    - Performance: Heavy traffic on one won’t harm the other
    - Security: Compartmentalizes sensitive channel data from the cache
    - Monitoring: Easier to track and troubleshoot each component’s load
* настройка
  + время жизни кэша (`TIMEOUT`), ...
  + `redis-cli info memory` **не перегружен?**
  + `redis-cli -n 1 flushdb` очистить кэш 
  + `FLUSHALL` (очищает весь инстанс)
  + `FLUSHDB` (очищает одну базу)
  +  **защищён паролем** #see
* `vm.overcommit_memory = 1` #see
  + параметр ядра (kernel)
  + как система распределяет виртуальную память
  + сохранять данные в условиях низкой памяти
  + избежать ошибки `OOM` при больших `maxmemory`
  + в `/etc/sysctl.conf` сохранить после перезагрузки
  + sysctls: vm.overcommit_memory: "1" в docker-compose.yml
    - Dockerfile: RUN echo "vm.overcommit_memory=1" >> /etc/sysctl.conf
    - если контейнер не в privileged-режиме, Docker не даёт ему доступ к глобальному kernel-параметру, кроме безопасных сетевых
    - запускать контейнер с `--privileged`
  + `cat /proc/sys/net/core/somaxconn` 4096 #see
    - макс длина очереди соединений (backlog) для ws **в режиме прослушивания**, сколько входящих соединений могут быть поставлены в очередь
    - внутри контейнера listen(fd, 10000) - очередь ограничена 4096 (если не переопределяется др. настройками)
    - значение берётся у хостового ядра, ведь контейнеры используют общее ядро с хостом
* очередь задач (отправки email, push-уведомления, логирование, обработка фоновых задач (обновление статистики матчей))
* для создания очередей задач (для обмена задачами между различными частями приложения, такими как обработка сообщений или фоновые задачи)
* как устроен технически
  + работает в оперативной памяти => высокая скорость
  + может сохранять данные на **диск**, чтобы избежать потери при сбоях #see
  + `channels_redis` библиотека для подключения и отправки сообщений
  + `django-redis` библиотека для сериализации, записи, извлечения
* Если используется Django Channels**, Redis выполняет сразу две роли
  + Backend channel layers (для WebSocket-обмена сообщениями)  
  + Хранилище сессий пользователей


### REDIS = BACKEND ДЛЯ CHANNEL LAYERS = реализация Channel Layer
* бэкэнд, брокер сообщений, посредник
  + Redis = брокер сообщений в режиме Pub/Sub
    - Подключение к Redis (`redis.createClient()` или аналог)
    - Вызовы методов `publish()` и `subscribe()`
* для передачи сообщений между потребителями ws
* интерфейс для Channels (методы `send()`, `receive()`, `group_add()`, ...)
* конкретный класс, реализует протокол Channel Layer
* для работы с асинхронными задачами через Django Channels (**какие ещё задачи**)
* распределять нагрузку **между несколькими контейнерами** (Горизонтальное Масштабирование)
+ **устойчивость к сбоям**
* долговременное хранение сообщений или очередей вне памяти приложения
* обработка большого количества одновременных WebSocket соединений и сообщений в реальном времени
* механизм Pub/Sub (Publish/Subscribe) для чата
  + Pub/Sub => идеальный для каналов WebSocket
  + рассылать сообщения всем пользователям чата
  + бекенд для Pub/Sub
  + механизм списков или Pub/Sub для реализации очередей сообщений
  + Поиск в коде `PubSub`, `publish`, `subscribe`, `event`, `broker`, `topic`
    - библиотеки  `Redis`, `RabbitMQ`, WebSocket-библиотеки с поддержкой Pub/Sub
  + Если чат реализован через WebSocket, Pub/Sub может быть встроен в логику
    . Например, отправка сообщения (`publish`) уведомляет подписчиков (`subscribe`)
    - Проверьте папку `services`, `chat`, или `events` на наличие Pub/Sub-логики
  + Найдите модули или файлы, связанные с чатом
    - если сообщения отправляются на брокер или WebSocket, это может быть признаком Pub/Sub
  + отправьте сообщение в чат и проверьте:
    - логирование на сервере
    - если сообщение доходит мгновенно, вероятно используется WebSocket с логикой Pub/Sub
* альтернатива: различные реализации того же абстрактного интерфейса, что и `channels_redis.core.RedisChannelLayer`
  + встроенный в Django Channels **InMemoryChannelLayer**
    - класс из пакета `channels.layers`  
    - хранит сообщения в памяти процесса Python  
    - не годится для продакшн: поддерживается единственным процессом, данные исчезнут при перезапуске
  + RabbitMQ брокер сообщений системных сообщений (уведомление о начале турнира, уведомление о добавлении в друзья)
  + PostgreSQL
  + IPC  
* bakyt: Redis is commonly used as a message broker and **in-memory store**
  + publish/subscribe capabilities, allowing messages to be instantly distributed to multiple subscribers
  + Channel Layer Backend
    - to handle real-time communication between different consumers
    - the backend for **Django’s ASGI channel layer**, enabling **inter-process communication**
    - Without Redis, different Django processes wouldn’t be able to share real-time events
  + Scalability: Redis allows your chat system to work across multiple Django instances. Even if your application is running on multiple servers, Redis ensures all instances remain synchronized.
  + Session and Message Storage
    - While messages are usually stored in a database, Redis can be used for temporary message caching before writing to the database.
    - It can also store user session data, improving chat performance.
  + Low Latency
    - Redis operates in-memory, making it extremely fast compared to traditional databases.
    - This ensures minimal delay in message delivery, improving the real-time experience.
* Протокол: Redis pub/sub
* если несколько серверов/контейнеров обрабатывают ws-запросы
  + daphne каждого инстанса/контейнера подключается к Redis-серверу
  + сервер получает сообщение от клиента, отправляет его в канал через Channel Layer
  + Redis доставляет соообщение на другой сервер/инстанс, подписаный на канал


### REDIS ДЛЯ ХРАНЕНЯ СЕССИЙ
* при подключении пользователей ws
* для управления **состоянием** ws-соединений
* управление пользовательскими сессиями, хранит **данные о подключениях**, о текущих авторизованных пользователях
* бекенд для **ws-сессий**
* функции Redis для управления WebSocket-сессиями и кэширование — это схожие, но разные задачи
  + Кэширование
    - Redis используется как высокоскоростное хранилище данных в памяти.
    - Обычно применяется для хранения результатов вычислений или запросов к базе данных, чтобы ускорить ответы сервера.
    - Данные в кэше имеют короткий срок жизни и могут быть сброшены.
  + Управление состоянием WebSocket-сессий
    - Redis может выступать как **хранилище данных о сессиях** и точка синхронизации для распределённых серверов.
    - Например, когда у вас несколько экземпляров backend-сервисов, Redis используется для:
      - Хранения информации о подключениях пользователей.
      - Сохранения авторизационных данных и текущих состояний пользователей.
      - Широковещательной передачи сообщений между backend-сервисами через Pub/Sub для синхронизации WebSocket-сообщений.
* В текущей конфигурации Redis не настроен для хранения пользовательских сессий
  + SESSION_ENGINE = 'django.contrib.sessions.backends.cache' указывает Django использовать кэш для хранения сессий
  + SESSION_CACHE_ALIAS = 'default' привязывает сессии к конфигурации Redis из вашего блока CACHES


### REDIS ДЛЯ КЭШИРОВАНИЯ
* django cash framework обращается к redis
  + инфраструктура для кэширования
  + Django API для кэширования данных
* что кэширует
  + данные, которые часто запрашиваются, но редко изменяются
  + запросы, объекты, шаблоны, ...
  + временне данные для ускорения работы приложений (имя, аватар, **временный статус пользователя: онлайн/офлайн**)
  + результатов сложных SQL-запросов, сложных вычислений
  + результаты REST API-запросов, чтобы не нагружать базу данных повторными запросами
  + API-ответов (данных профиля пользователя, данных статистики)
  + сеансы: **хранилище** сеансов для авторизованных пользователей
  + пользовательские сессии, что **ускоряет работу сессий в распределённых системах**
* **класс клиента определяет, как Django взаимодействует с Redis**
* `OPTIONS` = `CLIENT_CLASS` настраивает использование `DefaultClient` из библиотеки `django-redis` для взаимодействия с Redis
* django REST Framework обращается к redis
  + кэшировать ответы API, особенно одинаковые для множества пользователей
  + кэширование для представлений API: если данные уже есть в кэше, они будут возвращенbaы из Redis, если нет, будут получены из базы данных и затем кэшированы
  + кэширование на уровне вьюх или сериализаторов
* django channels обращается к redis
  + CHANNEL_LAYERS: позволяет Django Channels использовать Redis для обмена сообщениями через WebSocket или другие каналы
    - redis = очередь сообщений, которые могут быть отправлены клиентам
    - reids = механизм синхронизации для разных процессов или серверов
  + redis для кэширования промежуточных результатов в асинхронных операциях (промежуточные результаты обработки WebSocket-соединений)
* браузер кэширует картинки, CSS, JS), чтобы загружать их быстрее при повторных запросах
  + может кэшировать API-ответы, если сервер отправляет заголовки **`Cache-Control`**
* в DRF стоит кэшировать **запросы к базе данных**,
  + если данные часто запрашиваются, но редко изменяются
  + или если нужно снизить нагрузку на сервер
  + **можно** использовать Django Cache Framework и кэшировать API-ответы с `@cache_page()` или использовать `cache.get()`/`cache.set()`
  + django channels сам по себе не кэширует данные
    - redis используемый для Channel Layers может временно хранить сообщения до их доставки
    - если нужно кэшировать ws-сообщения** или результаты долгих вычислений, можно использовать Redis в качестве кэша.
* consumer может работать как напрямую с Redis, так и через Django Cache Framework (который **может** использовать Redis как бэкенд)
  + через Django Cache Framework (абстракция)
    - consumer не взаимодействует с Redis напрямую
    - `messages = cache.get(self.room_name, [])`  # Читаем из кеша (может быть Redis)
    - Django Cache сам выбирает бэкенд (можно легко переключиться с Redis на Memcached и т.д.)
    - код проще и унифицирован  
    - если нужен просто кеш (например, хранить сообщения), лучше использовать Django Cache Framework
    - если CACHES настроен, значит Django Cache Framework включен
  + напрямую с Redis (без Django Cache)
    - consumer сам подключается к Redis, управляет кэшем напрямую
    - `await self.redis.rpush(self.room_name, message)`
    - более гибкий контроль над Redis (можно использовать списки, pub/sub и т.д.)
    - можно хранить данные **без зависимости от Django**  
    - если ты используешь Redis как брокер сообщений **или** pub/sub, тогда лучше обращаться к нему напрямую
* сохранение сообщений в базу данных и кэширование могут быть организованы по разным принципам
  + 1) каждое входящее чат-сообщение записывается и в базу данных, и в кэш, сразу же после его получения
    - в бд сохраняются все детали сообщения: отправитель, получатель, дата, текст
    - кэш чтобы ускорить доступ к сообщениям в рамках текущей сессии чата, снижая нагрузку на базу данных при запросах
   - доступность сообщения в обоих хранилищах
   - нагрузка на серверы при большом объеме сообщений
   - более частым является первый вариант
  + 2) сохранение в базу данных только по достижении определенного порога (например, 10-12 сообщений)
    - сообщения сначала сохраняются в кэш
    - в базу данных они записываются только тогда, когда в чате накопится определенное количество сообщений или когда сессия завершена
    - минимизирует нагрузку на бд
    - эффективно управлять производительностью при больших объемах чатов
    - возможные **потери сообщений**, если что-то произойдет до того, как они будут сохранены в базу данных (если кэш будет очищен или сервер будет перезагружен)
  + 3) гибридный подход
    - система должна поддерживать высокую скорость работы
    - кэш для временного хранения
    - бд для долговременного
* альтернатива: по умолчанию django кэширует в памяти `django.core.cache.backends.locmem.LocMemCache` 
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
* возможности
  + сохранять кэшированные данные на диск (persistent storage) для предотвращения потери данных после перезапуска (например, для сессий)
  + сложные типы данных в кэше (списки, множества, хеши)
  + сложные стратегии кэширования: TTL (время жизни), евикция (удаление старых или неиспользуемых данных), ограничение количества запросов от одного пользователя за определённое время (rate limiting)
  + легко масштабируется
  + может использоваться в распределённых системах, в многосерверных / контейнеризованных средах
    - данные в процессе/сервисе Redis, а не в памяти отдельного экземпляра приложения
    - данные кэша разделяются между всеми процессами и серверами, что делает его идеальным для кластеров
    - несколько серверов обращаются к одному кэшированию
    - Redis общий слой
* тех подробности
  + протокол: Обычное key-value хранилище
  + `django_redis` берёт на себя работу с Redis, предоставляя **Django-friendly API**
  + не требует добавления в INSTALLED_APPS
* bakyt: только кэш сообщений, чат и **системные**


### DJANGO MODELS, DATABASE PostgreSQL
* Starting entrypoint script... : Apply all migrations: **admin, auth, contenttypes, myapp, sessions**
* ORM Object-Relational Mapping
  + `models.py`
  + описывают структуру данных (таблицы, поля, связи)  
  + вместо SQL-запросов, разработчик описывает модели (Python-классы), PUT = поменять поле в бд (вместо запроса sql)
  + Django преобразует модели в SQL-таблицы
  + легко менять базу с SQLite на PostgreSQL
  + ForeignKey, ManyToMany, OneToOne создают связи между таблицами
  + **защита от SQL-инъекций**
* `docker exec -it db psql -U myuser -d mydatabase`
  + then `\db`, `\dt`
    ```
                      List of relations
 Schema |                Name                | Type  | Owner  
--------+------------------------------------+-------+--------
 public | auth_group                         | table | myuser
 public | auth_group_permissions             | table | myuser
 public | auth_permission                    | table | myuser
 public | chat_chatgroup                     | table | myuser
 public | chat_groupmessage                  | table | myuser
 public | django_admin_log                   | table | myuser
 public | django_content_type                | table | myuser
 public | django_migrations                  | table | myuser
 public | django_session                     | table | myuser
 public | myapp_userprofile                  | table | myuser
 public | myapp_userprofile_blocked_users    | table | myuser
 public | myapp_userprofile_friends          | table | myuser
 public | myapp_userprofile_groups           | table | myuser
 public | myapp_userprofile_user_permissions | table | myuser
 public | pong_game                          | table | myuser
 public | tour_round                         | table | myuser
 public | tour_round_players                 | table | myuser
 public | tour_tour                          | table | myuser
 public | tour_tour_players                  | table | myuser    ```
* лучший вариант для Django – оставить `id`
  + django добавляет `id = AutoField(primary_key=True)`, если явный первичный ключ не указан
  + Минусы `game_id`:
    - Django уже создает `id`, так что `game_id` – это **избыточность**.  
    - В коде придется писать `game.game_id`, вместо простого `game.id`.  
    - Во всех внешних ключах (`ForeignKey`) тоже придется явно указывать `to_field='game_id'`, если хотят ссылаться на `game_id`.
* `AutoField` (по умолчанию) = 32-битный `int` (максимальное значение: ~2.1 млрд), чаще всего достаточно #see
  + `BigAutoField` = 64-битный `int` (максимальное значение: ~9 квинтиллионов)
* AUTH_USER_MODEL = 'myapp.UserProfile`,  кастомная модель `myapp.UserProfile` вместо стандартного `User`
  + class UserProfile(AbstractUser)
  + наследуется от `AbstractUser` => это полноценный пользователь Django
    - механизмы аутентификации **`login()`**, **`authenticate()`**, `request.user` работают
  + таблица пользователей `myapp_userprofile` (не `auth_user`)
    **public** | auth_group                         | table | myuser
    public | auth_group_permissions             | table | myuser
    public | auth_permission                    | table | myuser
    public | myapp_userprofile_user_permissions | table | myuser
  + со стандартной моделью django.contrib.auth.models.User невозможно напрямую добавить аватар, уникальный email, систему друзей, ..., нельзя изменить существующие поля (`username`, `email`)
  + `avatar` реализовать без расширения: Создать модель `UserProfile`, связанную с `User` через `OneToOneField`
  + `friends` реализовать без расширения: Создать модель **Friendship**, связанную `ManyToManyField(User)`
  + URL для фото picture_url реализовать без расширения: Добавить поле в `UserProfile`
  + когда дргуие модели ссылаются на `User`: **`get_user_model()`**
  + какую модель использует django: `docker exec -it backend python manage.py shell` -> `from django.contrib.auth import get_user_model` `print(get_user_model())`
* **с повторным емейлом не создаём?**
* генерировать граф: pip install django-extensions pygraphviz pydot
* migrations
  + `python manage.py makemigrations`
    - создаёт файлы миграций (инструкции по изменению базы)  
    - django выполняет изменения из файлов миграций
    - django обновляет структуру базы данных
  + python manage.py migrate применение миграций базы данных
    - создаёт таблицы для новых моделей
    - добавляет, изменяет, удаляет поля
    - обновляет связи между таблицами
  + docker exec -it backend python manage.py showmigrations
  + docker exec -it backend python manage.py inspectdb
  + docker exec -it backend python manage.py sqlmigrate myapp 0001


### GAME LOGIC
* **не может играть сам с собой**
* a player should also be possible to propose a tournament (subject)
  + a tournament displaies who is playing against whom and the order of the players (subject)
• a matchmaking system: the tournament system organize the matchmaking of the participants, and announce the next fight
* Remote players (subject)
  + two distant players, each player is located on a separated computer, accessing the same website and playing the same Pong game
  + think about **network issues**, like unexpected disconnection or lag
  + you have to offer the best user experience possible
* AI Opponent (subject)
  + to introduce data-driven elements to the project, with the major module introducing an AI opponent for enhanced gameplay, and the minor module focusing on user and game statistics dashboards, offering users a minimalistic yet insightful glimpse into their gaming experiences
  + an AI player into the game
  + the use of the A* algorithm is not permitted 
  + an AI opponent that provides a challenging and engaging gameplay experience for users
  + the AI replicates human behavior, meaning that in your AI implementation, you must simulate keyboard input
  + the AI refreshes its view of the game once per second, requiring it to anticipate bounces and other actions
  + AI logic and decision-making processes that enable the AI player to make intelligent and strategic moves
  + explore alternative algorithms and techniques to create an effective AI player without relying on A*
  + the AI adapts to different gameplay scenarios and user interactions
  + to explain in detail how your AI is working 
  + it must have the capability to win occasionally
  + an AI opponent that adds excitement and competitiveness without relying on the A* algorithm
* game logic, because we need it to do the multiplayer
* alexey: Tournaments – working 
* Амин 
  + whether we want to follow basic ping pong rules?
  + the ball should speed up when paddle hits the ball ?
  + https://stackoverflow.com/questions/54796089/python-ping-pong-game-speeding-up-the-ball-after-paddle-hit 
  + some sort of **anticheat** to be sure that the users mouvement are normal
    - my code outputs two players position and the ball and then you can render it however you want


### USER MANAGEMENT
* user management, authentication, users across tournaments (subject)
  + users can subscribe to the website in a secure way
  + registered users can log in in a secure way
  + users can select a **unique display name to play the tournaments**
  + users can **update their information**
  + users can add others as friends and view their online status
    - **"is_active": true** в http://localhost:8000/users/
    - через библиотеку channels - по вебсокетам следим пользователь онлайн или нет - НАМ ЭТОТ СПОСОБ (?)
    - через Django sessions - когда юзер делает действие, Джанго сохраняет в бд дату
    - через redis - не понял как работает
  + user profiles display stats, such as wins and losses
  + user has a Match History including **1v1** games, dates, relevant details, accessible to logged-in users
  + the management of duplicate usernames/emails is at your discretion, you must provide a solution that makes sense
  + users can upload an avatar, with a default option if none is provided
* myapp: логика пользовательских профилей, турниров, историй игр
• a registration system
  + at the start of a tournament, each player must input their alias name
  + the aliases will be reset when a new tournament begins
  + this requirement can be modified using the Standard User Management module


### СТАТИЧЕСКИЕ ФАЙЛЫ html js CSS изображения шрифты
* чтобы все файлы находились/отдавались там, где ожидает браузер
* **Frontend: права только на чтение статических файлов Backend**
* python manage.py collectstatic собирает стат файлы DRF в STATIC_ROOT/
  + админские CSS, стат. файлы DRF для browsable API, ... - в `django.contrib.admin` 
* daphne не обслуживает статические файлы
  + стили для jango приходят с фронтенда (собранный **бандл**)
* общий том, чтобы Nginx и backend контейнеры обменивались статическими файлами 
* nginx обслуживает ваши стат фалйы и стат фалйы Django
   + по пути STATIC_URL = '/static/ (url для nginx, не папка)
   + Django формирует ссылку <link rel="stylesheet" href="/static/admin/css/base.css">
   + браузер стучится по https://HOST/static/admin/css/base.css
   + nginx видит location /static/ { alias /usr/share/nginx/html/static/;} }
   + nginx берёт в /usr/share/nginx/html/static/backend/admin/css/base.css
* django.contrib.staticfiles.testing.**StaticLiveServerTestCase**: to transparently serve all the assets during execution of these tests in a way very similar to what we get at development time with DEBUG = True, i.e. without having to collect them using collectstatic first
* CSS-OM = дереао как DOM
* bootstrap готовые стили, можно создавать кастомные на основе них
* .map (Source Map) 
  + при минификации или транспиляции CSS/JS (при сборке) код CSS превращается в сжатую скомпилированную версию
  + Source Map хранит сопоставление (mapping) между сжатым и исходным кодом%, чтобоы видеть исходный код для отладки кода в браузере
  + в продакшен отключают генерацию .map-файлов, чтобы уменьшить вес приложения и не раскрывать детали кода
* в разработке три других варианта
  + django.contrib.staticfiles
    - обслуживает **стат файлы для /admin**
    - позволяет добавлять версии, хеши в имена файлов на фронтенд, чтобы избежать кеширования старых версий
    - если CSS-файл кэшировался, браузер может залипать => добавить ?v=123 в конце ссылки или очистить кэш
  + manually serve user-uploaded media files from MEDIA_ROOT
    - use django.views.static.serve() view
    - don’t have django.contrib.staticfiles in INSTALLED_APPS
    - if STATIC_URL = static/, addi urlpatterns = [ ] to your urls.py `static(settings.STATIC_URL, document_root=settings.STATIC_ROOT)`
      - works only if the prefix is local (static/) and not a URL (http://static.example.com/)
    - only serves STATIC_ROOT folder
    - doesn’t perform static files discovery like django.contrib.staticfiles
    - serves static files via a wrapper at the WSGI application layer => static files requests do not pass through the middleware chain
  + напрямую ссылаться на файлы через URL
* `MEDIA_ROOT` = корневая директория для хранения медиафайлов проекта
  + `upload_to` = папку внутри `MEDIA_ROOT`, куда будут загружаться файлы
  + в большинстве случаев достаточно настроить `upload_to` и `MEDIA_ROOT`
  + `AVATAR_UPLOAD_PATH` можно не использовать, если не используешь этот путь в других частях кода 
  + `AVATAR_UPLOAD_PATH` может быть использован для построения URL, если нужно динамически формировать ссылки на аватары в коде
  + `AVATAR_UPLOAD_PATH` может быть использован если требуется какая-то специфическая обработка (например, для загрузки в облако или на внешний сервер)

### DOCKER
* Чтобы Docker не загружал образ Python 3.10 с Docker Hub при каждом запуске  
  + Использовать кэш Docker при сборке
    - Когда ты выполняешь `docker-compose up --build`, Docker кэширует слои сборки. Но если изменяешь `requirements.txt`, кэш ломается
    - Сначала скопировать только `requirements.txt`, установить зависимости, а потом копировать весь код:  
      ```dockerfile
      FROM python:3.10
      ENV PYTHONUNBUFFERED=1
      ENV DJANGO_SETTINGS_MODULE=myproject.settings
      
      WORKDIR /app
      
      COPY requirements.txt /app/requirements.txt
      RUN pip install -r requirements.txt && rm requirements.txt
      
      COPY . /app
      
      COPY entrypoint.sh /entrypoint.sh
      RUN chmod +x /entrypoint.sh
      
      ENTRYPOINT ["/entrypoint.sh"]
      CMD ["daphne", "-b", "0.0.0.0", "-p", "8000", "myproject.asgi:application"]
      ```
  + локальный образ   
    - docker pull python:3.10
  + не перезапускать контейнеры при изменении кода  
    - `./backend:/app` позволяет обновлять код без пересборки образа  
    - `docker-compose restart backend
    - пересобрать без удаления кэша: `docker-compose build backend`, `docker-compose up -d backend`
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
* если вы хотите, чтобы том был доступен и через файловую систему хоста, вы можете использовать bind mount, а не именованный том
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
* For Ecole42 computers, I've updated settings of docker file in DEV branch 
  + порт, который нужен для django, занят
  + поменять номера портов в docker и в nginx
  + 6800  port for redis 
  + 4444 port frontend HTTP connections
  + 4443 port frontend for SSL connections over HTTPS
  + for local Ecole 42's network `ALLOWED_HOSTS = ['*']` in settings.py
* 'docker compose up --build'
  + все скачается и распакуется
  + потом в терминале можно Ctrl + C (условное клавиатурное прерывание)
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
* from django.contrib.auth.models import User  VS  AbstractUser, от которого наследует class UserProfile (AbstractUser) 
  + `django.contrib.auth.models.User` = стандартная модель Django
    - по умолчанию для аутентификации  
  + `AbstractUser` = абстрактная модель
    - расширяет стандартного `User` (добавляет поля и методы)  
    - class UserProfile(AbstractUser) =  кастомный юзер
    - `UserProfile` это не `User`
    - jango больше не использует `django.contrib.auth.models.User`
    - вам надо работать с `get_user_model()`
    - если AUTH_USER_MODEL = 'yourapp.UserProfile', то вместо `User` надо импортировать свою модель:  
      ```python
      from django.contrib.auth import get_user_model
      User = get_user_model()  # Теперь User = UserProfile
      ```
* CSR client-side rendering: данные загружаются на клиентскую сторону, HTML генерируется динамически с помощью js
* SSR server-side rendering: сервер генерирует и отправляет готовый HTML на клиентскую сторону, каждый запрос требует пересоздания всей страницы на сервере, медленно, сервер выполняет обработку данных и генерирует страницу каждый раз, когда поступает запрос 
  + сервер отправляет данные (JSON, ...)
  + клиент с помощью js генерирует HTML на основе полученных данных
  + не требуется повторная генерация страниц на сервере для каждого запроса
  + it is almost front-end
  + if we do module - power-ups for AI are obligatory (so it is back end part): для модуля AI требуется бэкенд (сложная обработка данных, вычисления, доступ к серверным ресурсам)
* rendering
  + фронтенд: рендеринг HTML-шаблонов или динамически обновляемых данных через js, процесс генерации и отображения контента на веб-странице
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
* pathname = часть адреса после корня
* `Uncaught SyntaxError: Unexpected token '<'`
  + `theme.js` и `nav_sidebar.js` отдаются со статусом 200, но на деле возвращают HTML
  + вместо js браузер получил от сервера HTML (страницу с ошибкой, редирект на логин, ...)
* Для 100 пользователей Transcendence можно считать небольшим или средним, в зависимости от нагрузки
  + **размер проекта зависит не только от количества пользователей, но и от**:
    - Частоты запросов (активные ли пользователи или заходят раз в неделю?)  
    - Типа данных (много ли храним? используем ли WebSockets?)  
    - Распределения нагрузки (одновременные соединения или распределённые по времени?)  
    - 100 пользователей играют одновременно в реальном времени — это средний уровень нагрузки  
    - 100 пользователей, но только 10-20 активны одновременно → небольшой проект  
    - 100 пользователей, но они все активно играют в реальном времени → средний проект  
    - 100 пользователей, но каждые 2 секунды генерируются сотни запросов → может быть нагруженным  
    - Если игра в реальном времени (ws + Redis Channels), Нагрузка выше, особенно если много обновлений состояния
    - Если используется кэширование (Redis как кэш), Нагрузка ниже, так как база нагружается меньше.  
    - Polling создаёт больше нагрузки, чем WebSockets.  
    - Если база данных нагружена (часто читаются и записываются данные, ...), **оптимизировать SQL-запросы и индексы**
  + минимизация нагрузки: кэширование, Channel Layers, daphne + Channels для асинхронной обработки, Nginx балансировка нагрузки
  + если код написан грамотно, то Redis, Django и PostgreSQL справятся с 100 пользователями
  + ws – Redis как Channel Layers поможет масштабировать и оптимизировать передачу данных. При 100 одновременных игроках трафик не критичен для Redis
  + PostgreSQL (профили, истории игр, турниры, сообщения) – Если запросы оптимизированы (**индексы на часто используемые поля, правильные связи**), выдержит
  + Сервер в одном инстансе – ок, нагрузка небольшая
* https://localhost:4443/ нормально открывается, а по https://localhost:4443/index.html 404
  + внутри SPA нет маршрута для пути "/index.html" — поэтому «клиентская» часть выводит надпись «404»
  + в SPA (React Router, Vue Router, ваш собственный роутер и т.д.) маршруты /, /login, /profile и т.п. могут быть прописаны в коде, но "/index.html" обычно не предусмотрен как валидный маршрут и воспринимается как «неизвестный адрес». В результате фронтенд при переходе на "/index.html" показывает экран «404 – Not Found» (или любое действие по умолчанию для неизвестных путей).
  + добавить соответствующее правило в вашем клиентском роутере (чтобы "/index.html" перенаправлялся на "/" или обрабатывался так же, как "/")
  + Сообщение "404 – Page Not Found" исходит из логики клиентского кода (SPA), потому что /index.html там не прописан
  + где-то в коде «клиента» (вашего фронтенда) прописать логику, которая:
    - Отслеживает текущий URL (например, через window.location.pathname или события popstate / hashchange).  
    - Если путь — `"/index.html"`, то вы либо  Перенаправляете браузер на "/", или Точно так же обрабатываете его, как если бы это был путь "/"
  + Nginx не должен выдавать реальную 404 на /index.html. Сейчас у вас уже настроено, что index.html физически отдается и статус запроса — 200.  
  + Если внутри app.js при загрузке вы показываете «404», значит это ваш JS решает, что «/index.html» — невалидный маршрут. Нужно добавить логику (как в примерах), чтобы считать его эквивалентным "/".  
  + При желании вы можете вообще редиректить браузер на "/", чтобы пользователь видел именно "/" в адресной строке. Либо просто обрабатывать "/index.html" так же, как "/", без изменения URL.
  + аким образом, добавить правило для "/index.html" можно прямо в ваш «клиентский роутер» (даже если он «кустарный»), переписывая путь на "/" или вручную вызывая код для главной страницы.
  + Пример 1. Моментальный редирект при загрузке
    - как только скрипт загружается, он проверяет, не находитесь ли вы на "/index.html". Если да, тут же подменяет URL на "/" (через history.replaceState) и продолжает работу.  
    - Визуально пользователь сразу же увидит "/" в адресной строке.  
    - Рендериться будет основной код, как будто пользователь зашёл на "/".
    - ```
      html
      <!-- index.html -->
      <!DOCTYPE html>
      <html>
      <head>
        <meta charset="utf-8">
        <title>My SPA</title>
      </head>
      <body>
        <div id="app"></div>
        <script src="app.js"></script>
      </body>
      </html>
      
      // app.js
      window.addEventListener("DOMContentLoaded", () => {
        // Если зашли на "/index.html", то заменяем путь на "/"
        if (window.location.pathname === "/index.html") {
          // Меняем URL (без перезагрузки) на "/"
          window.history.replaceState({}, "", "/");
        }
        // Далее ваш остальной код, который строит страницы в зависимости от location.pathname
        startSPA();
      });
      function startSPA() {
        // Простейший «роутер»:
        const path = window.location.pathname;
        if (path === "/") {
          showHomePage();
        } else if (path === "/login") {
          showLoginPage();
        } else {
          showNotFoundPage();
        }
      }
      ```
  + Пример 2. Обработка в роутере
    - принудительно считать "/index.html" за "/":
    - вы не меняете адресную строку, но показываете тот же контент, что и на "/"
    - ```
      js
      function startSPA() {
        let path = window.location.pathname;
        // Если путь /index.html, воспринимаем его как /
        if (path === "/index.html") {
          path = "/";
        }
        switch (path) {
          case "/":
            showHomePage();
            break;
          case "/login":
            showLoginPage();
            break;
          default:
            showNotFoundPage();
            break;
        }
      }
      ```
  + Пример 3. «Общий» роутер с несколькими маршрутами
    - объект routes, где ключи — это пути, а значения — функции для отрисовки
    - если человек набирает https://example.com/index.html в адресной строке, ваш JS увидит это, приравняет к "/", и покажет нужную страницу
    - ```
      js
      const routes = {
        '/': showHomePage,
        '/login': showLoginPage,
        // можете добавить и другие
      };
      // Функция, запускающая роутер
      function router() {
        let path = window.location.pathname;
        // Приравниваем /index.html к /
        if (path === '/index.html') {
          path = '/';
          // Дополнительно можно сделать history.replaceState({}, '', '/');
        }
        // Ищем обработчик, если нет — 404
        const routeHandler = routes[path] || showNotFoundPage;
        routeHandler();
      }
      // Запускаем роутер при загрузке страницы
      window.addEventListener('DOMContentLoaded', router);
      // Перехватываем «назад»/«вперёд» в браузере
      window.addEventListener('popstate', router);
      ```
* в REDIRECT_URI_42=http://127.0.0.1:8000/auth/callback **убрать 127.0.0.1:8000**
* oauth
  + без - ./.env:/app/.env
    ```
    backend  | 2025-02-08 11:13:06,348 INFO     Listening on TCP address 0.0.0.0:8000
    backend  | 2025-02-08 11:07:13,433 ERROR    code: b2576fcadb28f3cfc4ca237f216c604723996d05bf6637d4ce1308105a56d2b5
    backend  | 2025-02-08 11:07:13,433 ERROR    CLIENT_ID_42: u-s4t2ud-0f34f9a5a8eab694436a048c258fc509b67c21763329d02ade82ff3a77005eb8
    backend  | 2025-02-08 11:07:13,433 ERROR    CLIENT_SECRET_42: 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
    backend  | 2025-02-08 11:07:13,433 ERROR    REDIRECT_URI_42: http://127.0.0.1:8000/auth/callback
    backend  | 2025-02-08 11:07:13,581 ERROR    response: {'access_token': '7354455b03c5a130ac8e00e40511bd9bd820377c251f5aee91f4577c009dd733', 'token_type': 'bearer', 'expires_in': 6923, 'refresh_token': '7b8569e4268c2a9d83eb719302632647cf97eed1492eecbfd1e165a0eae748f7', 'scope': 'public', 'created_at': 1739012556, 'secret_valid_until': 1740819629}
    backend  | New user created with username: akostrik
    backend  | 172.22.0.1:49912 - - [08/Feb/2025:11:07:14] "GET /auth/callback?code=b2576fcadb28f3cfc4ca237f216c604723996d05bf6637d4ce1308105a56d2b5" 302 -
    ```
  + с - ./.env:/app/.env
    ```
    backend  | 2025-02-08 11:13:06,348 INFO     Listening on TCP address 0.0.0.0:8000
    backend  | 172.23.0.5:59248 - - [08/Feb/2025:11:23:03] "GET /api/auth/me" 200 54
    backend  | 172.23.0.5:59262 - - [08/Feb/2025:11:23:03] "GET /api/csrf-token/" 200 81
    ```

### ORGANISATION
* искать по истории коммитов и разным веткам в VS Code, установите GitLens
* https://github.com/bakyt92/14_ft_transendence
* https://docs.google.com/document/d/14zC4f2D8vdh9cYKosDQxsjWYc9aax2hPGuh8Y7CoENI/edit?tab=t.0
* https://docs.google.com/document/d/1O1r9jEdxISjMV29lZgLXWNh-bgPzSlnZ6Nr8QuyP_Jc/edit?pli=1
* Application - хранилище:
  + куки
  + local storage
  + ... storage
* basic requirements 20.01.2025 
  + All pong game part will be done by Amine? Do you need help with front-end (table, paddles, ball, some activity of JS or someone will do it?
  + Tournament, registration and matchmaking system by Alexey
  + Basic front-end will be done by Alexey? (profile page, other pages)
  + Security - probably we meet requirements by we need to validate input and follow some basic security rules on the front-end part.
* Modules (only that needs some response / comment)
  + User management - I did back-end (almost); but we need profile page with history of games, possibility to change profile data; see statistics of wins and loses - who will be responsible for this part? I can do it but I need template of js page (single page structure should be already applied) 
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
  + DEBUG = False
  + в продакшен отключают генерацию .map-файлов
  + remove frontend/etc/private.key
  + close ports 8000 and 6800 
  + localhost:8000 убрать
  + `http://backend:8000` хранить в перменной окружения
  + 42_REDIRECT_URI=http://127.0.0.1:8000/auth/callback убртаь
  + to justify your choices during the evaluation
