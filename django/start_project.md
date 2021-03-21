# Старт проекта на Django

+ [Оглавление](../README.md)

### Создать виртуальное окружение

```
python3 -m venv venv
```

### Запустить виртуальное окружение

```
source venv/bin/activate
```
### Стартуем проект
```bash
pip install django djangorestframework psycopg2-binary djangorestframework-simplejwt django-cors-headers
django-admin.py startproject <name>
python manage.py startapp api
```
### Добавить файл [.gitignore](gitignore.md)
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
### Добавляем .env файлик или описываем в environment в docker-compose
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
### Накатываем миграции
```python
python manage.py migrate
```


