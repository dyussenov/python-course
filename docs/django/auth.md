
# Регистрация и аутентификация пользователей в Django

В фреймворке Django присутствует *встроенная модель пользователя*, которая поддается модификации. На ее основе можно (и рекомендуется авторами фреймворка) *создавать кастомную модель пользователя*.

## Создание приложения с регистрацией и аутентификацией

- Создаем приложение

  ```shell
  python3 -m manage startapp users
  ```

- Добавляем новое созданное приложение `'users.apps.UsersConfig'` в список приложений и переопределим модель пользователя на кастомную, которую создадим в следующем шагу `AUTH_USER_MODEL = 'users.CustomUser'`:

    ```python
    # config/settings.py

    INSTALLED_APPS = [
        'django.contrib.admin',
        'django.contrib.auth',
        'django.contrib.contenttypes',
        'django.contrib.sessions',
        'django.contrib.messages',
        'django.contrib.staticfiles',
        'users.apps.UsersConfig', # new
    ]

    AUTH_USER_MODEL = 'users.CustomUser' # new
    ```

- А теперь создадим кастомную модель пользователя `CustomUser`:

  ```python
  # users/models.py

  from django.contrib.auth.models import AbstractUser
  from django.db import models

  class CustomUser(AbstractUser):
      pass
      # add additional fields in here

      def __str__(self):
          return self.username
  ```

- Теперь создадим формы для регистрации и смены данных:

  ```python
  # users/forms.py

  from django import forms
  from django.contrib.auth.forms import UserCreationForm, UserChangeForm
  from .models import CustomUser

  class CustomUserCreationForm(UserCreationForm):

      class Meta:
          model = CustomUser
          fields = ('username', 'email')

  class CustomUserChangeForm(UserChangeForm):

      class Meta:
          model = CustomUser
          fields = ('username', 'email')
  ```

- Работа с моделью в админ панели

  ```python
  # users/admin.py

  from django.contrib import admin
  from django.contrib.auth import get_user_model
  from django.contrib.auth.admin import UserAdmin

  from .forms import CustomUserCreationForm, CustomUserChangeForm
  from .models import CustomUser

  class CustomUserAdmin(UserAdmin):
      add_form = CustomUserCreationForm
      form = CustomUserChangeForm
      model = CustomUser
      list_display = ['email', 'username',]

  admin.site.register(CustomUser, CustomUserAdmin)
  ```

- Конфигурируем пути к темплейтам:

  ```python
  # config/settings.py

  TEMPLATES = [
      {
          ...
          'DIRS': [os.path.join(BASE_DIR, 'templates')], # new
          ...
      },
  ]
  ```

- И указываем имена URL, по которым приложение будет направлять пользователя при логине и логауте:

  ```python
  # config/settings.py

  LOGIN_REDIRECT_URL = 'home'
  LOGOUT_REDIRECT_URL = 'home'
  ```

- Создаем темплейты:

  ```html
  <!-- templates/base.html -->

  <!DOCTYPE html>
  <html>
  <head>
    <meta charset="utf-8">
    <title>{% block title %}Django Auth Tutorial{% endblock %}</title>
  </head>
  <body>
    <main>
      {% block content %}
      {% endblock %}
    </main>
  </body>
  </html>
  ```

  ```html
  <!-- templates/home.html -->

  {% extends 'base.html' %}

  {% block title %}Home{% endblock %}

  {% block content %}
  {% if user.is_authenticated %}
    Hi {{ user.username }}!
    <p><a href="{% url 'logout' %}">logout</a></p>
  {% else %}
    <p>You are not logged in</p>
    <a href="{% url 'login' %}">login</a> |
    <a href="{% url 'signup' %}">signup</a>
  {% endif %}
  {% endblock %}
  ```

  ```html
  <!-- templates/registration/login.html -->

  {% extends 'base.html' %}

  {% block title %}Login{% endblock %}

  {% block content %}
  <h2>Login</h2>
  <form method="post">
    {% csrf_token %}
    {{ form.as_p }}
    <button type="submit">Login</button>
  </form>
  {% endblock %}
  ```

  ```html
  <!-- templates/signup.html -->

  {% extends 'base.html' %}

  {% block title %}Sign Up{% endblock %}

  {% block content %}
  <h2>Sign up</h2>
  <form method="post">
    {% csrf_token %}
    {{ form.as_p }}
    <button type="submit">Sign up</button>
  </form>
  {% endblock %}
  ```

- Добавляем пути в глобальный файл url:

  ```python
  # config/urls.py

  from django.contrib import admin
  from django.urls import path, include
  from django.views.generic.base import TemplateView

  urlpatterns = [
      path('', TemplateView.as_view(template_name='home.html'), name='home'),
      path('admin/', admin.site.urls),
      path('users/', include('users.urls')),
      path('users/', include('django.contrib.auth.urls')),
  ]
  ```

  - `path('', TemplateView.as_view(template_name='home.html'), name='home'),` - позволяет отобразить темплейт без view. 
  -  `path('users/', include('django.contrib.auth.urls')),` - подключает специальные пути для работы с авторизацией:
  ```
  login/ [name='login']
  logout/ [name='logout']
  password_change/ [name='password_change']
  password_change/done/ [name='password_change_done']
  password_reset/ [name='password_reset']
  password_reset/done/ [name='password_reset_done']
  reset/<uidb64>/<token>/ [name='password_reset_confirm']
  reset/done/ [name='password_reset_complete']
  ```
  
- Локальные пути в приложении users:

  ```python
  # users/urls.py

  from django.urls import path
  from .views import SignUpView

  urlpatterns = [
      path('signup/', SignUpView.as_view(), name='signup'),
  ]
  ```

- View для регистрации пользователя:

  ```python
  # users/views.py

  from django.urls import reverse_lazy
  from django.views.generic.edit import CreateView

  from .forms import CustomUserCreationForm

  class SignUpView(CreateView):
      form_class = CustomUserCreationForm
      success_url = reverse_lazy('login')
      template_name = 'signup.html'
  ```