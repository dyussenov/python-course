
# Внешние ключи и объединение таблиц

## Внешние ключи. Установка связей между таблицами.

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
```