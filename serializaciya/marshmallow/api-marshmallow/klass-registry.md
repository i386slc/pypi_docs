# Класс Registry

Реестр классов схемы [Schema](skhema-schema.md#class-marshmallow.schema). Это позволяет выполнять строковый поиск схем, который можно использовать с классом: [fields.Nested](polya-fields-g-z.md#nested).

{% hint style="warning" %}
Этот модуль рассматривается как внутренний API. Пользователям не нужно использовать этот модуль напрямую.
{% endhint %}

## get\_class()

#### marshmallow.class\_registry.get\_class(_classname: str_, _all: bool = False_) → list\[SchemaType] | SchemaType

Получить класс из реестра.

**Поднимает**: `marshmallow.exceptions.RegistryError`, если класс не может быть найден или если для данного имени класса имеется несколько записей.

## register()

#### marshmallow.class\_registry.register(_classname: str_, _cls: SchemaType_) → None

Добавляет класс в реестр классов сериализаторов. Когда класс регистрируется, в реестр добавляется запись как для его имени класса, так и для его полного пути с указанием модуля.

Пример:

```python
class MyClass:
    pass

register('MyClass', MyClass)
# Registry:
# {
#   'MyClass': [path.to.MyClass],
#   'path.to.MyClass': [path.to.MyClass],
# }
```
