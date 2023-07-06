# Обнаружение пакетов и пакеты пространства имен

{% hint style="info" %}
полную спецификацию ключевых слов, поставляемых в **setup.cfg** или **setup.py**, можно найти в <mark style="color:purple;">справочнике по ключевым словам</mark>.
{% endhint %}

{% hint style="success" %}
**Важно**

Приведенные здесь примеры предназначены только для демонстрации представленных функциональных возможностей. Если вы хотите воспроизвести их в своей системе, необходимо предоставить больше аргументов метаданных и опций. Если вы новичок в **setuptools**, вам будет полезно начать с раздела «[Быстрый старт](bystryi-start-setuptools.md)».
{% endhint %}

**Setuptools** предоставляет мощные инструменты для обнаружения пакетов, включая поддержку пакетов пространства имен.

Обычно вы указываете пакеты для включения вручную следующим образом:

#### setup.cfg

```ini
[options]
#...
packages =
    mypkg
    mypkg.subpkg1
    mypkg.subpkg2
```

#### setup.py

```python
setup(
    # ...
    packages=['mypkg', 'mypkg.subpkg1', 'mypkg.subpkg2']
)
```

#### pyproject.toml (BETA) [\[1\]](obnaruzhenie-paketov-i-pakety-prostranstva-imen.md#1)

```toml
# ...
[tool.setuptools]
packages = ["mypkg", "mypkg.subpkg1", "mypkg.subpkg2"]
# ...
```

Если ваши пакеты не находятся в корне репозитория или не соответствуют точно структуре каталогов, вам также необходимо настроить **package\_dir**:

#### setup.cfg

```ini
[options]
# ...
package_dir =
    = src
    # каталог, содержащий все пакеты (например,  src/mypkg, src/mypkg/subpkg1, ...)
# ИЛИ
package_dir =
    mypkg = lib
    # mypkg.module соответствует lib/module.py
    mypkg.subpkg1 = lib1
    # mypkg.subpkg1.module1 соответствует lib1/module1.py
    mypkg.subpkg2 = lib2
    # mypkg.subpkg2.module2 соответствует lib2/module2.py
# ...
```

#### setup.py

```python
setup(
    # ...
    package_dir = {"": "src"}
    # каталог, содержащий все пакеты (например,  src/mypkg, src/mypkg/subpkg1, ...)
)

# ИЛИ

setup(
    # ...
    package_dir = {
        "mypkg": "lib",  # mypkg.module соответствует lib/module.py
        "mypkg.subpkg1": "lib1",  # mypkg.subpkg1.module1 соответствует lib1/module1.py
        "mypkg.subpkg2": "lib2"   # mypkg.subpkg2.module2 соответствует lib2/module2.py
        # ...
)
```

#### pyproject.toml (BETA) [\[1\]](obnaruzhenie-paketov-i-pakety-prostranstva-imen.md#1)

```toml
[tool.setuptools]
# ...
package-dir = {"" = "src"}
    # каталог, содержащий все пакеты (например,  src/mypkg1, src/mypkg2)

# ИЛИ

[tool.setuptools.package-dir]
mypkg = "lib"
# mypkg.module соответствует lib/module.py
"mypkg.subpkg1" = "lib1"
# mypkg.subpkg1.module1 соответствует lib1/module1.py
"mypkg.subpkg2" = "lib2"
# mypkg.subpkg2.module2 соответствует lib2/module2.py
# ...
```

Это может очень быстро надоесть. Чтобы ускорить процесс, вы можете положиться на автоматическое обнаружение **setuptools** или использовать предоставленные инструменты, как описано в следующих разделах.

{% hint style="success" %}
**Важно**

Хотя **setuptools** позволяет разработчикам создавать очень сложные сопоставления между именами каталогов и именами пакетов, лучше _сделать его простым_ и отразить желаемую иерархию пакетов в структуре каталогов, сохранив те же имена.
{% endhint %}

## Автоматическое обнаружение

