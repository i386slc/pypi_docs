# Расширенное использование

Вы можете настроить фабрику во время ее создания. Вы не можете изменить настройки позже, потому что они влияют на парсеры, которые создаются только один раз для каждого экземпляра фабрики.

Большая часть конфигурации выполняется через **Schemas**. Вы можете установить схему по умолчанию или по одной для каждого типа:

```python
factory = Factory(default_schema=Schema(...), schemas={ClassA: Schema(...)})
```

## Более подробные ошибки

В настоящее время ошибки не очень подробные. Но вы можете сделать их немного лучше, используя **debug\_path** фабрики. По умолчанию он отключен, так как влияет на производительность.

В этом режиме **InvalidFieldError** выдается, когда какое-то поле класса данных не может быть проанализировано. Он содержит **field\_path**, который представляет собой путь к полю в предоставленных данных (ключ и индексы).

## Работа с именами полей

### Сопоставление имен

В некоторых случаях у вас есть json с ключами, которые оставляют желать лучшего. Например, они могут содержать пробелы или просто иметь неясное значение. Самый простой способ исправить это — установить пользовательское сопоставление имен. Вы можете называть поля по своему усмотрению, и фабрика преобразует их, используя ваше сопоставление.

```python
from dataclasses import dataclass
import dataclass_factory
from dataclass_factory import Schema

@dataclass
class Book:
    title: str
    price: int

data = {
    "title": "Fahrenheit 451",
    "book price": 100,
}

book_schema = Schema(name_mapping={
    "price": "book price"
})
factory = dataclass_factory.Factory(schemas={Book: book_schema})
book: Book = factory.load(data, Book)
serialized = factory.dump(book)
```

Поля, отсутствующие в отображении, не переводятся и используются с их оригинальными именами (как в спецификации класса данных).

### Удаление подчеркивания

Часто нет необходимости заполнять сопоставление имен. Одним из наиболее распространенных случаев являются словарные ключи, которые являются ключевыми словами Python. Например, вы не можете использовать строку **from** в качестве имени поля, но с большой вероятностью увидите ее в API. Обычно это решается добавлением нижнего подчеркивания в конце (например, **from\_**).

Фабрика классов данных обрезает символы подчеркивания в конце, чтобы вы не столкнулись с этим случаем.

```python
from dataclasses import dataclass
import dataclass_factory
from dataclass_factory import Schema

@dataclass
class Period:
    from_: int
    to_: int

data = {
    "from": 1,
    "to": 100,
}

factory = dataclass_factory.Factory()
period = factory.load(data, Period)
```

Иногда такое поведение нежелательно, поэтому вы можете отключить эту функцию, установив `trim_trailing_underscore=False` в схеме (в схеме по умолчанию конкретной). Кроме того, вы можете повторно включить его для определенных типов.

```python
from dataclasses import dataclass
import dataclass_factory
from dataclass_factory import Schema

@dataclass
class Period:
    from_: int
    to_: int

data = {
    "from_": 1,
    "to_": 100,
}

factory = dataclass_factory.Factory(
    default_schema=Schema(trim_trailing_underscore=False)
)
period = factory.load(data, Period)
factory.dump(period)
```

### Стили имен

Иногда ключи json вполне нормальные, но некрасивые. Например, они названы с использованием **CamelCase**, но **PEP8** рекомендует использовать **snake\_case**. Конечно, можно подготовить сопоставление имен, но слишком много писать для такой глупости.

Библиотека может автоматически переводить такие имена. Вам необходимо объявить поля в соответствии с рекомендациями PEP8 (например, **field\_name**) и установить соответствующий **name\_style**. Как обычно, если для определенного типа не задан стиль, он будет взят из схемы по умолчанию.

Кстати, вы не можете преобразовать имена, которые не соответствуют стилю **snake\_case**. В этом случае единственным допустимым стилем является **ignore**.

```python
from dataclasses import dataclass
from dataclass_factory import Factory, Schema, NameStyle

factory = Factory(default_schema=Schema(
    name_style=NameStyle.camel
))

@dataclass
class Person:
    first_name: str
    last_name: str

person = Person("ivan", "petrov")

serial_person = {
    "FirstName": "ivan",
    "LastName": "petrov"
}
assert factory.dump(person) == serial_person
```

