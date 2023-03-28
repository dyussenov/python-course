
# Создание собственных функций в Python

## Что такое функция и как её создать?

Функция это блок организованного, многократно используемоего кода, который используется для выполнения конкретного задания. Функции обеспечивают лучшую модульность приложения и значительно повышают уровень повторного использования кода.

Вы уже знакомы со встроенными функциями, такими как ```print()```, ```input()```, ```min()```, ```max()``` и т.д. Помимо использования встроенных функций, вы также можете создвать собственные функции.

Создать функцию можно при помощи ключевого слова ```def``` и указания названия функции после него. Рассмотрим пример создания простой функции:

```Python
def my_function():
    print('This is my first function')
```

Слово  ```def``` это сокращение от английского define - определять. Именно это мы сделали - мы определили название функции и то, что она выполняет. В данном примере она просто выводит текст. Но этого недостаточно. Для того чтобы функция сработала мало её определить - её нужно *вызвать*:

```Python
def my_function(): #определение функции
    print('This is my first function')

my_function() #вызов функции
```

Выше было сказано, что функция - блок многократно используемого кода. Следовательно, определенные нами функции можно использовать неограниченное колчиство раз, точно также как и встроенные функции:

```Python
def my_function():
    print('This is my first function')

my_function()
my_function()
my_function() 
```

## Аргументы функций

Аргументы - это значения, передающиеся в функцию необходимые для её работы. Выше мы рассматривали пример функции, в которою передавалось 0 аргументов. Подобные функции без аргументов довольно редкое явление, так что рассмотрим пример с использованием аргументов:

```Python
#функция для вывода данных о пользователе
def user(name, age):
    print(name+"'s age is", age)

user('Mikhail', 33)
```

*Аргумент со значением по умолчанию* это аргумент, значение которого указано при создании функции. Это позволяет не указывать значение данного аргумента при вызове данной функции:

```Python
def user(name, age='unknown'):
    print(name+"'s age is", age)

user('Mikhail') #при вызове указываем только имя
user('Dmitry', 23) #при вызове указываем оба аргумента
```

При работе с такими аргументами следует быть особо внимательным. Для примера добавим в нашу функцию второй аргумент по умолчанию:

```Python
def user(name, age='unknown', gender='unknown'):
    print(name+"'s age is", age, 'and gender is', gender)

#при вызове указываем имя и пол пользователя, но не возраст
user('Mikhail', 'male') 
```

Результат:

```text
> Mikhail's age is male and gender is unknown
```

Как видите, пол, который мы указали при вызове функции был распознан как значение для первого аргумента со значением по умолчанию. Интерпритатор не может волшебным способом угадывать, какому именно аргументу вы хотели задать значение, поэтому он подставил его в самое первое.

Чтобы избежать подобных казусов, нужно прописывать название нужного нам аргумента, а после него его значение при вызове функции:

```Python
def user(name, age='unknown', gender='unknown'):
    print(name+"'s age is", age, 'and gender is', gender)

#при вызове указываем имя и пол пользователя, но не возраст
user('Mikhail', gender='male') 
```

Результат:

```text
> Mikhail's age is unknown and gender is male
```

И самое важное - аргументы по умолчанию при определении функции указываются после обычных аргументов:

```Python
def user( age='unknown', name, gender='unknown'):
    print(name+"'s age is", age, 'and gender is', gender)
```

Вывод:

```text
> SyntaxError: non-default argument follows default argument
```

> Также существуют аргументы с произвольной длинной, о них рассказывается здесь НУЖНО СДЕЛАТЬ ССЫЛКУ

## Return, возвращение значения

Выражение ```return``` прекращает выполнение функции и возвращает указанное после выражения значение. Соответственно, теперь становится возможным, например, присваивать результат выполнения функции какой либо переменной.

```Python
def bigger(a,b):
    if a > b:
        return a # Если a больше чем b, то возвращаем a и прекращаем выполнение функции
    return b # Незачем использовать else. Если мы дошли до этой строки, то b, точно не меньше чем a
 
# присваиваем результат функции bigger переменной num
num = bigger(23,42)
print(num)
```

 Выражение ```return``` без аргументов это то же самое, что и выражение ```return None```. Также функции, в которых нет ```return``` по умолчанию возвращают ```None```:

```Python
def user( age='unknown', name, gender='unknown'):
    print(name+"'s age is", age, 'and gender is', gender)

user_info = user('Mikhail') 
print(user_info)
```

## Область видимости переменных

Некоторые переменные скрипта могут быть недоступны некоторым областям программы. Все зависит от того, где вы объявили эти переменные.

В Python две базовых области видимости переменных: *глобальная* и *локальная*

Переменные объявленные внутри тела функции имеют локальную область видимости, те что объявлены вне какой-либо функции имеют глобальную область видимости.

Это означает, что доступ к локальным переменным имеют только те функции, в которых они были объявлены, в то время как доступ к глобальным переменным можно получить по всей программе в любой функции.

Например:

```Python
# глобальная переменная age
age = 44
 
def info():
    print age # Печатаем глобальную переменную age
 
def local_info():
    age = 22 # создаем локальную переменную age 
    print age
 
info() # напечатает 44
local_info() # напечатает 22
```

Важно помнить, что для того чтобы получить доступ к глобальной переменной, достаточно лишь указать ее имя. Однако, если перед нами стоит задача изменить глобальную переменную внутри функции - необходимо использовать ключевое слово ```global```.

```Python
# глобальная переменная age
age = 13
 
# функция изменяющая глобальную переменную
def get_older():
    global age
    age += 1
 
print age # напечатает 13
get_older() # увеличиваем age на 1
print age # напечатает 14
```