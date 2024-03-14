# Замыкания в Python

Замыкание - комбинация внешней и внутренней функций, в которой внутрення фукнция ссылается на переменные внешней функции, а внешняя фукнция возвращает внутреннюю фукнцию не вывзывая её.

Рассмотрим замыкание на распространенном примере счетчика вызовов:

```python
def counter(initial_value=0):
    def step(step_value=1):
        # обращаемся к имени из внешнего скоупа
        nonlocal initial_value

        initial_value += step_value
        return initial_value

    # возвращаем функцию без вызова
    return step


counter1 = counter()
counter1()
counter1()
a = counter1()
print(a)
```

При вызове обычной фукнции все её внутренние переменные удаляются после окончания её работы. Но в замыкании все иначе:

- внутренняя фукнция использует значения из `nonlocal` области видимости (из внешней функции)

- существует внешняя ссылка на функцию `counter1`

Благодаря этим нюансам происходит "замыкание", благодаря которому сборщик мусора не очищает переменные внешней функции.

## Преимущества замыканий

Одно из ключевых преимуществ замыканий - хранение значения с доступом к модификации через установленный интерфейс.

В примере со счетчиком мы можем добавить валидацию, например, на исключительно положительные или исключительно четные числа.


```python
def counter(initial_value=0):
    def step(step_value=1):
        # обращаемся к имени из внешнего скоупа
        nonlocal initial_value
        if step_value < 0:
            raise ValueError("Допустимы только положительные числа")
        initial_value += step_value
        return initial_value

    # возвращаем функцию без вызова
    return step
```

Замыкания доступны в любой части программы, а также доступ к значениям осуществляется только через установленный создателем интерфейс.

---

Также к преимуществам отнести независимость замыканий: вы можете создать два счетчика, состояния которых не будут пересекаться:

```python
counter1 = counter()
counter2 = counter()

counter1()
counter2(8)
counter1()

print(counter1()) # 3
print(counter2()) # 9
```


## Примеры замыканий

Интересный пример c выполнением команд по ssh взятый из [этой](https://advpyneng.readthedocs.io/ru/latest/book/07_closure/closure.html) статьи

```python
from netmiko import ConnectHandler

device_params = {
    'device_type': 'cisco_ios',
    'ip': '192.168.100.1',
    'username': 'cisco',
    'password': 'cisco',
    'secret': 'cisco'
}

def netmiko_ssh(**params_dict):
        ssh = ConnectHandler(**params_dict)
        ssh.enable()
        def send_show_command(command):
            return ssh.send_command(command)
        netmiko_ssh.send_show_command = send_show_command
        return send_show_command


In [25]: r1 = netmiko_ssh(**device_params)

In [26]: r1('sh clock')
Out[26]: '*15:14:13.240 UTC Wed Oct 2 2019'
```