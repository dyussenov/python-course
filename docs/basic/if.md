
# If else elif

---

## Введение

![Image title](/assets/image.png)

Конструкция if-elif-else в Python используется для выполнения различных действий в зависимости от выполнения условий. Эта конструкция представляет собой цепочку условных выражений, в которых каждое последующее условие проверяется только в том случае, если все предыдущие условия ложные. Давайте рассмотрим эту конструкцию на примерах.

---

## Пример 1: Определение знака числа

В этом примере мы используем конструкцию if-elif-else для определения знака числа, введенного пользователем:

```python
number = int(input("Введите число: "))

if number > 0:
    print("Число положительное")
elif number == 0:
    print("Число равно нулю")
else:
    print("Число отрицательное")

```

В этом примере мы сначала принимаем число от пользователя с помощью функции ```input()```, а затем преобразуем его в целочисленное значение с помощью функции ```int()```. Затем мы используем конструкцию ```if-elif-else``` для проверки условия и выводим соответствующее сообщение в зависимости от результата.

---

## Пример 2: Проверка длины строки

В этом примере мы используем конструкцию ```if-elif-else``` для проверки длины строки:

```python
string = input("Введите строку: ")

if len(string) < 5:
    print("Строка слишком короткая")
elif len(string) > 10:
    print("Строка слишком длинная")
else:
    print("Длина строки подходит")
```

В этом примере мы сначала принимаем строку от пользователя с помощью функции ```input()```. Затем мы используем конструкцию ```if-elif-else``` для проверки длины строки и выводим соответствующее сообщение в зависимости от результата.

---

## Пример 3: Проверка на четность

В этом примере мы используем конструкцию if-elif-else для проверки на четность числа:

```python
number = int(input("Введите число: "))

if number % 2 == 0:
    print("Число четное")
else:
    print("Число нечетное")
```

Здесь мы сначала принимаем число от пользователя с помощью функции input(), а затем преобразуем его в целочисленное значение с помощью функции int(). Затем мы используем конструкцию ```if-elif-else``` для проверки на четность и выводим соответствующее сообщение в зависимости от результата.

---

## Пример 4: Вложенные условия

Дополнительно к конструкции if-elif-else, в Python также можно использовать вложенные условия. Вложенные условия представляют собой конструкции if-elif-else, которые находятся внутри других условных выражений. Вот пример использования вложенных условий:

```python

number = int(input("Введите число: "))

if number > 0:
    print("Число положительное")
    if number % 2 == 0:
        print("Число четное")
    else:
        print("Число нечетное")
elif number == 0:
    print("Число равно нулю")
else:
    print("Число отрицательное")
    if abs(number) % 2 == 0:
        print("Абсолютное значение числа четное")
    else:
        print("Абсолютное значение числа нечетное")
```

В этом примере мы использовали вложенные условия для определения как знака, так и четности числа. При первом условии, если число больше нуля, то мы выводим "Число положительное" и проверяем его на четность. При втором условии, если число меньше нуля, то мы выводим "Число отрицательное" и проверяем абсолютное значение числа на четность. Обратите внимание, что вложенные условия могут быть произвольно вложенными, в зависимости от требований.

В целом, использование вложенных условий может быть полезным, когда необходимо более точное определение каких-либо условий или выполнение определенных действий при выполнении разных условий. Однако, при использовании вложенных условий необходимо быть осторожным, чтобы не создавать слишком много вложенных блоков кода, что может усложнить чтение и понимание кода.

---

## Пример 5: Использование логических операторов

Логические операторы используются для создания сложных условий, которые состоят из нескольких операторов сравнения.

Список логических операторов в Python:

- and - логическое "И"
- or - логическое "ИЛИ"
- not - логическое отрицание

Например:

```python

x = 5
y = 10
z = 15

# логическое "И"
print(x < y and y < z) # True
print(x < y and y > z) # False

# логическое "ИЛИ"
print(x > y or y > z) # False
print(x < y or y > z) # True

# логическое отрицание
print(not x < y) # False
print(not x > y) # True
```

---

Логические операторы могут быть использованы вместе с операторами сравнения для создания более сложных условий. Например:

```python
x = 5
y = 10
z = 15

if x < y and y < z:
    print("y находится между x и z")
elif x < y and y > z:
    print("y больше x, но меньше z")
else:
    print("y меньше x или больше z")
```

