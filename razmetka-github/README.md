# Шпаргалка по разметке Markdown

## Разрыв и перенос строки

По умолчанию разрыв строки в markdown игнорируется, например:

```markdown
Это первая строка.
Это вторая строка.
Это третья строка.
```

Получим результат:

<script src="https://gist.github.com/borisde/4001550415b9e1670fe8881f3e36b4a2.js"></script>

Чтобы текст начинался с новой строки нужно поставить `  ` (два пробела) в конце строки.

Результат:

<script src="https://gist.github.com/borisde/8c4f9772d1bb45e744a403cdf7cb0da5.js"></script>

## Горизонтальная линия

Чтобы нарисовать горизонтальную линию нужно поставить `три или больше` звездочки `*` ,  
минуса `-` или подчеркивания `_`

```
Пример горизонтальной линиии
***
```

Пример горизонтальной линиии
***

## Цитаты

Создать цитату можно с помощью скобки `>`

```
> Это пример цитаты
```
> Это пример цитаты

```
> Это пример 
>> Вложенной цитаты
```

> Это пример 
>> Вложенной цитаты

## Заголовки

```markdown
# Заголовок H1
## Заголовок H2
### Заголовок H3
#### Заголовок H4
##### Заголовок H5
###### Заголовок H6

или для заголовков H1 и H2

Заголовок-H1
======

Заголовок-H2
------
```

Результат:

<script src="https://gist.github.com/borisde/c2b3d71d3fa603ca947c2ac20fa06cf5.js"></script>

## Начертание шрифта

### italic
```markdown
*пример italic*  
_также пример italic_
```

<script src="https://gist.github.com/borisde/0bffa1c2715b180f21eacc40e87424d6.js"></script>

### bold
```markdown
**жирный текст**  
__также жирный текст__
```

<script src="https://gist.github.com/borisde/c8c2dacb70456e77c861e10e0bd2b739.js"></script>

### bold и italic
```markdown
**комбинированный _жирный и косой:)_**
```

<script src="https://gist.github.com/borisde/9b7022e4ac6f39844476132f2f41964a.js"></script>

### перечеркнутый
```markdown
~~перечеркнутый текст~~
```

<script src="https://gist.github.com/borisde/89f460e131e68f58461c764e085b199b.js"></script>

## Ссылки

### Формат ссылок

```markdown
[Текст ссылки](url "title")
```

### Обычная ссылка
```markdown
[Мой блог](http://refs.su)
```

<script src="https://gist.github.com/borisde/c80b66520329b5a1fe8c416c1ada73a8.js"></script>

### Ссылка с title

```markdown
[Мой блог](http://refs.su "Ссылка на блог http://refs.su")
```

<script src="https://gist.github.com/borisde/390b5c8438aa0695af9844f3ad0cfee3.js"></script>

###  Ссылки в виде сносок

```markdown
[Мой блог][blog]
```

потом добавить в любом месте:
```markdown
[blog]:http://refs.su
```

## Изображения

### Формат картинок

```markdown
![текст тега alt](url "title")
```

### Пример

```markdown
![Корабль](http://demo.cloudimg.io/s/width/300/sample.li/boat.jpg "Пример картинки")
```

<script src="https://gist.github.com/borisde/758c96a0f47a8dca5b688ec2c65f5d79.js"></script>

## Подсветка кода и синтаксиса

Выделить код в строке можно с помощью символа обратного апострофа `` ` ``

```markdown
В этой строке подсвечена `эта часть`
```
<script src="https://gist.github.com/borisde/aa9e883b88834d51b3a172fce52cc843.js"></script>

Блок кода выделяется тремя обратными апострофами \`\`\` или четырьмя пробелами в каждой строке:

````markdown
```php
$highlight = "Подсветка кода php";
echo $highlight;
```  
  
```javascript
var highlight = "Подсветка кода JavaScript";
alert(highlight);
```  
    
```
<html>
<p> Код без подсветки
</html>
```
````

Результат:

```php
$highlight = "Подсветка кода php";
echo $highlight;
```

```javascript
var highlight = "Подсветка кода JavaScript";
alert(highlight);
```
       
``` 
<html>
<p> Код без подсветки
</html>
```

## Списки

### Нумерованные списки

Нумерованные списки создаются с помощью числа, точки, пробела после них и текста пункта.
Чтобы сделать вложенный список нужно поставить 4 пробела.

```markdown
1. Первый пункт нумерованного списка
2. Второй пункт
    1. первый подпункт
        1. еще подпункт        
    2. второй подпункт
3. Третий пункт
    * маркированный подпункт 
```

<script src="https://gist.github.com/borisde/6c132f9928dbfdf3c3104e66a5bd401d.js"></script>

### Маркированные списки

Маркированные списки создаются с помощью символов `*`, `+` или `-`

```markdown
* Первый пункт маркированного списка
+ Второй пункт
    + первый подпункт
    + второй подпункт
- Третий пункт
    - маркированный подпункт 
```

<script src="https://gist.github.com/borisde/eaccfa921e579c52af78f4af62a2b5de.js"></script>

## Отмена форматирования

Чтобы отменить форматирование тегов markdown, HTML и другого кода, нужно поставить в начале строки `четыре пробела`

        *пример italic*, <br>код html</br>

## Экранирование спецсимволов

Чтобы экранировать символы markdown нужно поставить перед ними `обратную косую черту \`

```markdown
\*, \- ,+
```

Результат:

\*, \- ,+