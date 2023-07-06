# Управление зависимостями в Setuptools

Существует три типа стилей зависимостей, предлагаемых **setuptools**:

1. требование системы сборки
2. обязательная зависимость
3. необязательная зависимость.

Каждая зависимость, независимо от типа, должна быть указана в соответствии с [PEP 508](https://peps.python.org/pep-0508/) и [PEP 440](https://peps.python.org/pep-0440/). Это позволяет добавлять [ограничения диапазона](https://peps.python.org/pep-0440/#version-specifiers) версий (_range restrictions_) и [маркеры среды](upravlenie-zavisimostyami-v-setuptools.md#zavisimosti-ot-konkretnoi-platformy) (_environmant markers_).

## Требования к системе сборки

После организации всех скриптов и файлов и подготовки к упаковке должен быть способ указать, какие программы и библиотеки действительно необходимы для упаковки (в нашем случае, конечно, **setuptools**). Это необходимо указать в вашем файле `pyproject.toml` (если вы забыли, что это такое, перейдите в [Quickstart](bystryi-start-setuptools.md) или <mark style="color:purple;">Build System Support</mark>):

```toml
[build-system]
requires = ["setuptools"]
#...
```

Обратите внимание, что вы также должны указать здесь любой другой плагин **setuptools** (например, [setuptools-scm](https://pypi.org/project/setuptools-scm), [setuptools-golang](https://pypi.org/project/setuptools-golang), [setuptools-rust](https://pypi.org/project/setuptools-rust)) или зависимость времени сборки (например, [Cython](https://pypi.org/project/Cython), [cppy](https://pypi.org/project/cppy), [pybind11](https://pypi.org/project/pybind11)).

{% hint style="info" %}
В предыдущих версиях **setuptools** это выполнялось с помощью ключевого слова **setup\_requires**, но теперь считается устаревшим в пользу стиля [PEP 517](https://peps.python.org/pep-0517/), описанного выше. Чтобы узнать, как используется это устаревшее ключевое слово, обратитесь к нашему <mark style="color:purple;">руководству по устаревшей практике (WIP)</mark>.
{% endhint %}

## Объявление требуемой зависимости

Здесь пакет объявляет свои основные зависимости, без которых он не сможет работать. **setuptools** поддерживает автоматическую загрузку и установку этих зависимостей при установке пакета. Хотя в этом больше изящества, давайте начнем с простого примера.

#### pyproject.toml

```toml
[project]
# ...
dependencies = [
    "docutils",
    "BazSpam == 1.1",
]
# ...
```

#### setup.cfg

```ini
[options]
#...
install_requires =
    docutils
    BazSpam ==1.1
```

#### setup.py

```python
setup(
    ...,
    install_requires=[
        'docutils',
        'BazSpam ==1.1',
    ],
)
```

Когда ваш проект установлен (например, с помощью [pip](https://pypi.org/project/pip)), все еще не установленные зависимости будут обнаружены (через [PyPI](https://pypi.org/)), загружены, построены (при необходимости) и установлены, и 2) любые скрипты в вашем проекте будут установлены с обертками которые проверяют доступность указанных зависимостей во время выполнения.

### Зависимости от конкретной платформы

**Setuptools** предлагает возможность оценить определенные условия перед слепой установкой всего, что указано в **install\_requires**. Это отлично подходит для конкретных зависимостей платформы. Например, пакет **enum** был добавлен в Python 3.4, поэтому зависимый от него пакет может установить его только в том случае, если версия Python старше 3.4. Для этого

#### pyproject.toml

```toml
[project]
# ...
dependencies = [
    "enum34; python_version<'3.4'",
]
# ...
```

#### setup.cfg

```ini
[options]
#...
install_requires =
    enum34;python_version<'3.4'
```

#### setup.py

```python
setup(
    ...,
    install_requires=[
        "enum34;python_version<'3.4'",
    ],
)
```

Точно так же, если вы также хотите объявить **pywin32** с минимальной версией 1.0 и установить его, только если пользователь использует операционную систему **Windows**:

#### pyproject.toml

```toml
[project]
# ...
dependencies = [
    "enum34; python_version<'3.4'",
    "pywin32 >= 1.0; platform_system=='Windows'",
]
# ...
```

#### setup.cfg

```ini
[options]
#...
install_requires =
    enum34;python_version<'3.4'
    pywin32 >= 1.0;platform_system=='Windows'
```

#### setup.py

```python
setup(
    ...,
    install_requires=[
        "enum34;python_version<'3.4'",
        "pywin32 >= 1.0;platform_system=='Windows'",
    ],
)
```

Маркеры окружающей среды, которые можно использовать для тестирования типов платформ, подробно описаны в [PEP 508](https://peps.python.org/pep-0508/).

{% hint style="info" %}
**Смотри также**

В качестве альтернативы можно использовать <mark style="color:purple;">backend wrapper</mark> для конкретных случаев использования, когда маркеров среды недостаточно.
{% endhint %}

### Прямые зависимости URL

{% hint style="danger" %}
[PyPI](https://pypi.org/) и другие индексы пакетов, соответствующие стандартам, **не принимают** пакеты, которые объявляют зависимости с использованием прямых URL-адресов. Однако **pip** примет их при установке пакетов из локальной файловой системы или с другого URL-адреса.
{% endhint %}

Зависимости, которые недоступны в индексе пакета, но могут быть загружены в другом месте в виде исходного репозитория или архива, могут быть указаны с использованием варианта [прямых ссылок PEP 440](https://peps.python.org/pep-0440/#direct-references):

#### pyproject.toml

```toml
[project]
# ...
dependencies = [
    "Package-A @ git+https://example.net/package-a.git@main",
    "Package-B @ https://example.net/archives/package-b.whl",
]
```

#### setup.cfg

```ini
[options]
#...
install_requires =
    Package-A @ git+https://example.net/package-a.git@main
    Package-B @ https://example.net/archives/package-b.whl
```

#### setup.py

```python
setup(
    install_requires=[
       "Package-A @ git+https://example.net/package-a.git@main",
       "Package-B @ https://example.net/archives/package-b.whl",
    ],
    ...,
)
```

Для URL-адресов исходного репозитория список поддерживаемых протоколов и функций, специфичных для VCS, таких как выбор определенных ветвей или тегов, можно найти в документации pip по [поддержке VCS](https://pip.pypa.io/en/latest/topics/vcs-support/). Поддерживаемые форматы URL-адресов архивов: **sdists** и **wheels**.

## Необязательные зависимости

**Setuptools** позволяет объявлять зависимости, которые не установлены по умолчанию. Фактически это означает, что вы можете создать `"variant"` своего пакета с набором дополнительных функций.

Например, давайте рассмотрим **Package-A**, который предлагает дополнительную поддержку **PDF** и требует для своей работы две другие зависимости:

#### pyproject.toml

```toml
[project]
name = "Package-A"
# ...
[project.optional-dependencies]
PDF = ["ReportLab>=1.2", "RXP"]
```

#### setup.cfg

```ini
[metadata]
name = Package-A

[options.extras_require]
PDF =
    ReportLab>=1.2
    RXP
```

#### setup.py

```python
setup(
    name="Package-A",
    ...,
    extras_require={
        "PDF": ["ReportLab>=1.2", "RXP"],
    },
)
```

Имя **PDF** является произвольным [идентификатором](https://peps.python.org/pep-0685/) (_identifier_) такого списка зависимостей, на который могут ссылаться другие компоненты и устанавливать их.

{% hint style="success" %}
**Совет**

Также удобно объявить необязательные требования для вспомогательных задач, таких как выполнение тестов и/или создание документации.
{% endhint %}

Вариант использования этого подхода заключается в том, что другие пакеты могут использовать это «дополнение» для своих собственных зависимостей. Например, если для **Package-B** требуется **Package-A** с установленной поддержкой **PDF**, он может объявить зависимость следующим образом:

#### pyproject.toml

```toml
[project]
name = "Package-B"
# ...
dependencies = [
    "Package-A[PDF]"
]
```

#### setup.cfg

```ini
[metadata]
name = Package-B
#...

[options]
#...
install_requires =
    Package-A[PDF]
```

#### setup.py

```python
setup(
    name="Package-B",
    install_requires=["Package-A[PDF]"],
    ...,
)
```

Это приведет к тому, что **ReportLab** будет установлен вместе с **Package-A**, если установлен **Package-B**, даже если **Package-A** уже был установлен. Таким образом, проект может инкапсулировать группы необязательных «нисходящих зависимостей» под именем функции, так что зависимым от него пакетам не нужно знать, что такое последующие зависимости. Если в более поздней версии **Package-A** встроена поддержка **PDF** и больше не требуется **ReportLab**, или если в конечном итоге потребуются другие зависимости, кроме **ReportLab**, для обеспечения поддержки **PDF**, информацию о настройке **Package-B** менять не нужно, но нужные пакеты все равно будет установлен при необходимости.

{% hint style="success" %}
**Совет**

Рекомендация: если проекту больше не нужны никакие другие пакеты для поддержки функции, он должен сохранить пустой список требований для этой функции в своем аргументе **extras\_require**, чтобы пакеты, зависящие от этой функции, не ломались (из-за недопустимого название функции).
{% endhint %}

{% hint style="warning" %}
Исторически **setuptools** также использовался для поддержки дополнительных зависимостей в консольных скриптах, например:

#### setup.cfg

```ini
[metadata]
name = Package-A
#...

[options]
#...
entry_points=
    [console_scripts]
    rst2pdf = project_a.tools.pdfgen [PDF]
    rst2html = project_a.tools.htmlgen
```

#### setup.py

```python
setup(
    name="Package-A",
    ...,
    entry_points={
        "console_scripts": [
            "rst2pdf = project_a.tools.pdfgen [PDF]",
            "rst2html = project_a.tools.htmlgen",
        ],
    },
)
```

Этот синтаксис указывает, что точка входа **entry\_points** (в данном случае консольный скрипт) действительна, только если установлен дополнительный PDF-файл. Установщик должен определить, как поступить в ситуации, когда PDF не указан (например, пропустить сценарий консоли, предоставить предупреждение при попытке загрузить точку входа, предположить, что дополнительные компоненты присутствуют, и позволить реализации завершиться позже).

**Однако** pip и другие инструменты могут не поддерживать этот вариант использования дополнительных зависимостей, поэтому эта практика считается **устаревшей**. См. <mark style="color:purple;">спецификацию точек входа</mark>.
{% endhint %}

## Требование Python

В некоторых случаях может потребоваться указать минимальную требуемую версию Python. Это можно настроить, как показано в примере ниже.

#### pyproject.toml

```toml
[project]
name = "Package-B"
requires-python = ">=3.6"
# ...
```

#### setup.cfg

```ini
[metadata]
name = Package-B
#...

[options]
#...
python_requires = >=3.6
```

#### setup.py

```python
setup(
    name="Package-B",
    python_requires=">=3.6",
    ...,
)
```
