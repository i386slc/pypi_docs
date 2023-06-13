# watchdog.observers

**модуль**: _watchdog.observers_

**краткий обзор**: Наблюдатель, который выбирает нативную реализацию, если она доступна.

**автор**: [yesudeep@google.com](mailto:yesudeep%40google.com) (Yesudeep Mangalapilly)

**автор**: [contact@tiger-222.fr](mailto:contact%40tiger-222.fr) (Mickaël Schoentgen)

## Классы

### Observer

#### watchdog.observers.Observer

псевдоним watchdog.observers.inotify.InotifyObserver

Поток наблюдателя, который планирует просмотр каталогов и отправляет вызовы обработчикам событий.

Вы также можете напрямую импортировать специфичные для платформы классы и использовать их вместо **Observer**. Вот список реализованных классов наблюдателей.:

<table><thead><tr><th width="269.3333333333333">Класс</th><th>Платформа</th><th>Примечания</th></tr></thead><tbody><tr><td>inotify.InotifyObserver</td><td>Linux 2.6.13+</td><td>базовый наблюдатель</td></tr><tr><td>fsevents.FSEventsObserver</td><td>Mac OS X</td><td>базовый наблюдатель</td></tr><tr><td>kqueue.KqueueObserver</td><td>Mac OS X и BSD с kqueue(2)</td><td>базовый наблюдатель</td></tr><tr><td>read_directory_changes.WindowsApiObserver</td><td>MS Windows</td><td>базовый наблюдатель Windows API</td></tr><tr><td><a href="https://python-watchdog.readthedocs.io/en/stable/api.html#watchdog.observers.polling.PollingObserver">polling.PollingObserver</a></td><td>Любой</td><td>резервная реализация</td></tr></tbody></table>
