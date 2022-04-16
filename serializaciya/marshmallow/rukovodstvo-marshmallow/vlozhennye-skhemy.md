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
