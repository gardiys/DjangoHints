# Старт проекта на Django

+ [Оглавление](../README.md)

### Создать виртуальное окружение

```shell
$ python3 -m venv venv
```

### Запустить виртуальное окружение

```shell
$ source venv/bin/activate
```
### Стартуем проект
```shell
$ pip install django djangorestframework psycopg2-binary djangorestframework-simplejwt django-cors-headers
$ pip freeze > requirements.txt
$ django-admin.py startproject <name>
$ python manage.py startapp api
```

### Добавить файл [.gitignore](gitignore.md)

### Скрываем переменные для `.env`
```python
# SECURITY WARNING: keep the secret key used in production secret!
SECRET_KEY = str(env("SECRET_KEY")) or get_random_secret_key()

# SECURITY WARNING: don't run with debug turned on in production!
DEBUG = int(env("DEBUG"))
```
### Изменить INSTALLED_APPS
```python
INSTALLED_APPS = [
    ...
    'corsheaders',
    'rest_framework',
    'api',
]
```

### Изменить конфигурацию БД

```python
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.postgresql_psycopg2',
        'NAME': os.environ.get("SQL_DATABASE"),
        'USER': os.environ.get("SQL_USER"),
        'PASSWORD': os.environ.get("SQL_PASSWORD"),
        'HOST': os.environ.get("SQL_HOST"),
        'PORT': os.environ.get("SQL_PORT"),
    }
}
```
### (Дополнительно) Можно включить постоянные соединения

### Добавляем Middleware для CORS
```python
MIDDLEWARE = [
    ...
    'corsheaders.middleware.CorsMiddleware',
    ...
]
```
### Разрешаем подключение
```python
CORS_ORIGIN_ALLOW_ALL = True # If this is used then `CORS_ORIGIN_WHITELIST` will not have any effect
CORS_ALLOW_CREDENTIALS = True
```
### Добавляем ссылочку на статику
```python
STATIC_ROOT = Path(BASE_DIR, 'static')
```
### Настраиваем среду для Django
#### [Настраиваем Docker](../devops/django-docker.md) или [Настраиваем димончиков]
#### Настраиваем БД
+ [Настраиваем PostgreSQL](../database/start_postgres.md)
+ [Настраиваем MySQL]

### Добавляем .env файлик или описываем в environment в docker-compose
```dotenv
SECRET_KEY=<secret_key>
DEBUG=0
DJANGO_ALLOWED_HOSTS=*
SQL_DATABASE=<db_name>
SQL_USER=<db_user>
SQL_PASSWORD=<db_password>
SQL_HOST=<db_host>
SQL_PORT=5432
```
### 12 Накатываем миграции
```shell
$ python manage.py migrate
```

## Что дальше?
+ [Авторизация, регистрация и аутентификация](authorization.md)
+ [Подключаем изображения, картинки и остальные медиа файлы]
+