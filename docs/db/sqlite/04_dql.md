
# DQL запрос SELECT. Получение записей

## Определение DQL

DQL (Data Query Language) - используется для выборки данных из таблиц базы данных. Эта группа включает в себя один основной оператор - SELECT, который может быть модифицирован операторми WHERE, GROUP BY, ORDER BY.

SQL запрос на получение всех записей из таблицы users:

```sql
SELECT * FROM "users";
```

Символ `*` означает, что будут выведены все поля для полученных записей. Вместо него указать список полей, например показать имена и города всех пользователей:

```sql
SELECT "name", "city" FROM users;
```

---

Перед совершением SELECT запросов, заполним таблицу записями с пользователями:

```python
from sqlite3 import connect

with connect('database.db') as connection:
    cur = connection.cursor()
    cur.execute('''CREATE TABLE IF NOT EXISTS "users" (
    "name"  TEXT,
    "age"   INTEGER,
    "city"  TEXT
    );''')
    # создаем список кортежей с данными для добавления
    users_data = [('Анна', 25, 'Москва'),
                  ('Иван', 30, 'Санкт-Петербург'),
                  ('Мария', 22, 'Москва'),
                  ('Алексей', 35, 'Новосибирск'),
                  ('Дмитрий', 28, 'Казань'),
                  ('Ксения', 29, 'Москва'),
                  ('Андрей', 32, 'Санкт-Петербург'),
                  ('Елена', 24, 'Краснодар'),
                  ('Николай', 27, 'Москва'),
                  ('Светлана', 31, 'Казань'),
                  ('Петр', 26, 'Новосибирск'),
                  ('Ольга', 33, 'Санкт-Петербург'),
                  ('Сергей', 23, 'Краснодар'),
                  ('Алена', 34, 'Москва'),
                  ('Артем', 29, 'Казань')]

    # используем метод executemany для добавления данных в таблицу
    cur.executemany('INSERT INTO users (name, age, city) VALUES (?, ?, ?)', users_data)

    # подтверждаем изменения в базе данных
    connection.commit()
```

## Получение всех записей. Методы fetchall(), fetchone(), fetchmany()

Выполним запрос, описанный выше и при помощи метода `fetchall()` поместим все полученные записи в переменную `result`. В результате мы получим список из записей в виде отдельных кортежей:

```python
from sqlite3 import connect

with connect('database.db') as connection:
    cur = connection.cursor()
    cur.execute('SELECT * FROM users;')
    result = cur.fetchall()
    print(result)
```

Метод `fetchall()` имеет критический недостаток: если запрос вернет огромное количество полей, это может привести к нехватке памяти и замедлению работы программы. Хорошей практикой считается использование метода `fetchone()` в цикле:

```python
from sqlite3 import connect

with connect('database.db') as connection:
    cur = connection.cursor()
    cur.execute('SELECT * FROM users;')
    result = cur.fetchone()
    while result is not None:
        print(result)
        result = cur.fetchone()
```

Также в качестве альтернативы можно использовать метод `fetchmany()`:

```python
from sqlite3 import connect

with connect('database.db') as connection:
    cur = connection.cursor()
    cur.execute('SELECT * FROM users;')
    results = cur.fetchmany(2)
    while results:
        for result in results:
            print(result)
        results = cur.fetchmany(2)
```

Этот код выполняет запрос `SELECT * FROM users;` и извлекает из результата две строки данных за раз, используя метод `fetchmany(2)`. Затем он выводит полученные строки данных на экран с помощью цикла for. После этого он снова вызывает метод `fetchmany(2)`, чтобы извлечь следующие две строки данных, и так продолжается, пока не будут извлечены все строки данных из результата запроса.

> Если SELECT-запрос не находит ни одной строки, то результатом запроса будет пустой набор строк. Это означает, что методы `fetchone()`, `fetchmany()` и `fetchall()` будут возвращать `None`, `[]` и `[]` соответственно.

## SELECT вместе с WHERE

Оператор `WHERE`, который мы ранее использовали вместе с `DELETE` и `UPDATE`, можно также использовать вместе с `INSERT`. Вот несколько примеров их совместного использования:

1. Выбор всех строк, где значение поля `city` равно "Москва":

    ```python
    from sqlite3 import connect

    with connect('database.db') as connection:
        cur = connection.cursor()
        cur.execute('SELECT * FROM users WHERE city = "Москва";')
        result = cur.fetchall()
        print(result)
    ```

2. Выбор всех строк, где значение поля `age` больше 30:

    ```python
    from sqlite3 import connect

    with connect('database.db') as connection:
        cur = connection.cursor()
        cur.execute('SELECT * FROM users WHERE age > 30;')
        result = cur.fetchall()
        print(result)
    ```

3. Выбор всех строк, где значение поля `name` начинается с буквы "А":

    ```python
    from sqlite3 import connect

    with connect('database.db') as connection:
        cur = connection.cursor()
        cur.execute('SELECT * FROM users WHERE age > 30;')
        result = cur.fetchall()
        print(result)
    ```

4. Выбор всех строк, где значение поля `name` содержит букву "н":

    ```python
    from sqlite3 import connect

    with connect('database.db') as connection:
        cur = connection.cursor()
        cur.execute('SELECT * FROM users WHERE name LIKE "%н%";')
        result = cur.fetchall()
        print(result)
    ```

5. Выбор всех строк, где значение поля `city` не равно "Санкт-Петербург":

    ```python
    from sqlite3 import connect

    with connect('database.db') as connection:
        cur = connection.cursor()
        cur.execute('SELECT * FROM users WHERE city != "Санкт-Петербург";')
        result = cur.fetchall()
        print(result)
    ```

6. Выбрать всех пользователей младше 30 лет, проживающих в городе "Москва":

    ```python
    from sqlite3 import connect

    with connect('database.db') as connection:
        cur = connection.cursor()
        cur.execute("SELECT * FROM users WHERE age < 30 AND city = 'Москва'")
        results = cur.fetchall()
        print(results)
    ```

7. Выбрать всех пользователей старше 30 лет, проживающих в городе "Москва" или "Санкт-Петербург":

    ```python
    from sqlite3 import connect

    with connect('database.db') as connection:
        cur = connection.cursor()
        cur.execute("SELECT * FROM users WHERE age > 30 AND (city = 'Москва' OR city = 'Санкт-Петербург')")
        results = cur.fetchall()
        print(results)
    ```

8. Выбрать всех пользователей проживающих в городе "Новосибирск" или "Казань", и младше 30 лет:

    ```python
    from sqlite3 import connect

    with connect('database.db') as connection:
        cur = connection.cursor()
        cur.execute("SELECT * FROM users WHERE city IN ('Новосибирск', 'Казань') AND age < 30")
        results = cur.fetchall()
        print(results)
    ```