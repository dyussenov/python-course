# Протокол HTTP

Протокол HTTP - неотъемлемая часть современного интернета. HTTP (Hypertext Transfer Protocol), или протокол передачи гипертекста - универсальный язык коммуникации между различными сервисами.

Протокол, в данном контексте, это набор правил и соглашений, определяющих формат и способ взаимодействия между двумя или более участниками в сети. В случае HTTP, протокол представляет собой набор правил, которые определяют, как клиент и сервер должны обмениваться данными, чтобы осуществлять передачу гипертекстовых документов и других ресурсов через Интернет.

## Клиенты и серверы

Протокол HTTP подразумевает диалог между двумя участниками: ***клиентом*** и ***сервером***.

-  ***Клиентом*** может являться браузер, мобильное приложение или любая другая программа, способная отправлять HTTP запросы. 

- ***Сервером*** является комьютер со специальным программным обеспечением, которое принимает запросы от клиентов, обрабатывает их и отправляет ответы клиентам.

> Диалог является отчасти односторонним, так как только клиент способен отправлять запросы. Сервер же способен только ждать этот запрос и отправить ответ после его обработки.

![query string](/assets/web/http/http протокол.png)

## Структура HTTP запроса

Ниже представлен пример простого HTTP запроса:

```http
POST /api/users HTTP/1.1
Host: example.com
Content-Type: application/json

{"name": "John", "email": "john@example.com"}
```

Разберем этот запрос:

1. Стартовая строка: `POST /api/users HTTP/1.1`

    - `POST`: в первую очередь указывается **HTTP-метод**, в нашем случае это метод `POST`. Если говорить вкратце - **HTTP-метод** говорит серверу в каком ключе обрабатывать запрос. Конвенционально метод `POST` используется для добавления данных. Это может означать, что данные отправленные в данном запросе будут добавлены, например, в базу данных. Подробнее все методы будут рассмотрены далее.

    - `/api/users`: это **URI** (Uniform Resource Identifier, унифицированный идентификатор ресурса). путь до конкретного ресурса (например, документа), над которым необходимо осуществить операцию. екоторые запросы могут не относиться к какому-либо ресурсу, в этом случае вместо URI в стартовую строку может быть добавлена звёздочка.

    - `HTTP/1.1`: версия протокола HTTP.

2. Заголовки. В нашем случае их 2, но может быть больше:

    - `Host: example.com` : указывает на сервер, к которому следует обратиться.

    - `Content-Type: application/json` : указывает, что тело запроса содержит данные в формате JSON.

3. Пустая строка. Её назначение - сообщить, что все заголовки перечислены и далее будет передано тело запроса.

4. Тело запроса: `{"name": "John", "email": "john@example.com"}`. В нашем случае это данные в формате JSON, как и сообщалось в заголовке `Content-Type`. Также бывают случаи, когда тело запроса остается пустым. Например у метода `GET` не может быть тела запроса.

![структура запроса](/assets/web/http/структура запроса.png)

## Query string

Помимо тела, информацию о запросе можно передать прямо в стартовой строке запроса. Для этого к URI нужно добавить знак вопроса `?` и прописать данные в формате `ключ=значение`, а пары разделить через `&`:

```url
https://www.example.com/search?q=apple&category=fruits
```

![query string](/assets/web/http/query-string.png)

> Зачастую query-string используется именно с GET запросами, но строгой привязки к этому типу запросов нет. Вы можете исспользовать query-string с любым методом запросов.

## HTTP методы

Различные методы HTTP существуют для разделения и определения различных типов операций, которые могут быть выполнены над ресурсами. Они обеспечивают удобство и гибкость при выполнении операций получения, создания, обновления и удаления данных на сервере. Мы можем обращаться к одному и тому же ресурсу, но с разными запросами: GET - чтобы получить, PUT - чтобы заменить, DELETE - чтобы удалить. Рассмотрим подробнее каждый из них:

- `GET`. Используется для получения ресурса с сервера. Клиент отправляет GET-запрос для получения данных и ресурса, указанного в URI. GET-запросы могут содержать параметры, передаваемые в строке запроса.

- `POST` используется для отправки данных на сервер для обработки. Клиент отправляет POST-запрос с данными, которые будут обработаны на сервере. POST-запросы могут содержать тело запроса, которое содержит данные для передачи.

