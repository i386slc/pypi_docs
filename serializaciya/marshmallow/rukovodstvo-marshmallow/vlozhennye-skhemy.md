# Вложенные схемы

Схемы могут быть вложены для представления отношений между объектами (например, отношений внешнего ключа). Например, у блога **Blog** может быть автор, представленный объектом **User**.

```python
import datetime as dt

class User:
    def __init__(self, name, email):
        self.name = name
        self.email = email
        self.created_at = dt.datetime.now()
        self.friends = []
        self.employer = None

class Blog:
    def __init__(self, title, author):
        self.title = title
        self.author = author  # объект User
```

Используйте вложенное поле [Nested](../api-marshmallow/polya-fields.md#class-marshmallow.fields.nested-nested-schemaabc-or-type-or-str-or-dict-str-field-or-type-or-typing.) для представления отношения, передавая вложенную схему.

```python
from marshmallow import Schema, fields

class UserSchema(Schema):
    name = fields.String()
    email = fields.Email()
    created_at = fields.DateTime()

class BlogSchema(Schema):
    title = fields.String()
    author = fields.Nested(UserSchema)
```

Сериализованный блог будет иметь вложенное представление пользователя.

```python
from pprint import pprint

user = User(name="Monty", email="monty@python.org")
blog = Blog(title="Something Completely Different", author=user)
result = BlogSchema().dump(blog)
pprint(result)
# {'title': u'Something Completely Different',
#  'author': {'name': u'Monty',
#             'email': u'monty@python.org',
#             'created_at': '2014-08-17T14:58:57.600623+00:00'}}
```

{% hint style="info" %}
Если поле представляет собой набор вложенных объектов, передайте вложенное поле [Nested](../api-marshmallow/polya-fields.md#class-marshmallow.fields.nested-nested-schemaabc-or-type-or-str-or-dict-str-field-or-type-or-typing.) в список [List](../api-marshmallow/polya-fields.md#class-marshmallow.fields.list-cls\_or\_instance-field-or-type-kwargs).
{% endhint %}

## Указание того, какие поля следует вкладывать

Вы можете явно указать, какие атрибуты вложенных объектов вы хотите (де)сериализовать, с помощью аргумента **only** схемы.

```python
class BlogSchema2(Schema):
    title = fields.String()
    author = fields.Nested(UserSchema(only=("email",)))

schema = BlogSchema2()
result = schema.dump(blog)
pprint(result)
# {
#     'title': u'Something Completely Different',
#     'author': {'email': u'monty@python.org'}
# }
```

Пути с точками могут быть переданы в **only** и **exclude** для указания вложенных атрибутов.

```python
class SiteSchema(Schema):
    blog = fields.Nested(BlogSchema2)

schema = SiteSchema(only=("blog.author.email",))
result = schema.dump(site)
pprint(result)
# {
#     'blog': {
#         'author': {'email': u'monty@python.org'}
#     }
# }
```

Вы можете заменить вложенные данные одним значением (или плоским списком значений, если `many = True`), используя поле [Pluck](../api-marshmallow/polya-fields.md#class-marshmallow.fields.pluck-nested-schemaabc-or-type-or-str-or-callable-schemaabc-field\_name-str).

```python
class UserSchema(Schema):
    name = fields.String()
    email = fields.Email()
    friends = fields.Pluck("self", "name", many=True)

# ... create ``user`` ...
serialized_data = UserSchema().dump(user)
pprint(serialized_data)
# {
#     "name": "Steve",
#     "email": "steve@example.com",
#     "friends": ["Mike", "Joe"]
# }
deserialized_data = UserSchema().load(result)
pprint(deserialized_data)
# {
#     "name": "Steve",
#     "email": "steve@example.com",
#     "friends": [{"name": "Mike"}, {"name": "Joe"}]
# }
```

## Частичная загрузка

Вложенные схемы также наследуют параметр **partial** родительского вызова загрузки **load**.

```python
class UserSchemaStrict(Schema):
    name = fields.String(required=True)
    email = fields.Email()
    created_at = fields.DateTime(required=True)

class BlogSchemaStrict(Schema):
    title = fields.String(required=True)
    author = fields.Nested(UserSchemaStrict, required=True)

schema = BlogSchemaStrict()
blog = {"title": "Something Completely Different", "author": {}}
result = schema.load(blog, partial=True)
pprint(result)
# {'author': {}, 'title': 'Something Completely Different'}
```

Вы можете указать подмножество полей, чтобы разрешить частичную загрузку, используя точечные разделители.

```python
author = {"name": "Monty"}
blog = {"title": "Something Completely Different", "author": author}
result = schema.load(blog, partial=("title", "author.created_at"))
pprint(result)
# {'author': {'name': 'Monty'}, 'title': 'Something Completely Different'}
```

## Двухсторонняя вложенность
