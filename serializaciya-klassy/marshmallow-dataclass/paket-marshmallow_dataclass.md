# Пакет marshmallow\_dataclass

## Таблица содержания

<table><thead><tr><th width="427"></th><th></th></tr></thead><tbody><tr><td><a href="dataclass.md">marshmallow_dataclass.dataclass()</a></td><td>Этот декоратор делает то же самое, что и dataclasses.dataclass, но также применяет <strong>add_schema()</strong>.</td></tr><tr><td><a href="class_schema.md">marshmallow_dataclass.class_schema(clazz[, ...])</a></td><td>Преобразование класса в схему marshmallow</td></tr></tbody></table>

## Модуль

Эта библиотека позволяет преобразовывать **dataclasses** Python 3.7 в схемы **marshmallow**.

Он берет класс Python и генерирует для него схему marshmallow.

Простой пример:

```python
from marshmallow import Schema
from marshmallow_dataclass import dataclass

@dataclass
class Point:
  x:float
  y:float

point = Point(x=0, y=0)
point_json = Point.Schema().dumps(point)
```

Полный пример:

```python
from marshmallow import Schema
from dataclasses import field
from marshmallow_dataclass import dataclass
import datetime

@dataclass
class User:
  birth: datetime.date = field(metadata= {
    # Параметр для передачи в поле marshmallow
    "required": True
  })
  website: str = field(metadata = {
    # Пользовательское поле marshmallow
    "marshmallow_field": marshmallow.fields.Url()
  })
  # Для проверки типов
  Schema: ClassVar[Type[Schema]] = Schema
```

### NewType

#### marshmallow\_dataclass.NewType(_name: str_, _typ: Type\[\_U]_, _field: Type\[Field] | None = None_, _\*\*kwargs_) → Callable\[\[\_U], \_U]

