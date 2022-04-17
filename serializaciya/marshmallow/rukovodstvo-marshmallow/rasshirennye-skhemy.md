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

Если вы хотите использовать исходный необработанный ввод, вы можете добавить **pass\_original=True** в <mark style="color:red;">post\_load</mark> или <mark style="color:red;">validates\_schema</mark>.

```python
from marshmallow import Schema, fields, post_load, ValidationError

class MySchema(Schema):
    foo = fields.Int()
    bar = fields.Int()

    @post_load(pass_original=True)
    def add_baz_to_bar(self, data, original_data, **kwargs):
        baz = original_data.get("baz")
        if baz:
            data["bar"] = data["bar"] + baz
        return data

schema = MySchema()
schema.load({"foo": 1, "bar": 2, "baz": 3})
# {'foo': 1, 'bar': 5}
```

{% hint style="info" %}
Поведением по умолчанию для неуказанных полей можно управлять с помощью параметра **unknown**, см. раздел «[Обработка неизвестных полей](bystryi-start.md#obrabotka-neizvestnykh-polei)» для получения дополнительной информации.
{% endhint %}

## Переопределение способа доступа к атрибутам

По умолчанию **marshmallow** использует [utils.get\_value](../api-marshmallow/vspomogatelnye-funkcii.md#marshmallow.utils.get\_value-obj-key-int-or-str-default-less-than-marshmallow.missing-greater-than) для извлечения атрибутов из различных типов объектов для сериализации. Это будет работать для _**большинства**_ случаев использования.

Однако, если вы хотите указать способ доступа к значениям из объекта, вы можете переопределить метод <mark style="color:red;">get\_attribute</mark>.

```python
class UserDictSchema(Schema):
    name = fields.Str()
    email = fields.Email()

    # Если мы знаем, что сериализуем только словари,
    # мы можем использовать dict.get для всех входных объектов.
    def get_attribute(self, obj, key, default):
        return obj.get(key, default)
```

## Пользовательская обработка ошибок

По умолчанию <mark style="color:red;">Schema.load()</mark> вызовет [ValidationError](../api-marshmallow/isklyucheniya.md#exception-marshmallow.exceptions.validationerror-message-str-or-list-or-dict-field\_name-str-\_schema), если будут переданы недопустимые данные.

Вы можете указать пользовательскую функцию обработки ошибок для схемы, переопределив метод <mark style="color:red;">handle\_error</mark>. Метод получает [ValidationError](../api-marshmallow/isklyucheniya.md#exception-marshmallow.exceptions.validationerror-message-str-or-list-or-dict-field\_name-str-\_schema) и исходные входные данные для десериализации.

```python
import logging
from marshmallow import Schema, fields

class AppError(Exception):
    pass

class UserSchema(Schema):
    email = fields.Email()

    def handle_error(self, exc, data, **kwargs):
        """Log and raise our custom exception when (de)serialization fails."""
        logging.error(exc.messages)
        raise AppError("An error occurred with input: {0}".format(data))

schema = UserSchema()
schema.load({"email": "invalid-email"})  # raises AppError
```

## Пользовательские параметры класса Meta

Параметры класса **Meta** — это способ настроить и изменить поведение схемы <mark style="color:red;">Schema</mark>. Список доступных опций см. в <mark style="color:red;">документации по API</mark>.

Вы можете добавить пользовательские параметры класса **Meta**, создав подкласс <mark style="color:red;">SchemaOpts</mark>.

### Пример: Конвертирование, Пересмотр

Давайте воспользуемся приведенным выше примером для добавления конвертации в сериализованный вывод. На этот раз мы позволим настраивать ключ конвертации с помощью параметров класса **Meta**.

```python
# Пример выходных данных
{
    'user': {
        'name': 'Keith',
        'email': 'keith@stones.com'
    }
}
# Вывод списка
{
    'users': [{'name': 'Keith'}, {'name': 'Mick'}]
}
```

Во-первых, мы добавим нашу конфигурацию пространства имен в класс настраиваемых параметров.

```python
from marshmallow import Schema, SchemaOpts

class NamespaceOpts(SchemaOpts):
    """То же, что и параметры класса Meta по умолчанию,
    но добавлены параметры «name» и «plural_name» для оболочки.
    """

    def __init__(self, meta, **kwargs):
        SchemaOpts.__init__(self, meta, **kwargs)
        self.name = getattr(meta, "name", None)
        self.plural_name = getattr(meta, "plural_name", self.name)
```

Затем мы создаем пользовательскую схему <mark style="color:red;">Schema</mark>, которая использует наш класс параметров.

```python
class NamespacedSchema(Schema):
    OPTIONS_CLASS = NamespaceOpts

    @pre_load(pass_many=True)
    def unwrap_envelope(self, data, many, **kwargs):
        key = self.opts.plural_name if many else self.opts.name
        return data[key]

    @post_dump(pass_many=True)
    def wrap_with_envelope(self, data, many, **kwargs):
        key = self.opts.plural_name if many else self.opts.name
        return {key: data}
```

Схемы наших приложений теперь могут наследоваться от нашего пользовательского класса схемы.

```python
class UserSchema(NamespacedSchema):
    name = fields.String()
    email = fields.Email()

    class Meta:
        name = "user"
        plural_name = "users"

ser = UserSchema()
user = User("Keith", email="keith@stones.com")
result = ser.dump(user)
result  # {"user": {"name": "Keith", "email": "keith@stones.com"}}
```

## Использование контекста

Атрибут **context** схемы <mark style="color:red;">Schema</mark> — это хранилище общего назначения для дополнительной информации, которая может понадобиться для (де)сериализации. Его можно использовать как в методах **Schema**, так и в методах **Field**.

```python
schema = UserSchema()
# Сделать текущий HTTP-запрос доступным для настраиваемых полей,
# методов схемы, средств проверки схемы и т. д.
schema.context["request"] = request
schema.dump(user)
```

## Пользовательские сообщения об ошибках

Чтобы настроить сообщения об ошибках на уровне схемы, которые используют <mark style="color:red;">load</mark> и <mark style="color:red;">loads</mark> при вызове [ValidationError](../api-marshmallow/isklyucheniya.md#exception-marshmallow.exceptions.validationerror-message-str-or-list-or-dict-field\_name-str-\_schema), переопределите переменную класса <mark style="color:red;">error\_messages</mark>:

```python
class MySchema(Schema):
    error_messages = {
        "unknown": "Custom unknown field error message.",
        "type": "Custom invalid type error message.",
    }
```

Значения по умолчанию для сообщений об ошибках на уровне поля можно установить в Field.default\_error\_messages.
