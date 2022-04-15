# marshmallow

## Marshmallow: упрощенная сериализация объектов

Выпуск v3.15.0. ([Журнал изменений](informaciya-o-proekte/zhurnal-izmenenii.md))

**marshmallow** — это ORM/ODM/независимая от фреймворка библиотека для преобразования сложных типов данных, таких как объекты, в собственные типы данных Python и обратно.

```python
from datetime import date
from pprint import pprint

from marshmallow import Schema, fields


class ArtistSchema(Schema):
    name = fields.Str()


class AlbumSchema(Schema):
    title = fields.Str()
    release_date = fields.Date()
    artist = fields.Nested(ArtistSchema())


bowie = dict(name="David Bowie")
album = dict(artist=bowie, title="Hunky Dory", release_date=date(1971, 12, 17))

schema = AlbumSchema()
result = schema.dump(album)
pprint(result, indent=2)
# { 'artist': {'name': 'David Bowie'},
#   'release_date': '1971-12-17',
#   'title': 'Hunky Dory'}
```

Короче говоря, схемы **marshmallow** можно использовать для:

* **Проверки** входных данных
* **Десериализации** входных данных в объекты уровня приложения
* **Сериализации** объектов уровня приложения в примитивные типы Python. Затем сериализованные объекты можно преобразовать в стандартные форматы, такие как JSON, для использования в HTTP API.

## Получить сейчас

```bash
$ pip install -U marshmallow
```

Готовы начать? Перейдите к [учебнику Quickstart](rukovodstvo-marshmallow/) или ознакомьтесь с некоторыми [примерами](rukovodstvo-marshmallow/primery-marshmallow.md).

## Обновление с более старой версии?

См. страницу «[Обновление до более новых версий](informaciya-o-proekte/obnovlenie-do-bolee-novykh-versii.md)», чтобы узнать, как обновить код до последней версии.

## Зачем еще одна библиотека?

См. [этот документ](informaciya-o-proekte/pochemu-marshmallow.md), чтобы узнать, что делает **marshmallow** уникальным.

## Оглавление
