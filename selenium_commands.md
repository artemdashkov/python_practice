# ПОДГОТОВКА СРЕДЫ / УСТАНОВКА ПАКЕТОВ

https://sites.google.com/chromium.org/driver/downloads
ссылка на драйвер для chrome

- **python -m venv selenium_env** - Создадим виртуальное окружение selenium_env:
- **selenium_env\Scripts\activate.bat** - Запустим созданный для нас приложением venv файл activate.bat, чтобы активировать окружение
- **pip3 install selenium webdriver-manager** - установка selenium и webdriver-manager (позволяет автоматически скачивать самую актуальную версию драйвера для того или иного браузера, без каких-либо дополнительных скачиваний)
- **pip3 list** -перечень всех установленных библиотек

- **pip3 freeze > requirements.txt** - создает в папке venv файл requirements.txt с перечнем всех исользуемых в проекте библиотек

# ИМПОРТЫ / КЛАССЫ
- **from selenium import webdriver** - webdriver это и есть набор команд для управления браузером
- **from webdriver_manager.chrome import ChromeDriverManager** - драйвер для хром
- **from selenium.webdriver.chrome.service import Service** - класс Service отвечает за установку драйвера, за его открытие и его закрытие
- **from selenium.webdriver.common.by import By** # импортируем класс By, который позволяет выбрать способ поиска элемента
- **import time**

# ИНИЦИАЛИЗАЦИЯ
- **driver = webdriver.Chrome()** - инициализируем драйвер браузера. После этой команды вы должны увидеть новое открытое окно браузера

1. Иницализация в одну строчку, через SeleniumManager - он автоматически распознает ваш Chrome и запустит его. Но важно перед этим найти у себя на компьютере через поиск chromedriver.exe и удалить его!
```python
from selenium import webdriver
driver = webdriver.Chrome()
```
2. Инициализация через WebDriverManager
```python
from webdriver_manager.chrome import ChromeDriverManager
from selenium.webdriver.chrome.service import Service
service = Service(ChromeDriverManager().install())
driver = webdriver.Chrome(service=service)
```
3. Инициализация через WebDriverManager + версия Chrome
   from webdriver_manager.chrome import ChromeDriverManager
   from selenium.webdriver.chrome.service import Service
   service = Service(ChromeDriverManager(driver_version="114.0.5735.16").install())
   driver = webdriver.Chrome(service=service)

# НАВИГАЦИЯ
driver.get("https://suninjuly.github.io/text_input_task.html")
Метод get сообщает браузеру, что нужно открыть сайт по указанной ссылке
driver.back()
метод back говорит браузеру вернуться на предыдущую страницу. если ссылка откралась в новом окне, то возврата на предыдущую страницу не произойдет
driver.forward()
метод forward возвращает нас на одну страницу вперед (это та, с которой мы вернулись назад...)

driver.refresh()
метод refresh обновляет страницу

# ПОЛУЧЕНИЕ ДАННЫХ ИЗ БРАУЗЕРА
driver.current_url
атрибут current_url выводит данные о текущем URL
driver.current_title
атрибут current_title возвращает тайтл текущей страницы
driver.page_source
получить код всей страницы. используется в автоматизации для парсинга и сравнения данных
===========ВАЛИДАЦИЯ ДАННЫХ===========
assert url == "https://www.wikipedia.org/", "адреса не равны"
метод assert позволяет сравнить данные, в случае несоответствия вывод сообщение в кавычках

# ПОИСК
Для поиска элементов на странице в Selenium WebDriver используются несколько стратегий, позволяющих искать:
- по атрибутам или тегам элементов: ID, CLASS_NAME (например "nav-link"), TAG_NAME (например "h2").
- текстам в ссылках,
- CSS-селекторам - Поиск с помощью CSS-селекторов, является наиболее удобным способом, т.к. он покрывает практически все возможные ситуации, и CSS-селекторы выглядят более читабельными.
- XPath-селекторам.

