# gunicorn

* веб-сайт: [http://gunicorn.org](http://gunicorn.org)
* исходик: [https://github.com/benoitc/gunicorn](https://github.com/benoitc/gunicorn)
* баг-трекер: [https://github.com/benoitc/gunicorn/issues](https://github.com/benoitc/gunicorn/issues)
* IRC: `#gunicorn` на Freenode
* вопросы: [https://github.com/benoitc/gunicorn/issues](https://github.com/benoitc/gunicorn/issues)

**Gunicorn** «Green Unicorn» — это HTTP-сервер Python **WSGI** для UNIX. Это предварительная рабочая модель, портированная из проекта **Ruby Unicorn**. Сервер **Gunicorn** широко совместим с различными веб-фреймворками, прост в реализации, требует мало ресурсов сервера и довольно быстр.

## Особенности

* Нативно поддерживает WSGI, Django и Paster.
* Автоматическое управление рабочими процессами
* Простая конфигурация Python
* Несколько рабочих конфигураций
* Различные серверные хуки для расширения
* Совместимость с Python 3.x >= 3.5

## Содержание

* [Установка](ustanovka-gunicorn.md)
  * Из источника
  * Асинхронные воркеры
  * Дополнительные пакеты
  * Debian GNU/Linux
  * Ubuntu
* [Запуск Gunicorn](zapusk-gunicorn.md)
  * Команды
  * Интеграция
* [Обзор конфигурации](obzor-konfiguracii-gunicorn.md)
  * Командная строка
  * Файл конфигурации
  * Настройки фреймворка
* [Настройки](nastroika-gunicorn.md)
  * Файл конфигурации
  * Отладка
  * Логирование
  * Именование процессов
  * SSL
  * Безопасность
  * Серверные хуки
  * Механика сервера
  * Сокет сервера
  * Процессы воркера
* [Инструментарий](instrumentarii-gunicorn.md)
* [Развертывание Gunicorn](razvertyvanie-gunicorn.md)
  * Конфигурация Nginx
  * Использование Virtualenv
  * Мониторинг
  * Логирование
* [Обработка сигналов](obrabotka-signalov-gunicorn.md)
  * Основной процесс
  * Процесс воркера
  * Перезагрузка конфигурации
  * Обновление до нового бинарного файла на лету
* [Пользовательское приложение](polzovatelskoe-prilozhenie.md)
  * Прямое использование существующих приложений WSGI
* [Дизайн](dizain-gunicorn.md)
  * Модель сервера
  * Выбор типа воркера
  * Сколько воркеров?
  * Сколько потоков?
* [Вопросы-Ответы](voprosy-otvety-gunicorn.md)
  * Биты WSGI
  * Серверный штат
  * Процессы воркера
  * Параметры ядра
  * Исправление проблем
* [Сообщество](soobshestvo-gunicorn.md)
  * Управление проектами и обсуждения
  * IRC
  * Отслеживание проблем
  * Проблемы с безопасностью
* [Список изменений](spisok-izmenenii-gunicorn.md)
  * 20.1.0 - 2021-02-12
  * История
