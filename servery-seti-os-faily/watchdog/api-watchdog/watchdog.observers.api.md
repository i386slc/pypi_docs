# watchdog.observers.api

## Неизменяемые объекты

### ObservedWatch

#### _class_ watchdog.observers.api.ObservedWatch(_path_, _recursive_)

\[[Исходник](https://python-watchdog.readthedocs.io/en/stable/\_modules/watchdog/observers/api.html#ObservedWatch)].

**Bases**: object

Запланированное отслеживание.

**Параметры**:

* **path** - Строка пути.
* **recursive** - `True`, если отслеживание рекурсивно; `False` в противном случае.

#### is\_recursive

Определяет, отслеживаются ли подкаталоги для пути.

#### path

Путь, который отслеживается.

## Коллекции

### EventQueue

#### _class_ watchdog.observers.api.EventQueue(_maxsize=0_)

\[[Исходник](https://python-watchdog.readthedocs.io/en/stable/\_modules/watchdog/observers/api.html#EventQueue)].

**Bases**: watchdog.utils.bricks.SkipRepeatsQueue

Потокобезопасная очередь событий, основанная на специальной очереди, которая пропускает добавление одного и того же события ([FileSystemEvent](watchdog.events.md#filesystemevent)) несколько раз подряд. Таким образом, можно избежать диспетчеризации нескольких вызовов обработки событий, когда несколько идентичных событий создаются быстрее, чем наблюдатель может их обработать.

## Классы

### EventEmitter

#### _class_ watchdog.observers.api.EventEmitter(_event\_queue_, _watch_, _timeout=1_)

\[[Исходник](https://python-watchdog.readthedocs.io/en/stable/\_modules/watchdog/observers/api.html#EventEmitter)].

**Bases**: watchdog.utils.BaseThread

Базовый класс потока-производителя, подкласс которого составляют эмиттеры событий, которые генерируют события и заполняют ими очередь.

**Параметры**:

* **event\_queue** (watchdog.events.EventQueue) - Очередь событий для заполнения сгенерированными событиями.
* **watch** ([ObservedWatch](watchdog.observers.api.md#observedwatch)) - watch для наблюдения и производства событий.
* **timeout** (float) - Время ожидания (в секундах) между последовательными попытками чтения событий.

#### queue\_event(_event_)

\[[Исходник](https://python-watchdog.readthedocs.io/en/stable/\_modules/watchdog/observers/api.html#EventEmitter.queue\_event)].

Ставит в очередь одно событие.

**Параметры**: **event**(экземпляр [watchdog.events.FileSystemEvent](watchdog.events.md#filesystemevent) или подкласс.) - Событие для постановки в очередь.

#### queue\_events(_timeout_)

\[[Исходник](https://python-watchdog.readthedocs.io/en/stable/\_modules/watchdog/observers/api.html#EventEmitter.queue\_events)].

Переопределите этот метод, чтобы заполнить очередь событий событиями за период интервала.

**Параметры**: **timeout**(float) - Время ожидания (в секундах) между последовательными попытками чтения событий.

#### run()

\[[Исходник](https://python-watchdog.readthedocs.io/en/stable/\_modules/watchdog/observers/api.html#EventEmitter.run)].

Метод, представляющий активность потока.

Вы можете переопределить этот метод в подклассе. Стандартный метод `run()` вызывает вызываемый объект, переданный конструктору объекта в качестве целевого аргумента, если он есть, с последовательными и ключевыми аргументами, взятыми из аргументов **args** и **kwargs** соответственно.

#### timeout

Тайм-аут блокировки для чтения событий.

#### watch

Watch, связанные с этим emitter.

### EventDispatcher

#### _class_ watchdog.observers.api.EventDispatcher(_timeout=1_)

\[[Исходник](https://python-watchdog.readthedocs.io/en/stable/\_modules/watchdog/observers/api.html#EventDispatcher)].

**Bases**: watchdog.utils.BaseThread

Базовый класс потока-потребителя, подкласс которого составляют потоки-наблюдатели за событиями, которые отправляют события из очереди событий в соответствующие обработчики событий.

**Параметры**: **timeout**(float) - Тайм-аут блокировки очереди событий (в секундах).

#### dispatch\_events(_event\_queue_, _timeout_)

\[[Исходник](https://python-watchdog.readthedocs.io/en/stable/\_modules/watchdog/observers/api.html#EventDispatcher.dispatch\_events)].

Переопределите этот метод, чтобы получать события из очереди событий, блокируя очередь на указанный тайм-аут, прежде чем вызывать queue.Empty.

**Параметры**:

* **event\_queue** (EventQueue) - Очередь событий для заполнения одним набором событий.
* **timeout** (float) - Интервал ожидания (в секундах) перед истечением времени ожидания в очереди событий.

**Поднимает**: queue.Empty

#### event\_queue

Очередь событий, которая заполняется событиями файловой системы эмиттерами и из которой события отправляются потоком диспетчера.

#### run()

\[[Исходник](https://python-watchdog.readthedocs.io/en/stable/\_modules/watchdog/observers/api.html#EventDispatcher.run)].

Метод, представляющий активность потока.

Вы можете переопределить этот метод в подклассе. Стандартный метод `run()` вызывает вызываемый объект, переданный конструктору объекта в качестве целевого аргумента, если он есть, с последовательными и ключевыми аргументами, взятыми из аргументов **args** и **kwargs** соответственно.

#### timeout

Тайм-аут блокировки очереди событий.

### BaseObserver

#### _class_ watchdog.observers.api.BaseObserver(_emitter\_class_, _timeout=1_)

\[[Исходник](https://python-watchdog.readthedocs.io/en/stable/\_modules/watchdog/observers/api.html#BaseObserver)].

**Bases**: [watchdog.observers.api.EventDispatcher](watchdog.observers.api.md#eventdispatcher)

Базовый наблюдатель.

#### add\_handler\_for\_watch(_event\_handler_, _watch_)

\[[Исходник](https://python-watchdog.readthedocs.io/en/stable/\_modules/watchdog/observers/api.html#BaseObserver.add\_handler\_for\_watch)].

Добавляет обработчик для данного отслеживания.

**Параметры**:

* **event\_handler** ([watchdog.events.FileSystemEventHandler](watchdog.events.md#filesystemeventhandler) или подкласс) - Экземпляр обработчика событий, который имеет соответствующие методы обработки событий, которые будут вызываться наблюдателем в ответ на события файловой системы.
* **watch** (экземпляр [ObservedWatch](watchdog.observers.api.md#observedwatch) или подкласс ObservedWatch) - watch, для которого нужно добавить обработчик.

#### dispatch\_events(_event\_queue_, _timeout_)

\[[Исходник](https://python-watchdog.readthedocs.io/en/stable/\_modules/watchdog/observers/api.html#BaseObserver.dispatch\_events)].

**Параметры**:

* **event\_queue** (EventQueue) - Очередь событий для заполнения одним набором событий.
* **timeout** (float) - Интервал ожидания (в секундах) перед истечением времени ожидания в очереди событий.

**Поднимает**: queue.Empty

#### emitters

Возвращает генератор событий, созданный этим наблюдателем.

#### on\_thread\_stop()

\[[Исходник](https://python-watchdog.readthedocs.io/en/stable/\_modules/watchdog/observers/api.html#BaseObserver.on\_thread\_stop)].

Переопределите этот метод вместо `stop()`. `stop()` вызывает этот метод.

Этот метод вызывается сразу после того, как поток получает сигнал об остановке.

#### remove\_handler\_for\_watch(_event\_handler_, _watch_)

\[[Исходник](https://python-watchdog.readthedocs.io/en/stable/\_modules/watchdog/observers/api.html#BaseObserver.remove\_handler\_for\_watch)].

Удаляет обработчик для данного watch.

**Параметры**:

* **event\_handler** ([watchdog.events.FileSystemEventHandler](watchdog.events.md#filesystemeventhandler) или подкласс) - Экземпляр обработчика событий, который имеет соответствующие методы обработки событий, которые будут вызываться наблюдателем в ответ на события файловой системы.
* **watch** (экземпляр [ObservedWatch](watchdog.observers.api.md#observedwatch) или подкласс ObservedWatch) - watch, для которого нужно удалить обработчик.

#### schedule(_event\_handler_, _path_, _recursive=False_)

\[[Исходник](https://python-watchdog.readthedocs.io/en/stable/\_modules/watchdog/observers/api.html#BaseObserver.schedule)].

Планирует просмотр пути и вызывает соответствующие методы, указанные в данном обработчике событий, в ответ на события файловой системы.

**Параметры**:

* **event\_handler** ([watchdog.events.FileSystemEventHandler](watchdog.events.md#filesystemeventhandler) или подкласс) - Экземпляр обработчика событий, который имеет соответствующие методы обработки событий, которые будут вызываться наблюдателем в ответ на события файловой системы.
* **path** (str) - Путь к каталогу, который будет отслеживаться.
* **recursive** (bool) - `True`, если события будут генерироваться для подкаталогов, проходимых рекурсивно; `False` в противном случае.

**Возвращает**: экземпляр объекта [ObservedWatch](watchdog.observers.api.md#observedwatch), представляющий watch.

#### start()

\[[Исходник](https://python-watchdog.readthedocs.io/en/stable/\_modules/watchdog/observers/api.html#BaseObserver.start)].

Запустите активность потока.

Он должен вызываться не более одного раза для каждого объекта потока. Он организует вызов метода `run()` объекта в отдельном потоке управления.

Этот метод вызовет ошибку **RuntimeError**, если он вызывается более одного раза для одного и того же объекта потока.

#### unschedule(_watch_)

\[[Исходник](https://python-watchdog.readthedocs.io/en/stable/\_modules/watchdog/observers/api.html#BaseObserver.unschedule)].

Отменяет расписание отслеживания.

**Параметры**:

* **watch** (экземпляр [ObservedWatch](watchdog.observers.api.md#observedwatch) или подкласс ObservedWatch) - отслеживание, которое нужно отменить.

#### unschedule\_all()

\[[Исходник](https://python-watchdog.readthedocs.io/en/stable/\_modules/watchdog/observers/api.html#BaseObserver.unschedule\_all)].

Отменяет расписание всех наблюдений и отсоединяет все связанные обработчики событий.