[Исходник](https://lovasoa.github.io/marshmallow\_dataclass/html/\_modules/marshmallow\_dataclass.html#NewType).

**NewType** создает простые уникальные типы, к которым можно прикрепить настраиваемые атрибуты marshmallow. Все аргументы ключевого слова, переданные этой функции, будут переданы в конструктор поля marshmallow.

```python
>>> import marshmallow.validate
>>> IPv4 = NewType('IPv4', str, validate=marshmallow.validate.Regexp(r'^([0-9]{1,3}\.){3}[0-9]{1,3}$'))
>>> @dataclass
... class MyIps:
...     ips: List[IPv4]
>>> MyIps.Schema().load({"ips": ["0.0.0.0", "grumble grumble"]})
Traceback (most recent call last):
...
marshmallow.exceptions.ValidationError: {'ips': {1: ['String does not match expected pattern.']}}

>>> MyIps.Schema().load({"ips": ["127.0.0.1"]})
MyIps(ips=['127.0.0.1'])
```

<pre class="language-python"><code class="lang-python"><strong>>>> Email = NewType('Email', str, field=marshmallow.fields.Email)
</strong>>>> @dataclass
... class ContactInfo:
...     mail: Email = dataclasses.field(default="anonymous@example.org")

>>> ContactInfo.Schema().load({})
ContactInfo(mail='anonymous@example.org')

>>> ContactInfo.Schema().load({"mail": "grumble grumble"})
Traceback (most recent call last):
...
marshmallow.exceptions.ValidationError: {'mail': ['Not a valid email address.']}
</code></pre>

### add\_schema()

#### marshmallow\_dataclass.add\_schema(_\_cls: Type\[\_U]_) → Type\[\_U]

#### marshmallow\_dataclass.add\_schema(_base\_schema: Type\[Schema] | None = None_) → Callable\[\[Type\[\_U]], Type\[\_U]]

#### marshmallow\_dataclass.add\_schema(_\_cls: Type\[\_U]_, _base\_schema: Type\[Schema] | None = None_, _cls\_frame: frame | None = None_) → Type\[\_U]

[Исходник](https://lovasoa.github.io/marshmallow\_dataclass/html/\_modules/marshmallow\_dataclass.html#add\_schema).

Этот декоратор добавляет схему marshmallow в качестве атрибута `'Schema'` в классе данных. Он использует [class\_schema()](class\_schema.md) внутри.

#### Параметры:

* **\_cls (type)** - Класс данных, к которому следует добавить схему
* **base\_schema** - схема marshmallow, используемая в качестве базового класса при получении схемы класса данных
* **cls\_frame** - кадр определения cls

```python
>>> class BaseSchema(marshmallow.Schema):
...    def on_bind_field(self, field_name, field_obj):
...        field_obj.data_key = (field_obj.data_key or field_name).upper()
```

<pre class="language-python"><code class="lang-python"><strong>>>> @add_schema(base_schema=BaseSchema)
</strong>... @dataclasses.dataclass
... class Artist:
...     names: Tuple[str, str]
>>> artist = Artist.Schema().loads('{"NAMES": ["Martin", "Ramirez"]}')
>>> artist
Artist(names=('Martin', 'Ramirez'))
</code></pre>

### class\_schema()

#### marshmallow\_dataclass.class\_schema(_clazz: type_, _base\_schema: Type\[Schema] | None = None_, _clazz\_frame: frame | None = None_) → Type\[Schema]

[Исходник](https://lovasoa.github.io/marshmallow\_dataclass/html/\_modules/marshmallow\_dataclass.html#class\_schema).

Преобразование класса в схему marshmallow.

#### Параметры:

* **clazz** - Класс Python (может быть dataclass)
* **base\_schema** - схема marshmallow, используемая в качестве базового класса при получении схемы класса данных
* **clazz\_frame** - кадр определения cls

**Возвращает**: схема marshmallow, соответствующая классу данных.

{% hint style="info" %}
Все аргументы, поддерживаемые классами полей marshmallow, могут быть переданы в словарь **metadata** поля.
{% endhint %}

Если вы хотите использовать собственное поле marshmallow (то, которое не имеет эквивалентного типа Python), вы можете передать его как ключ **marshmallow\_field** в словаре **metadata**.

```python
>>> import typing
>>> Meters = typing.NewType('Meters', float)
>>> @dataclasses.dataclass()
... class Building:
...   height: Optional[Meters]
...   name: str = dataclasses.field(default="anonymous")
...   class Meta:
...     ordered = True
...
# Возвращает класс схемы marshmallow (не экземпляр)
>>> class_schema(Building)
<class 'marshmallow.schema.Building'>

>>> @dataclasses.dataclass()
... class City:
...   name: str = dataclasses.field(metadata={'required':True})
# Ссылка на другой класс данных. Для него тоже будет создана схема.
...   best_building: Building
...   other_buildings: List[Building] = dataclasses.field(default_factory=lambda: [])
...
>>> citySchema = class_schema(City)()
>>> city = citySchema.load({"name":"Paris", "best_building": {"name": "Eiffel Tower"}})
>>> city
City(
    name='Paris',
    best_building=Building(height=None, name='Eiffel Tower'),
    other_buildings=[]
)
```

```python
>>> citySchema.load({"name":"Paris"})
Traceback (most recent call last):
    ...
marshmallow.exceptions.ValidationError: {'best_building': ['Missing data for required field.']}
```

```python
>>> city_json = citySchema.dump(city)

# Мы получаем OrderedDict, потому что мы указали order = True в классе Meta
>>> city_json['best_building']
OrderedDict([('height', None), ('name', 'Eiffel Tower')])
```

```python
>>> @dataclasses.dataclass()
... class Person:
...   name: str = dataclasses.field(default="Anonymous")
# Рекурсивное поле
...   friends: List['Person'] = dataclasses.field(default_factory=lambda:[])
...
>>> person = class_schema(Person)().load({
...     "friends": [{"name": "Roger Boucher"}]
... })

>>> person
Person(name='Anonymous', friends=[Person(name='Roger Boucher', friends=[])])
```

<pre class="language-python"><code class="lang-python"><strong>>>> @dataclasses.dataclass()
</strong>... class C:
...     important: int = dataclasses.field(init=True, default=0)
...     # Будут добавлены только те поля, которые находятся в методе __init__:
...     unimportant: int = dataclasses.field(init=False, default=0)
...
>>> c = class_schema(C)().load({
...     "important": 9, # Это поле будет импортировано
...     "unimportant": 9 # Это поле НЕ будет импортировано
... }, unknown=marshmallow.EXCLUDE)

>>> c
C(important=9, unimportant=0)
</code></pre>

```python
>>> @dataclasses.dataclass
... class Website:
...    url: str = dataclasses.field(metadata = {
...      "marshmallow_field": marshmallow.fields.Url() # Пользовательское поле marshmallow
...    })
...

>>> class_schema(Website)().load({"url": "I am not a good URL !"})
Traceback (most recent call last):
    ...
marshmallow.exceptions.ValidationError: {'url': ['Not a valid URL.']}
```

```python
>>> @dataclasses.dataclass
... class NeverValid:
...     @marshmallow.validates_schema
...     def validate(self, data, **_):
...         raise marshmallow.ValidationError('never valid')
...

>>> class_schema(NeverValid)().load({})
Traceback (most recent call last):
    ...
marshmallow.exceptions.ValidationError: {'_schema': ['never valid']}
```

```python
>>> @dataclasses.dataclass
... class Anything:
...     name: str
...     @marshmallow.validates('name')
...     def validates(self, value):
...         if len(value) > 5: raise marshmallow.ValidationError("Name too long")

>>> class_schema(Anything)().load({"name": "aaaaaargh"})
Traceback (most recent call last):
...
marshmallow.exceptions.ValidationError: {'name': ['Name too long']}
```

Вы можете использовать аргумент **metadata**, чтобы переопределить поведение поля по умолчанию, например. тот факт, что поля **Optional** допускают значения **None**:

```python
>>> @dataclasses.dataclass
... class Custom:
...     name: Optional[str] = dataclasses.field(metadata={"allow_none": False})

>>> class_schema(Custom)().load({"name": None})
Traceback (most recent call last):
    ...
marshmallow.exceptions.ValidationError: {'name': ['Field may not be null.']}

>>> class_schema(Custom)().load({})
Custom(name=None)
```

### dataclass()

#### marshmallow\_dataclass.dataclass(_\_cls: Type\[\_U]_, _\*_, _repr: bool = True_, _eq: bool = True_, _order: bool = False_, _unsafe\_hash: bool = False_, _frozen: bool = False_, _base\_schema: Type\[Schema] | None = None_, _cls\_frame: frame | None = None_) → Type\[\_U]

#### marshmallow\_dataclass.dataclass(_\*_, _repr: bool = True_, _eq: bool = True_, _order: bool = False_, _unsafe\_hash: bool = False_, _frozen: bool = False_, _base\_schema: Type\[Schema] | None = None_, _cls\_frame: frame | None = None_) → Callable\[\[Type\[\_U]], Type\[\_U]]

[Исходник](https://lovasoa.github.io/marshmallow\_dataclass/html/\_modules/marshmallow\_dataclass.html#dataclass).

Этот декоратор делает то же самое, что и **dataclasses.dataclass**, но также применяет [add\_schema()](paket-marshmallow\_dataclass.md#add\_schema). Он добавляет атрибут **.Schema** к объекту класса.

#### Параметры:

* **base\_schema** - схема marshmallow, используемая в качестве базового класса при получении схемы класса данных
* **cls\_frame** - Фрейм определения cls, используемый для получения локальных переменных с определениями других классов. Если передано значение **None**, кадр вызывающего объекта будет рассматриваться как **cls\_frame**.

```python
>>> @dataclass
... class Artist:
...     name: str

>>>Artist.Schema
<class 'marshmallow.schema.Artist'>
```

```python
>>> from typing import ClassVar
>>> from marshmallow import Schema
>>> @dataclass(order=True) # сохранить порядок полей
... class Point:
...  x: float
...  y: float
...
...  Schema: ClassVar[Type[Schema]] = Schema # Для проверки типов
...
 # Эта строка может быть статически проверена на тип
>>> Point.Schema().load({'x':0, 'y':0})
Point(x=0.0, y=0.0)
```

### field\_for\_schema()

#### marshmallow\_dataclass.field\_for\_schema(_typ: type_, _default=\<marshmallow.missing>_, _metadata: \~typing.Mapping\[str_, _\~typing.Any] | None = None_, _base\_schema: \~typing.Type\[\~marshmallow.schema.Schema] | None = None_, _typ\_frame: frame | None = None_) → Field

[Исходник](https://lovasoa.github.io/marshmallow\_dataclass/html/\_modules/marshmallow\_dataclass.html#field\_for\_schema).

Получает поле marshmallow, соответствующее данному типу python. Метаданные поля класса данных используются в качестве аргументов поля marshmallow.

#### Параметры:

* **typ** - Тип, для которого должно быть сгенерировано поле
* **default** - значение, используемое для (де)сериализации, когда поле отсутствует
* **metadata** - Дополнительные параметры для передачи в конструктор поля marshmallow
* **base\_schema** - схема marshmallow, используемая в качестве базового класса при получении схемы класса данных
* **typ\_frame** - кадр определения типа

```python
>>> int_field = field_for_schema(int, default=9, metadata=dict(required=True))
>>> int_field.__class__
<class 'marshmallow.fields.Integer'>
```

```python
>>> int_field.dump_default
9
```

```python
>>> field_for_schema(
...     str,
...     metadata={"marshmallow_field": marshmallow.fields.Url()}
... ).__class__
<class 'marshmallow.fields.Url'>
```
