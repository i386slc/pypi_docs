# (ma-sa) API

## Core

### fields\_for\_model

#### marshmallow\_sqlalchemy.fields\_for\_model _=func(...)_

### property2field

#### marshmallow\_sqlalchemy.property2field _=func(...)_

### column2field

#### marshmallow\_sqlalchemy.column2field _=func(...)_

### field\_for

#### marshmallow\_sqlalchemy.field\_for _=func(...)_

### ModelConversionError

#### _exception_ marshmallow\_sqlalchemy.ModelConversionError

Возбуждается при возникновении ошибки при преобразовании конструкции SQLAlchemy в объект marshmallow.

### ModelConverter

#### _class_ marshmallow\_sqlalchemy.ModelConverter(_schema\_cls=None_)

Класс, который преобразует модель SQLAlchemy в словарь соответствующих полей marshmallow [Field](../marshmallow/api-marshmallow/polya-fields-a-f.md#field).

### SQLAlchemyAutoSchema

#### _class_ marshmallow\_sqlalchemy.SQLAlchemyAutoSchema(_\*args_, _\*\*kwargs_)

Схема, которая автоматически генерирует поля из столбцов модели или таблица SQLAlchemy.

Пример:

```python
from marshmallow_sqlalchemy import SQLAlchemyAutoSchema, auto_field

from mymodels import User

class UserSchema(SQLAlchemyAutoSchema):
    class Meta:
        model = User
        # ИЛИ
        # table = User.__table__

    created_at = auto_field(dump_only=True)
```

#### OPTIONS\_CLASS

псевдоним [marshmallow\_sqlalchemy.schema.SQLAlchemyAutoSchemaOpts](ma-sa-api.md#sqlalchemyautoschemaopts)

### SQLAlchemyAutoSchemaOpts

#### _class_ marshmallow\_sqlalchemy.SQLAlchemyAutoSchemaOpts(_meta_, _\*args_, _\*\*kwargs_)

Класс параметров для [SQLAlchemyAutoSchema](ma-sa-api.md#sqlalchemyautoschema). Имеет те же параметры, что и <mark style="color:red;">SQLAlchemySchemaOpts</mark>, с добавлением:

* **include\_fk** - Включать ли внешние поля; по умолчанию `False`.
* **include\_relationships** - Включать ли отношения; по умолчанию `False`.

### SQLAlchemySchema

#### _class_ marshmallow\_sqlalchemy.SQLAlchemySchema(_\*args_, _\*\*kwargs_)

Схема для модели или таблицы SQLAlchemy. Используйте вместе с <mark style="color:red;">auto\_field</mark> для создания полей из столбцов.

Пример:

```python
from marshmallow_sqlalchemy import SQLAlchemySchema, auto_field

from mymodels import User

class UserSchema(SQLAlchemySchema):
    class Meta:
        model = User

    id = auto_field()
    created_at = auto_field(dump_only=True)
    name = auto_field()
```

#### OPTIONS\_CLASS

псевдоним [marshmallow\_sqlalchemy.schema.SQLAlchemySchemaOpts](ma-sa-api.md#sqlalchemyschemaopts)

### SQLAlchemySchemaOpts

#### _class_ marshmallow\_sqlalchemy.SQLAlchemySchemaOpts(_meta_, _\*args_, _\*\*kwargs_)

Класс параметров для [SQLAlchemySchema](ma-sa-api.md#sqlalchemyschema). Добавляет следующие опции:

* **model** - Модель SQLAlchemy для создания схемы **Schema** (взаимоисключающий с параметром **table**).
* **table** - Таблица SQLAlchemy, из которой создается схема **Schema** (взаимоисключающий с параметром **model**).
* **load\_instance** - Загружать ли экземпляры модели.
* **sqla\_session** - Сеанс SQLAlchemy, который будет использоваться для десериализации. Это необходимо только в том случае, если **load\_instance** имеет значение `True`. Вы также можете передать сеанс методу загрузки схемы.
* **transient** - Следует ли загружать экземпляры модели в переходном состоянии (фактически игнорируя сеанс). Имеет значение только в том случае, если **load\_instance** имеет значение `True`.
* **model\_converter** - Класс [ModelConverter](ma-sa-api.md#modelconverter) для преобразования модели SQLAlchemy в поля marshmallow.

## Поля Fields
