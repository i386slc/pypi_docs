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
