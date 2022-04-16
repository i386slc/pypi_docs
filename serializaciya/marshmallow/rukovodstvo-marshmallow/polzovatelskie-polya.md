# Пользовательские поля

Существует три способа создания поля пользовательского формата для схемы **Schema**:

* Создайте пользовательский класс поля [Field](../api-marshmallow/polya-fields.md#class-marshmallow.fields.field-load\_default-typing.any-less-than-marshmallow.missing-greater-than-mi)
* Используйте поле [Method](../api-marshmallow/polya-fields.md#class-marshmallow.fields.method-serialize-str-or-none-none-deserialize-str-or-none-none-kwargs)
* Используйте поле [Function](../api-marshmallow/polya-fields.md#class-marshmallow.fields.function-serialize-none-or-callable-any-any-or-callable-any-dict-any-none-d)

Выбранный вами метод будет зависеть от того, как вы собираетесь повторно использовать поле.

## Создание класса поля Field

Чтобы создать класс пользовательского поля, создайте подкласс [marshmallow.fields.Field](../api-marshmallow/polya-fields.md#class-marshmallow.fields.field-load\_default-typing.any-less-than-marshmallow.missing-greater-than-mi) и реализуйте его методы \_serialize и/или \_deserialize.
