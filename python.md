- [Install](#install)
- [Modules](#modules)
	- [black](#black)
	- [flake8](#flake8)
	- [os](#os)
    - [pip](#pip)
- [Работа с виртуальным окружением](#работа-с-виртуальным-окружением)
- [allure](#allure)
- [assert](#assert)
- [random](#random)
- [in](#in)
- [pickle](#pickle)
- [классы](#классы)
	- [Наследование](#наследование)
    - [Инкапсуляция](#инкапсуляция)
	- [Полиморфизм](#полиморфизм)
    - [Абстракция](#абстракция)
	    - [@abstractmethod](#abstractmethod)
	- [@dataclass](#dataclass)
	- [@staticmethod](#staticmethod)

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
pip install pip-review  # Обновить всех пакетов в окружении
pip3 list -перечень всех установленных библиотек
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
ООП - 

`Класс в Python` — это шаблон для создания объектов, который объединяет данные (атрибуты) и функции (методы), которые работают с этими данными. Классы позволяют создавать новые типы данных, инкапсулируя логику и состояние. Классы являются основой объектно-ориентированного программирования (ООП) в Python.

`Объект (Экземпляр класса)` - это программная сущность, которая создается на основе класса и содержит данные и методы. В концептуальном плане объект представляет собой автономную единицу, которая состоит из атрибутов данных и методов, которые оперируют атрибутами данных. 
`Атрибуты данных` - находящиеся внутри объекта данные 
`Методы` - Выполняемые объектом функции, которые выполняют операции с атрибутами данных.
`Инкапсуляция` - объединение данных и программного кода в одном объекте.

Принципы ООП в python:
1. Наследование
2. Инкапсуляция
3. Полиморфизм
4. Абстракция

## Наследование
Наследование - это механизм, который позволяет создавать новый класс на основе уже существующего, наследуя его свойства и методы. Это способствует повторному использованию кода и упрощает его поддержку.

Вот основные моменты о наследовании классов в Python:
1. Создание базового класса: Это класс, от которого будут наследоваться другие классы.
2. Создание производного класса: Это класс, который наследует свойства и методы базового класса.

```python
# Определяем базовый класс  
class Animal:  
    def __init__(self, name):  
        self.name = name  

    def speak(self):  
        return "Animal sound"  

# Определяем производный класс  
class Dog(Animal):  
    def speak(self):  
        return "Bark"  

class Cat(Animal):  
    def speak(self):  
        return "Meow"

class Cow(Animal):
    def __init__(self, name, weight):
        Animal.__init__(self, name)  # super().__init__(name)
        self.weight = weight
    def speak(self):
        return "Moo"

class 

# Создаем экземпляры производных классов  
dog = Dog("Rex")  
cat = Cat("Mittens") 
cow = Cow("Marta", 300)

# Вызываем метод speak  
print(dog.name + " says: " + dog.speak())  # Rex says: Bark  
print(cat.name + " says: " + cat.speak())  # Mittens says: Meow 
print(cow.name + " says: " + cow.speak(), "and weight is: " + str(cow.weight)  # Marta says: Moo and weight is: 300
```
### Особенности при наследовании классов 
1. Ключевое слово `super()`: Используется для вызова методов базового класса.
```python
class Dog(Animal):  
    def __init__(self, name, breed):  
        super().__init__(name)  # Вызов конструктора базового класса  
        self.breed = breed  
```
2. Множественное наследование: Класс может наследовать от нескольких классов.
```python
class Canine:  
    def bark(self):  
        return "Woof!"  

class Pet:  
    def play(self):  
        return "Playing!"  

class Dog(Canine, Pet):  
    pass 
```
3. Переопределение методов: Метод в производном классе может переопределять метод базового класса.
4. Абстрактные классы: Используются для создания интерфейсов. Для этого используется модуль abc.
```python
from abc import ABC, abstractmethod  

class Animal(ABC):  
    @abstractmethod  
    def speak(self):  
        pass  

class Dog(Animal):  
    def speak(self):  
        return "Bark"  
```

## Инкапсуляция
`Инкапсуляция` — скрытие внутренней реализации класса и предоставление только необходимых интерфейсов для взаимодействия с его объектами. Это помогает защитить внутренние данные и методы от некорректного использования, а также упрощает изменения и улучшает читаемость кода.

## Полиморфизм
Полиморфизм — позволяет объектам разных классов обрабатываться как объекты одного интерфейса. В Python полиморфизм достигается с помощью методов, которые могут иметь одинаковые имена, но разные реализации в разных классах.

Основные типы полиморфизма в Python:
- Полиморфизм через наследование: Когда дочерние классы переопределяют методы родительского класса.
- Полиморфизм через интерфейсы: Когда разные классы реализуют один и тот же метод, но ведут себя по-разному.

**Пример полиморфизма через наследование**
```python
class Animal:  
    def speak(self):  
        raise NotImplementedError("Subclasses must implement this method")  

class Dog(Animal):  
    def speak(self):  
        return "Woof!"  

class Cat(Animal):  
    def speak(self):  
        return "Meow!"  

def animal_sound(animal):  
    print(animal.speak())  

# Примеры использования  
dog = Dog()  
cat = Cat()  

animal_sound(dog)  # Вывод: Woof!  
animal_sound(cat)  # Вывод: Meow!  
```
Объяснение примера:
- В классе `Animal` определён метод `speak`, который должен быть переопределён в дочерних классах. Он вызывает ошибку, если вызывается в самом классе Animal.
- Классы `Dog` и `Cat` наследуют от `Animal` и предоставляют свою реализацию метода speak.
- Функция `animal_sound` принимает объект типа `Animal` и вызывает его метод `speak`. Благодаря полиморфизму, можно передавать объекты разных классов (`Dog` и `Cat`) и получать правильный звук.

**Пример полиморфизма через интерфейсы**
```python
class Bird:  
    def fly(self):  
        return "I'm flying!"  

class Airplane:  
    def fly(self):  
        return "Flying through the sky!"  

def fly_object(flyable):  
    print(flyable.fly())  

# Примеры использования  
sparrow = Bird()  
boeing = Airplane()  

fly_object(sparrow)  # Вывод: I'm flying!  
fly_object(boeing)   # Вывод: Flying through the sky!  
```
**Объяснение примера:**
- Класс `Bird` и класс `Airplane` оба имеют метод `fly()`.
- Функция `fly_object` принимает объект, имеющий метод `fly`, и вызывает этот метод. Таким образом, можно использовать любые объекты, которые реализуют этот метод, независимо от их внутренней структуры.

Таким образом, полиморфизм позволяет разработчикам писать более абстрактный и гибкий код, обеспечивая возможность для расширения и изменения, не влияя на существующий код.

## Абстракция
Абстракция в объектно-ориентированном программировании (ООП) в Python — это принцип, который позволяет скрыть сложные детали реализации и предоставить только необходимые интерфейсы для взаимодействия с объектами. Она помогает сосредоточиться на том, что делает объект, а не на том, как он это делает.

### @abstractmethod
Абстрактные классы в Python позволяют создавать базовые классы с определенными методами, которые должны быть реализованы в производных классах. Это полезно для определения интерфейса, который обязаны соблюдать подклассы. Для работы с абстрактными классами в Python используется модуль abc.

**Основные элементы абстрактных классов**
1. Модуль abc: Импортирует необходимые декораторы и классы для создания абстрактных классов.
2. Декоратор @abstractmethod: Используется для определения абстрактного метода, который должен быть реализован в производных классах.

```python
from abc import ABC, abstractmethod  

# Определяем абстрактный класс  
class Animal(ABC):  

    @abstractmethod  
    def speak(self):  
        pass  # Абстрактный метод без реализации  

    @abstractmethod  
    def move(self):  
        pass  # Другой абстрактный метод  

# Определяем производный класс  
class Dog(Animal):  
    
    def speak(self):  
        return "Bark"  

    def move(self):  
        return "Run"  

# Еще один производный класс  
class Bird(Animal):  

    def speak(self):  
        return "Tweet"  

    def move(self):  
        return "Fly"  

# Создаем экземпляры производных классов  
dog = Dog()  
bird = Bird()  

# Вызываем методы  
print(dog.speak())  # Output: Bark  
print(dog.move())   # Output: Run  
print(bird.speak()) # Output: Tweet  
print(bird.move())  # Output: Fly 
```
**Особенности абстрактных классов**
1. Невозможность создания экземпляра: Нельзя создать экземпляр абстрактного класса напрямую. Например, animal = Animal() вызовет ошибку TypeError.
2. Наследование: Продуктовые классы должны реализовывать все абстрактные методы базового класса, иначе они также станут абстрактными.
3. Дополнительные методы: Абстрактный класс может иметь и обычные методы с реализацией.

**Заключение**

Абстрактные классы являются мощным инструментом для обеспечения соблюдения интерфейсов в иерархиях классов. Они способствуют созданию более структурированного, поддерживаемого и понятного кода. Использование абстрактных классов помогает определить, какие методы должны быть реализованы в производных классах, что делает код более предсказуемым и удобным для понимания.

## @dataclass
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


## @staticmethod
`@staticmethod` в Python — это декоратор, который используется для определения статического метода внутри класса. 
Статический метод не требует доступа к экземпляру класса (то есть объекту, созданному из этого класса) или к самому классу. 
Он может быть вызван как через сам класс, так и через экземпляры класса.

Вот основные особенности статических методов:
1. Нет доступа к `self` и `cls`: Поскольку статический метод не привязан ни к экземпляру, ни к классу, он не принимает автоматически аргументы self (для экземпляра) или cls (для класса).
2. Пользовательские функции: Статические методы могут использоваться для того, чтобы группировать функции, которые логически относятся к классу, даже если они не работают с его экземплярами или атрибутами класса.
3. Вызов: Их можно вызывать как через экземпляр класса, так и через сам класс.

Пример использования:
```python
class MyClass:  
    @staticmethod
    def my_static_method(x, y):  
        return x + y  

# Вызов статического метода через класс  
result1 = MyClass.my_static_method(5, 3)  
print(result1)  # Вывод: 8  

# Вызов статического метода через экземпляр класса  
obj = MyClass()  
result2 = obj.my_static_method(10, 20)  
print(result2)  # Вывод: 30  
```

Пример использования из проекта:
```python
class Common:

	@staticmethod
	def skip_if_eng_lang_and_fca_license(cur_language, cur_country):
		if cur_country == "gb" and cur_language == "":
			pytest.skip("Current test case not available for the Eng language and FCA license")
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