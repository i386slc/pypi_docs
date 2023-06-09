# (ma-sa) Быстрый старт

Релиз v0.28.0 ([Журнал изменений](ma-sa-informaciya-o-proekte.md#zhurnal-izmenenii))

Интеграция [SQLAlchemy](http://www.sqlalchemy.org/) с библиотекой (де)сериализации [marshmallow](https://marshmallow.readthedocs.io/en/latest/).

## Объявление ваших моделей

```python
import sqlalchemy as sa
from sqlalchemy.ext.declarative import declarative_base
from sqlalchemy.orm import scoped_session, sessionmaker, relationship, backref

engine = sa.create_engine("sqlite:///:memory:")
session = scoped_session(sessionmaker(bind=engine))
Base = declarative_base()

class Author(Base):
    __tablename__ = "authors"
    id = sa.Column(sa.Integer, primary_key=True)
    name = sa.Column(sa.String, nullable=False)

    def __repr__(self):
        return "<Author(name={self.name!r})>".format(self=self)

class Book(Base):
    __tablename__ = "books"
    id = sa.Column(sa.Integer, primary_key=True)
    title = sa.Column(sa.String)
    author_id = sa.Column(sa.Integer, sa.ForeignKey("authors.id"))
    author = relationship("Author", backref=backref("books"))

Base.metadata.create_all(engine)
```

## Создание схем marshmallow

```python
from marshmallow_sqlalchemy import SQLAlchemySchema, auto_field

class AuthorSchema(SQLAlchemySchema):
    class Meta:
        model = Author
        load_instance = True  # Необязательно: десериализовать экземпляры модели

    id = auto_field()
    name = auto_field()
    books = auto_field()

class BookSchema(SQLAlchemySchema):
    class Meta:
        model = Book
        load_instance = True

    id = auto_field()
    title = auto_field()
    author_id = auto_field()
```

Вы можете автоматически создавать поля для столбцов модели с помощью **SQLAlchemyAutoSchema**. Следующие классы схемы эквивалентны приведенным выше.

```python
from marshmallow_sqlalchemy import SQLAlchemyAutoSchema

class AuthorSchema(SQLAlchemyAutoSchema):
    class Meta:
        model = Author
        include_relationships = True
        load_instance = True

class BookSchema(SQLAlchemyAutoSchema):
    class Meta:
        model = Book
        include_fk = True
        load_instance = True
```

Обязательно объявите модели перед созданием экземпляров схем. В противном случае [sqlalchemy.orm.configure\_mappers()](https://docs.sqlalchemy.org/en/latest/orm/mapping\_api.html) запустится слишком рано и завершится ошибкой.

{% hint style="info" %}
Любое свойство **column\_property** в модели, которое не является прямым производным от столбца **Column** (например, сопоставленное выражение), будет обнаружено и помечено как **dump\_only**.

**hybrid\_property** вообще не обрабатывается автоматически, и его необходимо явно объявить как поле.
{% endhint %}

## (Де)сериализовать ваши данные

```python
author = Author(name="Chuck Paluhniuk")
author_schema = AuthorSchema()
book = Book(title="Fight Club", author=author)
session.add(author)
session.add(book)
session.commit()

dump_data = author_schema.dump(author)
print(dump_data)
# {'id': 1, 'name': 'Chuck Paluhniuk', 'books': [1]}

load_data = author_schema.load(dump_data, session=session)
print(load_data)
# <Author(name='Chuck Paluhniuk')>
```

## Получи это сейчас

```bash
pip install -U marshmallow-sqlalchemy
```

Требуется `Python >= 3.7`, `marshmallow >= 3.0.0` и `SQLAlchemy >= 1.3.0`.
