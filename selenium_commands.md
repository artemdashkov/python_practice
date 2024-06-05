# Содержание:

- [Подготовка среды / установка пакетов](#подготовка-среды--установка-пакетов)
- [Импорты/ классы](#импорты--классы)
- [chrome_options/ options](#chrome_options)
- [prefs](#prefs)
- [Инициализация](#инициализация)
- [Навигация](#навигация)
- [Получение данных из браузера](#получение-данных-из-браузера)
- [Cookies](#cookies)
- [Checkboxes - Radiobutton](#checkboxes---radiobutton)
- [Поиск](#поиск)
    - [ID](#id)
    - [TAG](#tag)
    - [Поиск по значению атрибута](#поиск-по-значению-атрибута)
    - [NAME](#name)
    - [CLASS_NAME](#class_name)
    - [XPATH](#xpath)
    - [Составные CSS-селекторы](#составные-css-селекторы)
      - [Использование потомков](#использование-потомков)
      - [Использование дочерних элементов](#использование-дочерних-элементов)
      - [Использование порядкового номера дочернего элемента](#использование-порядкового-номера-дочернего-элемента)
      - [Использование нескольких классов](#использование-нескольких-классов)
      - [Регулярные выражения](#регулярные-выражения)
- [Методы](#методы)
    - [clear()](#clear)
    - [execute_script()](#execute_script)
    - [find_element()](#find_element)
    - [find_elements()](#find_elements)
    - [get_attribute()](#get_attribute)
    - [get_property()](#get_property)
    - [getText()](#getText)
    - [get_window_position()](#get_window_position)
    - [set_window_position()](#set_window_position)
    - [save_screenshot()](#save_screenshot)
    - [send_keys()](#send_keys)
    - [set_window_size()](#set_window_size)
    - [maximize_window()](#maximize_window)
- [Загрузка файлов](#загрузка-файлов)
- [Ожидания](#ожидания)
    - [Неявные ожидания - Implicit Waits](#неявные-ожидания---implicit-waits)
    - [Явное ожидание - Explicit Waits](#явное-ожидание---explicit-waits)
- [Работа с окнами и вкладками](#работа-с-окнами-и-вкладками)
- [iframe](#iframe)
- [Alert](#alert)
- [Исключения - Exceptions](#исключения---exceptions)
- [Action chains](#action-chains)
- [Drag and Drop](#drag-and-drop)


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

# chrome_options
Опции браузера - это по сути его настройки перед запуском (возможности браузера).
```python
from selenium.webdriver.chrome.options import Options

chrome_options = webdriver.ChromeOptions() # создаем объект опций, до инициализации драйвера.
chrome_options = Options() # Или создается вот так

chrome_options.add_argument('--headless') # объявляем опции, используя специальные методы
chrome_options.add_argument('--incognito')

options.add_argument("--disable-blink-features=AutomationControlled")

options.add_argument("--user-agent=Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/115.0.0.0 Safari/537.36")

options.add_argument("--user-agent=Mozilla/5.0 (Linux; Android 13; SAMSUNG SM-S918B) AppleWebKit/537.36 (KHTML, like Gecko) SamsungBrowser/21.0 Chrome/110.0.5481.154 Mobile Safari/537.36") # с данным user-agent браузер будет думать, что зашли с телефона и верстка сайта изменится

driver = webdriver.Chrome(service=service, options=chrome_options) # передаем опции драйверу через артибут options=

prefs = {
    'download.default_directory': f'{os.getcwd()}\\downloads'
}
chrome_options.add_experimental_option('prefs', prefs) # add_experimental_option() - опции, которые не доступны из коробки, "prefs" - зарезервированное имя, которое бует подтягивать настройки
```
другой способ инициализации
```python
from selenium.webdriver.chrome.options import Options
from selenium.webdriver.chrome.service import Service

options = Options()
options.add_argument("--window-size=1280,800")
service = Service(ChromeDriverManager().install())

driver = webdriver.Chrome(service=service, options=options)
```

Опции `chrome_options.add_argument('--xxx') `:
- `--disable-blink-features=AutomationControlled` - отключить режим автоматизации (WebDriver) 
- `--headless` - запускает браузер в режиме без графического интерфейса. Это позволяет выполнять тесты в фоновом режиме без отображения окна браузера, позволяет ускорить процесс автоматизации, использует меньше ресурсов процессора
- `--incognito` - запуск браузера в режиме инкогнито позволяет не использовать кэш и не сохранять данные, что является чистым тестированием
- `--user-agent=` - User agent – это программный элемент браузера (строка), обозначающий юзера, по сути некий идентификатор, который позволяет браузеру идентифицировать нас, как пользователя. https://useragents.ru/stable.html - ПК агенты
- `--ignore-certificate-errors` - игнорирование SSL сертификата, позволяет обходить ошибки связанные с SSL сертификатами (например отсуствие SSL сертификата или он закончился)
- `--window-size=1280,800` - опция в качестве аргумента принимает размер экрана, в selenium есть метод, который позволяет после инициализации драйвера также устанавливать разрешение экрана `driver.set_window_size(1280, 720)`. При использовании опции браузер сразу открывается в заданном размере
- `--disable-cache` - кэш не будет записываться, все ресурсы, скрипты и картинки всегда будут загружаться по новой не подгружаясь из кэша

## Стратегия загрузки страницы
```python
chrome_options.page_load_strategy = 'normal' # стандартная загрузка страницы, которая выполняется по умолчанию, ожидает загрузки всех ресурсов (картинки, js-код, шрифты и т.д). такую опцию можно и не устанавливать. 

chrome_options.page_load_strategy = 'eager' # данная опция загружает только DOM (html-структуру)

chrome_options.page_load_strategy = 'none' # вообще ничего не ждет, не понятно для чего нужен
```

# prefs
prefs (от "preference" - предпочтения) - специфичные настройки браузера, которых нет их коробки.
```python
prefs = {
    'download.default_directory': f'{os.getcwd()}\\downloads'
} # позволяет скачать файлы в выбранную директорию
chrome_options.add_experimental_option('prefs', prefs) # add_experimental_option() - опции, которые не доступны из коробки, "prefs" - зарезервированное имя, которое бует подтягивать настройки
```
ссылка на доступные prefs (их очень много!):
https://chromium.googlesource.com/chromium/src/+/32352ad08ee673a4d43e8593ce988b224f6482d3/chrome/common/pref_names.cc

например:
```cc
// String which specifies where to download files to by default.
const char kDownloadDefaultDirectory[] = "download.default_directory";
```

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
```python
driver.get("https://suninjuly.github.io/text_input_task.html") # Метод get сообщает браузеру, что нужно открыть сайт по указанной ссылке
```

```python
driver.back() # метод back говорит браузеру вернуться на предыдущую страницу. если ссылка откралась в новом окне, то возврата на предыдущую страницу не произойдет
```

```python
driver.forward() # метод forward возвращает нас на одну страницу вперед (это та, с которой мы вернулись назад...)
```

```python
driver.refresh() # метод refresh обновляет страницу
```

# ПОЛУЧЕНИЕ ДАННЫХ ИЗ БРАУЗЕРА
- `driver.current_url` - выводит данные о текущем URL
- `driver.title` - атрибут title возвращает тайтл текущей страницы
- `driver.page_source` получить код всей страницы. используется в автоматизации для парсинга и сравнения данных

# Cookies
**Куки (cookies)** - это небольшие текстовые файлы, которые веб-сайты сохраняют на вашем компьютере, когда вы их посещаете. Когда вы снова посещаете тот же веб-сайт, ваш браузер отправляет эти файлы на сервер, чтобы веб-сайт мог "вспомнить" некоторую информацию о вас.
```python
# получить куки для словаря со значением с name "country_code"
driver.get_cookie("country_code")
print(driver.get_cookie("country_code"))
>>> {'domain': 'www.freeconferencecall.com', 'httpOnly': True, 'name': 'country_code', 'path': '/', 'sameSite': 'Lax', 'secure': True, 'value': 'us'}
```

```python
# добавление cookies
driver.add_cookie({
    "name": "Example",
    "value": "Kukushka"
})
```

```python
# для замены cookies необходимо сначала удалить существующую куку и на ее место добавить новую куку с тем же именем
driver.delete_cookie("split")

driver.add_cookie({
    "name": "split",
    "value": "QWERTY"
})
```

```python
import pickle
# сохранение cookies в файл и загрузка из файла

pickle.dump(
    driver.get_cookies(),
    open(os.getcwd()+"/cookies/cookies.pkl", "wb")
    )

cookies = pickle.load(open(os.getcwd()+"/cookies/cookies.pkl", "rb"))

"""
перед добавлением/обновлением куков нужно их удалить,
иначе произодет дублирование куков и новые куки не сработают.
"""

driver.delete_all_cookies()
for cookie in cookies:
    driver.add_cookie(cookie)

driver.refresh() # чтобы куки применились
```

### Методы cookies:
- `get_cookie("country_code")` - получить какие-то конкретные куки, где `country_code` это name cookies 
- `get_cookies()` - получить все куки
- `.add_cookie({"": ""})` - добавить куки, принимает словарь
- `driver.delete_cookie("split")` - удалить куки с name split
- `driver.delete_all_cookies()` - удалить все куки

# Checkboxes - Radiobutton
### Checkboxes
Чек-бокс / флажок - это один из самых распространенных элементов в веб-формах, например, в тестах с вариантами ответа или подтверждение согласия на что-либо.

Но с развитием сферы веб-разработки он превратился в нечто большее и начал использоваться буквально где угодно.

**Например**:
- Выбор карточки
- Слайды (Toggle)
- Стандартный чекбокс

```python
# установка чек-бокса
driver.get("https://the-internet.herokuapp.com/checkboxes")
CHECKBOX_1 = driver.find_element(*CHECKBOX_1_LOCATOR)
CHECKBOX_1.click()
```

```python
# Проверка статуса чек-бокса - способ 1
print(CHECKBOX_1.get_attribute("checked")) # >>> None
CHECKBOX_1.click()
print(CHECKBOX_1.get_attribute("checked")) # >>> true
print(type(CHECKBOX_1.get_attribute("checked"))) # >>> <class 'str'>
```

```python
# Проверка статуса чек-бокса - способ 2
print(CHECKBOX_1.is_selected()) # >>> False
CHECKBOX_1.click()
print(CHECKBOX_1.is_selected()) # >>> True
print(type(CHECKBOX_1.is_selected())) # >>> <class 'bool'>
```

```python
# Особенности работы #1, например когда элемент перекрыт создается два элемента
CHECKBOX_HOME_STATUS_LOCATOR = ('xpath', '//input[@id="tree-node-home"]') # для получения статуса
CHECKBOX_HOME_ACTION_LOCATOR = ('xpath', '//span[@class="rct-checkbox"]') # для взимодействия

driver.get("https://demoqa.com/checkbox")

CHECKBOX_HOME_STATUS = driver.find_element(*CHECKBOX_HOME_STATUS_LOCATOR)
CHECKBOX_HOME_ACTION = driver.find_element(*CHECKBOX_HOME_ACTION_LOCATOR)

print(CHECKBOX_HOME_STATUS.is_selected()) # >>> False
CHECKBOX_HOME_ACTION.click()
print(CHECKBOX_HOME_STATUS.is_selected()) # >>> True
```

```python
# Особенности работы #2, когда нет тега input нужно обращать внимание на изменение в классе при клике на элемент, который ведет себя как чек-бокс
driver.get("https://demoqa.com/selectable")

status_1 = driver.find_element(*ELEMENT_ONE_LOCATOR).get_attribute("class")
print(status_1) # >>> mt-2 list-group-item list-group-item-action
driver.find_element(*ELEMENT_ONE_LOCATOR).click()

status_2 = driver.find_element(*ELEMENT_ONE_LOCATOR).get_attribute("class")
print(status_2) # >>> mt-2 list-group-item active list-group-item-action
assert "active" in status_2, "active not in status"
```

**Методы:**
- `click()` - метод для клика по чек-боксу
- `is_selected()` - проверяет статус чек-бокса
- `is_enabled()` - проверяет доступность элемента

### Radiobutton
Взаимодействие с Radiobutton аналогичное как и с Checkbox. Аналогичные особенности.
```python
YES_STATUS_LOCATOR = ("xpath", '//input[@id="yesRadio"]')
YES_ACTION_LOCATOR = ("xpath", '//label[@for="yesRadio"]')

driver.get("https://demoqa.com/radio-button")

print(driver.find_element(*YES_STATUS_LOCATOR).is_selected()) # >>> False
driver.find_element(*YES_ACTION_LOCATOR).click()
print(driver.find_element(*YES_STATUS_LOCATOR).is_selected())  # >>> True
```

```python
# Особенность работы с неактивным Radiobutton
NO_STATUS_LOCATOR = ("xpath", '//input[@id="noRadio"]')
NO_ACTION_LOCATOR = ("xpath", '//label[@for="noRadio"]')

driver.get("https://demoqa.com/radio-button")
print(driver.find_element(*NO_STATUS_LOCATOR).is_enabled()) # >>> False
```

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

### Методы поиска элементов:
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
Пример кода:
```html
<div class="col-sm-4">
  <div class="card mb-4 box-shadow">
    <img id="bullet" name="bullet-cat" data-type="animal" class="card-img-top" src="images/bullet_cat.jpg">
  </div>
</div>
```
Пример селектора на основе кода выше:
```python
login_selector = (By.ID, 'login-desktop')
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

## XPATH
XPATH (XML Path Language) - это язык запросов к элементам XML, HTML документов, и других документов класса xml.
Основные символы, которые используются в XPATH-локаторах:
- // - глобальный поиск относительно корня (начала) документа (обычно корень - это html тег)
- / - поиск по уровню вложенности, например когда элемент внутри элемента
- //employee -найдет глобально все теги employee
- //employee/name -найдет глобально employee и вложенный именно в него name
- //header//div[2] -найдет каждый второй div лежащий внутри header
- (//header//div)[2] -сначала скобки, вернут нам список всех div-блоков внутри header, а потом мы получаем второй div из полученного списка

```python
// элемент [ @атрибут = ’значение атрибута’ ] #синтаксис поиска по атрибутам.
```
  Исключением является text, в нем не используется знак @, а дописываются круглые скобки, text()=’текст’, потому что это не атрибут, а параметр!, Примеры:
```python
//div[@class=’table’] # по классу
//employee[@id=’2’] # по id
//employee[@id=’1’]/name[text()=’David’] # по тексту
//name[text()=’John’] # по тексту
```

//элемент [содержит (@атрибут, ‘значение содержимого’) ] -синтаксис поиска по содержимому, используется когда у элемента прописано несколько классов
например для:
```python
<button class="btn btn-white">Join</button>
//button[contains(@class, 'btn')] #или
//button[contains(text(), 'Join')]
and - используется, когда нужно выполнять поиск по нескольким аргументам
//button[@data-cy='submitButton' and @type='submit']
TAB_PRICING_2 = ("xpath", "//a[contains(@href, '/pricing') and contains(@class, 'nav-link')]")
BUTTON_ALL_TRACKS_2 = ('css selector', "a[href='/tracks'][class*='btn router-link-exact-active']")
```

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
### Использование потомков
```python
(By.CSS, '#post2 .title') # означает, что нужно найти элемент с id='post2', у которого элемент-потомок будет иметь класс со значением class=`title`
```
Здесь
- `#` означает, что надо искать элемент с `id` `post2`
- пробел - что также нужно найти элемент-потомок, 
- `.`, что элемент-потомок должен иметь класс со значением `title`.

Элемент `.title` называется потомком (англ. descendant) элемента `#post2`. **Потомок может находиться на любом уровне вложенности**, все элементы с селектором `.title` также являются и потомками элемента `#posts`, хотя и расположены от него на два уровня ниже. `#posts .title` найдет все 3 элемента с классом `title`.

    !Внимание. Символ пробела `" "` является значащим в CSS-селекторах. Это важный символ, который разделяет описание предка и потомка. Если бы мы записали селектор #post2.title без пробела, то в данном примере не было найдено ни одного элемента. Такая запись означала бы, что мы хотим найти элемент, который одновременно содержит id "post2" и класс "title". Таким образом #post2 .title и #post2.title — это разные селекторы.

### Использование дочерних элементов
```python
(By.CSS, '#post2 > div.title') # означает, что найти элемент, у которого родитель имеет id='post2', а искомый элемент имеет тег div и class='title'
```
Здесь мы указали еще тег элемента `div` и уточнили, что нужно взять элемент с тегом и классом: `div.title`, который находится **строго на один уровень иерархии ниже чем элемент #post2**. Для этого используется символ `>`
Элемент `#post2` в этом случае называется родителем (англ. parent) для элемента div.title, а элемент div.title называется дочерним элементом (англ. child) для элемента #post2. Если символа > нет, то будет выполнен поиск всех элементов div.title на любом уровне ниже первого элемента.

### Использование порядкового номера дочернего элемента
```python
(By.CSS, '#posts > .item:nth-child(2) > .title') # означает, что необходимо найти элемент со значением class='title', родителем которого явлется элемент class='item', который является вторым по очереди дочерним элементом к родителю id='posts'
```
Псевдо-класс `:nth-child(2)` — позволяет найти второй по порядку элемент среди дочерних элементов для `#posts`. Затем с помощью конструкции `> .title` мы указываем, что нам нужен элемент `.title`, родителем которого является найденный ранее элемент `.item`.

### Использование нескольких классов
```python
textarea = driver.find_element(By.CSS_SELECTOR, ".title.second") # найдет элемент, класс которого одновременно содержит оба значения class="title second"
```
Также мы можем использовать сразу несколько классов элемента, чтобы его найти. Для этого классы записываются подряд через точку, как в примере выше.

```python
textarea = driver.find_element(By.CSS_SELECTOR, ".tradingView[data-type='tradingview']") # найдет элемент, который одновременно содержит в классе значение tradingView class='tradingView brick cc-boxLg' и data-type="tradingview"
```
### Регулярные выражения
- `^=` - `[attribute^=value]` Атрибут начинается с value. Сам символ ^ используется для обозначения начала строки.
- `$=` - `[attribute$=value]`  Символ $ используется для обозначения окончания строки. Например, активировать триггер при нажатии на ссылки, которые заканчиваются на .jpg
- `*=` - `[attribute*=value]` Атрибут содержит подстроку value и может быть равен myvalue, value2020, myvalue2020 и т.д. То есть выбираются все элементы с attribute, в значении которого (в любом месте) встречается подстрока (в виде отдельного слова или его части) value
- `~=` - `[attribute~=value]` Атрибут содержит value, значением которого является набор слов, разделенных пробелами, одно из которых в точности равно value. То есть value должно идти как отдельное слово через пробел, иначе конструкция не будет работать. Например: [attribute~=value] верно для 2020 value и неверно для 2020value или 2020-value.
- `|=` - `[attribute|=value]`  Атрибут равен value или начинается с value- (дефис следом!)

```Python
<div class="modal_overlay__f_YlZ">
 <div class="box_box__5Jmfa box_xl__ox1gr modal_modal__Y9d1p light">
  <button type="button" class="button_btn__vOh9h button_empty__Nmv1h button_primary__raeTg button_md__Xhil0 modal_close__m5UZc"><img alt="" loading="lazy" width="32" height="32" decoding="async" data-nimg="1" class="icon_icon__EUcfw" src="/_next/static/media/close.fcefa7c3.svg" style="color: transparent;"></button>
   <div class="grid_grid__2D3md grid_gMd__Cwsg7">
    <div class="grid_grid__2D3md helpers_textCenter__cyckU grid_gXs__xir6K">
     <strong class="heading_h3__nTE01 heading_noMargins__P5e_q"><span>Login</span></strong>
```
Для поиска элемента используется следующая форма записи:
```python
div [class*="modal"] [class*="heading_h3"] # * означает, что наименование значения представлено в частичном виде
```



# Методы
## clear()
очищает текстовое поле. Используется, например, когда в поле ввода уже есть значение, установленное по-умолчанию. Если поле не очистить, то при выполнении команды `send_keys()` введенный текст будет добавлен к существующему тексту.

## execute_script()
- `driver.execute_script("return window.scrollY;")` - возвращает Y координату текущего положения окна барузера, т.е.верхнего левого угла
- `driver.execute_script("return window.scrollX;")` - возвращает X координату текущего положения окна барузера, т.е.верхнего левого угла, скорее всего всегда будет 0
- `driver.execute_script("return window.innerWidth;")`
- `driver.execute_script("return window.innerHeight;")`


## find_element() 
Ищем поле для ввода текста. Метод find_element позволяет найти нужный элемент на сайте, указав путь к нему. Метод принимает в качестве аргументов способ поиска и значение, по которому мы будем искать

textarea.send_keys("get()") # Напишем текст ответа в найденное поле

submit_button = driver.find_element(By.CSS_SELECTOR, ".submit-submission") # Найдем кнопку, которая отправляет введенное решение

submit_button.click() # Скажем драйверу, что нужно нажать на кнопку. После этой команды мы должны увидеть сообщение о правильном ответе

```python
welcome_text_elt = browser.find_element(By.TAG_NAME, "h1") # находим элемент, содержащий текст
welcome_text = welcome_text_elt.text # записываем в переменную welcome_text текст из элемента welcome_text_elt
```
## find_elements() 

## get_attribute()
Позволяет получить значение атрибута HTML элемента.
 - Возвращает значение атрибута в виде строки.
 - Может использоваться для получения значений стандартных HTML атрибутов, таких как `id`, `name`, `class`, `href`, `src`, и так далее.
 - Пример использования: `element.get_attribute("href")` - вернет значение атрибута `href` элемента.

Например найдём атрибут `checked` с помощью встроенного метода `get_attribute` и проверим его значение:
```python
people_radio = browser.find_element(By.ID, "peopleRule")
people_checked = people_radio.get_attribute("checked")
print("value of people radio: ", people_checked)
```
Что бы получить текст из веб-элемента (например внутри поля ввода) нужно использовать атрибут "value"
```python
email_field.get_attribute('value')
```

## get_property()
- Этот метод используется для получения значения свойства JavaScript объекта, представляющего элемент.
- Возвращает значение свойства в виде объекта соответствующего типа (строка, булево, число, список и т.д.), а не как строки, как `get_attribute`.
- Может использоваться для получения значений нестандартных свойств элементов, таких как `innerText`, `checked`, `value`, и так далее.
- Пример использования: `element.get_property("innerText")` - вернет текстовое содержимое элемента.

Ключевое различие между методами `get_attribute` и `get_property` заключается в том, что `get_attribute` работает с HTML атрибутами, в то время как `get_property` работает с JavaScript свойствами объекта элемента.

## getText()

## get_window_position()
```python
driver.get_window_position()
"""
Gets the x,y position of the current window. - Это означает, расположение окна браузера на мониторе ПК, например полученное значение {'x': 100, 'y': 0} означает, что окно браузера на дисплее ПК будет смещено вправо на 100 пикселей и располагаться вплонутю к верхней границе монитора
"""
```

## set_window_position()
```python
driver.set_window_position()
"""
Sets the x,y position of the current window. (window.moveTo). - Это метод располагает окна браузера на мониторе ПК в указанных значениях, например после выполнении метода driver.set_window_position(0, 0) окно браузера на дисплее ПК будет размещено в левом верхнем углу монитора
"""
```


## save_screenshot()
Скриншот выполняется с помощью команды `save_screenshot()` и по умолчанию сохраняется в папке с файлом
```python
driver.get("https://dzen.ru")
driver.save_screenshot("screen.png")
driver.save_screenshot("screen.png")
```

## send_keys()
вводит текст в текстовые поля. К тестовым полям можно отнести теги input и textarea.

Способ 1 - Ввод данных напрямую
```python
# Ищем текстовое поле и вписываем туда текст
driver.find_element("xpath", "//input[@id='email']").send_keys("ваш email")
В данном способе используя точку, мы применяем метод send_keys() сразу к найденному элементу.
```

Способ 2 - Ввод данных через переменную
```python
# Записываем найденный элемент (text box) в переменную 
email_field = driver.find_element("xpath", "//input[@id='email']")

# Обращаясь к переменной с найденным элементом вызываем метод send_keys()
email_field.send_keys("ваш email")
```

## set_window_size()
метод позволяет устанавливать разрешение экрана после инициализации драйвера
```python
driver.set_window_size(1280, 720)
```
При этом внутренние размеры будут меньше установленных, например при проверке:
```python
driver.set_window_size(1280, 720)
window_width = driver.execute_script("return window.innerWidth;")
window_height = driver.execute_script("return window.innerHeight;")

print('window_width', window_width) # вернет 1267
print('window_height', window_height) # вернет 575
print(driver.get_window_size()) # {'width': 1282, 'height': 722}
```

## maximize_window()
драйвер запускает браузер во весь размер экрана

# Загрузка файлов
Если нам понадобится загрузить файл на веб-странице, мы можем использовать уже знакомый нам метод send_keys. Только теперь нам нужно в качестве аргумента передать путь к нужному файлу на диске вместо простого текста. Загрузка файлов на сайтах реализована тегом `<input>`, но выделяется он специальным атрибутом `<input type=”file”/>`

1. Способ - указать абсолютный путь к файлу
```python
driver.get('https://the-internet.herokuapp.com/upload')
upload_field = driver.find_element('xpath', '//input[@type="file"]')
path = "C:\\Users\\admin\\Projects\\selenium\\lesson\\downloads\\file.png"
upload_field.send_keys(path)
```

2. Способ - использовать os.getcwd()
```python
import os
driver.get('https://the-internet.herokuapp.com/upload')
upload_field = driver.find_element('xpath', '//input[@type="file"]')
path = os.getcwd() + '\\downloads\\file.png'
# или path = f'{os.getcwd()}\\downloads\\cch3zl9gdv_Screenshot (2).png'
upload_field.send_keys(path)
```

3. Способ - использовать os.path.abspath()
```python
import os
current_dir = os.path.abspath(os.path.dirname(__file__)) # получаем путь к директории текущего исполняемого файла
file_path = os.path.join(current_dir, 'file.txt') # добавляем к этому пути имя файла
element.send_keys(file_path)
```
Чаще используется тег `button`, но `type="file"` находится внутри кода и в devtools он не виден, для этого его нужно предварительно найти, наприме через `"xpath"` `//input[@type="file"]`, далее использовать как и ранее `send_keys()`.

# Ожидания

## Неявные ожидания - Implicit Waits
Неявное ожидание - это количество времени (которое указывается нами) в течение которого, WebDriver будет опрашивать DOM. Неявным оно называется, так как мы не указываем чего ждать, изменения текста, размера и т.д…

Основные моменты:
  - неявные ожидания задается глобально для всех элементов
  - используется в основном для find_element() и find_elements() - для обнаружения элемента на странице
  - При проверке на исчезновение элемента будет задерживать наши тесты
  
  Рекомендации от Алексея:
  - неявные ожидания не использовать, т.к. нам нужна конкретика - т.е. лучше прописывать явные ожидания для каждого конкретного случая;
  - не мешать явные и неявные ожидания - использовать что то одно

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

## Явное ожидание - Explicit Waits
`Явное ожидание` - ожидание конкретного условия, появления элемента, исчезновения элемента, изменения текста и т.д. Если в течение заданного времени событие на наступило, то Selenium выбросит исключение "Timeout Exception", после чего дальнейшее выполнение кода прекрашается.

[ссылка на все условия](https://www.selenium.dev/selenium/docs/api/py/webdriver_support/selenium.webdriver.support.expected_conditions.html)

Синтаксис:
```python
wait.until(EC.условие(локатор), mesasge="") = жди.пока_не_выполниться_условие(список_условий.условие(локатор), сообщение в случае если событие не наступило)

wait = WebDriverWait(driver, время, частота)
wait.until(EC.условие(локатор)))

wait = WebDriverWait(driver, 15, 1)
BUTTON = ("xpath", "//button[@id='example']")
wait.until(EC.условие(BUTTON))
```

Особенности:
- Обьявляется только там где нужно
- Ожидает выполнения нужного условия
- Проверка на отсутствие элемента, может быть условием к завершению теста, так как можно проверить, что элемент пропал и сразу завершить тест, а не ждать лишние 10 секунд.

Методы **find_element** проверяют только то, что элемент появился на странице. В то же время элемент может иметь дополнительные свойства, которые могут быть важны для наших тестов, например:
- Кнопка может быть неактивной, то есть её нельзя кликнуть;
- Кнопка может содержать текст, который меняется в зависимости от действий пользователя. Например, текст "Отправить" после нажатия кнопки поменяется на "Отправлено";
- Кнопка может быть перекрыта каким-то другим элементом или быть невидимой.

```python
from selenium import webdriver
from webdriver_manager.chrome import ChromeDriverManager
from selenium.webdriver.chrome.service import Service
from selenium.webdriver.support.ui import WebDriverWait # отвечает за явные ожидания 
from selenium.webdriver.support import expected_conditions as EC # ожидаемые условия

service = Service(executable_path=ChromeDriverManager().install())
driver = webdriver.Chrome(service=service)
driver.implicitly_wait(10)

wait = WebDriverWait(driver, 15, poll_frequency=1)

driver.get("https://demoqa.com/dynamic-properties")

VISIBLE_AFTER_BUTTON = ('xpath', '//button[@id="visibleAfter"]')

BUTTON = wait.until(EC.visibility_of_element_located(VISIBLE_AFTER_BUTTON))
BUTTON.click()
```

```python
# другая форма использования индивидуальная для каждого случая
WebDriverWait(driver, 10).until(EC.visibility_of_element_located(VISIBLE_AFTER_BUTTON))

# или можно создать метод класса, который будет принимать только локатор и время
def element_is_visible(self, locator, timeout=1):
  return Wait(self.driver, timeout).until(
      EC.visibility_of_element_located(locator)
  )

element_is_visible(VISIBLE_AFTER_BUTTON, 5)

```

В модуле expected_conditions есть много других правил, которые позволяют реализовать необходимые ожидания:

- alert_is_present - Ожидает, что появится всплывающее окно (alert) `wait.until(EC.alert_is_present())` - в метод не передается никакой аргумент!
- element_located_to_be_selected
- element_located_selection_state_to_be - Ожидает, что состояние выбора элемента (по его локатору) будет соответствовать заданному состоянию (True/False). `wait.until(EC.element_located_selection_state_to_be((By.ID, 'element_id'), True))`
- element_selection_state_to_be - Ожидает, что состояние выбора элемента будет соответствовать заданному состоянию (True/False) `wait.until(EC.element_selection_state_to_be((By.ID, 'element_id'), True))`
- **element_to_be_clickable** - Ожидает, что элемент станет кликабельным. `wait.until(EC.element_to_be_clickable((By.ID, 'element_id')))`
- element_to_be_selected - Ожидает, что элемент будет выбран. `wait.until(EC.element_to_be_selected((By.ID, 'element_id')))`
- frame_to_be_available_and_switch_to_it - Ожидает, что фрейм будет доступен и переключается на него `wait.until(EC.frame_to_be_available_and_switch_to_it((By.NAME, 'frame_name')))`
- **invisibility_of_element_located** - Ожидание проверки того, является ли элемент невидимым или он исчез из DOM. `wait.until(EC.invisibility_of_element_located((By.CSS_SELECTOR, 'css_selector')))`
- presence_of_all_elements_located
- presence_of_element_located - Ожидает, что элемент будет присутствовать в DOM. `wait.until(EC.presence_of_element_located((By.ID, 'element_id')))`
- **text_to_be_present_in_element** - Ожидает, что определенный текст появится в элементе. `wait.until(EC.text_to_be_present_in_element((By.ID, 'element_id'), 'expected_text')))`
- text_to_be_present_in_element_value - Ожидает, что определенный текст появится в значении элемента (например, в поле ввода). `wait.until(EC.text_to_be_present_in_element_value((By.ID, 'element_id'), 'expected_text')))`
- title_is - Ожидает, что заголовок страницы будет точно соответствовать заданному тексту `wait.until(EC.title_is('Expected Title'))`
- title_contains -  Ожидает, что заголовок страницы будет содержать заданный текст `wait.until(EC.title_contains('Expected Text'))`
- staleness_of
- **visibility_of_element_located** - Ожидание проверки того, что элемент присутствует в DOM и виден визуально. Видимость означает, что элемент не только отображается но также имеет высоту и ширину, которые больше 0. `wait.until(EC.visibility_of_element_located((By.XPATH, 'xpath_expression')))`
- visibility_of - Ожидает, что элемент станет видимым и отображаемым на странице. `element = wait.until(EC.visibility_of(driver.find_element(By.ID, 'element_id')))`
- `wait.until(EC.url_changes(expected_link))` 
- url_contains - Ожидает, что URL страницы будет содержать заданный текст. `wait.until(EC.url_contains('example'))`
- url_to_be - Ожидает, что URL страницы будет точно соответствовать заданному URL `wait.until(EC.url_to_be('https://example.com'))`

Описание каждого правила можно найти на [сайте](https://selenium-python.readthedocs.io/api.html#module-selenium.webdriver.support.expected_conditions).

Если мы захотим проверять, что кнопка становится неактивной после отправки данных, то можно задать негативное правило с помощью метода until_not:
```python
# говорим Selenium проверять в течение 5 секунд пока кнопка станет неактивной
button = WebDriverWait(browser, 5).until_not(
        EC.element_to_be_clickable((By.ID, "verify"))
    )
```
### element_to_be_clickable
```python
from selenium.webdriver.support import expected_conditions as EC

wait = WebDriverWait(driver, 10)
element = wait.until(EC.element_to_be_clickable((By.ID, 'someid')))
```

An Expectation for checking an element is visible and enabled such that you can click it.

### visibility_of_element_located
```python
selenium.webdriver.support.expected_conditions.visibility_of_element_located(locator: Tuple[str, str]) → Callable[[WebDriver], Literal[False] | WebElement]
```
An expectation for checking that an element is present on the DOM of a page and visible. Visibility means that the element is not only displayed but also has a height and width that is greater than 0.

locator - used to find the element returns the WebElement once it is located and visible

# Работа с окнами и вкладками
    Важно!
    1. в Selenium нет различий между окнами и вкладками.
    2. при открытии новой вкладки кликом по ссылке автоматического переключения на новое окно/вкладку не произойдет
- `driver.current_window_handle` - параметр, который показывает уникальный идентификатор окна (дискриптор), с которым сейчас работает selenium, например: `A6AB6790986A839C856B43849AFF1214`
- `driver.window_handles` - параметр, который выдает список идентификаторов всех окон, открытых в течение сеанса в selenium, например: `['D6B4ECFADF7A54289E1AF0241759F93B', '330767AB91D52C80723B6973A381BD72']`
- `driver.switch_to.window(tabs[1])` - переключиться на вторую вкладку
- `driver.switch_to.new_window('tab')` - создаст новую вкладку, при этом фокус selenium также переключиться на созданную вкладку
- `driver.switch_to.new_window('window')` - создаст новое окно, при этом фокус selenium также переключиться на созданное окно
- `driver.close()` Закрытие активного окна / вкладки, при этом не произойдет автоматического переключения на другое окно, для это в явном виде необходимо переключиться с помощью команды `driver.switch_to.window()`
- `driver.quit()` Закрытие сессии, т.е всего браузера

```python
tabs = driver.window_handles # создать перечень всех вкладок/окон
driver.switch_to.window(tabs[1]) # переключиться на вторую вкладку

# driver.switch_to.window(driver.window_handles[1])
```

# iframe
**iframe** - это html-страница, внутри другой html-страницы.

Методы:
- `switch_to.frame()` - В качестве аргумента, он принимает WebElement, атрибут name или индекс нужного iframe.
- `switch_to.default_content()` - Переключение с iframe обратно на страницу

```python
iframe_volunteer = driver.find_element(By.XPATH, "//iframe")
driver.switch_to.frame(iframe_volunteer)

# Теперь попробуем ввести данные в поле First Name
first_name_field = driver.find_element(By.XPATH, "//input[@name='RESULT_TextField-1']")
first_name_field.send_keys("Alexey")
```

# Alert
Alert - это обычное предупреждающее или требующее подтверждения окно, появляется в случае нажатия на вызывающую алерт кнопку, либо автоматически при заходе какой-то сайт, к примеру мы будем работать с https://demoqa.com/alerts

Работа с алертами, максимально простая, у него не так много методов, но есть у него один нюанс, это не веб-элемент и его не найти в DOM. Для этого нам необходимо применить другой подход, а именно переключиться на alert.

- `switch_to.alert` - метод для переключения на алерт
- `accept()` - принимает алерт
- `dismiss()` - отклоняет алерт
- `alert.text` - получить текст
- `alert.send_keys("Hello world")` - ввод данных в alert

Примеры:
```python
BUTTON_3 = ("xpath", "//button[@id='confirmButton']")
wait.until(EC.element_to_be_clickable(BUTTON_3)).click()

alert = wait.until(EC.alert_is_present())

driver.switch_to.alert

alert.accept() # Принимаем алерт

"""другой пример"""
# Ввод данных в alert
alert.send_keys("Hello world")

# Обязательно либо примите либо отклоните alert после вводад анных
alert.accept()
```

# Исключения - Exceptions
- **NoSuchElementException** - если элемент не был найден за отведенное время
- **StaleElementReferenceException** - если элемент был найден в момент поиска, но при последующем обращении к элементу DOM изменился. Например, мы нашли элемент Кнопка и через какое-то время решили выполнить с ним уже известный нам метод click. Если кнопка за это время была скрыта скриптом, то метод применять уже бесполезно — элемент "устарел" (stale) и мы увидим исключение.
- **ElementClickInterceptedException** - элеммент перекрыт другим элементом, например всплывающим окном.
- **ElementNotInteractableException** - с элементом невозможно взаимодействовать. возможно элемент перекрыт другими элементами.
- **ElementNotVisibleException** - если элемент был найден в момент поиска, но сам элемент невидим (например, имеет нулевые размеры), и реальный пользователь не смог бы с ним взаимодействовать, то получим


# Action chains
Action chains (Цепочка действий) - это довольно таки низко-уровнивая автоматизация (более продвинутая), например есть задачи, когда нужно нажать правой кнопкой мыши, навестить на что-то, перетащить, и прочее…

**Алгоритм выполнения:**
1. Сначала мы обращаемся к action
2. Далее указываем цепочку действий которую хотим совершить, например "Кликни сюда". Самое важное, что через точку можно указать несколько действий по порядку "Кликни сюда. наведись сюда. кликни правой кнопкой сюда"
3. В конце цепочки говорим "Выполняй!" perform()

```python
from selenium.webdriver.common.action_chains import ActionChains

action = ActionChains(driver) # создан обьект через который будут выполняться действия мыши

# одинарный клик левой кнопкой мыши
left_click_button = driver.find_element(*LEFT_CLICK_BUTTON_LOCATOR)
action.click(left_click_button).preform()

# двойной клик левой кнопкой мыши
double_click_button = driver.find_element
action.double_click(double_click_button).perform()

# клик правой кнопкой мыши
right_click_button = driver.find_element(*RIGHT_CLICK_BUTTON_LOCATOR)
action.context_click(right_click_button).perform()

#выполнение цепочки действий
action.click(left_click_button) \
    .pause(1) \ # пауза в течение 1 секунды
    .double_click(double_click_button) \
    .pause(1) \
    .context_click(right_click_button) \
    .pause(1) \
    .perform()
```

Методы:
- `click()` - клик левой кнопкой мыши
- `double_click()` - двойной клик левой кнопкой мыши
- `context_click()` - клик правой кнопкой мыши
- `move_to_element()` - перемещение курсора на элемент
- `scroll_to_element()` - выполнить скроллирование до элемента
- `pause()` - паза
- `perform()` - обязательный метод, который устанавливается в конце цепочки


# Drag and Drop

```python
# first way
driver.get("https://the-internet.herokuapp.com/drag_and_drop")

A = driver.find_element(*A_LOCATOR)
B = driver.find_element(*B_LOCATOR)

action.drag_and_drop(A, B).perform()
```

```python
# second way
url = "https://tympanus.net/Development/DragDropInteractions/sidebar.html"
driver.get(url)

GRID_ITEM_3_LOCATOR = ('xpath', '(//div[@class="grid__item"])[3]')
DROP_AREA_ITEM_3_LOCATOR = ('xpath', '(//div[@class="drop-area__item"])[3]')

action.click_and_hold(driver.find_element(*GRID_ITEM_3_LOCATOR)) \
    .pause(1.5) \
    .move_to_element(driver.find_element(*DROP_AREA_ITEM_3_LOCATOR)) \
    .release() \
    .perform()
```

- `click_and_hold()` - Данная фишка нужна для работы с ползунками или для фич ориентированных на удержание курсора на элементе, в качестве аргумента принимает веб-элемент
- `drag_and_drop()` - метод для перетягивания одного элемента на другой
- `release()` - метод для отпускания кнопки мыши

# Javascript и скроллинг

- `driver.execute_script()` - команда, для выполнения javascript

```python
driver.execute_script("alert('Hello')")  # выводит алерт на странице с текстом 'Hello'
```

```python
class Scrolls:

    def __init__(self, driver, action):
        self.driver = driver
        self.action = action

    def scroll_by(self, x, y): # Скролл по x и y
        self.driver.execute_script(f"window.scrollTo({x}, {y})")

    def scroll_to_bottom(self): # Скролл в самый низ страницы
        self.driver.execute_script("window.scrollTo(0, document.body.scrollHeight)")

    def scroll_to_top(self): # Скролл на самый верх страницы
        self.driver.execute_script("window.scrollTo(0, 0)")

    def scroll_to_element(self, element):# Скролл к элементу с раскрытием контента под ним
        self.action.scroll_to_element(element).perform()
        self.driver.execute_script("""
        window.scrollTo({
            top: window.scrollY + 700,
        });
        """)
```

```python
driver.execute_script('return arguments[0].scrollIntoView({block: "center", inline: "nearest"});',
driver.find_elements(*BLOCK_LOCATOR)[0]
      )
```