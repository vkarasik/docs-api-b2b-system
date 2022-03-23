## Проверка изделия на принадлежность к поставке

Запрос позволяет по серийному номеру проверить реализовывалось ли устройство нашей компанией, для решения вопросов связанных с гарантийными обязательствами.

### POST /searchSn/

Для авторизации используются ваши текущие логин(**login**):пароль(**password**) в системе B2B.

### Параметры запроса

| Параметр | Тип    | Обязательный | Описание                                    |
| -------- | ------ | ------------ | ------------------------------------------- |
| sn       | string | да           | серийный номер изделия указанный на корпусе |

### Пример запроса

```http
POST /getPrice/
Authorization: Basic
```

```json
{
  "sn": "109EXTRULIFE11"
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
    "sn": "109EXTRULIFE11",
    "product": "Внешний аккумулятор Defender ExtraLife",
    "partner": "ООО &quot;Корпоративный стандарт&quot;",
    "date": "09.01.2020"
  },
  "code": "ok"
}
```

### Описание полей ответа

| Параметр     | Тип    | Описание                         |
| ------------ | ------ | -------------------------------- |
| data.sn      | string | серийный номер изделия           |
| data.product | string | наименование изделия             |
| data.partner | string | контрагент приобретавший изделие |
| data.date    | string | дата покупки                     |

### Ответ при отсутствии в базе серийного номера

```http
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8
```

```json
{
  "data": "false",
  "code": "ok"
}
```

### Ответ при ошибке авторизации

```http
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8
```

```json
{ "data": "Access denied", "code": "error" }
```

### Пример запроса PHP-cURL

```php
<?php

$curl = curl_init();

curl_setopt_array($curl, array(
    CURLOPT_URL => "http://b2b.cd-life.by/api/searchsn/",
    CURLOPT_RETURNTRANSFER => true,
    CURLOPT_ENCODING => "",
    CURLOPT_MAXREDIRS => 10,
    CURLOPT_TIMEOUT => 0,
    CURLOPT_FOLLOWLOCATION => true,
    CURLOPT_HTTP_VERSION => CURL_HTTP_VERSION_1_1,
    CURLOPT_CUSTOMREQUEST => "POST",
    CURLOPT_POSTFIELDS => array('sn' => '109EXTRALIFE12'),
    CURLOPT_HTTPHEADER => array(
        "Authorization: Basic YWxhZGRpbjpvcGVuc2VzYW1l",
    ),
));

$response = curl_exec($curl);

curl_close($curl);
echo $response;

```
