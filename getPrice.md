## Получение прайс-листа

### POST /getPrice/

Для авторизации используются ваши текущие логин(**login**):пароль(**password**) в системе B2B.

### Параметры запроса

| Параметр | Тип              | Обязательный | Описание                                   |
| -------- | ---------------- | ------------ | ------------------------------------------ |
| id       | array of strings | нет          | id товара (соответсвует коду товара в b2b) |

Запрос без параметров возвращает полный актуальный прайс-лист.

**Обращаем внимание:** при запросе через api, в прайс-лист не попадают товары с ограниченной доступностью (резервы) и товары находящиеся в транзите. Для просмотра и заказа таких позиций используйте сайт.

### Пример запроса

```http
POST /getPrice/
Authorization: Basic
```

```json
{
  "id": ["SP00005504"]
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
    "groups": [
      {
        "id": "1182",
        "name": "Печатная техника",
        "groups": [
          {
            "id": "1185",
            "name": "Расходные материалы",
            "groups": [
              {
                "id": "2411",
                "name": "Струйные картриджи"
              }
            ]
          }
        ]
      }
    ],
    "products": [
      {
        "id": "SP00005504",
        "group": "2411",
        "name": "Картридж CL-446XL &lt;MG2440, MG2540&gt;",
        "article": "8284B001AA",
        "brand": "Canon",
        "warranty": "12",
        "rest": "▀",
        "price": 46.7,
        "rrprice": ""
      }
    ]
  },
  "code": "ok"
}
```

### Описание полей ответа

| Параметр                 | Тип    | Описание                                                                                       |
| ------------------------ | ------ | ---------------------------------------------------------------------------------------------- |
| data.groups              | array  | список групп/подгрупп для запрошенных id                                                       |
| data.products            | array  | список товаров                                                                                 |
| data.products.N.id       | string | id товара (соответсвует коду товара в b2b)                                                     |
| data.products.N.group    | string | id группы товара                                                                               |
| data.products.N.name     | string | Наименование товара                                                                            |
| data.products.N.article  | string | SKU (артикул) товара                                                                           |
| data.products.N.brand    | string | Производитель товара, null если не указан                                                      |
| data.products.N.warranty | string | Гарантия месяцев, null если не указан                                                          |
| data.products.N.rest     | string | Наличие товара на складе ("▀" — мало, "▀▀" — достаточно, "▀▀▀" — много, "0" — товар не найден) |
| data.products.N.price    | number | Цена BYN с НДС                                                                                 |
| data.products.N.rrprice  | string | Рекомендованная розничная цена BYN с НДС                                                       |

### Ответ при ошибке авторизации

```http
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8
```

```json
{ "data": "Access denied", "code": "error" }
```

### Ответ при запросе единственного неверного id

```http
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8
```

```json
{
  "data": "Products not found",
  "code": "error"
}
```

В случае если неверный **id** запрошен в массиве с верными, ответ будет как в случае успеха, но для неверного **id** вернется **"rest" : "0"**

### Пример запроса c неверным/несуществующим id

```http
POST /getPrice/
Authorization: Basic
```

```json
{
  "id": ["SP00005504", "SP00000000"]
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
    "groups": [
      {
        "id": "1182",
        "name": "Печатная техника",
        "groups": [
          {
            "id": "1185",
            "name": "Расходные материалы",
            "groups": [
              {
                "id": "2411",
                "name": "Струйные картриджи"
              }
            ]
          }
        ]
      }
    ],
    "products": [
      {
        "id": "SP00005504",
        "group": "2411",
        "name": "Картридж CL-446XL &lt;MG2440, MG2540&gt;",
        "article": "8284B001AA",
        "brand": "Canon",
        "warranty": "12",
        "rest": "▀",
        "price": 46.7,
        "rrprice": ""
      },
      {
        "id": "SP0000000",
        "rest": "0"
      }
    ]
  },
  "code": "ok"
}
```

### Пример запроса PHP-cURL

```php
<?php

$curl = curl_init();

curl_setopt_array($curl, array(
  CURLOPT_URL => "http://b2b.cd-life.by/api/getPrice/",
  CURLOPT_RETURNTRANSFER => true,
  CURLOPT_ENCODING => "",
  CURLOPT_MAXREDIRS => 10,
  CURLOPT_TIMEOUT => 0,
  CURLOPT_FOLLOWLOCATION => true,
  CURLOPT_HTTP_VERSION => CURL_HTTP_VERSION_1_1,
  CURLOPT_CUSTOMREQUEST => "POST",
  CURLOPT_POSTFIELDS => array('id' => '["SP00014230"]'),
  CURLOPT_HTTPHEADER => array(
    "Authorization: Basic YWxhZGRpbjpvcGVuc2VzYW1l",
  ),
));

$response = curl_exec($curl);

curl_close($curl);
echo $response;
```
