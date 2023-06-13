# watchdog.tricks

**модуль**: _watchdog.tricks_

**краткий обзор**: Обработчики служебных событий.

**автор**: [yesudeep@google.com](mailto:yesudeep%40google.com) (Yesudeep Mangalapilly)

**автор**: [contact@tiger-222.fr](mailto:contact%40tiger-222.fr) (Mickaël Schoentgen)

## Классы

### Trick

#### _class_ watchdog.tricks.Trick(_patterns=None_, _ignore\_patterns=None_, _ignore\_directories=False_, _case\_sensitive=False_)

\[[Исходник](https://python-watchdog.readthedocs.io/en/stable/\_modules/watchdog/tricks.html#Trick)].

**Bases**: [watchdog.events.PatternMatchingEventHandler](watchdog.events.md#patternmatchingeventhandler)

Ваши трюки должны быть подклассами этого класса.

### LoggerTrick

#### _class_ watchdog.tricks.LoggerTrick(_patterns=None_, _ignore\_patterns=None_, _ignore\_directories=False_, _case\_sensitive=False_)

\[[Исходник](https://python-watchdog.readthedocs.io/en/stable/\_modules/watchdog/tricks.html#LoggerTrick)].

**Bases**: [watchdog.tricks.Trick](watchdog.tricks.md#trick)

Простой трюк, который регистрирует только события.

#### on\_any\_event(_event_)

\[[Исходник](https://python-watchdog.readthedocs.io/en/stable/\_modules/watchdog/tricks.html#LoggerTrick.on\_any\_event)].

Универсальный обработчик событий.

**Параметры**: **event**([FileSystemEvent](watchdog.tricks.md#filesystemevent)) - Объект события, представляющий событие файловой системы.

#### on\_created(_event_)

\[[Исходник](https://python-watchdog.readthedocs.io/en/stable/\_modules/watchdog/tricks.html#LoggerTrick.on\_created)].

Вызывается при создании файла или каталога.

**Параметры**: **event**([DirCreatedEvent](watchdog.tricks.md#dircreatedevent) или [FileCreatedEvent](watchdog.tricks.md#filecreatedevent)) - Событие, представляющее создание файла/каталога.

#### on\_deleted(_event_)

\[[Исходник](https://python-watchdog.readthedocs.io/en/stable/\_modules/watchdog/tricks.html#LoggerTrick.on\_deleted)].

Вызывается при удалении файла или каталога.

**Параметры**: **event**([DirDeletedEvent](watchdog.tricks.md#dirdeletedevent) или [FileDeletedEvent](watchdog.tricks.md#filedeletedevent)) - Событие, представляющее удаление файла/каталога.

#### on\_modified(_event_)

\[[Исходник](https://python-watchdog.readthedocs.io/en/stable/\_modules/watchdog/tricks.html#LoggerTrick.on\_modified)].

Вызывается при изменении файла или каталога.

**Параметры**: **event**([DirModifiedEvent](watchdog.tricks.md#dirmodifiedevent) или [FileModifiedEvent](watchdog.tricks.md#filemodifiedevent)) - Событие, представляющее изменение файла/каталога.

#### on\_moved(_event_)

\[[Исходник](https://python-watchdog.readthedocs.io/en/stable/\_modules/watchdog/tricks.html#LoggerTrick.on\_moved)].

Вызывается при перемещении или переименовании файла или каталога.

**Параметры**: **event**([DirMovedEvent](watchdog.tricks.md#dirmovedevent) или [FileMovedEvent](watchdog.tricks.md#filemovedevent)) - Событие, представляющее перемещение файла/каталога.

### ShellCommandTrick

#### _class_ watchdog.tricks.ShellCommandTrick(_shell\_command=None_, _patterns=None_, _ignore\_patterns=None_, _ignore\_directories=False_, _wait\_for\_process=False_, _drop\_during\_process=False_)

\[[Исходник](https://python-watchdog.readthedocs.io/en/stable/\_modules/watchdog/tricks.html#ShellCommandTrick)].

**Bases**: [watchdog.tricks.Trick](watchdog.tricks.md#trick)

Выполняет команды оболочки в ответ на соответствующие события.

#### on\_any\_event(_event_)

\[[Исходник](https://python-watchdog.readthedocs.io/en/stable/\_modules/watchdog/tricks.html#ShellCommandTrick.on\_any\_event)].

Универсальный обработчик событий.

**Параметры**: **event**([FileSystemEvent](watchdog.tricks.md#filesystemevent)) - Объект события, представляющий событие файловой системы.

### AutoRestartTrick

#### _class_ watchdog.tricks.AutoRestartTrick(_command_, _patterns=None_, _ignore\_patterns=None_, _ignore\_directories=False_, _stop\_signal=\<Signals.SIGINT: 2>_, _kill\_after=10_)

\[[Исходник](https://python-watchdog.readthedocs.io/en/stable/\_modules/watchdog/tricks.html#AutoRestartTrick)].

**Bases**: [watchdog.tricks.Trick](watchdog.tricks.md#trick)

Запускает продолжительный подпроцесс и перезапускает его при совпадении событий.

Параметр команды представляет собой список аргументов команды, таких как `[‘bin/myserver’, ‘-c’, ‘etc/myconfig.ini’]`.

Вызовите `start()` после создания трюка. Вызовите `stop()` при остановке процесса.

#### on\_any\_event(_event_)

\[[Исходник](https://python-watchdog.readthedocs.io/en/stable/\_modules/watchdog/tricks.html#ShellCommandTrick.on\_any\_event)].

Универсальный обработчик событий.

**Параметры**: **event**([FileSystemEvent](watchdog.tricks.md#filesystemevent)) - Объект события, представляющий событие файловой системы.
