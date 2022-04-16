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

Используйте вложенное поле Nested для представления отношения, передавая вложенную схему.
