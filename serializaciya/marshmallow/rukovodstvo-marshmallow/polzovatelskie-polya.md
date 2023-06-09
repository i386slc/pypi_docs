# Пользовательские поля

Существует три способа создания поля пользовательского формата для схемы **Schema**:

* Создайте пользовательский класс поля [Field](../api-marshmallow/polya-fields-a-f.md#class-marshmallow.fields.field-load\_default-typing.any-less-than-marshmallow.missing-greater-than-mi)
* Используйте поле [Method](../api-marshmallow/polya-fields-a-f.md#class-marshmallow.fields.method-serialize-str-or-none-none-deserialize-str-or-none-none-kwargs)
* Используйте поле [Function](../api-marshmallow/polya-fields-a-f.md#class-marshmallow.fields.function-serialize-none-or-callable-any-any-or-callable-any-dict-any-none-d)

Выбранный вами метод будет зависеть от того, как вы собираетесь повторно использовать поле.

## Создание класса поля Field

Чтобы создать класс пользовательского поля, создайте подкласс [marshmallow.fields.Field](../api-marshmallow/polya-fields-a-f.md#class-marshmallow.fields.field-load\_default-typing.any-less-than-marshmallow.missing-greater-than-mi) и реализуйте его методы [\_serialize](../api-marshmallow/polya-fields-a-f.md#\_serialize-value-any-attr-str-obj-any-kwargs) и/или [\_deserialize](../api-marshmallow/polya-fields-a-f.md#\_deserialize-value-any-attr-str-or-none-data-mapping-str-any-or-none-kwargs).

```python
from marshmallow import fields, ValidationError

class PinCode(fields.Field):
    """Field that serializes to a string of numbers and deserializes
    to a list of numbers.
    """

    def _serialize(self, value, attr, obj, **kwargs):
        if value is None:
            return ""
        return "".join(str(d) for d in value)

    def _deserialize(self, value, attr, data, **kwargs):
        try:
            return [int(c) for c in value]
        except ValueError as error:
            raise ValidationError("Pin codes must contain only digits.") from error

class UserSchema(Schema):
    name = fields.String()
    email = fields.String()
    created_at = fields.DateTime()
    pin_code = PinCode()
```

## Поля Method

Поле метода [Method](../api-marshmallow/polya-fields-a-f.md#class-marshmallow.fields.method-serialize-str-or-none-none-deserialize-str-or-none-none-kwargs) будет сериализовано в значение, возвращаемое методом схемы. Метод должен принимать параметр **obj**, который является сериализуемым объектом.

```python
class UserSchema(Schema):
    name = fields.String()
    email = fields.String()
    created_at = fields.DateTime()
    since_created = fields.Method("get_days_since_created")

    def get_days_since_created(self, obj):
        return dt.datetime.now().day - obj.created_at.day
```

## Поля Function

Поле [Function](../api-marshmallow/polya-fields-a-f.md#class-marshmallow.fields.function-serialize-none-or-callable-any-any-or-callable-any-dict-any-none-d) будет сериализовать значение функции, которое передается непосредственно в него. Как и поле [Method](../api-marshmallow/polya-fields-a-f.md#class-marshmallow.fields.method-serialize-str-or-none-none-deserialize-str-or-none-none-kwargs), функция должна принимать один аргумент **obj**.

```python
class UserSchema(Schema):
    name = fields.String()
    email = fields.String()
    created_at = fields.DateTime()
    uppername = fields.Function(lambda obj: obj.name.upper())
```

## Десериализация полей Method и Function

И [Function](../api-marshmallow/polya-fields-a-f.md#class-marshmallow.fields.function-serialize-none-or-callable-any-any-or-callable-any-dict-any-none-d), и [Method](../api-marshmallow/polya-fields-a-f.md#class-marshmallow.fields.method-serialize-str-or-none-none-deserialize-str-or-none-none-kwargs) получают необязательный аргумент **deserialize**, который определяет способ десериализации поля. Метод или функция, переданные **deserialize**, получают входное значение для поля.

```python
class UserSchema(Schema):
    # `Method` принимает имя метода (str), Function принимает вызываемый объект
    balance = fields.Method("get_balance", deserialize="load_balance")

    def get_balance(self, obj):
        return obj.income - obj.debt

    def load_balance(self, value):
        return float(value)

schema = UserSchema()
result = schema.load({"balance": "100.00"})
result["balance"]  # => 100.0
```

## Добавление контекста к полям Method и Function

Поле [Function](../api-marshmallow/polya-fields-a-f.md#class-marshmallow.fields.function-serialize-none-or-callable-any-any-or-callable-any-dict-any-none-d) или [Method](../api-marshmallow/polya-fields-a-f.md#class-marshmallow.fields.method-serialize-str-or-none-none-deserialize-str-or-none-none-kwargs) может нуждаться в информации о своей среде, чтобы знать, как сериализовать значение.

В этих случаях вы можете установить атрибут **context** (словарь) схемы. Поля **Function** и **Method** будут иметь доступ к этому словарю.

Например, вы можете захотеть, чтобы ваша **UserSchema** выводила, является ли пользователь **User** автором блога **Blog** или появляется ли определенное слово в заголовке блога.

```python
class UserSchema(Schema):
    name = fields.String()
    # поля Function необязательно получают аргумент context
    is_author = fields.Function(lambda user, context: user == context["blog"].author)
    likes_bikes = fields.Method("writes_about_bikes")

    def writes_about_bikes(self, user):
        return "bicycle" in self.context["blog"].title.lower()

schema = UserSchema()

user = User("Freddie Mercury", "fred@queen.com")
blog = Blog("Bicycle Blog", author=user)

schema.context = {"blog": blog}
result = schema.dump(user)
result["is_author"]  # => True
result["likes_bikes"]  # => True
```

## Настройка сообщений об ошибках

Сообщения об ошибках проверки для полей можно настроить на уровне класса или экземпляра.

На уровне класса сообщения об ошибках по умолчанию определяются как сопоставление кодов ошибок с сообщениями об ошибках.

```python
from marshmallow import fields

class MyDate(fields.Date):
    default_error_messages = {"invalid": "Please provide a valid date."}
```

{% hint style="info" %}
Словарь **default\_error\_messages** поля _**объединяется**_ со словарями **default\_error\_messages** его родительских классов.
{% endhint %}

Сообщения об ошибках также могут быть переданы конструктору поля.

```python
from marshmallow import Schema, fields

class UserSchema(Schema):

    name = fields.Str(
        required=True, error_messages={"required": "Please provide a name."}
    )
```

## Следующие шаги

* Нужно добавить проверку на уровне схемы, постобработку или обработку ошибок? См. страницу «[Расширенные схемы](rasshirennye-skhemy.md)».
* Примеры приложений, использующих **marshmallow**, см. на странице [примеров](primery-marshmallow.md).
