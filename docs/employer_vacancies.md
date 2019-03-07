# Вакансии для работодателя

<a name="creation"></a>
## Публикация вакансий

`POST /vacancies/`

### Общая информация

В качестве тела запроса необходимо передавать
[JSON](general.md#request-body) с данными размещаемой вакансии.

* при успешной публикации будут списаны соответствующие услуги.
* все вакансии проходят ручную модерацию.
* в течение нескольких минут после публикации вакансия станет доступна в поиске.

### Полезные ссылки

* [правила размещения вакансий](https://www.mjobs.ru/info/rules-vacancies/)

<a name="creation-example"></a>
### Пример тела запроса

```json
{
    "position": "Категорийный менеджер",
    "code": "Менеджер Москва",
    
    "categories": [
        {
            "id": 33
        },
        {
            "id": 21
        }
    ],
    
    "city_id": 606,
    "metro": [
        {
            "id": 294
        },
        {   
            "id": 297
        }
    ],
    "moving": false,
    
    "salary_from": 50000,
    "salary_to": 0,
    
    "workday": 7,
    "workplace": 3,
    "business_trip": 0,
    "conditions": "Условия работы",
    "obligations": "Обязанности",
    "requirements": "Требования к кандидату",
    "education": 3,
    "experience": 2,
    "languages": [
        {
            "id": 1, 
            "level": 4
        },
        {   
            "id": 2, 
            "level": 2
        }
    ],
    "driver_license": [2, 3],
    "for_invalid": true,
    
    "contact_fio": "Петров Василий",
    "phones": [
        {
            "country": "7",
            "city": "921",
            "number": "12312312"
        }
    ],
    "phone_comment": "Рабочие дни, с 10 до 18",
    "email": "mail@site.ru",
    "contacts": "дополнительные контакты",
    
    "subscribe": true,
    "allow_guest_requests": true,
    
    "type": 0,
    "duration": 1,
    "status": 2,
    
    "rules": true
}
```


<a name="creation_fields"></a>
### Поля запроса

* `[]` (например, в полях специализации и контактах) обозначает, что значение данного ключа является массивом объектов.
* `a.b` обозначает объект `a` с ключом `b` описанного типа.
* `*` - обязательные поля.

Путь | JSON тип | Описание
---- | -------- | --------
position `*` | string | название вакансии
code | string | внутренний код вакансии (не виден соискателям)
categories `*` | array | список категорий
categories[].id | numeric | категория [из справочника](sections.md)
city_id `*` | numeric | город публикации [из справочника](cities.md)
metro | array | список станций метро
metro[].id | numeric | станция [из справочника](metro.md)
moving | boolean | рассматриваются ли кандидаты из других городов
salary_from | numeric или null | нижняя граница зарплаты
salary_to | numeric или null | верхняя граница зарплаты
workday `*` | numeric | тип занятости из [справочника vacancy_workday](dictionaries.md#vacancy)
workplace `*` | numeric | тип занятости из [справочника vacancy_workplace](dictionaries.md#vacancy)
business_trip | boolean | наличие командировок
conditions `*` | string | условия работы
obligations `*` | string | обязанности
requirements | string | требования
education | numeric | уровень образования из [справочника vacancy_education](dictionaries.md#vacancy)
experience | numeric | опыт работы из [справочника vacancy_experience](dictionaries.md#vacancy)
languages | array | список требуемых иностранных языков
languages[].id | numeric | язык [из справочника](languages.md)
languages[].level | numeric | уровень владения языком [из справочника language_level](dictionaries.md#etc)
driver_license | array | список требуемых категорий водительских прав [из справочника driver_licence](dictionaries.md#etc)
for_invalid | boolean | доступность вакансии для лиц с ограниченными возможностями
contact_fio `*` | string | ФИО рекрутера
phones[] `*` | array | телефоны для связи
phones[].country | string | код страны
phones[].city | string | код города
phones[].number | string | телефон
phone_comment | string | комментарий по способам связи
email `*` | string | адрес электронной почты для уведомлений
contacts | string | дополнительные контакты
subscribe | boolean | подписка на подборки резюме
allow_guest_requests | boolean | разрешен ли отклик без резюме
type `*` | numeric | тип публикуемой вакансии из [справочника vacancy_type](dictionaries.md#vacancy)
duration | numeric | длительность спецпродвижения из [справочника vacancy_duration](dictionaries.md#vacancy)
status `*` | numeric | область видимости вакансии [справочника vacancy_status](dictionaries.md#vacancy)
rules `*` | boolean | согласие с [правилами публикации вакансий](https://www.mjobs.ru/info/rules-vacancies/)

<a name="creation-results"></a>
### Ответ

При успешном выполнении запроса вернется `201 Created`. В заголовке `Location` будет
  содержаться ссылка на добавленную вакансию:

```
HTTP/1.1 201 Created

Location: https://msk.mjobs.ru/vacancy/321654/
```

В теле ответа возвращается идентификатор добавленной вакансии:

```json
{
    "id": "321654"
}
```

### Ошибки

* `401 Unauthorized` – неавторизованный запрос.
* `403 Forbidden` – добавление вакансий недоступно данному пользователю.
* `422 Unprocessable Entity` – ошибки в полях при добавлении вакансии.

Дополнительно к HTTP коду сервер может вернуть описание
[причины ошибки](errors.md#vacancies-create).