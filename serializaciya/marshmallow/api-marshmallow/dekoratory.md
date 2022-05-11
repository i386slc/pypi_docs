# Декораторы

Декораторы для регистрации методов предварительной и последующей обработки схемы. Они должны быть импортированы из модуля **marshmallow** верхнего уровня.

Методы, декорированные **pre\_load**, **post\_load**, **pre\_dump**, **post\_dump** и **validates\_schema**, получают **many** в качестве аргумента ключевого слова. Кроме того, **pre\_load**, **post\_load** и **validates\_schema** получают **partial**. Если вам не нужны эти аргументы, добавьте `**kwargs` в сигнатуру метода.

Пример:

```python
from marshmallow import (
    Schema, pre_load, pre_dump, post_load, validates_schema,
    validates, fields, ValidationError
)

class UserSchema(Schema):

    email = fields.Str(required=True)
    age = fields.Integer(required=True)

    @post_load
    def lowerstrip_email(self, item, many, **kwargs):
        item['email'] = item['email'].lower().strip()
        return item

    @pre_load(pass_many=True)
    def remove_envelope(self, data, many, **kwargs):
        namespace = 'results' if many else 'result'
        return data[namespace]

    @post_dump(pass_many=True)
    def add_envelope(self, data, many, **kwargs):
        namespace = 'results' if many else 'result'
        return {namespace: data}

    @validates_schema
    def validate_email(self, data, **kwargs):
        if len(data['email']) < 3:
            raise ValidationError('Email must be more than 3 characters', 'email')

    @validates('age')
    def validate_age(self, data, **kwargs):
        if data < 14:
            raise ValidationError('Too young!')
```

{% hint style="info" %}
Эти декораторы работают только с методами экземпляра. Класс и статические методы не поддерживаются.
{% endhint %}

{% hint style="warning" %}
Порядок вызова оформленных методов одного типа не гарантируется. Если вам нужно гарантировать порядок различных шагов обработки, вы должны поместить их в один и тот же метод обработки.
{% endhint %}

| Функции                                       | Описание                                                                 |
| --------------------------------------------- | ------------------------------------------------------------------------ |
| post\_dump(\[fn, pass\_many, pass\_original]) | Зарегистрируйте метод для вызова после сериализации объекта.             |
| post\_load(\[fn, pass\_many, pass\_original]) | Зарегистрируйте метод для вызова после десериализации объекта.           |
| pre\_dump(\[fn, pass\_many])                  | Зарегистрируйте метод для вызова перед сериализацией объекта.            |
| pre\_load(\[fn, pass\_many])                  | Зарегистрируйте метод для вызова перед десериализацией объекта.          |
| set\_hook(fn, key, \*\*kwargs)                | Отметьте декорированную функцию как крючок, который нужно поднять позже. |
| validates(field\_name)                        | Зарегистрируйте валидатор полей.                                         |
| validates\_schema(\[fn, pass\_many, ...])     | Зарегистрируйте средство проверки на уровне схемы.                       |

## post\_dump

#### marshmallow.decorators.post\_dump(_fn: Callable\[..., Any] | None = None_, _pass\_many: bool = False_, _pass\_original: bool = False_) → Callable\[..., Any]\[source]

Зарегистрируйте метод для вызова после сериализации объекта. Метод получает сериализованный объект и возвращает обработанный объект.

