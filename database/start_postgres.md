# Создание БД PostgreSQL
+ [Оглавление](../README.md)
## Где постгрес?
+ [В докер контейнере](#postgres-docker)
+ [Установлен в операционной системе](#postgres-native)
+ [Его нет](#no-postgres)

### <a name="postgres-docker">Создание БД для postgres, запущенного в Docker контейнере</a>
#### 1 Подключаемся к PostgreSQL
```shell
#               ContainerId  shell mode
docker exec -it dd4a9e6684d1 sh
#       username
psql -U sixhands
```
### <a name="postgres-native">Создание БД для postgres в операционной системе</a>
#### 1 Подлючаемся к PostgreSQL
```shell
sudo -u postgres psql
```
#### 2 Создать БД + пользователя с правами
```postgres-psql
CREATE DATABASE <myproject>;
CREATE USER gardiys WITH PASSWORD 'password';
ALTER ROLE gardiys SET client_encoding TO 'utf8';
ALTER ROLE gardiys SET default_transaction_isolation TO 'read committed';
ALTER ROLE myprojectuser SET timezone TO 'UTC';
GRANT ALL PRIVILEGES ON DATABASE <myproject> TO gardiys;
```

### <a name="no-postgres">У меня нет постгреса:(</a>
#### Как будем устанавливать?
+ [В операционную систему](install_postgres_os.md)
+ [В Docker контейнер]