Поддерживаются следующие стили имени:

* `snake` (snake\_case)
* `kebab` (kebab-case)
* `camel_lower` (camelCaseLower)
* `camel` (CamelCase)
* `lower` (lowercase)
* `upper` (UPPERCASE)
* `upper_snake` (UPPER\_SNAKE\_CASE)
* `camel_snake` (Camel\_Snake)
* `dot` (dot.case)
* `camel_dot` (Camel.Dot)
* `upper_dot` (UPPER.DOT)
* `ignore` (не настоящий стиль; просто не конвертирует)

## Выбор и пропуск полей

У вас есть несколько способов пропустить обработку некоторых полей.

{% hint style="info" %}
Пропущенные поля НЕ ДОЛЖНЫ быть обязательными в конструкторе класса, иначе синтаксический анализ завершится ошибкой.
{% endhint %}

### only и exclude

Если вы точно знаете, какие поля должны быть проанализированы/сериализованы, и хотите игнорировать все остальные, просто установите их как параметр схемы **only**. Кроме того, вы можете предоставить список с исключенными именами через **exclude**.

Это влияет как на синтаксический анализ, так и на сериализацию.

```python
from dataclasses import dataclass
import dataclass_factory
from dataclass_factory import Schema

@dataclass
class Book:
    title: str
    price: int
    extra: str = ""

data = {
    "title": "Fahrenheit 451",
    "price": 100,
    "extra": "some extra string"
}

# используется `only`:
factory = dataclass_factory.Factory(schemas={Book: Schema(only=["title", "price"])})
# То же, что Book(title="Fahrenheit 451", price=100)
book: Book = factory.load(data, Book)
# никакой `extra` ключ не будет сериализован
serialized = factory.dump(book)

# используется `exclude`
factory = dataclass_factory.Factory(schemas={Book: Schema(exclude=["extra"])})
# То же, что Book(title="Fahrenheit 451", price=100)
book: Book = factory.load(data, Book)
# никакой `extra` ключ не будет сериализован
serialized = factory.dump(book)
```

### only\_mapped

У вас уже есть **name\_mapping** и вы не хотите повторять все имена в одном параметре **only**? Просто установите `only_mapped=True`. Он будет игнорировать все поля, которые не описаны в сопоставлении имен.

### skip\_internal

Более упрощенный случай — пропустить так называемые поля внутреннего использования (_internal use_), те поля, имя которых начинается с подчеркивания. Вы можете исключить их из синтаксического анализа и сериализации, используя параметр схемы **skip\_internal**.

По умолчанию он отключен. Это влияет как на синтаксический анализ, так и на сериализацию.

```python
from dataclasses import dataclass
import dataclass_factory
from dataclass_factory import Schema

@dataclass
class Book:
    title: str
    price: int
    _total: int = 0

data = {
    "title": "Fahrenheit 451",
    "price": 100,
    "_total": 1000,
}

factory = dataclass_factory.Factory(default_schema=Schema(skip_internal=True))
# То же, что Book(title="Fahrenheit 451", price=100)
book: Book = factory.load(data, Book)
# ключ `_total` создаваться не будет
serialized = factory.dump(book)
```

### omit\_default

Если у вас есть значения по умолчанию для некоторых полей, нет необходимости хранить их в сериализованном представлении. Например, это может быть **None**, пустой список или что-то другое. Вы можете опустить их при сериализации, используя опцию **omit\_default**. Те значения, которые равны значениям по умолчанию, будут удалены из результирующего словаря.

По умолчанию он отключен. Это влияет только на сериализацию.

```python
from typing import Optional, List
from dataclasses import dataclass, field
import dataclass_factory
from dataclass_factory import Schema

@dataclass
class Book:
    title: str
    price: Optional[int] = None
    authors: List[str] = field(default_factory=list)

data = {
    "title": "Fahrenheit 451",
}

factory = dataclass_factory.Factory(default_schema=Schema(omit_default=True))
book = Book(title="Fahrenheit 451", price=None, authors=[])
# ключ `price` и `authors` создаваться не будет
serialized = factory.dump(book)
assert data == serialized
```

