# Пуш уведомления через Firebase

## Создаем проект на сайте Firebase

1. Нужно создать приложения для любимых фронтендеров и мобильных разработчиков

![Изображение приложений](../static/push_notifications/firebase.png)

2. Получить серверный ключ для `Django` в настройках проекта (Project Settings) во
вкладке *Cloud Messaging*

3. Не забудьте отправить конфигурационные файлы для фронтендеров и мобильных разработчиков,
чтобы они смогли настроить уведомления
   
## Устанавливаем зависимости
```shell
$ pip install django-push-notifications
```
[Документация к библиотеке](https://github.com/jazzband/django-push-notifications#readme)
## Настраиваем `settings.py`
```python
INSTALLED_APPS = (
        ...
        "push_notifications"
)

PUSH_NOTIFICATIONS_SETTINGS = {
        "FCM_API_KEY": "[your api key]",
}
```

## Поднимаем модели в БД
```shell
$ python manage.py migrate
```

## Делаем преднастройку

1. `serializers.py`
```python
from push_notifications.api.rest_framework import GCMDeviceSerializer, GCMDevice

class FCMDeviceSerializer(GCMDeviceSerializer):

    def create(self, validated_data):
        validated_data['cloud_message_type'] = "FCM"
        instance = GCMDevice.objects.create(
            **validated_data
        )
        return instance
```
2. `views.py`
   
```python
from push_notifications.api.rest_framework import GCMDeviceAuthorizedViewSet

from . import serializers

class FCMDeviceAuthorizedViewSet(GCMDeviceAuthorizedViewSet):
    serializer_class = serializers.FCMDeviceSerializer
```
3. `urls.py`

```python
router = SimpleRouter()

router.register(r'device/fcm', views.FCMDeviceAuthorizedViewSet)
```
4. `notifications.py` (нужно создать файлик)
```python
from push_notifications.models import GCMDevice
from . import models


def notify_user(user, message):
    devices = GCMDevice.objects.filter(user=user)
    devices.send_message(message)


def notify_many_users(users, message):
    for user in users:
        notify_user(user, message)


def notify_all(message):
    devices = GCMDevice.objects.all()
    devices.send_message(message)

```

Дальше во всех местах, где нужно уведомлять пользователей системы используй те методы, которые
тебе нужны.