{% hint style="warning" %}
Автоматическое обнаружение — это **бета**-функция, которая может измениться в будущем. Дополнительные методы обнаружения см. в разделе <mark style="color:purple;">Выборочное обнаружение</mark>.
{% endhint %}

По умолчанию **setuptools** будет рассматривать 2 популярных макета проекта, каждый из которых имеет свой собственный набор преимуществ и недостатков [\[2\]](obnaruzhenie-paketov-i-pakety-prostranstva-imen.md#2) [\[3\]](obnaruzhenie-paketov-i-pakety-prostranstva-imen.md#3), как описано в следующих разделах.

**Setuptools** автоматически просканирует каталог вашего проекта в поисках этих макетов и попытается угадать правильные значения для <mark style="color:purple;">packages</mark> и конфигурации <mark style="color:purple;">py\_modules</mark>.

{% hint style="success" %}
Важно

Автоматическое обнаружение будет включено **только** в том случае, если вы **не предоставите** никаких настроек для **packages** и **py\_modules**. Если хотя бы один из них задан явно, автоматического обнаружения _**не произойдет**_.

Примечание: указание **ext\_modules** также может предотвратить автоматическое обнаружение, если только вы не выберете <mark style="color:purple;">настройку setuptools с помощью файлов pyproject.toml</mark> (что отключит поведение обратной совместимости).
{% endhint %}

### src-layout

Проект должен содержать каталог **src** в корне проекта, и все модули и пакеты, предназначенные для распространения, помещаются в этот каталог:

```
project_root_directory
├── pyproject.toml  # И/ИЛИ setup.cfg, setup.py
├── ...
└── src/
    └── mypkg/
        ├── __init__.py
        ├── ...
        ├── module.py
        ├── subpkg1/
        │   ├── __init__.py
        │   ├── ...
        │   └── module1.py
        └── subpkg2/
            ├── __init__.py
            ├── ...
            └── module2.py
```

Этот макет очень удобен, когда вы хотите использовать автоматическое обнаружение, поскольку вам не нужно беспокоиться о том, что другие файлы или папки Python в корне вашего проекта будут распространены по ошибке. В некоторых случаях это также может быть менее подвержено ошибкам при тестировании или при использовании пакетов в стиле [PEP 420](https://peps.python.org/pep-0420/). С другой стороны, вы не можете полагаться на неявный **PYTHONPATH=**. чтобы запустить Python REPL и поиграть с вашим пакетом (для этого вам понадобится <mark style="color:purple;">редактируемая установка</mark>).

### flat-layout

_(также известный как "adhoc")_

Папка(и) пакета размещается непосредственно в корне проекта:

```
project_root_directory
├── pyproject.toml  # И/ИЛИ setup.cfg, setup.py
├── ...
└── mypkg/
    ├── __init__.py
    ├── ...
    ├── module.py
    ├── subpkg1/
    │   ├── __init__.py
    │   ├── ...
    │   └── module1.py
    └── subpkg2/
        ├── __init__.py
        ├── ...
        └── module2.py
```

Этот макет очень удобен для использования **REPL**, но в некоторых ситуациях он может быть более подвержен ошибкам (например, во время тестов или если у вас есть куча папок или файлов Python, висящих в корне вашего проекта).

Во избежание путаницы имена файлов и папок, используемые популярными инструментами (или соответствующие общеизвестным соглашениям, таким как распространение документации вместе с кодом проекта), автоматически отфильтровываются в случае **flat-layout**:

#### Зарезервированные имена пакетов

```python
FlatLayoutPackageFinder.DEFAULT_EXCLUDE: Tuple[str, ...] = ('ci', 'ci.*', 'bin',
    'bin.*', 'doc', 'doc.*', 'docs', 'docs.*', 'documentation', 'documentation.*',
    'manpages', 'manpages.*', 'news', 'news.*', 'changelog', 'changelog.*', 'test',
    'test.*', 'tests', 'tests.*', 'unit_test', 'unit_test.*', 'unit_tests',
    'unit_tests.*', 'example', 'example.*', 'examples', 'examples.*', 'scripts',
    'scripts.*', 'tools', 'tools.*', 'util', 'util.*', 'utils', 'utils.*', 'python',
    'python.*', 'build', 'build.*', 'dist', 'dist.*', 'venv', 'venv.*', 'env',
    'env.*', 'requirements', 'requirements.*', 'tasks', 'tasks.*', 'fabfile',
    'fabfile.*', 'site_scons', 'site_scons.*', 'benchmark', 'benchmark.*',
    'benchmarks', 'benchmarks.*', 'exercise', 'exercise.*', 'exercises',
    'exercises.*', 'htmlcov', 'htmlcov.*', '[._]*', '[._]*.*'
)
```

Зарезервированные имена модулей верхнего уровня

```python
FlatLayoutModuleFinder.DEFAULT_EXCLUDE: Tuple[str, ...] = ('setup', 'conftest',
    'test', 'tests', 'example', 'examples', 'build', 'toxfile', 'noxfile',
    'pavement', 'dodo', 'tasks', 'fabfile', '[Ss][Cc]onstruct', 'conanfile',
    'manage', 'benchmark', 'benchmarks', 'exercise', 'exercises', '[._]*'
)
```

{% hint style="warning" %}
Если вы используете автоматическое обнаружение с **flat-layout**, **setuptools** откажется создавать [архивы дистрибутива](https://packaging.python.org/en/latest/glossary/#term-Distribution-Package) с несколькими пакетами или модулями верхнего уровня.

Это делается для предотвращения распространенных ошибок, таких как случайная публикация кода, не предназначенного для распространения (например, скрипты, связанные с обслуживанием).

Пользователям, которые целенаправленно хотят создавать дистрибутивы с несколькими пакетами, рекомендуется использовать [пользовательское обнаружение](obnaruzhenie-paketov-i-pakety-prostranstva-imen.md#polzovatelskoe-obnaruzhenie) или макет **src-layout**.
{% endhint %}

Существует также удобный вариант **flat-layout** для утилит/библиотек, который можно реализовать с помощью одного файла Python:

### распространение single-module

Отдельный модуль помещается непосредственно в корень проекта, а не в папку пакета:

```
project_root_directory
├── pyproject.toml  # И/ИЛИ setup.cfg, setup.py
├── ...
└── single_file_lib.py
```

## Пользовательское обнаружение

Если автоматическое обнаружение не работает для вас (например, вы хотите _включить_ в дистрибутив пакеты верхнего уровня с зарезервированными именами, такими как **tasks**, **example** или **docs**, или вы хотите _исключить_ вложенные пакеты, которые в противном случае были бы включены), вы можете используйте предоставленные инструменты для обнаружения пакетов:

#### setup.cfg

```ini
[options]
packages = find:
# или
packages = find_namespace:
```

#### setup.py

```python
from setuptools import find_packages
# или
from setuptools import find_namespace_packages
```

#### pyproject.toml (BETA) [\[1\]](obnaruzhenie-paketov-i-pakety-prostranstva-imen.md#1)

```toml
# ...
[tool.setuptools.packages]
find = {}  # Сканирование неявных пространств имен активно по умолчанию.
# ИЛИ
find = {namespaces = false}  # Отключить неявные пространства имен
```

### Поиск простых пакетов

Начнем с первого инструмента. `find:` (`find_packages()`) берет исходный каталог и два списка шаблонов имен пакетов для исключения и включения, а затем возвращает список строк, представляющих пакеты, которые он может найти. Чтобы использовать его, рассмотрите следующий каталог:

```
mypkg
├── pyproject.toml  # ИЛИ/И setup.cfg, setup.py
└── src
    ├── pkg1
    │   └── __init__.py
    ├── pkg2
    │   └── __init__.py
    ├── additional
    │   └── __init__.py
    └── pkg
        └── namespace
            └── __init__.py
```

Чтобы **setuptools** автоматически включал пакеты, найденные в **src**, которые начинаются с имени **pkg**, а не дополнительных:

#### setup.cfg

```ini
[options]
packages = find:
package_dir =
    =src

[options.packages.find]
where = src
include = pkg*
# альтернативно: `exclude = additional*`
```

#### setup.py

```python
setup(
    # ...
    packages=find_packages(
        where='src',
        include=['pkg*'],  # альтернативно: `exclude=['additional*']`
    ),
    package_dir={"": "src"}
    # ...
)
```

#### pyproject.toml (BETA) [\[1\]](obnaruzhenie-paketov-i-pakety-prostranstva-imen.md#1)

```toml
[tool.setuptools.packages.find]
where = ["src"]
include = ["pkg*"]  # альтернативно: `exclude = ["additional*"]`
namespaces = false
```

{% hint style="info" %}
При использовании `tool.setuptools.packages.find` в `pyproject.toml` **setuptools** по умолчанию будет учитывать [неявные пространства имен](https://peps.python.org/pep-0420/) (_implicit namespaces_) при сканировании каталога вашего проекта. Чтобы предотвратить добавление **pkg.namespace** в список пакетов, вы можете установить `namespaces = false`. Это предотвратит сканирование любой папки без файла `__init__.py`.
{% endhint %}

{% hint style="success" %}
**Важно**

**include** и **exclude** принимают строки, представляющие шаблоны **glob**. Эти шаблоны должны соответствовать полному имени модуля Python (как если бы оно было написано в операторе **import**).

Например, если у вас есть шаблон **util**, он будет соответствовать `util/__init__.py`, но не `util/files/__init__.py`.

Тот факт, что родительский пакет соответствует шаблону, не будет определять, будет ли подмодуль включен или исключен из дистрибутива. Вам нужно будет явно добавить подстановочный знак (например, **util\***), если вы хотите, чтобы шаблон также соответствовал подмодулям.
{% endhint %}

### Поиск пакетов пространства имен

**setuptools** предоставляет `find_namespace:` (`find_namespace_packages()`), который ведет себя аналогично `find:`, но работает с пакетами пространства имен.

Прежде чем углубляться в эту тему, важно иметь хорошее представление о том, что такое [пакеты пространств имен](https://peps.python.org/pep-0420/) (_namespace packages_). Вот краткий обзор.

Когда у вас есть два пакета, организованные следующим образом:

```
/Users/Desktop/timmins/foo/__init__.py
/Library/timmins/bar/__init__.py
```

Если и **Desktop**, и **Library** находятся в вашем **PYTHONPATH**, то при вызове механизма импорта для вас будет автоматически создан пакет пространства имен с именем **timmins**, что позволит вам выполнить следующее:

```python
>>> import timmins.foo
>>> import timmins.bar
```

как будто в вашей системе только один **timmins**. Затем эти два пакета можно распространять и устанавливать по отдельности, не затрагивая другой пакет.

Теперь предположим, что вы решили упаковать часть **foo** для распространения и начали с создания каталога проекта, организованного следующим образом:

```
foo
├── pyproject.toml  # И/ИЛИ setup.cfg, setup.py
└── src
    └── timmins
        └── foo
            └── __init__.py
```

Если вы хотите, чтобы `timmins.foo` автоматически включался в distrib, то вам нужно будет указать:

#### setup.cfg

```ini
[options]
package_dir =
    =src
packages = find_namespace:

[options.packages.find]
where = src
```

#### setup.py

```python
setup(
    # ...
    packages=find_namespace_packages(where='src'),
    package_dir={"": "src"}
    # ...
)
```

#### pyproject.toml (BETA) [\[1\]](obnaruzhenie-paketov-i-pakety-prostranstva-imen.md#1)

```toml
[tool.setuptools.packages.find]
where = ["src"]
```

При использовании `tool.setuptools.packages.find` в `pyproject.toml` **setuptools** по умолчанию будет учитывать [неявные пространства имен](https://peps.python.org/pep-0420/) при сканировании каталога вашего проекта.

После установки дистрибутива пакета `timmins.foo` станет доступен вашему интерпретатору.

{% hint style="warning" %}
Имейте в виду, что `find_namespace:` (**setup.cfg**), `find_namespace_packages()` (**setup.py**) и `find` (**pyproject.toml**) будут сканировать **все** папки, которые есть в вашем каталоге проекта, если вы используете **flat-layout**.

Если использовать наивно, это может привести к добавлению нежелательных файлов к вашему последнему **wheel**. Например, с каталогом проекта, организованным следующим образом:

```
foo
├── docs
│   └── conf.py
├── timmins
│   └── foo
│       └── __init__.py
└── tests
    └── tests_foo
        └── __init__.py
```

конечные пользователи в конечном итоге установят не только **timmins.foo**, но также `docs` и `tests.tests_foo`.

Простой способ исправить это — принять вышеупомянутый [src-layout](obnaruzhenie-paketov-i-pakety-prostranstva-imen.md#src-layout) или убедиться, что правильно настроены **include** и/или **exclude**.
{% endhint %}

{% hint style="success" %}
**Важно**

После <mark style="color:purple;">создания пакета</mark> вы можете проверить, все ли файлы верны (ничего не упущено или лишнего), выполнив следующие команды:

```bash
tar tf dist/*.tar.gz
unzip -l dist/*.whl
```

Для этого в вашей ОС должны быть установлены **tar** и **unzip**. В Windows вы также можете использовать программу с графическим интерфейсом, например [7zip](https://www.7-zip.org/).
{% endhint %}

## Устаревшие пакеты пространства имен

Тот факт, что вы можете так легко создавать пакеты пространств имен выше, приписывается [PEP 420](https://peps.python.org/pep-0420/). Раньше было более громоздко добиваться того же результата. Исторически существовало два метода создания пакетов пространств имен. Один из них — это стиль **pkg\_resources**, поддерживаемый **setuptools**, а другой — стиль **pkgutils**, предлагаемый модулем **pkgutils** в Python. Оба теперь считаются _устаревшими_, несмотря на то, что они все еще присутствуют во многих существующих пакетах. Эти два отличаются многими тонкими, но важными аспектами, и вы можете узнать больше в [руководстве пользователя упаковки Python](https://packaging.python.org/guides/packaging-namespace-packages/).

### Пакет пространства имен в стиле pkg\_resource

Это метод, который **setuptools** напрямую поддерживает. Начиная с того же макета, вам нужно добавить к нему две части. Во-первых, файл `__init__.py` непосредственно в каталоге пакета вашего пространства имен, который содержит следующее:

```python
__import__("pkg_resources").declare_namespace(__name__)
```

И ключевое слово **namespace\_packages** в файле `setup.cfg` или `setup.py`:

#### setup.cfg

```ini
[options]
namespace_packages = timmins
```

#### setup.py

```python
setup(
    # ...
    namespace_packages=['timmins']
)
```

И ваш каталог должен выглядеть так

```
foo
├── pyproject.toml  # И/ИЛИ setup.cfg, setup.py
└── src
    └── timmins
        ├── __init__.py
        └── foo
            └── __init__.py
```

Повторите то же самое для других пакетов, и вы сможете добиться того же результата, что и в предыдущем разделе.

### Пакет пространства имен в стиле pkgutil

Этот метод почти идентичен **pkg\_resource**, за исключением того, что объявление **namespace\_packages** опущено, а файл `__init__.py` содержит следующее:

```python
__path__ = __import__('pkgutil').extend_path(__path__, __name__)
```

Макет проекта остается прежним, и файл `pyproject.toml/setup.cfg` остается прежним.

#### \[ 1 ]

Поддержка добавления параметров конфигурации сборки через таблицу `[tool.setuptools]` в файле `pyproject.toml` все еще находится на стадии **бета**-тестирования. См. раздел <mark style="color:purple;">Настройка инструментов настройки с помощью файлов pyproject.toml</mark>.

#### \[ 2 ]

[https://blog.ionelmc.ro/2014/05/25/python-packaging/#the-structure](https://blog.ionelmc.ro/2014/05/25/python-packaging/#the-structure)

#### \[ 3 ]

[https://blog.ionelmc.ro/2017/09/25/rehashing-the-src-layout/](https://blog.ionelmc.ro/2017/09/25/rehashing-the-src-layout/)
