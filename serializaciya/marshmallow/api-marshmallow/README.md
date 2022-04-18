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

| Название                                             | Описание                                                                         |
| ---------------------------------------------------- | -------------------------------------------------------------------------------- |
| **Schema**(\*\[, only, exclude, many, context, ...]) | Базовый класс схемы, с помощью которого можно определить пользовательские схемы. |
| **SchemaOpts**(meta\[, ordered])                     | Параметры класса Meta для Schema.                                                |

## Исключения

| Название                                                | Описание                                    |
| ------------------------------------------------------- | ------------------------------------------- |
| **ValidationError**(message\[, field\_name, data, ...]) | Возникает при сбое проверки поля или схемы. |

## Функции

| Название                                          | Описание                                                                                   |
| ------------------------------------------------- | ------------------------------------------------------------------------------------------ |
| **post\_dump**(\[fn, pass\_many, pass\_original]) | Регистрирует метод для вызова после сериализации объекта.                                  |
| **post\_load**(\[fn, pass\_many, pass\_original]) | Регистрирует метод для вызова после десериализации объекта.                                |
| **pprint**(obj, \*args, \*\*kwargs)               | Функция красивой печати, которая может красиво печатать OrderedDicts, как обычные словари. |
| **pre\_dump**(\[fn, pass\_many])                  | Регистрирует метод для вызова перед сериализацией объекта.                                 |
| **pre\_load**(\[fn, pass\_many])                  | Регистрирует метод для вызова перед десериализацией объекта.                               |
| **validates**(field\_name)                        | Регистрирует валидатор полей.                                                              |
| **validates\_schema**(\[fn, pass\_many, ...])     | Регистрирует средство проверки на уровне схемы.                                            |

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

| Название       | Описание                                    |
| -------------- | ------------------------------------------- |
| Meta()         | Объект параметров для схемы.                |
| OPTIONS\_CLASS | Псевдоним для marshmallow.schema.SchemaOpts |

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
* **render\_module** -&#x20;
* **ordered** -&#x20;
* **index\_errors** -&#x20;
* **load\_only** -&#x20;
* **dump\_only** -&#x20;
* **unknown** -&#x20;
* **register** -&#x20;

### dump(_obj: Any_, _\*_, _many: bool | None = None_)

### dumps(_obj: Any_, _\*args_, _many: bool | None = None_, _\*\*kwargs_)

### error\_messages_: Dict\[str, str] = {}_

### get\_attribute(_obj: Any_, _attr: str_, _default: Any_)

### handle\_error(_error: marshmallow.exceptions.ValidationError_, _data: Any_, _\*_, _many: bool_, _\*\*kwargs_)

### load(_data: Mapping\[str, Any] | Iterable\[Mapping\[str, Any]]_, _\*_, _many: bool | None = None_, _partial: bool | types.StrSequenceOrSet | None = None_, _unknown: str | None = None_)

### loads(_json\_data: str_, _\*_, _many: bool | None = None_, _partial: bool | types.StrSequenceOrSet | None = None_, _unknown: str | None = None_, _\*\*kwargs_)

### validate(_data: Mapping\[str, Any] | Iterable\[Mapping\[str, Any]]_, _\*_, _many: bool | None = None_, _partial: bool | types.StrSequenceOrSet | None = None_) → dict\[str, list\[str]]

## _class_ marshmallow.SchemaOpts(_meta_, _ordered: bool = False_)