### Для поиска Selenium предоставляет методы:
- **find_element** - если метод не смог найти элемент на странице, то он вызовет ошибку NoSuchElementException, которая прервёт выполнение вашего кода
- **find_elements** - всегда возвращает валидный результат: если ничего не было найдено, то он вернёт пустой список и ваша программа перейдет к выполнению следующего шага в коде

которые принимают два аргумента:

1. Способ поиска - тип локатора
2. Значение атрибута - локатора.

### Существуют следующие методы поиска элементов:
- **find_element(By.ID, value)** — поиск по уникальному атрибуту id элемента. Если ваши разработчики проставляют всем элементам в приложении уникальный id, то вам повезло, и вы чаще всего будет использовать этот метод, так как он наиболее стабильный;
- **find_element(By.CSS_SELECTOR, value)** — поиск элемента с помощью правил на основе CSS. Это универсальный метод поиска, так как большинство веб-приложений использует CSS для вёрстки и задания оформления страницам. Если find_element_by_id вам не подходит из-за отсутствия id у элементов, то скорее всего вы будете использовать именно этот метод в ваших тестах;
- **find_element(By.XPATH, value)** — поиск с помощью языка запросов XPath, позволяет выполнять очень гибкий поиск элементов;
- **find_element(By.NAME, value)** — поиск по атрибуту name элемента;
- **find_element(By.TAG_NAME, value)** — поиск элемента по названию тега элемента;
- **find_element(By.CLASS_NAME, value)** — поиск по значению атрибута class;
- **find_element(By.LINK_TEXT, value)** — поиск ссылки на странице по полному совпадению;
- **find_element(By.PARTIAL_LINK_TEXT, value)** — поиск ссылки на странице, если текст селектора совпадает с любой частью текста ссылки.

### Список обозначений для использования вместо By:
- ID = "id"
- XPATH = "xpath"
- LINK_TEXT = "link text"
- PARTIAL_LINK_TEXT = "partial link text"
- NAME = "name"
- TAG_NAME = "tag name"
- CLASS_NAME = "class name"
- CSS_SELECTOR = "css selector"

## ID
```html
<div class="col-sm-4">
  <div class="card mb-4 box-shadow">
    <img id="bullet" name="bullet-cat" data-type="animal" class="card-img-top" src="images/bullet_cat.jpg">
  </div>
</div>
```
#bullet (знак # означает, что мы ищем по id со значением bullet).
```python
login_selector = (By.ID, '#login-desktop')
```

