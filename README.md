# pr1-protocol-research

Виконав студент КН-41 Феделеш Андрій

## Рівень 1

### 1. Дослідження методів REST API в Postman:
   - Виконати запит GET https://dummyjson.com/products/1 — проаналізувати отримане тіло
JSON, код статусу 200 OK, час відповіді та розмір завантажених даних.

<img width="952" height="820" alt="зображення" src="https://github.com/user-attachments/assets/9a75ac73-95ad-48c9-a6fc-96d6ed47ff8a" />

На скриншоті бачимо що запит пройшов успішно "200 ОК", та те що він пройшов за 175мс і важить 1.69Кб.

  - Виконати запит POST https://dummyjson.com/products/add — передати у тілі (Body → raw
JSON) об’єкт нового товару (поля title, price). Проаналізувати код статусу 201 Created
та отриманий згенерований id.

<img width="946" height="591" alt="зображення" src="https://github.com/user-attachments/assets/1868cfc8-52cc-4b42-b541-77af08d8496b" />

Код "201 Created" показує що ми успішно створили новий ресурс. Ми отримали новий згенерований id 195.

  - Виконати запит PUT https://dummyjson.com/products/1 — надіслати змінене поле title
та перевірити повернений результат.

<img width="938" height="783" alt="зображення" src="https://github.com/user-attachments/assets/b8327d92-65bc-44f5-941b-3e55bbfe12f0" />

Ми успішно змінили поле title на "Updated Test Product".

  - Виконати запит DELETE https://dummyjson.com/products/1 — зафіксувати код статусу та
наявність позначки видалення isDeleted: true.

<img width="916" height="826" alt="зображення" src="https://github.com/user-attachments/assets/82c17240-a256-4e0f-9d83-c7522b0cb40b" />

Код статусу "200 ОК" та isDeleted: true.

### 2. Аналіз структури в DevTools:
   - Відкрити у веббраузері адресу https://dummyjson.com/products із відкритою панеллю
DevTools (вкладка Network).
  Знайти відповідний запит у списку мережевої активності та зафіксувати в окремій
таблиці:
  ▪ General: Request URL, Request Method, Status Code, Remote Address.
  ▪ Response Headers: content-type, date, server, etag.
  ▪ Request Headers: accept, user-agent, accept-encoding.
  ▪ Вкладку Timing: проаналізувати тривалість фаз DNS Lookup, Initial connection,
Waiting for server response (TTFB), Content Download.

<img width="1265" height="754" alt="зображення" src="https://github.com/user-attachments/assets/57d5b7d7-a0ca-4073-b9dc-01aa28b7e00d" />


General:
- Метод: GET
- URL: https://dummyjson.com/products
- Статус: 200 OK
- HTTP-версія: HTTP/2
- Адреса сервера: 104.21.61.23:443
- Передано: 8,94 кБ
- Розмір відповіді: 44,09 кБ

Response Headers:
- Content-Type: application/json; charset=utf-8
- Date: Tue, 22 Sep 2026 14:08:57 GMT
- Server: cloudflare
- ETag: W/"ac3a-Q0j5X7Zb/GG4CpZwhP3POutAwN4"
- Content-Encoding: br

Request Headers:
- Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
- Accept-Encoding: gzip, deflate, br, zstd
- User-Agent: Mozilla/5.0 ... Firefox/156.0
- Accept-Language: uk-UA,uk;q=0.9,en-US;q=0.8,en;q=0.7

Timing:
- Заблоковано: 0 мс
- DNS: 0 мс
- З'єднання: 0 мс
- TLS: 0 мс
- Надсилання: 0 мс
- Очікування (TTFB): 18 мс
- Одержання: 0 мс
- Загальний час: 18 мс

## Рівень 2: консольна діагностика мережевої взаємодії через curl

### 1. Базові виклики та робота з методами:
- Надіслати запит GET на отримання списку користувачів https://dummyjson.com/users?limit=2&select=firstName,email через термінал за допомогою curl.
  
<img width="1261" height="50" alt="зображення" src="https://github.com/user-attachments/assets/db463bb5-ee9f-463a-b83a-f410c4d6fa15" />

- Виконати запит додавання сутності методом POST із передачею JSON-рядка через
прапорець -d та явним встановленням заголовка Content-Type: application/json за
допомогою прапорця -H:
▪ Ресурс: https://dummyjson.com/posts/add
▪ Тіло: об’єкт із полями title та userId.

<img width="1208" height="35" alt="зображення" src="https://github.com/user-attachments/assets/5b80feed-0e8a-4a1f-9a71-70d43199d957" />

### 2. Аналіз заголовків та статусів без завантаження тіла:
- Виконати команду curl -I для ресурсу https://dummyjson.com/products. Зафіксувати
отримані заголовки сервера.

<img width="1262" height="405" alt="зображення" src="https://github.com/user-attachments/assets/eca4579a-5b4c-4983-88aa-b2a5c7eeacea" />

- Здійснити запит до https://httpbin.org/status/404 та https://httpbin.org/status/500 з
прапорцем -i, щоб переконатися у виведенні заголовків із відповідними статусами
помилок.

<img width="474" height="330" alt="зображення" src="https://github.com/user-attachments/assets/23bdedff-7b69-4197-b66a-f18b9e79dc79" />

### 3. Автентифікація через заголовок та передача токена:
- Отримати тестовий токен автентифікації, надіславши POST-запит до https://dummyjson.com/auth/login з обліковими даними:
- username: "emilys"
- password: "emilyspass"
 
<img width="1261" height="50" alt="зображення" src="https://github.com/user-attachments/assets/eed1400d-f1d0-4068-ad96-e9286c829123" />

- Витягти отриманий accessToken та виконати наступний запит до захищеного
ендпоінта https://dummyjson.com/auth/me, передавши токен у заголовку Authorization:
Bearer <TOKEN> через прапорець -H. Зафіксувати успішну відповідь із даними
користувача.

<img width="1262" height="162" alt="зображення" src="https://github.com/user-attachments/assets/d4a8d660-d7b3-46c4-bf01-f73b7643421e" />

### 4. Вимірювання мережевих метрик (Performance profiling):
   
Скласти запит із використанням прапорця -w для отримання детальної розбивки
таймінгів з’єднання з https://dummyjson.com/products:
▪ Час DNS резолвінгу (%{time_namelookup}s);
▪ Час TCP з’єднання (%{time_connect}s);
▪ Час узгодження TLS (%{time_appconnect}s);
▪ Час очікування першого байта сервера TTFB (%{time_starttransfer}s);
▪ Загальний час операції (%{time_total}s).

<img width="1258" height="114" alt="зображення" src="https://github.com/user-attachments/assets/ae45d0e5-499b-4b2f-8941-74a27c2208c9" />









  






