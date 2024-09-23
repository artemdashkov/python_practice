Документация:

1. https://redocly.github.io/redoc/
2. https://petstore.swagger.io/

pip install requests

```python
# GET запрос без передачи параметров
response = requests.get(
    url='https://petstore.swagger.io/v2/store/inventory',
    headers={
        "api_key": "special-key"
    },

)
print('1.\t', response.text)
print('2.\t', response.json())
print('3.\t', response.status_code)
print('4.\t', response.json()['status'])
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
        "status": "available"
    }
)

print(response.json()[0]['tags'][0]['name'])
```