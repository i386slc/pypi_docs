# supervisor

**Supervisor** — это клиент-серверная система, позволяющая пользователям отслеживать и контролировать ряд процессов в UNIX-подобных операционных системах.

Он разделяет некоторые цели таких программ, как [launchd](http://supervisord.org/glossary.html#term-launchd), [daemontools](http://supervisord.org/glossary.html#term-daemontools) и [runit](http://supervisord.org/glossary.html#term-runit). В отличие от некоторых из этих программ, она не предназначена для запуска в качестве замены **init** как «process id 1». Вместо этого он предназначен для управления процессами, связанными с проектом или клиентом, и должен запускаться, как и любая другая программа, во время загрузки.

## Повествовательная  документация

* Введение
  * Обзор
  * Функции
  * Компоненты супервизора
  * &#x20;Требования к платформе
* Установка
  * Установка в систему с доступом в Интернет
  * Установка в систему без доступа в Интернет
  * Установка дистрибутива
  * Создание файла конфигурации
* Запуск супервизора
  * Добавление программы
  * Запуск supervisord
  * Запуск supervisortcl
  * Сигналы
  * Безопасность во время выполнения
  * Запуск supervisord автоматически при запуске
* Конфигурационный файл
  * Формат файла
  * Настройки раздела `[unix_http_server]`
  * Настройки раздела `[inet_http_server]`
  * Настройки раздела `[supervisord]`
  * Настройки раздела `[supervisorctl]`
  * Настройки раздела `[program:x]`
  * Настройки раздела `[include]`
  * Настройки раздела `[group:x]`
  * Настройки раздела `[fcgi-program:x]`
  * Настройки раздела `[eventlistener:x]`
  * Настройки раздела `[rpcinterface:x]`
* Подпроцессы
  * Демонизация подпроцессов
  * Программа pidproxy
  * Среда подпроцесса
  * Состояния процесса
* Ведение журнала
  * Журнал активности
  * Журналы дочерних процессов
* События
  * Слушатели событий и уведомления о событиях
  * Типы событий
* Расширение API Supervisor XML-RPC
  * Настройка фабрик интерфейсов XML-RPC
* Обновление Supervisor 2 до 3
* Часто задаваемые вопросы
* Ресурсы и развитие
  * Баг трекер
  * Репозиторий контроля версий
  * Содействие
  * Информация об авторе
* Глоссарий

## Документация API

* Документация API XML-RPC
  * Статус и контроль
  * Контроль над процессом
  * Ведение журнала процессов
  * Системные методы

## Плагины

* Сторонние приложения и библиотеки
  * Панели мониторинга и инструменты для нескольких экземпляров Supervisor
  * Сторонние подключаемые модули и библиотеки для Supervisor
  * Библиотеки, которые интегрируют сторонние приложения с Supervisor

## История релизов

* [Changelog](http://supervisord.org/changes.html)
  * [4.2.5 (2022-12-23)](http://supervisord.org/changes.html#id1)
  * [4.2.4 (2021-12-30)](http://supervisord.org/changes.html#id2)
  * [4.2.3 (2021-12-27)](http://supervisord.org/changes.html#id3)
  * [4.2.2 (2021-02-26)](http://supervisord.org/changes.html#id4)
  * [4.2.1 (2020-08-20)](http://supervisord.org/changes.html#id5)
  * [4.2.0 (2020-04-30)](http://supervisord.org/changes.html#id6)
  * [4.1.0 (2019-10-19)](http://supervisord.org/changes.html#id7)
  * [4.0.4 (2019-07-15)](http://supervisord.org/changes.html#id8)
  * [4.0.3 (2019-05-22)](http://supervisord.org/changes.html#id9)
  * [4.0.2 (2019-04-17)](http://supervisord.org/changes.html#id10)
  * [4.0.1 (2019-04-10)](http://supervisord.org/changes.html#id11)
  * [4.0.0 (2019-04-05)](http://supervisord.org/changes.html#id12)
  * [3.4.0 (2019-04-05)](http://supervisord.org/changes.html#id13)
  * [3.3.5 (2018-12-22)](http://supervisord.org/changes.html#id14)
  * [3.3.4 (2018-02-15)](http://supervisord.org/changes.html#id15)
  * [3.3.3 (2017-07-24)](http://supervisord.org/changes.html#id16)
  * [3.3.2 (2017-06-03)](http://supervisord.org/changes.html#id17)
  * [3.3.1 (2016-08-02)](http://supervisord.org/changes.html#id18)
  * [3.3.0 (2016-05-14)](http://supervisord.org/changes.html#id19)
  * [3.2.4 (2017-07-24)](http://supervisord.org/changes.html#id20)
  * [3.2.3 (2016-03-19)](http://supervisord.org/changes.html#id21)
  * [3.2.2 (2016-03-04)](http://supervisord.org/changes.html#id22)
  * [3.2.1 (2016-02-06)](http://supervisord.org/changes.html#id23)
  * [3.2.0 (2015-11-30)](http://supervisord.org/changes.html#id24)
  * [3.1.4 (2017-07-24)](http://supervisord.org/changes.html#id25)
  * [3.1.3 (2014-10-28)](http://supervisord.org/changes.html#id26)
  * [3.1.2 (2014-09-07)](http://supervisord.org/changes.html#id27)
  * [3.1.1 (2014-08-11)](http://supervisord.org/changes.html#id28)
  * [3.1.0 (2014-07-29)](http://supervisord.org/changes.html#id29)
  * [3.0.1 (2017-07-24)](http://supervisord.org/changes.html#id30)
  * [3.0 (2013-07-30)](http://supervisord.org/changes.html#id31)
  * [3.0b2 (2013-05-28)](http://supervisord.org/changes.html#b2-2013-05-28)
  * [3.0b1 (2012-09-10)](http://supervisord.org/changes.html#b1-2012-09-10)
  * [3.0a12 (2011-12-06)](http://supervisord.org/changes.html#a12-2011-12-06)
  * [3.0a11 (2011-12-06)](http://supervisord.org/changes.html#a11-2011-12-06)
  * [3.0a10 (2011-03-30)](http://supervisord.org/changes.html#a10-2011-03-30)
  * [3.0a9 (2010-08-13)](http://supervisord.org/changes.html#a9-2010-08-13)
  * [3.0a8 (2010-01-20)](http://supervisord.org/changes.html#a8-2010-01-20)
  * [3.0a7 (2009-05-24)](http://supervisord.org/changes.html#a7-2009-05-24)
  * [3.0a6 (2008-04-07)](http://supervisord.org/changes.html#a6-2008-04-07)
  * [3.0a5 (2008-03-13)](http://supervisord.org/changes.html#a5-2008-03-13)
  * [3.0a4 (2008-01-30)](http://supervisord.org/changes.html#a4-2008-01-30)
  * [3.0a3 (2007-10-02)](http://supervisord.org/changes.html#a3-2007-10-02)
  * [3.0a2 (2007-08-24)](http://supervisord.org/changes.html#a2-2007-08-24)
  * [3.0a1 (2007-08-16)](http://supervisord.org/changes.html#a1-2007-08-16)
  * [2.2b1 (2007-03-31)](http://supervisord.org/changes.html#b1-2007-03-31)
  * [2.1 (2007-03-17)](http://supervisord.org/changes.html#id32)
  * [2.1b1 (2006-08-30)](http://supervisord.org/changes.html#b1-2006-08-30)
  * [2.0 (2006-08-30)](http://supervisord.org/changes.html#id33)
  * [2.0b1 (2006-07-12)](http://supervisord.org/changes.html#b1-2006-07-12)
  * [1.0.7 (2006-07-11)](http://supervisord.org/changes.html#id34)
  * [1.0.6 (2005-11-20)](http://supervisord.org/changes.html#id35)
  * [1.0.5 (2004-07-29)](http://supervisord.org/changes.html#id36)
  * [1.0.4 or “Alpha 4” (2004-06-30)](http://supervisord.org/changes.html#or-alpha-4-2004-06-30)
  * [1.0.3 or “Alpha 3” (2004-05-26)](http://supervisord.org/changes.html#or-alpha-3-2004-05-26)
  * [1.0.2 or “Alpha 2” (Unreleased)](http://supervisord.org/changes.html#or-alpha-2-unreleased)
  * [1.0.0 or “Alpha 1” (Unreleased)](http://supervisord.org/changes.html#or-alpha-1-unreleased)

## Индексы и таблицы

* [Index](http://supervisord.org/genindex.html)
* [Module Index](http://supervisord.org/py-modindex.html)
* [Search Page](http://supervisord.org/search.html)
