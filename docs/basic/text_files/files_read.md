
# Открытие и чтение текстовых файлов

## О текстовых файлах

Большие объемы данных хранят не в списках или словарях, а в файлах и базах данных. В этом уроке изучим особенности работы с текстовыми файлами в Python. Файл — это всего лишь набор данных, сохраненный в виде последовательности битов на компьютере.

В Python существует два типа файлов:

- Текстовые
- Бинарные

### Текстовые файлы

Это файлы с человекочитаемым содержимым. В них хранятся последовательности символов, которые понимает человек. Блокнот и другие стандартные редакторы умеют читать и редактировать этот тип файлов.

Текст может храниться в двух форматах: ```.txt``` — простой текст и ```.rtf``` формат обогащенного текста.

### Бинарные файлы

В бинарных файлах данные отображаются в закодированной форме (с использованием только нулей (0) и единиц (1) вместо простых символов). В большинстве случаев это просто последовательности битов.

Они хранятся в формате ```.bin```.

## Открытие файла. Функция ```open()```

В Python есть встроенная функция ```open()```. С ее помощью можно открыть любой файл на компьютере. Технически Python создает на его основе объект:

```Python
f = open(file_name, access_mode, encoding=None)
```

Где,

- ```file_name``` - имя открываемого файла
- ```access_mode``` - режим открытия файла. Он может быть: для чтения, записи и т. д. По умолчанию используется режим чтения (r), если другое не указано. Далее полный список режимов открытия файла
- ```encoding``` - кодировка, используется для корректного чтения файлов не на латинице. Рекомендуется использовать значения ```'utf-8'``` или ```cp1252```.

| Режим |                                                            Описание                                                            |   |
|-------|------------------------------------------------------------------------------------------------------------------------------|---|
| ```r```     | Только для чтения.                                                                                                             |   |
| ```w```     | Только для записи. Создаст новый файл, если не найдет с указанным именем.                                                      |   |
| ```rb```    | Только для чтения (бинарный).                                                                                                  |   |
| ```wb```    | Только для записи (бинарный). Создаст новый файл, если не найдет с указанным именем.                                           |   |
| ```r+```    | Для чтения и записи.                                                                                                           |   |
| ```rb+```   | Для чтения и записи (бинарный).                                                                                                |   |
| ```w+```    | Для чтения и записи. Создаст новый файл для записи, если не найдет с указанным именем.                                         |   |
| ```wb+```   | Для чтения и записи (бинарный). Создаст новый файл для записи, если не найдет с указанным именем.                              |   |
| ```a```     | Откроет для добавления нового содержимого. Создаст новый файл для записи, если не найдет с указанным именем.                   |   |
| ```a+```    | Откроет для добавления нового содержимого. Создаст новый файл для чтения записи, если не найдет с указанным именем.            |   |
| ```ab```    | Откроет для добавления нового содержимого (бинарный). Создаст новый файл для записи, если не найдет с указанным именем.        |   |
| ```ab+```   | Откроет для добавления нового содержимого (бинарный). Создаст новый файл для чтения записи, если не найдет с указанным именем. |   |

---

Создадим текстовый файл ```example.txt``` и сохраним его в рабочей директории:

!['txt'](/assets/txt.jpg)


Следующий код используется для его открытия:

```Python
f = open('example.txt','r', encoding='utf-8')
```

## Методы ```read()```, ```seek()``` и ```tell()```. Посимвольное чтение

Для того, чтобы считать последовательность отдельных символов используется метод ```read()```. В качестве аргумента передается количество символок, которые нужно считать:

```Python
f = open('example.txt','r', encoding='utf-8')
print(f.read(4)) #выводим первые 4 символа
```

Вывод:

```text
This
```

---

При использовании метода ```read()``` виртуальный "курсор" перемещается на заданное количество символов (в нашем случае на 4). Благодаря этому мы можем повторно вызвать этот метод и получить следующие сиволы:

```Python
f = open('file.txt', 'r', encoding='utf-8')
print(f.read(4))
print(f.read(4))
```

Вывод:

```text
This
 is 
```

> В данном примере при втором вызове функции ```print()``` мы получили только слово ```is```, хотя считывали 4 символа. Никакой ошибки нет, так как слово окружено пробелами, которые считаются полноценными символами.

---

Мы также можем узнать текущее положение виртуального курсора при помощи метода ```tell()``` и переместить вирутальный курсор в любое место при помощи ```seek()```:

