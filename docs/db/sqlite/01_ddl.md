
# DDL запросы. Создание, изменение, удаление таблиц

## Создание таблиц

В предыдущей вводной статье рассматривалась следующая таблица:

| Имя    | Возраст | Город       |
| ------ | -------| -----------|
| Анна   | 25     | Москва     |
| Иван   | 30     | Санкт-Петербург |
| Катя   | 21     | Новосибирск |
| Петр   | 28     | Екатеринбург  |

---

Давайте для начала попробуем создать такую же в приложении SQLite DB Browser. Для этого нажмите кнопку "Новая база данных" и в открывшемся окне добавьте необходимые поля нажимая кнопку "add":

![создание таблицы в sqlite db browser](/assets/sqlite/dll/table-creation.png)

Обратите внимание на SQL запрос, составляемый в ходе определения полей создаваемой таблицы. Именно этот запрос выполнится при нажатии кнопки "ОК". Это зарпос `CREATE`. Он же является первым запросом из семейства DLL.

```sql
CREATE TABLE "users" (
	"name"	TEXT,
	"age"	INTEGER,
	"city"	TEXT
);
```

Давайте подробно рассмотрим структуру данного запроса:

- `CREATE TABLE "users"` - Создать таблицу с названием "users".

- `"name" TEXT, ` - Создать поле "name" с типом данных "TEXT".

- `"age" INTEGER, ` - Создать поле "age" с типом данных "INTEGER".

- `"city" TEXT, ` - Создать поле "city" с типом данных "TEXT".

> Обратите внимание, что в указываемя поля группируются скобками, названия полей заключаются в кавычки и в конце зарпоса стоит точка с зяпятой.

---

Теперь во вкладке "данные" мы можем наблюдать созданную таблицу и сохраненные записи (вы можете их добавить вручную)

![созданная таблица в sqlite db browser](/assets/sqlite/dll/created-table.png)

## Изменение таблиц

Следующий запрос в семействе DLL - `ALTER TABLE`. Он позволяет изменить название таблицы, добавить новые поля или отредактировать старые.

> Изменение структуры базы данных крайне опасный процесс. При изменении "боевой" базы (БД, используемая в запущенном проекте) рекомендую 10 раз перепроверить наличие бэкапа и 10 раз подумать над вашим запросом. Помните - хуже потерянных данных только данные слитые в интернет.

Давайте начнем с переименования таблицы. Переключимся во вкладку "SQL" и напишем следующий запрос:

```SQL
ALTER TABLE users
RENAME TO customers;
```

---

Добавим новое поле:

```SQL
ALTER TABLE customers
ADD COLUMN "email" TEXT;
```

---

Переименуем старое поле:

```SQL
ALTER TABLE customers
RENAME COLUMN "name" TO "first_name";
```

---

Удалим еще одно старое поле:

```SQL
ALTER TABLE customers
DROP COLUMN city;
```


В итоге наша отредактированная таблица выглядит так:

![промежуточный резулбтат](/assets/sqlite/dll/result1.png)

> Обратите внимание на то, что после добавления поля `email` у старой записи выставляется значение `Null`

## Очистка таблицы

В "каноничном" SQL существует оператор `TRUNCATE`, который полностью удаляет все записи в таблице. Но в SQLite он отсутствует. Вместо этого можно использовать оператор `DELETE` из семейства DML:

```SQL
DELETE FROM customers;
```

> Примечательным является то, что `DELETE` является транзакционным запросом, следовательно удаленные данные можно вернуть

## Удаление таблицы

Удалить таблицу можно при помощи оператора `DROP`:

```SQL
DROP TABLE customers;
```

После этой операции таблица со всеми её данными удаляется. Также удалить можно и всю базу данных:

![sql injection](/assets/sqlite/dll/dropdatabase.jpg)

> SQLite является встраиваемой базой данных, что означает - база хранится в качестве файла на жестком диске, в отличие от, например, Postgresql, который "висит" на отдельном серверном процессе. Поэтому база удаляется как обычный файл - перемещением в корзину, а не SQL запросом