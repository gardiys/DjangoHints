## PostgreSQL
+ [Оглавление](../README.md)

+ Запустить сервер для пользователя MacOS

```
pg_ctl -D /usr/local/var/postgres start
```
+ Создать БД + пользователя с правами
```
sudo -u postgres psql
CREATE DATABASE myproject;
CREATE USER gardiys WITH PASSWORD 'password';
ALTER ROLE gardiys SET client_encoding TO 'utf8';
ALTER ROLE gardiys SET default_transaction_isolation TO 'read committed';
ALTER ROLE myprojectuser SET timezone TO 'UTC';
GRANT ALL PRIVILEGES ON DATABASE myproject TO gardiys;
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

+ [Грейдим БД](https://ruhighload.com/Тюнинг+базы+postgres)
+ Подключиться к postgres-docker
```
docker exec -it dd4a9e6684d1 sh
psql -U sixhands
```
