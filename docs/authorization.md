# Авторизация

Для выполнения большинства запросов к API необходимо передавать access-токен.
Авторизация осуществляется по протоколу `OAuth 2.0`

* [Получение access-токена](#get-access_token)
* [Использование access-токена](#use-access_token)

<a name="get-access_token"></a>
## Получение access-токена

### Запрос

Для получения `access_token` необходимо сделать запрос:

```
POST https://www.mjobs.ru/api/v1/token/
```

В теле запроса необходимо передать дополнительные параметры:

* `grant_type=client_credentials`
* `client_id` и `client_secret` - необходимо заполнить значениями, 
выданными при регистрации приложения.

Тело запроса необходимо передавать в стандартном 
`application/x-www-form-urlencoded` с указанием соответствующего заголовка `Content-Type`.

### Ответ

В ответе вернётся JSON:

```json
{
    "access_token": "{access_token}",
    "token_type": "bearer"
}
```

Данный `access_token` имеет **неограниченный** срок жизни. При повторном запросе ранее выданный токен отзывается и выдается новый. Запрашивать `access_token` можно не чаще, чем один раз в 5 минут.

> :warning: В случае компрометации токена необходимо запросить токен заново!

<a name="use-access_token"></a>
## Использование access-токена

Приложение должно использовать полученный `access_token` для авторизации,
передавая его в заголовке в формате:

```Authorization: Bearer ACCESS_TOKEN```

<a name="check-access_token"></a>
## Проверка access-токена

Для тестирования токена удобно использовать метод `/me/`

Пример запроса:

```http
GET /api/v1/me/ HTTP/1.1
User-Agent: MyApp/1.0 (my-app-feedback@example.com)
Host: www.mjobs.ru
Accept: */*
Authorization: Bearer access_token
```

### Ответ

Успешный ответ приходит с кодом `200 OK` и содержит тело:

```json
{
    "user_id": "12345678",
    "last_name": "Фамилия",
    "first_name": "Имя",
    "company_id": "87654321",
    "company_name": "Название компании"
}
```


 Имя | Тип | Описание
 --- | --- | ---
 user_id | число | идентификатор пользователя
 last_name | строка | фамилия
 first_name | строка | имя
 company_id | число | идентификатор пользователя
 company_name | строка | название компании