- `PUT` используется для создания или обновления ресурса на сервере. Клиент отправляет PUT-запрос с данными, которые должны быть сохранены по указанному URI. Если ресурс уже существует, PUT-запрос обновит его содержимое, а если ресурс не существует, он будет создан. PUT-запросы также могут содержать тело запроса.

- `DELETE` используется для удаления ресурса на сервере. Клиент отправляет DELETE-запрос для удаления ресурса, указанного в URI. DELETE-запросы изменяют состояние сервера путем удаления ресурса. Повторные DELETE-запросы к одному и тому же ресурсу обычно должны возвращать статус "404 Not Found", поскольку ресурс будет удален.

- `PATCH` используется для частичного обновления ресурса на сервере. Клиент отправляет PATCH-запрос с данными, которые должны быть применены к ресурсу. Этот метод позволяет обновлять только определенные части ресурса, не трогая остальное содержимое. PATCH-запросы также могут содержать тело запроса.

- `HEAD` выполняет запрос на получение заголовков ответа от сервера для указанного ресурса. Ответ на HEAD-запрос содержит только заголовки ответа без тела ответа. Этот метод полезен, когда клиенту требуется получить метаданные ресурса без необходимости получения всего содержимого.

- `OPTIONS` используется для получения списка поддерживаемых методов, заголовков или другой конфигурационной информации для указанного ресурса на сервере. Этот метод позволяет клиенту определить возможности сервера и принять соответствующие решения перед отправкой реального запроса.

## Идемпотентность и безопасноть

**Идемпотентность** (в контексте HTTP) означает что при повторном выполнении одного и того же запроса результат будет одинаковым, и состояние сервера не изменится. Иначе говоря: идемпотентная операция может быть выполнена множество раз, но результат останется одинаковым, как если бы она была выполнена только один раз. Список идемпотентных методов:

- `GET`: GET-запросы являются идемпотентными, так как они просто запрашивают данные и не вносят изменений на сервере. Повторные GET-запросы к одному и тому же ресурсу не должны изменять его состояние.

- `PUT`: PUT-запросы используются для создания или обновления ресурса на сервере. Например, в `users/username_1` у нас имеется иформация о неком пользователе. Выполняя первый PUT запрос, мы полностью перезаписываем имеющуюся информацию. При последующих PUT запросах с тем же содержимым - мы перезаписываем те же данные, так что состояние сервера после выполненной операции останется тем же, что и после первого PUT запроса

- `DELETE`: По определению является идемпотентным, но есть нюанс. Проблема в том, что успешный DELETE-запрос по URI `users/username_1` должен удалить пользователя `username_1`, но для последующих запросов будет всё время возвращать 404 (Not Found), так как пользователь уже не существует. Возьмем в учет, что при первом DELETE запросе мы удалили запись о пользователе - изменили состоянии базы данных, а при повторном не нашли его - база данных осталось в прежнем состоянии - удалять уже нечего. Отталкиваясь от этого, мы можем назвать метод DELETE идемпотентным.

---

**Безопасность** означает, что вызов метода не имеет побочных эффектов. Следовательно, такие (безопасные) запросы клиенты могут безопасно совершать неоднократно, не опасаясь изменить состояние сервера. Это означает, что сервисы должны придерживаться определения безопасности для GET, HEAD, OPTIONS и TRACE операций. Не выполнения этого свойства может приводить в заблуждение потребителя сервиса, а также вызвать проблемы для веб-кеширования, поисковых систем и других автоматизированных агентов, которые непреднамеренно будут изменять состояние сервера.

Иначе говоря, безопасный метод гарантирует отсутсвие побочных эффектов при первом и последующих запросах, в то время как идемпотентные запросы изменяют состояние при первом запросе, но не изменяют при последующих. Также, исходя из этого утверждения можно усвоить, что все безопасные методы идемпотентны.

> Важно понимать, что идемпотентность и безопасность в вашем серверном приложении вы должны соблюдать самостоятельно. Например, вам ничего не мешает полностью удалить базу данных при получении GET запроса или ничего не добавлять при правильном POST запросе. Но хотели бы вы жить в мире, где каждый разработчик устраивал такую анархию?

## HTTP заголовки

Ниже приведен список наиболее распространенных HTTP заголовков:

