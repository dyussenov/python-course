
# Функции агрегации группировка и сортировка
 
## Функции агрегации

В SQLite функции агрегации (aggregation functions) - это функции, которые используются для вычисления агрегированных значений в результате выполнения запроса. Эти функции позволяют вычислять суммы, средние значения, максимальные и минимальные значения, количество строк и т.д. для группировки строк в результирующем наборе. Такие функции обычно используются в операторе `SELECT`.

Список функций агрегации:

1. `COUNT()` - Возвращает количество строк в таблице:

    ```python
    from sqlite3 import connect

    with connect('database.db') as connection:
        cur = connection.cursor()
        cur.execute('SELECT COUNT(*) FROM users;')
        result = cur.fetchone()
        print(result)
    ```

2. `SUM()` - Возвращает сумму значений по указанному столбцу:

    ```python
    from sqlite3 import connect

    with connect('database.db') as connection:
        cur = connection.cursor()
        cur.execute('SELECT SUM(age) FROM users;')
        result = cur.fetchone()
        print(result)
    ```

3. `MIN()` - Возвращает минимальное значение по указанному столбцу:

    ```python
    from sqlite3 import connect

    with connect('database.db') as connection:
        cur = connection.cursor()
        cur.execute('SELECT MIN(age) FROM users;')
        result = cur.fetchone()
        print(result)
    ```

4. `MAX()` - Возвращает максимальное значение по указанному столбцу:

    ```python
    from sqlite3 import connect

    with connect('database.db') as connection:
        cur = connection.cursor()
        cur.execute('SELECT MAX(age) FROM users;')
        result = cur.fetchone()
        print(result)
    ```

5. `AVG()` - Возвращает среднее значение по указанному столбцу:

    ```python
    from sqlite3 import connect

    with connect('database.db') as connection:
        cur = connection.cursor()
        cur.execute('SELECT AVG(age) FROM users;')
        result = cur.fetchone()
        print(result)
    ```

---

Кроме того, SQLite поддерживает две специальные функции:

- `GROUP_CONCAT` - возвращает строку, составленную из всех значений в группе, объединенных в одну строку с разделителем.

- `TOTAL` - возвращает сумму всех значений в столбце, включая NULL значения.

> Функции агрегации могут быть довольно ресурсоемкими, особенно при работе с большими объемами данных. Поэтому необходимо аккуратно использовать их, чтобы избежать проблем с производительностью.

> При использовании функций агрегации с большим объемом данных, может потребоваться оптимизировать производительность базы данных и запросов, например, путем создания индексов на соответствующих столбцах или разделения данных на несколько таблиц.

## Группировка. Оператор GROUP BY

Группировка в SQL - это процесс объединения нескольких строк в одну строку на основе значений одного или нескольких столбцов. Группировка позволяет агрегировать данные по определенным категориям и вычислять значения агрегатных функций для каждой группы.

Примеры использования группировки вместе с агрегирующими функциями:

1. Подсчет количества пользователей в каждом городе:

    ```python
    from sqlite3 import connect

    with connect('database.db') as connection:
        cur = connection.cursor()
        cur.execute('SELECT city, COUNT(*) FROM users GROUP BY city;')
        result = cur.fetchall()
        print(result)
    ```

2. Нахождение среднего возраста пользователей в каждом городе:

    ```python
    from sqlite3 import connect

    with connect('database.db') as connection:
        cur = connection.cursor()
        cur.execute('SELECT city, AVG(age) FROM users GROUP BY city;')
        result = cur.fetchall()
        print(result)
    ```

3. Нахождение максимального возраста пользователя в каждом городе:

    ```python
    from sqlite3 import connect

    with connect('database.db') as connection:
        cur = connection.cursor()
        cur.execute('SELECT city, MAX(age) FROM users GROUP BY city;')
        result = cur.fetchall()
        print(result)
    ```

4. Нахождение минимального возраста пользователя в каждом городе:

    ```python
    from sqlite3 import connect

    with connect('database.db') as connection:
        cur = connection.cursor()
        cur.execute('SELECT city, MIN(age) FROM users GROUP BY city;')
        result = cur.fetchall()
        print(result)
    ```   

5. Нахождение общего возраста пользователей в каждом городе:

    ```python
    from sqlite3 import connect

    with connect('database.db') as connection:
        cur = connection.cursor()
        cur.execute('SELECT city, SUM(age) FROM users GROUP BY city;')
        result = cur.fetchall()
        print(result)
    ```

6. Нахождение количества пользователей с возрастом больше 30 лет в каждом городе:

    ```python
    from sqlite3 import connect

    with connect('database.db') as connection:
        cur = connection.cursor()
        cur.execute('SELECT city, COUNT(*) FROM users WHERE age > 30 GROUP BY city;')
        result = cur.fetchall()
        print(result)
    ```

## Сортировка. Оператор ORDER BY

Сортировка в SQL позволяет упорядочить результаты запроса по одному или нескольким полям в порядке возрастания или убывания значений. Для сортировки используется ключевое слово ORDER BY с указанием полей и порядка сортировки:

- Сортировка по возрастанию поля age:

    ```sql
    SELECT * FROM users ORDER BY age;
    ```

- Сортировка по убыванию поля name:

    ```sql
    SELECT * FROM users ORDER BY name DESC;
    ```
    
- Сортировка сначала по полю city по возрастанию, затем по полю age по убыванию:

    ```sql
    SELECT * FROM users ORDER BY city, age DESC;
    ```

В Python сортировку можно выполнить аналогично предыдущим примерам, используя метод `execute()` объекта `cursor`:

```python

from sqlite3 import connect

with connect('database.db') as connection:
    cur = connection.cursor()

    # сортировка по возрастанию поля age
    cur.execute('SELECT * FROM users ORDER BY age;')
    result = cur.fetchall()
    print(result)

    # сортировка по убыванию поля name
    cur.execute('SELECT * FROM users ORDER BY name DESC;')
    result = cur.fetchall()
    print(result)

    # сортировка по полю city по возрастанию, затем по полю age по убыванию
    cur.execute('SELECT * FROM users ORDER BY city, age DESC;')
    result = cur.fetchall()
    print(result)
```