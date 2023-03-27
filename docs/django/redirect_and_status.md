
# Переадресация и отправка статусных кодов

## Переадресация в Django

Редирект — это перенаправление или переадресация. Благодаря редиректу можно перенаправлять посетителя на нужный адрес внутри сайта или за его пределы. Например, вы сохранили страницу в закладки, но за это время она поменяла свой адрес, однако, вебмастер настроил переадресацию (301 редирект) со старой страницы на новую.

Переадресация бывает временная и постоянная. При временной переадресации мы указываем, что документ временно перемещен на новый адрес. В этом случае в ответ отправляется статусный код 302. При постоянной переадресации мы уведомляем, что документ теперь постоянно будет достуен по новому адресу

Для создания временной переадресации применяется класс ```HttpResponseRedirect```, а для постоянной - класс ```HttpResponsePermanentRedirect```, которые расположены в пакете ```django.http```.

---
Оформим путь для новой страницы с использованием аргумента ```name```. Указывать конкретный URL-адрес (кроме их списка в коллекции urlpatterns) – это порочная практика, или, как еще говорят – хардкодинг. Вместо этого каждому шаблону пути можно присвоить свое уникальное имя и использовать его в рамках всего проекта.

```python
#blog_app/urls.py
from django.urls import path

from .views import *

urlpatterns = [
    path('', index), #127.0.0.1:8000
    path('about/', about, name='about'),  #127.0.0.1:8000/about
    path('categories/<str:category>/', categories), #127.0.0.1:8000/categories/cats
    path('contact/', contact)
]
```

---
Определим редирект при помощи функции ```redirect()```. В качестве адреса редиректа укажем не URL, а ```name``` необходимого маршрута:

```python
#blog_app/views.py
from django.http import HttpResponse
from django.shortcuts import redirect


def index(request):
    return HttpResponse("Index page of blog application")


def about(request):
    return HttpResponse("About blog application")


def categories(request, category):
    return HttpResponse(f"Посты с категорией {category}")


def contact(request):
    return redirect("about")
```

Теперь при переходе на страницу ```/contact``` произойдет редирект на страницу ```about```.

---

## Определение страницы 404

Для начала нам необходимо выйти из режима отладки. Он активируется по умолчанию при создании проекта. Благодаря ему мы видим отладочные сообщения, которые помогают найти и исправить ошибку в процессе разработки приложения. При запуске приложения в "боевом" режиме, режим отладки следует отключить. Сейчас же мы его отключим временно, чтобы посмотреть как отрабатывает ошибка 404.

Для этого в корневом приложении в файле ```settings.py``` присвоим значение ```False``` константе ```DEBUG``` и укажем локальный сервер в качестве константы ```ALLOWED_HOSTS```:

```python
#blog/settings.py

...
# SECURITY WARNING: don't run with debug turned on in production!
DEBUG = False

ALLOWED_HOSTS = ['127.0.0.1']

...
```
Теперь при переходе по несуществующему URL мы будем видеть понятную пользователю страницу:

![404 django](/assets/django/404/404.png)

---

Чтобы настроить свою страницу 404, нужно создать представление:

```python
#blog_app/views.py
from django.http import HttpResponse, HttpResponseNotFound
from django.shortcuts import redirect


def index(request):
    return HttpResponse("Index page of blog application")


def about(request):
    return HttpResponse("About blog application")


def categories(request, category):
    return HttpResponse(f"Посты с категорией {category}")


def contact(request):
    return redirect("/about")


#представление для ошибки 404
def page404(request, exception): 
    return HttpResponseNotFound('<h1>Страница не найдена</h1>')
```

---

Теперь в глобальном файле ```urls.py``` создадим специальную переменную, которая будет хранить функцию представления, которая отвечает за страницу 404:

```python
#blog/urls.py

from django.contrib import admin
from django.urls import include, path
from blog_app.views import page404

urlpatterns = [
    path('admin/', admin.site.urls),
    path('', include('blog_app.urls'))
]

handler404 = page404
```

---

В результате мы увидим кастомную страницу 404:

![404 django](/assets/django/404/custom_404.png)

## Вызов ошибки 404

В некоторых случаях необходимо вызвать ошибку 404. Предположим, пользователь вручную указал в адресной строке шаблон с некорректным значением. Определим представление с архивом постов за определенный год. Понятно, что нельзя выдать посты за год, который еще не наступил.

Создадим представление, которе будет выдавать ошибку 404 при некорректном значении в шаблоне:

```python
#blog_app/views.py

from datetime import date

from django.http import HttpResponse, HttpResponseNotFound, Http404
from django.shortcuts import redirect


def index(request):
    return HttpResponse("Index page of blog application")


def about(request):
    return HttpResponse("About blog application")


def categories(request, category):
    return HttpResponse(f"Посты с категорией {category}")


def contact(request):
    return redirect("/about")


def page404(request, exception):
    return HttpResponseNotFound('<h1>Страница не найдена</h1>')


#представление для архива
def archive(request, year):
    if(int(year) > date.today().year or int(year) < 2015):
        raise Http404()
 
    return HttpResponse(f"<h1>Архив по годам</h1>{year}</p>")

```

---

Определим URL шаблон:

```python
#blog_app/urls.py

from django.urls import path

from .views import *

urlpatterns = [
    path('', index),
    path('about/', about, name='about'), 
    path('categories/<str:category>/', categories),
    path('contact/', contact),
    path('archive/<int:year>/', archive)
]
```

---

Получаем вот такой результат:

![404 django](/assets/django/404/raise_404.png)