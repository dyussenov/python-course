
# Создание приложения. Работа с url

Проект Django состоит из приложений. Приложения декомпозируют сложный многофункциональный проект на более мелкие сущности. По отдельности работать с каждым приложением проще. В отдельных приложениях проще найти баг или добавить фичу. Также отдельные приложения можно использовать в различных проектах.

---

## Создание приложения в проекте

При создании проекта автоматически создаются служебные приложения. Список всех приложений можно посмотреть в файле ```settings.py``` в пакете конфигурации в списке ```INSTALLED_APPS```: 

```python
#blog/settings.py

INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
]
```

---

Создадим новое приложение, которе будет отвечать за блог:

```
py -m manage startapp blog_app
```

После выполнения команды структура проекта выглядит следующим образом:

![Структура проекта django](/assets/django/create_app/project-structure.png)

---

Рассмотрим структуру нашего нового приложения:

- Каталог ```migrations```: Предназначен для хранения миграций - скриптов, которые позволяют синхронизировать структуру базы данных с определением моделей.

- ```__init__.py``` - Указывает интерпретатору python, что текущий каталог будет рассматриваться в качестве пакета.

- ```admin.py``` - Предназначен для административных функций, в частности, здесь призводится регистрация моделей, которые используются в интерфейсе администратора

- ```apps.py``` - Определяет конфигурацию приложения.

- ```models.py``` - Хранит определение моделей, которые описывают используемые в приложении данные.

- ```tests.py``` - Хранит тесты приложения.

- ```views.py``` - Определяет функции, которые получают запросы пользователей, обрабатывают их и возвращают ответ.

---

На данный момент наше приложение не зарегистрировано. Грубо говоря, Django "не видит" его. Чтобы это исправить, достаточно прописать название приложения в вышеупомянутом списке

```python
#blog/settings.py

INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'blog_app',
]
```

> При указании названия приложения, Django будет искать пакет под названием ```blog_app```, а если быть точкее, то файл ```apps.py```. Внутри этого файла будет класс конфигурации приложения. Именно он нужен этому списку. Поэтому вместо простого названия приложения можно указать путь к этому классу: ```'blog_app.apps.BlogAppConfig'```

---

## Создание функции представления

Создадим простую функцию представления в нашем приложении, которая будет возвращать простой текст:

```python
#blog_app/views.py
from django.shortcuts import render
from django.http import HttpResponse


def index(request):
    return HttpResponse("Index page of blog application")
```

В данном случае мы импортируем класс HttpResponse из стандартного пакета django.http. Затем определяется функция index(), которая в качестве параметра получает объект запроса request. Класс HttpResponse предназначен для создания ответа, который отправляется пользователю. И с помощью выражения ```return HttpResponse("Index page of blog application")``` мы отправляем пользователю этот текст.

---

## Работа с файлами ```urls.py```. Маршрутизация

Для упрощения работы с маршрутами для представлений нашего приложения создадим файл ```urls.py``` внутри него:

![Структура проекта django](/assets/django/create_app/urls.png)

```python
#blog_app/urls.py
from django.urls import path

from .views import *

urlpatterns = [
    path('', index),
]
```

В этом файле мы соберем все представления из файла ```views.py``` при помощи импортирования. Создадим список ```urlpatterns```, в котором при помощи специальной функции ```path()``` создадим маршруты для отображения представлений. Первым аргументом передаем маршрут, вторым имя представления.

---

Теперь в файле ```urls.py``` пакета конфигурации добавим маршруты из нашего приложения при помощи функции ```include()```

```python
from django.contrib import admin
from django.urls import include, path

urlpatterns = [
    path('admin/', admin.site.urls),
    path('blog/', include('blog_app.urls'))
]
```

После этих операций нам будет доступно наше представление по адресу http://127.0.0.1:8000/blog/

![index](/assets/django/create_app/index.png)

---

Закрепим процесс создания представлений и указания их маршрутов. В файле ```views.py``` добавим представление ```about```:

```python
#blog_app/views.py
from django.shortcuts import render
from django.http import HttpResponse


def index(request):
    return HttpResponse("Index page of blog application")


def about(request):
    return HttpResponse("About blog application")
```

---

В файле ```urls.py``` нашего приложения укажем маршрут к новому представлению:

```python
#blog_app/urls.py
from django.urls import path

from .views import *

urlpatterns = [
    path('', index),
    path('about', about),
]
```

---

Теперь по адресу ```http://127.0.0.1:8000/blog/about``` доступно наше новое представление. Но что если мы хотим избавиться от префикса ```blog``` в нашем маршруте? Для этого достаточно убрать его в ```urls.py``` пакета конфигурации:

```python
#blog/urls.py
from django.contrib import admin
from django.urls import include, path

urlpatterns = [
    path('admin/', admin.site.urls),
    path('', include('blog_app.urls'))
]
```

---

Теперь новое представление доступно по адресу ```http://127.0.0.1:8000/about```, 
а ```index``` - по ```http://127.0.0.1:8000```

![index](/assets/django/create_app/about.png)