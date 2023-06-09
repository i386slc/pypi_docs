# Поля Fields (A-F)

Классы полей для различных типов данных.

## Классы:

<table><thead><tr><th width="326">Класс</th><th>Описание</th></tr></thead><tbody><tr><td><a href="polya-fields-a-f.md#awaredatetime">AwareDateTime</a>([format, default_timezone])</td><td>Отформатированная осведомленная строка даты и времени.</td></tr><tr><td><a href="polya-fields-a-f.md#bool">Bool</a></td><td>псевдоним <a href="polya-fields-a-f.md#boolean">marshmallow.fields.Boolean</a></td></tr><tr><td><a href="polya-fields-a-f.md#boolean">Boolean</a>(*[, truthy, falsy])</td><td>Логическое поле</td></tr><tr><td><a href="polya-fields-a-f.md#constant">Constant</a>(constant, **kwargs)</td><td>Поле, которое (де)сериализуется в предустановленную константу.</td></tr><tr><td><a href="polya-fields-a-f.md#date">Date</a>([format])</td><td>Строка даты в формате ISO8601.</td></tr><tr><td><a href="polya-fields-a-f.md#datetime">DateTime</a>([format])</td><td>Отформатированная строка даты и времени.</td></tr><tr><td><a href="polya-fields-a-f.md#decimal">Decimal</a>([places, rounding, allow_nan, as_string])</td><td>Поле, которое (де)сериализуется в тип Python <strong>decimal.Decimal</strong>.</td></tr><tr><td><a href="polya-fields-a-f.md#dict">Dict</a>([keys, values])</td><td>Поле словаря</td></tr><tr><td><a href="polya-fields-a-f.md#email">Email</a>(*args, **kwargs)</td><td>Поле электронной почты</td></tr><tr><td><a href="polya-fields-a-f.md#field">Field</a>(*, load_default, missing, ...)</td><td>Основное поле, из которого должны расширяться другие поля</td></tr><tr><td><a href="polya-fields-a-f.md#float">Float</a>(*[, allow_nan, as_string])</td><td><strong>Double</strong> как строка двойной точности IEEE-754</td></tr><tr><td><a href="polya-fields-a-f.md#function">Function</a>([serialize, deserialize])</td><td>Поле, принимающее значение, возвращенное функцией</td></tr><tr><td>IP(*args[, exploded])</td><td>Поле IP-адреса</td></tr><tr><td>IPInterface(*args[, exploded])</td><td>Поле IP-интерфейса</td></tr><tr><td>IPv4(*args[, exploded])</td><td>Поле адреса IPv4</td></tr><tr><td>IPv4Interface(*args[, exploded])</td><td>Поле сетевого интерфейса IPv4</td></tr><tr><td>IPv6(*args[, exploded])</td><td>Поле адреса IPv6</td></tr><tr><td>IPv6Interface(*args[, exploded])</td><td>Поле сетевого интерфейса IPv6</td></tr><tr><td>Int</td><td>псевдоним <mark style="color:red;">marshmallow.fields.Intege</mark>r</td></tr><tr><td>Integer(*[, strict])</td><td>Целочисленное поле</td></tr><tr><td>List(cls_or_instance, **kwargs)</td><td>Поле списка, составленное из другого класса или экземпляра <mark style="color:red;">Field</mark></td></tr><tr><td>Mapping([keys, values])</td><td>Абстрактный класс для объектов с парами ключ-значение</td></tr><tr><td>Method([serialize, deserialize])</td><td>Поле, которое принимает значение, возвращаемое методом схемы</td></tr><tr><td>NaiveDateTime([format, timezone])</td><td>Отформатированная наивная строка даты и времени.</td></tr><tr><td>Nested(nested, ...)</td><td>Позволяет вложить <a href="skhema-schema.md#class-marshmallow.schema">Schema</a> внутрь поля.</td></tr><tr><td>Number(*[, as_string])</td><td>Базовый класс для числовых полей.</td></tr><tr><td>Pluck(nested, field_name, **kwargs)</td><td>Позволяет заменить вложенные данные одним из полей данных.</td></tr><tr><td>Raw(*, load_default, missing, dump_default, ...)</td><td>Поле, к которому не применяется форматирование.</td></tr><tr><td>Str</td><td>псевдоним <mark style="color:red;">marshmallow.fields.String</mark></td></tr><tr><td>String(*, load_default, missing, ...)</td><td>Строковое поле.</td></tr><tr><td>Time([format])</td><td>Отформатированная строка времени.</td></tr><tr><td>TimeDelta([precision])</td><td>Поле, которое (де)сериализует объект <a href="https://python.readthedocs.io/en/latest/library/datetime.html#datetime.timedelta">datetime.timedelta</a> в целое число и наоборот.</td></tr><tr><td>Tuple(tuple_fields, *args, **kwargs)</td><td>Поле кортежа, состоящее из фиксированного числа других классов или экземпляров Field</td></tr><tr><td>URL</td><td>псевдоним <mark style="color:red;">marshmallow.fields.Url</mark></td></tr><tr><td>UUID(*, load_default, missing, dump_default, ...)</td><td>Поле UUID.</td></tr><tr><td>Url(*[, relative, schemes, require_tld])</td><td>Поле URL.</td></tr></tbody></table>

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

