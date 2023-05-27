# Инкапсуляция

***Инкапсуляция*** (*encapsulation*) - один из трех основных принципов ООП. У инкапсуляции 2 цели:

- Хранить данные и методы внутри класса.
- Управлять доступом к данным и методам.

## Хранение методов и полей внутри класса

С первым проявлением инкапсуляции мы столкнулись в предыдущей статье: сама идея того, что внутри класса мы можем объявлять поля или методы - это основная цель инкапсуляции, которую установили при зарождении ООП как концепции:

```python
class Car:
    def __init__(self, brand, model, year):
        print("new car created!")
        self.brand = brand 
        self.model = model
        self.year = year
        self.is_running = False

    def start(self):
        if not self.is_running:
            self.is_running = True
            print("The car has started.")
        else:
            print("The car is already running.")

car1 = Car('Lada', 'Priora', 2013)
car1.start()
print(car1.brand, car1.model, car1.year) # все поля находитяся внутри объекта
print(brand, model, year) # ошибка, так как данные имена находятся внутри пространства имен класса
```

Сам термин "инкапсуляция" происходит от латинского "in capsula" и подразумевает что нечто находится в капсуле. Далее не трудно вообразить некую коробочку (класс), сверху которой есть только какие-то кнопки управления (публичные методы), а в целом весь механизм скрыт от пользователя (программиста который использует класс).

Далее мы как раз таки поговорим о публичных методах и полях.

## Сокрытие полей и методов. Модификаторы доступа

Для разграничения доступа к полям и методам класса в Python существует три уровня доступа. Рассмотрим их по порядку.

> Термин «доступ» следует понимать как способность видеть и / или изменять внутреннее содержимое класса за пределами этого класса.

### Public

Публичный доступ (public) - публичные поля и методы доступны свободно. Для создания публичного поля/метода каких дополнительных обозначений не требуется.

```python
class Car:
    brand = 'Toyota' # публичное поле

    def drive(self, distance): # публичный метод
        print(f'машина едет {distance} км.')

car1 = Car()
print(car1.brand) # воспользуемся доступом к публичному полю

car1.brand = 'Mercedes' # изменим значение публичного поля
print(car1.brand) # проверим значение публичного поля

car1.drive(50) # воспользуемся публичным методом
```

### Private

Приватный доступ (private) - приватные поля и методы доступны только внутри класса. Для создания приватного поля/атрибута перед его названиям нужно указать два нижних пробела `__`:

```python
class Car:
    __total_kilometers = 0 # приватное поле

    def __add_kilometers(self, distance): # приватный метод
        self.__total_kilometers += distance 
    
    def drive(self, distance): # публичный метод
        print(f'машина едет {distance} км.')
        self.__add_kilometers(distance) # вызов приватного внутри класса

    def get_total_kilometers(self): # определение приватного метода
        print(f'машина проехала всего {self.__total_kilometers} км.')

car1 = Car()
car1.drive(50)
car1.drive(50)
car1.get_total_kilometers()
```

Теперь, при попытке вызвать метод `__add_kilometers()` или посмотреть значение `__total_kilometers` вне класса мы получим ошибку:

```python
print(car1.__total_kilometers)
car1.__add_kilometers(350)
```

### Protected

Защищенный доступ (protected) - защищенные поля и методы доступны только внутри самого класса и его дочерних классов (про дочерние классы и наследование расскажу в следующей статье). Защищеный доступ указывается через одно нижнее подчеркиваение `_`:

```python
class Car:
    _total_kilometers = 0 # приватное поле

    def _add_kilometers(self, distance): # приватный метод
        self._total_kilometers += distance 
    
    def drive(self, distance): # публичный метод
        print(f'машина едет {distance} км.')
        self._add_kilometers(distance) # вызов приватного внутри класса

    def get_total_kilometers(self): # определение приватного метода
        print(f'машина проехала всего {self._total_kilometers} км.')

car1 = Car()
car1.drive(50)
car1.drive(50)
car1.get_total_kilometers()
```

> Отличие защищенного доступа от приватного заключается в доступности в дочерних классах

## О реализации сокрытия в Python

В python сокрытие устроенно на уровне договоренности между программистами, иначе говоря - ***В python отсутствует строгое сокрытие.***

> Сам автор языка Гвидо Ван Россум в интервью пояснил такое дизайнерское решение: *Мы все здесь взрослые и по обоюдному согласию. Python доверяет вам. Он говорит: "Эй, если хочешь чтобы ковыряться в темных местах, я надеюсь, что у тебя есть уважительная причина, и вы не создаете проблем"*

---

Рассмотрим примеры. Модификатор доступа protected не подразумевает доступа из вне класса, но получить доступ мы можем без проблем:

```python
class Car:
    _total_kilometers = 0 # приватное поле

    def _add_kilometers(self, distance): # приватный метод
        self._total_kilometers += distance 
    
    def drive(self, distance): # публичный метод
        print(f'машина едет {distance} км.')
        self._add_kilometers(distance) # вызов приватного внутри класса

    def get_total_kilometers(self): # определение приватного метода
        print(f'машина проехала всего {self._total_kilometers} км.')

car1 = Car()
car1.drive(50)
car1.drive(50)
car1.get_total_kilometers()

print(car1._total_kilometers) # получаем доступ к приватному полю
```

Также вы можете получить доступ к приватным атрибутам, используя синтаксис именования манглинга (name mangling). Синтаксис именования манглинга заключается в добавлении префикса _ClassName к имени приватного атрибута:

```python
class Car:
    __total_kilometers = 0 # приватное поле

    def __add_kilometers(self, distance): # приватный метод
        self.__total_kilometers += distance 
    
    def drive(self, distance): # публичный метод
        print(f'машина едет {distance} км.')
        self.__add_kilometers(distance) # вызов приватного внутри класса

    def get_total_kilometers(self): # определение приватного метода
        print(f'машина проехала всего {self.__total_kilometers} км.')

car1 = Car()
car1.drive(50)
car1.drive(50)
car1.get_total_kilometers()

print(car1._Car__total_kilometers) # добавляем префикс _Car к приватному методу
```

> Важно помнить две вещи: не нарушать договоренность о сокрытии и пытаться по возможности максимально скрывать поля и методы.

## Геттеры и Сеттеры

Ранее мы уяснили, что нужно стремиться скрывать поля и методы. Но как работать с приватными полями вне класса? Начнем с установления значений приватных полей при создании класса. Тут все просто - значения просто устанавливаются в инициализаторе:

```python
class Coordinates:
    def __init__(self, x, y):
        # устанавливаем значения для приватных полей
        self.__x = x 
        self.__y = y


c = Coordinates(5, 10)
```

Но что делать, если мы в ходе написания программы нам понадобилось получить значения приватных полей? Тут нам поможет концепция геттеров - это публичные методы, которые позволяют получить значения приватных и защищенных полей:

```python
class Coordinates:
    def __init__(self, x, y):
        self.__x = x 
        self.__y = y

    def get_coordinates(self): # создаем метод геттер
        return self.__x, self.__y

c = Coordinates(5, 10)
print(c.get_coordinates())
```

И, наконец, нам может понадобиться изменять значения сокрытых полей. Тут нам помогут сеттеры:

```python
class Coordinates:
    def __init__(self, x, y):
        self.__x = x 
        self.__y = y

    def get_coordinates(self):
        return self.__x, self.__y

    def set_coordinates(self, x=0, y=0):
        self.__x = x
        self.__y = y

c = Coordinates(5, 10)
print(c.get_coordinates())

c.set_coordinates(25, 45) # Обновляем значения
print(c.get_coordinates())
```