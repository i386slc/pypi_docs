# Поведение особых типов

## Структуры

Из коробки фабрика классов данных поддерживает три типа структур:

* **dataclasses**
* **TypedDict** (**total** также проверяется)
* другие классы с аннотированным `__init__`

<table><thead><tr><th width="238">Особенности</th><th>dataclass</th><th>TypedDict</th><th>Класс с __init__</th></tr></thead><tbody><tr><td>Парсинг</td><td>X</td><td>X</td><td>X</td></tr><tr><td>Конверсия имен</td><td>X</td><td>X</td><td>X</td></tr><tr><td>Пропуск по умолчанию</td><td>X</td><td></td><td></td></tr><tr><td>Пропуск внутренних</td><td>X</td><td>X</td><td></td></tr><tr><td>Сериализация</td><td>X</td><td>X</td><td>X</td></tr></tbody></table>

## Пользовательские парсеры и сериализаторы

Не все типы поддерживаются из коробки. Например, непонятно, как парсить **datetime**: она может быть представлена в формате **unixtime** или **iso** или как-то еще. Если вы встретите неподдерживаемый тип, вы можете предоставить свой собственный парсер и сериализатор:

```python
from datetime import datetime, timezone
from dataclasses import dataclass
from dataclass_factory import Schema, Factory

@dataclass
class Author:
    name: str
    born_at: datetime

def parse_timestamp(data):
    print("parsing timestamp")
    return datetime.fromtimestamp(data, tz=timezone.utc)

unixtime_schema = Schema(
    parser=parse_timestamp,
    serializer=datetime.timestamp
)

factory = Factory(
    schemas={
        datetime: unixtime_schema,
    }
)
expected_author = Author("Petr", datetime(1970, 1, 2, 3, 4, 56, tzinfo=timezone.utc))
data = {'born_at': 97496, 'name': 'Petr'}
author = factory.load(data, Author)
print(author, expected_author)
assert author == expected_author

serialized = factory.dump(author)
assert data == serialized
```

## Общие схемы

У нас есть подпакет **dataclass\_factory.schema\_helpers** с некоторыми общими схемами:

* **unixtime\_schema** — конвертирует дату и время **datetime** в **unixtime** и наоборот
* **isotime\_schema** — преобразует дату и время **datetime** в строку, содержащую **ISO 8601**. Поддерживается только в Python 3.7+.
* **uuid\_schema** — преобразует объекты **UUID** в строку

## Самоссылающиеся типы

Просто поместите правильные аннотации и используйте **factory**.

```python
from typing import Any, Optional
from dataclasses import dataclass
import dataclass_factory

@dataclass
class LinkedItem:
    value: Any
    next: Optional["LinkedItem"] = None

data = {
    "value": 1,
    "next": {
        "value": 2
    }
}

factory = dataclass_factory.Factory()
items = factory.load(data, LinkedItem)
# items = LinkedItem(1, LinkedItem(2))
```

## Общие классы Generic

Общие классы **Generic** поддерживаются из коробки. Разница в том, что если для конкретного класса схема не найдена, она будет принята за универсальную (generic).

```python
from dataclasses import dataclass
from typing import TypeVar, Generic
from dataclass_factory import Factory, Schema

T = TypeVar("T")

@dataclass
class FakeFoo(Generic[T]):
    value: T

factory = Factory(schemas={
    FakeFoo[str]: Schema(name_mapping={"value": "s"}),
    FakeFoo: Schema(name_mapping={"value": "i"}),
})
data = {"i": 42, "s": "Hello"}
# найдена схема для конкретного типа
assert factory.load(data, FakeFoo[str]) == FakeFoo("Hello")
# схема взята из универсальной версии
assert factory.load(data, FakeFoo[int]) == FakeFoo(42)
# конкретный тип задается явно
assert factory.dump(FakeFoo("hello"), FakeFoo[str]) == {"s": "hello"}
# универсальный тип определяется автоматически
assert factory.dump(FakeFoo("hello")) == {"i": "hello"}
```

{% hint style="info" %}
Всегда передавайте конкретный тип в качестве второго аргумента метода **dump**. В противном случае он будет рассматриваться как универсальный из-за стирания типа.
{% endhint %}

## Полиморфный анализ

Очень распространенным случаем является выбор класса на основе информации в данных.

Если требуемые поля различаются между классами, настройка не требуется. Но иногда вы хотите сделать выбор более явно. Например, если поле данных `"type"` равно `"item"`, данные должны быть проанализированы как **Item**, если это `"group"`, тогда следует использовать класс **Group**.

В таком случае вы можете использовать **type\_checker** из модуля **schema\_helpers**. Он создает функцию, которую следует использовать на этапе **pre\_parse**. По умолчанию она проверяет поле типа данных **type**, но вы можете изменить его

```python
from typing import Union
from dataclasses import dataclass
from dataclass_factory import Factory, Schema
from dataclass_factory.schema_helpers import type_checker

@dataclass
class Item:
    name: str
    type: str = "item"

@dataclass
class Group:
    name: str
    type: str = "group"

# Доступные типы
Something = Union[Item, Group]

factory = Factory(schemas={
    Item: Schema(pre_parse=type_checker("item", field="type")),
    # `type` - имя по умолчанию для проверяемого поля
    Group: Schema(pre_parse=type_checker("group")),
})

assert factory.load(
    {"name": "some name", "type": "group"}, Something
) == Group("some name")
```

Если вам нужна собственная функция **pre\_parse**, вы можете установить ее в качестве параметра для фабрики **type\_checker**.

Для более сложных случаев вы можете написать свою функцию. Просто поднимите **ValueError**, если вы обнаружили, что текущий класс неприемлем для предоставленных данных, и синтаксический анализатор **parser** перейдет к следующему в **Union**.
