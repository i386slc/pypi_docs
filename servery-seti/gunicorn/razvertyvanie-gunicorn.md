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

## Мониторинг

## Логирование
