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

```python
collaborators = fields.List(fields.Nested(UserSchema))
```
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

Если у вас есть два объекта, которые вложены друг в друга, вы можете передать вызываемый объект в [Nested](../api-marshmallow/polya-fields.md#class-marshmallow.fields.nested-nested-schemaabc-or-type-or-str-or-dict-str-field-or-type-or-typing.). Это позволяет решить проблемы с порядком объявления, например, когда одна схема вкладывает схему, которая объявлена под ней.

Например, представление модели **Author** может включать в себя книги, которые связаны с ней отношением «многие к одному». Соответственно, представление **Book** будет включать представление ее автора.

```python
class BookSchema(Schema):
    id = fields.Int(dump_only=True)
    title = fields.Str()

    # Убедитесь, что вы используете 'only' или 'exclude'
    # чтобы избежать бесконечной рекурсии
    author = fields.Nested(lambda: AuthorSchema(only=("id", "title")))

class AuthorSchema(Schema):
    id = fields.Int(dump_only=True)
    title = fields.Str()

    books = fields.List(fields.Nested(BookSchema(exclude=("author",))))
```

```python
from marshmallow import pprint
from mymodels import Author, Book

author = Author(name="William Faulkner")
book = Book(title="As I Lay Dying", author=author)
book_result = BookSchema().dump(book)
pprint(book_result, indent=2)
# {
#   "id": 124,
#   "title": "As I Lay Dying",
#   "author": {
#     "id": 8,
#     "name": "William Faulkner"
#   }
# }

author_result = AuthorSchema().dump(author)
pprint(author_result, indent=2)
# {
#   "id": 8,
#   "name": "William Faulkner",
#   "books": [
#     {
#       "id": 124,
#       "title": "As I Lay Dying"
#     }
#   ]
# }
```

Вы также можете передать имя класса в виде строки в [Nested](../api-marshmallow/polya-fields.md#class-marshmallow.fields.nested-nested-schemaabc-or-type-or-str-or-dict-str-field-or-type-or-typing.). Это полезно, чтобы избежать _**циклического импорта**_, когда ваши схемы расположены в разных модулях.

```python
# books.py
from marshmallow import Schema, fields

class BookSchema(Schema):
    id = fields.Int(dump_only=True)
    title = fields.Str()

    author = fields.Nested("AuthorSchema", only=("id", "title"))
```

```python
# authors.py
from marshmallow import Schema, fields

class AuthorSchema(Schema):
    id = fields.Int(dump_only=True)
    title = fields.Str()

    books = fields.List(fields.Nested("BookSchema", exclude=("author",)))
```

{% hint style="info" %}
Если у вас есть несколько схем с одним и тем же именем класса, вы должны передать полный путь с указанием модуля.

```python
author = fields.Nested("authors.BookSchema", only=("id", "title"))
```
{% endhint %}

## Вложение схемы внутрь самой себя

Если объект для сортировки имеет отношение к объекту того же типа, вы можете вложить схему **Schema** внутрь себя, передав вызываемый объект, который возвращает экземпляр той же схемы.

```python
class UserSchema(Schema):
    name = fields.String()
    email = fields.Email()
    # используй аргумент 'exclude' чтобы избежать бесконечной рекурсии
    employer = fields.Nested(lambda: UserSchema(exclude=("employer",)))
    friends = fields.List(fields.Nested(lambda: UserSchema()))


user = User("Steve", "steve@example.com")
user.friends.append(User("Mike", "mike@example.com"))
user.friends.append(User("Joe", "joe@example.com"))
user.employer = User("Dirk", "dirk@example.com")
result = UserSchema().dump(user)
pprint(result, indent=2)
# {
#     "name": "Steve",
#     "email": "steve@example.com",
#     "friends": [
#         {
#             "name": "Mike",
#             "email": "mike@example.com",
#             "friends": [],
#             "employer": null
#         },
#         {
#             "name": "Joe",
#             "email": "joe@example.com",
#             "friends": [],
#             "employer": null
#         }
#     ],
#     "employer": {
#         "name": "Dirk",
#         "email": "dirk@example.com",
#         "friends": []
#     }
# }
```
