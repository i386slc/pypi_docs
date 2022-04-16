# Поля Fields

### _class_ marshmallow.fields.Email(_\*args_, _\*\*kwargs_)

### _class_ marshmallow.fields.DateTime(_format: str | None = None_, _\*\*kwargs_)

### _class_ marshmallow.fields.Field(_\*, load\_default: typing.Any = \<marshmallow.missing>, missing: typing.Any = \<marshmallow.missing>, dump\_default: typing.Any = \<marshmallow.missing>, default: typing.Any = \<marshmallow.missing>, data\_key: str | None = None, attribute: str | None = None, validate: None | (typing.Callable\[\[typing.Any], typing.Any] | typing.Iterable\[typing.Callable\[\[typing.Any], typing.Any]]) = None, required: bool = False, allow\_none: bool | None = None, load\_only: bool = False, dump\_only: bool = False, error\_messages: dict\[str, str] | None = None, metadata: typing.Mapping\[str, typing.Any] | None = None, \*\*additional\_metadata_)

### _class_ marshmallow.fields.Function(_serialize: None | Callable\[\[Any], Any] | Callable\[\[Any, dict], Any] = None_, _deserialize: None | Callable\[\[Any], Any] | Callable\[\[Any, dict], Any] = None_, _\*\*kwargs_)

### _class_ marshmallow.fields.List(_cls\_or\_instance: Field | type_, _\*\*kwargs_)

### _class_ marshmallow.fields.Method(_serialize: str | None = None_, _deserialize: str | None = None_, _\*\*kwargs_)

### _class_ marshmallow.fields.Nested(_nested: SchemaABC | type | str | dict\[str, Field | type] | typing.Callable\[\[], SchemaABC | dict\[str, Field | type]], \*, dump\_default: typing.Any = \<marshmallow.missing>, default: typing.Any = \<marshmallow.missing>, only: types.StrSequenceOrSet | None = None, exclude: types.StrSequenceOrSet = (), many: bool = False, unknown: str | None = None, \*\*kwargs_)

### _class_ marshmallow.fields.Pluck(_nested: SchemaABC | type | str | Callable\[\[], SchemaABC]_, _field\_name: str_, _\*\*kwargs_)

### _class_ marshmallow.fields.String(_\*, load\_default: typing.Any = \<marshmallow.missing>, missing: typing.Any = \<marshmallow.missing>, dump\_default: typing.Any = \<marshmallow.missing>, default: typing.Any = \<marshmallow.missing>, data\_key: str | None = None, attribute: str | None = None, validate: None | (typing.Callable\[\[typing.Any], typing.Any] | typing.Iterable\[typing.Callable\[\[typing.Any], typing.Any]]) = None, required: bool = False, allow\_none: bool | None = None, load\_only: bool = False, dump\_only: bool = False, error\_messages: dict\[str, str] | None = None, metadata: typing.Mapping\[str, typing.Any] | None = None, \*\*additional\_metadata_)

### marshmallow.fields.URL
