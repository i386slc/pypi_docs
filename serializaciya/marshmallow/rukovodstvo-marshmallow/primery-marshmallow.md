# Примеры marshmallow

## Проверка package.json

**marshmallow** можно использовать для проверки конфигурации в соответствии со схемой. Ниже приведена схема, которую можно использовать для проверки файлов **package.json**. Этот пример демонстрирует следующие возможности:

* Проверка и десериализация с использованием <mark style="color:red;">Schema.load()</mark>
* [Пользовательские поля](polzovatelskie-polya.md)
* Указание ключей десериализации с помощью **data\_key**
* Включение неизвестных ключей с использованием `unknown = INCLUDE`

```python
import sys
import json
from packaging import version
from pprint import pprint

from marshmallow import Schema, fields, INCLUDE, ValidationError

class Version(fields.Field):
    """Поле Version, которое десериализуется в объект Version."""

    def _deserialize(self, value, *args, **kwargs):
        try:
            return version.Version(value)
        except version.InvalidVersion as e:
            raise ValidationError("Not a valid version.") from e

    def _serialize(self, value, *args, **kwargs):
        return str(value)

class PackageSchema(Schema):
    name = fields.Str(required=True)
    version = Version(required=True)
    description = fields.Str(required=True)
    main = fields.Str(required=False)
    homepage = fields.URL(required=False)
    scripts = fields.Dict(keys=fields.Str(), values=fields.Str())
    license = fields.Str(required=True)
    dependencies = fields.Dict(
        keys=fields.Str(),
        values=fields.Str(),
        required=False
    )
    dev_dependencies = fields.Dict(
        keys=fields.Str(),
        values=fields.Str(),
        required=False,
        data_key="devDependencies",
    )

    class Meta:
        # Включить неизвестные поля в десериализованный вывод
        unknown = INCLUDE

if __name__ == "__main__":
    pkg = json.load(sys.stdin)
    try:
        pprint(PackageSchema().load(pkg))
    except ValidationError as error:
        print("ERROR: package.json is invalid")
        pprint(error.messages)
        sys.exit(1)
```

Учитывая следующий файл **package.json**…

```json
{
  "name": "dunderscore",
  "version": "1.2.3",
  "description": "The Pythonic JavaScript toolkit",
  "devDependencies": {
    "pest": "^23.4.1"
  },
  "main": "index.js",
  "scripts": {
    "test": "pest"
  },
  "license": "MIT"
}
```

Мы можем проверить это, используя приведенный выше скрипт.

```bash
$ python examples/package_json_example.py < package.json
{'description': 'The Pythonic JavaScript toolkit',
'dev_dependencies': {'pest': '^23.4.1'},
'license': 'MIT',
'main': 'index.js',
'name': 'dunderscore',
'scripts': {'test': 'pest'},
'version': <Version('1.2.3')>}
```

Обратите внимание, что наше пользовательское поле десериализовало строку версии в объект версии **Version**.

Но если мы передаем недопустимый файл **package.json**…

```json
{
  "name": "dunderscore",
  "version": "INVALID",
  "homepage": "INVALID",
  "description": "The Pythonic JavaScript toolkit",
  "license": "MIT"
}
```

Видим соответствующие сообщения об ошибках.

```bash
$ python examples/package_json_example.py < invalid_package.json
ERROR: package.json is invalid
{'homepage': ['Not a valid URL.'], 'version': ['Not a valid version.']}
```

## API анализа текста (Bottle+ TextBlob)

