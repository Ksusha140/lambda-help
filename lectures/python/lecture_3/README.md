
![](pics/presentation.png)

# План на сегодня

- модули
- исключения
- регулярные выражения

# Модули на Python

Импоритируем лежащий в том же каталоге файл  `test_module.py`


```python
%cat test_module.py
```

    """Демонстрационный модуль python"""
    
    class MyClass():
    	test_atr = "Lambda"
    
    def func1():
    	print('Func 1')
    
    def func2():
    	print('Func 2')
    
    x = 1
    y = 2
    
    
    if __name__ == "__main__":
    	print('test module has been ran independently')
    else:
    	print('test module has been imported')


```python
import test_module
print('Type of object: %s'%(str(type(test_module))) )
```

    test module has been imported
    Type of object: <class 'module'>


Теперь `test_module` это объект типа `module`, то есть модуль Python.

---------

Атрибуты модуля - переменные, функции и классы, объявленые в файле, доступны нам как атрибуты класса


```python
print('\nAccesing module attributes:')
test_module.func1()
test_module.func2()

print('X equals: %s'%(str(test_module.x)))
print('Y equals: %s'%(str(test_module.y)))
print('Class atr: %s'%(str(test_module.MyClass.test_atr)))
```

    
    Accesing module attributes:
    Func 1
    Func 2
    X equals: 1
    Y equals: 2
    Class atr: Lambda


Второй вариант импорта — взятие непосредственно имени без имени модуля.

Импорт на основе `from` обладает такой особенностью, что он делает импортируемые атрибуты _read-only_.


```python
from test_module import func1, func2, x
print('\nAccesing attributes with a different import:')
func1()
func2()

x = 10
print("local x equals:",x)
print("Module x equals", test_module.x)
```

    
    Accesing attributes with a different import:
    Func 1
    Func 2
    local x equals: 10
    Module x equals 1


В данном случае `x` — это локальная переменная, в то время как переменные `x` в модуле не меняются.

--------

Можно задать `alias` ипортируемому имени для упрощения или избежания конфликта имен


```python
import test_module as md
print('\nImporting with alias')
print("Module x equals", md.x)

from test_module import func1 as f1
f1()
```

    
    Importing with alias
    Module x equals 1
    Func 1


Для выяснения имен, определенных в модуле, можно использовать встроенную функцию `dir()`. Она возвращает отсортированный список строк.
У модулей есть различные атрибуты, например `__doc__` выведет документацию к модулю. В нашем случае это будет многострочный комментарий в начале файла.


```python
print('\nUsing dir:')
print(dir(test_module))
with open('out.txt', "w", encoding='utf-8') as f:
    f.write(test_module.__doc__)
```

    
    Using dir:
    ['MyClass', '__builtins__', '__cached__', '__doc__', '__file__', '__loader__', '__name__', '__package__', '__spec__', 'func1', 'func2', 'x', 'y']



```python
%cat out.txt
```

    Демонстрационный модуль python

Пакеты (packages)
Пакеты — способ структурирования пространств имен модулей на основе файловой системы. Пакетная организация дает все удобства по управлению большим количеством файлов. Пакетный импорт делает код более читабельным и значительно упрощает поиск. 
Пример:
```
    TCP/
       _init_.py
       main.py
       
       Server/
             _init_.py
             tcp.py
             server.py
             lib.py
       Client/
             _init_.py
             tcp.py
             client.py
             lib.py
```

Файл `_init_.py` необходим для того, чтобы интерпретатор распознал каталог, как содержащий пакет.
Этот файл может быть пустым. В этом файле обычно содержится информация о пакете (автор, версия, контакты и другое).

Мы можем импортировать тестовый модуль из пакета например так


