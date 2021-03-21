# Полезные команды терминала в Ubuntu
+ [Оглавление](../README.md)
1. [Базовые команды](#base)
2. [journalctl](#journalctl)
3. [Crontab](#crontab)
4. [7z](#7z)

## <a name="base">Базовые команды</a>



## <a name="journalctl">journalctl</a>
### Прочитать файл журнала для сервиса
```bash
journalctl -u mysql.service -f
```


## <a name="crontab">Crontab</a>
### Посмотреть логи crontab
```bash
grep CRON /var/log/syslog
```
### Изменить файл crontab
```bash
EDITOR=nano crontab -e
```
## <a name="7z">7z</a>
### Распаковать 7z
```bash
7z x PACKAGE.7z 
```
### Запустить скрипт из джанги в фоне
```bash
python manage.py shell -c "exec(open(r'/var/www/analytics_marketplace/parser/items_update_adapter.py').read(), {})" &
```