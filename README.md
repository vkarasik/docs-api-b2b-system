# API Description

Для авторизации используются ваши текущие логин(**login**):пароль(**password**) в системе B2B.

API Route – http://b2b.cd-life.by/api/

| Endpoint                             | Описание                                      |
| ------------------------------------ | --------------------------------------------- |
| [POST /getPrice/](getPrice.md)       | Получение прайс-листа c группировкой          |
| [POST /getProducts/](getProducts.md) | Получение прайс-листа без группировки         |
| [POST /addOrder/](addOrder.md)       | Добавление заказа                             |
| [POST /getOrder/](getOrder.md)       | Получение статуса заказа                      |
| [POST /delOrder/](delOrder.md)       | Отмена заказа                                 |
| [POST /getInvoice/](getInvoice.md)   | Запрос счета на утвержденный заказ            |
| [POST /serarchSn/](serarchSn.md)     | Проверка изделия на принадлежность к поставке |
