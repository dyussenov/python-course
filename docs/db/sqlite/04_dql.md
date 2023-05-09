
# SELECT. Получение записей

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

