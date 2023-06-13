# watchdog.utils.dirsnapshot

**модуль**: _watchdog.utils.dirsnapshot_

**краткий обзор**: Снимки каталога и сравнение.

**автор**: [yesudeep@google.com](mailto:yesudeep%40google.com) (Yesudeep Mangalapilly)

**автор**: [contact@tiger-222.fr](mailto:contact%40tiger-222.fr) (Mickaël Schoentgen)

{% hint style="info" %}
**Где перемещенные события? Они «исчезли»**

Эта реализация не принимает во внимание границы разделов **partition**. Это будет работать только тогда, когда дерево каталогов полностью находится в одной файловой системе. Точнее говоря, любая часть кода, зависящая от номеров айнодов, может сломаться при пересечении границ раздела. В этих случаях в моментальном снимке сравнения движение файла/каталога будет представляться как созданное и удаленное событие.
{% endhint %}

## Классы

### DirectorySnapshot

#### _class_ watchdog.utils.dirsnapshot.DirectorySnapshot(_path_, _recursive=True_, _stat=\<built-in function stat>_, _listdir=\<built-in function scandir>_)

\[[Исходник](https://python-watchdog.readthedocs.io/en/stable/\_modules/watchdog/utils/dirsnapshot.html#DirectorySnapshot)].

**Bases**: object

Моментальный снимок статистической информации о файлах в каталоге.

**Параметры**:

* **path** (str) - Путь к каталогу, для которого должен быть сделан снимок.
* **recursive** (bool) - `True`, если все дерево каталогов должно быть включено в снимок; `False` в противном случае.
* **stat** - Используйте пользовательскую функцию статистики, которая возвращает структуру статистики для пути. В настоящее время нужны только **st\_dev**, **st\_ino**, **st\_mode** и **st\_mtime**. Функция, принимающая путь в качестве аргумента, которая будет вызываться для каждой записи в дереве каталогов.
* **listdir** - Используйте пользовательскую функцию **listdir**. Подробнее см. `os.scandir`.

#### inode(path)

\[[Исходник](https://python-watchdog.readthedocs.io/en/stable/\_modules/watchdog/utils/dirsnapshot.html#DirectorySnapshot.inode)].

Возвращает идентификатор пути.

#### path(id)

\[[Исходник](https://python-watchdog.readthedocs.io/en/stable/\_modules/watchdog/utils/dirsnapshot.html#DirectorySnapshot.path)].

Возвращает путь для идентификатора. `None`, если идентификатор неизвестен для этого снимка.

#### paths

Набор путей к файлам/каталогам в моментальном снимке.

#### stat\_info(path)

\[[Исходник](https://python-watchdog.readthedocs.io/en/stable/\_modules/watchdog/utils/dirsnapshot.html#DirectorySnapshot.stat\_info)].

Возвращает объект статистической информации для указанного пути из моментального снимка.

Прикрепленная информация может быть изменена. Не используйте, если вы не укажете **stat** в конструкторе. Вместо этого используйте [inode()](watchdog.utils.dirsnapshot.md#inode-path), `mtime()`, `isdir()`.

**Параметры**:

* **path** - Путь, для которого следует получить статистическую информацию из моментального снимка.

### DirectorySnapshotDiff

#### _class_ watchdog.utils.dirsnapshot.DirectorySnapshotDiff(_ref_, _snapshot_, _ignore\_device=False_)

\[[Исходник](https://python-watchdog.readthedocs.io/en/stable/\_modules/watchdog/utils/dirsnapshot.html#DirectorySnapshotDiff)].

**Bases**: object

Сравнивает два снимка каталога и создает объект, представляющий разницу между двумя снимками.

**Параметры**:

* **ref** ([DirectorySnapshot](watchdog.utils.dirsnapshot.md#directorysnapshot)) - Моментальный снимок эталонного каталога.
* **shapshot** ([DirectorySnapshot](watchdog.utils.dirsnapshot.md#directorysnapshot)) - Снимок каталога, который будет сравниваться с эталонным снимком.
* **ignore\_device** (bool) - Логическое значение, указывающее, следует ли игнорировать идентификатор устройства или нет. По умолчанию файл может быть однозначно идентифицирован комбинацией его первого индексного дескриптора и его идентификатора устройства. Проблема в том, что идентификатор устройства может (а может и не меняться) между загрузками системы. Эта проблема может привести к тому, что **DirectorySnapshotDiff** будет думать, что файл был удален и создан снова, но это будет точно такой же файл. Установите значение `True`, только если вы уверены, что всегда будете использовать одно и то же устройство.

#### dirs\_created

Список каталогов, которые были созданы.

#### dirs\_deleted

Список каталогов, которые были удалены.

#### dirs\_modified

Список каталогов, которые были изменены.

#### dirs\_moved

Список каталогов, которые были перемещены.

Каждое событие представляет собой кортеж из двух элементов, первый элемент которого является путем, переименованным во второй элемент кортежа.

#### files\_created

Список файлов, которые были созданы.

#### files\_deleted

Список файлов, которые были удалены.

#### files\_modified

Список файлов, которые были изменены.

#### files\_moved

Список файлов, которые были перемещены.

Каждое событие представляет собой кортеж из двух элементов, первый элемент которого является путем, переименованным во второй элемент кортежа.

### EmptyDirectorySnapshot

#### _class_ watchdog.utils.dirsnapshot.EmptyDirectorySnapshot

\[[Исходник](https://python-watchdog.readthedocs.io/en/stable/\_modules/watchdog/utils/dirsnapshot.html#EmptyDirectorySnapshot)].

**Bases**: object

Класс для реализации пустого снимка. Это используется вместе с **DirectorySnapshot** и **DirectorySnapshotDiff**, чтобы получить все файлы/папки в созданном каталоге.

#### _static_ path(_\__)

\[[Исходник](https://python-watchdog.readthedocs.io/en/stable/\_modules/watchdog/utils/dirsnapshot.html#EmptyDirectorySnapshot.path)].

Смоделируйте метод, чтобы вернуть путь к полученному индексному узлу. Поскольку снимок должен быть пустым, он всегда возвращает `None`.

**Возвращает**: `None`

#### paths

Макет метода для возврата набора путей к файлам/каталогам в моментальном снимке. Поскольку снимок должен быть пустым, он всегда возвращает пустой набор.

**Возвращает**: пустой набор.
