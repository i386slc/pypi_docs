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
