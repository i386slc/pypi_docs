# marshmallow-dataclass

Автоматическое создание схем [marshmallow](https://marshmallow.readthedocs.io/) из классов данных.

```python
from dataclasses import dataclass, field
from typing import List, Optional
import marshmallow_dataclass
import marshmallow.validate

@dataclass
class Building:
    # метаданные поля используются для создания экземпляра поля marshmallow
    height: float = field(metadata={"validate": marshmallow.validate.Range(min=0)})
    name: str = field(default="anonymous")

@dataclass
class City:
    name: Optional[str]
    buildings: List[Building] = field(default_factory=list)

city_schema = marshmallow_dataclass.class_schema(City)()

city = city_schema.load(
    {"name": "Paris", "buildings": [{"name": "Eiffel Tower", "height": 324}]}
)
# => City(name='Paris', buildings=[Building(height=324.0, name='Eiffel Tower')])

city_dict = city_schema.dump(city)
# => {'name': 'Paris', 'buildings': [{'name': 'Eiffel Tower', 'height': 324.0}]}
```

## Почему

Использование схем в Python часто означает наличие как класса для представления ваших данных, так и класса для представления его схемы, что приводит к дублированию кода, который может не синхронизироваться. Начиная с Python 3.6, типы могут быть определены для членов класса, что позволяет библиотекам автоматически генерировать схемы.

Таким образом, вы можете документировать свои API таким образом, чтобы вы могли статически проверять соответствие кода документации.

## Установка

Этот пакет [размещен на PyPI](https://pypi.org/project/marshmallow-dataclass/).

```bash
pip3 install marshmallow-dataclass
```

Вы можете дополнительно установить следующие дополнения:

* **enum** : поддержка enum для версий marshmallow `<3.18` [marshmallow-enum](https://github.com/justanr/marshmallow\_enum).
* **union** : для перевода [типов Union](https://docs.python.org/3/library/typing.html#typing.Union) python в поля union.

```bash
pip3 install "marshmallow-dataclass[enum,union]"
```

### Поддержка marshmallow 2

**marshmallow-dataclass** больше не поддерживает **marshmallow 2**. Установите `marshmallow_dataclass<6.0`, если вам нужна совместимость с **marshmallow 2**.

## Применение

Используйте функцию [class\_schema](https://lovasoa.github.io/marshmallow\_dataclass/html/marshmallow\_dataclass.html#marshmallow\_dataclass.class\_schema) для создания класса marshmallow [Schema](https://marshmallow.readthedocs.io/en/latest/api\_reference.html#marshmallow.Schema) из класса данных [dataclass](https://docs.python.org/3/library/dataclasses.html#dataclasses.dataclass).

```python
from dataclasses import dataclass
from datetime import date
import marshmallow_dataclass

@dataclass
class Person:
    name: str
    birth: date

PersonSchema = marshmallow_dataclass.class_schema(Person)
```

Тип ваших полей должен быть базовым [типом, поддерживаемым marshmallow](https://marshmallow.readthedocs.io/en/stable/api\_reference.html#marshmallow.Schema.TYPE\_MAPPING) (таким как **float**, **str**, **bytes**, **datetime**,...), **Union** или другими **dataclass**.

### Принуждение к объединению (де)сериализации

Обычно тип **Union**; `Union[X, Y]` означает — с точки зрения теории множеств — либо **X**, либо **Y**, т. е. неупорядоченный набор, однако порядок подтипов определяет приоритет при попытке эфирной десериализации или сериализации значения согласно [здесь](https://github.com/lovasoa/marshmallow\_dataclass/blob/master/marshmallow\_dataclass/union\_field.py).

Например,

```python
from typing import Union
from dataclasses import dataclass

@dataclass
class Person:
    name: str
    age: Union[int, float]

PersonSchema = marshmallow_dataclass.class_schema(Person)
PersonSchema().load({"name": "jane", "age": 50.0})
# => Person(name="jane", age=50)
```

сначала (успешно) попытается привести `50.0` к **int**. Если принудительное принуждение нежелательно, можно использовать тип **Any** с оговоркой, что значения не будут проверяться без дополнительной [проверки](https://marshmallow.readthedocs.io/en/stable/marshmallow.validate.html).

### Настройка сгенерированных полей

Чтобы передать аргументы сгенерированным полям marshmallow (например, **validate**, **load\_only**, **dump\_only** и т. д.), передайте их в аргумент **metadata** функции **field**.

Обратите внимание, что, начиная с версии 4, marshmallow запрещает передачу произвольных аргументов, поэтому любые дополнительные метаданные должны быть помещены в отдельный словарь **metadata**:

```python
from dataclasses import dataclass, field
import marshmallow_dataclass
import marshmallow.validate

@dataclass
class Person:
    name: str = field(
        metadata=dict(
            load_only=True, metadata=dict(description="The person's first name")
        )
    )
    height: float = field(metadata=dict(validate=marshmallow.validate.Range(min=0)))

PersonSchema = marshmallow_dataclass.class_schema(Person)
```

### Сокращение @dataclass

**marshmallow\_dataclass** предоставляет декоратор **@dataclass**, который ведет себя как стандартная библиотека **@dataclasses.dataclass** и добавляет атрибут **Schema** с созданной marshmallow [Schema](https://marshmallow.readthedocs.io/en/2.x-line/api\_reference.html#marshmallow.Schema).

```python
# Используйте ярлык @dataclass marshmallow_dataclass
from marshmallow_dataclass import dataclass

@dataclass
class Point:
    x: float
    y: float

Point.Schema().dump(Point(4, 2))
# => {'x': 4, 'y': 2}
```

Примечание. Поскольку свойство **.Schema** добавляется динамически, оно может запутать средства проверки типов. Чтобы избежать этого, вы можете объявить **Schema** как [ClassVar](https://docs.python.org/3/library/typing.html#typing.ClassVar).

```python
from typing import ClassVar, Type
from marshmallow_dataclass import dataclass
from marshmallow import Schema

@dataclass
class Point:
    x: float
    y: float
    Schema: ClassVar[Type[Schema]] = Schema
```

### Настройка базовой схемы

Также возможно получить все схемы из вашего собственного базового класса **Schema** (см. [документацию marshmallow о расширении схемы](https://marshmallow.readthedocs.io/en/stable/extending.html)). Это позволяет реализовать настраиваемое поведение (де)сериализации, например, указать настраиваемое сопоставление между вашими классами и полями **marshmallow** или переименовать поля при сериализации.

### Пользовательское сопоставление между классами и полями

```python
class BaseSchema(marshmallow.Schema):
    TYPE_MAPPING = {CustomType: CustomField, List: CustomListField}

class Sample:
    my_custom: CustomType
    my_custom_list: List[int]

SampleSchema = marshmallow_dataclass.class_schema(Sample, base_schema=BaseSchema)
# SampleSchema теперь сериализует my_custom, используя поле marshmallow CustomField,
# и сериализует my_custom_list, используя поле marshmallow CustomListField.
```

### Переименование полей при сериализации

```python
import marshmallow
import marshmallow_dataclass

class UppercaseSchema(marshmallow.Schema):
    """Схема, которая упорядочивает данные с ключами в верхнем регистре."""

    def on_bind_field(self, field_name, field_obj):
        field_obj.data_key = (field_obj.data_key or field_name).upper()

class Sample:
    my_text: str
    my_int: int

SampleSchema = marshmallow_dataclass.class_schema(Sample, base_schema=UppercaseSchema)
SampleSchema().dump(Sample(my_text="warm words", my_int=1))
# -> {"MY_TEXT": "warm words", "MY_INT": 1}
```

Вы также можете передать **base\_schema** в **marshmallow\_dataclass.dataclass**.

```python
@marshmallow_dataclass.dataclass(base_schema=UppercaseSchema)
class Sample:
    my_text: str
    my_int: int
```

См. [документацию marshmallow о расширении Schema](https://marshmallow.readthedocs.io/en/stable/extending.html).

### Пользовательские объявления NewType

Эта библиотека экспортирует функцию **NewType** для создания типов, которые генерируют [настраиваемые поля marshmallow](https://marshmallow.readthedocs.io/en/stable/custom\_fields.html#creating-a-field-class).

Аргументы ключевого слова для **NewType** передаются конструктору поля marshmallow.

```python
import marshmallow.validate
from marshmallow_dataclass import NewType

IPv4 = NewType(
    "IPv4", str, validate=marshmallow.validate.Regexp(r"^([0-9]{1,3}\\.){3}[0-9]{1,3}$")
)
```

Вы также можете передать поле marshmallow в **NewType**.

```python
import marshmallow
from marshmallow_dataclass import NewType

Email = NewType("Email", str, field=marshmallow.fields.Email)
```

Для удобства предусмотрено несколько пользовательских типов:

```python
from marshmallow_dataclass.typing import Email, Url
```

Примечание: если вы используете **mypy**, вы заметите, что **mypy** выдает ошибку, если переменная, определенная с помощью **NewType**, используется в аннотации типа. Чтобы решить эту проблему, добавьте плагин `marshmallow_dataclass.mypy` в конфигурацию **mypy**, например:

```ini
[mypy]
plugins = marshmallow_dataclass.mypy
# ...
```

### Параметры Meta

[Параметры Meta](https://marshmallow.readthedocs.io/en/stable/api\_reference.html#marshmallow.Schema.Meta) устанавливаются так же, как и **Schema** marshmallow.

```python
from marshmallow_dataclass import dataclass

@dataclass
class Point:
    x: float
    y: float

    class Meta:
        ordered = True
```

## Документация

Документация проекта размещена на страницах GitHub: [https://lovasoa.github.io/marshmallow\_dataclass/](https://lovasoa.github.io/marshmallow\_dataclass/)

## Содействие

Чтобы установить этот проект и внести в него локальные изменения, следуйте инструкциям на сайте [CONTRIBUTING.md](https://pypi.org/project/marshmallow-dataclass/CONTRIBUTING.md).
