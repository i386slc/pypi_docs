# dataclass()

### dataclass()

#### marshmallow\_dataclass.dataclass(_\_cls: Type\[\_U]_, _\*_, _repr: bool = True_, _eq: bool = True_, _order: bool = False_, _unsafe\_hash: bool = False_, _frozen: bool = False_, _base\_schema: Type\[Schema] | None = None_, _cls\_frame: frame | None = None_) → Type\[\_U]

#### marshmallow\_dataclass.dataclass(_\*_, _repr: bool = True_, _eq: bool = True_, _order: bool = False_, _unsafe\_hash: bool = False_, _frozen: bool = False_, _base\_schema: Type\[Schema] | None = None_, _cls\_frame: frame | None = None_) → Callable\[\[Type\[\_U]], Type\[\_U]]

[Исходник](https://lovasoa.github.io/marshmallow\_dataclass/html/\_modules/marshmallow\_dataclass.html#dataclass).

Этот декоратор делает то же самое, что и **dataclasses.dataclass**, но также применяет [add\_schema()](dataclass.md#add\_schema). Он добавляет атрибут **.Schema** к объекту класса.

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
