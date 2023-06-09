# Хранилище ошибок

Утилиты для хранения коллекций сообщений об ошибках.

{% hint style="warning" %}
Этот модуль рассматривается как внутренний API. Пользователям не нужно использовать этот модуль напрямую.
{% endhint %}

## merge\_errors()

#### marshmallow.error\_store.merge\_errors(_errors1_, _errors2_)

Глубоко объединить два сообщения об ошибках.

Формат **error1** и **error2** соответствует параметру сообщения [marshmallow.exceptions.ValidationError](isklyucheniya.md#exception-marshmallow.exceptions.validationerror-message-str-or-list-or-dict-field\_name-str-\_schema).