По умолчанию он получает один объект за раз, прозрачно обрабатывая аргумент **many**, переданный вызову схемы [dump()](skhema-schema.md#dump). Если `pass_many=True`, передаются необработанные данные (которые могут быть коллекцией).

Если `pass_original=True`, исходные данные (до сериализации) будут переданы в метод в качестве дополнительного аргумента.

_Изменено в версии 3.0.0_: **many** всегда передается в качестве аргумента ключевого слова декорированному методу.

## post\_load

#### marshmallow.decorators.post\_load(_fn: Callable\[..., Any] | None = None_, _pass\_many: bool = False_, _pass\_original: bool = False_) → Callable\[..., Any]

Зарегистрируйте метод для вызова после десериализации объекта. Метод получает десериализованные данные и возвращает обработанные данные.

По умолчанию он получает один объект за раз, прозрачно обрабатывая аргумент **many**, переданный в вызов [load()](skhema-schema.md#load) схемы. Если `pass_many=True`, передаются необработанные данные (которые могут быть коллекцией).

Если `pass_original=True`, исходные данные (до десериализации) будут переданы в метод в качестве дополнительного аргумента.

_Изменено в версии 3.0.0_: **partial** и **many** всегда передаются в качестве аргументов ключевого слова в декорированный метод.

## pre\_dump

#### marshmallow.decorators.pre\_dump(_fn: Callable\[..., Any] | None = None_, _pass\_many: bool = False_) → Callable\[..., Any]

Зарегистрируйте метод для вызова перед сериализацией объекта. Метод получает объект для сериализации и возвращает обработанный объект.

По умолчанию он получает один объект за раз, прозрачно обрабатывая аргумент many, переданный вызову схемы [dump()](skhema-schema.md#dump). Если `pass_many=True`, передаются необработанные данные (которые могут быть коллекцией).

_Изменено в версии 3.0.0_: **many** всегда передаются в качестве аргументов ключевого слова декорированному методу.

## pre\_load

#### marshmallow.decorators.pre\_load(_fn: Callable\[..., Any] | None = None_, _pass\_many: bool = False_) → Callable\[..., Any]

Зарегистрируйте метод для вызова перед десериализацией объекта. Метод получает данные для десериализации и возвращает обработанные данные.

По умолчанию он получает один объект за раз, прозрачно обрабатывая аргумент many, переданный в вызов [load()](skhema-schema.md#load) схемы. Если `pass_many=True`, передаются необработанные данные (которые могут быть коллекцией).

_Изменено в версии 3.0.0_: **partial** и **many** всегда передаются в качестве аргументов ключевого слова в декорированный метод.

## set\_hook

#### marshmallow.decorators.set\_hook(_fn: Callable\[..., Any] | None_, _key: tuple\[str, bool] | str_, _\*\*kwargs: Any_) → Callable\[..., Any]

Отмечает декорированную функцию как крючок, который нужно поднять позже. Вам не нужно использовать этот метод напрямую.

{% hint style="info" %}
В настоящее время работает только с функциями и методами экземпляра. Класс и статические методы не поддерживаются.
{% endhint %}

**Возвращает**: Декорированную функцию, если она предоставлена, иначе этот декоратор со связанными аргументами.

## validates

#### marshmallow.decorators.validates(_field\_name: str_) → Callable\[\[...], Any]

Зарегистрируйте валидатор полей.

#### Параметры:

* **field\_name** (str) - Имя поля, которое проверяет метод.

## validates\_schema

#### marshmallow.decorators.validates\_schema(_fn: Callable\[..., Any] | None = None_, _pass\_many: bool = False_, _pass\_original: bool = False_, _skip\_on\_field\_errors: bool = True_) → Callable\[..., Any]

Зарегистрируйте средство проверки на уровне схемы.

По умолчанию он получает один объект за раз, прозрачно обрабатывая аргумент **many**, переданный в вызов Schema [validate()](skhema-schema.md#validate). Если `pass_many=True`, передаются необработанные данные (которые могут быть коллекцией).

Если `pass_original=True`, исходные данные (до демаршалинга) будут переданы в метод в качестве дополнительного аргумента.

Если `skip_on_field_errors=True`, этот метод проверки будет пропущен всякий раз, когда при проверке полей будут обнаружены ошибки проверки.

_Изменено в версии 3.0.0b1_: **skip\_on\_field\_errors** по умолчанию имеет значение `True`.

_Изменено в версии 3.0.0_: **partial** и **many** всегда передаются в качестве аргументов ключевого слова в декорированный метод.