| Название       | Пример                                                                     | Описание                                                                                                                                                                                                                        |
|----------------|----------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|  Host          | Host: www.example.com                                                      | Указывает доменное имя сервера, на который отправляется запрос.                                                                                                                                                                 |
| User-Agent     | User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64)                      | Содержит информацию о программе (например, веб-браузере) и операционной системе, используемых для выполнения запроса. Серверы могут использовать эту информацию для оптимизации ответов для конкретных пользовательских агентов |
| Content-Type   | Content-Type: application/json                                             | Определяет тип данных, содержащихся в теле запроса или ответа. Это позволяет серверу и клиенту корректно интерпретировать данные.                                                                                               |
| Content-Length | Content-Length: 1024                                                       | Указывает размер тела запроса или ответа в байтах. С помощью этого заголовка клиент и сервер могут определить конец передачи данных.                                                                                            |
| Accept         | Accept: text/html, application/xhtml+xml, application/xml;q=0.9, */*;q=0.8 | Содержит список MIME-типов (типов содержимого), которые клиент готов принять в ответ на запрос. Сервер может использовать этот заголовок для определения наилучшего формата ответа.                                             |
| Authorization  | Authorization: Basic YWxhZGRpbjpvcGVuc2VzYW1l                              | Используется для передачи учетных данных аутентификации на сервер. В примере указан базовый метод аутентификации (Basic Auth), где "YWxhZGRpbjpvcGVuc2VzYW1l" представляет закодированные имя пользователя и пароль.            |
| Cache-Control  | Cache-Control: max-age=3600                                                | Определяет настройки кеширования для ресурса. Значение "max-age=3600" указывает, что ресурс может быть кэширован на протяжении 3600 секунд (1 часа).                                                                            |
| Referer        | Referer: https://www.example.com/page1.html                                | Содержит URL предыдущей страницы, с которой был сделан текущий запрос. Сервер может использовать эту информацию для аналитики или контроля.                                                                                     |
| Cookie         | Cookie: sessionid=abc123; username=johndoe                                 | Передает серверу информацию о ранее сохраненных cookie, связанных с текущим доменом. Сервер может использовать эту информацию для управления состоянием сессии.                                                                 |


> При необходимости, вы можете определить свой новый заголовок. В таком случае к названию нового заголовок стоит добавить префикс `X-`, например, `"X-CustomHeader: value"`. Но прежде чем изобретать свой велосипед, рекомендуется ознакомиться с полным списком заголовков. 

## Структура HTTP ответа

Во многом HTTP ответ похож на запрос. Ниже приведен пример успешного ответа на запрос:

```http
HTTP/1.1 200 OK
Content-Type: application/json
Content-Length: 1024

{
   "message": "Hello, World!"
}
```

Видимым отличием от HTTP запроса является стартовая строка. Стркутра стартовой строки HTTP ответа:

- `HTTP/1.1`: тут все должно быть понятно

- `200`: статус-код. Числовое обозначение статуса обработки запроса. Подробнее - далее.

- `OK`: пояснение стастус-кода

Также существуют заголовки, которые возвращаются только при ответе:

```http
200 OK
Access-Control-Allow-Origin: *
Connection: Keep-Alive
Content-Encoding: gzip
Content-Type: text/html; charset=utf-8
Date: Mon, 18 Jul 2016 16:06:00 GMT
Etag: "c561c68d0ba92bbeb8b0f612a9199f722e3a621a"
Keep-Alive: timeout=5, max=997
Last-Modified: Mon, 18 Jul 2016 02:36:04 GMT
Server: Apache
Set-Cookie: mykey=myvalue; expires=Mon, 17-Jul-2017 16:06:00 GMT; Max-Age=31449600; Path=/; secure
Transfer-Encoding: chunked
Vary: Cookie, Accept-Encoding
X-Backend-Server: developer2.webapp.scl3.mozilla.com
X-Cache-Info: not cacheable; meta data too large
X-kuma-revision: 1085259
x-frame-options: DENY
```

## Статус-коды HTTP

Статус-код в протоколе HTTP представляет числовой код, который возвращается сервером в ответ на запрос клиента. Он предоставляет информацию о результате выполнения запроса и состоянии ответа. Статус-коды используются для обозначения различных сценариев и условий, которые могут возникать при обработке HTTP запросов.

Статус-код состоит из трех цифр и обычно представлен в виде трехзначного числа. Каждая группа статус-кодов имеет свою семантику:

| Категория | Описание                                                                                                                                                                                                    |
|-----------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 1xx       | Коды из данной категории носят исключительно информативный характер и никак не влияют на обработку запроса.                                                                                                 |
| 2xx       | Коды состояния из этой категории возвращаются в случае успешной обработки клиентского запроса.                                                                                                              |
| 3xx       | Эта категория содержит коды, которые возвращаются, если серверу нужно перенаправить клиента.                                                                                                                |
| 4xx       | Коды данной категории означают, что на стороне клиента был отправлен некорректный запрос. Например, клиент в запросе указал не поддерживаемый метод или обратился к ресурсу, к которому у него нет доступа. |
| 5xx       | Ответ с кодами из этой категории приходит, если на стороне сервера возникла ошибка.                                                                                                                         |

Список наиболее распротраненных статус кодов:

| Статус-код | Описание                           | Развернутое описание                                 |
|------------|------------------------------------|----------------------------------------------------------------|
| 200        | OK                                 | Запрос успешно выполнен, содержимое ответа передается в теле ответа. |
| 201        | Created                            | Запрос успешно выполнен, новый ресурс был создан.                 |
| 204        | No Content                         | Запрос успешно выполнен, но в ответе отсутствует содержимое.       |
| 301        | Moved Permanently                  | Запрашиваемый ресурс был перемещен на новый постоянный URI.       |
| 302        | Found                              | Запрашиваемый ресурс временно доступен по другому URI.            |
| 304        | Not Modified                       | Ресурс не изменился с момента последнего запроса, используется кэш. |
| 400        | Bad Request                        | Запрос содержит синтаксическую ошибку или неверные параметры.      |
| 401        | Unauthorized                       | Для доступа к запрашиваемому ресурсу требуется аутентификация.      |
| 403        | Forbidden                          | Доступ к запрашиваемому ресурсу запрещен, отсутствуют необходимые права. |
| 404        | Not Found                          | Запрашиваемый ресурс не найден на сервере.                         |
| 500        | Internal Server Error              | Произошла внутренняя ошибка сервера при обработке запроса.         |
| 502        | Bad Gateway                        | Сервер, выступая в роли шлюза или прокси, получил недействительный ответ от сервера выше по цепочке. |
| 503        | Service Unavailable                | Сервер временно недоступен, обычно из-за перегрузки или проведения технического обслуживания. |
| 504        | Gateway Timeout                    | Сервер, выступая в роли шлюза или прокси, не получил ответ вовремя от сервера выше по цепочке. |

## HTTPS
HTTPS (HyperText Transfer Protocol Secure) - это защищенный протокол передачи гипертекста. Он является комбинацией стандартного протокола HTTP и протокола шифрования SSL/TLS (Secure Sockets Layer/Transport Layer Security). HTTPS обеспечивает безопасное и зашифрованное соединение между веб-браузером пользователя и веб-сервером, что позволяет защитить конфиденциальность и целостность передаваемых данных.

Чтобы установить HTTPS-соединение, браузеру и серверу надо договориться о симметричном ключе. Для этого сначала браузер и сервер обмениваются асимметрично зашифрованными сообщениями, где указывают секретный ключ и далее общаются при помощи симметричного шифрования.

Функции протокола HTTPS:
1. Шифрование данных. При использовании HTTPS все данные, передаваемые между клиентом (обычно веб-браузером) и сервером, шифруются с использованием криптографических алгоритмов. Это предотвращает возможность перехвата и чтения данных злоумышленниками.
2. Аутентификация. Посетители уверены, что переходят на официальный сайт компании, а не на дубликат, сделанный злоумышленником.
3. Сохранение данных. Протокол фиксирует все изменения данных. Если злоумышленник всё-таки пытался взломать защиту, об этом можно узнать из сохранённых данных.

## GET-запрос Curl

Чтобы отправить HTTP-запрос GET через URL в терминале, вы можете использовать утилиту curl:

```
curl <URL>
```

Ниже представлен пример простого GET-запроса Curl:

```bash
curl https://example.com/
```

Ответ сервера на ваш запрос Curl:

```bash
HTTP/1.1 200 OK
Content-Type: text/html
Content-Length: 643

[html-код]
```