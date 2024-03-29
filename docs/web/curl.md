Curl (сокращение от "client for URLs") — это универсальная командная строка для работы с URL-адресами. Она позволяет выполнять различные операции сетевого взаимодействия, такие как отправка HTTP-запросов, загрузка файлов, выполнение простых запросов DNS и многое другое. 



Всего у `curl` около 250 опций, но популярностью пользуются только 25-40. Самые частоиспользуемые показаны в таблице ниже.


| Опция | Второе имя опции | Описание |
|-----------|----------|---------|
| --data  `<data>`  | -d | Определяет данные, которые нужно отправить вместе с запросом. Например: `param1=value1&param2=value2`|
| --include    | -i   | Включает заголовки ответа в вывод|
| --output `<file>`   | -o   | Позволяет сохрянять файл с новым именем.|
| --remote-name    | -O   | Скачивание и сохранение файла с тем же именем, что и на сервере.|
| --silent    | -s   | Отключает индикатор прогресса при скачивании файла через curl.|
| --upload-file    | -T   | Позволяет выгружать локальный файл на удалённый сервер.|
| --user `<user:password>`    | -u   |Можно не вводить пароль пользователя, тогда curl попросит ввести его в режиме no-echo. |
| --user-agent    | -A   | Сообщает серверу тип клиента, отправляющего запрос. При отправке запроса curl на сервер по умолчанию используется `user-agent curl/<version>`. Если сервер настроен так, чтобы блокировать запросы curl, можно задать собственный user-agent при помощи этой опции. |
| --verbose    | -v   | Отображает дополнительный заголовок, отправляемый с каждым запросом curl.|
| --version    | -V   | Выводит список протоколов и функций, поддерживаемых текущей версией curl. Если вместо этого отображается сообщение об ошибке, curl может быть недоступен потому, что вы используете более раннюю версию Windows (например, Windows 8.1 или Server 2016). В таком случае вам потребуется установить curl в Windows вручную.|
| --fail    | -f   | Создает ошибку без вывода. |
| --head    | -I   | Позволяет просматривать только заголовки HTTP. |
| --location    | -L   | Перенаправляет пользователя в другую точку. |
| --request    | -X   | Позволяет curl использовать методы HEAD, POST, PUT и DELETE |
| --insecure    | -k   | Разрешает незащищённые подключения и избежать ошибок с сертификатом TLS в случае, если целевой URL использует самоподписанный сертификат.|
| --continue-at    | -C   | Продолжить прерванное скачивание файла.|
| --config    | -K   | Curl поддерживает использование файлов конфигурации `.curlrc, _curlrc` и `.netrc`, позволяющих задавать различные опции curl в файле, а затем добавлять файл в команду при помощи данной опции.  |
| --header    | -H   | Иногда вместе с запросом к серверу необходимо отправить дополнительную информацию. Сделать это можно при помощи этой опции.|
| --proxy  `<server:port>`  | -x   |Если вы пользуетесь прокси-сервером для подключения к интернету, в curl можно указать прокси этой опцией.  |
| --proxy-user  `<username:password>`  | -U   |  Если прокси-сервер требует аутентификации, то необходимо использовать данную функцию. Можно не вводить пароль пользователя прокси, тогда curl попросит ввести его в режиме no-echo. |

На самом деле, опций у `curl` намного больше. Для того, чтобы получить полный список опций, необходимо ввести следующую команду:
```bash
curl --help all
```



Вот несколько основных способов использования curl:

1. Получение содержимого веб-страницы:
   ```bash
   curl https://example.com
   ```
   В данном случае выводом будет содержимое HTML-файла с сайта example.com

2. Загрузка файла:
   ```bash
   curl -O https://www.africau.edu/images/default/sample.pdf
   ```
   Эта команда загружает файл с указанного URL-адреса и сохраняет его в текущей директории с именем, соответствующим имени файла на сервере.

3. Отправка POST-запроса:
   ```bash
   curl -X POST -d "param1=value1&param2=value2" <URL>
   ```
   В этом примере `-X POST` указывает curl на выполнение POST-запроса, а `-d` определяет данные, которые нужно отправить вместе с запросом.

4. Включение вывода заголовков:

   ```bash
   curl -i https://example.com
   ```
   
   Помимо HTML-файла, консоль выведет информацию о ответе сервера:
   ```bash
   HTTP/1.1 200 OK 
   Age: 356277     
   Cache-Control: max-age=604800
   Content-Type: text/html; charset=UTF-8
   Date: Tue, 11 Jul 2023 07:23:01 GMT
   Etag: "3147526947+gzip+ident"
   Expires: Tue, 18 Jul 2023 07:23:01 GMT
   Last-Modified: Thu, 17 Oct 2019 07:18:26 GMT
   Server: ECS (nyb/1D2F)
   Vary: Accept-Encoding
   X-Cache: HIT
   Content-Length: 1256 
   ```

Это только небольшая часть возможностей curl. Он поддерживает множество опций и параметров командной строки, которые позволяют настраивать поведение запросов и работать с разными протоколами.