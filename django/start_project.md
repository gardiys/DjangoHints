# Старт проекта на Django

+ [Оглавление](../README.md)

```bash
pip install django djangorestframework psycopg2-binary djangorestframework-simplejwt django-cors-headers
django-admin.py startproject <name>
python manage.py startapp api
```
Добавить файл [.gitignore](gitignore.md)

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
    ...
]
```
```python
STATIC_ROOT = Path(BASE_DIR, 'static')
CORS_ORIGIN_ALLOW_ALL = True # If this is used then `CORS_ORIGIN_WHITELIST` will not have any effect
CORS_ALLOW_CREDENTIALS = True
```
Накатить миграции
```python
python manage.py migrate
```