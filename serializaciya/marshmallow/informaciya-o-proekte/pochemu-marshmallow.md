# Почему marshmallow ?

В экосистеме Python есть много отличных библиотек для форматирования данных и проверки схемы.

На самом деле, на **marshmallow** повлиял ряд этих библиотек. **Marshmallow** вдохновлен [Django REST Framework](https://www.django-rest-framework.org/), [Flask-RESTful](https://flask-restful.readthedocs.io/) и [colander](https://docs.pylonsproject.org/projects/colander/en/latest/). Он заимствует ряд идей реализации и дизайна из этих библиотек, чтобы создать гибкое и продуктивное решение для сортировки, десортировки и проверки данных.

Вот лишь несколько причин, по которым вы можете использовать **marshmallow**.

### Агностик

**Marshmallow** не делает никаких предположений о веб-фреймворках или уровнях базы данных. Он будет работать практически с любым ORM, ODM или вообще без ORM. Это дает вам свободу выбора компонентов, соответствующих потребностям вашего приложения, без необходимости изменять код форматирования данных. При желании вы можете создать слои интеграции, чтобы **marshmallow** работал более тесно с выбранными вами фреймворками и библиотеками (например, см. [Flask-Marshmallow](https://github.com/marshmallow-code/flask-marshmallow) и [Django REST Marshmallow](https://github.com/marshmallow-code/flask-marshmallow)).

### Краткий, знакомый синтаксис.

Если вы использовали [Django REST Framework](https://www.django-rest-framework.org/) или [WTForms](https://wtforms.readthedocs.io/en/stable/), синтаксис схемы marshmallow [Schema](../api-marshmallow/skhema-schema.md#class-marshmallow.schema) покажется вам знакомым. Атрибуты полей уровня класса определяют схему форматирования данных. Конфигурация добавляется с использованием парадигмы класса **Meta**. Параметры конфигурации можно переопределить во время выполнения приложения, передав аргументы конструктору схемы. Методы [dump](../api-marshmallow/skhema-schema.md#dump) и [load](../api-marshmallow/skhema-schema.md#load) используются для сериализации и десериализации (конечно же!).

### Схемы на основе классов позволяют повторно использовать и настраивать код

В отличие от [Flask-RESTful](https://flask-restful.readthedocs.io/), который использует словари для определения выходных схем, **marshmallow** использует классы. Это позволяет легко повторно использовать и настраивать код. Он также позволяет использовать мощные средства для настройки и расширения схем, такие как [добавление поведения постобработки и обработки ошибок](../rukovodstvo-marshmallow/rasshirennye-skhemy.md).

### Согласованность в сочетании с гибкостью

**Marshmallow** упрощает изменение вывода схемы во время выполнения приложения. Одна схема [Schema](../api-marshmallow/skhema-schema.md#class-marshmallow.schema) может создавать несколько выходных форматов, сохраняя согласованность выходных данных отдельных полей.

Например, у вас может быть конечная точка JSON для получения всей информации о состоянии видеоигры. Затем вы добавляете конечную точку с малой задержкой, которая возвращает только минимальное подмножество информации о состоянии игры. Обе конечные точки могут обрабатываться одной и той же схемой [Schema](../api-marshmallow/skhema-schema.md).

```python
class GameStateSchema(Schema):
    _id = fields.UUID(required=True)
    score = fields.Nested(ScoreSchema)
    players = fields.List(fields.Nested(PlayerSchema))
    last_changed = fields.DateTime(format="rfc")

    class Meta:
        additional = ("title", "date_created", "type", "is_active")

# Сериализирует полное состояние игры
full_serializer = GameStateSchema()
# Сериализирует подмножество информации для конечной точки с малой задержкой.
summary_serializer = GameStateSchema(only=("_id", "last_changed"))
# Также фильтруйте поля при сериализации нескольких игр.
gamelist_serializer = GameStateSchema(
    many=True, only=("_id", "players", "last_changed")
)
```

В этом примере одна схема дала три разных результата! Динамический характер схемы приводит к **меньшему количеству кода** и **более последовательному форматированию**.

### Контекстно-зависимая сериализация

Схемы **Marshmallow** могут изменять свой вывод в зависимости от контекста, в котором они используются. Объекты полей имеют доступ к контекстному словарю **context**, который можно изменить во время выполнения.

Вот простой пример, показывающий, как схема [Schema](../api-marshmallow/skhema-schema.md#class-marshmallow.schema) может анонимизировать имя человека, когда в контексте задано логическое значение.

```python
class PersonSchema(Schema):
    id = fields.Integer()
    name = fields.Method("get_name")

    def get_name(self, person, context):
        if context.get("anonymize"):
            return "<anonymized>"
        return person.name

person = Person(name="Monty")
schema = PersonSchema()
schema.dump(person)  # {'id': 143, 'name': 'Monty'}

# В другом контексте анонимизировать имя
schema.context["anonymize"] = True
schema.dump(person)  # {'id': 143, 'name': '<anonymized>'}
```

{% hint style="info" %}
**СМОТРИ ТАКЖЕ:**

См. соответствующий раздел [руководства по использованию](../rukovodstvo-marshmallow/polzovatelskie-polya.md#deserializaciya-polei-method-i-function), чтобы узнать больше о сериализации с учетом контекста.
{% endhint %}

### Расширенное вложение схемы

Большинство библиотек сериализации предоставляют некоторые средства для вложения схем друг в друга, но они часто не соответствуют обычным случаям использования в чистом виде. **Marshmallow** стремится заполнить эти пробелы, добавив несколько приятных функций для [схем вложенности](../rukovodstvo-marshmallow/vlozhennye-skhemy.md):

* Вы можете указать, какое [подмножество полей](../rukovodstvo-marshmallow/vlozhennye-skhemy.md#ukazanie-togo-kakie-polya-sleduet-vkladyvat) включать во вложенные схемы.
* [Двустороннее вложение](../rukovodstvo-marshmallow/vlozhennye-skhemy.md#dvukhstoronnyaya-vlozhennost). Две разные схемы могут вкладываться друг в друга.
* [Самовложение](../rukovodstvo-marshmallow/vlozhennye-skhemy.md#vlozhenie-skhemy-vnutr-samoi-sebya). Схема может быть вложена сама в себя.
