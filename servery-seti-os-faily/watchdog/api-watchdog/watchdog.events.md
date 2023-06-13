# watchdog.events

**модуль**: _watchdog.events_

**краткий обзор**: События файловой системы и обработчики событий.

**автор**: [yesudeep@google.com](mailto:yesudeep%40google.com) (Yesudeep Mangalapilly)

**автор**: [contact@tiger-222.fr](mailto:contact%40tiger-222.fr) (Mickaël Schoentgen)

## Классы событий

### FileSystemEvent

#### _class_ watchdog.events.FileSystemEvent(_src\_path_)

\[[Исходник](https://python-watchdog.readthedocs.io/en/stable/\_modules/watchdog/events.html#FileSystemEvent)].

Bases: object

Неизменяемый тип, представляющий событие файловой системы, которое запускается, когда в отслеживаемой файловой системе происходит изменение.

Все объекты **FileSystemEvent** должны быть неизменяемыми и, следовательно, могут использоваться в качестве ключей в словарях или добавляться в наборы.

#### event\_type = None

Тип события в виде строки.

#### is\_directory = False

`True`, если событие было сгенерировано для каталога; `False` в противном случае.

#### is\_synthetic = False

`True`, если событие было синтезировано; `False` в противном случае.

Это события, которые на самом деле не транслировались ОС, но предположительно произошли на основе других реальных событий.

#### src\_path

Исходный путь к объекту файловой системы, вызвавшему это событие.

### FileSystemMovedEvent

#### _class_ watchdog.events.FileSystemMovedEvent(_src\_path_, _dest\_path_)

\[[Исходник](https://python-watchdog.readthedocs.io/en/stable/\_modules/watchdog/events.html#FileSystemMovedEvent)].

Bases: [watchdog.events.FileSystemEvent](watchdog.events.md#filesystemevent)

Событие файловой системы, представляющее любое движение файловой системы.

#### dest\_path

Путь назначения события перемещения.

### FileMovedEvent

#### _class_ watchdog.events.FileMovedEvent(_src\_path_, _dest\_path_)

\[[Исходник](https://python-watchdog.readthedocs.io/en/stable/\_modules/watchdog/events.html#FileMovedEvent)].

Bases: [watchdog.events.FileSystemMovedEvent](watchdog.events.md#filesystemmovedevent)

Событие файловой системы, представляющее перемещение файла в файловой системе.

### DirMovedEvent

#### _class_ watchdog.events.DirMovedEvent(_src\_path_, _dest\_path_)

\[[Исходник](https://python-watchdog.readthedocs.io/en/stable/\_modules/watchdog/events.html#DirMovedEvent)].

Bases: [watchdog.events.FileSystemMovedEvent](watchdog.events.md#filesystemmovedevent)

Событие файловой системы, представляющее перемещение каталога в файловой системе.

### FileModifiedEvent

#### _class_ watchdog.events.FileModifiedEvent(_src\_path_)

\[[Исходник](https://python-watchdog.readthedocs.io/en/stable/\_modules/watchdog/events.html#FileModifiedEvent)].

Bases: [watchdog.events.FileSystemEvent](watchdog.events.md#filesystemevent)

Событие файловой системы, представляющее изменение файла в файловой системе.

### DirModifiedEvent

#### _class_ watchdog.events.DirModifiedEvent(_src\_path_)

\[[Исходник](https://python-watchdog.readthedocs.io/en/stable/\_modules/watchdog/events.html#DirModifiedEvent)].

Bases: [watchdog.events.FileSystemEvent](watchdog.events.md#filesystemevent)

Событие файловой системы, представляющее изменение каталога в файловой системе.

### FileCreatedEvent

#### _class_ watchdog.events.FileCreatedEvent(_src\_path_)

\[[Исходник](https://python-watchdog.readthedocs.io/en/stable/\_modules/watchdog/events.html#FileCreatedEvent)].

Bases: [watchdog.events.FileSystemEvent](watchdog.events.md#filesystemevent)

Событие файловой системы, представляющее создание файла в файловой системе.

### FileClosedEvent

#### _class_ watchdog.events.FileClosedEvent(_src\_path_)

\[[Исходник](https://python-watchdog.readthedocs.io/en/stable/\_modules/watchdog/events.html#FileClosedEvent)].

Bases: [watchdog.events.FileSystemEvent](watchdog.events.md#filesystemevent)

Событие файловой системы, представляющее закрытие файла в файловой системе.

### DirCreatedEvent

#### _class_ watchdog.events.DirCreatedEvent(_src\_path_)

\[[Исходник](https://python-watchdog.readthedocs.io/en/stable/\_modules/watchdog/events.html#DirCreatedEvent)].

Bases: [watchdog.events.FileSystemEvent](watchdog.events.md#filesystemevent)

Событие файловой системы, представляющее создание каталога в файловой системе.

### FileDeletedEvent

#### _class_ watchdog.events.FileDeletedEvent(_src\_path_)

\[[Исходник](https://python-watchdog.readthedocs.io/en/stable/\_modules/watchdog/events.html#FileDeletedEvent)].

Bases: [watchdog.events.FileSystemEvent](watchdog.events.md#filesystemevent)

Событие файловой системы, представляющее удаление файла в файловой системе.

### DirDeletedEvent

#### _class_ watchdog.events.DirDeletedEvent(_src\_path_)

\[[Исходник](https://python-watchdog.readthedocs.io/en/stable/\_modules/watchdog/events.html#DirDeletedEvent)].

Bases: [watchdog.events.FileSystemEvent](watchdog.events.md#filesystemevent)

Событие файловой системы, представляющее удаление каталога в файловой системе.

## Классы обработчиков событий

### FileSystemEventHandler

#### _class_ watchdog.events.FileSystemEventHandler

\[[Исходник](https://python-watchdog.readthedocs.io/en/stable/\_modules/watchdog/events.html#FileSystemEventHandler)].

Bases: object

Базовый обработчик событий файловой системы, методы которого можно переопределить.

#### dispatch(_event_)

\[[Исходник](https://python-watchdog.readthedocs.io/en/stable/\_modules/watchdog/events.html#FileSystemEventHandler.dispatch)].

Отправляет события в соответствующие методы.

**Параметры**: **event**([FileSystemEvent](watchdog.events.md#filesystemevent)) - Объект события, представляющий событие файловой системы.

#### on\_any\_event(_event_)

\[[Исходник](https://python-watchdog.readthedocs.io/en/stable/\_modules/watchdog/events.html#FileSystemEventHandler.on\_any\_event)].

Универсальный обработчик событий.

**Параметры**: **event**([FileSystemEvent](watchdog.events.md#filesystemevent)) - Объект события, представляющий событие файловой системы.

#### on\_closed(_event_)

\[[Исходник](https://python-watchdog.readthedocs.io/en/stable/\_modules/watchdog/events.html#FileSystemEventHandler.on\_closed)].

Вызывается, когда файл, открытый для записи, закрывается.

**Параметры**: **event**([FileClosedEvent](watchdog.events.md#fileclosedevent)) - Событие, представляющее закрытие файла.

#### on\_created(_event_)

\[[Исходник](https://python-watchdog.readthedocs.io/en/stable/\_modules/watchdog/events.html#FileSystemEventHandler.on\_created)].

Вызывается при создании файла или каталога.

**Параметры**: **event**([DirCreatedEvent](watchdog.events.md#dircreatedevent) или [FileCreatedEvent](watchdog.events.md#filecreatedevent)) - Событие, представляющее создание файла/каталога.

#### on\_deleted(_event_)

\[[Исходник](https://python-watchdog.readthedocs.io/en/stable/\_modules/watchdog/events.html#FileSystemEventHandler.on\_deleted)].

Вызывается при удалении файла или каталога.

**Параметры**: **event**([DirDeletedEvent](watchdog.events.md#dirdeletedevent) или [FileDeletedEvent](watchdog.events.md#filedeletedevent)) - Событие, представляющее удаление файла/каталога.

#### on\_modified(_event_)

\[[Исходник](https://python-watchdog.readthedocs.io/en/stable/\_modules/watchdog/events.html#FileSystemEventHandler.on\_modified)].

Вызывается при изменении файла или каталога.

**Параметры**: **event**([DirModifiedEvent](watchdog.events.md#dirmodifiedevent) или [FileModifiedEvent](watchdog.events.md#filemodifiedevent)) - Событие, представляющее изменение файла/каталога.

#### on\_moved(_event_)

\[[Исходник](https://python-watchdog.readthedocs.io/en/stable/\_modules/watchdog/events.html#FileSystemEventHandler.on\_moved)].

Вызывается при перемещении или переименовании файла или каталога.

**Параметры**: **event**([DirMovedEvent](watchdog.events.md#dirmovedevent) или [FileMovedEvent](watchdog.events.md#filemovedevent)) - Событие, представляющее перемещение файла/каталога.

### PatternMatchingEventHandler

#### _class_ watchdog.events.PatternMatchingEventHandler(_patterns=None_, _ignore\_patterns=None_, _ignore\_directories=False_, _case\_sensitive=False_)

\[[Исходник](https://python-watchdog.readthedocs.io/en/stable/\_modules/watchdog/events.html#PatternMatchingEventHandler)].

Bases: [watchdog.events.FileSystemEventHandler](watchdog.events.md#filesystemeventhandler)

Сопоставляет заданные шаблоны с путями к файлам, связанными с происходящими событиями.

#### case\_sensitive

(Только для чтения) `True`, если имена путей должны сопоставляться с учетом регистра; `False` в противном случае.

#### dispatch(_event_)

\[[Исходник](https://python-watchdog.readthedocs.io/en/stable/\_modules/watchdog/events.html#PatternMatchingEventHandler.dispatch)].

Отправляет события в соответствующие методы.

**Параметры**: **event**([FileSystemEvent](watchdog.events.md#filesystemevent)) - Объект события, представляющий событие файловой системы.

#### ignore\_directories

(Только для чтения) `True`, если каталоги следует игнорировать; `False` в противном случае.

#### ignore\_patterns

(Только для чтения) Шаблоны для игнорирования соответствующих путей событий.

#### patterns

(Только для чтения) Шаблоны, позволяющие сопоставлять пути событий.

### RegexMatchingEventHandler

#### _class_ watchdog.events.RegexMatchingEventHandler(_regexes=None_, _ignore\_regexes=None_, _ignore\_directories=False_, _case\_sensitive=False_)

\[[Исходник](https://python-watchdog.readthedocs.io/en/stable/\_modules/watchdog/events.html#RegexMatchingEventHandler)].

Bases: [watchdog.events.FileSystemEventHandler](watchdog.events.md#filesystemeventhandler)

Сопоставляет заданные регулярные выражения с путями к файлам, связанными с происходящими событиями.

#### case\_sensitive

(Только для чтения) `True`, если имена путей должны сопоставляться с учетом регистра; `False` в противном случае.

#### dispatch(_event_)

\[[Исходник](https://python-watchdog.readthedocs.io/en/stable/\_modules/watchdog/events.html#RegexMatchingEventHandler.dispatch)].

Отправляет события в соответствующие методы.

**Параметры**: **event**([FileSystemEvent](watchdog.events.md#filesystemevent)) - Объект события, представляющий событие файловой системы.

#### ignore\_directories

(Только для чтения) `True`, если каталоги следует игнорировать; `False` в противном случае.

#### ignore\_regexes

(Только для чтения) Регулярные выражения для игнорирования соответствующих путей событий.

#### regexes

(Только для чтения) Регулярные выражения, позволяющие сопоставлять пути событий.

### LoggingEventHandler

#### _class_ watchdog.events.LoggingEventHandler(_logger=None_)

\[[Исходник](https://python-watchdog.readthedocs.io/en/stable/\_modules/watchdog/events.html#LoggingEventHandler)].

Записывает все захваченные события.

#### on\_created(_event_)

\[[Исходник](https://python-watchdog.readthedocs.io/en/stable/\_modules/watchdog/events.html#LoggingEventHandler.on\_created)].

Вызывается при создании файла или каталога.

**Параметры**: **event**([DirCreatedEvent](watchdog.events.md#dircreatedevent) или [FileCreatedEvent](watchdog.events.md#filecreatedevent)) - Событие, представляющее создание файла/каталога.

#### on\_deleted(_event_)

\[[Исходник](https://python-watchdog.readthedocs.io/en/stable/\_modules/watchdog/events.html#LoggingEventHandler.on\_deleted)].

Вызывается при удалении файла или каталога.

**Параметры**: **event**([DirDeletedEvent](watchdog.events.md#dirdeletedevent) или [FileDeletedEvent](watchdog.events.md#filedeletedevent)) - Событие, представляющее удаление файла/каталога.

#### on\_modified(_event_)

\[[Исходник](https://python-watchdog.readthedocs.io/en/stable/\_modules/watchdog/events.html#LoggingEventHandler.on\_modified)].

Вызывается при изменении файла или каталога.

**Параметры**: **event**([DirModifiedEvent](watchdog.events.md#dirmodifiedevent) или [FileModifiedEvent](watchdog.events.md#filemodifiedevent)) - Событие, представляющее изменение файла/каталога.

#### on\_moved(_event_)

\[[Исходник](https://python-watchdog.readthedocs.io/en/stable/\_modules/watchdog/events.html#LoggingEventHandler.on\_moved)].

Вызывается при перемещении или переименовании файла или каталога.

**Параметры**: **event**([DirMovedEvent](watchdog.events.md#dirmovedevent) или [FileMovedEvent](watchdog.events.md#filemovedevent)) - Событие, представляющее перемещение файла/каталога.
