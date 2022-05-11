# Поля Fields (G-Z)

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

## IP

#### _class_ marshmallow.fields.IP(_\*args_, _exploded=False_, _\*\*kwargs_)

Поле IP-адреса.

#### Параметры:

* **exploded** (bool) - Если `True`, сериализовать адрес **ipv6** в длинной форме, т.е. с включенными группами, полностью состоящими из нулей.

_Новое в версии 3.8.0_.

#### Методы:

| Метод                                        | Описание                                           |
| -------------------------------------------- | -------------------------------------------------- |
| \_deserialize(value, attr, data, \*\*kwargs) | Десериализовать значение.                          |
| \_serialize(value, attr, obj, \*\*kwargs)    | Сериализует **value** в базовый тип данных Python. |

#### \_deserialize(_value_, _attr_, _data_, _\*\*kwargs_)→ ipaddress.IPv4Address | ipaddress.IPv6Address | None

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

#### \_serialize(_value_, _attr_, _obj_, _\*\*kwargs_)→ str | None

Сериализует **value** в базовый тип данных Python. Noop по умолчанию. Конкретные классы  [Field](polya-fields-a-f.md#field) должны реализовывать этот метод.

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

## IPInterface

#### _class_ marshmallow.fields.IPInterface(_\*args_, _exploded=False_, _\*\*kwargs_)

Поле IPInterface.

IP-интерфейс — это нестрогая форма типа IPNetwork, где всегда принимаются произвольные адреса узлов.

IP-адрес и маска, например. «192.168.0.2/24» или «192.168.0.2/255.255.255.0»

см. [https://python.readthedocs.io/en/latest/library/ipaddress.html#interface-objects](https://python.readthedocs.io/en/latest/library/ipaddress.html#interface-objects)

#### Параметры:

* **exploded** (bool) - Если `True`, сериализовать адрес **ipv6** в длинной форме, т.е. с включенными группами, полностью состоящими из нулей.

_Новое в версии 3.8.0_.

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

#### \_serialize(_value_, _attr_, _obj_, _\*\*kwargs_)→ str | None

Сериализует **value** в базовый тип данных Python. Noop по умолчанию. Конкретные классы  [Field](polya-fields-a-f.md#field) должны реализовывать этот метод.

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

## IPv4

#### _class_ marshmallow.fields.IPv4(_\*args_, _exploded=False_, _\*\*kwargs_)

Поле адреса IPv4.

_Новое в версии 3.8.0_.

#### Классы:

| Класс                  | Описание                                                                                                                |
| ---------------------- | ----------------------------------------------------------------------------------------------------------------------- |
| DESERIALIZATION\_CLASS | псевдоним [ipaddress.IPv4Address](https://python.readthedocs.io/en/latest/library/ipaddress.html#ipaddress.IPv4Address) |

### DESERIALIZATION\_CLASS

Псевдоним [ipaddress.IPv4Address](https://python.readthedocs.io/en/latest/library/ipaddress.html#ipaddress.IPv4Address)

#### Атрибуты:

| Атрибут             | Описание                                                       |
| ------------------- | -------------------------------------------------------------- |
| **is\_link\_local** | Проверяет, зарезервирован ли адрес для link-local.             |
| is\_loopback        | Проверяет, является ли адрес петлевым адресом.                 |
| is\_multicast       | Проверяет, зарезервирован ли адрес для многоадресной рассылки. |
| is\_private         | Проверяет, выделен ли этот адрес для частных сетей.            |
| is\_reserved        | Проверяет, не зарезервирован ли адрес в противном случае IETF. |
| is\_unspecified     | Проверяет, не указан ли адрес.                                 |
| packed              | Двоичное представление этого адреса.                           |

## IPv4Interface

#### _class_ marshmallow.fields.IPv4Interface(_\*args_, _exploded=False_, _\*\*kwargs_)

Поле сетевого интерфейса IPv4.

#### Классы:

| Класс                  | Описание                                                                                                                    |
| ---------------------- | --------------------------------------------------------------------------------------------------------------------------- |
| DESERIALIZATION\_CLASS | псевдоним [ipaddress.IPv4Interface](https://python.readthedocs.io/en/latest/library/ipaddress.html#ipaddress.IPv4Interface) |

### DESERIALIZATION\_CLASS

Псевдоним [ipaddress.IPv4Interface](https://python.readthedocs.io/en/latest/library/ipaddress.html#ipaddress.IPv4Interface)

## IPv6

#### _class_ marshmallow.fields.IPv6(_\*args_, _exploded=False_, _\*\*kwargs_)

Поле адреса IPv6.

_Новое в версии 3.8.0_.

#### Классы:

| Класс                  | Описание                                                                                                                |
| ---------------------- | ----------------------------------------------------------------------------------------------------------------------- |
| DESERIALIZATION\_CLASS | псевдоним [ipaddress.IPv6Address](https://python.readthedocs.io/en/latest/library/ipaddress.html#ipaddress.IPv6Address) |

### DESERIALIZATION\_CLASS

Псевдоним [ipaddress.IPv6Address](https://python.readthedocs.io/en/latest/library/ipaddress.html#ipaddress.IPv6Address)

#### Атрибуты:

| Атрибут             | Описание                                                       |
| ------------------- | -------------------------------------------------------------- |
| ipv4\_mapped        | Возвращает сопоставленный адрес IPv4.                          |
| is\_global          | Проверяет, выделен ли этот адрес для общедоступных сетей.      |
| **is\_link\_local** | Проверяет, зарезервирован ли адрес для link-local.             |
| is\_loopback        | Проверяет, является ли адрес петлевым адресом.                 |
| is\_multicast       | Проверяет, зарезервирован ли адрес для многоадресной рассылки. |
| is\_private         | Проверяет, выделен ли этот адрес для частных сетей.            |
| is\_reserved        | Проверяет, не зарезервирован ли адрес в противном случае IETF. |
| **is\_site\_local** | Проверяет, зарезервирован ли адрес для локального сайта.       |
| is\_unspecified     | Проверяет, не указан ли адрес.                                 |
| packed              | Двоичное представление этого адреса.                           |
| sixtofour           | Возвращает встроенный адрес IPv4 6to4.                         |
| teredo              | Кортеж встроенных IP-адресов Teredo.                           |

## IPv6Interface

#### _class_ marshmallow.fields.IPv6Interface(_\*args_, _exploded=False_, _\*\*kwargs_)

Поле сетевого интерфейса IPv6.

#### Классы:

| Класс                  | Описание                                                                                                                    |
| ---------------------- | --------------------------------------------------------------------------------------------------------------------------- |
| DESERIALIZATION\_CLASS | псевдоним [ipaddress.IPv6Interface](https://python.readthedocs.io/en/latest/library/ipaddress.html#ipaddress.IPv6Interface) |

### DESERIALIZATION\_CLASS

Псевдоним [ipaddress.IPv6Interface](https://python.readthedocs.io/en/latest/library/ipaddress.html#ipaddress.IPv6Interface)

| Атрибут         | Описание                                       |
| --------------- | ---------------------------------------------- |
| is\_loopback    | Проверяет, является ли адрес петлевым адресом. |
| is\_unspecified | Проверяет, не указан ли адрес.                 |

## Int

#### marshmallow.fields.Int

Псевдоним [marshmallow.fields.Integer](polya-fields-g-z.md#integer)

| Метод              | Описание                                                                                                                                                                                            |
| ------------------ | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| \_validated(value) | Форматирует значение или вызывает [ValidationError](isklyucheniya.md#exception-marshmallow.exceptions.validationerror-message-str-or-list-or-dict-field\_name-str-\_schema), если возникнет ошибка. |

| Класс     | Описание                                                                            |
| --------- | ----------------------------------------------------------------------------------- |
| num\_type | псевдоним [int](https://python.readthedocs.io/en/latest/library/functions.html#int) |

## Integer

#### _class_ marshmallow.fields.Integer(_\*_, _strict: bool = False_, _\*\*kwargs_)

Целочисленное поле.

#### Параметры:

* **strict** - Если `True`, допустимы только целочисленные типы. В противном случае допустимо любое значение, которое можно привести к типу [int](https://python.readthedocs.io/en/latest/library/functions.html#int).
* **kwargs** - Те же аргументы ключевого слова, которые получает <mark style="color:red;">Number</mark>.

| Метод              | Описание                                                                                                                                                                                            |
| ------------------ | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| \_validated(value) | Форматирует значение или вызывает [ValidationError](isklyucheniya.md#exception-marshmallow.exceptions.validationerror-message-str-or-list-or-dict-field\_name-str-\_schema), если возникнет ошибка. |

| Атрибут                      | Описание                           |
| ---------------------------- | ---------------------------------- |
| **default\_error\_messages** | Сообщения об ошибках по умолчанию. |

| Класс     | Описание                                                                            |
| --------- | ----------------------------------------------------------------------------------- |
| num\_type | псевдоним [int](https://python.readthedocs.io/en/latest/library/functions.html#int) |

### \_validated(_value_)

Форматирует значение или вызывает [ValidationError](isklyucheniya.md#exception-marshmallow.exceptions.validationerror-message-str-or-list-or-dict-field\_name-str-\_schema), если возникнет ошибка.

### default\_error\_messages _= {'invalid': 'Not a valid integer.'}_

Сообщения об ошибках по умолчанию.

### num\_type

псевдоним [int](https://python.readthedocs.io/en/latest/library/functions.html#int)

| Метод                                       | Описание                                                                        |
| ------------------------------------------- | ------------------------------------------------------------------------------- |
| as\_integer\_ratio()                        | Возвращает целочисленное отношение.                                             |
| bit\_length()                               | Количество битов, необходимых для представления самого себя в двоичном формате. |
| conjugate                                   | Возвращает self, комплексное сопряжение любого int.                             |
| from\_bytes(byteorder, \*\[, signed])       | Возвращает целое число, представленное данным массивом байтов.                  |
| to\_bytes(length, byteorder, \*\[, signed]) | Возвращает массив байтов, представляющий целое число.                           |

| Атрибут     | Описание                                              |
| ----------- | ----------------------------------------------------- |
| denominator | знаменатель рационального числа в наименьших условиях |
| imag        | мнимая часть комплексного числа                       |
| numerator   | числитель рационального числа в наименьших условиях   |
| real        | действительная часть комплексного числа               |

## List

#### _class_ marshmallow.fields.List(_cls\_or\_instance: Field | type_, _\*\*kwargs_)

### _class_ marshmallow.fields.Method(_serialize: str | None = None_, _deserialize: str | None = None_, _\*\*kwargs_)

### _class_ marshmallow.fields.Nested(_nested: SchemaABC | type | str | dict\[str, Field | type] | typing.Callable\[\[], SchemaABC | dict\[str, Field | type]], \*, dump\_default: typing.Any = \<marshmallow.missing>, default: typing.Any = \<marshmallow.missing>, only: types.StrSequenceOrSet | None = None, exclude: types.StrSequenceOrSet = (), many: bool = False, unknown: str | None = None, \*\*kwargs_)

### _class_ marshmallow.fields.Pluck(_nested: SchemaABC | type | str | Callable\[\[], SchemaABC]_, _field\_name: str_, _\*\*kwargs_)

### _class_ marshmallow.fields.String(_\*, load\_default: typing.Any = \<marshmallow.missing>, missing: typing.Any = \<marshmallow.missing>, dump\_default: typing.Any = \<marshmallow.missing>, default: typing.Any = \<marshmallow.missing>, data\_key: str | None = None, attribute: str | None = None, validate: None | (typing.Callable\[\[typing.Any], typing.Any] | typing.Iterable\[typing.Callable\[\[typing.Any], typing.Any]]) = None, required: bool = False, allow\_none: bool | None = None, load\_only: bool = False, dump\_only: bool = False, error\_messages: dict\[str, str] | None = None, metadata: typing.Mapping\[str, typing.Any] | None = None, \*\*additional\_metadata_)

### marshmallow.fields.URL
