# Старт проекта на Django

+ [Оглавление](../README.md)

### 1 Создать виртуальное окружение

```
python3 -m venv venv
```

### 2 Запустить виртуальное окружение

```
source venv/bin/activate
```
### 3 Стартуем проект
```bash
pip install django djangorestframework psycopg2-binary djangorestframework-simplejwt django-cors-headers
pip freeze > requirements.txt
django-admin.py startproject <name>
python manage.py startapp api
```
### 4 Добавить файл [.gitignore](gitignore.md)
### 5 Изменить INSTALLED_APPS
```python
INSTALLED_APPS = [
    ...
    'corsheaders',
    'rest_framework',
    'api',
]
```

### 6 Изменить конфигурацию БД

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

### 7 Добавляем Middleware для CORS
```python
MIDDLEWARE = [
    ...
    'corsheaders.middleware.CorsMiddleware',
    ...
]
```
### 8 Разрешаем подключение
```python
CORS_ORIGIN_ALLOW_ALL = True # If this is used then `CORS_ORIGIN_WHITELIST` will not have any effect
CORS_ALLOW_CREDENTIALS = True
```
### 9 Добавляем ссылочку на статику
```python
STATIC_ROOT = Path(BASE_DIR, 'static')
```
### 10 Настраиваем среду для Django
#### [Настраиваем Docker](../devops/django-docker.md) или [Настраиваем димончиков]
#### Настраиваем БД
+ [Настраиваем PostgreSQL](../database/start_postgres.md)
+ [Настраиваем MySQL]

### 11 Добавляем .env файлик или описываем в environment в docker-compose
```
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
```python
python manage.py migrate
```

## Что дальше?
+ [Авторизация, регистрация и аутентификация](authorization.md)
+ 
+