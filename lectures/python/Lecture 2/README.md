
![](pics/presentation.png)

# План на сегодня

- инкрементирование
- наборы данных
- циклы
- работа с файлами
- синтаксический сахар

# Операторы ин- и декремента

`i += 1`     <==>         `i = i + 1`       <==>         `i++`


```python
a = 5

a += 5
print(a)

a -= 5
print(a)

a *= 5
print(a)

a //= 5
print(a)
```

    10
    5
    25
    5


# Наборы данных

## Списки (Lists)

Изменяемые наборы данных

```python
list_ = [ ]
list_ = list()
```



```python
x = ['zebra', 49, -879, 'aardvark', 200]
```


```python
len(x)
```




    5




```python
x[0]
```




    'zebra'




```python
x[-1]
```




    200



## Числовая последовательность

```python
range(START, STOP, STEP)
```


```python
print(list(range(10)))

print(list(range(1, 10)))

print(list(range(1, 10, 2)))
```

    [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
    [1, 2, 3, 4, 5, 6, 7, 8, 9]
    [1, 3, 5, 7, 9]


Для того, чтобы развернуть последовательность в обратную сторону, используется функция `reversed()`


```python
list(reversed(range(10)))
```




    [9, 8, 7, 6, 5, 4, 3, 2, 1, 0]



## Слайсы / срезы / два ствола
Общий вид
```python
list[START:STOP:STEP]
```


```python
x[:]
```




    ['zebra', 49, -879, 'aardvark', 200]




```python
x[:-1]
```




    ['zebra', 49, -879, 'aardvark']




```python
x[1:-1]
```




    [49, -879, 'aardvark']




```python
x[::2]
```




    ['zebra', -879, 200]



### Операции со списками
```python
lst.append(item) # добавить элемент в конец
lst.extend(seq)  # добавить последовательность в конец
lst.insert(idx,val) # вставить значение по индексу
lst.remove(val)     # удалить первое вхождение val
lst.pop(idx)        # удалить значение по индексу и вернуть его
lst.sort() lst.reverse() # сортировать/обратить список по месту
```


```python
x.append('datass')
x
```




    ['zebra', 49, -879, 'aardvark', 200, 'datass']




```python
x.insert(1, 'face')
x
```




    ['zebra', 'face', 49, -879, 'aardvark', 200, 'datass']




```python
last_val = x.pop(0)
print(x, last_val)
```

    ['face', 49, -879, 'aardvark', 200, 'datass'] zebra



```python
a = [123, 4, 0, 44, 14, 2, 8]
a.sort()
a
```




    [0, 2, 4, 8, 14, 44, 123]



Проверка на принадлежность списку


```python
'face' in x
```




    True




```python
'MADI' in x
```




    False



### task

Сколько элементов будет содержать список _students_ после следующих операций?

```python
students = ['Ivan', 'Masha', 'Sasha']
students += ['Olga']
students += 'Olga'
```

# 8

![](http://risovach.ru/thumb/upload/200s400/2011/06/templ_1307805679_orig_Filosoraptor.jpg?b7jd5)

## Кортежи (Tuples)

Иммутабельны, т.е. неизменяемы.

```python
tuple_ = ()
tuple_ = tuple()
```



```python
tuple_ = ('Denmark', 'Finland', 'Norway', 'Sweden')
```


```python
print("Длина кортежа - {0}".format(len(tuple_)))
```

    Длина кортежа - 4



```python
print(tuple_[0])
```

    Denmark



```python
print(tuple_[-1])
```

    Sweden


Попытка изменить кортеж приведет к закономерному провалу, т.к. кортеж __неизменяем__


```python
tuple_.append('something')
```


    ---------------------------------------------------------------------------

    AttributeError                            Traceback (most recent call last)

    <ipython-input-22-f5f46c8f6eef> in <module>()
    ----> 1 tuple_.append('something')
    

    AttributeError: 'tuple' object has no attribute 'append'


## Словари (Dictionaries)

```python
my_dict = {}
my_dict = dict()

my_dict = {'key':'value'}
```


```python
MAI = {1:'авиастроение', 'инжэкин':5, 9:'прикладная механика'}
```


```python
MAI.keys()
```




    dict_keys([1, 'инжэкин', 9])




```python
MAI.values()
```




    dict_values(['авиастроение', 5, 'прикладная механика'])




```python
MAI[1], MAI['инжэкин']
```




    ('авиастроение', 5)




```python
MAI_2 = {8: 'прикладная математика',
         4: 'радиоэлектроника летательных аппаратов'}

MAI.update(MAI_2)
MAI
```




    {8: 'прикладная математика',
     1: 'авиастроение',
     4: 'радиоэлектроника летательных аппаратов',
     'инжэкин': 5,
     9: 'прикладная механика'}



# Циклы

## For

Общий вид:
```python
for item in iterable:
    expression
```


```python
MAI = ['я', 'мы', 'лучшие люди страны']

for people in MAI:
    print('МАИ - это {0}'.format(people))
```

    МАИ - это я
    МАИ - это мы
    МАИ - это лучшие люди страны


можно итерироваться по строке


```python
for letter in 'long_word':
    print(letter)
```

    l
    o
    n
    g
    _
    w
    o
    r
    d


также можно итерироваться по числовой последовательности


```python
for i in range(1, 5):
    print('{0} muffins'.format(i))
```

    1 muffins
    2 muffins
    3 muffins
    4 muffins



```python
list(enumerate(MAI))
```




    [(0, 'я'), (1, 'мы'), (2, 'лучшие люди страны')]




```python
for people in enumerate(MAI):
    print(people)
```

    (0, 'я')
    (1, 'мы')
    (2, 'лучшие люди страны')



```python
for index, value in enumerate(MAI):
        print('index = {0}, value = {1}'.format(index, value))
```

    index = 0, value = я
    index = 1, value = мы
    index = 2, value = лучшие люди страны



```python
MAI = {1:'авиастроение', 'инжэкин':5, 9:'прикладная механика'}

for key, value in MAI.keys(), MAI.values():
    print('ключ "{0}"\t со значением "{1}"'.format(key, value))
```


    ---------------------------------------------------------------------------

    ValueError                                Traceback (most recent call last)

    <ipython-input-34-f328d3f97fa9> in <module>()
          1 MAI = {1:'авиастроение', 'инжэкин':5, 9:'прикладная механика'}
          2 
    ----> 3 for key, value in MAI.keys(), MAI.values():
          4     print('ключ "{0}"\t со значением "{1}"'.format(key, value))


    ValueError: too many values to unpack (expected 2)



```python
for key, value in zip(MAI.keys(), MAI.values()):
    print('ключ "{0}"\t со значением "{1}"'.format(key, value))
```

    ключ "1"	 со значением "авиастроение"
    ключ "инжэкин"	 со значением "5"
    ключ "9"	 со значением "прикладная механика"


## While

Общий вид (аналог функции $\displaystyle s=\sum_{i=1}^{100} i^2$)

```python
s = 0
i = 1

while i <= 100:
    s += i**2
    i += 1
```

### _task_

Чему будет равна переменная i ?

```python
i = 0
while i <= 10:
    i += 1
    if i > 7:
        i += 2
```

__13__

## Операторы `break` и `continue`

```python
i = 0
while i < 20:
    i += 1
    if i < 5:
        continue
    elif i == 10:
        break
```

# Работа с файлами

```python
file = open('file.txt', 'w', encoding='utf8')
```
1. имя файла на диске (+путь...)
2. режим работы
    - `r` - read
    - `w` - write
    - `a` - append
3. кодировка символов в текстовых файлах: _utf8_, _ascii_, _cp1251_


```python
file = open('file.txt', 'w')
file.write('hello\n')
file.close()

file = open('file.txt', 'r')
print(file.readline())
file.close()
```

    hello
    



```python
file = open('file.txt', 'a')
file.write('lorem ipsum\n')
file.write('hello world\n')
file.close()

file = open('file.txt', 'r')
line = file.read()
file.close()

print(line)
```

    hello
    lorem ipsum
    hello world
    



```python
file = open('file.txt', 'r')
line = file.read(10)
file.close()

print(line)
```

    hello
    lore



```python
with open('lambda.txt', 'r') as text:
    for line in text:
        print(line)
```

    Lorem ipsum dolor sit amet, consectetur adipiscing elit,
    
    sed do eiusmod tempor incididunt ut labore et dolore magna aliqua.
    
    Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris
    
    nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor
    
    in reprehenderit in voluptate velit esse cillum dolore eu fugiat
    
    nulla pariatur. Excepteur sint occaecat cupidatat non proident,
    
    sunt in culpa qui officia deserunt mollit anim id est laborum.
    


### task

Вывести из файла lambda.txt слова, начинающиеся с букв `a` или `u`


```python
with open('lambda.txt', 'r') as text:
    for line in text:
        for word in line.split():
            if word[0] == 'a' or word[0] == 'u':
                print(word)
            else:
                continue
```

    amet,
    adipiscing
    ut
    aliqua.
    ad
    ullamco
    ut
    aliquip
    aute
    anim


# Функциональное программирование

## Лямбда-выражения

Общий вид:
```python
lambda variable: expression_with_variable
```


```python
(lambda x: x+2)(5)
```




    7



Для применения лямбда-функции к элементам списка используется команда `map()`

Общий вид:
```python
map(lambda variable: expression, iterable)
```



```python
from __future__ import print_function

MAI = ['я','мы','лучшие люди страны']

print(list(map(lambda who: print('МАИ - это {0}'.format(who)), MAI)))
```

    МАИ - это я
    МАИ - это мы
    МАИ - это лучшие люди страны
    [None, None, None]


страшный пример из реальной жизни

```python
sigmoid = lambda y: 1. / (1 + np.exp(-y))

log_loss_from_staged_decision = lambda staged_decision, y: \
    map(lambda pred: log_loss(y, pred), map(sigmoid, staged_decision))
    
get_train_loss = lambda lr, X, y: \
    log_loss_from_staged_decision(get_clf(lr).staged_decision_function(X), y)
```

### task

Что делает эта строчка?

```python
data['Sex'] = data['Sex'].map(lambda x: 1 if x == 'M' else (-1 if x == 'F' else 0))
```

## Компоновки / списковые включения (Listing Comprehensions)


```python
def sqares(l):
    for x in l:
        print(x**2)

sqares(range(1, 10))
```

    1
    4
    9
    16
    25
    36
    49
    64
    81



```python
[x**2 for x in range(1, 10)]
```




    [1, 4, 9, 16, 25, 36, 49, 64, 81]



### task

Написать ту же функцию, но с использованием лямбда-функций


```python
list(map(lambda x: x**2, range(1,10)))
```




    [1, 4, 9, 16, 25, 36, 49, 64, 81]



# ДЗ

1. [Парсер](https://github.com/lambdafrela/parser/issues/11) - по ссылке достаточно подробное описание того, что надо сделать, если останутся вопросы - ждем в [#python](http://lambdafrela.slack.com/)
2. [Coding Bat Lists 1](http://codingbat.com/python/List-1) - задания на списки попроще
3. [Coding Bat Lists 2](http://codingbat.com/python/List-2) - задания на списки посложнее

## в следующий раз

- исключения
- регулярные выражения

# референс

- [Хорошая статья про функциональное программирование с примерами](https://ru.wikipedia.org/wiki/%D0%A4%D1%83%D0%BD%D0%BA%D1%86%D0%B8%D0%BE%D0%BD%D0%B0%D0%BB%D1%8C%D0%BD%D0%BE%D0%B5_%D0%BF%D1%80%D0%BE%D0%B3%D1%80%D0%B0%D0%BC%D0%BC%D0%B8%D1%80%D0%BE%D0%B2%D0%B0%D0%BD%D0%B8%D0%B5_%D0%BD%D0%B0_Python)
- [Страшные примеры компоновок](https://en.wikipedia.org/wiki/List_comprehension)
- [Еще немного страшных примеров на русском](https://habrahabr.ru/post/30232/)
