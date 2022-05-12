# Вспомогательные функции

Полезные методы для **marshmallow**.

## callable\_or\_raise

#### marshmallow.utils.callable\_or\_raise(_obj_)

Проверяет, что объект можно вызывать, иначе вызовите [TypeError](https://python.readthedocs.io/en/latest/library/exceptions.html#TypeError).

## from\_iso\_date

#### marshmallow.utils.from\_iso\_date(_value_)

Разбирает строку и возвращает `datetime.date`.

## from\_iso\_datetime

#### marshmallow.utils.from\_iso\_datetime(_value_)

Разбирает строку и возвращает `datetime.datetime`.

Эта функция поддерживает смещения часовых поясов. Когда вход содержит один, выход использует часовой пояс с фиксированным смещением от UTC.

## from\_iso\_time

#### marshmallow.utils.from\_iso\_time(_value_)

Разбирает строку и возвращает `datetime.time`.

Эта функция не поддерживает смещения часовых поясов.

## from\_rfc

#### marshmallow.utils.from\_rfc(_datestring: str_) → datetime.datetime

Разбирает строку даты и времени в формате RFC822 и возвращает объект даты и времени.

[https://stackoverflow.com/questions/885015/how-to-parse-a-rfc-2822-date-time-into-a-python-datetime](https://stackoverflow.com/questions/885015/how-to-parse-a-rfc-2822-date-time-into-a-python-datetime) # noqa: B950

## from\_fixed\_timezone

#### marshmallow.utils.get\_fixed\_timezone(_offset: int | float | dt.timedelta_) → dt.timezone

Возвращает экземпляр **tzinfo** с фиксированным смещением от UTC.

## get\_func\_args

#### marshmallow.utils.get\_func\_args(_func: Callable_) → list\[str]

Учитывая вызываемый объект, вернуть список имен аргументов. Обрабатывает объекты [functools.partial](https://python.readthedocs.io/en/latest/library/functools.html#functools.partial) и вызываемые объекты на основе классов.

_<mark style="color:green;">Изменено в версии 3.0.0a1</mark>_: Не возвращает связанные аргументы, например, self.

## get\_value

#### marshmallow.utils.get\_value(_obj_, _key: int | str_, _default=\<marshmallow.missing>_)

Помощник для извлечения ключевого значения из различных типов объектов. Поля используют этот метод по умолчанию для доступа к атрибутам исходного объекта. Для объекта **x** и атрибута **i** этот метод сначала пытается получить доступ к **`x[i]`**, а затем возвращается к **`x.i`**, если возникает исключение.

{% hint style="warning" %}
Если объект **x** не вызывает исключение, когда **`x[i]`** не существует, **get\_value** никогда не будет проверять значение **`x.i`**. В этом случае рассмотрите возможность переопределения `marshmallow.fields.Field.get_value`.
{% endhint %}

## is\_collection

#### marshmallow.utils.is\_collection(_obj_) → bool

Возвращает `True`, если **obj** является типом коллекции, например, списком, кортежем, набором запросов.

## is\_generator

#### marshmallow.utils.is\_generator(_obj_) → bool

Возвращает `True`, если **obj** является генератором

## is\_instance\_or\_subclass

#### marshmallow.utils.is\_instance\_or\_subclass(_val_, _class\__) → bool

Возвращает `True`, если значение **val** является либо подклассом, либо экземпляром класса.

## is\_iterable\_but\_not\_string

#### marshmallow.utils.is\_iterable\_but\_not\_string(_obj_) → bool

Возвращает `True`, если **obj** является итерируемым объектом, который не является строкой.

## is\_keyed\_tuple

#### marshmallow.utils.is\_keyed\_tuple(_obj_) → bool

Возвращает `True`, если **obj** имеет ключевое поведение кортежа, такого как **namedtuples** или **KeyedTuples** SQLAlchemy.

## isoformat

#### marshmallow.utils.isoformat(_datetime: datetime.datetime_) → str

Возвращает представление объекта **datetime** в формате ISO8601.

#### Параметры:

* **datetime** (datetime) - Дата и время.

## pluck

#### marshmallow.utils.pluck(_dictlist: list\[dict\[str, Any]]_, _key: str_)

Извлекает список значений словаря из списка словарей.

```python
>>> dlist = [{'id': 1, 'name': 'foo'}, {'id': 2, 'name': 'bar'}]
>>> pluck(dlist, 'id')
[1, 2]
```

## pprint

#### marshmallow.utils.pprint(_obj_, _\*args_, _\*\*kwargs_) → None

Функция красивой печати, которая может красиво печатать **OrderedDicts**, как обычные словари. Полезно для печати вывода [marshmallow.Schema.dump()](skhema-schema.md#dump).

_<mark style="color:orange;">Устарело, начиная с версии 3.7.0</mark>_: **marshmallow.pprint** будет удален в **marshmallow 4**.

## resolve\_field\_instance

#### marshmallow.utils.resolve\_field\_instance(_cls\_or\_instance_)

Возвращает экземпляр схемы из класса или экземпляра схемы.

#### Параметры:

* **cls\_or\_instance (type | Schema)** - Класс или экземпляр схемы **Marshmallow**.

## rfcformat

#### marshmallow.utils.rfcformat(_datetime: datetime.datetime_) → str

Возвращает представление объекта **datetime** в формате RFC822.

#### Параметры:

* **datetime** (datetime) - Дата и время.

## set\_value

#### marshmallow.utils.set\_value(_dct: dict\[str, Any]_, _key: str_, _value: Any_)

Установить значение в **dict**. Если ключ содержит `«.»`, предполагается, что это путь (т. е. строка, разделенная точками) к местоположению значения.

```python
>>> d = {}
>>> set_value(d, 'foo.bar', 42)
>>> d
{'foo': {'bar': 42}}
```

## timedelta\_to\_microseconds

#### marshmallow.utils.timedelta\_to\_microseconds(_value: datetime.timedelta_) → int

Вычислить общее количество микросекунд **timedelta**

[https://github.com/python/cpython/blob/bb3e0c240bc60fe08d332ff5955d54197f79751c/Lib/datetime.py#L665-L667](https://github.com/python/cpython/blob/bb3e0c240bc60fe08d332ff5955d54197f79751c/Lib/datetime.py#L665-L667) # noqa: B950
