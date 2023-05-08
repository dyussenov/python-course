
# DML запросы: INSERT, UPDATE и DELETE

## Определение DML

DML (Data Manipulation Language) - язык манипулирования данными. Под манипулированием данными подразумевается изменение состояния записей в таблицах: добавление новых, изменинение или удаление старых. За эти действия отвечают операторы `INSERT`, `DELETE` и `UPDATE`.

> В рамках некоторых определений считается, что к DML можно отнести оператор `SELECT`. В данном цикле статей оператор SELECT выносится в отдельную категорию DQL.

Для начала работы с примерами из данной статьи, запустите скрипт для создания базы данных и таблицы (этот скрипт подробно рассматривался в предыдущей статье):

```python
from sqlite3 import connect

with connect('database.db') as connection:
    cur = connection.cursor()
    cur.execute('''CREATE TABLE "users" (
    "name"  TEXT,
    "age"   INTEGER,
    "city"  TEXT
);
''')
```

## INSERT. Оператор добавления записей.

SQL запрос на добавление новой записи выглядит так:

```sql
INSERT INTO "users" ("name", "age", "city") VALUES("Ваня", 25, "Москва");
```

- INSERT - оператор добавления записи.

- INTO "users" ("name", "age", "city") - название таблицы и список полей, в которые будут помещаться данные.

- VALUES("Ваня", 25, "Москва") - значения, которые будут помещены в соответствующие поля.

---

В простейшем виде подобный запрос в Python приложении выполняется так:

```python
from sqlite3 import connect

with connect('database.db') as connection:
    cur = connection.cursor()
    cur.execute('INSERT INTO "users" ("name", "age", "city") VALUES("Ваня", 25, "Москва");')
    connection.commit()
```

> При всех DML запросах нужно выполнять коммит `connection.commit()`

---

Запрос описанный выше работает, но не считается хорошей практикой. В такой запрос неудобно подставлять необходимые данные, а также уязвим к потенциальным SQL-инъекциям. Хорошей практикой является использование знаков вопроса в строке запроса: 

```python
from sqlite3 import connect

with connect('database.db') as connection:
    cur = connection.cursor()
    data = ('Иван', 25, 'Москва')
    cur.execute('INSERT INTO "users" ("name", "age", "city") VALUES (?, ?, ?)', data)
    connection.commit()
```

В этом примере, мы создали кортеж `data` и передали его как параметры в запрос на добавление записи. Вместо `?` в запросе будут подставлены соответствующие значения переменных.

---

Также мы можем передать список кортежей и обработать его при помощи метода `executemany()`:

```python
from sqlite3 import connect

with connect('database.db') as connection:
    cur = connection.cursor()
    data = [
        ('Иван', 25, 'Москва'),
        ('Дмитрий', 19, 'Казань'),
        ('Елена', 23, 'Краснодар'),
    ]
    cur.executemany('INSERT INTO "users" ("name", "age", "city") VALUES (?, ?, ?)', data)
    connection.commit()
```

## UPDATE. Обновление записей

Для обновления информации в существующих записях используется оператор `UPDATE`:

```SQL
UPDATE "users" SET "city" = "Санкт-Петербург", "age"=24;
```

Данный запрос установит город Санкт-Петербург и возраст 24 для всех записей в таблице. Подобное обновление всех записей нужно крайне редко, а вот наоборот - изменить поле в какой-то конкретной записи необходимо довольно часто. Для этого мы можем установить фильтрацию при помощи оператора `WHERE`:

```SQL
UPDATE "users" SET "city" = "Санкт-Петербург", "age"=24 WHERE "name"="Ваня";
```

Данный запрос установит новые значения полей только для записей, где поле "имя" имеет значение "Ваня". В выражении `WHERE` можно использовать различные операторы сравнения и логические операторы, такие как:

- `=` - равно
- `<>` или `!=` - не равно
- `<` - меньше
- `>` - больше
- `<=` - меньше или равно
- `>=` - больше или равно
- `AND` - логическое И
- `OR` - логическое ИЛИ
- `NOT` - логическое НЕ

Пример запроса на обновление в Python:

```python
from sqlite3 import connect

with connect('database.db') as connection:
    cur = connection.cursor()
    cur.execute('UPDATE "users" SET "city" = "Санкт-Петербург", "age"=24 WHERE "name"="Ваня";')
    connection.commit()
```

## DELETE. Удаление записей

Команда `DELETE` используется для удаления строк из таблицы. Она позволяет удалить все строки из таблицы или только те строки, которые удовлетворяют определенному условию, заданному в выражении `WHERE`.

```sql
DELETE FROM "users" WHERE "name"="Ваня";
```

Пример запроса на удаление в Python:

```python
from sqlite3 import connect

with connect('database.db') as connection:
    cur = connection.cursor()
    cur.execute('DELETE FROM users WHERE age > 23;')
    connection.commit()
```

В в одной из предыдущих статей рассматривался запрос на удаление всех записей в таблице как замена отсутствующего оператора `TRUNCATE`:

```sql
DELETE FROM users;
```