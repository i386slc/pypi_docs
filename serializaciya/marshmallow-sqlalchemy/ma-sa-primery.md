# (ma-sa) Примеры

## Базовая схема I

Общим шаблоном для **marshmallow** является определение базового класса схемы **Schema**, который имеет общую конфигурацию и поведение для схем вашего приложения.

Вы можете определить общий объект сеанса, например, **scoped\_session** для использования во всех схемах.

```python
# myproject/db.py
import sqlalchemy as sa
from sqlalchemy import orm

Session = orm.scoped_session(orm.sessionmaker())
Session.configure(bind=engine)
```

```python
# myproject/schemas.py

from marshmallow_sqlalchemy import SQLAlchemySchema

from .db import Session

class BaseSchema(SQLAlchemySchema):
    class Meta:
        sqla_session = Session
```

```python
# myproject/users/schemas.py

from ..schemas import BaseSchema
from .models import User

class UserSchema(BaseSchema):
    # Наследовать параметры BaseSchema
    class Meta(BaseSchema.Meta):
        model = User
```

## Базовая схема II

Вот альтернативный способ определения класса **BaseSchema** с общим объектом **Session**.

```python
# myproject/schemas.py

from marshmallow_sqlalchemy import SQLAlchemySchemaOpts, SQLAlchemySchema
from .db import Session

class BaseOpts(SQLAlchemySchemaOpts):
    def __init__(self, meta, ordered=False):
        if not hasattr(meta, "sqla_session"):
            meta.sqla_session = Session
        super(BaseOpts, self).__init__(meta, ordered=ordered)

class BaseSchema(SQLAlchemySchema):
    OPTIONS_CLASS = BaseOpts
```

Это позволяет вам определять параметры класса **Meta** без создания подкласса **BaseSchema.Meta**.

```python
# myproject/users/schemas.py

from ..schemas import BaseSchema
from .models import User

class UserSchema(BaseSchema):
    class Meta:
        model = User
```

## Анализ сгенерированных полей

Часто бывает полезно проанализировать, какие поля создаются для **SQLAlchemyAutoSchema**.

Сгенерированные поля добавляются в атрибут **\_declared\_fields** схемы.

```python
AuthorSchema._declared_fields["books"]
# <fields.RelatedList(default=<marshmallow.missing>, ...>
```

Вы также можете напрямую использовать функции преобразования **marshmallow\_sqlalchemy**.

```python
from marshmallow_sqlalchemy import property2field

id_prop = Author.__mapper__.get_property("id")

property2field(id_prop)
# <fields.Integer(default=<marshmallow.missing>, ...>
```

## Переопределение сгенерированных полей

Любое поле, созданное <mark style="color:red;">SQLAlchemyAutoSchema</mark>, может быть переопределено.

```python
from marshmallow import fields
from marshmallow_sqlalchemy import SQLAlchemyAutoSchema
from marshmallow_sqlalchemy.fields import Nested

class AuthorSchema(SQLAlchemyAutoSchema):
    class Meta:
        model = Author

    # Переопределить поле books, чтобы использовать вложенное представление,
    # а не pks
    books = Nested(BookSchema, many=True, exclude=("author",))
```