Здесь мы используем операторы сравнения ```<``` и ```>``` в сочетании с логическим оператором and, чтобы проверить, находится ли y между ```x``` и ```z```. Если это условие не выполняется, мы проверяем, больше ли y ```x``` или ```z```. Если ни одно из условий не выполняется, мы печатаем сообщение, указывающее, что y находится за пределами диапазона ```x``` и ```z```.

---

## Пример 6: Тернарный оператор

В Python, как и во многих других языках программирования, существует тернарный оператор, который позволяет сократить запись простых условных выражений. Тернарный оператор является сокращенной формой записи конструкции if-else.

Синтаксис тернарного оператора выглядит следующим образом:

```python
<значение1> if <условие> else <значение2>
```
Если <условие> истинно, то выражение вернет ```<значение1>```, в противном случае - ```<значение2>```.

Например, предположим, что мы хотим найти максимальное число из двух чисел x и y. Вместо использования конструкции if-else мы можем использовать тернарный оператор:

```python
x = 5
y = 7

max_number = x if x > y else y

print(max_number) # выведет 7
```
Здесь мы сравниваем ```x``` и ```y```, и если ```x``` больше ```y```, то в ```max_number``` записываем значение ```x```, иначе - ```y```. Результатом выполнения этого кода будет число ```7```, так как y больше ```x```.

---

## Summary

В этом уроке мы на примерах рассмотрели использовение условных операторов, операторов сравнения и логических операторов. Условные операторы ```if-elif-else``` позволяют программистам создавать различные ветвления в своем коде, основываясь на определенных условиях. Операторы сравнения используются для сравнения значений и создания условных выражений, а логические операторы позволяют комбинировать несколько условий.

---

## Задачи

- Напишите программу, которая запрашивает у пользователя число и проверяет, является ли оно четным или нечетным.

- Напишите программу, которая запрашивает у пользователя два числа и проверяет, равны ли они. Если они равны, программа должна напечатать "Числа равны", в противном случае - "Числа не равны".

- Напишите программу, которая запрашивает у пользователя три числа и проверяет, находится ли среднее из них между наименьшим и наибольшим.

- Напишите программу, которая запрашивает у пользователя возраст и проверяет, является ли он допустимым для получения прав на вождение. Допустимый возраст для получения прав на вождение - от 18 лет и старше.

- Напишите программу, которая запрашивает у пользователя два слова и проверяет, являются ли они анаграммами друг друга (то есть содержат ли они одни и те же буквы в разном порядке).

- Напишите программу, которая запрашивает у пользователя число и проверяет, является ли оно простым (то есть делится только на 1 и на само себя).

- Напишите программу, которая запрашивает у пользователя возраст и проверяет, находится ли он в диапазоне от 13 до 19 лет включительно.

- Напишите программу, которая запрашивает у пользователя три числа и проверяет, является ли сумма первых двух чисел больше или равна третьему числу. Если да, программа должна напечатать "Сумма первых двух чисел больше или равна третьему числу", в противном случае - "Сумма первых двух чисел меньше третьего числа".

- Напишите программу, которая запрашивает у пользователя год и проверяет, является ли он високосным. Год является високосным, если он делится на 4 без остатка, но если он также делится на 100, то он не является високосным, за исключением тех лет, которые делятся на 400.

- Напишите программу, которая запрашивает у пользователя число и проверяет, является ли оно положительным, отрицательным или нулем. Если число положительное, программа должна напечатать "Число положительное", если отрицательное - "Число отрицательное", если ноль - "Число равно нулю".

- Напишите программу, которая запрашивает у пользователя его имя и проверяет, является ли оно коротким (менее 5 символов), средним (от 5 до 10 символов) или длинным (более 10 символов).

- Напишите программу, которая запрашивает у пользователя две буквы и проверяет, находятся ли они в алфавитном порядке. Если да, программа должна напечатать "Буквы находятся в алфавитном порядке", в противном случае - "Буквы не находятся в алфавитном порядке".

- Напишите программу, которая запрашивает у пользователя два числа и операцию (сложение, вычитание, умножение или деление) и выполняет эту операцию над этими числами.

- Напишите программу, которая запрашивает у пользователя его рост и вес и вычисляет его индекс массы тела (ИМТ). Формула для расчета ИМТ: вес (в килограммах) / (рост (в метрах) в квадрате).

- Напишите программу, которая запрашивает у пользователя два числа и проверяет, являются ли они взаимно простыми (то есть не имеют общих делителей, кроме 1).