```Python
f = open('file.txt', 'r', encoding='utf-8')
print(f.read(4)) #выводим первые 4 сивола

f.seek(10) #перемещаем курсор на 10 позицию
pos = f.tell() #сохраняем в переменную текущую позицию
print('Позиция курсора:', pos)

print(f.read(4)) #считываем следующие 4 символа начиная с 10
pos = f.tell() #сохраняем в переменную новую позицию
print('Позиция курсора:', pos)
```

Вывод:

```text
This
Позиция курсора 10
text
Позиция курсора 14
```

## Методы ```readline()``` и ```readlines()```. Построчное чтение

Считывать файл посимвольно не всегда удобно. Во многих задачах более уместным будет чтение файла *построчно*. Для примера обновим содержимое нашего текстового файла:

```Text
Не выходи из комнаты, не совершай ошибку.
Зачем тебе Солнце, если ты куришь Шипку?
За дверью бессмысленно всё, особенно — возглас счастья.
Только в уборную — и сразу же возвращайся.

О, не выходи из комнаты, не вызывай мотора.
Потому что пространство сделано из коридора
и кончается счетчиком. А если войдет живая
милка, пасть разевая, выгони не раздевая.

Иосиф Бродский
```

---

При помощи ```readline()``` считаем первою строку текстового файла:

```Python
f = open('file.txt', 'r', encoding='utf-8')
print(f.readline())
```

Вывод:

```text
Не выходи из комнаты, не совершай ошибку.
```

---

Интересная деталь: символы на кирилице занимают 2 бита, а не 1. Поэтому вызвав метод ```tell()``` мы получим значение гораздо больше, чем количество сиволов в прочитанной строке:

```Python
f = open('file.txt', 'r', encoding='utf-8')
print(f.readline())
print(f.tell())
```

Вывод:

```text
Не выходи из комнаты, не совершай ошибку.

75
```

---

Пример с использованием двух методов ```readline()```:

```Python
f = open('file.txt', 'r', encoding='utf-8')
print(f.readline())
print(f.readline())
```

Вывод:

```text
Не выходи из комнаты, не совершай ошибку.

Зачем тебе Солнце, если ты куришь Шипку?


```

> Лишние пустые строки - не баг, а фича. Метод ```readline()``` определяет конец строки по спецсимволу ```\n``` (переход на новую строку). Так как этот спецсимвол является частю прочитанной строки - он выводится вместе с ней. А также спецсимвол ```\n``` по умолчанию используется в функции ```print()```. Чтобы избежать лишних пустых строк нужно вызывать функцию ```print()``` с аргументом ```end=''```

---

Объект ```f``` является итерируемым. Поэтому весь файл можно построчно вывести при помощи цикла ```for```:

```python
f = open('file.txt', 'r', encoding='utf-8')
for line in f:
    print(line, end='')
```

Вывод:

```text
Не выходи из комнаты, не совершай ошибку.
Зачем тебе Солнце, если ты куришь Шипку?
За дверью бессмысленно всё, особенно — возглас счастья.
Только в уборную — и сразу же возвращайся.

О, не выходи из комнаты, не вызывай мотора.
Потому что пространство сделано из коридора
и кончается счетчиком. А если войдет живая
милка, пасть разевая, выгони не раздевая.

Иосиф Бродский
```

---

И наконец, если мы хотим прочитать весь файл целиком и сразу можно воспользоваться методом ```readlines()```. Он возвращает список, в котором каждый элемент это отдельная строка из файла:

```Python
f = open('file.txt', 'r', encoding='utf-8')
lines = f.readlines()
print(lines)
```

Вывод:

```text
['Не выходи из комнаты, не совершай ошибку.\n', 'Зачем тебе Солнце, если ты куришь Шипку?\n', 'За дверью бессмысленно всё, особенно — возглас счастья.\n', 'Только в уборную — и сразу же возвращайся.\n', '\n', 'О, не выходи из комнаты, не вызывай мотора.\n', 'Потому что пространство сделано из коридора\n', 'и кончается счетчиком. А если войдет живая\n', 'милка, пасть разевая, выгони не раздевая.\n', '\n', 'Иосиф Бродский']
```

> Следует быть аккуратным при использовании данного метода. Открытие больших файлов занимает большой объем оперативной памяти. Например, при открытии целой книги "Война и мир" вы можете потратить неприлично огромные 4мб памяти.

---

И, напоследок, наш файл после открытия и работы с ним необходимо закрыть, чтобы освободить занятую память. Правильный вариант предыдущего скрипта:

```Python
f = open('file.txt', 'r', encoding='utf-8')
lines = f.readlines()
print(lines)
f.close()
```