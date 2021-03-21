# Микросервисная авторизация
> В общем я не претендую на супер-правильное решение и объективность в этом вопросе. 
> Используй на свой страх и риск :)
+ [Оглавление](../README.md)
### Создаем файлик `authentication.py` в папке настроект проекта (там же где `settings.py`)
> Я использую либу djangorestframework-simplejwt для переопределения, так что нужно ее установить
```
pip install djangorestframework-simplejwt requests
pip freeze > requirements.txt
```
```python
from rest_framework_simplejwt import authentication, exceptions
from django.conf import settings
import requests


class User:

    def __init__(self, **kwargs):
        for key, value in kwargs.items():
            setattr(self, key, value)

    @property
    def is_authenticated(self):
        """
        Always return True. This is a way to tell if the user has been
        authenticated in templates.
        """
        return True


class MicroUsersAuthentication(authentication.JWTAuthentication):

    def get_validated_token(self, raw_token):
        headers = {
            "Authorization": f"Bearer {raw_token.decode('utf-8')}"
        }
        r = requests.get(settings.ERP_USERS_AUTH_URL, headers=headers)

        self.validate_status_code(r.status_code)

        return r.json()

    def get_user(self, response_data):
        data = self.get_user_fields(response_data)
        return User(**data)

    def validate_status_code(self, status_code):
        """
        Валидируем статус код ответа сервера, 
        это нужно, чтобы наш сервис не падал в случае отказа сервиса
        Users
        """
        if str(status_code).startswith("5"):
            raise exceptions.AuthenticationFailed("Micro Users is not available")
        if str(status_code).startswith("4"):
            raise exceptions.InvalidToken("Authentication failed")
        if status_code != 200:
            raise exceptions.AuthenticationFailed("Authentication failed")

    def get_user_fields(self, response_data):
        """
        Этот метод возвращает отвалидированный словарь с полями JWT токена
        """
        self.validate_user_fields(response_data)
        user_instance = response_data["result"]["user_instance"]

        user_id = user_instance.pop("user_id")
        role_id = user_instance.pop("role_id")
        company_id = user_instance.pop("company_id")
        status = user_instance.pop("user_status")

        data = {
            "id": user_id,
            "role_id": role_id,
            "company_id": company_id,
            "status": status,
            **user_instance
        }
        return data

    def validate_user_fields(self, response_data):
        """
        Валидируем поля JWT токена
        """
        if not response_data.get("result", None):
            raise exceptions.AuthenticationFailed("Invalid raw data from ERP Users")
        if not response_data["result"].get('user_instance', None):
            raise exceptions.AuthenticationFailed("User instance is not received from ERP Users")

        user_instance = response_data["result"]["user_instance"]

        if not user_instance.get("user_id", None):
            raise exceptions.AuthenticationFailed("User id is not received from ERP Users")
        if not user_instance.get("role_id", None):
            raise exceptions.AuthenticationFailed("User role id is not received from ERP Users")
        if not user_instance.get("user_status", None):
            raise exceptions.AuthenticationFailed("User status is not received from ERP Users")
        if user_instance["user_status"] != "ACTIVE":
            raise exceptions.AuthenticationFailed(('User is inactive'), code='user_inactive')
        if not user_instance.get("company_id", None):
            raise exceptions.AuthenticationFailed("User company id is not received from ERP Users")
```

### Нужно не забыть поменять файл `settings.py`
```python
REST_FRAMEWORK = {
    "DEFAULT_AUTHENTICATION_CLASSES": (
        'reports.authentication.MicroUsersAuthentication',
    )
}
```
