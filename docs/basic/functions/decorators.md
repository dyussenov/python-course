# Декораторы

Декоратор - паттерн программирования, при котором используется специальная функция-декоратор для модифицирования целевой функции не изменяя ее. Рассмотрим паттерн на популярном примере - таймере выполнения функции:

```python
from time import time, sleep


def timer(func):  # функция декоратор
    def wrapper(*args, **kwargs):
        start_time = time()
        result = func(*args, **kwargs)  # вызов целевой функции
        print(f"function {func.__name__} worked for {time() - start_time} s.")
        return result

    return wrapper


def summa(a, b):  # целевая функция
    sleep(1)
    return a + b


summa = timer(summa)  # декорирование целевой функции
res = summa(1, 2)
print(res)
```

Для декорирования в python существой специальный синтаксис `@`, позволяющий при определении целевой функции наложить на нее декоратор, тем самым избежав конструкцию `summa = timer(summa)`:

```python
@timer  # декорирование целевой функции при определении
def summa(a, b):  # целевая функция
    sleep(1)
    return a + b


res = summa(1, 2)
print(res)
```

В случае, если необходимо использвать оригинальный вариант функции, то можно вызвать поле `__wrapped__`. Для этого нужно предварительно обернуть функцию внутреннюю функцию декоратора во `@wraps`:

```python
from time import time, sleep
from functools import wraps


def timer(func):  # функция декоратор
    @wraps(func)
    def wrapper(*args, **kwargs):
        start_time = time()
        result = func(*args, **kwargs)  # вызов целевой функции
        print(f"function {func.__name__} worked for {time() - start_time} s.")
        return result

    return wrapper


@timer  # декорирование целевой функции при определении
def summa(a, b):  # целевая функция
    sleep(1)
    return a + b


res = summa(1, 2)
print(res)

origin_summa = summa.__wrapped__

print(origin_summa(1, 2))
```

## Декораторы с параметрами

Декораторы также могут принимать параметры. Для этого внутри декоратора определяется функция-обертка, которая принимает параметры и возвращает внутреннюю функцию-декоратор. Пример декоратора с параметрами:

```python
def repeat(num):
    def decorator(func):
        def wrapper(*args, **kwargs):
            for i in range(num):
                result = func(*args, **kwargs)
            return result
        return wrapper
    return decorator


def my_function():
    print('Function is executed')


my_function = repeat(3)(my_function)
my_function()
```

Здесь также можно применить синтаксический сахар:

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

## Примеры декораторов

- Декоратор кеширования:

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