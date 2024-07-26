- Установка
- [Правила](#Правила)
- [Фикстуры](#Фикстуры)
- Параметризация
- [Маркеры](#Маркеры)
- [Флаги](#Флаги)
- [pytest.ini](#pytestini)

# Установка

# Правила
PyTest: правила запуска тестов 
В этом шаге мы коротко обсудим важные особенности запуска тестов с помощью PyTest. Когда мы выполняем команду pytest, тест-раннер собирает все тесты для запуска по определенным правилам:

-если мы не передали никакого аргумента в команду, а написали просто pytest, тест-раннер начнёт поиск в текущей директории

-как аргумент можно передать файл, путь к директории или любую комбинацию директорий и файлов, например: 
	pytest scripts/selenium_scripts
	# найти все тесты в директории scripts/selenium_scripts

	pytest test_user_interface.py
	# найти и выполнить все тесты в файле 

	pytest scripts/drafts.py::test_register_new_user_parametrized
	# найти тест с именем test_register_new_user_parametrized в указанном файле в указанной директории и выполнить

-дальше происходит рекурсивный поиск: то есть PyTest обойдет все вложенные директории

-во всех директориях PyTest ищет файлы, которые удовлетворяют правилу  test_*.py или *_test.py (то есть начинаются на test_ или заканчиваются _test и имеют расширение .py)

-внутри всех этих файлов находит тестовые функции по следующему правилу:
	-все тесты, название которых начинается с test, которые находятся вне классов
	-все тесты, название которых начинается с test внутри классов, имя которых начинается с Test (и без метода __init__ внутри класса)
	
Параметры pytest:
	-v (verbose, то есть подробный), то в отчёт добавится дополнительная информация со списком тестов и статусом их прохождения
	py.test test_sample.py -v  # outputs verbose messages
	py.test -q test_sample.py  # omit filename output
	python -m pytest -q test_sample.py  # calling pytest through python
	py.test --markers  # show available markers
# In order to create a reusable marker.
/*
# content of pytest.ini
[pytest]
markers =
    webtest: mark a test as a webtest.
*/

py.test -k "TestClass and not test_one"  # only run tests with names that match the "string expression"
py.test test_server.py::TestClass::test_method  # cnly run tests that match the node ID
py.test -x  # stop after first failure
py.test --maxfail=2  # stop after two failures
py.test --showlocals  # show local variables in tracebacks
py.test -l  # (shortcut)
py.test --tb=long  # the default informative traceback formatting
py.test --tb=native  # the Python standard library formatting
py.test --tb=short  # a shorter traceback format
py.test --tb=line  # only one line per failure
py.test --tb=no  # no tracebak output
py.test -x --pdb # drop to PDB on first failure, then end test session
py.test --durations=10  # list of the slowest 10 test durations.
py.test --maxfail=2 -rf  # exit after 2 failures, report fail info.
py.test -n 4  # send tests to multiple CPUs
py.test -m slowest  # run tests with decorator @pytest.mark.slowest or slowest = pytest.mark.slowest; @slowest
py.test --traceconfig  # find out which py.test plugins are active in your environment.
py.test --instafail  # if pytest-instafail is installed, show errors and failures instantly instead of waiting until the end of test suite.

# Test using parametrize
/*
    import pytest

    @pytest.mark.parametrize(
        ('n', 'expected'), [
            (1, 2),
            (2, 3),
            (3, 4),
            (4, 5),
            pytest.mark.xfail((1, 0)),
            pytest.mark.xfail(reason="some bug")((1, 0)),
            pytest.mark.skipif('sys.version_info >= (3,0)')((10, 11)),
        ]
    )
    def test_increment(n, expected):
        assert n + 1 == expected
*/

# Фикстуры

Фикстуры в контексте PyTest — это вспомогательные функции для наших тестов, которые не являются частью тестового сценария. Одно из распространенных применений фикстур — это подготовка тестового окружения и очистка тестового окружения и данных после завершения теста.

Классический способ работы с фикстурами — создание setup- и teardown-методов в файле с тестами.

# Маркеры

Маркеры в pytest - используются как теги, чтобы сказать pytest что то особенное о наших тестах. Например мы можем пометить медленные тесты @pytest.mark.slow и исключить их во время, когда мы торопимся. Или мы можем пометить тесты @pytest.mark.smoke и запускать их в первую очередь при очередном прогоне CI/CD.

**Маркеры бывают:**
- встроенные - меняют поведение, например @pytest.mark.parametrize
- пользовательские  - для выбора тестов, которые мы собираемся запускать

## Встроенные маркеры
Часто используемые:
- ```@pytest.mark.parametrize()```
- ```@pytest.mark.skip()```
- ```@pytest.mark.skipif()```
- ```@pytest.mark.xfail()```

Редко используемые:
- ```@pytest.mark.filterwarnings()```
- ```@pytest.mark.usefixtures()```


### @pytest.mark.parametrize()
- ```@pytest.mark.parametrize``` - (argnames, argvalues, indirect, ids, scope) - This marker calls a test function multiple times, passing in different arguments in turn

### @pytest.mark.skip()
- ```@pytest.mark.skip(reason=None)``` - The skip marker allows us to skip a test
```python
@pytest.mark.skip(reason="Card doesn't support < comparison yet")
def test_less_than():
c1 = Card("a task")
c2 = Card("b task")
assert c1 < c2
```

### @pytest.mark.skipif()
- ```@pytest.mark.skipif(condition, ..., *, reason)``` - This marker skips the test if any of the conditions are True.

```python
from packaging.version import parse

@pytest.mark.skipif(
    parse(cards.__version__).major < 2, reason="Card < comparison not supported in 1.x",)
def test_less_than():
    c1 = Card("a task")
    c2 = Card("b task")
    assert c1 < c2
```

### @pytest.mark.xfail()
- ```@pytest.mark.xfail(condition, ..., *, reason, run=True, raises=None, strict=xfail_strict)``` - If we want to run all tests, even those that we know will fail, we can use the xfail marker. This marker tells pytest that we expect the test to fail
    -  ```run=False``` - The test is run anyway, by default, but the run parameter can be used to tell pytest to not run the test by setting run=False
    -  ```raises``` - allows you to provide an exception type or a tuple of exception types that you want to result in an xfail. 
    - ```strict``` - tells pytest if passing tests should be marked as XPASS (strict=False) or FAIL, strict=True.  

Status of xfail:
- XFAIL - когда тест не прошел. “You were right, it did fail,” 
- XPASS - когда тест прошел (with no strict setting). “Good news, the test you
thought would fail just passed.” 
- FAILED - когда тест прошел, но strict=true. “You thought it would fail, but it didn’t. You were wrong.”

```python
@pytest.mark.xfail(
parse(cards.__version__).major < 2,
reason="Card < comparison not supported in 1.x",
)
def test_less_than():
c1 = Card("a task")
c2 = Card("b task")
assert c1 < c2
@pytest.mark.xfail(reason="XPASS demo")
def test_xpass():
c1 = Card("a task")
c2 = Card("a task")
assert c1 == c2
@pytest.mark.xfail(reason="strict demo", strict=True)
def test_xfail_strict():
c1 = Card("a task")
c2 = Card("a task")
assert c1 == c2
```

## Пользовательские маркеры
Custom markers are markers we make up ourselves and apply to tests. Think of them like tags or labels. Custom markers can be used to select tests to run or skip.

Пользовательские маркеры необходимо регистровать в файле pytest.ini, иначе будет появляться предупредительное сообщение о неизвестном маркере. 

### @pytest.mark.filterwarnings()
- ```@pytest.mark.filterwarnings(warning)``` - 

### @pytest.mark.usefixtures()
- ```@pytest.mark.usefixtures(fixturename1, fixturename2, ...)``` - This marker marks tests as needing all the specified fixtures.

## Маркировка файлов, классов и параметров
```python
import pytest
from cards import Card, InvalidCardId
pytestmark = pytest.mark.finish
```
If pytest sees a `pytestmark` attribute in a test module, it will apply the marker(s) to all the tests in that module. If you want to apply more than one marker to the file, you can use a list form: `pytestmark = [pytest.mark.marker_one, pytest.mark.marker_two]`.

Another way to mark multiple tests at once is to have tests in a class and use class-level markers:
```python
@pytest.mark.smoke
class TestFinish:
def test_finish_from_todo(self, cards_db):
i = cards_db.add_card(Card("foo", state="todo"))
cards_db.finish(i)
c = cards_db.get_card(i)
assert c.state == "done"
def test_finish_from_in_prog(self, cards_db):
i = cards_db.add_card(Card("foo", state="in prog"))
cards_db.finish(i)
c = cards_db.get_card(i)
assert c.state == "done
```
The test class TestFinish is marked with @pytest.mark.smoke. Marking a test class like this effectively marks each test method in the class with the same marker. 

# Флаги
- -v --verbose - 
- -r -tells pytest to report reasons for different test results at the end of the session
- -ra - The a in -ra stands for “all except passed.” The -ra flag is therefore the most useful, as we almost always want to know the reason why certain tests did not pass.
- -rfE - The default display is the same as passing in. f for failed tests; E for errors
- -a
- -m для запуска маркированных тестов,  `pytest -v -m exception test_start.py` - означает, запустить тесты с маркером `exception`
- --tb=short

# pytest.ini
pytest.ini - the main configuration file for pytest.
```python
[pytest]
markers =
    smoke:  subset of tests
```