Псевдоним [marshmallow.fields.Boolean](polya-fields-a-f.md#undefined).

## Boolean

#### _class_ marshmallow.fields.Boolean(_\*_, _truthy: set | None = None_, _falsy: set | None = None_, _\*\*kwargs_)

Логическое поле.

#### Параметры:

* **truthy** - Значения, которые будут (де)сериализованы в **True**. Если набор пуст, любое неложное значение будет десериализовано до **True**. Если нет, будет использоваться <mark style="color:red;">marshmallow.fields.Boolean.truthy</mark>.
* **falsy** - Значения, которые будут (де)сериализованы в **False**. Если нет, будет использоваться <mark style="color:red;">marshmallow.fields.Boolean.falsy</mark>.
* **kwargs** - Те же аргументы ключевого слова, которые получает <mark style="color:red;">Field</mark>.

#### Методы:

<table><thead><tr><th width="221.27047694782067">Название</th><th>Описание</th></tr></thead><tbody><tr><td>_deserialize(value, attr, data, **kwargs)</td><td>Десериализовать значение.</td></tr><tr><td>_serialize(value, attr, obj, **kwargs)</td><td>Сериализует <strong>value</strong> в базовый тип данных Python.</td></tr></tbody></table>

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

<table><thead><tr><th width="287.13053003256334">Название</th><th>Описание</th></tr></thead><tbody><tr><td>_deserialize(value, attr, data, **kwargs)</td><td>Десериализовать значение.</td></tr><tr><td>_serialize(value, attr, obj, **kwargs)</td><td>Сериализует <strong>value</strong> в базовый тип данных Python.</td></tr></tbody></table>

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

<table><thead><tr><th width="254.84231192288433">Название</th><th>Описание</th></tr></thead><tbody><tr><td><strong>_bind_to_schema</strong>(field_name, schema)</td><td>Поле обновления со значениями из его родительской схемы.</td></tr><tr><td>_deserialize(value, attr, data, **kwargs)</td><td>Десериализовать значение.</td></tr><tr><td>_serialize(value, attr, obj, **kwargs)</td><td>Сериализует <strong>value</strong> в базовый тип данных Python.</td></tr></tbody></table>

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

| Название                                  | Описание                                                                                                                                                            |
| ----------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| adjusted()                                | Возвращает скорректированный показатель степени числа.                                                                                                              |
| as\_integer\_ratio()                      | Возвращает пару целых чисел, отношение которых точно равно исходному Decimal и с положительным знаменателем.                                                        |
| as\_tuple()                               | Возвращает кортежное представление числа.                                                                                                                           |
| canonical()                               | Возвращает каноническую кодировку аргумента.                                                                                                                        |
| compare(other\[, context])                | Сравнивает себя self с другим числом other.                                                                                                                         |
| compare\_signal(other\[, context])        | Идентичен для сравнения, за исключением того, что все сигналы NaN.                                                                                                  |
| compare\_total(other\[, context])         | Сравнивает два операнда, используя их абстрактное представление, а не числовое значение.                                                                            |
| compare\_total\_mag(other\[, context])    | Сравнивает два операнда, используя их абстрактное представление, а не их значение, как в **compare\_total()**, но игнорируя знак каждого операнда.                  |
| conjugate()                               | Возвращает себя self.                                                                                                                                               |
| copy\_abs()                               | Возвращает абсолютное значение аргумента.                                                                                                                           |
| copy\_negate()                            | Возвращает отрицание аргумента.                                                                                                                                     |
| copy\_sign(other\[, context])             | Возвращает копию первого операнда со знаком, совпадающим со знаком второго операнда.                                                                                |
| exp(\[context])                           | Возвращает значение (натуральной) экспоненциальной функции e\*\*x для заданного числа.                                                                              |
| fma(other, third\[, context])             | Слитное умножение-сложение.                                                                                                                                         |
| from\_float()                             | Метод класса, который точно преобразует число с плавающей запятой в десятичное число.                                                                               |
| is\_canonical()                           | Возвращает True, если аргумент является каноническим, и False в противном случае.                                                                                   |
| is\_finite()                              | Возвращает True, если аргумент является конечным числом, и False, если аргумент бесконечен или NaN.                                                                 |
| is\_infinite()                            | Возвращает True, если аргумент представляет собой положительную или отрицательную бесконечность, и False в противном случае.                                        |
| is\_nan()                                 | Возвращает True, если аргумент является (тихим или сигнальным) NaN, и False в противном случае.                                                                     |
| is\_normal(\[context])                    | Возвращает True, если аргумент является нормальным конечным числом, отличным от нуля, с скорректированным показателем степени, большим или равным Emin.             |
| is\_qnan()                                | Возвращает True, если аргументом является тихий NaN, и False в противном случае.                                                                                    |
| is\_signed()                              | Возвращает True, если аргумент имеет отрицательный знак, и False в противном случае.                                                                                |
| is\_snan()                                | Возвращает True, если аргумент является сигнальным NaN, и False в противном случае.                                                                                 |
| is\_subnormal(\[context])                 | Возвращает True, если аргумент субнормальный, и False в противном случае.                                                                                           |
| is\_zero()                                | Возвращает True, если аргумент является (положительным или отрицательным) нулем, и False в противном случае.                                                        |
| ln(\[context])                            | Возвращает натуральный (по основанию e) логарифм операнда.                                                                                                          |
| log10(\[context])                         | Возвращает десятичный логарифм операнда.                                                                                                                            |
| logb(\[context])                          | Для ненулевого числа возвращает скорректированный показатель степени операнда в виде экземпляра Decimal.                                                            |
| logical\_and(other\[, context])           | Возвращает цифровое «и» двух (логических) операндов.                                                                                                                |
| logical\_invert(\[context])               | Возвращает поразрядную инверсию (логического) операнда.                                                                                                             |
| logical\_or(other\[, context])            | Возвращает цифровое «или» двух (логических) операндов.                                                                                                              |
| logical\_xor(other\[, context])           | Возвращает поразрядное «исключающее или» двух (логических) операндов.                                                                                               |
| max(other\[, context])                    | Максимум себя self и другого числа other.                                                                                                                           |
| max\_mag(other\[, context])               | Аналогичен методу max(), но сравнение выполняется с использованием абсолютных значений операндов.                                                                   |
| min(other\[, context])                    | Минимум себя self и другого числа other.                                                                                                                            |
| min\_mag(other\[, context])               | Аналогичен методу min(), но сравнение выполняется с использованием абсолютных значений операндов.                                                                   |
| next\_minus(\[context])                   | Возвращает наибольшее число, представленное в заданном контексте (или в текущем контексте по умолчанию, если контекст не задан), которое меньше заданного операнда. |
| next\_plus(\[context])                    | Возвращает наименьшее число, представленное в данном контексте (или в текущем контексте по умолчанию, если контекст не задан), которое больше заданного операнда.   |
| next\_toward(other\[, context])           | Если два операнда не равны, вернуть число, ближайшее к первому операнду в направлении второго операнда.                                                             |
| normalize(\[context])                     | Нормализует число, удалив крайние правые конечные нули и преобразовав любой результат, равный десятичному ('0'), в десятичный ('0e0').                              |
| number\_class(\[context])                 | Возвращает строку, описывающую класс операнда.                                                                                                                      |
| quantize(exp\[, rounding, context])       | Возвращает значение, равное первому операнду после округления и имеющее показатель степени второго операнда.                                                        |
| radix()                                   | Возвращает Decimal(10), основу (основание), в которой класс Decimal выполняет все свои арифметические действия.                                                     |
| remainder\_near(other\[, context])        | Возвращает остаток от деления себя self на другое число other.                                                                                                      |
| rotate(other\[, context])                 | Возвращает результат поворота цифр первого операнда на величину, указанную вторым операндом.                                                                        |
| same\_quantum(other\[, context])          | Проверяет, имеют ли self и other одинаковую степень или оба являются NaN.                                                                                           |
| scaleb(other\[, context])                 | Возвращает первый операнд с экспонентой, скорректированной вторым.                                                                                                  |
| shift(other\[, context])                  | Возвращает результат сдвига цифр первого операнда на величину, указанную вторым операндом.                                                                          |
| sqrt(\[context])                          | Возвращает квадратный корень аргумента с полной точностью.                                                                                                          |
| to\_eng\_string(\[context])               | Преобразование в строку инженерного типа.                                                                                                                           |
| to\_integral(\[rounding, context])        | Идентичен методу to\_integral\_value().                                                                                                                             |
| to\_integral\_exact(\[rounding, context]) | Округляет до ближайшего целого числа, сигнализируя Inexact или Rounded, если округление происходит.                                                                 |
| to\_integral\_value(\[rounding, context]) | Округляет до ближайшего целого числа, не сигнализируя о неточном или округленном значении.                                                                          |

## Dict

#### _class_ marshmallow.fields.Dict(_keys: Field | type | None = None_, _values: Field | type | None = None_, _\*\*kwargs_)

Поле словаря. Поддерживает словари и подобные им объекты. Расширяет отображение с помощью dict в качестве типа отображения.

Пример:

```python
numbers = fields.Dict(keys=fields.Str(), values=fields.Float())
```

#### Параметры:

* kwargs - Те же аргументы ключевого слова, которые получает <mark style="color:red;">Mapping</mark>.

_Новое в версии 2.1.0_.

| Классы        | Описание                                                                                 |
| ------------- | ---------------------------------------------------------------------------------------- |
| mapping\_type | псевдоним для [dict](https://python.readthedocs.io/en/latest/library/stdtypes.html#dict) |

### mapping\_type

Псевдоним для [dict](https://python.readthedocs.io/en/latest/library/stdtypes.html#dict)

| Методы mapping\_type        | Описание                                                                                                                                                                                                                  |
| --------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| clear()                     |                                                                                                                                                                                                                           |
| copy()                      |                                                                                                                                                                                                                           |
| fromkeys(\[value])          | Создает новый словарь с ключами из итерации и значениями, установленными в value.                                                                                                                                         |
| get(key\[, default])        | Возвращает значение для ключа, если ключ есть в словаре, иначе default.                                                                                                                                                   |
| items()                     |                                                                                                                                                                                                                           |
| keys()                      |                                                                                                                                                                                                                           |
| pop(k\[,d])                 | Если ключ не найден, возвращается d, если оно задано, в противном случае возникает KeyError                                                                                                                               |
| popitem()                   | Удалить и вернуть пару (ключ, значение) в виде 2-кортежа.                                                                                                                                                                 |
| setdefault(key\[, default]) | Вставляет ключ со значением по умолчанию, если ключ отсутствует в словаре.                                                                                                                                                |
| update(\[E, ]\*\*F)         | Если E присутствует и имеет метод .keys(), то делает: для k в E: D\[k] = E\[k] Если E присутствует и не имеет метода .keys(), то делает: для k, v в E: D\[k] = v В любом случае за этим следует: для k в F: D\[k] = F\[k] |
| values()                    |                                                                                                                                                                                                                           |

## Email

#### _class_ marshmallow.fields.Email(_\*args_, _\*\*kwargs_)

Поле электронной почты.

#### Параметры:

* **args** - Те же позиционные аргументы, которые получает <mark style="color:red;">String</mark>.
* **kwargs** - Те же ключевые аргументы, которые получает <mark style="color:red;">String</mark>.

| Атрибут                      | Описание                           |
| ---------------------------- | ---------------------------------- |
| **default\_error\_messages** | Сообщения об ошибках по умолчанию. |

### default\_error\_messages

#### default\_error\_messages _= {'invalid': 'Not a valid email address.'}_

Сообщения об ошибках по умолчанию.

## Field

#### _class_ marshmallow.fields.Field(_\*, load\_default: typing.Any = \<marshmallow.missing>, missing: typing.Any = \<marshmallow.missing>, dump\_default: typing.Any = \<marshmallow.missing>, default: typing.Any = \<marshmallow.missing>, data\_key: str | None = None, attribute: str | None = None, validate: None | (typing.Callable\[\[typing.Any], typing.Any] | typing.Iterable\[typing.Callable\[\[typing.Any], typing.Any]]) = None, required: bool = False, allow\_none: bool | None = None, load\_only: bool = False, dump\_only: bool = False, error\_messages: dict\[str, str] | None = None, metadata: typing.Mapping\[str, typing.Any] | None = None, \*\*additional\_metadata_)

#### default\_error\_messages _= {'null': 'Field may not be null.', 'required': 'Missing data for required field.', 'validator\_failed': 'Invalid value.'}_

Основное поле, из которого должны расширяться другие поля. По умолчанию форматирование не применяется, и его следует использовать только в тех случаях, когда данные не нужно форматировать перед сериализацией или десериализацией. В случае ошибки будет возвращено имя поля.

#### Параметры:

* **dump\_default** - Если установлено, это значение будет использоваться во время сериализации, если входное значение отсутствует. Если не задано, поле будет исключено из сериализованного вывода, если входное значение отсутствует. Может быть значением или вызываемым.
* **load\_default** - Значение десериализации по умолчанию для поля, если поле не найдено во входных данных. Может быть значением или вызываемым.
* **data\_key** - Имя ключа dict во внешнем представлении, т.е. вход load и выход dump. Если `None`, ключ будет соответствовать имени поля.
* **attribute** - Имя атрибута, из которого нужно получить значение при сериализации. Если `None`, предполагается, что атрибут имеет то же имя, что и поле. Примечание: Это следует использовать только в очень специфических случаях использования, таких как вывод нескольких полей для одного атрибута. В большинстве случаев вместо этого следует использовать **data\_key**.
* **validate** - Валидатор или набор валидаторов, которые вызываются во время десериализации. Валидатор принимает входное значение поля в качестве единственного параметра и возвращает логическое значение. Если он возвращает `False`, возникает **ValidationError**.
* **required** - Поднимает **ValidationError**, если значение поля не указано во время десериализации.
* **allow\_none** - Установите для этого параметра значение `True`, если `None` следует считать допустимым значением во время проверки/десериализации. Если `load_default=None` и **allow\_none** не установлено, по умолчанию будет установлено значение `True`. В противном случае по умолчанию используется значение `False`.
* **load\_only** - Если `True`, пропустите это поле при сериализации, иначе его значение будет присутствовать в сериализованных данных.
* **dump\_only** - Если `True`, пропустите это поле при десериализации, иначе его значение будет присутствовать в десериализованном объекте. В контексте HTTP API это фактически помечает поле как **«read-only»**.
* **error\_messages** (dict) - Переопределения для <mark style="color:red;">Field.default\_error\_messages</mark>.
* **metadata** - Дополнительная информация, которая будет храниться как метаданные поля.

_Изменено в версии 2.0.0_: Удален параметр ошибки. Вместо этого используйте **error\_messages**.

_Изменено в версии 2.0.0_: Добавлен параметр **allow\_none**, который делает проверку/десериализацию `None` согласованной для всех полей.

_Изменено в версии 2.0.0_: Добавлены параметры **load\_only** и **dump\_only**, которые позволяют пропускать поля в процессе (де)сериализации.

_Изменено в версии 2.0.0_: Добавлен параметр **missing**, указывающий значение для поля, если поле не найдено при десериализации.

_Изменено в версии 2.0.0_: значение по умолчанию **default** используется только в том случае, если оно установлено явно. В противном случае ввод отсутствующих значений исключается из сериализованного вывода.

_Изменено в версии 3.0.0b8_: Добавлен параметр **data\_key** для указания ключа во входных и выходных данных. Этот параметр заменил как **load\_from**, так и **dump\_to**.

#### Методы:

| Методы                                       | Описание                                                                                                |
| -------------------------------------------- | ------------------------------------------------------------------------------------------------------- |
| \_bind\_to\_schema(field\_name, schema)      | Обновляет поле значениями из его родительской схемы.                                                    |
| \_deserialize(value, attr, data, \*\*kwargs) | Десериализовать значение.                                                                               |
| \_serialize(value, attr, obj, \*\*kwargs)    | Сериализует **value** в базовый тип данных Python.                                                      |
| \_validate(value)                            | Выполняет проверку **value**.                                                                           |
| \_validate\_missing(value)                   | Проверяет отсутствующие значения.                                                                       |
| deserialize(value\[, attr, data])            | Десериализует значение **value**.                                                                       |
| fail(key, \*\*kwargs)                        | Вспомогательный метод, вызывающий **ValidationError** с сообщением об ошибке из `self.error_messages`.  |
| get\_value(obj, attr\[, accessor, default])  | Возвращает значение для данного ключа из объекта.                                                       |
| make\_error(key, \*\*kwargs)                 | Вспомогательный метод для создания **ValidationError** с сообщением об ошибке из `self.error_messages`. |
| serialize(attr, obj\[, accessor])            | Извлекает значение для данного ключа из объекта, применяет форматирование поля и возвращает результат.  |

#### Атрибуты:

| Атрибут                      | Описание                                                      |
| ---------------------------- | ------------------------------------------------------------- |
| context                      | Словарь контекста для родительской схемы.                     |
| **default\_error\_messages** | Сообщения об ошибках по умолчанию для различных типов ошибок. |

### \_bind\_to\_schema()

#### \_bind\_to\_schema(_field\_name_, _schema_)

Обновляет поле значениями из его родительской схемы. Вызывается `Schema._bind_field`.

#### Параметры:

* **field\_name** (str) - Имя поля, заданное в схеме.
* **schema** (Schema | Field) - Родительский объект.

### \_deserialize()

#### \_deserialize(_value: Any_, _attr: str | None_, _data: Mapping\[str, Any] | None_, _\*\*kwargs_)

Десериализовать значение. Конкретные классы Field должны реализовывать этот метод.

#### Параметры:

* **value** - Значение для десериализации.
* **attr** - Атрибут/ключ данных для десериализации.
* **data** - Необработанные входные данные передаются в Schema.load.
* **kwargs** - Аргументы ключевого слова для конкретных полей.

**Поднимает**: [ValidationError](isklyucheniya.md#exception-marshmallow.exceptions.validationerror-message-str-or-list-or-dict-field\_name-str-\_schema) - В случае сбоя форматирования или проверки.

**Возвращает**: Десериализованное значение.

_Изменено в версии 2.0.0_: Добавлены параметры **attr** и **data**.

_Изменено в версии 3.0.0_: Добавлены **\*\*kwargs** в сигнатуру.

### \_serialize()

#### \_serialize(_value: Any_, _attr: str_, _obj: Any_, _\*\*kwargs_)

Сериализует значение в базовый тип данных Python. Noop по умолчанию. Конкретные классы  Field должны реализовывать этот метод.

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

### \_validate()

#### \_validate(_value_)

Выполняет проверку значения **value**. Поднимает **ValidationError**, если проверка не прошла успешно.

### \_validate\_missing()

#### \_validate\_missing(_value_)

Проверяет отсутствующие значения. Поднимает **ValidationError**, если значение следует считать отсутствующим.

### _property_ context

Словарь контекста для родительской схемы.

### default\_error\_messages

#### default\_error\_messages _= {'null': 'Field may not be null.', 'required': 'Missing data for required field.', 'validator\_failed': 'Invalid value.'}_

Сообщения об ошибках по умолчанию для различных типов ошибок. Ключи в этом словаре передаются в <mark style="color:red;">Field.make\_error</mark>. Значения представляют собой сообщения об ошибках, передаваемые в [marshmallow.exceptions.ValidationError](isklyucheniya.md#exception-marshmallow.exceptions.validationerror-message-str-or-list-or-dict-field\_name-str-\_schema).

### deserialize()

#### deserialize(_value: Any_, _attr: str | None = None_, _data: Mapping\[str, Any] | None = None_, _\*\*kwargs_)

Десериализует значение **value**.

#### Параметры:

* **value** - Значение для десериализации.
* **attr** - Атрибут/ключ данных для десериализации.
* **data** - Необработанные входные данные передаются в Schema.load.
* **kwargs** - Аргументы ключевого слова для конкретных полей.

**Поднимает**: [ValidationError](isklyucheniya.md#exception-marshmallow.exceptions.validationerror-message-str-or-list-or-dict-field\_name-str-\_schema) - Если передано недопустимое значение или если требуемое значение отсутствует.

### fail()

#### fail(_key: str_, _\*\*kwargs_)

Вспомогательный метод, вызывающий **ValidationError** с сообщением об ошибке из `self.error_messages`.

_**Устарело**, начиная с версии 3.0.0_: вместо этого используйте <mark style="color:red;">make\_error</mark>.

### get\_value()

#### get\_value(_obj_, _attr_, _accessor=None_, _default=\<marshmallow.missing>_)

Возвращает значение для данного ключа из объекта.

#### Параметры:

* **obj** (object) - Объект, из которого нужно получить значение.
* **attr** (str) - Атрибут/ключ в объекте **obj**, из которого нужно получить значение.
* **accessor** (callable) - Вызываемый объект, используемый для получения значения **attr** из объекта **obj**. По умолчанию используется <mark style="color:red;">marshmallow.utils.get\_value</mark>.

### make\_error()

#### make\_error(_key: str_, _\*\*kwargs_)→ marshmallow.exceptions.ValidationError

Вспомогательный метод для создания **ValidationError** с сообщением об ошибке из `self.error_messages`.

### serialize()

#### serialize(_attr: str_, _obj: Any_, _accessor: Callable\[\[Any, str, Any], Any] | None = None_, _\*\*kwargs_)

Извлекает значение для данного ключа из объекта, применяет форматирование поля и возвращает результат.

#### Параметры:

* **attr** - Атрибут/ключ, который нужно получить от объекта.
* **obj** - Объект для доступа к атрибуту/ключу.
* **accessor** - Функция, используемая для доступа к значениям из **obj**.
* **kwargs** - Аргументы ключевого слова для конкретных полей.

## Float

#### _class_ marshmallow.fields.Float(_\*_, _allow\_nan: bool = False_, _as\_string: bool = False_, _\*\*kwargs_)

Число double как строка двойной точности IEEE-754.

#### Параметры:

* **allow\_nan** (bool) - Если `True`, разрешены **NaN**, **Infinity** и **-Infinity**, даже если они недопустимы в соответствии со спецификацией **JSON**.
* **as\_string** (bool) - Если `True`, форматирует значение как строку.
* **kwargs** - Те же аргументы ключевого слова, которые получает <mark style="color:red;">Number</mark>.

#### Методы:

| Метод              | Описание                                                                      |
| ------------------ | ----------------------------------------------------------------------------- |
| \_validated(value) | Форматирует значение или вызывает **ValidationError**, если возникнет ошибка. |

#### Атрибуты:

| Атрибут                      | Описание                           |
| ---------------------------- | ---------------------------------- |
| **default\_error\_messages** | Сообщения об ошибках по умолчанию. |

#### Классы:

| Класс     | Описание                                                                                    |
| --------- | ------------------------------------------------------------------------------------------- |
| num\_type | псевдоним для [float](https://python.readthedocs.io/en/latest/library/functions.html#float) |

### \_validated()

#### \_validated(_value_)

Форматирует значение **value** или вызывает **ValidationError**, если возникнет ошибка.

### default\_error\_messages

#### default\_error\_messages _= {'special': 'Special numeric values (nan or infinity) are not permitted.'}_

Сообщения об ошибках по умолчанию.

### num\_type

Псевдоним для [float](https://python.readthedocs.io/en/latest/library/functions.html#float)

| Методы num\_type     | Описание                                                                  |
| -------------------- | ------------------------------------------------------------------------- |
| as\_integer\_ratio() | Возвращает целочисленное отношение.                                       |
| conjugate()          | Возвращает self, комплексное сопряжение любого числа с плавающей запятой. |
| fromhex()            | Создает число с плавающей запятой из шестнадцатеричной строки.            |
| hex()                | Возвращает шестнадцатеричное представление числа с плавающей запятой.     |
| is\_integer()        | Возвратите True, если число с плавающей запятой является целым числом.    |

| Атрибуты num\_type | Описание                                |
| ------------------ | --------------------------------------- |
| imag               | мнимая часть комплексного числа         |
| real               | действительная часть комплексного числа |

## Function

#### _class_ marshmallow.fields.Function(_serialize: None | Callable\[\[Any], Any] | Callable\[\[Any, dict], Any] = None_, _deserialize: None | Callable\[\[Any], Any] | Callable\[\[Any, dict], Any] = None_, _\*\*kwargs_)

Поле, которое принимает значение, возвращаемое функцией.

#### Параметры:

* serialize - Вызываемый объект, из которого можно получить значение. Функция должна принимать единственный аргумент **obj**, который является сериализуемым объектом. Он также может дополнительно принимать аргумент **context**, который представляет собой словарь переменных контекста, передаваемых сериализатору. Если **callable** не указан, флаг **load\_only** будет установлен в `True`.
* deserialize - Вызываемый объект, из которого можно получить значение. Функция должна принимать один аргумент **value**, который является десериализуемым значением. Он также может дополнительно принимать аргумент **context**, который представляет собой словарь контекстных переменных, передаваемых десериализатору. Если вызываемый объект не указан, значение `value` будет передано без изменений.

_Изменено в версии 2.3.0_: Устарел параметр **func** в пользу **serialize**.

_Изменено в версии 3.0.0a1_: Удален параметр **func**.

#### Методы:

| Метод                                        | Описание                                           |
| -------------------------------------------- | -------------------------------------------------- |
| \_deserialize(value, attr, data, \*\*kwargs) | Десериализовать значение.                          |
| \_serialize(value, attr, obj, \*\*kwargs)    | Сериализует **value** в базовый тип данных Python. |

#### \_deserialize(_value_, _attr_, _data_, _\*\*kwargs_)

Десериализовать значение. Конкретные классы [Field](polya-fields-a-f.md#field) должны реализовывать этот метод.

#### Параметры:

* **value** - Значение для десериализации
* **attr** - Атрибут/ключ в данных для десериализации
* **data** - Необработанные входные данные передаются в `Schema.load`.
* **kwargs** - Аргументы ключевого слова, специфичные для [Field](polya-fields-a-f.md#field).

**Поднимает**: [ValidationError](isklyucheniya.md#exception-marshmallow.exceptions.validationerror-message-str-or-list-or-dict-field\_name-str-\_schema) - В случае сбоя форматирования или проверки.

**Возвращает**: Десериализованное значение.

_Изменено в версии 2.0.0_: Добавлены параметры **attr** и **data**.

_Изменено в версии 3.0.0_: Добавлены **\*\*kwargs** в сигнатуру.

#### \_serialize(_value_, _attr_, _obj_, _\*\*kwargs_)

Сериализует **value** в базовый тип данных Python. Noop по умолчанию. Конкретные классы  Field должны реализовывать этот метод.

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
* **kwargs** (_dict_) - Аргументы ключевого слова, специфичные для [Field](polya-fields-a-f.md#field).

**Возвращает**: Сериализованное значение.
