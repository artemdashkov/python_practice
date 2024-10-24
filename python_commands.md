- [Install](#install)
- [Modules](#modules)
	- [black](#black)
	- [flake8](#flake8)
	- [os](#os)
- [Работа с виртуальным окружением](#работа-с-виртуальным-окружением)
- [allure](#allure)
- [assert](#assert)
- [random](#random)
- [in](#in)
- [pickle](#pickle)
- [классы](#классы)

# Install
```python
apt install python3 # Установка Python в linux
```

```python
#!/usr/bin/python3  -строка в файле линукс, которая позволяет интерпретировать файл как python файл
```

# Modules
## black
black - форматтер, позволяет выполнять автоматическое редактирование кода
```python
pip install black # запуска форматтера
black 
```
## flake8
```python
pip install flake8 # установка линтера flake8 для программ на Python с открытыми исходными кодами, позволяет находить ошибки в стиле оформления кода
flake8 programm.py # применить линтер к выбранному файлу
flake8 project_folder # применить линтер к всему проекту
```
## os
```python
import os
print(os.getcwd()) # получить путь к папке, где находится текущий файл
> C:\Users\admin\PycharmProjects\selenium\lesson_10

print(f'{os.getcwd()}\\downloads')
> C:\Users\admin\PycharmProjects\selenium\lesson_10\downloads
```
- `os.mkdir('./dir_1')` - создает папку в месте размещению исполняемого файла
- `os.mkdir('./dir_2./dir_3')` - создает папку dir_2 в которой будет размещена папка dir_3, выдаст исключение "FileExistsError" если папка уже создана
- `os.environ` - возвращает в виде словаря переменные окружения

# Versions
```python
python --version # показывает версию python
python3 --version # показывает версию python
```

# chmod a+x <имя скрипта>
	-сделать файл испольняемым для всех пользователей
	
Запуска скрипта python
ИЗ ТОЙ ДИРЕКТОРИИ, ГДЕ НАХОДИТСЯ ФАЙЛ СКРИПТА
	# ./<имя скрипта>	-С указанием интерпретатора в начале скрипта
	# /usr/bin/python3 <имя скрипта>	-Без указания интерпретатора в начале скрипта

ИЗ ДРУГОЙ ДИРЕКТОРИИ, ГДЕ НЕТ ФАЙЛА СКРИПТА
	# <полный путь к файлу скрипта>	-С указанием интерпретатора в начале скрипта
	# /usr/bin/python3 <полный путь к файлу скрипта>	-Без указания интерпретатора в начале скрипта

## pip
 - система управления пакетами или менеджера пакетов (Python Package Index)
```python
apt install python3-pip # установка pip в линуксе
pip3 --version	# проверить версию pip
pip install -r requirements.txt		# установка зависимостей
pip freeze	# просмотр зависимостей
pip freeze > requirements.txt	# все зависимости скопировать в файл requirements.txt
```

#### pip3 команда опции пакет(ы)
	show		показать информацию о пакете
	search		найти пакет
	install		установить пакет
	uninstall	удалить пакет
	download	скачать пакет и зависимости (без установки)
	list		вывести список установленных пакетов


	

# poetry
Установка Windows (Powershell)
	официальная документация https://python-poetry.org/docs/#installing-with-the-official-installer
	в powershell: (Invoke-WebRequest -Uri https://install.python-poetry.org -UseBasicParsing).Content | py -
	pip install poetry
	
poetry --version
	Poetry (version 1.7.1)

poetry config --list
	перечень настроек poetry

Установка зависимостей
	poetry install 	# для первичной установки
	poetry update 	# для обновления
	
poetry show --tree
	показать зависимости
	
poetry shell
	создать и активировать виртуальное окружение одной командой
	

# РАБОТА С ВИРТУАЛЬНЫМ ОКРУЖЕНИЕМ
apt install python3-venv
	-(команда линукс)установка пакета venv

python3 -m venv myenv
или
python -m venv myenv
	-создать новое окружение, где myenv имя вирутального окружения. После выполнения команды будет создана папка с именем myenv, содержащая все необходимые файлы для виртуального окружения.

python -m venv virt_name

pip install virtualenv
	установка virtualenv, требуется для python	


bin include lib lib64 pyvenv.cfg share
	-посмотреть содержимое виртуального окружения

source myenv/bin/activate
	-активация виртуального окружения. где myenv название папки среды

после активации виртуального окружения в командную строку будет добавлено имя виртуального окружения.


(myenv) user@host:~$ pip install requests
	-Эта команда установит пакет requests только в текущем виртуальном окружении.

deactivate
	-используется для удаления виртуальной среды

exit()

# allure
```python
$ pip install allure-pytest
$ py.test --alluredir=%allure_result_folder% ./tests
$ allure serve %allure_result_folder%
https://pypi.org/project/allure-pytest/
```
# Трехместное выражение if/else в Python

```python
if a < b:
    rez = a + b
else:
    rez = a - b

# общий вид if/else в одну строку
x = a if condition else b

# Выражение примера выше будет выглядеть следующим образом
rez = a + b if a < b else a - b.
```

# assert

```python
assert True, 'Print message'
> 

assert False, 'Print message'
> # assert False, 'Print message', AssertionError: Print message
```

**При выявлении несоответствии в методе assert дальнейший код не выполняется, а при положительном assert - дальнейший код выполняется, например**:
- Assert завершился AssertionError 
```python
value_1 = 3
value_2 = 5
assert value_1 == value_2, "Not equal"
print('this is code after assert method')

>>>
Traceback (most recent call last):
  File "C:\Users\dashkov\PycharmProjects\selenium_stepik_2\lesson_18\draft_3.py", line 4, in <module>
    assert value_1 == value_2, "Not equal"
AssertionError: Not equal
```

- Assert завершился без ошибок
```python
value_1 = 3
value_2 = 3
assert value_1 == value_2, "Not equal"
print('this is code after assert method')

>>>
this is code after assert method
```

# in
- `in` - Возвращает True если последовательность присутствует в объекте
- `not in` - Возвращает True если последовательность не присутствует в объекте
```python
ur_1 = "yandex.ru/slash_1/slash_2"
ur_2 = "yandex.ru"

print (ur_2 in ur_1)
> True
print (ur_2 not in ur_1)
> False

print (ur_1 in ur_2)
> False
print (ur_1 not in ur_2)
> True
```
Вхождение/наличие элемента в список/ кортеж/ множество
```python
>>> x = [1, 2, 3, 4, 5, 6, 7, 8]
>>> 5 in x
# True

>>> 5 not in x
# False

>>> 0 in x
# False

>>> 0 not in x
# True
```

# range
```python
a_1 = range(5)
a_2 = list(a_1) 

print(a_1)		> range(0, 5)
print(a_2)		> [0, 1, 2, 3, 4]
```

```python
c_1 = range(0, 50, 10)
c_2 = list(c_1)

print(c_1)		> range(0, 50, 10)
print(c_2)		> [0, 10, 20, 30, 40]
```

```python
a_1 = range(10, 0, -1)
a_2 = list(a_1)

print(a_1)		range(10, 0, -1)
print(a_2)		[10, 9, 8, 7, 6, 5, 4, 3, 2, 1]
```


# random

### random.randint и random.randrange
```python
import random

""" Функции randint и randrange возвращают целое число. """
number = random.randint(1, 10) # вернет значение от 1 до 10, оба значения включены в диапазон
number = random.randrange(10) # вернет значение от 0 до 9
number = random.randrange(5,10) # вернет значение от 5 до 9
number = random.randrange(1, 102, 10) # вернет одно значение из списка [1, 11, 21, 31, 41, 51, 61, 71, 81, 91, 101]
```

### random.random
```python
""" Функции random возвращает случайное число с плавающей точкой."""
number = random.random() # возвращает случайное число с плавающей точкой в диапазоне от 0.0 до 1.0 (но исключая 1.0)
```

### random.uniform
```python
""" Функции uniform тоже возвращает случайное число с плавающей точкой,  но при этом она позволяет задавать диапазон значений, из которого следует отбирать значения. """
number = random.uniform(1.0, 10.0) # возвращает случайное число с плавающей точкой в диапазоне от 1.0 до 10.0
```

### random.choices
```python
""" Функции choices возвращает список из n повторяющихся элементов."""
a = random.sample([1, 2, 3, 4, 5, 6, 7], k=3)
print(a)

> [3, 1, 3]
```

### random.sample
```python
""" Функции sample возвращает список из n не повторяющихся элементов."""
a = random.sample([1, 2, 3, 4, 5, 6, 7], 2)
print(a)

> [3, 1]
```


# pickle
```python
import os
import pickle

# синтаксис pickle.dump: что, куда и операции
pickle.dump(
    driver.get_cookies(),
    open(os.getcwd()+"/cookies/cookies.pkl", "wb"))

# синтаксис pickle.load: что загружем, откудакуда и операции
cookies = pickle.load(open(os.getcwd()+"/cookies/cookies.pkl", "rb"))
```

Операции pickle:
- wb - write binary: записать в бинарном формате
- rb - read binary: считать из  бинарного формата


# Классы
Объект - это программная сущность, которая содержит данные и процедуры. В концептуальном плане объект представляет собой автономную
единицу, которая состоит из атрибутов данных и методов, которые оперируют атрибутами данных 
Атрибуты данных - находящиеся внутри объекта данные называются 
Методы - Выполняемые объектом процедуры, это функции, которые выполняют операции с атрибутами данных.
Инкапсуляция - объединение данных и программного кода в одном объекте.
Класс - это программный код, который задает атрибуты данных и методы для объекта определенного типа. класс - это описание свойств объекта.
Экземпляр класса - объект, который создается на основе класса.



## dataclass


```python
from dataclasses import dataclass 

@dataclass
class Card:
    summary: str = None
    owner: str = None
    state: str = "todo"
    id: int = field(default=None, compare=False)

    @classmethod
    def from_dict(cls, d):
        return Card(**d)
    def to_dict(self):
        return asdict(self)
```


# with, as

Конструкция with ... as в Python используется для работы с контекстными менеджерами, 
что позволяет управлять ресурсами, обеспечивая их правильное открытие и закрытие.
Это особенно полезно при работе с файлами, сетевыми соединениями и другими ресурсами, 
требующими очистки (например, освобождения памяти или закрытия файлов).

Конструкция with ... as упрощает работу с ресурсами, 
обеспечивает их автоматическое управление и позволяет избежать утечек ресурсов, 
что делает ваш код более чистым и безопасным.

Когда вы используете конструкцию with, она гарантирует правильное выполнение действий 
при входе и выходе из блока кода, даже если в нем возникает ошибка. 
Основными преимуществами являются:
- Автоматическое управление ресурсами.
- Упрощение синтаксиса и улучшение читаемости кода.

```python
with open('example.txt', 'w') as file:  
    file.write('Привет, мир!')  
```
1. open('example.txt', 'w'): Функция open открывает файл example.txt в режиме записи.
2. as file: Открытый файл присваивается переменной file, и теперь мы можем использовать её для записи в файл.
3. with: Когда блок кода завершен (либо нормально, либо с исключением), файл автоматически закрывается, что устраняет необходимость явно вызывать file.close().

# @contextmanager
В Python @contextmanager — это декоратор из модуля contextlib, который позволяет создавать контекстные менеджеры без необходимости реализовывать методы __enter__ и __exit__ в классе. Контекстные менеджеры используются в конструкции with для автоматического управления ресурсами, такими как файлы или сетевые соединения, обеспечивая их корректное открытие и закрытие.

Когда вы используете @contextmanager, вы можете определить yield в функции, что позволяет разделить код на две части: до yield и после. Код до yield выполняется при входе в контекст, а код после yield выполняется при выходе из контекста, что позволяет обрабатывать освобождение ресурсов.

Вот пример использования @contextmanager:
```python
 print("Entering the context")  
 try:  
 yield42 # Значение, которое будет возвращено в блоке with finally:  
 print("Exiting the context")  

# Используем контекстный менеджерwith my_context_manager() as value:  
 print(f"Inside the context: {value}")  
```

Вывод будет следующим:
```python
Entering the contextInside the context:42Exiting the context```  

В этом примере при входе в контекст выполняется код перед `yield`, при выходе — код после `yield`. Это удобно для управления ресурсами и обеспечения их правильного освобождения.  
```

пример
```python
@contextmanager
def cards_db():
    db_path = get_path()
    db = cards.CardsDB(db_path)
    yield db
    db.close()
```