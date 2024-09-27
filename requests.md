Документация:

1. https://redocly.github.io/redoc/
2. https://petstore.swagger.io/

pip install requests
# GET
```python
# GET запрос без передачи параметров
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

```python
# GET запрос с указанием path-параметра: например "any-site.com/users/15df4s"
```

```python
# GET запрос с указанием query-параметров
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
```python
import requests

# POST запрос с JSON телом
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
    json=body_pet_post,
    verify=False # при запросе не будет проверяться сертификат, выполняет код, но выводит сообщение, что запроос не безопасный
)

assert response.status_code == 200
print(response.json())
```