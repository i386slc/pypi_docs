# Развертывание gunicorn

Мы настоятельно рекомендуем использовать **Gunicorn** за прокси-сервером.

## Конфигурация Nginx

Хотя доступно множество HTTP-прокси, мы настоятельно рекомендуем вам использовать [Nginx](https://nginx.org/). Если вы выберете другой прокси-сервер, вам нужно убедиться, что он буферизует медленных клиентов, когда вы используете рабочие процессы **Gunicorn** по умолчанию. Без этой буферизации **Gunicorn** будет легко подвержен атакам типа «отказ в обслуживании». Вы можете использовать [Hey](https://github.com/rakyll/hey), чтобы проверить, правильно ли ведет себя ваш прокси.

[Пример файла конфигурации](https://github.com/benoitc/gunicorn/blob/master/examples/nginx.conf) для быстрых клиентов с [Nginx](https://nginx.org/):

### nginx.conf

```nginx
worker_processes 1;

user nobody nogroup;
# 'user nobody nobody;' для систем с 'nobody' вместо группы
error_log  /var/log/nginx/error.log warn;
pid /var/run/nginx.pid;

events {
  worker_connections 1024; # увеличить, если у вас много клиентов
  accept_mutex off; # установите значение 'on', если nginx worker_processes > 1
  # 'use epoll;' включить для Linux 2.6+
  # 'use kqueue;' включить для FreeBSD, OSX
}

http {
  include mime.types;
  # запасной вариант на случай, если мы не сможем определить тип
  default_type application/octet-stream;
  access_log /var/log/nginx/access.log combined;
  sendfile on;

  upstream app_server {
    # fail_timeout=0 означает, что мы всегда повторяем восходящий поток,
    # даже если он не смог вернуть хороший HTTP-ответ.

    # для настройки сокета домена UNIX
    server unix:/tmp/gunicorn.sock fail_timeout=0;

    # для конфигурации TCP
    # server 192.168.0.7:8000 fail_timeout=0;
  }

  server {
    # если хост не совпадает, закройте соединение, чтобы предотвратить спуфинг хоста.
    listen 80 default_server;
    return 444;
  }

  server {
    # используем 'listen 80 deferred;' для Linux
    # используем 'listen 80 accept_filter=httpready;' для FreeBSD
    listen 80;
    client_max_body_size 4G;

    # установите правильный хост(ы) для вашего сайта
    server_name example.com www.example.com;

    keepalive_timeout 5;

    # путь для статических файлов
    root /path/to/app/current/public;

    location / {
      # проверяет статический файл, если не найден прокси для приложения
      try_files $uri @proxy_to_app;
    }

    location @proxy_to_app {
      proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
      proxy_set_header X-Forwarded-Proto $scheme;
      proxy_set_header Host $http_host;
      # мы не хотим, чтобы nginx пытался сделать что-то умное с перенаправлениями,
      # мы уже установили заголовок Host: выше.
      proxy_redirect off;
      proxy_pass http://app_server;
    }

    error_page 500 502 503 504 /500.html;
    location = /500.html {
      root /path/to/app/current/public;
    }
  }
}
```

Если вы хотите иметь возможность обрабатывать потоковые запросы/ответы или другие необычные функции, такие как **Comet**, длительный опрос или веб-сокеты, вам необходимо отключить буферизацию прокси-сервера. **Когда вы делаете это**, вы должны работать с одним из асинхронных классов воркеров.

Чтобы отключить буферизацию, нужно всего лишь добавить `proxy_buffering off`; к вашему блоку местоположения:

```nginx
...
location @proxy_to_app {
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    proxy_set_header Host $http_host;
    proxy_redirect off;
    proxy_buffering off;

    proxy_pass http://app_server;
}
...
```

Рекомендуется передавать информацию протокола в **Gunicorn**. Многие веб-фреймворки используют эту информацию для создания URL-адресов. Без этой информации приложение может ошибочно генерировать URL-адреса `'http'` в ответах `'https'`, что приводит к предупреждениям о смешанном содержании или неработающим приложениям. Чтобы настроить **Nginx** для передачи соответствующего заголовка, добавьте директиву **proxy\_set\_header** в свой блок **location**:

```nginx
...
proxy_set_header X-Forwarded-Proto $scheme;
...
```

Если вы используете **Nginx** на хосте, отличном от **Gunicorn**, вам нужно указать **Gunicorn** доверять заголовкам `X-Forwarded-*`, отправляемым **Nginx**. По умолчанию **Gunicorn** будет доверять этим заголовкам, только если соединение исходит с локального хоста. Это делается для того, чтобы злонамеренный клиент не подделывал эти заголовки:

```bash
$ gunicorn -w 3 --forwarded-allow-ips="10.170.3.217,10.170.3.220" test:app
```

Когда хост **Gunicorn** полностью защищен брандмауэром от внешней сети, так что все соединения исходят от доверенного прокси-сервера (например, **Heroku**), это значение может быть установлено на `'*'`. Использование этого значения **потенциально опасно**, если соединения с **Gunicorn** могут исходить от ненадежных прокси-серверов или напрямую от клиентов, так как приложение может быть обмануто для предоставления содержимого только для SSL через небезопасное соединение.

**Gunicorn 19** представил критическое изменение, касающееся обработки **REMOTE\_ADDR**. До **Gunicorn 19** для этого было установлено значение **X-Forwarded-For**, если оно было получено от доверенного прокси-сервера. Однако это не соответствовало [RFC 3875](https://tools.ietf.org/html/rfc3875.html), поэтому **REMOTE\_ADDR** теперь является IP-адресом прокси-сервера, а не фактическим пользователем.

Чтобы журналы доступа указывали **фактический IP-адрес пользователя** при проксировании, установите формат [access\_log\_format](nastroika-gunicorn.md#access\_log\_format) с форматом, включающим **X-Forwarded-For**. Например, в этом формате вместо **REMOTE\_ADDR** используется **X-Forwarded-For**:

```bash
%({x-forwarded-for}i)s %(l)s %(u)s %(t)s "%(r)s" %(s)s %(b)s "%(f)s" "%(a)s"
```

Также стоит отметить, что **REMOTE\_ADDR** будет полностью пустым, если вы привяжете **Gunicorn** к сокету UNIX, а не к кортежу TCP `host:port`.

## Использование Virtualenv

Чтобы обслуживать приложение из [Virtualenv](https://pypi.python.org/pypi/virtualenv), как правило, проще всего просто установить **Gunicorn** непосредственно в **Virtualenv**. Это создаст набор сценариев **Gunicorn** для этого **Virtualenv**, которые можно использовать для обычного запуска приложений.

Если у вас установлен **Virtualenv**, вы сможете сделать что-то вроде этого:

```bash
$ mkdir ~/venvs/
$ virtualenv ~/venvs/webapp
$ source ~/venvs/webapp/bin/activate
$ pip install gunicorn
$ deactivate
```

Тогда вам просто нужно использовать один из трех скриптов **Gunicorn**, которые были установлены в `~/venvs/webapp/bin`.

Примечание. Вы можете принудительно установить **Gunicorn** в **Virtualenv**, передав параметр `-I` или `--ignore-installed` для **pip**:

```bash
$ source ~/venvs/webapp/bin/activate
$ pip install -I gunicorn
```

## Мониторинг

{% hint style="info" %}
Убедитесь, что при использовании любого из этих сервисных мониторов вы не включаете режим демона **Gunicorn**. Эти мониторы ожидают, что процесс, который они запускают, будет процессом, который им нужно контролировать. Демонизация вызовет **fork-exec**, который создаст неконтролируемый процесс и, как правило, просто запутает службы мониторинга.
{% endhint %}

### Gaffer

#### Использование Gafferd и gaffer

[Gaffer](https://gaffer.readthedocs.io/) можно использовать для мониторинга **Gunicorn**. Простая конфигурация:

```ini
[process:gunicorn]
cmd = gunicorn -w 3 test:app
cwd = /path/to/project
```

Затем вы можете легко управлять **Gunicorn** с помощью [Gaffer](https://gaffer.readthedocs.io/).

#### Использование Procfile

Создайте **Procfile** в своем проекте:

```ini
gunicorn = gunicorn -w 3 test:app
```

Вы можете запускать любые другие приложения, которые должны быть запущены одновременно.

Затем вы можете запустить свое приложение **Gunicorn** с помощью [Gaffer](https://gaffer.readthedocs.io/):

```bash
gaffer start
```

Если **gafferd** запущен, вы также можете напрямую загрузить в него свой **Procfile**:

```bash
gaffer load
```

Затем все ваши заявки будут контролироваться **gaffrd**.

### Runit

Популярным методом развертывания **Gunicorn** является его мониторинг с помощью [runit](http://smarden.org/runit/). Вот [пример определения службы](https://github.com/benoitc/gunicorn/blob/master/examples/gunicorn\_rc):

```bash
#!/bin/sh

GUNICORN=/usr/local/bin/gunicorn
ROOT=/path/to/project
PID=/var/run/gunicorn.pid

APP=main:application

if [ -f $PID ]; then rm $PID; fi

cd $ROOT
exec $GUNICORN -c $ROOT/gunicorn.conf.py --pid=$PID $APP
```

Сохраните это как `/etc/sv/[app_name]/run` и сделайте его исполняемым (`chmod u+x /etc/sv/[app_name]/run`). Затем запустите `ln -s /etc/sv/[app_name] /etc/service/[app_name]`. Если **runit** установлен, **Gunicorn** должен запуститься автоматически, как только вы создадите символическую ссылку.

Если он не запускается автоматически, запустите скрипт напрямую для устранения неполадок.

### Supervisor

Еще одним полезным инструментом для мониторинга и управления **Gunicorn** является [Supervisor](http://supervisord.org/). Простая [конфигурация](https://github.com/benoitc/gunicorn/blob/master/examples/supervisor.conf):

```ini
[program:gunicorn]
command=/path/to/gunicorn main:application -c /path/to/gunicorn.conf.py
directory=/path/to/project
user=nobody
autostart=true
autorestart=true
redirect_stderr=true
```

### Upstart

Использовать **Gunicorn** с **upsrart** очень просто. В этом примере мы запустим приложение `"myapp"` из виртуальной среды. Все ошибки будут попадать в `/var/log/upstart/myapp.log`.

#### **/etc/init/myapp.conf**:

```ini
description "myapp"

start on (filesystem)
stop on runlevel [016]

respawn
setuid nobody
setgid nogroup
chdir /path/to/app/directory

exec /path/to/virtualenv/bin/gunicorn myapp:app
```

### Systemd

Инструмент, который становится распространенным в Linux-системах, — это [Systemd](https://www.freedesktop.org/wiki/Software/systemd/). Это менеджер системных служб, который позволяет строго управлять процессами, ресурсами и разрешениями.

Ниже приведены файлы конфигурации и инструкции по использованию **systemd** для создания сокета unix для входящих запросов **Gunicorn**. **Systemd** будет прослушивать этот сокет и автоматически запускать **gunicorn** в ответ на трафик. Далее в этом разделе приведены инструкции по настройке **Nginx** для перенаправления веб-трафика во вновь созданный сокет **unix**:

#### **/etc/systemd/system/gunicorn.service**:

```ini
[Unit]
Description=gunicorn daemon
Requires=gunicorn.socket
After=network.target

[Service]
Type=notify
# конкретный пользователь, от имени которого будет работать наша служба
User=someuser
Group=someuser
# другой вариант для еще более ограниченного сервиса
# DynamicUser=yes
# see http://0pointer.net/blog/dynamic-users-with-systemd.html
RuntimeDirectory=gunicorn
WorkingDirectory=/home/someuser/applicationroot
ExecStart=/usr/bin/gunicorn applicationname.wsgi
ExecReload=/bin/kill -s HUP $MAINPID
KillMode=mixed
TimeoutStopSec=5
PrivateTmp=true

[Install]
WantedBy=multi-user.target
```

#### **/etc/systemd/system/gunicorn.socket**:

```ini
[Unit]
Description=gunicorn socket

[Socket]
ListenStream=/run/gunicorn.sock
# Нашему сервису не потребуются разрешения для сокета, так как он наследует
# файловый дескриптор при активации сокета, доступ к сокету потребуется
# только демону nginx.
SocketUser=www-data
# При желании еще больше ограничьте права доступа к сокету.
# SocketMode=600

[Install]
WantedBy=sockets.target
```

Затем включите и запустите сокет (он также будет автоматически запускаться при загрузке):

```bash
systemctl enable --now gunicorn.socket
```

Теперь давайте посмотрим, сможет ли демон **nginx** подключиться к сокету. Запустив `sudo -u www-data curl --unix-socket /run/gunicorn.sock http`, наша служба **Gunicorn** будет автоматически запущена, и вы должны увидеть HTML-код с вашего сервера в терминале.

{% hint style="info" %}
**systemd** использует контрольные группы для отслеживания процессов службы, поэтому ему не нужны файлы **pid**. В редком случае, когда вам нужно узнать основной **pid** службы, вы можете использовать `systemctl show --value -p MainPID gunicorn.service`, но если вы хотите только отправить сигнал, еще лучшим вариантом будет `systemctl kill -s HUP gunicorn.service`.
{% endhint %}

{% hint style="info" %}
**www-data** — это пользователь **nginx** по умолчанию в **debian**, другие дистрибутивы используют других пользователей (например: **http** или **nginx**). Проверьте свой дистрибутив, чтобы узнать, что указать для пользователя сокета и для команды **sudo**.
{% endhint %}

Теперь вы должны настроить свой веб-прокси для отправки трафика на новый сокет **Gunicorn**. Отредактируйте ваш **nginx.conf**, включив в него следующее:

#### **/etc/nginx/nginx.conf**:

```nginx
user www-data;
...
http {
    server {
        listen          8000;
        server_name     127.0.0.1;
        location / {
            proxy_pass http://unix:/run/gunicorn.sock;
        }
    }
}
...
```

{% hint style="info" %}
Используемые здесь прослушивание и **server\_name** настроены для локальной машины. На рабочем сервере вы, скорее всего, будете прослушивать порт **80** и использовать свой URL-адрес в качестве **server\_name**.
{% endhint %}

Теперь убедитесь, что вы включили службу **nginx**, чтобы она автоматически запускалась при загрузке:

```bash
systemctl enable nginx.service
```

Либо перезагрузитесь, либо запустите **Nginx** с помощью следующей команды:

```bash
systemctl start nginx
```

Теперь вы сможете протестировать **Nginx** с **Gunicorn**, посетив **http://127.0.0.1:8000/** в любом веб-браузере. **Systemd** настроен.

## Логирование

Ведение журнала можно настроить с помощью различных флагов, подробно описанных в [документации по настройке](nastroika-gunicorn.md#logirovanie), или путем создания [файла конфигурации ведения журнала](https://github.com/benoitc/gunicorn/blob/master/examples/logging.conf). Отправьте сигнал **USR1** для ротации журналов, если вы используете утилиту **logrotate**:

```bash
kill -USR1 $(cat /var/run/gunicorn.pid)
```

{% hint style="info" %}
Для переопределения словаря **LOGGING** необходимо установить `disable_existing_loggers: False`, чтобы не мешать ведению журнала **Gunicorn**.
{% endhint %}

{% hint style="warning" %}
Журнал ошибок **Gunicorn** предназначен для регистрации ошибок **Gunicorn**, а не другого приложения.
{% endhint %}
