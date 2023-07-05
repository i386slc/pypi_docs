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

#### pyproject.toml (BETA)

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

#### pyproject.toml (BETA)

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

По умолчанию **setuptools** будет рассматривать 2 популярных макета проекта, каждый из которых имеет свой собственный набор преимуществ и недостатков \[2] \[3], как описано в следующих разделах.

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

#### pyproject.toml (BETA)

```toml
# ...
[tool.setuptools.packages]
find = {}  # Сканирование неявных пространств имен активно по умолчанию.
# ИЛИ
find = {namespaces = false}  # Отключить неявные пространства имен
```