Вот очень простой API анализа текста с использованием [Bottle](https://bottlepy.org) и [TextBlob](https://textblob.readthedocs.io/en/dev/), который демонстрирует, как объявить сериализатор объекта.

Предположим, что объекты **TextBlob** имеют свойства **polarity**, **subjectivity**, **noun\_phrase**, **tags** и **words**.

```python
from bottle import route, request, run
from textblob import TextBlob
from marshmallow import Schema, fields

class BlobSchema(Schema):
    polarity = fields.Float()
    subjectivity = fields.Float()
    chunks = fields.List(fields.String, attribute="noun_phrases")
    tags = fields.Raw()
    discrete_sentiment = fields.Method("get_discrete_sentiment")
    word_count = fields.Function(lambda obj: len(obj.words))

    def get_discrete_sentiment(self, obj):
        if obj.polarity > 0.1:
            return "positive"
        elif obj.polarity < -0.1:
            return "negative"
        else:
            return "neutral"

blob_schema = BlobSchema()

@route("/api/v1/analyze", method="POST")
def analyze():
    blob = TextBlob(request.json["text"])
    return blob_schema.dump(blob)

run(reloader=True, port=5000)
```

#### Использование API

Сначала запустите приложение.

```bash
$ python examples/textblob_example.py
```

Затем отправьте запрос **POST** с текстом с помощью [httpie](https://github.com/jkbr/httpie) (инструмент, похожий на **curl**) для тестирования API.

```bash
$ pip install httpie
$ http POST :5000/api/v1/analyze text="Simple is better"
HTTP/1.0 200 OK
Content-Length: 189
Content-Type: application/json
Date: Wed, 13 Nov 2013 08:58:40 GMT
Server: WSGIServer/0.1 Python/2.7.5

{
    "chunks": [
        "simple"
    ],
    "discrete_sentiment": "positive",
    "polarity": 0.25,
    "subjectivity": 0.4285714285714286,
    "tags": [
        [
            "Simple",
            "NN"
        ],
        [
            "is",
            "VBZ"
        ],
        [
            "better",
            "JJR"
        ]
    ],
    "word_count": 3
}
```

## API цитат (Flask + SQLAlchemy)

Ниже приведен полный пример REST API для приложения цитат, использующего [Flask](http://flask.pocoo.org) и [SQLAlchemy](https://www.sqlalchemy.org) с **marshmallow**. Он демонстрирует ряд особенностей, в том числе:

* Пользовательская проверка
* Вложенные поля
* Использование `dump_only=True` для указания полей только для чтения
* Фильтрация вывода по параметру **only**
* Использование [@pre\_load](../api-marshmallow/dekoratory.md#marshmallow.decorators.pre\_load-fn-callable-...-any-or-none-none-pass\_many-bool-false-callable-...-a) для предварительной обработки входных данных.

```python
import datetime

from flask import Flask, request
from flask_sqlalchemy import SQLAlchemy
from sqlalchemy.exc import NoResultFound
from marshmallow import Schema, fields, ValidationError, pre_load

app = Flask(__name__)
app.config["SQLALCHEMY_DATABASE_URI"] = "sqlite:////tmp/quotes.db"
db = SQLAlchemy(app)

##### MODELS #####

class Author(db.Model):  # type: ignore
    id = db.Column(db.Integer, primary_key=True)
    first = db.Column(db.String(80))
    last = db.Column(db.String(80))

class Quote(db.Model):  # type: ignore
    id = db.Column(db.Integer, primary_key=True)
    content = db.Column(db.String, nullable=False)
    author_id = db.Column(db.Integer, db.ForeignKey("author.id"))
    author = db.relationship("Author", backref=db.backref("quotes", lazy="dynamic"))
    posted_at = db.Column(db.DateTime)

##### SCHEMAS #####

class AuthorSchema(Schema):
    id = fields.Int(dump_only=True)
    first = fields.Str()
    last = fields.Str()
    formatted_name = fields.Method("format_name", dump_only=True)

    def format_name(self, author):
        return f"{author.last}, {author.first}"

# Пользовательский валидатор
def must_not_be_blank(data):
    if not data:
        raise ValidationError("Data not provided.")

class QuoteSchema(Schema):
    id = fields.Int(dump_only=True)
    author = fields.Nested(AuthorSchema, validate=must_not_be_blank)
    content = fields.Str(required=True, validate=must_not_be_blank)
    posted_at = fields.DateTime(dump_only=True)

    # Разрешает клиенту передавать полное имя автора в теле запроса
    # например {"author': 'Tim Peters"} вместо {"first": "Tim", "last": "Peters"}
    @pre_load
    def process_author(self, data, **kwargs):
        author_name = data.get("author")
        if author_name:
            first, last = author_name.split(" ")
            author_dict = dict(first=first, last=last)
        else:
            author_dict = {}
        data["author"] = author_dict
        return data

author_schema = AuthorSchema()
authors_schema = AuthorSchema(many=True)
quote_schema = QuoteSchema()
quotes_schema = QuoteSchema(many=True, only=("id", "content"))

##### API #####

@app.route("/authors")
def get_authors():
    authors = Author.query.all()
    # Сериализация набора запросов
    result = authors_schema.dump(authors)
    return {"authors": result}

@app.route("/authors/<int:pk>")
def get_author(pk):
    try:
        author = Author.query.filter(Author.id == pk).one()
    except NoResultFound:
        return {"message": "Author could not be found."}, 400
    author_result = author_schema.dump(author)
    quotes_result = quotes_schema.dump(author.quotes.all())
    return {"author": author_result, "quotes": quotes_result}

@app.route("/quotes/", methods=["GET"])
def get_quotes():
    quotes = Quote.query.all()
    result = quotes_schema.dump(quotes, many=True)
    return {"quotes": result}

@app.route("/quotes/<int:pk>")
def get_quote(pk):
    try:
        quote = Quote.query.filter(Quote.id == pk).one()
    except NoResultFound:
        return {"message": "Quote could not be found."}, 400
    result = quote_schema.dump(quote)
    return {"quote": result}

@app.route("/quotes/", methods=["POST"])
def new_quote():
    json_data = request.get_json()
    if not json_data:
        return {"message": "No input data provided"}, 400
    # Проверка и десериализация ввода
    try:
        data = quote_schema.load(json_data)
    except ValidationError as err:
        return err.messages, 422
    first, last = data["author"]["first"], data["author"]["last"]
    author = Author.query.filter_by(first=first, last=last).first()
    if author is None:
        # Создать нового автора
        author = Author(first=first, last=last)
        db.session.add(author)
    # Создать новую цитату
    quote = Quote(
        content=data["content"], author=author, posted_at=datetime.datetime.utcnow()
    )
    db.session.add(quote)
    db.session.commit()
    result = quote_schema.dump(Quote.query.get(quote.id))
    return {"message": "Created new quote.", "quote": result}

if __name__ == "__main__":
    db.create_all()
    app.run(debug=True, port=5000)
```

#### Использование API

Запуск приложения.

```bash
$ pip install flask flask-sqlalchemy
$ python examples/flask_example.py
```

Сначала мы опубликуем несколько цитат (POST запрос).

```bash
$ pip install httpie
$ http POST :5000/quotes/ author="Tim Peters" content="Beautiful is better than ugly."
$ http POST :5000/quotes/ author="Tim Peters" content="Now is better than never."
$ http POST :5000/quotes/ author="Peter Hintjens" content="Simplicity is always better than functionality."
```

Если мы предоставим неверные входные данные, мы получим ответ с ошибкой 400. Опустим `«author»` во входных данных.

```bash
$ http POST :5000/quotes/ content="I have no author"
{
    "author": [
        "Data not provided."
    ]
}
```

Теперь мы можем получить список всех цитат (GET запрос).

```bash
$ http :5000/quotes/
{
    "quotes": [
        {
            "content": "Beautiful is better than ugly.",
            "id": 1
        },
        {
            "content": "Now is better than never.",
            "id": 2
        },
        {
            "content": "Simplicity is always better than functionality.",
            "id": 3
        }
    ]
}
```

Мы также можем получить цитаты для одного автора (GET запрос).

```bash
$ http :5000/authors/1
{
    "author": {
        "first": "Tim",
        "formatted_name": "Peters, Tim",
        "id": 1,
        "last": "Peters"
    },
    "quotes": [
        {
            "content": "Beautiful is better than ugly.",
            "id": 1
        },
        {
            "content": "Now is better than never.",
            "id": 2
        }
    ]
}
```

## API ToDo (Flask + Peewee)

В этом примере **Flask** и [Peewee](https://peewee.readthedocs.io/en/latest/index.html) ORM используются для создания простого приложения **Todo**.

Здесь мы используем <mark style="color:red;">Schema.load</mark> для проверки и десериализации входных данных для данных модели. Также обратите внимание, как [pre\_load](../api-marshmallow/dekoratory.md#marshmallow.decorators.pre\_load-fn-callable-...-any-or-none-none-pass\_many-bool-false-callable-...-a) используется для очистки входных данных, а [post\_load](../api-marshmallow/dekoratory.md#marshmallow.decorators.post\_load-fn-callable-...-any-or-none-none-pass\_many-bool-false-pass\_original) используется для добавления конвертации к данным ответа.

```python
import datetime as dt
from functools import wraps

from flask import Flask, request, g, jsonify
import peewee as pw
from marshmallow import (
    Schema,
    fields,
    validate,
    pre_load,
    post_dump,
    post_load,
    ValidationError,
)

app = Flask(__name__)
db = pw.SqliteDatabase("/tmp/todo.db")

###### MODELS #####

class BaseModel(pw.Model):
    """Класс базовой модели. Все потомки используют одну и ту же базу данных."""

    class Meta:
        database = db

class User(BaseModel):
    email = pw.CharField(max_length=80, unique=True)
    password = pw.CharField()
    joined_on = pw.DateTimeField()

class Todo(BaseModel):
    content = pw.TextField()
    is_done = pw.BooleanField(default=False)
    user = pw.ForeignKeyField(User)
    posted_on = pw.DateTimeField()

    class Meta:
        order_by = ("-posted_on",)

def create_tables():
    db.connect()
    User.create_table(True)
    Todo.create_table(True)

##### SCHEMAS #####

class UserSchema(Schema):
    id = fields.Int(dump_only=True)
    email = fields.Str(
        required=True, validate=validate.Email(error="Not a valid email address")
    )
    password = fields.Str(
        required=True, validate=[validate.Length(min=6, max=36)], load_only=True
    )
    joined_on = fields.DateTime(dump_only=True)

    # Очистить данные
    @pre_load
    def process_input(self, data, **kwargs):
        data["email"] = data["email"].lower().strip()
        return data

    # Добавляем хук post_dump для добавления конвертации к ответам
    @post_dump(pass_many=True)
    def wrap(self, data, many, **kwargs):
        key = "users" if many else "user"
        return {key: data}

class TodoSchema(Schema):
    id = fields.Int(dump_only=True)
    done = fields.Boolean(attribute="is_done", missing=False)
    user = fields.Nested(UserSchema(
        exclude=("joined_on", "password")
    ), dump_only=True)
    content = fields.Str(required=True)
    posted_on = fields.DateTime(dump_only=True)

    # Опять добавляем конвертацию к ответам
    @post_dump(pass_many=True)
    def wrap(self, data, many, **kwargs):
        key = "todos" if many else "todo"
        return {key: data}

    # Мы используем make_object для создания нового Todo из проверенных данных.
    @post_load
    def make_object(self, data, **kwargs):
        if not data:
            return None
        return Todo(
            content=data["content"],
            is_done=data["is_done"],
            posted_on=dt.datetime.utcnow(),
        )

user_schema = UserSchema()
todo_schema = TodoSchema()
todos_schema = TodoSchema(many=True)

###### HELPERS ######

def check_auth(email, password):
    """Проверьте, действительна ли комбинация имени пользователя и пароля."""
    try:
        user = User.get(User.email == email)
    except User.DoesNotExist:
        return False
    return password == user.password

def requires_auth(f):
    @wraps(f)
    def decorated(*args, **kwargs):
        auth = request.authorization
        if not auth or not check_auth(auth.username, auth.password):
            resp = jsonify({"message": "Please authenticate."})
            resp.status_code = 401
            resp.headers["WWW-Authenticate"] = 'Basic realm="Example"'
            return resp
        kwargs["user"] = User.get(User.email == auth.username)
        return f(*args, **kwargs)

    return decorated

# Обеспечьте отдельное соединение для каждого потока
@app.before_request
def before_request():
    g.db = db
    g.db.connect()

@app.after_request
def after_request(response):
    g.db.close()
    return response

#### API #####

@app.route("/register", methods=["POST"])
def register():
    json_input = request.get_json()
    try:
        data = user_schema.load(json_input)
    except ValidationError as err:
        return {"errors": err.messages}, 422
    try:  # Используйте get, чтобы увидеть, существует ли пользователь
        User.get(User.email == data["email"])
    except User.DoesNotExist:
        user = User.create(
            email=data["email"], joined_on=dt.datetime.now(), password=data["password"]
        )
        message = f"Successfully created user: {user.email}"
    else:
        return {"errors": "That email address is already in the database"}, 400

    data = user_schema.dump(user)
    data["message"] = message
    return data, 201

@app.route("/todos/", methods=["GET"])
def get_todos():
    todos = Todo.select().order_by(Todo.posted_on.asc())  # Получить все задачи
    return todos_schema.dump(list(todos))

@app.route("/todos/<int:pk>")
def get_todo(pk):
    todo = Todo.get(Todo.id == pk)
    if not todo:
        return {"errors": "Todo could not be find"}, 404
    return todo_schema.dump(todo)

@app.route("/todos/<int:pk>/toggle", methods=["POST", "PUT"])
def toggledone(pk):
    try:
        todo = Todo.get(Todo.id == pk)
    except Todo.DoesNotExist:
        return {"message": "Todo could not be found"}, 404
    status = not todo.is_done
    update_query = todo.update(is_done=status)
    update_query.execute()
    return todo_schema.dump(todo)

@app.route("/todos/", methods=["POST"])
@requires_auth
def new_todo(user):
    json_input = request.get_json()
    try:
        todo = todo_schema.load(json_input)
    except ValidationError as err:
        return {"errors": err.messages}, 422
    todo.user = user
    todo.save()
    return todo_schema.dump(todo)

if __name__ == "__main__":
    create_tables()
    app.run(port=5000, debug=True)
```

#### Использование API

Запуск приложения.

```bash
$ pip install flask peewee
$ python examples/peewee_example.py
```

После регистрации пользователя и создания некоторых задач в базе данных, вот пример ответа.

```python
$ pip install httpie
$ http GET :5000/todos/
{
    "todos": [
        {
            "content": "Install marshmallow",
            "done": false,
            "id": 1,
            "posted_on": "2015-05-05T01:51:12.832232+00:00",
            "user": {
                "user": {
                    "email": "foo@bar.com",
                    "id": 1
                }
            }
        },
        {
            "content": "Learn Python",
            "done": false,
            "id": 2,
            "posted_on": "2015-05-05T01:51:20.728052+00:00",
            "user": {
                "user": {
                    "email": "foo@bar.com",
                    "id": 1
                }
            }
        },
        {
            "content": "Refactor everything",
            "done": false,
            "id": 3,
            "posted_on": "2015-05-05T01:51:25.970153+00:00",
            "user": {
                "user": {
                    "email": "foo@bar.com",
                    "id": 1
                }
            }
        }
    ]
}
```

## Изменение формы слова (ключи в верблюжьем стиле)

API-интерфейсы HTTP часто используют ключи в верблюжьем стиле для своих входных и выходных представлений. В этом примере показано, как вы можете использовать хук <mark style="color:red;">Schema.on\_bind\_field</mark> для автоматического изменения ключей.

```python
from marshmallow import Schema, fields

def camelcase(s):
    parts = iter(s.split("_"))
    return next(parts) + "".join(i.title() for i in parts)

class CamelCaseSchema(Schema):
    """Схема, использующая верблюжий регистр для внешнего представления
    и змеиный регистр для внутреннего представления.
    """

    def on_bind_field(self, field_name, field_obj):
        field_obj.data_key = camelcase(field_obj.data_key or field_name)

# -----------------------------------------------------------------------------

class UserSchema(CamelCaseSchema):
    first_name = fields.Str(required=True)
    last_name = fields.Str(required=True)

schema = UserSchema()
loaded = schema.load({"firstName": "David", "lastName": "Bowie"})
print(loaded)  # => {'last_name': 'Bowie', 'first_name': 'David'}
dumped = schema.dump(loaded)
print(dumped)  # => {'lastName': 'Bowie', 'firstName': 'David'}
```