## TAG
Чтобы найти элемент по тегу, просто напишите название тега в поисковой строке, как мы делали это при поиске по id (только без знака #), например, h1. Поиск по h1 найдёт для нас элемент с названием страницы. Поиск по тегам не очень удобен, т.к. разработчики используют небольшое количество тегов для разметки страниц, и скорее всего, одному тегу будет соответствовать множество элементов.

## Поиск по значению атрибута
```html
<div class="col-sm-4">
  <div class="card mb-4 box-shadow">
    <img id="bullet" name="bullet-cat" data-type="animal" class="card-img-top" src="images/bullet_cat.jpg">
  </div>
</div>
```
Можно найти элемент, указав название атрибута и его значение. Например, можно переписать поиск по id в следующем виде [id="bullet"] вместо #bullet.

Лучше использовать вариант с квадратными скобками при поиске значения атрибута для тех атрибутов, у которых нет собственных коротких команд поиска. Например, давайте найдем элемент h1 по значению его атрибута value: [value="Cat memes"].

## NAME
```html
<div class="col-sm-4">
  <div class="card mb-4 box-shadow">
    <img id="bullet" name="bullet-cat" data-type="animal" class="card-img-top" src="images/bullet_cat.jpg">
  </div>
</div>
```
Этот вариант поиска является разновидностью поиска по значению атрибута и записывается так же: [name="bullet-cat"]. Мы выделяем этот вариант потому что он довольно часто используется, а также выделяется как отдельный вид поиска элементов в Selenium WebDriver.

## CLASS_NAME
Поиск по классу можно записать в виде [class="jumbotron-heading"], так как class тоже является атрибутом элемента. Но раз уж классы используются практически в каждой странице при задании стилей страниц, то для них также имеется свой короткий вариант поиска: .jumbotron-heading. То есть мы пишем значение класса и предваряем его точкой.

Давайте рассмотрим важную разницу между двумя способами поиска по классу. Допустим, у элемента article задано больше одного класса:
```html
<article id="moto" class="lead text-muted" title="one-thing" name="moto">If there's one thing that the internet was made for, it's funny cat memes.</article>
```
Вариант [class="lead"] не найдет нам этот элемент, так как он ищет по точному совпадению. Чтобы найти элемент, нам нужно будет написать [class="lead text-muted"], порядок классов при этом важен. [class="text-muted lead"] — уже не найдет искомый элемент.

Вариант .lead при этом позволит найти данный элемент, так как он ищет простое вхождение класса в элемент. Для уточнения селектора можно задать также оба класса, для этого нужно добавить второй класс к строке поиска без пробела и предварить его точкой: .lead.text-muted. Порядок классов в отличие от первого способа здесь не важен — .text-muted.lead так же найдет нужный элемент. Рекомендуем пользоваться вторым способом поиска классов, так как он является более гибким

## By.XPATH/XPath-селекторам
XPATH (XML Path Language) - это язык запросов к элементам XML, HTML документов, и других документов класса xml.
Основные символы, которые используются в XPATH-локаторах:
// - глобальный поиск относительно корня (начала) документа (обычно корень - это html тег)
/ - поиск по уровню вложенности, например когда элемент внутри элемента

//employee -найдет глобально все теги employee
//employee/name -найдет глобально employee и вложенный именно в него name
//header//div[2] -найдет каждый второй div лежащий внутри header
(//header//div)[2] -сначала скобки, вернут нам список всех div-блоков внутри header, а потом мы получаем второй div из полученного списка

// элемент [ @атрибут = ’значение атрибута’ ] -синтаксис поиска по атрибутам. Исключением является text, в нем не используется знак @, а дописываются круглые скобки, text()=’текст’, потому что это не атрибут, а параметр!, Примеры:
//div[@class=’table’] - по классу
//employee[@id=’2’] - по id
//employee[@id=’1’]/name[text()=’David’] - по тексту
//name[text()=’John’] - по тексту

//элемент [содержит (@атрибут, ‘значение содержимого’) ] -синтаксис поиска по содержимому, используется когда у элемента прописано несколько классов
например для:
<button class="btn btn-white">Join</button>
//button[contains(@class, 'btn')] или
//button[contains(text(), 'Join')]
and - используется, когда нужно выполнять поиск по нескольким аргументам
//button[@data-cy='submitButton' and @type='submit']

## СОСТАВНЫЕ CSS-СЕЛЕКТОРЫ
Пример:
```html
<div id="posts" class="post-list">
  <div id="post1" class="item">
    <div class="title">Как я провел лето</div>
    <img src="./images/summer.png">
  </div>
  <div id="post2" class="item">
    <div class="title second">Ходили купаться</div>
    <img src="./images/bad_dog.jpg">
  </div>
  <div id="post3" class="item">
    <div class="title">С друзьями</div>
    <img src="./images/friends.jpg">
  </div>
</div>
```
## Использование потомков
```css
#post2 .title
```
Здесь символ # означает, что надо искать элемент с id post2, пробел - что также нужно найти элемент-потомок, а ., что элемент-потомок должен иметь класс со значением title.

    Элемент .title называется потомком (англ. descendant) элемента #post2. Потомок может находиться на любом уровне вложенности, все элементы с селектором .title также являются и потомками элемента #posts, хотя и расположены от него на два уровня ниже. #posts .title найдет все 3 элемента с классом title.

    !Внимание. Символ пробела " " является значащим в CSS-селекторах. Это важный символ, который разделяет описание предка и потомка. Если бы мы записали селектор #post2.title без пробела, то в данном примере не было найдено ни одного элемента. Такая запись означала бы, что мы хотим найти элемент, который одновременно содержит id "post2" и класс "title". Таким образом #post2 .title и #post2.title — это разные селекторы.

## Использование дочерних элементов

#post2 > div.title
Здесь мы указали еще тег элемента div и уточнили, что нужно взять элемент с тегом и классом: div.title, который находится строго на один уровень иерархии ниже чем элемент #post2. Для этого используется символ >
Элемент #post2 в этом случае называется родителем (англ. parent) для элемента div.title, а элемент div.title называется дочерним элементом (англ. child) для элемента #post2. Если символа > нет, то будет выполнен поиск всех элементов div.title на любом уровне ниже первого элемента.
=====Использование порядкового номера дочернего элемента=====

#posts > .item:nth-child(2) > .title
Псевдо-класс :nth-child(2) — позволяет найти второй по порядку элемент среди дочерних элементов для #posts. Затем с помощью конструкции > .title мы указываем, что нам нужен элемент .title, родителем которого является найденный ранее элемент .item.

## Использование нескольких классов

.title.second
Также мы можем использовать сразу несколько классов элемента, чтобы его найти. Для этого классы записываются подряд через точку



textarea = driver.find_element(By.CSS_SELECTOR, ".textarea")
Ищем поле для ввода текста. Метод find_element позволяет найти нужный элемент на сайте, указав путь к нему. Метод принимает в качестве аргументов способ поиска и значение, по которому мы будем искать

textarea.send_keys("get()") # Напишем текст ответа в найденное поле

submit_button = driver.find_element(By.CSS_SELECTOR, ".submit-submission") # Найдем кнопку, которая отправляет введенное решение

submit_button.click() # Скажем драйверу, что нужно нажать на кнопку. После этой команды мы должны увидеть сообщение о правильном ответе

# находим элемент, содержащий текст

welcome_text_elt = browser.find_element(By.TAG_NAME, "h1")

# записываем в переменную welcome_text текст из элемента welcome_text_elt

welcome_text = welcome_text_elt.text

Метод get_attribute
people_radio = browser.find_element(By.ID, "peopleRule")
Найдём атрибут "checked" с помощью встроенного метода get_attribute и проверим его значение:
people_checked = people_radio.get_attribute("checked")
print("value of people radio: ", people_checked)

# После выполнения всех действий мы должны не забыть закрыть окно браузера

driver.quit()

Работа со списками
from selenium.webdriver.support.ui import Select
select = Select(browser.find_element(By.TAG_NAME, "select"))
select.select_by_value("1") # ищем элемент с текстом "Python"

# Загрузка файлов
Если нам понадобится загрузить файл на веб-странице, мы можем использовать уже знакомый нам метод send_keys. Только теперь нам нужно в качестве аргумента передать путь к нужному файлу на диске вместо простого текста
import os

    current_dir = os.path.abspath(os.path.dirname(__file__))    # получаем путь к директории текущего исполняемого файла
    file_path = os.path.join(current_dir, 'file.txt')           # добавляем к этому пути имя файла
    element.send_keys(file_path)

# Ожидания

## Неявные ожидания - Selenium Waits (Implicit Waits)
Решение с time.sleep() плохое: оно не масштабируемое и трудно поддерживаемое.

Идеальное решение могло бы быть таким: нам всё равно надо избежать ложного падения тестов из-за асинхронной работы скриптов или задержек от сервера, поэтому мы будем ждать появление элемента на странице в течение заданного количества времени (например, 5 секунд). Проверять наличие элемента будем каждые 500 мс. Как только элемент будет найден, мы сразу перейдем к следующему шагу в тесте. Таким образом, мы сможем получить нужный элемент в идеальном случае сразу, в худшем случае за 5 секунд.

В Selenium WebDriver есть специальный способ организации такого ожидания, который позволяет задать ожидание при инициализации драйвера, чтобы применить его ко всем тестам. Ожидание называется неявным (Implicit wait), так как его не надо явно указывать каждый раз, когда мы выполняем поиск элементов, оно автоматически будет применяться при вызове каждой последующей команды.

Улучшим наш тест с помощью неявных ожиданий. Для этого нам нужно будет убрать time.sleep() и добавить одну строчку с методом implicitly wait:
```python
from selenium import webdriver
from selenium.webdriver.common.by import By

browser = webdriver.Chrome()
# говорим WebDriver искать каждый элемент в течение 5 секунд
browser.implicitly_wait(5)

browser.get("http://suninjuly.github.io/wait1.html")
```
Теперь мы можем быть уверены, что при небольших задержках в работе сайта наши тесты продолжат работать стабильно. На каждый вызов команды find_element WebDriver будет ждать 5 секунд до появления элемента на странице прежде, чем выбросить исключение NoSuchElementException.

## Явное ожидание - Explicit Waits (WebDriverWait и expected_conditions)

Методы **find_element** проверяют только то, что элемент появился на странице. В то же время элемент может иметь дополнительные свойства, которые могут быть важны для наших тестов, например:

- Кнопка может быть неактивной, то есть её нельзя кликнуть;
- Кнопка может содержать текст, который меняется в зависимости от действий пользователя. Например, текст "Отправить" после нажатия кнопки поменяется на "Отправлено";
- Кнопка может быть перекрыта каким-то другим элементом или быть невидимой.

Explicit Waits (WebDriverWait и expected_conditions)
Чтобы тест был надежным, нам нужно не только найти кнопку на странице, но и дождаться, когда кнопка станет кликабельной. Для реализации подобных ожиданий в Selenium WebDriver существует понятие явных ожиданий (Explicit Waits), которые позволяют задать специальное ожидание для конкретного элемента. Задание явных ожиданий реализуется с помощью инструментов WebDriverWait и expected_conditions.

Улучшим наш тест:

```python
from selenium.webdriver.common.by import By
from selenium.webdriver.support.ui import WebDriverWait
from selenium.webdriver.support import expected_conditions as EC
from selenium import webdriver

browser = webdriver.Chrome()

browser.get("http://suninjuly.github.io/wait2.html")

# говорим Selenium проверять в течение 5 секунд, пока кнопка не станет кликабельной
button = WebDriverWait(browser, 5).until(
    EC.element_to_be_clickable((By.ID, "verify"))
)
button.click()
message = browser.find_element(By.ID, "verify_message")
```

В модуле expected_conditions есть много других правил, которые позволяют реализовать необходимые ожидания:

- title_is
- title_contains
- presence_of_element_located
- visibility_of_element_located
- visibility_of
- presence_of_all_elements_located
- text_to_be_present_in_element
- text_to_be_present_in_element_value
- frame_to_be_available_and_switch_to_it
- invisibility_of_element_located
- element_to_be_clickable
- staleness_of
- element_to_be_selected
- element_located_to_be_selected
- element_selection_state_to_be
- element_located_selection_state_to_be
- alert_is_present

Описание каждого правила можно найти на [сайте](https://selenium-python.readthedocs.io/api.html#module-selenium.webdriver.support.expected_conditions).

Если мы захотим проверять, что кнопка становится неактивной после отправки данных, то можно задать негативное правило с помощью метода until_not:
```python
# говорим Selenium проверять в течение 5 секунд пока кнопка станет неактивной
button = WebDriverWait(browser, 5).until_not(
        EC.element_to_be_clickable((By.ID, "verify"))
    )
```
# Исключения - Exceptions
- **NoSuchElementException** - если элемент не был найден за отведенное время
- **StaleElementReferenceException** - если элемент был найден в момент поиска, но при последующем обращении к элементу DOM изменился. Например, мы нашли элемент Кнопка и через какое-то время решили выполнить с ним уже известный нам метод click. Если кнопка за это время была скрыта скриптом, то метод применять уже бесполезно — элемент "устарел" (stale) и мы увидим исключение.
- **ElementNotVisibleException** - если элемент был найден в момент поиска, но сам элемент невидим (например, имеет нулевые размеры), и реальный пользователь не смог бы с ним взаимодействовать, то получим
- **ElementClickInterceptedException** - элеммент перекрыт другим элементом, например всплывающим окном.
