
# Внешние ключи и объединение таблиц

## Внешние ключи. Установка связей между таблицами.

Создадим две таблицы - `users` и `orders`. Обратите внимание, что у обеих таблиц указано поле `id` - уникальный идентификатор. Также у него стоит свойство `AUTOINCREMENT` - оно автоматически выставляет новое значение, которое больше на единицу, чем значение в предыдущей записи. Это избавляет нас от необходимости вручную выставлять `id` для каждой новой записи.

```python
import sqlite3

with sqlite3.connect("mydatabase.db") as conn:
    cursor = conn.cursor()

    # Создание таблицы пользователей
    cursor.execute("""CREATE TABLE users (
                            id INTEGER PRIMARY KEY AUTOINCREMENT,
                            name TEXT NOT NULL,
                            age INTEGER,
                            city TEXT
                            );""")

    # Создание таблицы заказов
    cursor.execute("""CREATE TABLE orders (
                            id INTEGER PRIMARY KEY AUTOINCREMENT,
                            user_id INTEGER,
                            product TEXT NOT NULL,
                            price REAL,
                            FOREIGN KEY(user_id) REFERENCES users(id)
                            );""")

    cursor.execute("INSERT INTO users (name, age, city) VALUES ('John Doe', 25, 'New York')")
    cursor.execute("INSERT INTO users (name, age, city) VALUES ('Jane Smith', 30, 'Los Angeles')")
    cursor.execute("INSERT INTO users (name, age, city) VALUES ('Bob Johnson', 40, 'Chicago')")

    # Добавление заказов
    cursor.execute("INSERT INTO orders (user_id, product, price) VALUES (1, 'Shoes', 49.99)")
    cursor.execute("INSERT INTO orders (user_id, product, price) VALUES (2, 'T-shirt', 19.99)")
    cursor.execute("INSERT INTO orders (user_id, product, price) VALUES (3, 'Hat', 9.99)")

    conn.commit()
```

Также в таблице orders присутствует новое для вас поле - `FOREIGN KEY(user_id) REFERENCES users(id)`, которое используется для создания связи (внешнего ключа) между двумя таблицами. Он указывает, что значения в столбце текущей таблицы (в данном случае `user_id` в таблице orders) должны быть ссылками на значения в столбце другой таблицы (в данном случае `id` в таблице `users`).

В приведенном примере, выражение `FOREIGN KEY(user_id) REFERENCES users(id)` говорит о следующем:

- `user_id` - это столбец в таблице orders, который будет содержать ссылки на значения столбца id в таблице users.
    
- `users(id)` - указывает, что связь будет установлена с таблицей `users` и столбцом `id` в ней.

Таким образом, поле `user_id` в таблице `orders` будет содержать только те значения, которые существуют в столбце `id` таблицы `users`. Если вы попытаетесь вставить значение в поле `user_id`, которого нет в столбце `id` таблицы `users`, будет сгенерировано исключение или ошибка, в зависимости от настроек базы данных.

В результате использования внешнего ключа, вы можете создавать связи между таблицами и выполнять операции, такие как обновление, удаление или выборка данных из связанных таблиц, чтобы получить целостность и связность данных.

## Оператор JOIN

Оператор `JOIN` в SQL используется для объединения данных из двух или более таблиц на основе совпадающих значений в указанных столбцах. Он позволяет комбинировать строки из разных таблиц в один результат, основываясь на заданных условиях соединения.

Оператор `JOIN` имеет различные типы, включая:

- `INNER JOIN`: Возвращает только те строки, для которых есть совпадение в обеих таблицах. Он объединяет строки, основываясь на совпадении значений в указанных столбцах.

- `LEFT JOIN` (или `LEFT OUTER JOIN`): Возвращает все строки из левой (первой) таблицы и соответствующие строки из правой (второй) таблицы. Если в правой таблице нет совпадающих строк, возвращается NULL.

- `RIGHT JOIN` (или `RIGHT OUTER JOIN`): Возвращает все строки из правой (второй) таблицы и соответствующие строки из левой (первой) таблицы. Если в левой таблице нет совпадающих строк, возвращается `NULL`.

- `FULL JOIN` (или `FULL OUTER JOIN`): Возвращает все строки из обеих таблиц и соответствующие строки по условию совпадения. Если в одной из таблиц нет совпадающих строк, возвращается `NULL`.

- `CROSS JOIN`: Производит комбинаторное объединение всех строк из двух таблиц. В результате получается декартово произведение строк из обеих таблиц.

Оператор `JOIN` обычно используется с условием соединения, указывающим, по каким столбцам нужно сравнивать значения для объединения строк. Обычно используются операторы сравнения, такие как "=", ">", "<", "!=" и другие.

---

```python
from sqlite3 import connect

with connect('database.db') as connection:
    cur = connection.cursor()
    cur.execute('SELECT * FROM users JOIN orders ON users.id = orders.user_id')
    result = cur.fetchall()
    print(result)
```


- INNER JOIN:

    ```python
    from sqlite3 import connect

    with connect('database.db') as connection:
        cur = connection.cursor()
        cur.execute('SELECT * FROM users INNER JOIN orders ON users.id = orders.user_id')
        result = cur.fetchall()
        print(result)
    ```

    Результат INNER JOIN:

    ```python
    [(1, 'John', 30, 'New York', 1, 1, 'Shoes'),  (1, 'John', 30, 'New York', 2, 1, 'T-shirt'),  (2, 'Jane', 25, 'Los Angeles', 3, 2, 'Hat'),  (3, 'Bob', 35, 'Chicago', 4, 3, 'Pants')]

    ```

    
- LEFT JOIN:

    ```python
    from sqlite3 import connect

    with connect('database.db') as connection:
        cur = connection.cursor()
        cur.execute('SELECT * FROM users LEFT JOIN orders ON users.id = orders.user_id')
        result = cur.fetchall()
        print(result)
    ```

    Результат LEFT JOIN:

    ```python
    [(1, 'John', 30, 'New York', 1, 1, 'Shoes'),  (1, 'John', 30, 'New York', 2, 1, 'T-shirt'),  (2, 'Jane', 25, 'Los Angeles', 3, 2, 'Hat'),  (3, 'Bob', 35, 'Chicago', 4, 3, 'Pants')]
    ```

- RIGHT JOIN:

    ```python
    from sqlite3 import connect

    with connect('database.db') as connection:
        cur = connection.cursor()
        cur.execute('SELECT * FROM users RIGHT JOIN orders ON users.id = orders.user_id')
        result = cur.fetchall()
        print(result)
    ```


- Результат RIGHT JOIN:


    ```python
    [(1, 'John', 30, 'New York', 1, 1, 'Shoes'),  (1, 'John', 30, 'New York', 2, 1, 'T-shirt'),  (2, 'Jane', 25, 'Los Angeles', 3, 2, 'Hat'),  (3, 'Bob', 35, 'Chicago', 4, 3, 'Pants')]
    ```

    FULL JOIN:

    ```python
    from sqlite3 import connect

    with connect('database.db') as connection:
        cur = connection.cursor()
        cur.execute('SELECT * FROM users FULL JOIN orders ON users.id = orders.user_id')
        result = cur.fetchall()
        print(result)
    ```

    Результат FULL JOIN:

    ```python
    (1, 'John', 30, 'New York', 1, 1, 'Shoes')
    ```