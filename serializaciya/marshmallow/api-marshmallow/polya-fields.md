# Поля Fields

Классы полей для различных типов данных.

## Классы:

| Класс                                                | Описание                                                                                                                                                              |
| ---------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| AwareDateTime(\[format, default\_timezone])          | Отформатированная осведомленная строка даты и времени.                                                                                                                |
| Bool                                                 | псевдоним <mark style="color:red;">marshmallow.fields.Boolean</mark>                                                                                                  |
| Boolean(\*\[, truthy, falsy])                        | Логическое поле                                                                                                                                                       |
| Constant(constant, \*\*kwargs)                       | Поле, которое (де)сериализуется в предустановленную константу.                                                                                                        |
| Date(\[format])                                      | Строка даты в формате ISO8601.                                                                                                                                        |
| DateTime(\[format])                                  | Отформатированная строка даты и времени.                                                                                                                              |
| Decimal(\[places, rounding, allow\_nan, as\_string]) | Поле, которое (де)сериализуется в тип Python **decimal.Decimal**.                                                                                                     |
| Dict(\[keys, values])                                | Поле словаря                                                                                                                                                          |
| Email(\*args, \*\*kwargs)                            | Поле электронной почты                                                                                                                                                |
| Field(\*, load\_default, missing, ...)               | Основное поле, из которого должны расширяться другие поля                                                                                                             |
| Float(\*\[, allow\_nan, as\_string])                 | **Double** как строка двойной точности IEEE-754                                                                                                                       |
| Function(\[serialize, deserialize])                  | Поле, принимающее значение, возвращенное функцией                                                                                                                     |
| IP(\*args\[, exploded])                              | Поле IP-адреса                                                                                                                                                        |
| IPInterface(\*args\[, exploded])                     | Поле IP-интерфейса                                                                                                                                                    |
| IPv4(\*args\[, exploded])                            | Поле адреса IPv4                                                                                                                                                      |
| IPv4Interface(\*args\[, exploded])                   | Поле сетевого интерфейса IPv4                                                                                                                                         |
| IPv6(\*args\[, exploded])                            | Поле адреса IPv6                                                                                                                                                      |
| IPv6Interface(\*args\[, exploded])                   | Поле сетевого интерфейса IPv6                                                                                                                                         |
| Int                                                  | псевдоним <mark style="color:red;">marshmallow.fields.Intege</mark>r                                                                                                  |
| Integer(\*\[, strict])                               | Целочисленное поле                                                                                                                                                    |
| List(cls\_or\_instance, \*\*kwargs)                  | Поле списка, составленное из другого класса или экземпляра <mark style="color:red;">Field</mark>                                                                      |
| Mapping(\[keys, values])                             | Абстрактный класс для объектов с парами ключ-значение                                                                                                                 |
| Method(\[serialize, deserialize])                    | Поле, которое принимает значение, возвращаемое методом схемы                                                                                                          |
| NaiveDateTime(\[format, timezone])                   | Отформатированная наивная строка даты и времени.                                                                                                                      |
| Nested(nested, ...)                                  | Позволяет вложить [Schema](skhema-schema.md#class-marshmallow.schema) внутрь поля.                                                                                    |
| Number(\*\[, as\_string])                            | Базовый класс для числовых полей.                                                                                                                                     |
| Pluck(nested, field\_name, \*\*kwargs)               | Позволяет заменить вложенные данные одним из полей данных.                                                                                                            |
| Raw(\*, load\_default, missing, dump\_default, ...)  | Поле, к которому не применяется форматирование.                                                                                                                       |
| Str                                                  | псевдоним <mark style="color:red;">marshmallow.fields.String</mark>                                                                                                   |
| String(\*, load\_default, missing, ...)              | Строковое поле.                                                                                                                                                       |
| Time(\[format])                                      | Отформатированная строка времени.                                                                                                                                     |
| TimeDelta(\[precision])                              | Поле, которое (де)сериализует объект [datetime.timedelta](https://python.readthedocs.io/en/latest/library/datetime.html#datetime.timedelta) в целое число и наоборот. |
| Tuple(tuple\_fields, \*args, \*\*kwargs)             | Поле кортежа, состоящее из фиксированного числа других классов или экземпляров Field                                                                                  |
| URL                                                  | псевдоним <mark style="color:red;">marshmallow.fields.Url</mark>                                                                                                      |
| UUID(\*, load\_default, missing, dump\_default, ...) | Поле UUID.                                                                                                                                                            |
| Url(\*\[, relative, schemes, require\_tld])          | Поле URL.                                                                                                                                                             |

## AwareDateTime

#### _class_ marshmallow.fields.AwareDateTime(_format: str | None = None_, _\*_, _default\_timezone: dt.tzinfo | None = None_, _\*\*kwargs_)

Отформатированная "осведомленная" строка даты и времени.

#### Параметры:

* **format** - смотри <mark style="color:red;">DateTime</mark>
* **default\_timezone** - Используется при десериализации. Если `None`, наивные даты и время отклоняются. Если не `None`, в этом часовом поясе устанавливаются наивные значения даты и времени.
* **kwargs** - Те же аргументы ключевого слова, которые получает <mark style="color:red;">Field</mark>.

_Новое в версии 3.0.0rc9_.

#### Методы:

| Метод                                        | Описание                  |
| -------------------------------------------- | ------------------------- |
| \_deserialize(value, attr, data, \*\*kwargs) | Десериализовать значение. |

### \_deserialize()

#### \_deserialize(_value_, _attr_, _data_, _\*\*kwargs_)

#### Параметры:

* **value** - Значение для десериализации
* **attr** - Атрибут/ключ в данных для десериализации
* **data** - Необработанные входные данные передаются в `Schema.load`.
* **kwargs** - Аргументы ключевого слова, специфичные для <mark style="color:red;">Field</mark>.

**Поднимает**: [ValidationError](isklyucheniya.md#exception-marshmallow.exceptions.validationerror-message-str-or-list-or-dict-field\_name-str-\_schema) - В случае сбоя форматирования или проверки.

**Возвращает**: Десериализованное значение.

_Изменено в версии 2.0.0_: Добавлены параметры **attr** и **data**.

_Изменено в версии 3.0.0_: Добавлены **\*\*kwargs** в сигнатуру.

## Bool

Псевдоним [marshmallow.fields.Boolean](polya-fields.md#undefined).

## Boolean

#### _class_ marshmallow.fields.Boolean(_\*_, _truthy: set | None = None_, _falsy: set | None = None_, _\*\*kwargs_)

Логическое поле.

#### Параметры:

* **truthy** - Значения, которые будут (де)сериализованы в **True**. Если набор пуст, любое неложное значение будет десериализовано до **True**. Если нет, будет использоваться <mark style="color:red;">marshmallow.fields.Boolean.truthy</mark>.
* **falsy** - Значения, которые будут (де)сериализованы в **False**. Если нет, будет использоваться <mark style="color:red;">marshmallow.fields.Boolean.falsy</mark>.
* **kwargs** - Те же аргументы ключевого слова, которые получает <mark style="color:red;">Field</mark>.

#### Методы:

| Название                                     | Описание                                           |
| -------------------------------------------- | -------------------------------------------------- |
| \_deserialize(value, attr, data, \*\*kwargs) | Десериализовать значение.                          |
| \_serialize(value, attr, obj, \*\*kwargs)    | Сериализует **value** в базовый тип данных Python. |

#### Атрибуты:

| Название                     | Описание                           |
| ---------------------------- | ---------------------------------- |
| **default\_error\_messages** | Сообщения об ошибках по умолчанию. |
| falsy                        | Ложные значения по умолчанию.      |
| truthy                       | Истинные значения по умолчанию.    |

### \_deserialize()

#### \_deserialize(_value_, _attr_, _data_, _\*\*kwargs_)

Десериализовать значение **value**. Конкретные классы <mark style="color:red;">Field</mark> должны реализовывать этот метод.

#### Параметры:

* **value** - Значение для десериализации
* **attr** - Атрибут/ключ в данных для десериализации
* **data** - Необработанные входные данные передаются в `Schema.load`.
* **kwargs** - Аргументы ключевого слова, специфичные для <mark style="color:red;">Field</mark>.

**Поднимает**: [ValidationError](isklyucheniya.md#exception-marshmallow.exceptions.validationerror-message-str-or-list-or-dict-field\_name-str-\_schema) - В случае сбоя форматирования или проверки.

**Возвращает**: Десериализованное значение.

_Изменено в версии 2.0.0_: Добавлены параметры **attr** и **data**.

_Изменено в версии 3.0.0_: Добавлены **\*\*kwargs** в сигнатуру.

### \_serialize()

#### \_serialize(_value_, _attr_, _obj_, _\*\*kwargs_)

Сериализует значение **value** в базовый тип данных Python. **Noop** по умолчанию. Конкретные классы <mark style="color:red;">Field</mark> должны реализовывать этот метод.

Пример:

```python
class TitleCase(Field):
    def _serialize(self, value, attr, obj, **kwargs):
        if not value:
            return ''
        return str(value).title()
```

#### Параметры:

* **value** - Значение, которое нужно сериализовать
* **attr** (_str_) - Атрибут или ключ объекта, который нужно сериализовать.
* **obj** (_object_) - Объект, из которого было извлечено значение.
* **kwargs** (_dict_) - Аргументы ключевого слова, специфичные для <mark style="color:red;">Field</mark>.

**Возвращает**: Сериализованное значение.

### default\_error\_messages

#### default\_error\_messages _= {'invalid': 'Not a valid boolean.'}_

Сообщения об ошибках по умолчанию.

### falsy

#### falsy _= {0, 'N', 'FALSE', 'OFF', 'n', 'Off', 'off', '0', 'No', 'false', 'no', 'F', 'NO', 'f', 'False'}_

Ложные значения по умолчанию.

### truthy

#### truthy _= {1, 't', 'true', 'On', 'ON', 'on', 'Y', 'TRUE', 'Yes', '1', 'T', 'True', 'yes', 'y', 'YES'}_

Истинные значения по умолчанию.

## Constant

#### _class_ marshmallow.fields.Constant(_constant: Any_, _\*\*kwargs_)

Поле, которое (де)сериализуется в предустановленную константу. Если вы хотите, чтобы константа добавлялась только для сериализации или десериализации, вы должны использовать `dump_only=True` или `load_only=True` соответственно.

#### Параметры:

* **constant** - Константа, возвращаемая для атрибута поля.

_Новое в версии 2.0.0_.

#### Методы:

| Название                                     | Описание                                           |
| -------------------------------------------- | -------------------------------------------------- |
| \_deserialize(value, attr, data, \*\*kwargs) | Десериализовать значение.                          |
| \_serialize(value, attr, obj, \*\*kwargs)    | Сериализует **value** в базовый тип данных Python. |

### \_deserialize()

#### \_deserialize(_value_, _attr_, _data_, _\*\*kwargs_)

Десериализовать значение **value**. Конкретные классы <mark style="color:red;">Field</mark> должны реализовывать этот метод.

#### Параметры:

* **value** - Значение для десериализации
* **attr** - Атрибут/ключ в данных для десериализации
* **data** - Необработанные входные данные передаются в `Schema.load`.
* **kwargs** - Аргументы ключевого слова, специфичные для <mark style="color:red;">Field</mark>.

**Поднимает**: [ValidationError](isklyucheniya.md#exception-marshmallow.exceptions.validationerror-message-str-or-list-or-dict-field\_name-str-\_schema) - В случае сбоя форматирования или проверки.

**Возвращает**: Десериализованное значение.

_Изменено в версии 2.0.0_: Добавлены параметры **attr** и **data**.

_Изменено в версии 3.0.0_: Добавлены **\*\*kwargs** в сигнатуру.

### \_serialize()

#### \_serialize(_value_, _attr_, _obj_, _\*\*kwargs_)

Сериализует значение **value** в базовый тип данных Python. **Noop** по умолчанию. Конкретные классы <mark style="color:red;">Field</mark> должны реализовывать этот метод.

Пример:

```python
class TitleCase(Field):
    def _serialize(self, value, attr, obj, **kwargs):
        if not value:
            return ''
        return str(value).title()
```

#### Параметры:

* **value** - Значение, которое нужно сериализовать
* **attr** (_str_) - Атрибут или ключ объекта, который нужно сериализовать.
* **obj** (_object_) - Объект, из которого было извлечено значение.
* **kwargs** (_dict_) - Аргументы ключевого слова, специфичные для <mark style="color:red;">Field</mark>.

**Возвращает**: Сериализованное значение.

## Date

#### _class_ marshmallow.fields.Date(_format: str | None = None_, _\*\*kwargs_)

Строка даты в формате ISO8601.

#### Параметры:

* **format** - Либо "iso" (для ISO8601), либо строка формата даты. Если `None`, по умолчанию используется «iso».
* **kwargs** - Те же аргументы ключевого слова, которые получает <mark style="color:red;">Field</mark>.

#### Атрибуты:

| Название                     | Описание                           |
| ---------------------------- | ---------------------------------- |
| **default\_error\_messages** | Сообщения об ошибках по умолчанию. |

### default\_error\_messages

#### default\_error\_messages _= {'format': '"{input}" cannot be formatted as a date.', 'invalid': 'Not a valid date.'}_

Сообщения об ошибках по умолчанию.

## DateTime

#### _class_ marshmallow.fields.DateTime(_format: str | None = None_, _\*\*kwargs_)

Отформатированная строка даты и времени.

Пример: `'2014-12-22T03:12:58.019077+00:00'`

#### Параметры:

* **format** - Либо "rfc" (для RFC822), "iso" (для ISO8601), либо строка формата даты. Если нет, по умолчанию используется «iso».
* **kwargs** - Те же аргументы ключевого слова, которые получает <mark style="color:red;">Field</mark>.

_Изменено в версии 3.0.0rc9_: не изменяет информацию о часовом поясе при (де)сериализации.

#### Методы:

| Название                                     | Описание                                                 |
| -------------------------------------------- | -------------------------------------------------------- |
| **\_bind\_to\_schema**(field\_name, schema)  | Поле обновления со значениями из его родительской схемы. |
| \_deserialize(value, attr, data, \*\*kwargs) | Десериализовать значение.                                |
| \_serialize(value, attr, obj, \*\*kwargs)    | Сериализует **value** в базовый тип данных Python.       |

#### Атрибуты:

| Название                     | Описание                           |
| ---------------------------- | ---------------------------------- |
| **default\_error\_messages** | Сообщения об ошибках по умолчанию. |

### \_bind\_to\_schema()

#### \_bind\_to\_schema(_field\_name_, _schema_)

Поле обновления со значениями из его родительской схемы. Вызывается `Schema._bind_field`.

#### Параметры:

* **field\_name** (str) - Имя поля задано в схеме.
* **schema** (Schema | Field) - Родительский объект.

### \_deserialize()

#### \_deserialize(_value_, _attr_, _data_, _\*\*kwargs_)

Десериализовать значение **value**. Конкретные классы <mark style="color:red;">Field</mark> должны реализовывать этот метод.

#### Параметры:

* **value** - Значение для десериализации
* **attr** - Атрибут/ключ в данных для десериализации
* **data** - Необработанные входные данные передаются в `Schema.load`.
* **kwargs** - Аргументы ключевого слова, специфичные для <mark style="color:red;">Field</mark>.

**Поднимает**: [ValidationError](isklyucheniya.md#exception-marshmallow.exceptions.validationerror-message-str-or-list-or-dict-field\_name-str-\_schema) - В случае сбоя форматирования или проверки.

**Возвращает**: Десериализованное значение.

_Изменено в версии 2.0.0_: Добавлены параметры **attr** и **data**.

_Изменено в версии 3.0.0_: Добавлены **\*\*kwargs** в сигнатуру.

### \_serialize()

#### \_serialize(_value_, _attr_, _obj_, _\*\*kwargs_)

Сериализует значение **value** в базовый тип данных Python. **Noop** по умолчанию. Конкретные классы <mark style="color:red;">Field</mark> должны реализовывать этот метод.

Пример:

```python
class TitleCase(Field):
    def _serialize(self, value, attr, obj, **kwargs):
        if not value:
            return ''
        return str(value).title()
```

#### Параметры:

* **value** - Значение, которое нужно сериализовать
* **attr** (_str_) - Атрибут или ключ объекта, который нужно сериализовать.
* **obj** (_object_) - Объект, из которого было извлечено значение.
* **kwargs** (_dict_) - Аргументы ключевого слова, специфичные для <mark style="color:red;">Field</mark>.

**Возвращает**: Сериализованное значение

### default\_error\_messages

#### default\_error\_messages _= {'format': '"{input}" cannot be formatted as a {obj\_type}.', 'invalid': 'Not a valid {obj\_type}.', 'invalid\_awareness': 'Not a valid {awareness} {obj\_type}.'}_

Сообщения об ошибках по умолчанию.

### Decimal

#### _class_ marshmallow.fields.Decimal(_places: int | None = None_, _rounding: str | None = None_, _\*_, _allow\_nan: bool = False_, _as\_string: bool = False_, _\*\*kwargs_)

Поле, которое (де)сериализуется в тип Python `decimal.Decimal`. Его безопасно использовать при работе с денежными значениями, процентами, отношениями или другими числами, где точность имеет решающее значение.

{% hint style="info" %}
Это поле сериализуется в объект [decimal.Decimal](https://python.readthedocs.io/en/latest/library/decimal.html#decimal.Decimal) по умолчанию. Если вам нужно отобразить ваши данные в формате **JSON**, имейте в виду, что модуль [json](https://python.readthedocs.io/en/latest/library/json.html#module-json) из стандартной библиотеки не кодирует [decimal.Decimal](https://python.readthedocs.io/en/latest/library/decimal.html#decimal.Decimal). Поэтому вы должны использовать библиотеку **JSON**, которая может обрабатывать десятичные числа, например **simplejson**, или сериализовать в строку, передав `as_string=True`.
{% endhint %}

{% hint style="info" %}
Если в это поле для десериализации передается значение [float](https://python.readthedocs.io/en/latest/library/functions.html#float) **JSON**, оно сначала будет приведено к соответствующему строковому значению [string](https://python.readthedocs.io/en/latest/library/string.html#module-string), а затем десериализовано в объект [decimal.Decimal](https://python.readthedocs.io/en/latest/library/decimal.html#decimal.Decimal). Реализация по умолчанию **\_\_str\_\_** встроенного типа с плавающей запятой [float](https://python.readthedocs.io/en/latest/library/functions.html#float) Python может применять деструктивное преобразование к своим входным данным, и поэтому на нее нельзя полагаться для сохранения точности. Чтобы избежать этого, вы можете вместо этого передать строку [string](https://python.readthedocs.io/en/latest/library/string.html#module-string) JSON для прямой десериализации.
{% endhint %}

#### Параметры:

* **places** - До скольких знаков после запятой квантовать значение. Если `None`, не квантует значение.
* **rounding** - Как округлить значение при квантовании, например [decimal.ROUND\_UP](https://python.readthedocs.io/en/latest/library/decimal.html#decimal.ROUND\_UP). Если `None`, использует значение округления из контекста текущего потока.
* **allow\_nan** - Если `True`, разрешены **NaN**, **Infinity** и **-Infinity**, даже если они недопустимы в соответствии со спецификацией **JSON**.
* **as\_string** - Если `True`, сериализовать в строку вместо типа Python [decimal.Decimal](https://python.readthedocs.io/en/latest/library/decimal.html#decimal.Decimal).
* **kwargs** - Те же аргументы ключевого слова, которые получает <mark style="color:red;">Number</mark>.

_Новое в версии 1.2.0_.

#### Методы:

| Название             | Описание                                                                                                                                                                                            |
| -------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| \_format\_num(value) | Возвращает числовое значение для значения, учитывая <mark style="color:red;">num\_type</mark> этого поля.                                                                                           |
| \_validated(value)   | Форматирует значение или вызывает [ValidationError](isklyucheniya.md#exception-marshmallow.exceptions.validationerror-message-str-or-list-or-dict-field\_name-str-\_schema), если возникнет ошибка. |

#### Атрибуты:

| Название                     | Описание                           |
| ---------------------------- | ---------------------------------- |
| **default\_error\_messages** | Сообщения об ошибках по умолчанию. |

#### Классы:

| Название  | Описание                                                                                                  |
| --------- | --------------------------------------------------------------------------------------------------------- |
| num\_type | псевдоним [decimal.Decimal](https://python.readthedocs.io/en/latest/library/decimal.html#decimal.Decimal) |

### \_format\_num()

#### \_format\_num(_value_)

Возвращает числовое значение для значения, учитывая <mark style="color:red;">num\_type</mark> этого поля.

### \_validated()

#### \_validated(_value_)

Форматирует значение или вызывает [ValidationError](isklyucheniya.md#exception-marshmallow.exceptions.validationerror-message-str-or-list-or-dict-field\_name-str-\_schema), если возникнет ошибка.

### default\_error\_messages

#### default\_error\_messages _= {'special': 'Special numeric values (nan or infinity) are not permitted.'}_

Сообщения об ошибках по умолчанию.

### num\_type

Псевдоним [decimal.Decimal](https://python.readthedocs.io/en/latest/library/decimal.html#decimal.Decimal)

#### Методы num\_type:

### _class_ marshmallow.fields.Email(_\*args_, _\*\*kwargs_)

### _class_ marshmallow.fields.Field(_\*, load\_default: typing.Any = \<marshmallow.missing>, missing: typing.Any = \<marshmallow.missing>, dump\_default: typing.Any = \<marshmallow.missing>, default: typing.Any = \<marshmallow.missing>, data\_key: str | None = None, attribute: str | None = None, validate: None | (typing.Callable\[\[typing.Any], typing.Any] | typing.Iterable\[typing.Callable\[\[typing.Any], typing.Any]]) = None, required: bool = False, allow\_none: bool | None = None, load\_only: bool = False, dump\_only: bool = False, error\_messages: dict\[str, str] | None = None, metadata: typing.Mapping\[str, typing.Any] | None = None, \*\*additional\_metadata_)

#### default\_error\_messages _= {'null': 'Field may not be null.', 'required': 'Missing data for required field.', 'validator\_failed': 'Invalid value.'}_

#### \_deserialize(_value: Any_, _attr: str | None_, _data: Mapping\[str, Any] | None_, _\*\*kwargs_)

#### \_serialize(_value: Any_, _attr: str_, _obj: Any_, _\*\*kwargs_)

### _class_ marshmallow.fields.Function(_serialize: None | Callable\[\[Any], Any] | Callable\[\[Any, dict], Any] = None_, _deserialize: None | Callable\[\[Any], Any] | Callable\[\[Any, dict], Any] = None_, _\*\*kwargs_)

### _class_ marshmallow.fields.List(_cls\_or\_instance: Field | type_, _\*\*kwargs_)

### _class_ marshmallow.fields.Method(_serialize: str | None = None_, _deserialize: str | None = None_, _\*\*kwargs_)

### _class_ marshmallow.fields.Nested(_nested: SchemaABC | type | str | dict\[str, Field | type] | typing.Callable\[\[], SchemaABC | dict\[str, Field | type]], \*, dump\_default: typing.Any = \<marshmallow.missing>, default: typing.Any = \<marshmallow.missing>, only: types.StrSequenceOrSet | None = None, exclude: types.StrSequenceOrSet = (), many: bool = False, unknown: str | None = None, \*\*kwargs_)

### _class_ marshmallow.fields.Pluck(_nested: SchemaABC | type | str | Callable\[\[], SchemaABC]_, _field\_name: str_, _\*\*kwargs_)

### _class_ marshmallow.fields.String(_\*, load\_default: typing.Any = \<marshmallow.missing>, missing: typing.Any = \<marshmallow.missing>, dump\_default: typing.Any = \<marshmallow.missing>, default: typing.Any = \<marshmallow.missing>, data\_key: str | None = None, attribute: str | None = None, validate: None | (typing.Callable\[\[typing.Any], typing.Any] | typing.Iterable\[typing.Callable\[\[typing.Any], typing.Any]]) = None, required: bool = False, allow\_none: bool | None = None, load\_only: bool = False, dump\_only: bool = False, error\_messages: dict\[str, str] | None = None, metadata: typing.Mapping\[str, typing.Any] | None = None, \*\*additional\_metadata_)

### marshmallow.fields.URL
