---
title:  "Docker commands"
date:   2022-10-18 20:29:12
author: nosov-ao
---

### Main docker commands:

* ```sudo docker compose up -d``` -d: запуск контейнеров в фоновом
* ```sudo docker compose down```
* ```docker ps``` - список запущенных контейнеров 
* ```docker ps -a``` - Список всех контейнеров
* ```docker stop my_container```
* ```docker rm -f my_container``` - принудительно останавливает и удаляет контейнер без предупреждения
* ```docker logs -f my_container``` - просмотр логов в реальном времени
* ```docker exec -it my_container /bin/bash``` - запустить интерактивную оболочку в контейнере



