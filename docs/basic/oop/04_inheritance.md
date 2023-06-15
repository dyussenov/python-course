# Наследование

***Наследование*** (*inheritance*) - один из трех основных принципов ООП. Суть наследования в создании новых классов на основе уже существующих. Такие новые классы называют *дочерними* или *подклассами*, а те, от которых они были созданы - *родительскими* или *суперклассами*.

Смысл наследования заключается в том, что дочерние классы наследуют методы и поля родительских:

```python
class Parent: # создаем родительский класс
    value = "Parent value"

    def show_value(self):
        print(self.value)


class Child(Parent): # создаем дочерний класс
    def __init__(self):
        print("child created!")


child_obj = Child()
child_obj.show_value() # получаем доступ к методу родительского класса
```

Вывод:

```bash
child created!
Parent value
```

---

Основноая цель ***наследования*** - решить проблему дублирования кода. Предположим, что мы пришем игру, в которой есть 2 схожие сущности: игрок и бот. Вот как бы выглядел код без использования наследования:

```python

```