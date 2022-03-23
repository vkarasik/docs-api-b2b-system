## Добавление заказа

### POST /addOrder/

Для авторизации используются ваши текущие логин(**login**):пароль(**password**) в системе B2B.

### Параметры запроса

| Параметр       | Тип    | Обязательный | Описание                     |
| -------------- | ------ | ------------ | ---------------------------- |
| products       | array  | да           | список товаров для заказа    |
| products.N.id  | string | да           | id товара из прайс-листа     |
| products.N.qty | string | да           | количество товара для заказа |

### Пример запроса

```http
POST /addOrder/
Authorization: Basic
```

```json
{
  "products": [
    {
      "id": "SP00009726",
      "qty": "1"
    }
  ]
}
```

### Ответ в случае успеха

```http
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8
```

```json
{
  "data": {
    "order": 97007
  },
  "code": "ok"
}
```

### Описание полей ответа

| Параметр   | Тип    | Описание                |
| ---------- | ------ | ----------------------- |
| data.order | number | Номер созданного заказа |

### Ответ при ошибке авторизации

```http
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8
```

```json
{ "data": "Access denied", "code": "error" }
```

### Ответ при запросе с единственным неверным id

```http
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8
```

```json
{
  "data": "Products not available",
  "code": "error"
}
```

В случае если товар с неверным **id** отправлен в массиве с верными, ответ будет как в случае успеха, но товар с неверным **id** не попадет в заказ

### Пример запроса c неверным/несуществующим id

```http
POST /addOrder/
Authorization: Basic
```

```json
{
  "products": [
    {
      "id": "SP00009726",
      "qty": 1
    },
    {
      "id": "SP00000000",
      "qty": 1
    }
  ]
}
```

### Ответ в случае успеха

```http
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8
```

```json
{
  "data": {
    "order": 97011
  },
  "code": "ok"
}
```
