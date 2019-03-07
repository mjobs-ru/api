# Общая информация

* API работает по протоколу HTTPS.
* Авторизация осуществляется по протоколу OAuth2.
* Все данные доступны только в формате JSON.

<a name="request-body"></a>
### Формат запроса при отправке JSON

Данные, передающиеся в теле запроса, должны удовлетворять требованиям:

* Валидный JSON.
* Рекомендуется использование кодировки UTF-8 без дополнительного экранирования
  (`{"position": "Менеджер по продажам"}`).
* К типам данных в определённым полях накладываются дополнительные условия,
  описанные в каждом конкретном методе. В JSON типами данных являются `string`,
  `number`, `boolean`, `null`, `object`, `array`.

<a name="response"></a>
### Ответ
Ответ свыше определенной длины может сжиматься методом gzip.

<a name="errors-and-codes"></a>
### Ошибки и коды ответов

API широко использует информирование при помощи кодов ответов.
Приложение должно корректно их обрабатывать.

В случае неполадок и сбоев, возможны ответы с кодом `503` и `500`.

При каждой ошибке, помимо кода ответа, в теле ответа может быть выдана
дополнительная информация, позволяющая разработчику понять
причину соответствующего ответа.

[Более подробно про возможные ошибки](errors.md).


<a name="deprecated"></a>
### Недокументированные поля и параметры запросов

В ответах и параметрах API можно найти ключи не описанные в документации.
Обычно это означает, что они оставлены для совместимости со старыми версиями.
Их использование не рекомендуется. Если ваше приложение использует такие ключи,
перейдите на использование актуальных ключей, описанных в документации.