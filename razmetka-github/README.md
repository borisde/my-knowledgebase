# Шпаргалка по разметке Markdown

## Разрыв и перенос строки

По умолчанию разрыв строки в markdown игнорируется, например:

```markdown
Это первая строка.
Это вторая строка.
Это третья строка.
```

Получим результат:

Это первая строка.
Это вторая строка.
Это третья строка.

Чтобы текст начинался с новой строки нужно поставить `  ` (два пробела) в конце строки.

Результат:

Это первая строка.  
Это вторая строка.  
Это третья строка.  

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

## Начертание шрифта

### italic
```markdown
*пример italic*  
_также пример italic_
```

*пример italic*  
_также пример italic_

### bold
```markdown
**жирный текст**  
__также жирный текст__
```

**жирный текст**  
__также жирный текст__

### bold и italic
```markdown
**комбинированный _жирный и косой:)_**
```

**комбинированный _жирный и косой:)_**

### перечеркнутый
```markdown
~~перечеркнутый текст~~
```

~~перечеркнутый текст~~

## Ссылки

### Формат ссылок

```markdown
[Текст ссылки](url "title")
```

### Обычная ссылка
```markdown
[Мой блог](http://test.test)
```

[Мой блог](http://test.test)

### Ссылка с title

```markdown
[Мой блог](http://test.test "Ссылка на блог http://test.test")
```

[Мой блог](http://test.test "Ссылка на блог http://test.test")

###  Ссылки в виде сносок

```markdown
[Мой блог][blog]
```

потом добавить в любом месте:
```markdown
[blog]:http://test.test
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

![Корабль](http://demo.cloudimg.io/s/width/300/sample.li/boat.jpg "Пример картинки")

## Подсветка кода и синтаксиса

Выделить код в строке можно с помощью символа обратного апострофа `` ` ``

```markdown
В этой строке подсвечена `эта часть`
```
В этой строке подсвечена `эта часть`

Блок кода выделяется тремя обратными апострофами \`\`\` или четырьмя пробелами в каждой строке:

````

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

1. Первый пункт нумерованного списка
2. Второй пункт
    1. первый подпункт
        1. еще подпункт        
    2. второй подпункт
3. Третий пункт
    * маркированный подпункт 

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

* Первый пункт маркированного списка
+ Второй пункт
    + первый подпункт
    + второй подпункт
- Третий пункт
    - маркированный подпункт 

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
