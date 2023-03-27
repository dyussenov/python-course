# Работа с DOM при помощи JavaScript

## Атрибут ```onclick```

В качестве основы возьмем страницу с кнопкой ```btn```, при нажатии на которую будет меняться  блок ```change```:

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <title>JS test</title>
</head>
<body>
    <div id="btn">
        Press me!
    </div>
    <div id="change">
        Change me!
    </div>
</body>
</html>
<style>
    #btn{
        display: flex;
        justify-content: center;
        align-items: center;
        padding: 10px;
        margin-bottom: 5px;
        width: 100px;
        background-color: #1bca96;
        color: white;
        cursor: pointer;
    }
    #change{
        display: flex;
        justify-content: center;
        align-items: center;
        width: 300px;
        height: 300px;
        background-color: #5b5d63;
    }
</style>
```

---

Один из способов совершать действия при нажатии на элемент - указать атрибут ```onclick``` в элементе, по которому будет производиться нажатие. В качестве значения этого атрибута передадим функцию, которую опишем позже:

```html
<div id="btn" onclick="ChangeElement()">
        Press me!
</div>
```

---

А теперь опишем функцию ```ChangeElement()``` в конце нашего HTML файла:

```html
<script>
    function ChangeElement(){
        console.log('Clicked!')
    }
</script>
```

---

На данный момент наша функция производить операцию ```console.log('Clicked!')```, которая выводит в консоль сообщение. Функция срабатывает при клике на кнопу ```btn```:

![console log js](/assets/js/consolelog.png)

---

## Получение и изменение элемента по Id

При помощи метода ```document.getElementById('change')``` мы можем получить объект с id ```change```:

```js
    function ChangeElement(){
        console.log(document.getElementById('change'))
    }
```

![element log js](/assets/js/elementlog.png)

---

Eсли добавить фильтрацию по стилям, то мы получим список стилей, которые мы можем изменить:

```js
<script>
    function ChangeElement(){
            console.log(document.getElementById('change').style)
    }
</script>
```

---

И, наконец, сохраним полученные объект в переменную ```element``` , изменим некторые стили и текст внутри блока:

```js
<script>
    function ChangeElement () {
        let element = document.getElementById('change')
        element.innerHTML = 'Changed'2
        element.style.color = 'white'
        element.style.backgroundColor = 'blue'
        element.style.fontSize = '24px'
    }
</script>
```