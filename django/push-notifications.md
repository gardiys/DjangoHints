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