```python
!tree test_package/
```

    [01;34mtest_package/[00m
    ├── __init__.py
    └── package_test_module.py
    
    0 directories, 2 files



```python
from test_package import package_test_module as ptm
print('\nImporting from package')
print("Module x equals", ptm.x)
```

    test module has been imported
    
    Importing from package
    Module x equals 100


Или так


```python
import test_package.package_test_module as ptm
print('\nImporting from package 2')
print("Module x equals", ptm.x)
```

    
    Importing from package 2
    Module x equals 100


Или так


```python
from test_package import *
print('\nImporting from package 3')
print("Module z equals", package_test_module.z)
```

    
    Importing from package 3
    Module z equals 300


Что означает импорт всех имен из пакета.

Этот вариант работает только если в `__init__` файле пакета определен список `__all__` , в котором перечисляются модули, которые импортируются в этом случае.  
    `__all__ = ["package_test_module"]`

# Исключения в Python


```python
result = 1 / 0
```


    ---------------------------------------------------------------------------

    ZeroDivisionError                         Traceback (most recent call last)

    <ipython-input-12-3b10e2a1c32c> in <module>()
    ----> 1 result = 1 / 0
    

    ZeroDivisionError: division by zero


Если запустить этот код мы получим ошибку `ZeroDivisionError`.
Более корректно называть это исключением.

Существует (как минимум) два различимых вида ошибок: синтаксические ошибки (_syntax errors_) и исключения (_exceptions_).

Синтаксические ошибки, появляются во время разбора кода интерпретатором. С точки зрения синтаксиса в коде выше ошибки нет, интерпретатор видит деление одного integer на другой.

Однако в процессе выполнения возникает исключение. Интерпретатор разобрал код, но провести операцию не смог.
Таким образом ошибки, обнаруженные при исполнении, называются исключениями (_exceptions_). 

Исключения бывают разных типов и тип исключения выводится в сообщении об ошибке, например `ZeroDivisionError`, `NameError`, `ValueError`

__Давайте обрабатывать!__

Существует возможность написать код, который будет перехватывать избранные исключения. Посмотрите на представленный пример, в котором пользователю предлагают вводить число до тех пор, пока оно не окажется корректным целым. Тем не менее, пользователь может прервать программу (используя сочетание клавиш Control-C или какое-либо другое, поддерживаемое операционной системой)
Заметьте — о вызванном пользователем прерывании сигнализирует исключение KeyboardInterrupt.


```python
while True:
    try:
        x = int(input("Input a number:"))
        break
    except ValueError:
        print("Incorrect integer")
```

    Input a number:one
    Incorrect integer
    Input a number:forty two
    Incorrect integer
    Input a number:42


Оператор `try` работает следующим образом:

В начале исполняется блок `try` (операторы между ключевыми словами `try` и `except`).
Если при этом не появляется исключений, блок `except` не выполняется и оператор `try` заканчивает работу.
Если во время выполнения блока `try` было возбуждено какое-либо исключение, оставшаяся часть блока не выполняется. Затем, если тип этого исключения совпадает с исключением, указанным после ключевого слова `except`, выполняется блок `except`, а по его завершению выполнение продолжается сразу после оператора `try`-`except`.
Если порождается исключение, не совпадающее по типу с указанным в блоке `except` — оно передаётся внешним операторам `try`; если ни одного обработчика не найдено, исключение считается необработанным (_unhandled exception_), и выполнение полностью останавливается и выводится сообщение об ошибке.

Блок `except` может указывать несколько исключений в виде заключённого в скобки кортежа.


```python
try:
    x = int(input("Input another number:"))
except (RuntimeError, TypeError, NameError, ValueError):
    print("Caught an exception")
```

    Input another number:forty two
    Caught an exception


В последнем блоке except можно не указывать имени (или имён) исключений. Тогда он будет действовать как обработчик всех исключений. 


```python
while True:
    try:
        x = int(input("Input a number:"))
        break
    except:
        print("Incorrect integer")
```

    Input a number:4.5
    Incorrect integer
    Input a number:5


Получить доступ к информации об исключении можно используя `sys.exc_info()[0]`


```python
import sys
while True:
    try:
        x = int(input("Input a number:"))
        break
    except:
        print("Caught exception:",sys.exc_info()[0])
```

    Input a number:5.6
    Caught exception: <class 'ValueError'>
    Input a number:forty two
    Caught exception: <class 'ValueError'>
    Input a number:-42


Более простой способ: записать экземпляр исключения в переменную


```python
while True:
    try:
        x = int(input("Input a number:"))
        break
    except ValueError as e: 
        print("Incorrect integer", e)
```

    Input a number:5.4
    Incorrect integer invalid literal for int() with base 10: '5.4'
    Input a number:forty two
    Incorrect integer invalid literal for int() with base 10: 'forty two'
    Input a number:5


Исключения могут охватывать несколько уровней.

При возбуждении исключения оно передается "вверх" пока не достигнет самого высокого уровня или не будет "поймано" блоком except.

Это значит, что исключения внутри функции не вызовут ошибку, если функция будет в блоке try-except:


```python
def zero_division():
    return 1/0

try:
    zero_division()
except:
    print("Caught exception")
```

    Caught exception


Можно увеличить вложенность 


```python
def level_3():
    return 1/0

def level_2():
    return level_3()

def level_1():
    return level_2

try:
    level_1()
except:
    print("Caught exception")
```

Можно порождать свои исключения оператором raise


```python
try:
    raise(Exception("amazing!"))
except Exception as e:
    print(e)
```

    amazing!


Можно добавить в блок try-except блоки else и finally.

Блок else будет выполнен если try не породил исключений.

Блок finally будет выполнен в любом случае.


```python
def divide(x, y):
    try:
        result = x / y
    except ZeroDivisionError:
        print("Zero division!")
    else:
        print("Result ", result)
    finally:
        print("finally\n")

print(divide(1,2))
print(divide(1,0))
```

    Result  0.5
    finally
    
    None
    Zero division!
    finally
    
    None


Можно создавать собственные исключения - для этого нужно объявить новый класс, наследующийся от Exception.


```python
class MyError(Exception):
    def __init__(self, value):
        self.value = value
    def __str__(self):
        return repr(self.value)

try:
    raise MyError(2*2)
except MyError as e:
    print('My exception occurred, value:', e.value)
```

    My exception occurred, value: 4


# Регулярные выражения в Python

## Теория:

Регулярные выражения - формальный язык для поиска и манипуляций текстом, в частности подстроками.

Регулярные выражения основаны на масках (`pattern`). Это шаблоны или правила, которые удовлетворяют некоторому множеству строк.
Так, из простых примеров, можно найти все вхождения "кот" в строку "кот терракот котом котором".

__Плюсы__:
+ удобны в использовании
+ универсальны

__Минусы__:
- регулярные выражения для сложных задач (с множеством условий) нечитабельны и сложны в разработке
- регулярные выражения работают медленно

В Python регулярные выражения предоставляются библиотекой `re`. Она изначально установлена для всех официальных сборок Python.

Рассмотрим самые часто используемые методы:
- `re.match()`
- `re.search()`
- `re.findall()`
- `re.split()`
- `re.sub()`
- `re.compile()`


```python
import re

# Текст, над которым мы будем проводить операции с помощью регулярных выражений
text = "The object has the words \"NO STEP\" on it and could be from the plane's horizontal stabilizer - \
        the wing-like parts attached to the tail, sources say. It was discovered by an American who has been \
        blogging about the search for MH370."

print('Text for searching:\n{0}'.format(text))
```

    Text for searching:
    The object has the words "NO STEP" on it and could be from the plane's horizontal stabilizer -         the wing-like parts attached to the tail, sources say. It was discovered by an American who has been         blogging about the search for MH370.


Рассмотрим методны на простом примере: поиске полного соответствия

```python
re.match(pattern, string)
``` 
ищет подходящую под маску pattern строку в начале строки text.



```python
pattern = r'The'  
```

`r` перед строкой указывает, что это "raw string" для регулярного выражения

Почему так см. https://docs.python.org/3/howto/regex.html#the-backslash-plague


```python
result = re.match(pattern, text)
```

Почему так см. https://docs.python.org/3/howto/regex.html#the-backslash-plague

При успешном поиске будет создан особый объект с результатом, при неуспешном в result запишется None, то есть ничего.
Если попытаться вывести result - возникнет ошибка

Если найдено, вывести найденный текст, если нет, вывести, что не найдено.


```python
result = result.group(0) if result else "Not found"
# используем метод .group(0) чтобы указать, что хотим получить результат
# первой группы. О группах позже
print('Searching for \"{0}\" using match.\nResult:\n{1}'.format(str(pattern), str(result)))
```

    Searching for "The" using match.
    Result:
    The


Попробуем использовать match для поиска второго слова

Напишем вспомогательную функцию


```python
def result_or_not_found(result):
    return result.group(0) if result else "Not found"

pattern = r'object'

result = result_or_not_found(re.match(pattern, text))

print("Searching for \"{0}\" using match.\nResult:\n{1}".format(str(pattern), str(result)))
```

    Searching for "object" using match.
    Result:
    Not found


`re.search(pattern, string)` похож на `match()`, но он ищет не только в начале строки

Повторим опыт с помощью `search`


```python
pattern = r'The'

result = result_or_not_found(re.search(pattern, text))

print("Searching for \"{0}\" using search.\nResult:\n{1}".format(str(pattern), str(result)))
```

    Searching for "The" using search.
    Result:
    The


Попробуем использовать search для поиска второго слова


```python
pattern = r'object'

result = result_or_not_found(re.search(pattern, text))

print("Searching for \"%s\" using search.\nResult:\n%s\n" %
      (str(pattern), str(result)))
```

    Searching for "object" using search.
    Result:
    object
    


В отличие от match мы получили искомую строку.

-----

`re.findall(pattern, string)` возвращает список всех найденных совпадений


```python
pattern = r'the'
result = re.findall(pattern, text)
print("Searching for \"%s\" using findall.\nResult:\n%s\n" %
      (str(pattern), str(result)))
```

    Searching for "the" using findall.
    Result:
    ['the', 'the', 'the', 'the', 'the']
    


`re.split(pattern, string, [maxsplit=0])` делит строку по маске
`maxsplit` определяет максимальное количество разделений. При 0 метод разделит строку столько раз, сколько возможно.


```python
pattern = r'the'
result = re.split(pattern, text)
print("Splitting text by \"%s\" using split.\nResult:\n%s\n" %
      (str(pattern), str(result)))
```

    Splitting text by "the" using split.
    Result:
    ['The object has ', ' words "NO STEP" on it and could be from ', " plane's horizontal stabilizer -         ", ' wing-like parts attached to ', ' tail, sources say. It was discovered by an American who has been         blogging about ', ' search for MH370.']
    


`re.sub(pattern, repl, string)` ищет маску `pattern` в строке `string` и заменяет её на строку `repl`


```python
pattern = r'NO STEP'
repl = 'LAMBDA'
result = re.sub(pattern, repl, text)
print("Replacing \"%s\" by \"%s\" using sub.\nResult:\n%s\n" %
      (str(pattern), str(repl), str(result)))
```

    Replacing "NO STEP" by "LAMBDA" using sub.
    Result:
    The object has the words "LAMBDA" on it and could be from the plane's horizontal stabilizer -         the wing-like parts attached to the tail, sources say. It was discovered by an American who has been         blogging about the search for MH370.
    


`re.compile()` создает из строки отдельный объект, который мы можем использовать для дальнейших операций.
Компиляция паттерна регулярного выражения ускоряет поиск.


```python
pattern = re.compile(r'the')
result = pattern.findall(text)
print("Searching for \"%s\" using findall with compiled str in text1.\nResult:\n%s\n" %
      (str(pattern), str(result)))
```

    Searching for "re.compile('the')" using findall with compiled str in text1.
    Result:
    ['the', 'the', 'the', 'the', 'the']
    



```python
text2 = "Early photographic analysis of the object suggests it could have come from the doomed jet,\
         which vanished almost exactly 2 years ago."

result = pattern.findall(text2)  # Не нужно компилировать паттерн заново

print("Searching for \"%s\" using findall with compiled str in text2.\nResult:\n%s\n" %
      (str(pattern), str(result)))
```

    Searching for "re.compile('the')" using findall with compiled str in text2.
    Result:
    ['the', 'the']
    


Пока что в наших паттернах использовались только обычные символы.

"The" соответствует на языке регулярных выражений только строке "The".

Посмотрим на мощный инструмент: метасимволы.
Метасимволы это символы, которые соответстуют особым шаблонам. Вот они.

- `.`   Один любой символ, кроме новой строки `\n`.
- `?`   0 или 1 вхождение шаблона слева
- `+`   1 и более вхождений шаблона слева
- `*`   0 и более вхождений шаблона слева
- `\w`  Любая цифра или буква (`\W` — все, кроме буквы или цифры)
- `\d`  Любая цифра `[0-9]` (`\D` — все, кроме цифры)
- `\s`  Любой пробельный символ (`\S` — любой непробельнй символ)
- `\b`  Граница слова
- `[..]`    Один из символов в скобках (`[^..]` — любой символ, кроме тех, что в скобках)
- `\`   Экранирование специальных символов (`\.` означает точку или `\+` — знак «плюс»)
- `^` и `$`   Начало и конец строки соответственно
- `{n,m}`   От `n` до `m` вхождений (`{,m}` — от `0` до `m`)
- `a|b` Соответствует `a` или `b`
- `()`  Группирует выражение и возвращает найденный текст
- `\t`, `\n`, `\r`  Символ табуляции, новой строки и возврата каретки соответственно

-----

Примеры использования:

- `all_symbols = r'*'` - соответствует всей строе 
- `symbols = r'.'` - соответстует одному символу, `findall` с этим паттерном вернет список символов в строке
- `letters_and_numbers = r'\w'` - соответстует одной букве или цифре, `findall` с этим паттерном вернет, список символов в строке за исключением пробелов
- `number = r'\d'` - `findall` с этим паттерном вернет список цифр найденных в строке
- `articules = r'a|an'` - `findall` с этим паттерном вернет список a и an найденных в строке
- `dots = r'\.'` - `findall` с этим паттерном вернет список со всеми точками в строке, заметьте что из-за экранирования паттерн не соответствует никаким символам, кроме точки

- `last_word = r'\w*\.$'` - `findall` с этим паттерном вернет список с последним словом в строке
- `all_words = r'\w+'` - `findall` с этим паттерном вернет список слов
- `quoted = r'\".*\"'` - `findall` с этим паттерном вернет слова, заключенные в кавычки
- `longwords = r'\w{5,}'` - `findall` с этим паттерном вернет слова с 5 или более буквами
- `first_three_letters = r'\b\w{3}'` - `findall` с этим паттерном вернет первые 3 буквы каждого слова
- `starting_with = r'\b[abc]\w+'` - findall с этим паттерном вернет слова начинающиеся на a, b или с
- `starting_not_with = r'\b[^abc ]\w+'` - findall с этим паттерном вернет слова не начинающиеся на a, b или с. Обратите внимание на пробел в скобках: он означает, что мы не ищем последовательности символов начинающиеся с пробела.

### Проверка телефонного номера


```python
li = ['9999999999', '999999-999', '99999x9999', '892512303', '89293536800']
for val in li:
    if re.match(r'[8-9]{1}[0-9]{9}', val) and len(val) == 10:
        print(True)
    else:
        print(False)
```

    True
    False
    False
    False
    False


## Источники и дальнейшее чтение:

- [Программирование на Python: Часть 5. Модули](https://www.ibm.com/developerworks/ru/library/l-python_part_5/)
- [5. The import system](https://docs.python.org/3/reference/import.html)
- [6. Modules](https://docs.python.org/3/tutorial/modules.html)
- [Ошибки и исключения](http://pep8.ru/doc/tutorial-3.1/8.html)
- [Использование регулярных выражений в Python для новичков](http://tproger.ru/translations/regular-expression-python/)
- [Регулярные выражения, пособие для новичков. Часть 1](https://habrahabr.ru/post/115825/)
- [Регулярные выражения](http://pep8.ru/doc/dive-into-python-3/7.html)
- [Regular Expression HOWTO](https://docs.python.org/3/howto/regex.html)

## Домашнее задание
- Напишите программу, которая позволяет пользователю ввести с клавиатуры email и пароль. Проверьте их на следующие правила:
 - email:
    - содержит только латинские буквы, цифры, @ и точку 
    - содержит @ и домен и зону (.ru, .com и прочее)
    - домен не короче 3 символов, не длиннее 10 символов, не начинается с цифры
    - доменная зона не короче двух символов, не имеет цифр
    - имя пользователя не длиннее 10 символов, не начинается с цифры

 - пароль:
    - длиннее трех, короче четырех
    - содержит любые символы кроме пробела, таба и переноса строки
    - содержит хотя бы одну латинскую букву, одну цифру, одну латинскую букву верхнего регистра
    - не содержит последовательностей букв длиннее трех символов
 
Не обязетельно реализовывать все правила в одном регулярном выражении. Вы можете поступать как удобно, главное, чтобы это работало корректно и вы сами могли понять то, что написали.

- добавить обработку исключений в парсер, чтобы программа не "вылетала" при неудачных попытках читать и писать несуществующие или заблокированные файлы (исключение IOError например).
