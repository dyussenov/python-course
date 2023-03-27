
# SQLITE в Python

SQLite – это библиотека на языке С, реализующая легковесную дисковую базу данных (БД), не требующую отдельного серверного процесса и позволяющую получить доступ к БД с использованием языка запросов SQL. Некоторые приложения могут использовать SQLite для внутреннего хранения данных.

> Для комфортной работы рекомендуется установить программу для работы с базами данных SQLite (Standard installer for 64-bit Windows):
>
> <https://sqlitebrowser.org/dl/>

Также возможно создать прототип приложения с использованием SQLite, а затем перенести код в более многофункциональную БД, такую как PostgreSQL или Oracle.

 > Модуль ```sqlite3``` реализует интерфейс SQL, соответствующий спецификации DB-API 2.0, описанной в PEP 249.

## Создание базы данных и подключение к ней

Есть несколько способов создания базы данных в Python с помощью SQLite. Для этого используется объект ```Connection```, который и представляет собой базу. Он создается с помощью функции ```connect()```.

Создадим файл ```.db```, поскольку это стандартный способ управления базой SQLite. Файл будет называться orders.db. За соединение будет отвечать переменная ```conn```.

```Python
conn = sqlite3.connect('orders.db')
```

Эта строка создает объект ```connection```, а также новый файл ```orders.db``` в рабочей директории (папке). Если нужно использовать другую, ее нужно обозначить явно:

```Python
conn = sqlite3.connect(r'database/orders.db')
```

Если файл уже существует, то функция ```connect``` осуществит подключение к нему.

> Перед строкой с путем стоит символ ```r```. Это дает понять Python, что речь идет о «сырой» строке, где символы ```/``` не отвечают за экранирование.

## Создание объекта ```cursor```

После создания объекта соединения с базой данных нужно создать объект ```cursor```. Он позволяет делать SQL-запросы к базе. Используем переменную ```cur``` для хранения объекта:

```Python
cur = conn.cursor()
```

Теперь выполнять запросы можно следующим образом:

```Python
import sqlite3

conn = sqlite3.connect('orders.db')
cur = conn.cursor()

cur.execute("ЗАПРОС-ЗДЕСЬ;")
```

Обратите внимание на то, что сами запросы должны быть помещены в кавычки — это важно. Это могут быть одинарные, двойные или тройные кавычки. Последние используются в случае особенно длинных запросов, которые часто пишутся на нескольких строках.

## Создание таблиц в SQLite в Python

Пришло время создать первую таблицу в базе данных. С объектами соединения (conn) и cursor (cur) это можно сделать. Будем следовать этой схеме:

| users       | orders      |
|-------------|-------------|
| **userid** *int*  | orderid *int* |
| fname *text*  | date *date*   |
| lname *text*  | **userid** *int*  |
| gender *text* | total *int*   |

Начнем с таблицы ```users```:

```Python
import sqlite3

conn = sqlite3.connect('orders.db')
cur = conn.cursor()

cur.execute("""CREATE TABLE IF NOT EXISTS users(
   userid INT PRIMARY KEY,
   fname TEXT,
   lname TEXT,
   gender TEXT);
""")
conn.commit()
```

В коде выше выполняются следующие операции:

- Функция ```execute``` отвечает за SQL-запрос
- SQL генерирует таблицу ```users```
- ```IF NOT EXISTS``` поможет при попытке повторного подключения к базе данных. Запрос проверит, существует ли таблица. Если да — проверит, ничего ли не поменялось.
- Создаем первые четыре колонки: ```userid```, ``fname``, ```lname``` и ```gender```. ```Userid``` — это основной ключ.
- Сохраняем изменения с помощью функции ```commit``` для объекта соединения.

Для создания второй таблицы просто повторим последовательность действий, используя следующие команды:

```Python
import sqlite3

conn = sqlite3.connect('orders.db')
cur = conn.cursor()

cur.execute("""CREATE TABLE IF NOT EXISTS orders(
   orderid INT PRIMARY KEY,
   date TEXT,
   userid TEXT,
   total TEXT);
""")
conn.commit()
```

После исполнения этих двух скриптов база данных будет включать две таблицы. Теперь можно добавлять данные.

> Создавать таблицы нужно лишь один раз. Поэтому рекумендуется создать отдельный файл с названием ```create-tables.py``` и прописать скрипты для создания таблиц в нем.

Пример скрипта для создания таблиц:

```Python
import sqlite3

conn = sqlite3.connect('orders.db')
cur = conn.cursor()

cur.execute("""CREATE TABLE IF NOT EXISTS users(
   userid INT PRIMARY KEY,
   fname TEXT,
   lname TEXT,
   gender TEXT);
""")

cur.execute("""CREATE TABLE IF NOT EXISTS orders(
   orderid INT PRIMARY KEY,
   date TEXT,
   userid TEXT,
   total TEXT);
""")

conn.commit()
conn.close() #закрываем соединение с базой данных
```

## CRUD

CRUD - сокращение от *create*, *read*, *update*, *delete* — создать, прочесть, обновить, удалить — акроним, обозначающий четыре базовые функции, используемые при работе с персистентными хранилищами данных.

> Практически любой сайт можно назвать CRUD-приложением.
CREATE, UPDATE, DELETE — например, комментарии пользователей, или наполнение сайта информацией контент-менеджером через админку. READ, соответственно, самая базовая операция по получению информации из БД. Открыли главную страницу — вот и READ.

### Добавление данных с SQLite в Python

По аналогии с запросом для создания таблиц для добавления данных также нужно использовать объект ```cursor```:

```Python
import sqlite3

conn = sqlite3.connect('orders.db')
cur = conn.cursor()

cur.execute("""
INSERT INTO users(userid, fname, lname, gender)
VALUES('00001', 'Alex', 'Smith', 'male');
""")
conn.commit()
```

В Python часто приходится иметь дело с переменными, в которых хранятся значения. Например, это может быть кортеж с информацией о пользователе:

```Python
user = ('00002', 'Lois', 'Lane', 'Female')
```

Если его нужно загрузить в базу данных, тогда подойдет следующий формат:

```Python
cur.execute("INSERT INTO users VALUES(?, ?, ?, ?);", user)
conn.commit()
```

В данном случае все значения заменены на знаки вопроса и добавлен параметр, содержащий значения, которые нужно добавить.

Важно заметить, что SQLite ожидает получить значения в формате кортежа. Однако в переменной может быть и список с набором кортежей. Таким образом можно добавить несколько пользователей:

```Python
import sqlite3

conn = sqlite3.connect('orders.db')
cur = conn.cursor()

more_users = [('00003', 'Peter', 'Parker', 'Male'), ('00004', 'Bruce', 'Wayne', 'male')]

#используем executemany()
cur.executemany("INSERT INTO users VALUES(?, ?, ?, ?);", more_users)
conn.commit()
```

Если применить ```execute```, то функция подумает, то пользователь хочет передать в таблицу два объекта, а не два кортежа, каждый из которых содержит по 4 значения для каждого пользователя. Хотя в первую очередь вообще должна была возникнуть ошибка.

Здесь вы можете взять данные для множественной загрузки данных:
<https://gitlab.com/destruction268/python-course-files/-/blob/main/sqlite_customers_orders_data.py>

Возьмите код из файла выше и добавьте это:

```Python
import sqlite3

conn = sqlite3.connect('orders.db')
cur = conn.cursor()

#вставить здесь две переменные из файла по ссылке

cur.executemany("INSERT INTO users VALUES(?, ?, ?, ?);", customers)
cur.executemany("INSERT INTO orders VALUES(?, ?, ?, ?);", orders)
conn.commit()
```

### Получение данных с SQLite в Python

Следующий момент касательно SQLite в Python — выбор данных. Структура формирования запроса та же, но к ней будет добавлен еще один элемент.

Начнем с использования функции ```fetchone()```. Создадим переменную one_result для получения только одного результата:

```Python
import sqlite3

conn = sqlite3.connect('orders.db')
cur = conn.cursor()

cur.execute("SELECT * FROM users;")
one_result = cur.fetchone()
print(one_result)
```

Она вернет следующее:

```Python
[(1, 'Alex', 'Smith', 'male')]
```

Если же нужно получить много данных, то используется функция ```fetchmany()```. Выполним другой скрипт для генерации 3 результатов:

```Python
import sqlite3

conn = sqlite3.connect('orders.db')
cur = conn.cursor()

cur.execute("SELECT * FROM users;")
three_results = cur.fetchmany(3)
print(three_results)
```

Он вернет следующее:

```Pyton
[(1, 'Alex', 'Smith', 'male'), (2, 'Lois', 'Lane', 'Female'), (3, 'Peter', 'Parker', 'Male')]
```

Функцию ```fetchall()``` можно использовать для получения всех результатов. Получение всех пользователей:

```Python
import sqlite3

conn = sqlite3.connect('orders.db')
cur = conn.cursor()

cur.execute("SELECT * FROM users;")
all_results = cur.fetchall()
print(all_results)
```

Рассмотрим алгоритм получения одной конкретной записи, которая нам нужна. Например, достанем пользователя с фамилией Parker:

```Python
import sqlite3

conn = sqlite3.connect('orders.db')
cur = conn.cursor()

cur.execute("SELECT * FROM users WHERE lname='Parker';")
all_results = cur.fetchall()
print(all_results)
```

### Удаление данных в SQLite в Python

Теперь рассмотрим процесс удаления данных с SQLite в Python. Здесь та же структура. Предположим, нужно удалить любого пользователя с фамилией «Parker». Напишем следующее:

```Python
import sqlite3

conn = sqlite3.connect('orders.db')
cur = conn.cursor()

cur.execute("DELETE FROM users WHERE lname='Parker';")
conn.commit()
```

Если затем сделать следующий запрос, то будет выведен пустой список, подтверждающий, что запись удалена:

```Python
import sqlite3

conn = sqlite3.connect('orders.db')
cur = conn.cursor()

cur.execute("select * from users where lname='Parker'")
print(cur.fetchall())
```

### Обновление данных в SQLite в Python

Процесс обновления данных схож с удалением. Предположим, нужно сменить имя у пользователя с id 10. Напишем следующее:

```Python
import sqlite3

conn = sqlite3.connect('orders.db')
cur = conn.cursor()

cur.execute("UPDATE users SET fname='John' where userid=10;")
conn.commit()
```

## SQL запросы и f-строки

Довольно частро приходится в запросах использовать данные из python объектов. Для вставки подобных значений отлично подходят f-строки:

```Python
import sqlite3

conn = sqlite3.connect('orders.db')
cur = conn.cursor()

user_lname = input('Введите фамилию: ')

cur.execute("SELECT * FROM users WHERE lname='{user_lname}';")
all_results = cur.fetchall()
print(all_results)
```

> Обратите внимание на кавычки вокруг фигурных скобок внутри f-строки. Их обязательно нужно использовать при обращении к текстовым полям таблицы, так как этого требует SQL. Фигурные скобки сами по себе не добавляют кавычек.
