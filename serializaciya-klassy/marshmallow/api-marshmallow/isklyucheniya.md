# Исключения

Классы исключений для ошибок, связанных с **marshmallow**.

## FieldInstanceResolutionError

#### _exception_ marshmallow.exceptions.FieldInstanceResolutionError

Возникает, когда схема для создания экземпляра не является ни классом схемы, ни экземпляром.

## MarshmallowError

#### _exception_ marshmallow.exceptions.MarshmallowError

Базовый класс для всех ошибок, связанных с marshmallow.

## RegistryError

#### _exception_ marshmallow.exceptions.RegistryError

Возникает при выполнении недопустимой операции в реестре класса сериализатора.

## StringNotCollectionError

#### _exception_ marshmallow.exceptions.StringNotCollectionError

Возникает при передаче строки, когда ожидается список строк.

## ValidationError

#### _exception_ marshmallow.exceptions.ValidationError(_message: str | list | dict_, _field\_name: str = '\_schema'_, _data: Mapping\[str, Any] | Iterable\[Mapping\[str, Any]] | None = None_, _valid\_data: list\[dict\[str, Any]] | dict\[str, Any] | None = None_, _\*\*kwargs_)

Возникает при сбое проверки поля или схемы.

Валидаторы и настраиваемые поля должны вызывать это исключение.

#### Параметры:

* **message** - Сообщение об ошибке, список сообщений об ошибках или список сообщений об ошибках. Если это словарь, ключи являются подэлементами, а значения — сообщениями об ошибках.
* **field\_name** - Имя поля для сохранения ошибки. Если `None`, ошибка сохраняется как ошибка уровня схемы.
* **data** - Сырые входные данные.
* **valid\_data** - Действительные (де)сериализованные данные.