## Выравнивание структуры

Еще один пример уродливого API — слишком сложная иерархия данных. Вы можете исправить это, используя уже известное **name\_mapping**. Раньше вы использовали его для переименования полей, но вы также можете использовать его для сопоставления имени с вложенным значением, указав путь к нему.

Целые числа в пути рассматриваются как индексы списка, строки — как ключи словаря. Это влияет на синтаксический анализ и сериализацию.

Например, у вас есть автор книги с единственным полем - **name** (см. [Вложенные объекты](bystryi-start-dataclass-factory.md#vlozhennye-obekty)). Вы можете расширить этот словарь и сохранить имя автора непосредственно в классе **Book**.

```python
from dataclasses import dataclass
import dataclass_factory
from dataclass_factory import Schema

@dataclass
class Book:
    title: str
    price: int
    author: str

data = {
    "title": "Fahrenheit 451",
    "price": 100,
    "author": {
        "name": "Ray Bradbury"
    }
}

book_schema = Schema(
    name_mapping={
        "author": ("author", "name")
    }
)
factory = dataclass_factory.Factory(schemas={Book: book_schema})

# Book(title="Fahrenheit 451", price=100, author="Ray Bradbury")
book: Book = factory.load(data, Book)
serialized = factory.dump(book)
assert serialized == data
```

Мы можем изменить приведенный выше пример, чтобы сохранить автора в виде списка с именем

```python
from dataclasses import dataclass
import dataclass_factory
from dataclass_factory import Schema

@dataclass
class Book:
    title: str
    price: int
    author: str

data = {
    "title": "Fahrenheit 451",
    "price": 100,
    "author": ["Ray Bradbury"]
}

book_schema = Schema(
    name_mapping={
        "author": ("author", 0)
    }
)
factory = dataclass_factory.Factory(schemas={Book: book_schema})

# Book(title="Fahrenheit 451", price=100, author="Ray Bradbury")
book: Book = factory.load(data, Book)
serialized = factory.dump(book)
assert serialized == data
```

### Автоматическое именование во время выравнивания

Если имена где-то в «сложной» структуре такие же, как и в вашем классе, вы можете упростить свою схему, используя многоточие (`...`). Есть два простых правила:

* `...` так как ключ в **name\_mapping** означает любое поле. Путь будет применяться к каждому полю, которое не объявлено явно при сопоставлении.
* `...` внутренний путь в **name\_mapping** означает, что исходное имя поля будет использоваться повторно. Если указан стиль имени или другие правила, они будут применены к имени.

Примеры:

```python
from dataclasses import dataclass
import dataclass_factory
from dataclass_factory import Schema

@dataclass
class Book:
    title: str
    price: int
    author: str

data = {
    "book": {
        "title": "Fahrenheit 451",
        "price": 100,
    },
    "author": {
        "name": "Ray Bradbury"
    }
}

book_schema = Schema(
    name_mapping={
        "author": (..., "name"),
        ...: ("book", ...)
    }
)
factory = dataclass_factory.Factory(schemas={Book: book_schema})

# Book(title="Fahrenheit 451", price=100, author="Ray Bradbury")
book: Book = factory.load(data, Book)
serialized = factory.dump(book)
assert serialized == data
```

## Парсинг неизвестных полей

По умолчанию все лишние поля, отсутствующие в целевой структуре, игнорируются. Но такое поведение не обязательно. На данный момент вы можете выбрать один из нескольких вариантов установки атрибута **unknown** схемы.

* **Unknown.SKIP** — поведение по умолчанию. Все неизвестные поля игнорируются (пропускаются)
* **Unknown.FORBID** — ошибка **UnknownFieldsError** возникает в случае обнаружения любого неизвестного поля.
* **Unknown.STORE** — все неизвестные поля передаются конструктору класса без разбора. Ваш `__init__` должен быть готов к этому
* Имя поля (**str**). Указанное поле заполняется всеми неизвестными и вызывается парсер соответствующего типа. В простых случаях вы можете аннотировать это поле типом **Dict**. В случае сериализации это поле также сериализуется, и результат объединяется с текущим результатом.
* Несколько имен полей (последовательность **str**). Поведение очень похоже на случай с одним именем поля. Все неизвестные собираются в один словарь и передаются парсерам каждого предоставленного поля (будьте осторожны при изменении данных на этапе **pre\_parse**). Кроме того, их результаты дампа объединяются при сериализации.

```python
from typing import Optional, Dict
from dataclasses import dataclass
from dataclass_factory import Factory, Schema

@dataclass
class Sub:
    b: str

@dataclass
class Data:
    a: str
    unknown: Optional[Dict] = None
    sub: Optional[Sub] = None

serialized = {
    "a": "A1",
    "b": "B2",
    "c": "C3",
}

factory = Factory(default_schema=Schema(unknown=["unknown", "sub"]))
data = factory.load(serialized, Data)
assert data == Data(a="A1", unknown={"b": "B2", "c": "C3"}, sub=Sub("B2"))
```

## Дополнительные шаги

Большая часть работы выполняется автоматически, но вы можете захотеть выполнить некоторую дополнительную обработку.

Реальный процесс синтаксического анализа имеет следующий поток:

```
╔══════======╗      ┌───────────┐      ┌────────┐      ┌────────────┐      ╔════════╗
║ data: dict ║ ---> │ pre_parse │ ---> │ parser │ ---> │ post_parse │ ---> ║ result ║
╚══════======╝      └───────────┘      └────────┘      └────────────┘      ╚════════╝
```

То же самое для сериализации:

```
╔══════=====╗    ┌───────────────┐    ┌────────────┐    ┌────────────────┐    ╔════════╗
║ data: obj ║--->│ pre_serialize │--->│ serializer │--->│ post_serialize │--->║ result ║
╚══════=====╝    └───────────────┘    └────────────┘    └────────────────┘    ╚════════╝
```

Таким образом, возвращаемое значение **pre\_parse** передается синтаксическому анализатору **parser**, а возвращаемое значение **post\_parse** используется как общий результат процесса синтаксического анализа. Вы можете добавить свою логику на любом этапе, но обратите внимание на главное отличие:

* **pre\_parse** и **post\_serialize** работают с сериализованным представлением данных (например, **dict** для dataclasses)
* **post\_parse** и **pre\_serialize** работают с экземплярами ваших классов.

Так что если вы хотите сделать какую-то валидацию - лучше сделать это на шаге **post\_parse**. И если вы хотите сделать _полиморфный парсинг_ — проверьте, подходит ли тип, прежде чем парсинг будет запущен в **pre\_parse**.

Другой случай — изменить представление некоторых полей: сериализовать json в строку, разделить значения и так далее.

```python
import json
from typing import List
from dataclasses import dataclass
from dataclass_factory import Schema, Factory

@dataclass
class Data:
    items: List[str]
    name: str

def post_serialize(data):
    data["items"] = json.dumps(data["items"])
    return data

def pre_parse(data):
    data["items"] = json.loads(data["items"])
    return data

def post_parse(data: Data) -> Data:
    if not data.name:
        raise ValueError("Name must not be empty")
    return data

data_schema = Schema[Data](
    post_serialize=post_serialize,
    pre_parse=pre_parse,
    post_parse=post_parse,
)
factory = Factory(schemas={Data: data_schema})

data = Data(['a', 'b'], 'My Name')
serialized = {'items': '["a", "b"]', 'name': 'My Name'}
assert factory.dump(data) == serialized
assert factory.load(serialized, Data) == data

try:
    factory.load({'items': '[]', 'name': ''}, Data)
except ValueError as e:
    # Обнаружена ошибка: имя не должно быть пустым
    print("Error detected:", e)
```

## Наследование схемы

В некоторых случаях может быть полезно создать подкласс **Schema** вместо обычного создания экземпляров.

```python
from typing import Any
from dataclasses import dataclass
import dataclass_factory
from dataclass_factory import Schema

@dataclass
class Book:
    title: str
    price: int
    _author: str = "Unknown author"

data = {
    "title": "Fahrenheit 451",
    "price": 100,
}

class DataSchema(Schema[Any]):
    skip_internal = True

    def post_parse(self, data):
        print("parsing done")
        return data

factory = dataclass_factory.Factory(
    schemas={Book: DataSchema(trim_trailing_underscore=False)}
)
# То же, что Book(title="Fahrenheit 451", price=100)
book: Book = factory.load(data, Book)
serialized = factory.dump(book)
```

{% hint style="info" %}
В версиях `<2.9`: **Factory** создавала копию схемы для каждого типа, заполняющую пропущенные аргументы. Если вам нужно получить доступ к некоторым данным в схеме, получите рабочий экземпляр схемы с помощью метода **Factory.schema**.
{% endhint %}

{% hint style="info" %}
Один экземпляр схемы можно использовать несколько раз одновременно из-за многопоточности или рекурсивных структур. Будьте осторожны при изменении данных в схеме
{% endhint %}

## Json-схема

Вы можете создать схему json для своих классов.

Обратите внимание, что **factory** делает это лениво и кэширует результат. Итак, если вам нужны определения для всех ваших классов, создайте схему для каждого класса верхнего уровня, используя метод **json\_schema**, а затем соберите все определения, используя **json\_schema\_definitions**.

```python
import json
from enum import Enum
from typing import Dict, Union
from dataclasses import dataclass, field
from dataclass_factory import Factory, Schema

class A(Enum):
    X = "x"
    Y = 1

@dataclass
class Data:
    a: A
    dict_: Dict[str, Union[int, float]]
    dictw_: Dict[str, Union[int, float]] = field(default_factory=dict)
    optional_num: int = 0

factory = Factory(schemas={A: Schema(description="My super `A` class")})
print(json.dumps(factory.json_schema(Data), indent=2))
print(json.dumps(factory.json_schema_definitions(), indent=2))
```

Результат вызова **json\_schema**:

```json
{
  "title": "Data",
  "type": "object",
  "properties": {
    "a": {
      "$ref": "#/definitions/A"
    },
    "dict": {
      "$ref": "#/definitions/typing.Dict[str, typing.Union[int, float]]"
    },
    "dictw": {
      "$ref": "#/definitions/typing.Dict[str, typing.Union[int, float]]",
      "default": {}
    },
    "optional_num": {
      "type": "integer",
      "default": 0
    }
  },
  "additionalProperties": true,
  "required": [
    "a",
    "dict"
  ]
}
```

Результат вызова **json\_schema\_definitions**:

```json
{
  "definitions": {
    "A": {
      "title": "A",
      "description": "My super `A` class",
      "enum": [
        "x",
        1
      ]
    },
    "typing.Union[int, float]": {
      "title": "typing.Union[int, float]",
      "anyOf": [
        {
          "type": "integer"
        },
        {
          "type": "number"
        }
      ]
    },
    "typing.Dict[str, typing.Union[int, float]]": {
      "title": "typing.Dict[str, typing.Union[int, float]]",
      "type": "object",
      "additionalProperties": {
        "$ref": "#/definitions/typing.Union[int, float]"
      }
    },
    "Data": {
      "title": "Data",
      "type": "object",
      "properties": {
        "a": {
          "$ref": "#/definitions/A"
        },
        "dict": {
          "$ref": "#/definitions/typing.Dict[str, typing.Union[int, float]]"
        },
        "dictw": {
          "$ref": "#/definitions/typing.Dict[str, typing.Union[int, float]]",
          "default": {}
        },
        "optional_num": {
          "type": "integer",
          "default": 0
        }
      },
      "additionalProperties": true,
      "required": [
        "a",
        "dict"
      ]
    }
  }
}
```

{% hint style="info" %}
В настоящее время поддерживаются не все функции фабрики классов данных. Вы не можете сгенерировать json-схему, если используете выравнивание структуры, дополнительный анализ неизвестных полей или анализ на основе инициализации. Кроме того, если у вас есть собственные синтаксические анализаторы или этап предварительного синтаксического анализа, схема может быть неверной.
{% endhint %}
