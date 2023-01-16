# Пользовательское приложение

_Новое в версии 19.0_.

Иногда вы хотите интегрировать **Gunicorn** с вашим приложением **WSGI**. В этом случае вы можете наследовать от **gunicorn.app.base.BaseApplication**.

Вот небольшой пример, где мы создаем очень маленькое приложение **WSGI** и загружаем его с помощью пользовательского приложения:

```python
import multiprocessing
import gunicorn.app.base

def number_of_workers():
    return (multiprocessing.cpu_count() * 2) + 1

def handler_app(environ, start_response):
    response_body = b'Works fine'
    status = '200 OK'
    response_headers = [
        ('Content-Type', 'text/plain'),
    ]
    start_response(status, response_headers)
    return [response_body]

class StandaloneApplication(gunicorn.app.base.BaseApplication):

    def __init__(self, app, options=None):
        self.options = options or {}
        self.application = app
        super().__init__()

    def load_config(self):
        config = {key: value for key, value in self.options.items()
                  if key in self.cfg.settings and value is not None}
        for key, value in config.items():
            self.cfg.set(key.lower(), value)

    def load(self):
        return self.application


if __name__ == '__main__':
    options = {
        'bind': '%s:%s' % ('127.0.0.1', '8080'),
        'workers': number_of_workers(),
    }
    StandaloneApplication(handler_app, options).run()
```

## Прямое использование существующих приложений WSGI

При необходимости вы можете запустить **Gunicorn** прямо из Python, что позволит вам указать WSGI-совместимое приложение во время выполнения. Это может быть удобно для последовательного развертывания или в случае использования файлов **PEX** для развертывания вашего приложения, поскольку приложение и **Gunicorn** могут быть объединены в один и тот же файл **PEX**. **Gunicorn** имеет эту встроенную функцию как гражданин первого класса, известную как **gunicorn.app.wsgiapp**. Это можно использовать для запуска экземпляров приложений, совместимых с **WSGI**, например, созданных **Flask** или **Django**. Предполагая, что ваш пакет **API WSGI** — это **exampleapi**, а экземпляр вашего приложения — **app**, это все, что вам нужно для начала работы:

```bash
gunicorn.app.wsgiapp exampleapi:app
```

Эта команда будет работать с любыми параметрами командной строки **Gunicorn** или файлом конфигурации — просто передайте их, как если бы вы напрямую отдавали их **Gunicorn**:

```bash
# Пользовательские параметры
$ python gunicorn.app.wsgiapp exampleapi:app --bind=0.0.0.0:8081 --workers=4
# Использование файла конфигурации
$ python gunicorn.app.wsgiapp exampleapi:app -c config.py
```

Примечание для тех, кто использует **PEX**: используйте `-c gunicorn` в качестве записи во время сборки, и ваше скомпилированное приложение должно работать с точкой входа, переданной ему во время выполнения.

```bash
# Общая команда сборки pex через bash из корня проекта exampleapi
$ pex . -v -c gunicorn -o compiledapp.pex
# Его запуск
./compiledapp.pex exampleapi:app -c gunicorn_config.py
```
