
# Анонимные (Lambda) функции 

Лямбда-функции в Python являются одним из основных инструментов функционального программирования. Они также называются анонимными функциями, потому что они не имеют имени и создаются на лету. Лямбда-функции в Python имеют следующий синтаксис:

```python
lambda arguments: expression
```

Здесь ```arguments``` это список аргументов, разделенных запятыми, и ```expression``` это выражение, которое возвращает значение. Лямбда-функции могут иметь любое число аргументов, но только одно выражение.

---

Вот пример использования лямбда-функции, которая возвращает квадрат переданного ей числа:

```python
square = lambda x: x ** 2
print(square(5))  # выведет 25
```

Эта лямбда-функция принимает один аргумент ```x``` и возвращает ```x``` в квадрате.

## map(), filter(), reduce()

Лямбда-функции обычно используются в Python вместе с функциями высшего порядка, такими как ```map()```, ```filter()```, ```reduce()``` и т.д.

---

Функция ```map()``` принимает функцию и итерируемый объект и возвращает новый итератор, содержащий результат применения функции ко всем элементам итерируемого объекта. Например, следующий код использует лямбда-функцию для возведения в квадрат каждого элемента списка:

```python
my_list = [1, 2, 3, 4, 5]
squared_list = list(map(lambda x: x ** 2, my_list))
print(squared_list)  # выведет [1, 4, 9, 16, 25]
```

---

Функция ```filter()``` принимает функцию и итерируемый объект и возвращает новый итератор, содержащий только те элементы итерируемого объекта, для которых функция вернула ```True```. Например, следующий код использует лямбда-функцию для фильтрации четных элементов списка:

```python
my_list = [1, 2, 3, 4, 5]
even_list = list(filter(lambda x: x % 2 == 0, my_list))
print(even_list)  # выведет [2, 4]
```

---

Функция ```reduce()``` принимает функцию и итерируемый объект и последовательно применяет функцию к парам элементов, пока не останется один элемент. Например, следующий код использует лямбда-функцию для вычисления произведения всех элементов списка:

```python
from functools import reduce

my_list = [1, 2, 3, 4, 5]
product = reduce(lambda x,y: x * y, my_list)
print(product) # выведет 120
```

В этом примере лямбда-функция принимает два аргумента `x` и `y` и возвращает их произведение.

---

Лямбда-функции также могут использоваться в качестве ключа сортировки для функции `sorted()`. Например, следующий код сортирует список строк по их длине:

```python
my_list = ['apple', 'banana', 'cherry', 'date']
sorted_list = sorted(my_list, key=lambda x: len(x))
print(sorted_list) # выведет ['date', 'apple', 'banana', 'cherry']
```

В этом примере лямбда-функция принимает строку `x` и возвращает ее длину.

---

Однако, не стоит злоупотреблять лямбда-функциями. Они могут усложнить код и сделать его менее читаемым. Если вы чувствуете, что лямбда-функция становится слишком сложной, лучше создать обычную именованную функцию.

## Summary

Таким образом, лямбда-функции в Python - это удобный способ определения небольших анонимных функций на лету. Они часто используются вместе с функциями высшего порядка для применения функций к итерируемым объектам, а также в качестве ключей сортировки.

## Задачи

1. Напишите лямбда-функцию, которая возвращает квадрат переданного числа.

2. Напишите лямбда-функцию, которая проверяет, является ли переданное число четным.

3. Напишите программу, которая использует лямбда-функцию для умножения всех элементов списка my_list и выводит результат.

    Пример:
    ```python
    my_list = [1, 2, 3, 4, 5]
    # Ваш код здесь
    # Ожидаемый вывод: 120 
    ```


4. Напишите программу, которая использует лямбда-функцию для фильтрации только четных чисел из списка my_list и выводит результат.

    Пример:

    ```python
    my_list = [1, 2, 3, 4, 5]
    # Ваш код здесь
    # Ожидаемый вывод: [2, 4]
    ```
5. Напишите программу, которая использует лямбда-функцию для сортировки списка строк my_list по их длине и выводит результат.

    Пример:

    ```python
    my_list = ['apple', 'banana', 'cherry', 'date']
    # Ваш код здесь
    # Ожидаемый вывод: ['date', 'apple', 'banana', 'cherry']
    ```