# class\_schema()

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
