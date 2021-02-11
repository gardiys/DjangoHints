# DjangoHints
Решение различных кейсов в процессе разработки проектов на Django (Конструктор Django проектов v1)

## Оглавление
1. [Dev-ops]
  + [Nginx]
  + [Docker]
  + [Ubuntu]
  + [Gunicorn]
  
2. [Django]
3. [Django REST Framework]
# Полезные ссылки для работы
## Сигналы
+ [Django Signals](https://simpleisbetterthancomplex.com/tutorial/2016/07/28/how-to-create-django-signals.html)
## Dev-ops
+ [Настройка Django с Postgres, Nginx и Gunicorn в Ubuntu 18.04](https://www.digitalocean.com/community/tutorials/how-to-set-up-django-with-postgres-nginx-and-gunicorn-on-ubuntu-18-04-ru)
+ [How To Secure Nginx with Let's Encrypt on Ubuntu 18.04](https://www.digitalocean.com/community/tutorials/how-to-secure-nginx-with-let-s-encrypt-on-ubuntu-18-04)
+ [How To Create a Self-Signed SSL Certificate for Nginx in Ubuntu 18.04](https://www.digitalocean.com/community/tutorials/how-to-create-a-self-signed-ssl-certificate-for-nginx-in-ubuntu-18-04)
+ [Как установить и использовать Docker в Ubuntu 18.04](https://www.digitalocean.com/community/tutorials/docker-ubuntu-18-04-1-ru)
+ [Как использовать Django, PostgreSQL и Docker](https://webdevblog.ru/kak-ispolzovat-django-postgresql-i-docker/)
+ [Supervisor](https://ruhighload.com/Запуск+процессов+в+supervisor)
```bash
journalctl -u mysql.service -f
```
## Django-telegram
+ [Bots](https://qna.habr.com/q/736377)
+ [Репозиторий с готовым ботом](https://github.com/adilkhash/planetpython_telegrambot)
Для сборки и пуша образа на локалке рекомендуется использовать команду:
-----
```bash
docker build -t sixhands/<название проекта> .
docker push sixhands/<название проекта>
```

Дла загрузки контейнера с Docker Hub на сервер, нужно на сервере запустить команду.
```bash
docker-compose pull
```
Файл должен содержать строку
```bash
image: sixhands/<название проекта>
```

## PostgreSQL
+ Запустить сервер для пользователя MacOS

```
pg_ctl -D /usr/local/var/postgres start
```
+ Перезапустить сервер macOs
```
brew services restart postgres
```
+ Удалить каскадно
```
TRUNCATE some_table CASCADE;
```
Адрес management tools http://127.0.0.1:51774/browser/

## Сообщения и чатики
+ [WebSocket](https://prognote.ru/web-dev/beck-end/how-to-make-a-chat-in-python-django/)

## Virtualenv
+ Создать виртуальное окружение

```
python3 -m venv venv
```

+ Запустить виртуальное окружение

```
source venv/bin/activate
```
# Алгоритмы
+ [30 наиболее полезных алгосов и структур данных](https://dev.to/iuliagroza/complete-introduction-to-the-30-most-essential-data-structures-algorithms-43kd)

# Старт проекта на Django
```bash
pip install django djangorestframework psycopg2-binary djangorestframework-simplejwt django-cors-headers
django-admin.py startproject <name>
python manage.py startapp api
```
Добавить файл [.gitignore](https://gist.github.com/santoshpurbey/6f982faf1eacdac153ffd86a3a694239)
```python
INSTALLED_APPS = [
    ...
    'corsheaders',
    'rest_framework',
    'api',
]
```
Изменить конфигурацию 
```python
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.postgresql_psycopg2',
        'NAME': '<db_name>',
        'USER': '<db_user>',
        'PASSWORD': '<user_password>',
        'HOST': '127.0.0.1',
        'PORT': '5432',
    }
}
```
```python
MIDDLEWARE = [
    ...
    'corsheaders.middleware.CorsMiddleware',
    'django.middleware.common.CommonMiddleware',
    ...
]
```
```python
CORS_ORIGIN_ALLOW_ALL = True # If this is used then `CORS_ORIGIN_WHITELIST` will not have any effect
CORS_ALLOW_CREDENTIALS = True
```
Накатить миграции
```python
python manage.py migrate
```
Запустить скрипт в shell
```python
./manage.py shell < myscript.py
```
> Можно сюда добавить картиночки
# Ссылки по Django
+ [Djoser](https://djoser.readthedocs.io/en/latest/introduction.html)
+ [Simple JWT](https://django-rest-framework-simplejwt.readthedocs.io/en/latest/getting_started.html)
+ [Django REST Framework](https://www.django-rest-framework.org)
+ [Django Filter](https://django-filter.readthedocs.io/en/master/guide/usage.html)

# Email
+ [Гугл](https://www.hostinger.ru/rukovodstva/kak-ispolzovat-smtp-server)
+ [Яндекс](https://netpoint-dc.com/blog/nastroika-pochti-dlya-raboti-cherez-smtp-yandex/)

# Bulk M2M
```python
Users().save()
Users().save()

# Access the through model directly
ThroughModel = Sample.users.through

users = Users.objects.filter(pk__in=[1,2])

sample_object = Sample()
sample_object.save()

ThroughModel.objects.bulk_create([
    ThroughModel(users_id=users[0].pk, sample_id=sample_object.pk),
    ThroughModel(users_id=users[1].pk, sample_id=sample_object.pk)
])
```
