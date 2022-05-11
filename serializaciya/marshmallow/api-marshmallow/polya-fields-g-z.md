# Поля Fields (G-Z)

Классы полей для различных типов данных.

## Классы:

| Класс                                                | Описание                                                                                                                                                              |
| ---------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| AwareDateTime(\[format, default\_timezone])          | Отформатированная осведомленная строка даты и времени.                                                                                                                |
| Bool                                                 | псевдоним <mark style="color:red;">marshmallow.fields.Boolean</mark>                                                                                                  |
| Boolean(\*\[, truthy, falsy])                        | Логическое поле                                                                                                                                                       |
| Constant(constant, \*\*kwargs)                       | Поле, которое (де)сериализуется в предустановленную константу.                                                                                                        |
| Date(\[format])                                      | Строка даты в формате ISO8601.                                                                                                                                        |
| DateTime(\[format])                                  | Отформатированная строка даты и времени.                                                                                                                              |
| Decimal(\[places, rounding, allow\_nan, as\_string]) | Поле, которое (де)сериализуется в тип Python **decimal.Decimal**.                                                                                                     |
| Dict(\[keys, values])                                | Поле словаря                                                                                                                                                          |
| Email(\*args, \*\*kwargs)                            | Поле электронной почты                                                                                                                                                |
| Field(\*, load\_default, missing, ...)               | Основное поле, из которого должны расширяться другие поля                                                                                                             |
| Float(\*\[, allow\_nan, as\_string])                 | **Double** как строка двойной точности IEEE-754                                                                                                                       |
| Function(\[serialize, deserialize])                  | Поле, принимающее значение, возвращенное функцией                                                                                                                     |
| IP(\*args\[, exploded])                              | Поле IP-адреса                                                                                                                                                        |
| IPInterface(\*args\[, exploded])                     | Поле IP-интерфейса                                                                                                                                                    |
| IPv4(\*args\[, exploded])                            | Поле адреса IPv4                                                                                                                                                      |
| IPv4Interface(\*args\[, exploded])                   | Поле сетевого интерфейса IPv4                                                                                                                                         |
| IPv6(\*args\[, exploded])                            | Поле адреса IPv6                                                                                                                                                      |
| IPv6Interface(\*args\[, exploded])                   | Поле сетевого интерфейса IPv6                                                                                                                                         |
| Int                                                  | псевдоним <mark style="color:red;">marshmallow.fields.Intege</mark>r                                                                                                  |
| Integer(\*\[, strict])                               | Целочисленное поле                                                                                                                                                    |
| List(cls\_or\_instance, \*\*kwargs)                  | Поле списка, составленное из другого класса или экземпляра <mark style="color:red;">Field</mark>                                                                      |
| Mapping(\[keys, values])                             | Абстрактный класс для объектов с парами ключ-значение                                                                                                                 |
| Method(\[serialize, deserialize])                    | Поле, которое принимает значение, возвращаемое методом схемы                                                                                                          |
| NaiveDateTime(\[format, timezone])                   | Отформатированная наивная строка даты и времени.                                                                                                                      |
| Nested(nested, ...)                                  | Позволяет вложить [Schema](skhema-schema.md#class-marshmallow.schema) внутрь поля.                                                                                    |
| Number(\*\[, as\_string])                            | Базовый класс для числовых полей.                                                                                                                                     |
| Pluck(nested, field\_name, \*\*kwargs)               | Позволяет заменить вложенные данные одним из полей данных.                                                                                                            |
| Raw(\*, load\_default, missing, dump\_default, ...)  | Поле, к которому не применяется форматирование.                                                                                                                       |
| Str                                                  | псевдоним <mark style="color:red;">marshmallow.fields.String</mark>                                                                                                   |
| String(\*, load\_default, missing, ...)              | Строковое поле.                                                                                                                                                       |
| Time(\[format])                                      | Отформатированная строка времени.                                                                                                                                     |
| TimeDelta(\[precision])                              | Поле, которое (де)сериализует объект [datetime.timedelta](https://python.readthedocs.io/en/latest/library/datetime.html#datetime.timedelta) в целое число и наоборот. |
| Tuple(tuple\_fields, \*args, \*\*kwargs)             | Поле кортежа, состоящее из фиксированного числа других классов или экземпляров Field                                                                                  |
| URL                                                  | псевдоним <mark style="color:red;">marshmallow.fields.Url</mark>                                                                                                      |
| UUID(\*, load\_default, missing, dump\_default, ...) | Поле UUID.                                                                                                                                                            |
| Url(\*\[, relative, schemes, require\_tld])          | Поле URL.                                                                                                                                                             |

## IP
