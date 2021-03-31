# Настройка Docker для Django проекта

## 1 Создать в папке с проектом `Dockerfile`
```dockerfile
FROM python:3
WORKDIR /usr/src/code/

ENV PYTHONDONTWRITEBYTECODE 1
ENV PYTHONUNBUFFERED 1

RUN pip3 install --upgrade pip
COPY ./requirements.txt .

RUN apt-get update

RUN pip3 install -r requirements.txt

COPY . .
```
## 2 Создать в папке с проектом `docker-compose.yml`
```dockerfile
version: '3.5'
services:
  web:
    build:
      context: .
      dockerfile: Dockerfile
    command: python manage.py runserver 0.0.0.0:port
    volumes:
      - ./<project_directory>/:/usr/src/code/
    ports:
      - real_port:docker_port
    environment:
      - SECRET_KEY=<secret_key>
      - DEBUG=1
      - DJANGO_ALLOWED_HOSTS=*
      - SQL_DATABASE=<db_name>
      - SQL_USER=<db_user>
      - SQL_PASSWORD=<db_password>
      - SQL_HOST=<db_host>
      - SQL_PORT=<db_port>
    image: sixhands/<image_name>
```