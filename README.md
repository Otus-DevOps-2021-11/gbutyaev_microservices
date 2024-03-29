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


## Docker образы. Микросервисы

- Скачал архив с сервисами и сохранил в своем microservice репозитории
- Создал Dockerfile в каждом отдельном репозитории comment, post-by, ui для создания образов
- Собрал образы на основе добавленных докер файлов
- Создал отдельную сеть для нашего приложения и запустил контейнеры на основе образов


## Docker.Сетевое взаимодействие Docker контейнеров. Docker Compose. Тестирование образов 

- Запустил контейнер с использованием none драйвера
- Создал bridge сеть в docker
- Запустил наш проект reddit с использованием bridge сети, присвоив сетевые алиасы на старте
- Запустил наш проект в двух bridge сетях
- Установил docker-compose на локальную машину
- Создал файл /src/docker-compose.yml
- Параметризировал docker-compose.yml
 
 
## Устройство Gitlab CI. Построение процесса непрерывной интеграции 

- Запустил контейнер с использованием `none` драйвера
- Создал `bridge` сеть в docker
- Запустил наш проект `reddit` с использованием `bridge` сети, присвоив сетевые алиасы на старте
- Запустил наш проект в двух bridge сетях
- Установил `docker-compose` на локальную машину
- Создал файл `/src/docker-compose.yml`
- Параметризировал `docker-compose.yml`

## Введение в мониторинг. Модели и принципы работы систем мониторинга

- Подготовил окружение. Создал Docker хост в YC. Настроил локальное окружение для работы с ним
- Инициализировал окружение Docker. Зашел в него с помощью команды `eval $(docker-machine env docker-host)`
- Запустил `Prometheus` в конейнере, использовал готовый образ из Docker hub - `docker run --rm -p 9090:9090 -d --name prometheus prom/prometheus`
- Проверил работу Prometheus, перейдя в браузере по адресу - `ip_ВМ:9090` все файлы, связанные с Docker - докер файлы наших сервисов, компоуз, файл с переменными
- Создал директорию `docker`, в которую перенес 
- Создал в корне репозитория директорию `monitoring` для хранения файлов мониторинга
- В директории `monitoring/prometheus` создал `Dockerfile` для будущего образа с `Prometheus`. Рядом создал конфигурационный файл для образа - `prometheus.yml`
- Cобираю образ `Prometheus`
- Собираю образы остальных сервисов при помощи скриптов `docker_build.sh` - `for i in ui post-py comment; do cd src/$i; bash docker_build.sh; cd -; done`
- Определил в  `docker-compose.yml` сервис `prometheus`
- Запустил наше приложение вместе с сервисом `pormetheus` - `docker-compose up -d`
- В UI проверил наличие `enpoint-ов` и состояние `Healtcheck-ов` сервисов
- Остановил сервис `post` и зафиксировал, что на графике метрика изменила значение на **0**
- После запуска сервиса `post` значение метрики - **1**
- Определяю в `docker-compose.yml` еще один сервис для сбора информации о работе Docker хоста - `node-exporter`
- Чтобы `Prometheus` следил еще за одним сервисом добавил джобу `node-exporter-ра` в конфигурационный файл - `prometheus.yml`
- Т.к. мы изменили конфиурационный файл, необходимо собрать новый образ `prometheus` - `monitoring/prometheus $ docker build -t $USER_NAME/prometheus .`
- Пересоздал сервисы: 
```
 docker-compose down
 docker-compose up -d
```
- В UI получил информацию об использовании CPU, используя `node_load1`
- Зашел на хост - `docker-machine ssh docker-host` и добавил нагрузки - `yes > /dev/null`
- В UI на `node_load1`- зафиксировал изменение состояние - нагрузка увеличилась 
- Запушил собранные образы в свой аккаунт на DockerHub - `gbutyaev`

## Применение системы логирования в инфраструктуре на основе Docker

- Обновил свой репозиторий с севрвисами, склонив ветку `logging` из гит репо `express43\reddit`
- Собрал новые образы наших сервисов и запушил их в свой аккаунт на `Docker hub`
- Поднял новый инстанс в `YC` с `docker-machine`, количество памяти - `8 Gb`
- Создал докер компоуз файл для централизованного сбора логов - `docker/docker-compose-logging.yml`
- Создал Dockerfile для образа `Fluentd`. Для докерфайла в той же директории создал файл конфигурации - `fluent.conf`
- Собрал образ Fleuntd
- Запустил `docker-compose` и проверил логи сервиса `post`:   
```
cd docker && docker-compose up -d
docker-compose logs -f post
```
- Запустил систему логирования :   
```
$ docker-compose -f docker-compose-logging.yml up -d
$ docker-compose down
$ docker-compose up -d
```
- В UI проверил работу `Kibana` путем создания индексов и просмотров логов
- Добавил фильтры для парсинга json-логов в `fluent.conf`
- Перезапустил сервисы и отследил структурированные и неструктурированные логи
- В `fluent.conf` заменил регулярные выражение `grok` шаблонами, перезапустил сервисы, проверил работу
- Добавил в `docker-compose-logging` сервис `zipkin`
- В `docker-compose` довил окружение `zipkin` в каждый сервис. Проверил работу в UI
