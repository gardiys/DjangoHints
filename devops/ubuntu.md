# Ubuntu
+ [Оглавление](../README.md)
## Модели

### Распаковать 7z
```bash
7z x PACKAGE.7z 
```
### Запустить скрипт из джанги в фоне
```bash
python manage.py shell -c "exec(open(r'/var/www/analytics_marketplace/parser/items_update_adapter.py').read(), {})" &
```
