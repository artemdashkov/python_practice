- [Install](#install)
- [Modules](#modules)
	- [black](#black)
	- [flake8](#flake8)
	- [os](#os)
- [Работа с виртуальным окружением](#работа-с-виртуальным-окружением)
- [assert](#assert)
- [in](#in)
- [pickle](#pickle)

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


	

## poetry
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


$ pip install allure-pytest
$ py.test --alluredir=%allure_result_folder% ./tests
$ allure serve %allure_result_folder%
https://pypi.org/project/allure-pytest/

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

При выявлении несоответствии в методе assert дальнейший код не выполняется, например:
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

# random
```python
import random

""" Функции randint и randrange возвращают целое число. """
number = random.randint(l, 10) # вернет значение от 1 до 10, оба значения включены в диапазон
number = random.randrange(lO) # вернет значение от 0 до 9
number = random.randrange(5,10) # вернет значение от 5 до 9
number = random.randrange(1, 102, 10) # вернет одно значение из списка [1, 11, 21, 31, 41, 51, 61, 71, 81, 91, 101]

""" Функции random возвращает случайное число с плавающей точкой."""
number = random.random() # возвращает случайное число с плавающей точкой в диапазоне от 0.0 до 1.0 (но исключая 1.0)

""" Функции uniform тоже возвращает случайное число с плавающей точкой,  но при этом она позволяет задавать диапазон значений, из которого следует отбирать значения. """
number = random.uniform(1.0, 10.0) # возвращает случайное число с плавающей точкой в диапазоне от 1.0 до 10.0 
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
