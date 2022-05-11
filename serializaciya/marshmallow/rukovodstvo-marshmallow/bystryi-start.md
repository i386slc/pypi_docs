# Быстрый старт

Это руководство познакомит вас с основами создания схем для сериализации и десериализации данных.

## Объявление схем

Начнем с базовой модели пользователя.

```python
import datetime as dt

class User:
    def __init__(self, name, email):
        self.name = name
        self.email = email
        self.created_at = dt.datetime.now()

    def __repr__(self):
        return "<User(name={self.name!r})>".format(self=self)
```

Создайте схему, определив класс с переменными, сопоставляющими имена атрибутов объектам [Field](../api-marshmallow/polya-fields-a-f.md#class-marshmallow.fields.field-load\_default-typing.any-less-than-marshmallow.missing-greater-than-mi).

```python
from marshmallow import Schema, fields

class UserSchema(Schema):
    name = fields.Str()
    email = fields.Email()
    created_at = fields.DateTime()
```

{% hint style="info" %}
Смотри также:

Полную справку по доступным классам полей см. в [документации по API](../api-marshmallow/).
{% endhint %}

## Создание схем из словарей

Вы можете создать схему из словаря полей, используя метод [from\_dict](../api-marshmallow/skhema-schema.md#classmethod-from\_dict-fields-dict-str-ma\_fields.field-or-type-name-str-generatedschema-type).

```python
from marshmallow import Schema, fields

UserSchema = Schema.from_dict(
    {
        "name": fields.Str(),
        "email": fields.Email(),
        "created_at": fields.DateTime()
    }
)
```

[from\_dict](../api-marshmallow/skhema-schema.md#classmethod-from\_dict-fields-dict-str-ma\_fields.field-or-type-name-str-generatedschema-type) особенно полезен для создания схем во время выполнения.

## Сериализация объектов («дампинг»)

Сериализует объекты, передав их методу [dump](../api-marshmallow/skhema-schema.md#dump-obj-any-many-bool-or-none-none) вашей схемы, который возвращает отформатированный результат.

```python
from pprint import pprint

user = User(name="Monty", email="monty@python.org")
schema = UserSchema()
result = schema.dump(user)
pprint(result)
# {"name": "Monty",
#  "email": "monty@python.org",
#  "created_at": "2014-08-17T14:54:16.049594+00:00"}
```

Вы также можете сериализовать строку в кодировке JSON, используя [dumps](../api-marshmallow/skhema-schema.md#dumps-obj-any-args-many-bool-or-none-none-kwargs).

```python
json_result = schema.dumps(user)
pprint(json_result)
# '{
      "name": "Monty",
      "email": "monty@python.org",
      "created_at": "2014-08-17T14:54:16.049594+00:00"
   }'
```

## Фильтрация вывода

Вам может не понадобиться выводить все объявленные поля каждый раз, когда вы используете схему. Вы можете указать, какие поля выводить с параметром **only**.

```python
summary_schema = UserSchema(only=("name", "email"))
summary_schema.dump(user)
# {"name": "Monty", "email": "monty@python.org"}
```

Вы также можете исключить поля, передав параметр **exclude**.

## Десериализация объектов («Загрузка»)

Обратной стороной метода [dump](../api-marshmallow/skhema-schema.md#dump-obj-any-many-bool-or-none-none) является [load](../api-marshmallow/skhema-schema.md#load-data-mapping-str-any-or-iterable-mapping-str-any-many-bool-or-none-none-partial-bool-or-types.s), который проверяет и десериализует входной словарь в структуру данных уровня приложения.

По умолчанию [load](../api-marshmallow/skhema-schema.md#load-data-mapping-str-any-or-iterable-mapping-str-any-many-bool-or-none-none-partial-bool-or-types.s) вернет словарь имен полей, сопоставленных с десериализованными значениями (или вызовет [ValidationError](../api-marshmallow/isklyucheniya.md#exception-marshmallow.exceptions.validationerror-message-str-or-list-or-dict-field\_name-str-\_schema) со словарем ошибок проверки, к которым мы [вернемся позже](bystryi-start.md#undefined)).

```python
from pprint import pprint

user_data = {
    "created_at": "2014-08-11T05:26:03.869245",
    "email": "ken@yahoo.com",
    "name": "Ken",
}
schema = UserSchema()
result = schema.load(user_data)
pprint(result)
# {'name': 'Ken',
#  'email': 'ken@yahoo.com',
#  'created_at': datetime.datetime(2014, 8, 11, 5, 26, 3, 869245)},
```

Обратите внимание, что строка даты и времени была преобразована в объект [datetime](https://python.readthedocs.io/en/latest/library/datetime.html#module-datetime).

### Десериализация в объекты

Чтобы десериализовать объект, определите метод вашей схемы [Schema](../api-marshmallow/#class-marshmallow.schema-only-types.strsequenceorset-or-none-none-exclude-types.strsequenceorset-man) и декорируйте его [post\_load](../api-marshmallow/dekoratory.md#marshmallow.decorators.post\_load-fn-callable-...-any-or-none-none-pass\_many-bool-false-pass\_original). Метод получает словарь десериализованных данных.

```python
from marshmallow import Schema, fields, post_load

class UserSchema(Schema):
    name = fields.Str()
    email = fields.Email()
    created_at = fields.DateTime()

    @post_load
    def make_user(self, data, **kwargs):
        return User(**data)
```

Теперь метод загрузки [load](../api-marshmallow/skhema-schema.md#load-data-mapping-str-any-or-iterable-mapping-str-any-many-bool-or-none-none-partial-bool-or-types.s) возвращает экземпляр пользователя **User**.

```python
user_data = {"name": "Ronnie", "email": "ronnie@stones.com"}
schema = UserSchema()
result = schema.load(user_data)
print(result)  # => <User(name='Ronnie')>
```

## Обработка коллекций объектов

Установите `many=True` при работе с итерируемыми коллекциями объектов.

```python
user1 = User(name="Mick", email="mick@stones.com")
user2 = User(name="Keith", email="keith@stones.com")
users = [user1, user2]
schema = UserSchema(many=True)
result = schema.dump(users)  # OR UserSchema().dump(users, many=True)
pprint(result)
# [{'name': u'Mick',
#   'email': u'mick@stones.com',
#   'created_at': '2014-08-17T14:58:57.600623+00:00'}
#  {'name': u'Keith',
#   'email': u'keith@stones.com',
#   'created_at': '2014-08-17T14:58:57.600623+00:00'}]
```

## Валидация

[Schema.load()](../api-marshmallow/#load-data-mapping-str-any-or-iterable-mapping-str-any-many-bool-or-none-none-partial-bool-or-types.s) (и ее аналог для декодирования JSON, [Schema.loads()](../api-marshmallow/#loads-json\_data-str-many-bool-or-none-none-partial-bool-or-types.strsequenceorset-or-none-none-unkno)) вызывает ошибку [ValidationError](../api-marshmallow/isklyucheniya.md#exception-marshmallow.exceptions.validationerror-message-str-or-list-or-dict-field\_name-str-\_schema) при передаче недопустимых данных. Вы можете получить доступ к словарю ошибок проверки из атрибута `ValidationError.messages`. Данные, которые были правильно десериализованы, доступны в `ValidationError.valid_data`. Некоторые поля, такие как поля [Email](../api-marshmallow/polya-fields-a-f.md#class-marshmallow.fields.email-args-kwargs) и [URL](../api-marshmallow/polya-fields-a-f.md#marshmallow.fields.url), имеют встроенную проверку.

```python
from marshmallow import ValidationError

try:
    result = UserSchema().load({"name": "John", "email": "foo"})
except ValidationError as err:
    print(err.messages)  # => {"email": ['"foo" is not a valid email address.']}
    print(err.valid_data)  # => {"name": "John"}
```

При проверке коллекции словарь ошибок будет содержать индексы недопустимых элементов.

```python
from pprint import pprint

from marshmallow import Schema, fields, ValidationError

class BandMemberSchema(Schema):
    name = fields.String(required=True)
    email = fields.Email()

user_data = [
    {"email": "mick@stones.com", "name": "Mick"},
    {"email": "invalid", "name": "Invalid"},  # invalid email
    {"email": "keith@stones.com", "name": "Keith"},
    {"email": "charlie@stones.com"},  # missing "name"
]

try:
    BandMemberSchema(many=True).load(user_data)
except ValidationError as err:
    pprint(err.messages)
    # {1: {'email': ['Not a valid email address.']},
    #  3: {'name': ['Missing data for required field.']}}
```

Вы можете выполнить дополнительную проверку поля, передав аргумент **validate**. В модуле [marshmallow.validate](../api-marshmallow/validatory.md) есть несколько встроенных валидаторов.

```python
from pprint import pprint

from marshmallow import Schema, fields, validate, ValidationError

class UserSchema(Schema):
    name = fields.Str(validate=validate.Length(min=1))
    permission = fields.Str(validate=validate.OneOf(["read", "write", "admin"]))
    age = fields.Int(validate=validate.Range(min=18, max=40))

in_data = {"name": "", "permission": "invalid", "age": 71}
try:
    UserSchema().load(in_data)
except ValidationError as err:
    pprint(err.messages)
    # {'age': ['Must be greater than or equal to 18 and less than or equal to 40.'],
    #  'name': ['Shorter than minimum length 1.'],
    #  'permission': ['Must be one of: read, write, admin.']}
```

Вы можете реализовать свои собственные валидаторы. Валидатор — это вызываемый объект, который принимает один аргумент — значение для проверки. Если проверка не пройдена, вызываемый объект должен вызвать [ValidationError](../api-marshmallow/isklyucheniya.md#exception-marshmallow.exceptions.validationerror-message-str-or-list-or-dict-field\_name-str-\_schema) с полезным сообщением об ошибке или вернуть `False` (для общего сообщения об ошибке).

```python
from marshmallow import Schema, fields, ValidationError

def validate_quantity(n):
    if n < 0:
        raise ValidationError("Quantity must be greater than 0.")
    if n > 30:
        raise ValidationError("Quantity must not be greater than 30.")

class ItemSchema(Schema):
    quantity = fields.Integer(validate=validate_quantity)

in_data = {"quantity": 31}
try:
    result = ItemSchema().load(in_data)
except ValidationError as err:
    print(err.messages)  # => {'quantity': ['Quantity must not be greater than 30.']}
```

Вы также можете передать коллекцию (список, кортеж, генератор) вызываемых объектов для проверки.

{% hint style="warning" %}
Проверка происходит при десериализации, но не при сериализации. Чтобы повысить производительность сериализации, данные, переданные в [Schema.dump()](../api-marshmallow/#dump-obj-any-many-bool-or-none-none), считаются допустимыми.
{% endhint %}

{% hint style="info" %}
Вы можете зарегистрировать пользовательскую функцию обработчика ошибок для схемы, переопределив метод [handle\_error](../api-marshmallow/#handle\_error-error-marshmallow.exceptions.validationerror-data-any-many-bool-kwargs). Дополнительную информацию см. на странице «[Расширенные схемы](rasshirennye-skhemy.md)».
{% endhint %}

{% hint style="info" %}
Нужна проверка на уровне схемы? См. страницу «[Расширенные схемы](rasshirennye-skhemy.md)».
{% endhint %}

### Валидаторы полей как методы

Иногда удобно писать валидаторы как методы. Используйте декоратор [validates](../api-marshmallow/dekoratory.md#marshmallow.decorators.validates-field\_name-str-callable-...-any) для регистрации методов проверки поля.

```python
from marshmallow import fields, Schema, validates, ValidationError

class ItemSchema(Schema):
    quantity = fields.Integer()

    @validates("quantity")
    def validate_quantity(self, value):
        if value < 0:
            raise ValidationError("Quantity must be greater than 0.")
        if value > 30:
            raise ValidationError("Quantity must not be greater than 30.")
```

## Обязательные поля

Сделайте поле обязательным, передав `required=True`. Ошибка будет вызвана, если значение отсутствует на входе в [Schema.load()](../api-marshmallow/#load-data-mapping-str-any-or-iterable-mapping-str-any-many-bool-or-none-none-partial-bool-or-types.s).

Чтобы настроить сообщение об ошибке для обязательных полей, передайте [dict](https://python.readthedocs.io/en/latest/library/stdtypes.html#dict) с ключом **required** в качестве аргумента **error\_messages** для поля.

```python
from pprint import pprint

from marshmallow import Schema, fields, ValidationError

class UserSchema(Schema):
    name = fields.String(required=True)
    age = fields.Integer(
        required=True,
        error_messages={"required": "Age is required."}
    )
    city = fields.String(
        required=True,
        error_messages={"required": {"message": "City required", "code": 400}},
    )
    email = fields.Email()

try:
    result = UserSchema().load({"email": "foo@bar.com"})
except ValidationError as err:
    pprint(err.messages)
    # {'age': ['Age is required.'],
    # 'city': {'code': 400, 'message': 'City required'},
    # 'name': ['Missing data for required field.']}
```

## Частичная загрузка

При использовании одной и той же схемы в нескольких местах вы можете только пропустить проверку **required** путем передачи **partial**.

```python
class UserSchema(Schema):
    name = fields.String(required=True)
    age = fields.Integer(required=True)

result = UserSchema().load({"age": 42}, partial=("name",))
# OR UserSchema(partial=('name',)).load({'age': 42})
print(result)  # => {'age': 42}
```

Вы можете полностью игнорировать все отсутствующие поля, установив `partial=True`.

```python
class UserSchema(Schema):
    name = fields.String(required=True)
    age = fields.Integer(required=True)

result = UserSchema().load({"age": 42}, partial=True)
# OR UserSchema(partial=True).load({'age': 42})
print(result)  # => {'age': 42}
```

## Указание значений по умолчанию

**load\_default** указывает значение десериализации по умолчанию для поля. Аналогично, **dump\_default** указывает значение сериализации по умолчанию.

```python
class UserSchema(Schema):
    id = fields.UUID(load_default=uuid.uuid1)
    birthdate = fields.DateTime(dump_default=dt.datetime(2017, 9, 29))

UserSchema().load({})
# {'id': UUID('337d946c-32cd-11e8-b475-0022192ed31b')}
UserSchema().dump({})
# {'birthdate': '2017-09-29T00:00:00+00:00'}
```

## Обработка неизвестных полей

По умолчанию [load](../api-marshmallow/#load-data-mapping-str-any-or-iterable-mapping-str-any-many-bool-or-none-none-partial-bool-or-types.s) вызовет [ValidationError](../api-marshmallow/isklyucheniya.md#exception-marshmallow.exceptions.validationerror-message-str-or-list-or-dict-field\_name-str-\_schema), если обнаружит ключ без соответствующего поля **Field** в схеме.

Это поведение можно изменить с помощью параметра **unknown**, который принимает одно из следующих значений:

* **RAISE** (по умолчанию): поднять [ValidationError](../api-marshmallow/isklyucheniya.md#exception-marshmallow.exceptions.validationerror-message-str-or-list-or-dict-field\_name-str-\_schema), если есть какие-либо неизвестные поля
* **EXCLUDE**: исключить неизвестные поля
* **INCLUDE**: принять и включить неизвестные поля

Вы можете указать **unknown** в классе **Meta** вашей схемы [Schema](../api-marshmallow/#class-marshmallow.schema-only-types.strsequenceorset-or-none-none-exclude-types.strsequenceorset-man),

```python
from marshmallow import Schema, INCLUDE

class UserSchema(Schema):
    class Meta:
        unknown = INCLUDE
```

во время создания экземпляра,

```python
schema = UserSchema(unknown=INCLUDE)
```

или при вызове [load](../api-marshmallow/#load-data-mapping-str-any-or-iterable-mapping-str-any-many-bool-or-none-none-partial-bool-or-types.s).

```python
UserSchema().load(data, unknown=INCLUDE)
```

Значение параметра **unknown**, установленное при загрузке [load](../api-marshmallow/#load-data-mapping-str-any-or-iterable-mapping-str-any-many-bool-or-none-none-partial-bool-or-types.s), переопределит значение, примененное во время создания экземпляра, которое само переопределит значение, определенное в классе **Meta**.

Этот порядок приоритета позволяет изменять поведение схемы для различных контекстов.

## Проверка без десериализации

Если вам нужно только проверить входные данные (без десериализации в объект), вы можете использовать [Schema.validate()](../api-marshmallow/#validate-data-mapping-str-any-or-iterable-mapping-str-any-many-bool-or-none-none-partial-bool-or-typ).

```python
errors = UserSchema().validate({"name": "Ronnie", "email": "invalid-email"})
print(errors)  # {'email': ['Not a valid email address.']}
```

## Поля «Read-only» и «Write-only»

В контексте веб-API параметры **dump\_only** и **load\_only** концептуально эквивалентны полям «только для чтения» и «только для записи» соответственно.

```python
class UserSchema(Schema):
    name = fields.Str()
    # password is "write-only"
    password = fields.Str(load_only=True)
    # created_at is "read-only"
    created_at = fields.DateTime(dump_only=True)
```

{% hint style="warning" %}
При загрузке поля **dump-only** считаются неизвестными. Если для параметра **unknown** установлено значение **INCLUDE**, значения с ключами, соответствующими этим полям, загружаются без проверки.
{% endhint %}

## Указание ключей сериализации/десериализации

Схемы будут (де)сериализовать входной словарь из/в выходной словарь, ключи которого идентичны именам полей. Если вы потребляете и производите данные, которые не соответствуют вашей схеме, вы можете указать выходные ключи с помощью аргумента **data\_key**.

```python
class UserSchema(Schema):
    name = fields.String()
    email = fields.Email(data_key="emailAddress")

s = UserSchema()

data = {"name": "Mike", "email": "foo@bar.com"}
result = s.dump(data)
# {'name': u'Mike',
# 'emailAddress': 'foo@bar.com'}

data = {"name": "Mike", "emailAddress": "foo@bar.com"}
result = s.load(data)
# {'name': u'Mike',
# 'email': 'foo@bar.com'}
```

## Неявное создание поля

Когда ваша модель имеет много атрибутов, указание типа поля для каждого атрибута может стать повторяющимся, особенно если многие из атрибутов уже являются собственными типами данных Python.

Параметр **fields** позволяет указать неявно созданные поля. **Marshmallow** выберет соответствующий тип поля на основе типа атрибута.

Давайте реорганизуем нашу схему **User**, чтобы сделать ее более лаконичной.

```python
class UserSchema(Schema):
    uppername = fields.Function(lambda obj: obj.name.upper())

    class Meta:
        fields = ("name", "email", "created_at", "uppername")
```

Обратите внимание, что **name** будет автоматически отформатировано как строка [String](../api-marshmallow/polya-fields-a-f.md#class-marshmallow.fields.string-load\_default-typing.any-less-than-marshmallow.missing-greater-than-m), а **created\_at** будет отформатировано как [DateTime](../api-marshmallow/polya-fields-a-f.md#class-marshmallow.fields.datetime-format-str-or-none-none-kwargs).

{% hint style="info" %}
Если вместо этого вы хотите указать, какие имена полей следует включать _**в дополнение**_ к явно объявленным полям, вы можете использовать дополнительную опцию **additional**.

Схема ниже эквивалентна приведенной выше:

```python
class UserSchema(Schema):
    uppername = fields.Function(lambda obj: obj.name.upper())

    class Meta:
        # Нет необходимости включать 'uppername'
        additional = ("name", "email", "created_at")
```
{% endhint %}

## Упорядочивание вывода

Чтобы сохранить порядок полей, установите для параметра **ordered** значение `True`. Это заставит **marshmallow** сериализовать данные в [collections.OrderedDict](https://python.readthedocs.io/en/latest/library/collections.html#collections.OrderedDict).

```python
from collections import OrderedDict
from pprint import pprint

from marshmallow import Schema, fields

class UserSchema(Schema):
    first_name = fields.String()
    last_name = fields.String()
    email = fields.Email()

    class Meta:
        ordered = True

u = User("Charlie", "Stones", "charlie@stones.com")
schema = UserSchema()
result = schema.dump(u)
assert isinstance(result, OrderedDict)
pprint(result, indent=2)
#  OrderedDict([('first_name', 'Charlie'),
#              ('last_name', 'Stones'),
#              ('email', 'charlie@stones.com')])
```

## Следующие шаги

* Нужно представить отношения между объектами? См. страницу [вложенные схемы](vlozhennye-skhemy.md).
* Хотите создать свой собственный тип поля? См. страницу [настраиваемых полей](polzovatelskie-polya.md).
* Нужно добавить проверку на уровне схемы, постобработку или обработку ошибок? См. страницу «[Расширенные схемы](rasshirennye-skhemy.md)».
* Примеры приложений, использующих **marshmallow**, см. на странице [примеров](primery-marshmallow.md).
