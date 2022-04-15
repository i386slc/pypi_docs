# API marshmallow

### _class_ marshmallow.Schema(_\*_, _only: types.StrSequenceOrSet | None = None_, _exclude: types.StrSequenceOrSet = ()_, _many: bool = False_, _context: dict | None = None_, _load\_only: types.StrSequenceOrSet = ()_, _dump\_only: types.StrSequenceOrSet = ()_, _partial: bool | types.StrSequenceOrSet = False_, _unknown: str | None = None_)

### dump(_obj: Any_, _\*_, _many: bool | None = None_)

### dumps(_obj: Any_, _\*args_, _many: bool | None = None_, _\*\*kwargs_)

### handle\_error(_error: marshmallow.exceptions.ValidationError_, _data: Any_, _\*_, _many: bool_, _\*\*kwargs_)

### load(_data: Mapping\[str, Any] | Iterable\[Mapping\[str, Any]]_, _\*_, _many: bool | None = None_, _partial: bool | types.StrSequenceOrSet | None = None_, _unknown: str | None = None_)

### loads(_json\_data: str_, _\*_, _many: bool | None = None_, _partial: bool | types.StrSequenceOrSet | None = None_, _unknown: str | None = None_, _\*\*kwargs_)

### validate(_data: Mapping\[str, Any] | Iterable\[Mapping\[str, Any]]_, _\*_, _many: bool | None = None_, _partial: bool | types.StrSequenceOrSet | None = None_) → dict\[str, list\[str]]
