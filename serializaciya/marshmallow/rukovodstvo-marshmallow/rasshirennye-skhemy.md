# Расширенные схемы

## Методы предварительной и последующей обработки

Методы предварительной и последующей обработки данных можно зарегистрировать с помощью декораторов [pre\_load](../api-marshmallow/dekoratory.md#marshmallow.decorators.pre\_load-fn-callable-...-any-or-none-none-pass\_many-bool-false-callable-...-a), [post\_load](../api-marshmallow/dekoratory.md#marshmallow.decorators.post\_load-fn-callable-...-any-or-none-none-pass\_many-bool-false-pass\_original), [pre\_dump](../api-marshmallow/dekoratory.md#marshmallow.decorators.pre\_dump-fn-callable-...-any-or-none-none-pass\_many-bool-false-callable-...-a) и [post\_dump](../api-marshmallow/dekoratory.md#marshmallow.decorators.post\_dump-fn-callable-...-any-or-none-none-pass\_many-bool-false-pass\_original).

```python
from marshmallow import Schema, fields, post_load

class UserSchema(Schema):
    name = fields.Str()
    slug = fields.Str()

    @post_load
    def slugify_name(self, in_data, **kwargs):
        in_data["slug"] = in_data["slug"].lower().strip().replace(" ", "-")
        return in_data

schema = UserSchema()
result = schema.load({"name": "Steve", "slug": "Steve Loria "})
result["slug"]  # => 'steve-loria'
```

### Прохождение «many»

По умолчанию методы предварительной и последующей обработки получают по одному объекту/данному за раз, прозрачно обрабатывая параметр **many**, передаваемые методу [dump()](../api-marshmallow/#dump-obj-any-many-bool-or-none-none) / [load()](../api-marshmallow/#load-data-mapping-str-any-or-iterable-mapping-str-any-many-bool-or-none-none-partial-bool-or-types.s) схемы во время выполнения.

В тех случаях, когда ваши методы предварительной и последующей обработки должны обрабатывать входную коллекцию при обработке нескольких объектов, добавьте `pass_many=True` в декораторы методов.

Затем ваш метод получит входные данные (которые могут быть отдельными данными или набором, в зависимости от вызова **dump/load**).

### Пример: конвертирование

Одним из распространенных вариантов использования является перенос данных в пространство имен при сериализации и распаковка данных во время десериализации.

```python
from marshmallow import Schema, fields, pre_load, post_load, post_dump

class BaseSchema(Schema):
    # Пользовательские параметры
    __envelope__ = {"single": None, "many": None}
    __model__ = User

    def get_envelope_key(self, many):
        """Helper to get the envelope key."""
        key = self.__envelope__["many"] if many else self.__envelope__["single"]
        assert key is not None, "Envelope key undefined"
        return key

    @pre_load(pass_many=True)
    def unwrap_envelope(self, data, many, **kwargs):
        key = self.get_envelope_key(many)
        return data[key]

    @post_dump(pass_many=True)
    def wrap_with_envelope(self, data, many, **kwargs):
        key = self.get_envelope_key(many)
        return {key: data}

    @post_load
    def make_object(self, data, **kwargs):
        return self.__model__(**data)

class UserSchema(BaseSchema):
    __envelope__ = {"single": "user", "many": "users"}
    __model__ = User
    name = fields.Str()
    email = fields.Email()

user_schema = UserSchema()

user = User("Mick", email="mick@stones.org")
user_data = user_schema.dump(user)
# {'user': {'email': 'mick@stones.org', 'name': 'Mick'}}

users = [
    User("Keith", email="keith@stones.org"),
    User("Charlie", email="charlie@stones.org"),
]
users_data = user_schema.dump(users, many=True)
# {'users': [{'email': 'keith@stones.org', 'name': 'Keith'},
#            {'email': 'charlie@stones.org', 'name': 'Charlie'}]}

user_objs = user_schema.load(users_data, many=True)
# [<User(name='Keith Richards')>, <User(name='Charlie Watts')>]
```

### Вызов ошибок в методах пре-/постпроцессора

Методы предварительной и последующей обработки могут вызвать ошибку [ValidationError](../api-marshmallow/isklyucheniya.md#exception-marshmallow.exceptions.validationerror-message-str-or-list-or-dict-field\_name-str-\_schema). По умолчанию ошибки будут храниться в ключе "**\_schema**" в словаре ошибок.

```python
from marshmallow import Schema, fields, ValidationError, pre_load

class BandSchema(Schema):
    name = fields.Str()

    @pre_load
    def unwrap_envelope(self, data, **kwargs):
        if "data" not in data:
            raise ValidationError('Input data must have a "data" key.')
        return data["data"]

sch = BandSchema()
try:
    sch.load({"name": "The Band"})
except ValidationError as err:
    err.messages
# {'_schema': ['Input data must have a "data" key.']}
```

Если вы хотите сохранить ошибку в другом ключе, передайте имя ключа в качестве второго аргумента [ValidationError](../api-marshmallow/isklyucheniya.md#exception-marshmallow.exceptions.validationerror-message-str-or-list-or-dict-field\_name-str-\_schema).

```python
from marshmallow import Schema, fields, ValidationError, pre_load

class BandSchema(Schema):
    name = fields.Str()

    @pre_load
    def unwrap_envelope(self, data, **kwargs):
        if "data" not in data:
            raise ValidationError(
                'Input data must have a "data" key.', "_preprocessing"
            )
        return data["data"]

sch = BandSchema()
try:
    sch.load({"name": "The Band"})
except ValidationError as err:
    err.messages
# {'_preprocessing': ['Input data must have a "data" key.']}
```

### Порядок вызова пре/постпроцессора

Таким образом, конвейер обработки для _**десериализации**_ выглядит следующим образом:

1. `@pre_load(pass_many=True)` методы
2. `@pre_load(pass_many=False)` методы
3. `load(in_data, many)` (валидация и десериализация)
4. `@validates` методы (валидаторы поля)
5. `@validates_schema` методы (валидаторы схемы)
6. `@post_load(pass_many=True)` методы
7. `@post_load(pass_many=False)` методы

Конвейер для сериализации аналогичен, за исключением того, что процессоры `pass_many=True` вызываются после процессоров `pass_many=False` и отсутствуют валидаторы.

1. `@pre_dump(pass_many=False)` методы
2. `@pre_dump(pass_many=True)` методы
3. `dump(obj, many)` (сериализация)
4. `@post_dump(pass_many=False)` методы
5. `@post_dump(pass_many=True)` методы

{% hint style="warning" %}
Вы можете зарегистрировать несколько методов процессора на схеме. Однако имейте в виду, что порядок вызова оформленных методов одного и того же типа не гарантируется. Если вам нужно гарантировать порядок шагов обработки, вы должны поместить их в один и тот же метод.
{% endhint %}

```python
from marshmallow import Schema, fields, pre_load

# YES
class MySchema(Schema):
    field_a = fields.Field()

    @pre_load
    def preprocess(self, data, **kwargs):
        step1_data = self.step1(data)
        step2_data = self.step2(step1_data)
        return step2_data

    def step1(self, data):
        do_step1(data)

    # Зависит от step1
    def step2(self, data):
        do_step2(data)

# NO
class MySchema(Schema):
    field_a = fields.Field()

    @pre_load
    def step1(self, data, **kwargs):
        do_step1(data)

    # Зависит от step1
    @pre_load
    def step2(self, data, **kwargs):
        do_step2(data)
```

## Проверка на уровне схемы

Вы можете зарегистрировать функции проверки на уровне схемы для [Schema](../api-marshmallow/#class-marshmallow.schema-only-types.strsequenceorset-or-none-none-exclude-types.strsequenceorset-man), используя декоратор [marshmallow.validates\_schema](../api-marshmallow/dekoratory.md#marshmallow.decorators.validates\_schema-fn-callable-...-any-or-none-none-pass\_many-bool-false-pass\_o). По умолчанию ошибки проверки на уровне схемы будут храниться в ключе **\_schema** словаря ошибок.

```python
from marshmallow import Schema, fields, validates_schema, ValidationError

class NumberSchema(Schema):
    field_a = fields.Integer()
    field_b = fields.Integer()

    @validates_schema
    def validate_numbers(self, data, **kwargs):
        if data["field_b"] >= data["field_a"]:
            raise ValidationError("field_a must be greater than field_b")

schema = NumberSchema()
try:
    schema.load({"field_a": 1, "field_b": 2})
except ValidationError as err:
    err.messages["_schema"]
# => ["field_a must be greater than field_b"]
```

### Хранение ошибок в конкретных полях

Можно сообщать об ошибках в полях и подполях с помощью словаря [dict](https://python.readthedocs.io/en/latest/library/stdtypes.html#dict).

Когда несколько валидаторов на уровне схемы возвращают ошибки, структуры ошибок объединяются в [ValidationError](../api-marshmallow/isklyucheniya.md#exception-marshmallow.exceptions.validationerror-message-str-or-list-or-dict-field\_name-str-\_schema), возникающем в конце проверки.

```python
from marshmallow import Schema, fields, validates_schema, ValidationError

class NumberSchema(Schema):
    field_a = fields.Integer()
    field_b = fields.Integer()
    field_c = fields.Integer()
    field_d = fields.Integer()

    @validates_schema
    def validate_lower_bound(self, data, **kwargs):
        errors = {}
        if data["field_b"] <= data["field_a"]:
            errors["field_b"] = ["field_b must be greater than field_a"]
        if data["field_c"] <= data["field_a"]:
            errors["field_c"] = ["field_c must be greater than field_a"]
        if errors:
            raise ValidationError(errors)

    @validates_schema
    def validate_upper_bound(self, data, **kwargs):
        errors = {}
        if data["field_b"] >= data["field_d"]:
            errors["field_b"] = ["field_b must be lower than field_d"]
        if data["field_c"] >= data["field_d"]:
            errors["field_c"] = ["field_c must be lower than field_d"]
        if errors:
            raise ValidationError(errors)

schema = NumberSchema()
try:
    schema.load({"field_a": 3, "field_b": 2, "field_c": 1, "field_d": 0})
except ValidationError as err:
    err.messages
# => {
#     'field_b': [
#         'field_b must be greater than field_a',
#         'field_b must be lower than field_d'
#     ],
#     'field_c': [
#         'field_c must be greater than field_a',
#         'field_c must be lower than field_d'
#     ]
#    }
```

## Использование оригинальных входных данных
