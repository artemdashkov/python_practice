## Install
```python
apt install python3 # Установка Python в linux
```

```python
#!/usr/bin/python3  -строка в файле линукс, которая позволяет интерпретировать файл как python файл
```

## Versions
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
	

======РАБОТА С ВИРТУАЛЬНЫМ ОКРУЖЕНИЕМ======
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