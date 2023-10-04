# Работа с изображениями в  Django

> необходимо создание виртуального окружения

- создание модели

    ```python
    from django.db import models
    
    class ImageModel(models.Model):
        title = models.CharField(max_length=100)
        image = models.ImageField(upload_to='images/')
    ```

- Миграции (Windows)

    ```cmd
    py -m manage makemigrations
    py -m manage migrate
    ```

- дополнительные настройки в settings.py. MEDIA_ROOT - это абсолютный путь к папке на файловой системе, где будут храниться загруженные пользовательские медиафайлы. Это место, куда Django будет сохранять фактически загруженные файлы.   

    ```python
    MEDIA_URL = '/media/'
    MEDIA_ROOT = os.path.join(BASE_DIR, 'media/')
    ```

- доплонительные урлы    

    ```python
    from django.conf import settings
    from django.conf.urls.static import static
    
    urlpatterns = [
        # ... ваши пути URL ...
    ] + static(settings.MEDIA_URL, document_root=settings.MEDIA_ROOT)
    ```

- форма для модели

    ```python
    from django import forms
    from .models import ImageModel
    
    class ImageForm(forms.ModelForm):
        class Meta:
            model = ImageModel
            fields = ('title', 'image')
    ```
    
- вьюха для загрузки изображений

    ```python
    from django.shortcuts import render
    from .forms import ImageForm
    
    def upload_image(request):
        if request.method == 'POST':
            form = ImageForm(request.POST, request.FILES)
            if form.is_valid():
                form.save()
        else:
            form = ImageForm()
        return render(request, 'upload_image.html', {'form': form})
    ```

- темплейт

    ```python
    <form method="post" enctype="multipart/form-data">
    {% csrf_token %}
    {{ form.as_p }}
    <button type="submit">Upload</button>
    </form>
    ```
    
- вьюха для демонстрации изображений

    ```python
    from django.shortcuts import render
    from .models import ImageModel
    
    def image_list(request):
        images = ImageModel.objects.all()
        return render(request, 'image_list.html', {'images': images})
    ```

- темплейт

    ```html
    <!DOCTYPE html>
    <html>
    <head>
        <title>Image List</title>
    </head>
    <body>
        <h1>Image List</h1>
        <ul>
            {% for image in images %}
                <li>
                    <h2>{{ image.title }}</h2>
                    <img src="{{ image.image.url }}" alt="{{ image.title }}">
                </li>
            {% endfor %}
        </ul>
    </body>
    </html>
    ```