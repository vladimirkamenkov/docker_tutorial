# Docker tutorial

Инструкция к использованию Docker'а на практике.

## Команды

1. `docker ps` - получить информацию о всех запущенных контейнерах
2. `docker ps -a` - список всех контейнеров (запущенные и нет)
3. `docker stop {id_container}` - оставить контейнер
4. `docker rm {id_container}` - удаляет остановленный контейнер
5. `docker images` - показать список всех образов на тачке
6. `docker rmi nginx` - удалить образ (убедиться в отсутствии запущенных контейнеров от образа)
7. `docker pull redis` - скачает образ с docker hub, но не запускает его
8. `docker run redis` - запуск образа (если нет образа локально, то делает `docker pull`)
9. `docker exec {id_container} cat /etc/hosts` - запуск команды на контейнере
10. `docker inspect {id_container}` - инспектирование контейнера
11. `docker logs {id_container}` - логи -d контейнера
12. `docker attach {id_container}` - прикрепить (запустить) терминал запущенного контейнера в -d(etouch) моде
13. `export DOCKER_HOST=1.1.1.1` - переключить докер на удаленный хост (`DOCKER_HOST=` для переключения на локаль)

## Docker run

1. Флаг `-d` запускается в бэкгрвунд режиме
2. Указать версию контейнера `docker run redis:5.0.0`
3. Флаг `-it` запуск и прием ввода с терминала
4. Проброс порта `-p 80:5000` (замена стандартного 5000-ого порта на 80-ый)
5. Проброс "внешнего диска" (для предотвращения потери данных при удалении контейнера) `docker run -v /opt/data/:/var/lib/mysql mysql`
6. `-e DOCKER_VARIABLE=value` - проброс переменных окружения
7. `--name=name` - пробросить имя контейнера
8. `--link redis:redis` - связь контейнеров 

## Docker images

1. Создать Dockerfile с описанием инструкций уствновки окружения
2. Инструкции для Dockerfile: `FROM ubuntu`, `RUN apt-get update`, ` COPY app.py`, `CMD sleep 5`, `ENTRYPOINT app.py` 
3. `docker build . -t {user}/{name_container}`
4. `docker push {name}/{name_container}`

## Docker Compose

1. docker-compose.yml
`
version: 2
services:
    web: 
        image: "image_web"
        networks:
        - backend
        - frontend
    database:
        image: "image_db"
        ports:
        - 5000:80
        links:
        - redis
        networks:
        - backend
    orchestration:
        image: "image_orch"
        depends_on:
        - database
        networks:
        - backend

networks:
    frontend:
    backend:
`
2. `docker-compose up` - стартануть приложение