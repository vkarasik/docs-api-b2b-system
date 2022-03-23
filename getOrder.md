## Получение состояния заказа

### POST /getOrder/

Для авторизации используются ваши текущие логин(**login**):пароль(**password**) в системе B2B.

### Параметры запроса

| Параметр | Тип              | Обязательный | Описание                            |
| -------- | ---------------- | ------------ | ----------------------------------- |
| order    | array of strings | да           | список заказов из ответа /addOrder/ |

### Пример запроса

```http
POST /getOrder/
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
  "data": [
    {
      "order": "97042",
      "dorder": "2018-09-06 14:57:54",
      "status": "A",
      "sum": "24606.91",
      "products": [
        {
          "id": false,
          "name": "Корпус FSP QD-503BGM GAMING ATX Black <USB3.0*1,USB2.0*2,HDA,2*5.25'ext/2*3.5'+2*2.5'int,7*FHFL,12cm reafan,0.5mm,MB=245mm,VGA=350mm,CPUfan=160mm,Window,L=435,W=190,H=465// +PSU QD550 80+>",
          "qty": "5.00"
        },
        {
          "id": false,
          "name": "Корпус FSP QD-503BGM GAMING ATX Black <USB3.0*1,USB2.0*2,HDA,2*5.25'ext/2*3.5'+2*2.5'int,7*FHFL,12cm reafan,0.5mm,MB=245mm,VGA=350mm,CPUfan=160mm,Window,L=435,W=190,H=465// noPSU>",
          "qty": "1.00"
        }
      ]
    }
  ],
  "code": "ok"
}
```

### Описание полей ответа

| Параметр               | Тип    | Описание                            |
| ---------------------- | ------ | ----------------------------------- |
| data.N.order           | string | Номер заказа                        |
| data.N.dorder          | string | Дата заказа                         |
| data.N.status          | string | Статус заказа                       |
| data.N.sum             | string | Сумма заказа BYN c НДС              |
| data.N.products.N.id   | string | id товара                           |
| data.N.products.N.name | string | Название товара                     |
| data.N.products.N.qty  | string | Количество зарезервированого товара |

Возможные статусы `data.N.status` заказа

- `"status": "N"` — Заказ отправлен и обрабатывается
- `"status": "A"` — Заказ утвержден, товары `data.N.products`, в количестве `data.N.products.N.qty`, на сумму `data.N.sum` зарезервированы
- `"status": "F"` — Запрошен счет на утвержденный заказ
- `"status": "C"` — Заказ отменен

### Ответ при ошибке авторизации

```http
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8
```

```json
{ "data": "Access denied", "code": "error" }
```

### Ответ при запросе с единственным неверным номером заказа

```http
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8
```

```json
{
  "data": "Order not found",
  "code": "error"
}
```

### Пример запроса PHP-cURL

```php
<?php

$curl = curl_init();

curl_setopt_array($curl, array(
  CURLOPT_URL => "http://b2b.cd-life.by/api/getOrder/",
  CURLOPT_RETURNTRANSFER => true,
  CURLOPT_ENCODING => "",
  CURLOPT_MAXREDIRS => 10,
  CURLOPT_TIMEOUT => 0,
  CURLOPT_FOLLOWLOCATION => true,
  CURLOPT_HTTP_VERSION => CURL_HTTP_VERSION_1_1,
  CURLOPT_CUSTOMREQUEST => "POST",
  CURLOPT_POSTFIELDS => array('order' => '["217716"]'),
  CURLOPT_HTTPHEADER => array(
    "Authorization: Basic YWxhZGRpbjpvcGVuc2VzYW1l"
  ),
));

$response = curl_exec($curl);

curl_close($curl);
echo $response;

```
