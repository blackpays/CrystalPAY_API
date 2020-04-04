# Документация по API онлайн кассы CrystalPAY.
В данной документации описаны все доступные методы/запросы для работы с кассой.

***

### Возможные ошибки
|    Ошибка       |                      Описание                                          |
|:---------------:|------------------------------------------------------------------------|
| `auth`: "error" | Неверно указан секретный ключ 1, либо логин кассы.                     |
| `error`: true   | Возникла ошибка при выполнении запроса, возможно не соблюдены условия. |

&nbsp;

## Получение баланса кассы

| Параметр |     Описание     |
|:--------:|------------------|
|     o    | Операция (balance)|
|     n    | Имя/Логин кассы  |
|     s    | Секретный ключ 1 |

`GET` Пример запроса:

```sh
https://api.crystalpay.ru/api.php?s=0f5601d7150d663cc1dd05bbe035c0cb02b4bc8f&n=testkassa&o=balance
```

`JSON` Ответ:
```json
{"balance":{"YANDEX":52,"QIWI":100,"VISAMASTER.RUR":947},"auth":"ok"}
```

## Генерация ссылки для оплаты

| Параметр |     Описание     |
|:--------:|------------------|
|     o    | Операция (generate) |
|     n    | Имя/Логин кассы  |
|     s    | Секретный ключ 1 |
|  amount  | Сумма в рубля    |

`GET` Пример запроса:

```sh
https://api.crystalpay.ru/api.php?s=0f5601d7150d663cc1dd05bbe035c0cb02b4bc8f&n=testkassa&o=generate&amount=100
```

`JSON` Ответ:
```json
{"id":"1_gdTSSewzsE","url":"https:\/\/pay.crystalpay.ru\/?i=1_gdTSSewzsE","error":false,"auth":"ok"}
```

## Получение статуса оплаты

| Параметр |     Описание     |
|:--------:|------------------|
|     o    | Операция (checkpay) |
|     n    | Имя/Логин кассы  |
|     s    | Секретный ключ 1 |
|  i       | ID Чека (Операции)    |

`GET` Пример запроса:

```sh
https://api.crystalpay.ru/api.php?s=0f5601d7150d663cc1dd05bbe035c0cb02b4bc8f&n=testkassa&o=checkpay&i=1_gdTSSewzsE
```

`JSON` Ответ:
```json
{"amount":"100","state":"notpayed","error":false,"auth":"ok"}
```
#### ИЛИ
```json
{"amount":"100","state":"payed","error":false,"auth":"ok"}
```

## Вывод средств с кассы

| Параметр |     Описание     |
|:--------:|------------------|
|     o    | Операция (withdraw) |
|     n    | Имя/Логин кассы  |
|     s    | Секретный ключ 1 |
|  secret  | Секретный ключ для вывода средств    |
|  amount  | Сумма в рубля    |
|  wallet  | Номер кошелька    |
|  service  | Сервис для вывода (Список доступен в боте)    |

`GET` Пример запроса:

```sh
https://api.crystalpay.ru/api.php?s=0f5601d7150d663cc1dd05bbe035c0cb02b4bc8f&n=testkassa&o=withdraw&wallet=79999999999999&amount=100&service=qw&secret=%СЕКРЕТНЫЙ_КЛЮЧ%
```

`JSON` Ответ:
```json
{"error":false,"auth":"ok"}
```

&nbsp;

`PHP` Пример генерации секретного ключа для вывода:

```php
$wallet = "79999999999999";
$amount = "100";
$secret2 = "7551ffeba39ddbf705d238c97ed9d26986f4c0f9"; //Секретный ключ 2

$withdraw_secret = md5($wallet.'-'.$amount.'-'.$secret2);
```

