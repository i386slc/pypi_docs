# Установка watchdog

Для работы watchdog требуется `3.6+`. См. список [зависимостей](ustanovka-watchdog.md#zavisimosti).

## Установка из PyPI с помощью pip

```bash
$ python -m pip install -U watchdog

# или установить утилиту watchmedo:
$ python -m pip install -U |project_name|[watchmedo]
```

## Установка из архивов с исходным кодом

```bash
$ wget -c https://pypi.python.org/packages/source/w/watchdog/watchdog-2.1.5.tar.gz
$ tar zxvf watchdog-2.1.5.tar.gz
$ cd watchdog-2.1.5
$ python -m pip install -e .

# или установить утилиту watchmedo:
$ python -m pip install -e ".[watchmedo]"
```

## Установка из репозитория кода

```bash
$ git clone --recursive git://github.com/gorakhargosh/watchdog.git
$ cd watchdog
$ python -m pip install -e .

# или установить утилиту watchmedo:
$ python -m pip install -e ".[watchmedo]"
```

## Зависимости

**watchdog** зависит от множества библиотек для выполнения своей работы. Ниже приведен список зависимостей, которые вам нужны в зависимости от используемой операционной системы.

<table><thead><tr><th>Зависимость/ОС</th><th width="107">Windows</th><th width="106">Linux 2.6</th><th width="166">Mac OS X/Darwin</th><th>BSD</th></tr></thead><tbody><tr><td><a href="https://developer.apple.com/technologies/tools/xcode.html">XCode</a></td><td></td><td></td><td>Yes</td><td></td></tr></tbody></table>

Ниже приведен список необходимых вам зависимостей в зависимости от операционной системы, в которой вы используете утилиту **watchmedo**.

<table><thead><tr><th>Зависимость/ОС</th><th width="107">Windows</th><th width="106">Linux 2.6</th><th width="166">Mac OS X/Darwin</th><th>BSD</th></tr></thead><tbody><tr><td><a href="https://www.pyyaml.org/">PyYAML</a></td><td>Yes</td><td>Yes</td><td>Yes</td><td>Yes</td></tr><tr><td><a href="https://pypi.python.org/pypi/argh">argh</a></td><td>Yes</td><td>Yes</td><td>Yes</td><td>Yes</td></tr></tbody></table>

### Установка зависимостей

Скрипт **watchmedo** зависит от [PyYAML](https://www.pyyaml.org/), который связан с [LibYAML](https://pyyaml.org/wiki/LibYAML). В Mac OS X вы можете использовать [homebrew](https://brew.sh/) для установки **LibYAML**:

```bash
brew install libyaml
```

В Linux используйте ваш любимый менеджер пакетов для установки **LibYAML**. Вот как это сделать в Ubuntu:

```bash
sudo apt install libyaml-dev
```

В Windows установите [PyYAML](https://www.pyyaml.org/), используя предоставленные двоичные файлы.

## Поддерживаемые платформы (и предостережения)

**watchdog** максимально использует собственные API-интерфейсы, периодически возвращаясь к опросу диска для сравнения снимков каталога только тогда, когда он не может использовать API-интерфейс, изначально предоставляемый базовой операционной системой. В настоящее время поддерживаются следующие операционные системы:

{% hint style="danger" %}
Различия между поведением этих нативных API указаны ниже.
{% endhint %}

### Linux 2.6+

Версия ядра Linux 2.6 и более поздние версии поставляются с API, называемым [inotify](https://linux.die.net/man/7/inotify), который программы могут использовать для мониторинга событий файловой системы.

{% hint style="info" %}
В большинстве систем максимальное количество наблюдений, которое может быть создано для каждого пользователя, ограничено **8192**. Для мониторинга watchdog требуется по одному на каждый каталог. Чтобы изменить это ограничение, отредактируйте `/etc/sysctl.conf` и добавьте:

```
fs.inotify.max_user_watches=16384
```
{% endhint %}

### Mac OS X

API ядра Darwin/OS X поддерживает два способа мониторинга каталогов на предмет событий файловой системы:

* [kqueue](https://www.freebsd.org/cgi/man.cgi?query=kqueue\&sektion=2)
* [FSEvents](https://developer.apple.com/library/mac/#documentation/Darwin/Conceptual/FSEvents\_ProgGuide/Introduction/Introduction.html)

**watchdog** может использовать любой доступный, предпочитая `FSEvents kqueue(2)`. **kqueue(2)** использует дескрипторы открытых файлов для мониторинга, а текущая реализация использует [рекомендации по мониторингу производительности файловой системы Mac OS X](https://developer.apple.com/library/ios/#documentation/Performance/Conceptual/FileSystem/Articles/TrackingChanges.html), чтобы открывать эти дескрипторы файлов только для мониторинга событий, что позволяет OS X размонтировать отслеживаемые тома, не блокируя их.

{% hint style="info" %}
Дополнительная информация о том, как watchdog использует kqueue(2), приведена в [вариантах BSD Unix](ustanovka-watchdog.md#varianty-bsd-unix). Большая часть этой информации относится и к Mac OS X.
{% endhint %}

### Варианты BSD Unix

Варианты BSD поставляются с [kqueue](https://www.freebsd.org/cgi/man.cgi?query=kqueue\&sektion=2), который программы могут использовать для отслеживания изменений в дескрипторах открытых файлов. Из-за того, как работает **kqueue(2)**, watchdog должен открывать эти файлы и каталоги в неблокирующем режиме только для чтения и хранить книги о них.

**watchdog** автоматически открывает файловые дескрипторы для всех новых созданных файлов/каталогов и закрывает те, для которых были удалены.

{% hint style="info" %}
Максимальное количество дескрипторов открытых файлов на процесс в вашей операционной системе может препятствовать способности watchdog отслеживать файлы.

Вы должны убедиться, что это ограничение установлено как минимум на **1024** (или значение, подходящее для вашего использования). Следующая команда, добавленная к вашему файлу конфигурации `~/.profile`, сделает это за вас:

```
ulimit -n 1024
```
{% endhint %}

### Windows Vista и позднее

Windows API предоставляет свойство [ReadDirectoryChangesW](https://docs.microsoft.com/windows/win32/api/winbase/nf-winbase-readdirectorychangesw). В настоящее время watchdog содержит реализацию для синхронного подхода, требующего дополнительных функций API, доступных только в Windows Vista и более поздних версиях.

{% hint style="info" %}
Поскольку переименование — это не то же самое, что перемещение в Windows, watchdog изо всех сил старается преобразовать переименования в события перемещения. Кроме того, поскольку API-функция [ReadDirectoryChangesW](https://docs.microsoft.com/windows/win32/api/winbase/nf-winbase-readdirectorychangesw) возвращает события переименования/перемещения для каталогов еще до того, как основной ввод-вывод завершен, watchdog может быть не в состоянии полностью просканировать перемещенный каталог, чтобы успешно поставить в очередь события перемещения для файлов и каталогов в нем.
{% endhint %}

{% hint style="info" %}
Поскольку Windows API не предоставляет информацию о том, является ли объект файлом или каталогом, события удаления для каталогов могут быть зарегистрированы как событие удаления файла.
{% endhint %}

### Опрос, не зависящий от ОС

**watchdog** также включает резервную реализацию, которая опрашивает отслеживаемые каталоги на наличие изменений, периодически сравнивая снимки дерева каталогов.
