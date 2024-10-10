- [GET](#GET)
- [POST](#POST)
- [PUT](#PUT)
- [PATCH](#PATCH)
- [Chains](#Chains)
- [pydantic](#pydantic)
- [Структура](#Структура)

# POST
Документация:

1. https://redocly.github.io/redoc/
2. https://petstore.swagger.io/

pip install requests
# GET
GET запрос позволяет просто получать информацию от сервера
### GET запрос без передачи параметров
```python
response = requests.get(
    url='https://petstore.swagger.io/v2/store/inventory', # endpoint куда мы обращаемся
    headers={
        "api_key": "special-key"    # header авторизации, где "api_key" -имя, "special-key" - токен
    },

)
print('1.\t', response.text) # получить ответ в виде текста, если обратиться к какой-то странице, то вернет весь html-код страницы
print('2.\t', response.json()) # получить ответ в виде json
print('3.\t', response.status_code) # получить статус код ответа
print('4.\t', response.json()['status']) # получить значение ключа 'status' из json ответа
print('4.\t', response.cookies) # получить куки
```

### GET запрос с указанием path-параметра: например "any-site.com/users/15df4s"
Это путь к ресурсу, от которого мы хотим получить информацию
`{{postman_base_url}}/collections/:collection_id?find=<find>&replace=<replace>`
Query Params:
- find              <find>
- replace           <replace>
Path Variables:
- collection_id     {{collection_id}}


### GET запрос с указанием query-параметров
query-параметры это фильтрации, перечисления свойств, тоже является частью url, но передается как параметр `?status=available`
```python
response = requests.get(
    url="https://petstore.swagger.io/v2/pet/findByStatus",
    headers={
        "api_key": "special-key"
    },
    params={
        "status": "available" # при запросе добавится в url и станет его частью (https://petstore.swagger.io/v2/pet/findByStatus?status=available)
    },
    # auth={'login':'password'} # авторизация через логин и пароль, используется редко
)

print(response.json()[0]['tags'][0]['name'])
```

# POST
Параметры в requests.post:
- url="" - передача url
- json={} - передача словаря, библиотека requests сама переконвертирует в json формат

## POST запрос с JSON телом
```python
import requests

url_pet_post = 'https://petstore.swagger.io/v2/pet'
body_pet_post = {
  "id": 0,
  "category": {
    "id": 0,
    "name": "string"
  },
  "name": "doggie",
  "photoUrls": [
    "string"
  ],
  "tags": [
    {
      "id": 0,
      "name": "string"
    }
  ],
  "status": "available"
}
response = requests.post(
    url=url_pet_post,
    json=body_pet_post, # 
    verify=False # при запросе не будет проверяться сертификат, выполняет код, но выводит сообщение, что запроос не безопасный
)

assert response.status_code == 200
print(response.json())
```

## POST запрос с Data/ Files телом
```python
response = requests.post(
  url="https://petstore.swagger.io/v2/pet/1/uploadImage",
  files={
    "file": ("unnamed.jpg", open('unnamed.jpg','rb'), 'image/jpeg') 
  }
)

print(response.json())

# В случае, если не удается передать файлы через "data", то нужно передавать данные через "files"
```

# PUT
если передать не все данные, то не переданные данные затрутся
```python
# необходимо передвать все тело, чтобы не потерять данные
response = requests.put(
    url="https://petstore.swagger.io/v2/pet",
    json={
          "id": 1,
          "category": {
            "id": 0,
            "name": "string"
          },
          "name": "Alex",
          "photoUrls": [
            "string"
          ],
          "tags": [
            {
              "id": 0,
              "name": "string"
            }
          ],
          "status": "available"
        }
)

print(response.json())
```

# PATCH
...

# Chains
```python
HEADERS = {
    "Authorization":"any_token"
}

class TestUsers:
    # setup
    def setup(self):
        requests.post(
            url="any_url",
            headers=HEADERS
        )

    # get
    def test_patch_user(self):
        user_list = requests.get(
            url="any_url",
            headers=HEADERS
        )

        user = user_list.json()['items'][0]['uuid']
        old_user_name = user_list.json()['items'][0]['name']
        patched_user = requests.patch(
            url=f'any_utl{user}',
            headers=HEADERS,
            json={
                "name":"QWERTY"
            }
        )

        assert old_user_name != patched_user.json()['name']
```


# pydantic
pip3 install pydantic - установка
### pydantic - автоматическое преобразование типов
```python
from pydantic import BaseModel # Для того чтобы начать работать с дата классами

class UserModel(BaseModel):
    id: int
    name: str
    status: str
    admin: bool

response = {
    'id': 145,
    'name': 'Ivan',
    'status': 'active',
    'admin': True
}

user = UserModel(**response) # распарсивает json объект 'response' в виде полей класса и валидирует их по типам данных
print(user)
print(user.admin)

> id=145 name='Ivan' status='active' admin=True
> True
```

### pydantic - вывод и отображение ошибок

```python
from pydantic import BaseModel # Для того чтобы начать работать с дата классами

class UserModel(BaseModel):
    id: int
    name: str
    status: str
    admin: bool

response = {
    'id': '145w',
    'name': 'Ivan',
    'status': 'active',
    'admin': True
}

user = UserModel(**response) 

> Input should be a valid integer, unable to parse string as an integer [type=int_parsing, input_value='145w', input_type=str]

```

### pydantic - Наличие полей в Class и JSON, Опциональные поля
```python
from pydantic import BaseModel # Для того чтобы начать работать с дата классами
from typing import Optional


class UserModel(BaseModel):
    id: int
    name: str
    status: str
    # admin: bool ## например нет поля "admin"
    admin: Optional[bool] # или  admin: Optional[bool] = None - установка значения 'None' по умолчанию
response = {
    'id': 145,
    'name': 'Ivan',
    'status': 'active',
    'admin': True
}

user = UserModel(**response)
print(user)
print(user.admin)
```

### pydantic - Вложенная модель и dataclass
```python
from pydantic import BaseModel # Для того чтобы начать работать с дата классами

class Post(BaseModel):
    id: int
    content: str

class DataModel(BaseModel):
    id: int
    status: str
    tags: list[str]
    posts: dict[str, Post]


response = {
    'id': 1234,
    'status': 'active',
    'tags': ['music', 'people', 'greeting'],
    'posts': {
        "post_1":   {
                    'id': 1,
                    'content': 'Hello my dear friends'
                    },
        "post_2":   {
                    'id': 2,
                    'content': "Thing it's bad"
                    }
                }
}

model = DataModel(**response)
print(model.posts["post_1"].content)
```

### pydantic - Валидация списков
```python
from pydantic import BaseModel

# Ситуация 1
class ResponseListModel(BaseModel):
    name: str
    last_name: Annotated[str, constr(max_length=50)]
    admin: Optional[bool] = None

response_1 = [
    {
        "name": "Ivan",
        "last_name": "Ivanov"
    },
    {
        "name": "Petr",
        "last_name": "Petrov"
    }
]

model_1 = ([ResponseListModel(**item) for item in response_1])


# Ситуация 2
class MainModel(BaseModel):
    items: list[ResponseListModel]

response_2 = {
    'items': [
    {
        "name": "Ivan",
        "last_name": "Ivanov"
    },
    {
        "name": "Petr",
        "last_name": "Petrov"
    }
]}

model_2 = MainModel(**response_2)
print(model_2.items[0].last_name)
```

### pydantic - extra_model
```python
from pydantic import BaseModel, Extra

class DynamicModel(BaseModel):
    class Config:
        extra = 'allow'

class UserModel(DynamicModel):
    name: str
    age: int
    status: str
    # tarif: str = None

response = {
    "name": "Ivan",
    "age": 25,
    "status": "Helthy",
     "admin": True
}

model = UserModel(**response)
print(model.admin)
```

### pydantic - Валидаторы
```python
from pydantic import BaseModel, field_validator

class ServerLogs(BaseModel):
    id: int
    logs: str

    @field_validator('logs')
    def logs_has_contain_lag(cls, value):
        if 'lag' not in value.lower():
            raise ValueError(f"Field doesn't contain the 'lag'")
        else:
            return value

response = {
    "id": 12412,
    "logs": "dsfdsfsdfsdf"
}
```


