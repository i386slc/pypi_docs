# Быстрый старт dataclass-factory

Фабрика классов данных анализирует подсказки типов и создает соответствующие синтаксические анализаторы на основе полученной информации. Для классов данных он проверяет, какие поля объявлены, а затем вызывает обычный конструктор. Для других типов поведение может отличаться.

Также вы можете настроить его, используя разные схемы (см. [Расширенное использование](rasshirennoe-ispolzovanie.md)).

## Установка

Просто используйте **pip** для установки библиотеки:

```bash
pip install dataclass_factory
```

## Простой случай

Все, что вам нужно сделать, чтобы начать синтаксический анализ ваших классов данных, — это создать экземпляр **Factory**. Затем вызовите методы **load** или **dump** с соответствующим типом, и все будет сделано автоматически.

```python
from dataclasses import dataclass
import dataclass_factory

@dataclass
class Book:
    title: str
    price: int
    author: str = "Unknown author"

data = {
    "title": "Fahrenheit 451",
    "price": 100,
}

factory = dataclass_factory.Factory()
# То же, что Book(title="Fahrenheit 451", price=100)
book: Book = factory.load(data, Book)
serialized = factory.dump(book)
```

Вся информация о вводе извлекается из ваших аннотаций, поэтому от вас не требуется предоставлять какую-либо схему или даже изменять декораторы классов данных или базы классов.

В приведенном примере `book.author == "Unknown author"`, потому что вызывается обычный конструктор класса данных.

Фабрику лучше создавать только один раз, потому что все парсеры кэшируются внутри нее после первого использования. В противном случае структура ваших классов будет анализироваться снова и снова для каждого нового экземпляра **Factory**.

## Вложенные объекты

Вложенные объекты поддерживаются из коробки. Удивительно, но вам не нужно ничего делать, кроме как определять свои классы данных. Например, вы ожидаете, что автор книги является экземпляром **Person**, но в сериализованной форме это словарь.

Объявите свои классы данных как обычно, а затем просто проанализируйте свои данные.

```python
from dataclasses import dataclass
import dataclass_factory

@dataclass
class Person:
    name: str

@dataclass
class Book:
    title: str
    price: int
    author: Person

data = {
    "title": "Fahrenheit 451",
    "price": 100,
    "author": {
        "name": "Ray Bradbury"
    }
}

factory = dataclass_factory.Factory()

# Book(title="Fahrenheit 451", price=100, author=Person("Ray Bradbury"))
book: Book = factory.load(data, Book)
serialized = factory.dump(book)
```

## Списки и другие коллекции

Хотите разобрать коллекцию классов данных? Никаких изменений не требуется, просто укажите правильный тип цели (например, `List[SomeClass]` или `Dict[str, SomeClass])`.

```python
from typing import List
from dataclasses import dataclass
import dataclass_factory

@dataclass
class Book:
    title: str
    price: int

data = [
    {
        "title": "Fahrenheit 451",
        "price": 100,
    },
    {
        "title": "1984",
        "price": 100,
    }
]

factory = dataclass_factory.Factory()
books = factory.load(data, List[Book])
serialized = factory.dump(books)
```

Поля также могут содержать любые поддерживаемые коллекции.

## Обработка ошибок

В настоящее время синтаксический анализатор не генерирует никаких конкретных исключений в случае сбоя синтаксического анализатора. Ошибки такие же, как и у соответствующих конструкторов. В обычных случаях все подходящие исключения описаны в `dataclass_factory.PARSER_EXCEPTIONS`.

```python
from dataclasses import dataclass

import dataclass_factory

@dataclass
class Book:
    title: str
    price: int
    author: str = "Unknown author"

data = {
    "title": "Fahrenheit 451"
}

factory = dataclass_factory.Factory()

try:
    book: Book = factory.load(data, Book)
except dataclass_factory.PARSER_EXCEPTIONS as e:
    # Cannot parse:  <class 'TypeError'> __init__() missing 1 required positional argument: 'price'
    print("Cannot parse: ", type(e), e)
```

## Валидация

Валидация данных может быть выполнена в двух случаях:

* проверки для каждого поля
* проверка всей конструкции

В первом случае вы можете использовать декоратор **@validate** с именем поля для проверки данных.

Вот подробности:

* валидатор может быть вызван до парсинга данных поля (установите `pre=True`) или после него.
* валидаторы полей применяются после всех преобразований имени (стили имени, выравнивание структуры). Поэтому используйте имя поля, как оно называется в вашем классе
* валидатор может применяться к нескольким полям. Просто укажите несколько имен
* валидатор можно применить к любому полю отдельно. Просто не устанавливайте имя поля
* валидатор должен возвращать данные, если проверки прошли успешно. Данные могут быть такими же, как переданные ему или что-то еще. Валидатор МОЖЕТ изменить данные
* валидаторы полей не могут быть установлены в схеме по умолчанию
* несколько декораторов могут использоваться в одном методе
* несколько валидаторов могут быть применены к одному полю
* используйте разные имена методов, чтобы предотвратить перезапись

```python
from dataclasses import dataclass
from dataclass_factory import validate, Factory, Schema, NameStyle

class MySchema(Schema):
    SOMETHING = "Some string"

    # использовать исходное имя поля в классе
    @validate("int_field")
    def validate_field(self, data):
        if data > 100:
            raise ValueError
        # валидатор может изменить значение
        return data * 100

    # этот валидатор будет вызываться перед парсингом поля
    @validate("complex_field", pre=True)
    def validate_field_pre(self, data):
        return data["value"]

    @validate("info")
    def validate_stub(self, data):
        # валидатор может получить доступ к полям схемы
        return self.SOMETHING

@dataclass
class My:
    int_field: int
    complex_field: int
    info: str

factory = Factory(schemas={
    # стиль имени не влияет на то, как валидаторы привязаны к полям
    My: MySchema(name_style=NameStyle.upper_snake)
})

result = factory.load(
    {"INT_FIELD": 1, "COMPLEX_FIELD": {"value": 42}, "INFO": "ignored"},
    My
)
assert result == My(100, 42, "Some string")
```

Если вы хотите проверить всю структуру, вы можете сделать это на этапе **pre\_parse** или **post\_parse**. Идея та же:

* **pre\_parse** вызывается перед разбором структуры (но даже до сведения данных и обработки имен).
* **post\_parse** вызывается после успешного синтаксического анализа
* в классе может быть только один метод **pre\_parse** и один метод **post\_parse**.

```python
from dataclasses import dataclass
import dataclass_factory
from dataclass_factory import Schema

@dataclass
class Book:
    title: str
    price: int
    author: str = "Unknown author"

data = {
    "title": "Fahrenheit 451",
    "price": 100,
}
invalid_data = {
    "title": "1984",
    "price": -100,
}

def validate_book(book: Book) -> Book:
    if not book.title:
        raise ValueError("Empty title")
    if book.price <= 0:
        raise ValueError("InvalidPrice")
    return book

factory = dataclass_factory.Factory(schemas={Book: Schema(post_parse=validate_book)})
book = factory.load(data, Book)  # Нет ошибок
other_book = factory.load(invalid_data, Book)  # ValueError: InvalidPrice
```
