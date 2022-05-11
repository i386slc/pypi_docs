# Поля Fields (G-Z)

Классы полей для различных типов данных.

## Классы:

| Класс                                                                               | Описание                                                                                                                                                              |
| ----------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| [AwareDateTime](polya-fields-a-f.md#awaredatetime)(\[format, default\_timezone])    | Отформатированная осведомленная строка даты и времени.                                                                                                                |
| [Bool](polya-fields-a-f.md#bool)                                                    | псевдоним [marshmallow.fields.Boolean](polya-fields-a-f.md#boolean)                                                                                                   |
| [Boolean](polya-fields-a-f.md#boolean)(\*\[, truthy, falsy])                        | Логическое поле                                                                                                                                                       |
| [Constant](polya-fields-a-f.md#constant)(constant, \*\*kwargs)                      | Поле, которое (де)сериализуется в предустановленную константу.                                                                                                        |
| [Date](polya-fields-a-f.md#date)(\[format])                                         | Строка даты в формате ISO8601.                                                                                                                                        |
| [DateTime](polya-fields-a-f.md#datetime)(\[format])                                 | Отформатированная строка даты и времени.                                                                                                                              |
| [Decimal](polya-fields-a-f.md#decimal)(\[places, rounding, allow\_nan, as\_string]) | Поле, которое (де)сериализуется в тип Python **decimal.Decimal**.                                                                                                     |
| [Dict](polya-fields-a-f.md#dict)(\[keys, values])                                   | Поле словаря                                                                                                                                                          |
| [Email](polya-fields-a-f.md#email)(\*args, \*\*kwargs)                              | Поле электронной почты                                                                                                                                                |
| [Field](polya-fields-a-f.md#field)(\*, load\_default, missing, ...)                 | Основное поле, из которого должны расширяться другие поля                                                                                                             |
| [Float](polya-fields-a-f.md#float)(\*\[, allow\_nan, as\_string])                   | **Double** как строка двойной точности IEEE-754                                                                                                                       |
| [Function](polya-fields-a-f.md#function)(\[serialize, deserialize])                 | Поле, принимающее значение, возвращенное функцией                                                                                                                     |
| [IP](polya-fields-g-z.md#ip)(\*args\[, exploded])                                   | Поле IP-адреса                                                                                                                                                        |
| [IPInterface](polya-fields-g-z.md#ipinterface)(\*args\[, exploded])                 | Поле IP-интерфейса                                                                                                                                                    |
| [IPv4](polya-fields-g-z.md#ipv4)(\*args\[, exploded])                               | Поле адреса IPv4                                                                                                                                                      |
| [IPv4Interface](polya-fields-g-z.md#ipv4interface)(\*args\[, exploded])             | Поле сетевого интерфейса IPv4                                                                                                                                         |
| [IPv6](polya-fields-g-z.md#ipv6)(\*args\[, exploded])                               | Поле адреса IPv6                                                                                                                                                      |
| [IPv6Interface](polya-fields-g-z.md#ipv6interface)(\*args\[, exploded])             | Поле сетевого интерфейса IPv6                                                                                                                                         |
| [Int](polya-fields-g-z.md#int)                                                      | псевдоним [marshmallow.fields.Integer](polya-fields-g-z.md#integer)                                                                                                   |
| [Integer](polya-fields-g-z.md#integer)(\*\[, strict])                               | Целочисленное поле                                                                                                                                                    |
| [List](polya-fields-g-z.md#list)(cls\_or\_instance, \*\*kwargs)                     | Поле списка, составленное из другого класса или экземпляра [Field](polya-fields-a-f.md#field)                                                                         |
| [Mapping](polya-fields-g-z.md#mapping)(\[keys, values])                             | Абстрактный класс для объектов с парами ключ-значение                                                                                                                 |
| [Method](polya-fields-g-z.md#method)(\[serialize, deserialize])                     | Поле, которое принимает значение, возвращаемое методом схемы                                                                                                          |
| [NaiveDateTime](polya-fields-g-z.md#naivedatetime)(\[format, timezone])             | Отформатированная наивная строка даты и времени.                                                                                                                      |
| [Nested](polya-fields-g-z.md#nested)(nested, ...)                                   | Позволяет вложить [Schema](skhema-schema.md#class-marshmallow.schema) внутрь поля.                                                                                    |
| [Number](polya-fields-g-z.md#number)(\*\[, as\_string])                             | Базовый класс для числовых полей.                                                                                                                                     |
| [Pluck](polya-fields-g-z.md#pluck)(nested, field\_name, \*\*kwargs)                 | Позволяет заменить вложенные данные одним из полей данных.                                                                                                            |
| [Raw](polya-fields-g-z.md#raw)(\*, load\_default, missing, dump\_default, ...)      | Поле, к которому не применяется форматирование.                                                                                                                       |
| [Str](polya-fields-g-z.md#str)                                                      | псевдоним [marshmallow.fields.String](polya-fields-g-z.md#string)                                                                                                     |
| [String](polya-fields-g-z.md#string)(\*, load\_default, missing, ...)               | Строковое поле.                                                                                                                                                       |
| [Time](polya-fields-g-z.md#time)(\[format])                                         | Отформатированная строка времени.                                                                                                                                     |
| [TimeDelta](polya-fields-g-z.md#timedelta)(\[precision])                            | Поле, которое (де)сериализует объект [datetime.timedelta](https://python.readthedocs.io/en/latest/library/datetime.html#datetime.timedelta) в целое число и наоборот. |
| [Tuple](polya-fields-g-z.md#tuple)(tuple\_fields, \*args, \*\*kwargs)               | Поле кортежа, состоящее из фиксированного числа других классов или экземпляров Field                                                                                  |
| [URL](polya-fields-g-z.md#url)                                                      | псевдоним [marshmallow.fields.Url](polya-fields-g-z.md#url-1)                                                                                                         |
| [UUID](polya-fields-g-z.md#uuid)(\*, load\_default, missing, dump\_default, ...)    | Поле UUID.                                                                                                                                                            |
| [Url](polya-fields-g-z.md#url-1)(\*\[, relative, schemes, require\_tld])            | Поле URL.                                                                                                                                                             |

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

Поле списка, составленное из другого класса [Field](polya-fields-a-f.md#field) или экземпляра.

Пример:

```python
numbers = fields.List(fields.Float())
```

#### Параметры:

* **cls\_or\_instance** - Класс поля или экземпляр.
* **kwargs** - Те же аргументы ключевого слова, которые получает [Field](polya-fields-a-f.md#field).

_Изменено в версии 2.0.0_: параметр **allow\_none** теперь применяется к десериализации и имеет ту же семантику, что и другие поля.

_Изменено в версии 3.0.0rc9_: скалярные значения не сериализуются в списки из одного элемента.

| Методы                                       | Описание                                               |
| -------------------------------------------- | ------------------------------------------------------ |
| **\_bind\_to\_schema**(field\_name, schema)  | Обновить поле со значениями из его родительской схемы. |
| \_deserialize(value, attr, data, \*\*kwargs) | Десериализовать значение.                              |
| \_serialize(value, attr, obj, \*\*kwargs)    | Сериализует **value** в базовый тип данных Python.     |

| Атрибут                      | Описание                           |
| ---------------------------- | ---------------------------------- |
| **default\_error\_messages** | Сообщения об ошибках по умолчанию. |

### \_bind\_to\_schema(_field\_name_, _schema_)

Обновляет поле со значениями из его родительской схемы. Вызывается `Schema._bind_field`.

#### Параметры:

* **field\_name** (str) - Имя поля задано в схеме.
* **schema** (Schema | Field) - Родительский объект.

### \_deserialize(_value_, _attr_, _data_, _\*\*kwargs_) → list\[Any]

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

### \_serialize(_value_, _attr_, _obj_, _\*\*kwargs_) → list\[Any] | None

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

### default\_error\_messages _= {'invalid': 'Not a valid list.'}_

Сообщения об ошибках по умолчанию.

## Mapping

#### _class_ marshmallow.fields.Mapping(_keys: Field | type | None = None_, _values: Field | type | None = None_, _\*\*kwargs_)

Абстрактный класс для объектов с парами ключ-значение.

* **keys** - Класс поля или экземпляр для ключей dict.
* **values** - Класс поля или экземпляр для значений dict.
* **kwargs** - Те же аргументы ключевого слова, которые получает [Field](polya-fields-a-f.md#field).

{% hint style="info" %}
Если структура вложенных данных неизвестна, вы можете опустить аргументы **keys** и **values**, чтобы предотвратить проверку содержимого.
{% endhint %}

_Новое в версии 3.0.0rc4_.

| Методы                                       | Описание                                               |
| -------------------------------------------- | ------------------------------------------------------ |
| **\_bind\_to\_schema**(field\_name, schema)  | Обновить поле со значениями из его родительской схемы. |
| \_deserialize(value, attr, data, \*\*kwargs) | Десериализовать значение.                              |
| \_serialize(value, attr, obj, \*\*kwargs)    | Сериализует **value** в базовый тип данных Python.     |

| Атрибут                      | Описание                           |
| ---------------------------- | ---------------------------------- |
| **default\_error\_messages** | Сообщения об ошибках по умолчанию. |

| Классы        | Описание                                                                             |
| ------------- | ------------------------------------------------------------------------------------ |
| mapping\_type | псевдоним [dict](https://python.readthedocs.io/en/latest/library/stdtypes.html#dict) |

### \_bind\_to\_schema(_field\_name_, _schema_)

Обновляет поле со значениями из его родительской схемы. Вызывается `Schema._bind_field`.

#### Параметры:

* **field\_name** (str) - Имя поля задано в схеме.
* **schema** (Schema | Field) - Родительский объект.

### \_deserialize(_value_, _attr_, _data_, _\*\*kwargs_) → list\[Any]

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

### \_serialize(_value_, _attr_, _obj_, _\*\*kwargs_) → list\[Any] | None

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

### default\_error\_messages _= {'invalid': 'Not a valid mapping type.'}_

Сообщения об ошибках по умолчанию.

### mapping\_type

Псевдоним [dict](https://python.readthedocs.io/en/latest/library/stdtypes.html#dict)

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

## Method

#### _class_ marshmallow.fields.Method(_serialize: str | None = None_, _deserialize: str | None = None_, _\*\*kwargs_)

Поле, которое принимает значение, возвращаемое методом схемы.

#### Параметры:

* **serialize** - Имя метода схемы, из которого извлекается значение. Метод должен принимать аргумент **obj** (в дополнение к **self**), который является сериализуемым объектом.
* **deserialize** - Необязательное имя метода Schema для десериализации значения. Метод должен принимать один аргумент **value**, который является значением для десериализации.

_Изменено в версии 2.0.0_: удален необязательный параметр **context** в методах. Вместо этого используйте `self.context`.

_Изменено в версии 2.3.0_: параметр **method\_name** устарел в пользу **serialize** и позволяет вообще не передавать **serialize**.

_Изменено в версии 3.0.0_: Удален параметр **method\_name**.

### \_bind\_to\_schema(_field\_name_, _schema_)

Обновляет поле со значениями из его родительской схемы. Вызывается `Schema._bind_field`.

#### Параметры:

* **field\_name** (str) - Имя поля задано в схеме.
* **schema** (Schema | Field) - Родительский объект.

### \_deserialize(_value_, _attr_, _data_, _\*\*kwargs_) → list\[Any]

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

### \_serialize(_value_, _attr_, _obj_, _\*\*kwargs_) → list\[Any] | None

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

## NaiveDateTime

#### _class_ marshmallow.fields.NaiveDateTime(_format: str | None = None_, _\*_, _timezone: dt.timezone | None = None_, _\*\*kwargs_)

Отформатированная наивная строка даты и времени.

#### **Параметры:**

* **format** - Смотри [DateTime](polya-fields-a-f.md#datetime)
* **timezone** - Используется при десериализации. Если `None`, осведомленные даты и время отклоняются. Если не `None`, осведомленные даты и время преобразуются в этот часовой пояс, прежде чем их информация о часовом поясе будет удалена.
* **kwargs** - Те же аргументы ключевого слова, которые получает [Field](polya-fields-a-f.md#field).

_Новое в версии 3.0.0rc9_.

| Метод                                        | Описание                  |
| -------------------------------------------- | ------------------------- |
| \_deserialize(value, attr, data, \*\*kwargs) | Десериализовать значение. |

### \_deserialize(_value_, _attr_, _data_, _\*\*kwargs_) → list\[Any]

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

## Nested

#### _class_ marshmallow.fields.Nested(_nested: SchemaABC | type | str | dict\[str, Field | type] | typing.Callable\[\[], SchemaABC | dict\[str, Field | type]], \*, dump\_default: typing.Any = \<marshmallow.missing>, default: typing.Any = \<marshmallow.missing>, only: types.StrSequenceOrSet | None = None, exclude: types.StrSequenceOrSet = (), many: bool = False, unknown: str | None = None, \*\*kwargs_)

Позволяет вложить схему внутрь поля.

Примеры:

```python
class ChildSchema(Schema):
    id = fields.Str()
    name = fields.Str()
    # Используйте лямбда-функции, когда вам нужно двустороннее вложение
    # или самовложение.
    parent = fields.Nested(lambda: ParentSchema(only=("id",)), dump_only=True)
    siblings = fields.List(fields.Nested(lambda: ChildSchema(only=("id", "name"))))

class ParentSchema(Schema):
    id = fields.Str()
    children = fields.List(
        fields.Nested(ChildSchema(only=("id", "parent", "siblings")))
    )
    spouse = fields.Nested(lambda: ParentSchema(only=("id",)))
```

При передаче экземпляра [Schema](skhema-schema.md#class-marshmallow.schema) в качестве первого аргумента будут учитываться только атрибуты экземпляров, представленные в аргументах **exclude**, **only** и **many**.

Следовательно, при передаче аргументов **exclude**, **only** и **many** в `fields.Nested`, вы должны передать класс [Schema](skhema-schema.md#class-marshmallow.schema) (а не экземпляр) в качестве первого аргумента.

```python
# Да
author = fields.Nested(UserSchema, only=('id', 'name'))

# Нет
author = fields.Nested(UserSchema(), only=('id', 'name'))
```

#### Параметры:

* **nested** - Экземпляр схемы, класс, имя класса (строка), словарь или вызываемый объект, который возвращает схему или словарь. Словари конвертируются с помощью `Schema.from_dict`.
* **exclude** - Список или кортеж полей для исключения.
* **only** - Список или кортеж полей для маршалинга. Если `None`, маршалируются все поля. Этот параметр имеет приоритет над параметром **exclude**.
* **many** - Является ли поле набором объектов.
* **unknown** - Следует ли исключить, включить или вызвать ошибку для неизвестных полей в данных. Используйте EXCLUDE, INCLUDE или RAISE.
* **kwargs** - Те же аргументы ключевого слова, которые получает [Field](polya-fields-a-f.md#field).

| Методы                                          | Описание                                                                                                               |
| ----------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------- |
| \_deserialize(value, attr, data\[, partial])    | То же, что и [Field.\_deserialize()](polya-fields-a-f.md#\_deserialize-4), но с дополнительным аргументом **partial**. |
| \_serialize(nested\_obj, attr, obj, \*\*kwargs) | Сериализует значение в базовый тип данных Python.                                                                      |

| Атрибуты                     | Описание                           |
| ---------------------------- | ---------------------------------- |
| **default\_error\_messages** | Сообщения об ошибках по умолчанию. |
| schema                       | Вложенный объект схемы Schema.     |

### \_deserialize(_value_, _attr_, _data_, _partial=None_, _\*\*kwargs_)

То же, что и [Field.\_deserialize()](polya-fields-a-f.md#\_deserialize-4), но с дополнительным аргументом **partial**.

#### Параметры:

* **partial (bool | tuple)**- Для вложенных схем параметр **partial** передается в `Schema.load`.

_Изменено в версии 3.0.0_: Добавлен параметр **partial**.

### \_serialize(_value_, _attr_, _obj_, _\*\*kwargs_) → list\[Any] | None

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

### default\_error\_messages _= {'type': 'Invalid type.'}_

Сообщения об ошибках по умолчанию

### _property_ schema

Вложенный объект схемы Schema.

_Изменено в версии 1.0.0_: **serializer** переименован в **schema**.

## Number

#### _class_ marshmallow.fields.Number(_\*_, _as\_string: bool = False_, _\*\*kwargs_)

Базовый класс для числовых полей.

#### Параметры:

* **as\_string** (bool) - Если `True`, форматирует сериализованное значение как строку.
* **kwargs** - Те же аргументы ключевого слова, которые получает [Field](polya-fields-a-f.md#field).

| Методы                                       | Описание                                                                                                                                                                                            |
| -------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| \_deserialize(value, attr, data, \*\*kwargs) | Десериализовать значение.                                                                                                                                                                           |
| \_format\_num(value)                         | Возвращает числовое значение для значения, учитывая **num\_type** этого поля.                                                                                                                       |
| \_serialize(value, attr, obj, \*\*kwargs)    | Возвращает строку, если `self.as_string=True`, иначе возвращает **num\_type** этого поля.                                                                                                           |
| \_validated(value)                           | Форматирует значение или вызывает [ValidationError](isklyucheniya.md#exception-marshmallow.exceptions.validationerror-message-str-or-list-or-dict-field\_name-str-\_schema), если возникнет ошибка. |

| Атрибут                      | Описание                           |
| ---------------------------- | ---------------------------------- |
| **default\_error\_messages** | Сообщения об ошибках по умолчанию. |

| Класс     | Описание                                                                                |
| --------- | --------------------------------------------------------------------------------------- |
| num\_type | псевдоним [float](https://python.readthedocs.io/en/latest/library/functions.html#float) |

### \_deserialize(_value_, _attr_, _data_, _\*\*kwargs_) → \_T | None

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

### \_format\_num(_value_) → Any

Возвращает числовое значение для значения, учитывая **num\_type** этого поля.

### \_serialize(_value_, _attr_, _obj_, _\*\*kwargs_) → str | \_T | None

Возвращает строку, если `self.as_string=True`, иначе возвращает **num\_type** этого поля.

### \_validated(_value_) → \_T | None

Форматирует значение или вызывает [ValidationError](isklyucheniya.md#exception-marshmallow.exceptions.validationerror-message-str-or-list-or-dict-field\_name-str-\_schema), если возникнет ошибка.

### default\_error\_messages _= {'invalid': 'Not a valid number.', 'too\_large': 'Number too large.'}_

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

## Pluck

#### _class_ marshmallow.fields.Pluck(_nested: SchemaABC | type | str | Callable\[\[], SchemaABC]_, _field\_name: str_, _\*\*kwargs_)

Позволяет заменить вложенные данные одним из полей данных.

Пример:

```python
from marshmallow import Schema, fields

class ArtistSchema(Schema):
    id = fields.Int()
    name = fields.Str()

class AlbumSchema(Schema):
    artist = fields.Pluck(ArtistSchema, 'id')

in_data = {'artist': 42}
loaded = AlbumSchema().load(in_data) # => {'artist': {'id': 42}}
dumped = AlbumSchema().dump(loaded)  # => {'artist': 42}
```

#### Параметры:

* **nested** (Schema) - Класс **Schema** или имя класса (строка) для вложения или `"self"` для вложения схемы в себя.
* **field\_name** (str) - Ключ, из которого извлекается значение.
* **kwargs** - Те же аргументы ключевого слова, которые получает [Nested](polya-fields-g-z.md#nested).

| Методы                                          | Описание                                                                                                               |
| ----------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------- |
| \_deserialize(value, attr, data\[, partial])    | То же, что и [Field.\_deserialize()](polya-fields-a-f.md#\_deserialize-4), но с дополнительным аргументом **partial**. |
| \_serialize(nested\_obj, attr, obj, \*\*kwargs) | Сериализует значение в базовый тип данных Python.                                                                      |

### \_deserialize(_value_, _attr_, _data_, _partial=None_, _\*\*kwargs_)

То же, что и [Field.\_deserialize()](polya-fields-a-f.md#\_deserialize-4), но с дополнительным аргументом **partial**.

#### Параметры:

* **partial (bool | tuple)**- Для вложенных схем параметр **partial** передается в `Schema.load`.

_Изменено в версии 3.0.0_: Добавлен параметр **partial**.

### \_serialize(_nested\_obj_, _attr_, _obj_, _\*\*kwargs_) → list\[Any] | None

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

## Raw

#### _class_ marshmallow.fields.Raw(_\*, load\_default: typing.Any = \<marshmallow.missing>, missing: typing.Any = \<marshmallow.missing>, dump\_default: typing.Any = \<marshmallow.missing>, default: typing.Any = \<marshmallow.missing>, data\_key: str | None = None, attribute: str | None = None, validate: None | (typing.Callable\[\[typing.Any], typing.Any] | typing.Iterable\[typing.Callable\[\[typing.Any], typing.Any]]) = None, required: bool = False, allow\_none: bool | None = None, load\_only: bool = False, dump\_only: bool = False, error\_messages: dict\[str, str] | None = None, metadata: typing.Mapping\[str, typing.Any] | None = None, \*\*additional\_metadata_)

Поле, к которому не применяется форматирование.

## Str

#### marshmallow.fields.Str

Псевдоним [marshmallow.fields.String](polya-fields-g-z.md#undefined)

#### Методы:

| Метод                                        | Описание                                           |
| -------------------------------------------- | -------------------------------------------------- |
| \_deserialize(value, attr, data, \*\*kwargs) | Десериализовать значение.                          |
| \_serialize(value, attr, obj, \*\*kwargs)    | Сериализует **value** в базовый тип данных Python. |

## String

#### _class_ marshmallow.fields.String(_\*, load\_default: typing.Any = \<marshmallow.missing>, missing: typing.Any = \<marshmallow.missing>, dump\_default: typing.Any = \<marshmallow.missing>, default: typing.Any = \<marshmallow.missing>, data\_key: str | None = None, attribute: str | None = None, validate: None | (typing.Callable\[\[typing.Any], typing.Any] | typing.Iterable\[typing.Callable\[\[typing.Any], typing.Any]]) = None, required: bool = False, allow\_none: bool | None = None, load\_only: bool = False, dump\_only: bool = False, error\_messages: dict\[str, str] | None = None, metadata: typing.Mapping\[str, typing.Any] | None = None, \*\*additional\_metadata_)

Строковое поле.

#### Параметры:

* **kwargs** - Те же аргументы ключевого слова, которые получает [Field](polya-fields-a-f.md#field).

| Методы                                       | Описание                                           |
| -------------------------------------------- | -------------------------------------------------- |
| \_deserialize(value, attr, data, \*\*kwargs) | Десериализовать значение.                          |
| \_serialize(value, attr, obj, \*\*kwargs)    | Сериализует **value** в базовый тип данных Python. |

| Атрибут                      | Описание                           |
| ---------------------------- | ---------------------------------- |
| **default\_error\_messages** | Сообщения об ошибках по умолчанию. |

### \_deserialize(_value_, _attr_, _data_, _\*\*kwargs_) → list\[Any]

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

### \_serialize(_value_, _attr_, _obj_, _\*\*kwargs_) → list\[Any] | None

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

### default\_error\_messages _= {'invalid': 'Not a valid string.', 'invalid\_utf8': 'Not a valid utf-8 string.'}_

Сообщения об ошибках по умолчанию.

## Time

#### _class_ marshmallow.fields.Time(_format: str | None = None_, _\*\*kwargs_)

Отформатированная строка времени.

Пример: `«03:12:58.019077»`

#### Параметры:

* **format** - Либо "iso" (для ISO8601), либо строка формата даты. Если нет, по умолчанию используется «iso».
* **kwargs** - Те же аргументы ключевого слова, которые получает [Field](polya-fields-a-f.md#field).

## TimeDelta

#### _class_ marshmallow.fields.TimeDelta(_precision: str = 'seconds'_, _\*\*kwargs_)

Поле, которое (де)сериализует объект [datetime.timedelta](https://python.readthedocs.io/en/latest/library/datetime.html#datetime.timedelta) в целое число и наоборот. Целое число может представлять количество дней, секунд или микросекунд.

#### Параметры:

* **precision** - Влияет на то, как целое число интерпретируется во время (де)сериализации. Должны быть «days», «seconds», «microseconds», «milliseconds», «minutes», «hours» или «weeks».
* **kwargs** - Те же аргументы ключевого слова, которые получает [Field](polya-fields-a-f.md#field).

_Изменено в версии 2.0.0_: всегда сериализуется в целочисленное значение, чтобы избежать ошибок округления. Добавьте параметр **precision**.

| Методы                                       | Описание                                           |
| -------------------------------------------- | -------------------------------------------------- |
| \_deserialize(value, attr, data, \*\*kwargs) | Десериализовать значение.                          |
| \_serialize(value, attr, obj, \*\*kwargs)    | Сериализует **value** в базовый тип данных Python. |

| Атрибут                      | Описание                           |
| ---------------------------- | ---------------------------------- |
| **default\_error\_messages** | Сообщения об ошибках по умолчанию. |

### \_deserialize(_value_, _attr_, _data_, _\*\*kwargs_) → list\[Any]

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

### \_serialize(_value_, _attr_, _obj_, _\*\*kwargs_) → list\[Any] | None

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

### default\_error\_messages _= {'format': '{input!r} cannot be formatted as a timedelta.', 'invalid': 'Not a valid period of time.'}_

Сообщения об ошибках по умолчанию.

## Tuple

#### _class_ marshmallow.fields.Tuple(_tuple\_fields_, _\*args_, _\*\*kwargs_)

Поле кортежа, состоящее из фиксированного числа других классов или экземпляров поля.

Пример:

```python
row = Tuple((fields.String(), fields.Integer(), fields.Float()))
```

{% hint style="info" %}
Из-за структурированного характера [collections.namedtuple](https://python.readthedocs.io/en/latest/library/collections.html#collections.namedtuple) и [typing.NamedTuple](https://python.readthedocs.io/en/latest/library/typing.html#typing.NamedTuple) использование для них Schema внутри поля **Nested** является более подходящим, чем использование поля **Tuple**.
{% endhint %}

#### Параметры:

* **tuple\_fields** (Iterable\[Field]) - Итерируемые классы полей или экземпляров.
* **kwargs** - Те же аргументы ключевого слова, которые получает [Field](polya-fields-a-f.md#field).

_Новое в версии 3.0.0rc4_.

| Методы                                       | Описание                                               |
| -------------------------------------------- | ------------------------------------------------------ |
| **\_bind\_to\_schema**(field\_name, schema)  | Обновить поле со значениями из его родительской схемы. |
| \_deserialize(value, attr, data, \*\*kwargs) | Десериализовать значение.                              |
| \_serialize(value, attr, obj, \*\*kwargs)    | Сериализует **value** в базовый тип данных Python.     |

| Атрибут                      | Описание                           |
| ---------------------------- | ---------------------------------- |
| **default\_error\_messages** | Сообщения об ошибках по умолчанию. |

### \_bind\_to\_schema(_field\_name_, _schema_)

Обновляет поле со значениями из его родительской схемы. Вызывается `Schema._bind_field`.

#### Параметры:

* **field\_name** (str) - Имя поля задано в схеме.
* **schema** (Schema | Field) - Родительский объект.

### \_deserialize(_value_, _attr_, _data_, _\*\*kwargs_) → list\[Any]

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

### \_serialize(_value_, _attr_, _obj_, _\*\*kwargs_) → list\[Any] | None

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

### default\_error\_messages _= {'invalid': 'Not a valid tuple.'}_

Сообщения об ошибках по умолчанию.

## URL

#### marshmallow.fields.URL

псевдоним marshmallow.fields.Url

## UUID

#### _class_ marshmallow.fields.UUID(_\*, load\_default: typing.Any = \<marshmallow.missing>, missing: typing.Any = \<marshmallow.missing>, dump\_default: typing.Any = \<marshmallow.missing>, default: typing.Any = \<marshmallow.missing>, data\_key: str | None = None, attribute: str | None = None, validate: None | (typing.Callable\[\[typing.Any], typing.Any] | typing.Iterable\[typing.Callable\[\[typing.Any], typing.Any]]) = None, required: bool = False, allow\_none: bool | None = None, load\_only: bool = False, dump\_only: bool = False, error\_messages: dict\[str, str] | None = None, metadata: typing.Mapping\[str, typing.Any] | None = None, \*\*additional\_metadata_)

Поле UUID.

| Методы                                       | Описание                                                                                                                                                                                            |
| -------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| \_deserialize(value, attr, data, \*\*kwargs) | Десериализовать значение.                                                                                                                                                                           |
| \_validated(value)                           | Форматирует значение или вызывает [ValidationError](isklyucheniya.md#exception-marshmallow.exceptions.validationerror-message-str-or-list-or-dict-field\_name-str-\_schema), если возникнет ошибка. |

| Атрибут                      | Описание                           |
| ---------------------------- | ---------------------------------- |
| **default\_error\_messages** | Сообщения об ошибках по умолчанию. |

### \_deserialize(_value_, _attr_, _data_, _\*\*kwargs_) → uuid.UUID | None

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



### \_validated(_value_) → uuid.UUID | None

Форматирует значение или вызывает [ValidationError](isklyucheniya.md#exception-marshmallow.exceptions.validationerror-message-str-or-list-or-dict-field\_name-str-\_schema), если возникнет ошибка.

### default\_error\_messages _= {'invalid\_uuid': 'Not a valid UUID.'}_

Сообщения об ошибках по умолчанию.

## Url

#### _class_ marshmallow.fields.Url(_\*_, _relative: bool = False_, _schemes: types.StrSequenceOrSet | None = None_, _require\_tld: bool = True_, _\*\*kwargs_)

Поле URL.

#### Параметры:

* **default** - Значение по умолчанию для поля, если атрибут не установлен.
* **relative** - Разрешить ли относительные URL-адреса.
* **require\_tld** - Следует ли отклонять имена хостов, отличные от FQDN.
* **schemes** - Действующие схемы. По умолчанию разрешены **http**, **https**, **ftp** и **ftps**.
* **kwargs** - Те же аргументы ключевого слова, которые получает [String](polya-fields-g-z.md#string).

| Атрибут                      | Описание                           |
| ---------------------------- | ---------------------------------- |
| **default\_error\_messages** | Сообщения об ошибках по умолчанию. |

### default\_error\_messages _= {'invalid': 'Not a valid URL.'}_

Сообщения об ошибках по умолчанию.
