# Быстрый старт watchdog

Ниже мы представляем простой пример, который рекурсивно отслеживает текущий каталог (что означает, что он будет проходить через любые подкаталоги) для обнаружения изменений. Вот что мы будем делать с API:

* Создайте экземпляр класса потока watchdog.observers.Observer.
* Реализуйте подкласс watchdog.events.FileSystemEventHandler (или, как в нашем случае, мы будем использовать встроенный watchdog.events.LoggingEventHandler, который уже используется).
* Запланируйте мониторинг нескольких путей с экземпляром наблюдателя, присоединяющим обработчик событий.
* Запустите поток наблюдателя и подождите, пока он сгенерирует события, не блокируя наш основной поток.

По умолчанию экземпляр watchdog.observers.Observer не будет отслеживать подкаталоги. Путем передачи `recursive=True` в вызове `watchdog.observers.Observer.schedule()` обеспечивается мониторинг всего дерева каталогов.

## Простой пример

Следующий пример программы будет рекурсивно отслеживать изменения файловой системы в текущем каталоге и просто выводить их на консоль:

```python
import sys
import logging
from watchdog.observers import Observer
from watchdog.events import LoggingEventHandler

if __name__ == "__main__":
    logging.basicConfig(level=logging.INFO,
                        format='%(asctime)s - %(message)s',
                        datefmt='%Y-%m-%d %H:%M:%S')
    path = sys.argv[1] if len(sys.argv) > 1 else '.'
    event_handler = LoggingEventHandler()
    observer = Observer()
    observer.schedule(event_handler, path, recursive=True)
    observer.start()
    try:
        while observer.isAlive():
            observer.join(1)
    finally:
        observer.stop()
        observer.join()
```

Чтобы остановить программу, нажмите **Control-C**.
