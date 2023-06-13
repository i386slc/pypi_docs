# watchdog.observers.polling

**модуль**: _watchdog.observers.polling_

**краткий обзор**: Реализация эмиттера опроса.

**автор**: [yesudeep@google.com](mailto:yesudeep%40google.com) (Yesudeep Mangalapilly)

**автор**: [contact@tiger-222.fr](mailto:contact%40tiger-222.fr) (Mickaël Schoentgen)

## Классы

### PollingObserver

#### _class_ watchdog.observers.polling.PollingObserver(_timeout=1_)

\[[Исходник](https://python-watchdog.readthedocs.io/en/stable/\_modules/watchdog/observers/polling.html#PollingObserver)].

**Bases**: [watchdog.observers.api.BaseObserver](watchdog.observers.api.md#baseobserver)

Независимый от платформы наблюдатель, который опрашивает каталог для обнаружения изменений файловой системы.

### PollingObserverVFS

#### _class_ watchdog.observers.polling.PollingObserverVFS(_stat_, _listdir_, _polling\_interval=1_)

\[[Исходник](https://python-watchdog.readthedocs.io/en/stable/\_modules/watchdog/observers/polling.html#PollingObserverVFS)].

**Bases**: [watchdog.observers.api.BaseObserver](watchdog.observers.api.md#baseobserver)

Независимый от файловой системы наблюдатель, который опрашивает каталог для обнаружения изменений.

#### \_\_init\_\_(_stat_, _listdir_, _polling\_interval=1_)

**Параметры**:

* **stat** - функция **stat**. Подробности смотрите в `os.stat`.
* **listdir** - функция списка каталогов **listdir**. Подробности смотрите на `os.scandir`.
* **polling\_interval** (float) - интервал в секундах между опросом файловой системы.
