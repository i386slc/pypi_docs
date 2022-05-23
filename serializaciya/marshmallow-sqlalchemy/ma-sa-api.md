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

### auto\_field()

#### marshmallow\_sqlalchemy.auto\_field(_column\_name: Optional\[str] = None_, _\*_, _model: Optional\[sqlalchemy.orm.decl\_api.DeclarativeMeta] = None_, _table: Optional\[sqlalchemy.sql.schema.Table] = None_, _\*\*kwargs_)

Отмечает поле для автоматического создания из модели или таблицы.

#### Параметры:

* **column\_name** - Имя столбца, из которого создается поле. Если `None`, соответствует имени поля. Если **attribute** не указан, для **attribute** будет установлено то же значение, что и для **column\_name**.
* **model** - Модель для создания поля. Если `None`, использует модель **model**, указанную в классе **Meta**.
* **table** - Таблица, из которой создается поле. Если `None`, использует **table**, указанную в классе **Meta**.
* **kwargs** - Переопределение аргументов поля Field.

## Поля Fields

### Nested

#### _class_ marshmallow\_sqlalchemy.fields.Nested(_nested: SchemaABC | type | str | dict\[str, Field | type] | typing.Callable\[\[], SchemaABC | dict\[str, Field | type]], \*, dump\_default: typing.Any = \<marshmallow.missing>, default: typing.Any = \<marshmallow.missing>, only: types.StrSequenceOrSet | None = None, exclude: types.StrSequenceOrSet = (), many: bool = False, unknown: str | None = None, \*\*kwargs_)

Вложенное поле, которое наследует сеанс от своего родителя.

#### \_deserialize(_\*args_, _\*\*kwargs_)

То же, что и **Field.\_deserialize()**, но с дополнительным аргументом **partial**.

#### Параметры:

* **partial** (_bool_ | _tuple_) - Для вложенных схем параметр **partial** передается в **Schema.load**.

_Изменено в версии 3.0.0_: Добавлен параметр **partial**.

### Related

#### _class_ marshmallow\_sqlalchemy.fields.Related(_column=None_, _\*\*kwargs_)

Связанные данные, представленные отношением SQLAlchemy. Должен быть присоединен к классу **Schema**, параметры которого включают модель SQLAlchemy, например **SQLAlchemySchema**.

#### Параметры:

* **columns** (_list_) - Необязательные имена столбцов в связанной модели. Если он не указан, будут использоваться первичные ключи связанной модели.

#### \_deserialize(_value_, _\*args_, _\*\*kwargs_)

Десериализовать сериализованное значение в экземпляр модели.

Если родительская схема является переходной, создайте новый (переходный) экземпляр. В противном случае попытайтесь найти существующий экземпляр в базе данных. `:param value:` Значение для десериализации.

#### \_get\_existing\_instance(_query_, _value_)

Получить связанный объект из существующего экземпляра в БД.

#### Параметры:

* **query** - Объект запроса SQLAlchemy [Query](https://docs.sqlalchemy.org/en/14/orm/query.html#sqlalchemy.orm.Query).
* **value** - Сериализованное значение для сопоставления с существующим экземпляром.

**Поднимает**: **NoResultFound** – если нет соответствующей записи.

#### \_serialize(_value_, _attr_, _obj_)

Сериализует значение **value** в базовый тип данных Python. Noop по умолчанию. Конкретные классы **Field** должны реализовывать этот метод.

Пример:

```python
class TitleCase(Field):
    def _serialize(self, value, attr, obj, **kwargs):
        if not value:
            return ''
        return str(value).title()
```

#### Параметры:

* **value** - Значение, которое нужно сериализовать
* **attr** (_str_) - Атрибут или ключ объекта, который нужно сериализовать.
* **obj** (_object_) - Объект, из которого было извлечено значение.
* **kwargs** (_dict_) - Аргументы ключевого слова, специфичные для [Field](../marshmallow/api-marshmallow/polya-fields-a-f.md#field).

**Возвращает**: Сериализованное значение.

### RelatedList

#### _class_ marshmallow\_sqlalchemy.fields.RelatedList(_cls\_or\_instance: Field | type_, _\*\*kwargs_)

#### get\_value(_obj_, _attr_, _accessor=None_)

Возвращает значение для данного ключа из объекта.

#### Параметры:

* **obj** (_object_) - Объект, из которого нужно получить значение.
* **attr** (_str_) - Атрибут/ключ в объекте, из которого нужно получить значение.
* **accessor** (_callable_) - Вызываемый объект, используемый для получения значения **attr** из объекта **obj**. По умолчанию используется [marshmallow.utils.get\_value](https://marshmallow.readthedocs.io/en/latest/marshmallow.utils.html#marshmallow.utils.get\_value).

### get\_primary\_key()

#### marshmallow\_sqlalchemy.fields.get\_primary\_keys(_model_)

Получает свойства первичного ключа для модели SQLAlchemy.

#### Параметры:

* **model** - Класс модели SQLAlchemy
