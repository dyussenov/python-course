# Декораторы

Что такое декораторы в Python?
Декораторы в Python - это функции, которые принимают другую функцию в качестве аргумента, изменяют ее поведение и возвращают измененную функцию. Функция, которая передается в декоратор, называется декорируемой функцией.

Декораторы в Python используются для расширения функциональности функций и классов, не изменяя их исходного кода. Например, вы можете использовать декораторы для добавления логирования, проверки типов аргументов, кэширования результатов и т.д.

# Пример декоратора
Рассмотрим простой пример декоратора, который добавляет логирование к функции:

```python
def logger(func):
    def wrapper(*args, **kwargs):
        print('Logging start')
        result = func(*args, **kwargs)
        print('Logging end')
        return result
    return wrapper

@logger
def my_function():
    print('Function is executed')
```

В этом примере мы определяем декоратор ```logger```, который принимает функцию в качестве аргумента. Декоратор определяет внутреннюю функцию wrapper, которая добавляет логирование перед и после вызова функции ```my_function```. Декоратор возвращает функцию ```wrapper```, которая заменяет исходную функцию ```my_function```.

Затем мы используем декоратор ```@logger``` перед определением функции my_function. Это эквивалентно вызову функции ```logger``` с аргументом ```my_function``` и присваиванию результата обратно в ```my_function```. Теперь при вызове ```my_function``` будет выполняться не только ее основной код, но и логирование.

## Декораторы с параметрами

Декораторы также могут принимать параметры. Для этого внутри декоратора определяется функция-обертка, которая принимает параметры и возвращает внутреннюю функцию-декоратор. Вот пример декоратора с параметрами:

```python
def repeat(num):
    def decorator(func):
        def wrapper(*args, **kwargs):
            for i in range(num):
                result = func(*args, **kwargs)
            return result
        return wrapper
    return decorator

@repeat(num=3)
def my_function():
    print('Function is executed')
```

## Декоратор-таймер

Пример декоратора для измерения времени выполнения функции:

```python
import time

def timer(func):
    def wrapper(*args, **kwargs):
        start_time = time.time()
        result = func(*args, **kwargs)
        end_time = time.time()
        print(f"Elapsed time: {end_time - start_time:.5f} seconds")
        return result
    return wrapper

@timer
def my_function():
    time.sleep(2)
    print('Function is executed')

my_function()
```

В этом примере мы определяем декоратор ```timer```, который принимает функцию в качестве аргумента. Декоратор определяет внутреннюю функцию wrapper, которая измеряет время выполнения функции и выводит его в консоль. Декоратор возвращает функцию wrapper, которая заменяет исходную функцию.

Затем мы используем декоратор ```@timer``` перед определением функции ```my_function```. Это эквивалентно вызову функции ```timer``` с аргументом ```my_function``` и присваиванию результата обратно в ```my_function```. Теперь при вызове ```my_function``` будет выполняться не только ее основной код, но и измерение времени выполнения.

Декоратор кеширования

```python
def cache(func):
    storage = {}

    def wrapper(*args):
        if args not in storage.keys():
            res = func(*args)
            storage[args] = res
            print(f"{args}:{res} added to storage")
            return res
        else:
            print(f"res took from cache")
            return storage[args]

    return wrapper


@cache
def summa(a: int | float, b: int | float) -> int | float:
    return a + b


print(summa(1, 2))
print(summa(1, 2))
```