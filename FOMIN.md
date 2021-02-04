## Docker
+ Зайти внутрь контейнера

```
sudo docker exec -ti <container_id> sh
```

## Linux Bash
+ Посмотреть запущенные процессы

```
sudo ps aux
```

+ Посмотреть запущенные процессы / фильтрация

```
sudo ps aux | grep <process_name>
```

+ Посмотреть запущенные дерево процессов / фильтрация

```
sudo ps fax | grep <process_name>
```