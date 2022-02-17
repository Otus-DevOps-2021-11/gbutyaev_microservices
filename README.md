# gbutyaev_microservices
gbutyaev microservices repository



## Docker контейнеры. Docker под капотом 

- Локально у себя установил Docker, docker-compose, docker-machine
- Запустил первый контейнер с помощоью команды `docker run hello-world`. Нашел этот контейнер- `docker ps -a`. Посмотрел его образ - `docker images`
- Запустил контейнер с убунту в интерактивном режиме - `docker run -it ubuntu:18.04 /bin/bash`. Зашел в контейнер и создал файл - echo 'Hello world!' > /tmp/file`
- Запустил контейнер как в интерактивном - `docker run -it ubuntu:18.04 bash` так и в фононом режиме `docker run -dt nginx:latest`
- Создал новый процесс и зашел в контейнер  -`docker exec -it <u_container_id> bash`
- Создал коммит со своим образом - `docker commit <u_container_id> gbutyaev/ubuntu-tmp-file` и сохранил в репозиторий `docker-monolith`
- Удалил все не запущенные контейнеры `docker rm $(docker ps -a -q)` и образы ` docker rmi $(docker images -q)`
- Инициализировал в репозитории свой YC аккаунт
- С помощью команд из методички инициализировал инстанс на YC с окружением Docker
- Использую Dockerfile, создал образ, из которого запустил контейнер с нашим приложением
- Проверил работоспособность приложения - перешел на внешний ip созданного инстанса, указав порт 9292
- Залогинился на Dockerhub и запушил туда свой образ с приложением
