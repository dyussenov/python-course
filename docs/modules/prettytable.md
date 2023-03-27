
# Модуль Prettytable  в Python

## Установка Prettytable

Модуль prettytable - полезен при создании простых таблиц и вывода их в терминал или текстовый файл. Он был вдохновлен таблицами ASCII, используемыми в оболочке PostgreSQL.

Возможности модуля prettytable:

- установка ширины заполнения столбца, выравнивание текста или граница таблицы

- сортировка данных

- выбор отображения столбцов и строк в окончательном выводе

- чтение данных из CSV, HTML или курсора базы данных

- вывод данных в ASCII или HTML

Уставновка:

```bash
pip install prettytable
```

## Создание таблицы и добавление данных

Для начала, необходимо создать экземпляр класса ```PrettyTable()```:

```Python
from prettytable import PrettyTable

# создание экземпляра
mytable = PrettyTable()
```

Можно добавлять данные по одной строке за раз. Для этого необходимо сначала установить имена полей, используя атрибут ```PrettyTable.field_names```, а затем добавлять строки по одной, используя метод ```PrettyTable.add_row()```:

```Python
from prettytable import PrettyTable
mytable = PrettyTable()

# имена полей таблицы
mytable.field_names = ["City name", "Area", "Population", "Annual Rainfall"]

# добавление данных по одной строке за раз
mytable.add_row(["Adelaide", 1295, 1158259, 600.5])
mytable.add_row(["Brisbane", 5905, 1857594, 1146.4])
mytable.add_row(["Darwin", 112, 120900, 1714.7])
mytable.add_row(["Hobart", 1357, 205556, 619.5])
mytable.add_row(["Sydney", 2058, 4336374, 1214.8])
mytable.add_row(["Melbourne", 1566, 3806092, 646.9])
mytable.add_row(["Perth", 5386, 1554769, 869.4])

# вывод таблицы в терминал
print(mytable)
```

Вывод:

```text
+-----------+------+------------+-----------------+
| City name | Area | Population | Annual Rainfall |
+-----------+------+------------+-----------------+
|  Adelaide | 1295 |  1158259   |      600.5      |
|  Brisbane | 5905 |  1857594   |      1146.4     |
|   Darwin  | 112  |   120900   |      1714.7     |
|   Hobart  | 1357 |   205556   |      619.5      |
|   Sydney  | 2058 |  4336374   |      1214.8     |
| Melbourne | 1566 |  3806092   |      646.9      |
|   Perth   | 5386 |  1554769   |      869.4      |
+-----------+------+------------+-----------------+
```

Когда есть список строк, то можно добавить их за один раз с помощью метода ```PrettyTable.add_rows()```:

```Python
from prettytable import PrettyTable
mytable = PrettyTable()

# имена полей таблицы
mytable.field_names = ["City name", "Area", "Population", "Annual Rainfall"]

# добавление списка строк
mytable.add_rows(
    [
        ["Adelaide", 1295, 1158259, 600.5],
        ["Brisbane", 5905, 1857594, 1146.4],
        ["Darwin", 112, 120900, 1714.7],
        ["Hobart", 1357, 205556, 619.5],
        ["Sydney", 2058, 4336374, 1214.8],
        ["Melbourne", 1566, 3806092, 646.9],
        ["Perth", 5386, 1554769, 869.4],
    ]
)
print(mytable)
```

Также можно добавлять данные по одному столбцу за раз. Для этого необходимо использовать метод ```PrettyTable.add_column()```, который принимает два аргумента - строку, которая является именем поля таблицы добавляемого столбца, и список или кортеж, содержащий данные столбца:

```Python
from prettytable import PrettyTable
mytable = PrettyTable()

# Добавление колонки таблицы с именем 'City name'
mytable.add_column("City name",
["Adelaide", "Brisbane", "Darwin", "Hobart", "Sydney", "Melbourne", "Perth"])

# Добавление колонки таблицы с именем 'Area'
mytable.add_column("Area", [1295, 5905, 112, 1357, 2058, 1566, 5386])

# Добавление колонки таблицы с именем 'Population'
mytable.add_column("Population", 
[1158259, 1857594, 120900, 205556, 4336374, 3806092, 1554769])

# Добавление колонки таблицы с именем 'Annual Rainfall'
mytable.add_column("Annual Rainfall", [600.5, 1146.4, 1714.7, 619.5, 1214.8, 646.9, 869.4])

print(mytable)
```

Если данные таблицы хранятся в базе данных, к которой можно получить доступ с помощью модуля имеющего Python DB-API (например, база данных SQLite, доступная с помощью модуля ```sqlite3```), то можно создать экземпляр ```PrettyTable()``` с данными, используя объект курсора, например:

```Python
import sqlite3
from prettytable import from_db_cursor

connection = sqlite3.connect("mydb.db")
cursor = connection.cursor()
cursor.execute("SELECT field1, field2, field3 FROM my_table")

# создание таблицы из объекта курсора
mytable = from_db_cursor(cursor)
print(mytable)
```
