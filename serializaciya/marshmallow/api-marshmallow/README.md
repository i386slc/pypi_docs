# API marshmallow

* [Схема Schema](skhema-schema.md)
* [Поля Fields](polya-fields.md)
* [Декораторы](dekoratory.md)
* [Валидаторы](validatory.md)
* [Вспомогательные функции](vspomogatelnye-funkcii.md)
* [Хранилище ошибок](khranilishe-oshibok.md)
* [Класс Registry](klass-registry.md)
* [Исключения](isklyucheniya.md)

## Классы

| Название                                                                                | Описание                                                                         |
| --------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------- |
| ****[**Schema**](./#class-marshmallow.schema)(\*\[, only, exclude, many, context, ...]) | Базовый класс схемы, с помощью которого можно определить пользовательские схемы. |
| ****[**SchemaOpts**](./#class-marshmallow.schemaopts)(meta\[, ordered])                 | Параметры класса Meta для Schema.                                                |

## Исключения

| Название                                                                                                | Описание                                    |
| ------------------------------------------------------------------------------------------------------- | ------------------------------------------- |
| ****[**ValidationError**](./#exception-marshmallow.validationerror)(message\[, field\_name, data, ...]) | Возникает при сбое проверки поля или схемы. |

## Функции

| Название                                                                              | Описание                                                                                   |
| ------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------ |
| ****[**post\_dump**](./#marshmallow.post\_dump)(\[fn, pass\_many, pass\_original])    | Регистрирует метод для вызова после сериализации объекта.                                  |
| ****[**post\_load**](./#marshmallow.post\_load)(\[fn, pass\_many, pass\_original])    | Регистрирует метод для вызова после десериализации объекта.                                |
| ****[**pprint**](./#marshmallow.pprint)(obj, \*args, \*\*kwargs)                      | Функция красивой печати, которая может красиво печатать OrderedDicts, как обычные словари. |
| ****[**pre\_dump**](./#marshmallow.pre\_dump)(\[fn, pass\_many])                      | Регистрирует метод для вызова перед сериализацией объекта.                                 |
| ****[**pre\_load**](./#marshmallow.pre\_load)(\[fn, pass\_many])                      | Регистрирует метод для вызова перед десериализацией объекта.                               |
| ****[**validates**](./#marshmallow.validates)(field\_name)                            | Регистрирует валидатор полей.                                                              |
| ****[**validates\_schema**](./#marshmallow.validates\_schema)(\[fn, pass\_many, ...]) | Регистрирует средство проверки на уровне схемы.                                            |

## class marshmallow.Schema

#### _class_ marshmallow.Schema(_\*_, _only: types.StrSequenceOrSet | None = None_, _exclude: types.StrSequenceOrSet = ()_, _many: bool = False_, _context: dict | None = None_, _load\_only: types.StrSequenceOrSet = ()_, _dump\_only: types.StrSequenceOrSet = ()_, _partial: bool | types.StrSequenceOrSet = False_, _unknown: str | None = None_)

Базовый класс схемы, с помощью которого можно определить пользовательские схемы.

Пример использования:

```python
import datetime as dt
from dataclasses import dataclass

from marshmallow import Schema, fields

@dataclass
class Album:
    title: str
    release_date: dt.date

class AlbumSchema(Schema):
    title = fields.Str()
    release_date = fields.Date()

album = Album("Beggars Banquet", dt.date(1968, 12, 6))
schema = AlbumSchema()
data = schema.dump(album)
data  # {'release_date': '1968-12-06', 'title': 'Beggars Banquet'}
```

#### Параметры:

* **only** - Белый список объявленных полей для выбора при создании экземпляра схемы. Если нет, используются все поля. Вложенные поля могут быть представлены точечными разделителями.
* **exclude** - Черный список объявленных полей для исключения при создании экземпляра схемы. Если поле появляется как в режиме **only**, так и в режиме **exclude**, оно не используется. Вложенные поля могут быть представлены точечными разделителями.
* **many** - Должно быть установлено значение `True`, если **obj** является коллекцией, чтобы объект был сериализован в список.
* **context** - Необязательный контекст передается полям <mark style="color:red;">fields.Method</mark> и <mark style="color:red;">fields.Function</mark>.
* **load\_only** - Поля, которые нужно пропустить во время сериализации (поля только для записи)
* **dump\_only** - Поля, которые нужно пропустить при десериализации (поля только для чтения)
* **partial** - Следует ли игнорировать отсутствующие поля и не требовать объявления каких-либо полей. Также распространяется на вложенные (**Nested**) поля. Если его значение является итерируемым, будут игнорироваться только отсутствующие поля, перечисленные в этом итерируемом. Используйте точечные разделители для указания вложенных полей.
* **unknown** - Следует ли исключить, включить или вызвать ошибку для неизвестных полей в данных. Используйте **EXCLUDE**, **INCLUDE** или **RAISE**.

_Изменено в версии 3.0.0_: удален параметр **prefix**.

_Изменено в версии 2.0.0_: **\_\_validators\_\_**, **\_\_preprocessors\_\_** и **\_\_data\_handlers\_\_** удалены в пользу **marshmallow.decorators.validates\_schema**, **marshmallow.decorators.pre\_load** и **marshmallow.decorators.post\_dump.** Атрибуты **\_\_accessor\_\_** и **\_\_error\_handler\_\_** устарели. Вместо этого реализуйте методы **handle\_error** и **get\_attribute**.

### Классы Schema

| Название                       | Описание                                    |
| ------------------------------ | ------------------------------------------- |
| [Meta()](./#klass-meta)        | Объект параметров для схемы.                |
| [OPTIONS\_CLASS](./#undefined) | Псевдоним для marshmallow.schema.SchemaOpts |

### Атрибуты Schema

| Название        | Описание                                                              |
| --------------- | --------------------------------------------------------------------- |
| TYPE\_MAPPING   |                                                                       |
| dict\_class     |                                                                       |
| error\_messages | Переопределения для сообщений об ошибках на уровне схемы по умолчанию |
| fields          | Сопоставление словаря **field\_names** -> объекты поля **Fields**     |
| opts            |                                                                       |
| set\_class      |                                                                       |

### Методы Schema

| Название                                             | Описание                                                                                       |
| ---------------------------------------------------- | ---------------------------------------------------------------------------------------------- |
| **dump**(obj, \*\[, many])                           | Сериализует объект в собственные типы данных Python в соответствии с полями этой схемы.        |
| **dumps**(obj, \*args\[, many])                      | То же, что и dump(), за исключением того, что возвращает строку в формате JSON.                |
| **from\_dict**(fields, \*\[, name])                  | Создает класс Schema с учетом словаря полей.                                                   |
| **get\_attribute**(obj, attr, default)               | Определяет, как извлекать значения из объекта для сериализации.                                |
| **handle\_error**(error, data, \*, many, \*\*kwargs) | Пользовательская функция обработчика ошибок для схемы.                                         |
| **load**(data, \*\[, many, partial, unknown])        | Десериализует структуру данных в объект, определенный полями этой схемы.                       |
| **loads**(json\_data, \*\[, many, partial, unknown]) | То же, что и load(), за исключением того, что в качестве входных данных принимает строку JSON. |
| **on\_bind\_field**(field\_name, field\_obj)         | Перехватчик для изменения поля, когда оно привязано к схеме.                                   |
| **validate**(data, \*\[, many, partial])             | Проверяет данные по схеме, возвращая словарь ошибок проверки.                                  |

### Класс Meta

Объект параметров для схемы.

Пример использования:

```python
class Meta:
    fields = ("id", "email", "date_created")
    exclude = ("password", "secret_attribute")
```

Доступные опции:

* **fields** - Кортеж или список полей для включения в сериализованный результат.
* **additional** - Кортеж или список полей, которые необходимо включить в _**дополнение**_ к явно объявленным полям. **additional** и **fields** являются взаимоисключающими параметрами.
* **include** - Словарь дополнительных полей для включения в схему. Обычно лучше определять поля как переменные класса, но вам может понадобиться использовать эту опцию, например, если ваши поля являются ключевыми словами Python. Может быть OrderedDict.
* **exclude** - Кортеж или список полей, которые следует исключить из сериализованного результата. Вложенные поля могут быть представлены точечными разделителями.
* **dateformat** - Формат по умолчанию для полей <mark style="color:red;">Date</mark>.
* **datetimeformat** - Формат по умолчанию для полей <mark style="color:red;">DateTime</mark>
* **timeformat** - Формат по умолчанию для полей <mark style="color:red;">Time</mark>
* **render\_module** - Модуль для <mark style="color:red;">loads</mark> и <mark style="color:red;">dumps</mark>. По умолчанию используется [json](https://python.readthedocs.io/en/latest/library/json.html#module-json) из стандартной библиотеки.
* **ordered** - Если `True`, упорядочить выходные данные сериализации в соответствии с порядком, в котором были объявлены поля. Вывод <mark style="color:red;">Schema.dump</mark> будет [collections.OrderedDict](https://python.readthedocs.io/en/latest/library/collections.html#collections.OrderedDict).
* **index\_errors** - Если значение равно `True`, словари ошибок будут включать в себя индекс недопустимых элементов в коллекции.
* **load\_only** - Кортеж или список полей, которые необходимо исключить из сериализованных результатов.
* **dump\_only** - Кортеж или список полей для исключения из десериализации
* **unknown** - Следует ли исключить, включить или вызвать ошибку для неизвестных полей в данных. Используйте **EXCLUDE**, **INCLUDE** или **RAISE**.
* **register** - Регистрировать ли схему во внутреннем реестре классов **marshmallow**. Должно быть `True`, если вы собираетесь обращаться к этой схеме по имени класса во вложенных полях. Установите значение `False` только в том случае, если использование памяти критично. По умолчанию `True`.

### OPTIONS\_CLASS

Псевдоним для <mark style="color:red;">marshmallow.schema.SchemaOpts</mark>.

### TYPE\_MAPPING

#### TYPE\_MAPPING_: Dict\[type, Type\[marshmallow.fields.Field]] = {\<class 'str'>: \<class 'marshmallow.fields.String'>, \<class 'bytes'>: \<class 'marshmallow.fields.String'>, \<class 'datetime.datetime'>: \<class 'marshmallow.fields.DateTime'>, \<class 'float'>: \<class 'marshmallow.fields.Float'>, \<class 'bool'>: \<class 'marshmallow.fields.Boolean'>, \<class 'tuple'>: \<class 'marshmallow.fields.Raw'>, \<class 'list'>: \<class 'marshmallow.fields.Raw'>, \<class 'set'>: \<class 'marshmallow.fields.Raw'>, \<class 'int'>: \<class 'marshmallow.fields.Integer'>, \<class 'uuid.UUID'>: \<class 'marshmallow.fields.UUID'>, \<class 'datetime.time'>: \<class 'marshmallow.fields.Time'>, \<class 'datetime.date'>: \<class 'marshmallow.fields.Date'>, \<class 'datetime.timedelta'>: \<class 'marshmallow.fields.TimeDelta'>, \<class 'decimal.Decimal'>: \<class 'marshmallow.fields.Decimal'>}_

### _property_ dict\_class: type

### dump()

#### dump(_obj: Any_, _\*_, _many: bool | None = None_)

Сериализует объект в собственные типы данных Python в соответствии с полями этой схемы.

#### Параметры:

* **obj** - Объект для сериализации.
* **many** - Следует ли сериализовать **obj** как коллекцию. Если `None`, используется значение для `self.many`.

#### Возвращает:

* Сериализованные данные

_Новое в версии 1.0.0_.

_Изменено в версии 3.0.0b7_: этот метод возвращает сериализованные данные, а не дубликат `(data, errors)`. Ошибка [ValidationError](isklyucheniya.md#exception-marshmallow.exceptions.validationerror-message-str-or-list-or-dict-field\_name-str-\_schema) возникает, если **obj** недействителен.

_Изменено в версии 3.0.0rc9_: проверка больше не происходит при сериализации.

### dump\_fields()

#### dump\_fields_: Dict\[str, marshmallow.fields.Field]_

### dumps()

#### dumps(_obj: Any_, _\*args_, _many: bool | None = None_, _\*\*kwargs_)

То же, что и [dump()](./#dump), за исключением того, что возвращает строку в формате **JSON**.

#### Параметры:

* **obj** - Объект для сериализации.
* **many** - Следует ли сериализовать **obj** как коллекцию. Если `None`, используется значение для `self.many`.

#### Возвращает:

* Сериализованные данные

_Новое в версии 1.0.0_.

_Изменено в версии 3.0.0b7_: этот метод возвращает сериализованные данные, а не дубликат `(data, errors)`. Ошибка [ValidationError](isklyucheniya.md#exception-marshmallow.exceptions.validationerror-message-str-or-list-or-dict-field\_name-str-\_schema) возникает, если **obj** недействителен.

_Изменено в версии 3.0.0rc9_: проверка больше не происходит при сериализации.

### error\_messages

#### error\_messages_: Dict\[str, str] = {}_

Переопределения для сообщений об ошибках на уровне схемы по умолчанию

### _fields_

#### fields_: Dict\[str, marshmallow.fields.Field]_

Сопоставление словаря **field\_names** -> Объекты поля __ **Field**

### classmethod from\_dict()

#### _classmethod_ from\_dict(_fields: dict\[str, ma\_fields.Field | type]_, _\*_, _name: str = 'GeneratedSchema'_) → type

Создает класс [Schema](./#class-marshmallow.schema) с учетом словаря полей.

```python
from marshmallow import Schema, fields

PersonSchema = Schema.from_dict({"name": fields.Str()})
print(PersonSchema().load({"name": "David"}))  # => {'name': 'David'}
```

Сгенерированные схемы не добавляются в реестр классов, и поэтому на них нельзя ссылаться по имени во вложенных полях.

#### Параметры:

* **fields (dict)** - Словарь, сопоставляющий имена полей с экземплярами полей.
* **name (str)** - Необязательное имя класса, которое будет отображаться в представлении **repr** класса.

_Новое в версии 3.0.0_.

### get\_attribute()

#### get\_attribute(_obj: Any_, _attr: str_, _default: Any_)

Определяет, как извлекать значения из объекта для сериализации.

_Новое в версии 2.0.0_.

_Изменено в версии 3.0.0a1_: Изменено положение **obj** и **attr**.

### handle\_error()

#### handle\_error(_error: marshmallow.exceptions.ValidationError_, _data: Any_, _\*_, _many: bool_, _\*\*kwargs_)

Пользовательская функция обработчика ошибок для схемы.

#### Параметры:

* **error** - Ошибка проверки [ValidationError](isklyucheniya.md#exception-marshmallow.exceptions.validationerror-message-str-or-list-or-dict-field\_name-str-\_schema), возникшая во время (де)сериализации.
* **data** - Исходные входные данные
* **many** - Значение **many** при дампе или загрузке.
* **partial** - Значение **partial** при загрузке.

_Новое в версии 2.0.0_.

_Изменено в версии 3.0.0rc9_: получает **many** и **partial** (при десериализации) аргументы ключевого слова.

### load()

#### load(_data: Mapping\[str, Any] | Iterable\[Mapping\[str, Any]]_, _\*_, _many: bool | None = None_, _partial: bool | types.StrSequenceOrSet | None = None_, _unknown: str | None = None_)

Десериализует структуру данных в объект, определенный полями этой схемы.

#### Параметры:

* **data** - Данные для десериализации.
* **many** - Следует ли десериализовать данные как коллекцию. Если `None`, используется значение для `self.many`.
* **partial** - Следует ли игнорировать отсутствующие поля и не требовать объявления каких-либо полей. Также распространяется на вложенные поля. Если его значение является итерируемым, будут игнорироваться только отсутствующие поля, перечисленные в этом итерируемом. Используйте точечные разделители для указания вложенных полей.
* **unknown** - Следует ли исключить, включить или вызвать ошибку для неизвестных полей в данных. Используйте **EXCLUDE**, **INCLUDE** или **RAISE**. Если нет, используется значение для `self.unknown`.

#### Возвращает:

* Десериализованные данные

_Новое в версии 1.0.0_.

_Изменено в версии 3.0.0b7_: этот метод возвращает сериализованные данные, а не дубликат `(data, errors)`. Ошибка [ValidationError](isklyucheniya.md#exception-marshmallow.exceptions.validationerror-message-str-or-list-or-dict-field\_name-str-\_schema) возникает, если **obj** недействителен.

### load\_fields

#### load\_fields_: Dict\[str, marshmallow.fields.Field]_

### loads()

#### loads(_json\_data: str_, _\*_, _many: bool | None = None_, _partial: bool | types.StrSequenceOrSet | None = None_, _unknown: str | None = None_, _\*\*kwargs_)

То же, что и [load()](./#load), за исключением того, что в качестве входных данных принимает строку **JSON**.

#### Параметры:

* **json\_data** - Строка JSON данных для десериализации.
* **many** - Следует ли десериализовать данные как коллекцию. Если `None`, используется значение для `self.many`.
* **partial** - Следует ли игнорировать отсутствующие поля и не требовать объявления каких-либо полей. Также распространяется на вложенные поля. Если его значение является итерируемым, будут игнорироваться только отсутствующие поля, перечисленные в этом итерируемом. Используйте точечные разделители для указания вложенных полей.
* **unknown** - Следует ли исключить, включить или вызвать ошибку для неизвестных полей в данных. Используйте **EXCLUDE**, **INCLUDE** или **RAISE**. Если нет, используется значение для `self.unknown`.

#### Возвращает:

* Десериализованные данные

_Новое в версии 1.0.0_.

_Изменено в версии 3.0.0b7_: этот метод возвращает сериализованные данные, а не дубликат `(data, errors)`. Ошибка [ValidationError](isklyucheniya.md#exception-marshmallow.exceptions.validationerror-message-str-or-list-or-dict-field\_name-str-\_schema) возникает, если **obj** недействителен.

### on\_bind\_field()

#### on\_bind\_field(_field\_name: str_, _field\_obj: marshmallow.fields.Field_) → None

Перехватчик для изменения поля, когда оно привязано к схеме.

Нет операции по умолчанию.

### opts

#### opts_: marshmallow.schema.SchemaOpts = \<marshmallow.schema.SchemaOpts object>_

### _property_ set\_class_: type_

### validate()

#### validate(_data: Mapping\[str, Any] | Iterable\[Mapping\[str, Any]]_, _\*_, _many: bool | None = None_, _partial: bool | types.StrSequenceOrSet | None = None_) → dict\[str, list\[str]]

Проверяет данные по схеме, возвращая словарь ошибок проверки.

#### Параметры:

* **data** - Данные для проверки.
* **many** - Проверять ли данные как коллекцию. Если `None`, используется значение для `self.many`.
* **partial** - Следует ли игнорировать отсутствующие поля и не требовать объявления каких-либо полей. Также распространяется на вложенные поля. Если его значение является итерируемым, будут игнорироваться только отсутствующие поля, перечисленные в этом итерируемом. Используйте точечные разделители для указания вложенных полей.

#### Возвращает:

* Словарь ошибок проверки.

_Новое в версии 1.1.0_.

## class marshmallow.SchemaOpts

#### _class_ marshmallow.SchemaOpts(_meta_, _ordered: bool = False_)

Параметры класса Meta для схемы [Schema](./#class-marshmallow.schema). Определяет значения по умолчанию.

## _exception_ marshmallow.ValidationError

#### _exception_ marshmallow.ValidationError(_message: str | list | dict_, _field\_name: str = '\_schema'_, _data: Mapping\[str, Any] | Iterable\[Mapping\[str, Any]] | None = None_, _valid\_data: list\[dict\[str, Any]] | dict\[str, Any] | None = None_, _\*\*kwargs_)

Возникает при сбое проверки поля или схемы.

Валидаторы и настраиваемые поля должны вызывать это исключение.

#### Параметры:

**message** - Сообщение об ошибке, список сообщений об ошибках или словарь сообщений об ошибках. Если это словарь, ключи являются субэлементами, а значения — сообщениями об ошибках.

**field\_name** - Имя поля для сохранения ошибки. Если `None`, ошибка сохраняется как ошибка уровня схемы.

**data** - Необработанные входные данные.

**valid\_data** - Действительные (де)сериализованные данные.

## normalized\_messages()

## marshmallow.post\_dump()

#### marshmallow.post\_dump(_fn: Callable\[..., Any] | None = None_, _pass\_many: bool = False_, _pass\_original: bool = False_) → Callable\[..., Any]

Регистрирует метод для вызова после сериализации объекта. Метод получает сериализованный объект и возвращает обработанный объект.

По умолчанию он получает один объект за раз, прозрачно обрабатывая аргумент **many**, переданный вызову схемы [dump()](./#dump). Если `pass_many=True`, передаются необработанные данные (которые могут быть коллекцией).

Если `pass_original=True`, исходные данные (до сериализации) будут переданы в метод в качестве дополнительного аргумента.

_Изменено в версии 3.0.0_: **many** всегда передаются в качестве аргументов ключевого слова декорированному методу.

## marshmallow.post\_load()

#### marshmallow.post\_load(_fn: Callable\[..., Any] | None = None_, _pass\_many: bool = False_, _pass\_original: bool = False_) → Callable\[..., Any]

Регистрирует метод для вызова после десериализации объекта. Метод получает десериализованные данные и возвращает обработанные данные.

По умолчанию он получает один объект за раз, прозрачно обрабатывая аргумент **many**, переданный в вызов [load()](./#load) схемы. Если `pass_many=True`, передаются необработанные данные (которые могут быть коллекцией).

Если `pass_original=True`, исходные данные (до десериализации) будут переданы в метод в качестве дополнительного аргумента.

_Изменено в версии 3.0.0_: **partial** и **many** всегда передаются в качестве аргументов ключевого слова в декорированный метод.

## marshmallow.pprint()

#### marshmallow.pprint(_obj_, _\*args_, _\*\*kwargs_) → None

Функция красивой печати, которая может красиво печатать **OrderedDicts**, как обычные словари. Полезно для печати вывода [marshmallow.Schema.dump()](./#dump).

_<mark style="color:orange;">Устарело, начиная с версии 3.7.0</mark>_: **marshmallow.pprint** будет удален в **marshmallow 4**.

## marshmallow.pre\_dump()

#### marshmallow.pre\_dump(_fn: Callable\[..., Any] | None = None_, _pass\_many: bool = False_) → Callable\[..., Any]

Регистрирует метод для вызова перед сериализацией объекта. Метод получает объект для сериализации и возвращает обработанный объект.

По умолчанию он получает один объект за раз, прозрачно обрабатывая аргумент **many**, переданный вызову схемы [dump()](./#dump). Если `pass_many=True`, передаются необработанные данные (которые могут быть коллекцией).

_Изменено в версии 3.0.0_: **many** всегда передаются в качестве аргументов ключевого слова декорированному методу.

## marshmallow.pre\_load()

#### marshmallow.pre\_load(_fn: Callable\[..., Any] | None = None_, _pass\_many: bool = False_) → Callable\[..., Any]

Регистрирует метод для вызова перед десериализацией объекта. Метод получает данные для десериализации и возвращает обработанные данные.

По умолчанию он получает один объект за раз, прозрачно обрабатывая аргумент **many**, переданный в вызов [load()](./#load) схемы. Если `pass_many=True`, передаются необработанные данные (которые могут быть коллекцией).

_Изменено в версии 3.0.0_: **partial** и **many** всегда передаются в качестве аргументов ключевого слова в декорированный метод.

## marshmallow.validates()

#### marshmallow.validates(_field\_name: str_) → Callable\[\[...], Any]

Регистрирует валидатор полей.

#### Параметры:

* **field\_name** (str) - Имя поля, которое проверяет метод.

## marshmallow.validates\_schema()

#### marshmallow.validates\_schema(_fn: Callable\[..., Any] | None = None_, _pass\_many: bool = False_, _pass\_original: bool = False_, _skip\_on\_field\_errors: bool = True_) → Callable\[..., Any]

Регистрирует средство проверки на уровне схемы.

По умолчанию он получает один объект за раз, прозрачно обрабатывая аргумент **many**, переданный в вызов Schema [validate()](./#validate). Если `pass_many=True`, передаются необработанные данные (которые могут быть коллекцией).

Если `pass_original=True`, исходные данные (до демаршалинга) будут переданы в метод в качестве дополнительного аргумента.

Если `skip_on_field_errors=True`, этот метод проверки будет пропущен всякий раз, когда при проверке полей будут обнаружены ошибки проверки.

_Изменено в версии 3.0.0b1_: **skip\_on\_field\_errors** по умолчанию имеет значение `True`.

_Изменено в версии 3.0.0_: **partial** и **many** всегда передаются в качестве аргументов ключевого слова в декорированный метод.

## marshmallow.EXCLUDE

## marshmallow.INCLUDE

## marshmallow.RAISE

## marshmallow.missing