Вы можете использовать функцию <mark style="color:red;">auto\_field</mark> для создания поля marshmallow [Field](../marshmallow/api-marshmallow/polya-fields-a-f.md#field) на основе одного свойства модели. Это полезно для передачи дополнительных аргументов ключевого слова в сгенерированное поле.

```python
from marshmallow_sqlalchemy import SQLAlchemyAutoSchema, field_for

class AuthorSchema(SQLAlchemyAutoSchema):
    class Meta:
        model = Author

    # Сгенерируйте поле, передав дополнительный аргумент dump_only
    date_created = auto_field(dump_only=True)
```

Если внешний ключ данных поля отличается от имени столбца модели, вы можете передать имя столбца в <mark style="color:red;">auto\_field</mark>.

```python
class AuthorSchema(SQLAlchemyAutoSchema):
    class Meta:
        model = Author
        # Исключить date_created, потому что мы используем псевдоним ниже
        exclude = ("date_created",)

    # Сгенерировать поле «created_date» из столбца «date_created»
    created_date = auto_field("date_created", dump_only=True)
```

## Автоматическое создание схем для моделей SQLAlchemy

Может быть утомительно реализовывать большое количество схем, если не переопределять ни одно из сгенерированных полей, как описано выше. SQLAlchemy имеет хук, которую можно использовать для запуска создания схем, назначая их свойству модели SQLAlchemy `Model.__marshmallow__`.

```python
from marshmallow_sqlalchemy import ModelConversionError, SQLAlchemyAutoSchema

def setup_schema(Base, session):
    # Создайте функцию, которая включает информацию о базе и сеансе.
    def setup_schema_fn():
        for class_ in Base._decl_class_registry.values():
            if hasattr(class_, "__tablename__"):
                if class_.__name__.endswith("Schema"):
                    raise ModelConversionError(
                        "For safety, setup_schema can not be used when a"
                        "Model class ends with 'Schema'"
                    )

                class Meta(object):
                    model = class_
                    sqla_session = session

                schema_class_name = "%sSchema" % class_.__name__

                schema_class = type(
                    schema_class_name, (SQLAlchemyAutoSchema,), {"Meta": Meta}
                )

                setattr(class_, "__marshmallow__", schema_class)

    return setup_schema_fn
```

Пример использования этого:

```python
import sqlalchemy as sa
from sqlalchemy.ext.declarative import declarative_base
from sqlalchemy.orm import scoped_session, sessionmaker
from sqlalchemy import event
from sqlalchemy.orm import mapper

# Либо импортируйте, либо объявите setup_schema здесь

engine = sa.create_engine("sqlite:///:memory:")
session = scoped_session(sessionmaker(bind=engine))
Base = declarative_base()

class Author(Base):
    __tablename__ = "authors"
    id = sa.Column(sa.Integer, primary_key=True)
    name = sa.Column(sa.String)

    def __repr__(self):
        return "<Author(name={self.name!r})>".format(self=self)

# Прослушайте событие SQLAlchemy и запустите setup_schema.
# Примечание: Это необходимо сделать после настройки базы и сеанса.
event.listen(mapper, "after_configured", setup_schema(Base, session))

Base.metadata.create_all(engine)

author = Author(name="Chuck Paluhniuk")
session.add(author)
session.commit()

# Model.__marshmallow__ возвращает класс, а не экземпляр схемы,
# поэтому не забудьте создать его экземпляр
author_schema = Author.__marshmallow__()

print(author_schema.dump(author))
```

Это вдохновлено функциональностью **ColanderAlchemy**.

## Умное вложенное поле

Чтобы сериализовать вложенные атрибуты в первичные ключи, если они еще не загружены, вы можете использовать это настраиваемое поле.

```python
from marshmallow_sqlalchemy.fields import Nested

class SmartNested(Nested):
    def serialize(self, attr, obj, accessor=None):
        if attr not in obj.__dict__:
            return {"id": int(getattr(obj, attr + "_id"))}
        return super(SmartNested, self).serialize(attr, obj, accessor)
```

Пример использования этого:

```python
from marshmallow_sqlalchemy import SQLAlchemySchema, auto_field

class BookSchema(SQLAlchemySchema):
    id = auto_field()
    author = SmartNested(AuthorSchema)

    class Meta:
        model = Book
        sqla_session = Session

book = Book(id=1)
book.author = Author(name="Chuck Paluhniuk")
session.add(book)
session.commit()

book = Book.query.get(1)
print(BookSchema().dump(book)["author"])
# {'id': 1}

book = Book.query.options(joinedload("author")).get(1)
print(BookSchema().dump(book)["author"])
# {'id': 1, 'name': 'Chuck Paluhniuk'}
```

## Создание переходного объекта

Иногда может потребоваться десериализовать переходные экземпляры (не привязанные к сеансу). В этих случаях вы можете указать переходную опцию в классе <mark style="color:red;">Meta</mark> схемы <mark style="color:red;">SQLAlchemySchema</mark>.

```python
from marshmallow_sqlalchemy import SQLAlchemyAutoSchema

class AuthorSchema(SQLAlchemyAutoSchema):
    class Meta:
        model = Author
        load_instance = True
        transient = True

dump_data = {"id": 1, "name": "John Steinbeck"}
print(AuthorSchema().load(dump_data))
# <Author(name='John Steinbeck')>
```

Вы также можете явно указать переопределение, передав тот же аргумент для загрузки.

```python
from marshmallow_sqlalchemy import SQLAlchemyAutoSchema

class AuthorSchema(SQLAlchemyAutoSchema):
    class Meta:
        model = Author
        sqla_session = Session
        load_instance = True

dump_data = {"id": 1, "name": "John Steinbeck"}
print(AuthorSchema().load(dump_data, transient=True))
# <Author(name='John Steinbeck')>
```

Обратите внимание, что переходность распространяется на отношения (т. е. автоматически сгенерированные схемы для вложенных элементов также будут временными).

{% hint style="info" %}
СМОТРИ ТАКЖЕ:

См. [Управление состоянием](https://docs.sqlalchemy.org/en/latest/orm/session\_state\_management.html), чтобы понять управление состоянием сеанса.
{% endhint %}

## Управление загрузкой экземпляра

Вы можете переопределить флаг **load\_instance** схемы, передав аргумент **load\_instance** при создании экземпляра схемы. Используйте это для переключения между загрузкой в словарь или в экземпляр модели:

```python
from marshmallow_sqlalchemy import SQLAlchemyAutoSchema

class AuthorSchema(SQLAlchemyAutoSchema):
    class Meta:
        model = Author
        sqla_session = Session
        load_instance = True

dump_data = {"id": 1, "name": "John Steinbeck"}
print(AuthorSchema().load(dump_data))  # загрузка экземпляра
# <Author(name='John Steinbeck')>
print(AuthorSchema(load_instance=False).load(dump_data))  # загрузка словаря
# {"id": 1, "name": "John Steinbeck"}
```
