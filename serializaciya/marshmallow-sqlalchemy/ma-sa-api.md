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

псевдоним marshmallow\_sqlalchemy.schema.SQLAlchemyAutoSchemaOpts

### SQLAlchemyAutoSchemaOpts

#### _class_ marshmallow\_sqlalchemy.SQLAlchemyAutoSchemaOpts(_meta_, _\*args_, _\*\*kwargs_)

Класс параметров для [SQLAlchemyAutoSchema](ma-sa-api.md#sqlalchemyautoschema). Имеет те же параметры, что и <mark style="color:red;">SQLAlchemySchemaOpts</mark>, с добавлением:

* **include\_fk** - Включать ли внешние поля; по умолчанию `False`.
* **include\_relationships** - Включать ли отношения; по умолчанию `False`.

### SQLAlchemySchema

#### _class_ marshmallow\_sqlalchemy.SQLAlchemySchema(_\*args_, _\*\*kwargs_)

## Поля Fields
