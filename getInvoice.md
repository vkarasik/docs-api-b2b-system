## Запрос счета на утвержденный заказ

Запрос счета возможен только для заказов получивших статус `"status": "A"` — Заказ утвержден. Запрошенный счет будет выслан менеджером на вашу электронную почту.

### POST /getInvoice/

Для авторизации используются ваши текущие логин(**login**):пароль(**password**) в системе B2B.

### Параметры запроса

| Параметр | Тип              | Обязательный | Описание                                        |
| -------- | ---------------- | ------------ | ----------------------------------------------- |
| order    | array of strings | да           | Номер утвержденного заказа из ответа /getOrder/ |

### Пример запроса

```http
POST /getInvoice/
Authorization: Basic
```

```json
{
  "order": ["97042"]
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
    "order": "97042"
  },
  "code": "ok"
}
```

### Описание полей ответа

| Параметр   | Тип    | Описание     |
| ---------- | ------ | ------------ |
| data.order | string | Номер заказа |

### Ответ при ошибке авторизации

```http
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8
```

```json
{ "data": "Access denied", "code": "error" }
```

### Ответ при запросе с неверным номером заказа, или при неутвержденном заказе

```http
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8
```

```json
{
  "data": "Order not found or not approved",
  "code": "error"
}
```
