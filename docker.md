![Screenshot from 2024-04-06 00-40-31+](https://github.com/akostrik/general-culture/assets/22834202/d7532890-67e0-437a-bb90-7f0ca9b29e56)

## Docker 
* def: a set of platform as a service (PaaS) products 
  + упаковать приложение и его зависимости в единый модуль
  + для разработки, доставки и запуска контейнерных приложений
* создавать контейнеры, автоматизировать их запуск и развертывание, управлять жизненным циклом и сетевым окружением контейнеров
* **Docker objets** entities used to assemble an application (сети, хранилища, образы, контейнеры)

## Usage
* reproduces a run-time environments
* запускать контейнер в облачной инфраструктуре и на любом локальном устройстве
* развертывание
  + на уровне операционной системы
  + процесс развертывания хорошо интегрируется в CI/CD pipline
  + переносить приложение на другие операционные системы с поддержкой cgroups
  + упростить настройку на уровне инфраструктуры
* изолировать приложения, отделить приложение от инфраструктуры
  + приложение внутри контейнера не имеет доступа к основной ОС
  + не важно, в каком окружении оно будет работать, есть ли там нужные зависимости и настройки
* перейти с монолита на микросервисную архитектуру
  + изменения в одной компоненте не затронут остальную систему
* отладка: для автотестов требуются дополнительные зависимости (СУБД, брокеры сообщений и др), при их установке есть вероятность ошибки и повреждения данных
  + с докером если тестирование завершится некорректно и повредит данные в контейнере, они удалятся вместе с контейнером

## Как работает
![Screenshot from 2024-04-06 01-09-43+](https://github.com/akostrik/general-culture/assets/22834202/4b0ea467-2d6b-45a7-b1f6-c9e22093b2dc)
* OS-level virtualization: имитирует Linux дистрибутивы, окружения или установочные процессы вместо их запуска
* packages an application and its dependencies in a virtual container that runs, in isolation, in a variety of locations
  + разработчикам создают программу и упаковают зависимости и настройки в единый образ
* контейнер работает в операционной системе, в изолированной среде, не влияющей на основную операционную систему
  + виртуальная среда запускается из ядра основной ОС
  + не создается виртуальное железо
* VM:
  + как отдельный компьютер со своей ОС и виртуальным оборудованием
  + работает поверх операционной системы
  + создается виртуальное железо
* libcontainer собственная библиотека, абстрагирующая виртуализационные возможности ядра Linux
* ещё один уровень абстракции, что позволяет использовать на одном хосте различные версии языков, библиотек, etc
* проверенные технологии ядра
* минимум своих решений
* a client-server application

## API
* served by Docker Engine
* the Docker client uses it to communicate with the Engine
  + everything the Docker client can do can be done with the API
* REST API
* HTTP API
* specifies interfaces that programs can use to talk to and instruct the Docker daemon
* most of the client's commands map directly to API endpoints (e.g. `docker ps` is `GET /containers/json`)

### server daemon `dockerd`= Docker Engine ?
* фоновый процесс (демон)
* ожидает запросов через API от Клиента и выполняет заданную команду
* следует инструкциям из Dockerfile и Docker-compose.yaml
* управляет:
  + Docker-объектами
  + процессами докера: скачивание и создание образов, собирает образ, запуск и остановка контейнеров
  + коммуникацией между контейнерами
* собственный драйвер `Docker runc`
* commands
  + `dockerd` запуск сервиса

### client `docker`
* консольный (command-line) или графический
* пользователи отправляют команды, создают контейнеры, управляют ими, создаёт/управляет/запускает контейнеризованные приложения
 
### image (object)
* is not a runtime
* Неизменяемый файл (образ), из которого можно неограниченное количество раз развернуть контейнер
* образ = CD-диск, контейнер = запущенная копия образа
* исполняемый пакет (код, среда выполнения, библиотеки, переменные окружения, конфигурационные файлы)
* в его основе лежит родительский образ, как правило, содержащий ОС
* состоит из слоёв-изменений
* при запуске одного образа можно создать несколько контейнеров
* Docker старается максимально переиспользовать готовые шаблоны, если нужный шаблон не будет найден локально, он будет скачан из удалённого репозитория
* возможно создание кастомных образов

### Container (object)
![Screenshot from 2024-04-06 01-14-25+](https://github.com/akostrik/general-culture/assets/22834202/c42f1635-8a66-4610-8b50-1162d741c4de)
![Screenshot from 2024-05-13 14-33-01](https://github.com/privet100/general-culture/assets/22834202/028daefa-01ba-4f47-b948-fcbece2bce91)
* runtime-сущность
* one container = one service = одно развёрнутое и запущенное приложение = a process created from an image
* runs applications - a database, a web server, a web framework, a test server, execute big data scripts, work on shell scripts, ... 
* содержит все для запуска (системные программы, библиотеки, код, среды исполнения, настройки)
* условия запуска контейнера могут быть заданы
  + в Dockerfile
  + в качестве аргументов и ключей `docker run`
* performs the command in ENTRYPOINT or CMD, this will add to the container startup time
* is managed using the Docker API or CLI
* изолирован от хостовой ОС и других контейнеров 
  + файловая система, имя хоста, пользователи, сетевая среда и процессы любого контейнера полностью отделены от остальной части системы
* технологии ядра Linux для изоляции контейнеров:
  + namespaces
    - контейнер работает в отдельном пространстве имен, с ограничением доступа к другим пространствам
    - контролирует доступ к структурам данных ядра
      - изоляция процессов друг от друга
      - возможность иметь параллельно «одинаковые», но не пересекающиеся друг с другом иерархии процессов, пользователей и сетевых интерфейсов. Примеры: PID Process ID изоляция иерархии процессов, NET Networking изоляция сетевых интерфейсов, PC InterProcess Communication управление взаимодействием между процессами, MNT Mount управление точками монтирования, UTS Unix Timesharing System изоляция ядра и идентификаторов версии
  + Контрольные группы (Cgroups)
    - контроль над распределением, приоритизацией и управлением системными ресурсами
    - управление ресурсами, используемыми контейнером (процессор, оперативная память и т.д.)
  + Средства управления привилегиями (Linux Capabilities)
    - разбить привилегии пользователя root на небольшие группы привилегий и назначать их по отдельности
    - контейнеры запускаются с ограниченным набором привилегий.
  + Дополнительные системы обеспечения безопасности
    - AppArmor
    - SELinux
  + LXC (Linux Containers) хоть используют те же технологии ядра Linux != container
* верхний write/read-слоей, хранящий временные данные, которые уничтожаются после удаления контейнера, но их можно сохранить с помощью:
  + volumes, который можно подключить двумя способами (результат будет одинаковым):
    - `docker run -v — docker run --name python --rm -v home/test-volume/ : /data/ -d` 
    - инструкция `VOLUME /my_volume в Dockerfile` = создать том при запуске контейнера
  + монтирования папки с хоста к контейнеру, подходит для разработки и отладки приложения, вносимые изменения на хосте сразу отражаются в контейнере, для продакшна не годится
* технология для хранения слоев: Union File System (UnionFS)
* контейнерам можно назначать лимиты ресурсов и строить между ними сети
* communicate with each other through channels
 
### сети (object)

### хранилища (object)
 
### Dockerfile (object)
* инструкция для сборки образа
  + набор софта, который мы хотим развернуть
  + настройки контейнера (порты, переменные окружения, ...)
* каждая команда создаёт новый слой образа с результатом вызванной команды, подобно тому, как система снапшотов сохраняет изменения в виртуальной машине
  + финальный Docker-образ = объединение всех слоев в один
  + можно переиспользовать уже готовые образа для создания новых образов
* Финальной инструкцией является CMD или ENTRYPOINT
  + The default command to run when the container is started
  + CMD может быть только одна
  + CMD может быть переопределена при старте командой docker run
  + CMD наследует условия, установленные инструкцией WORKDIR
  + `CMD` и `ENTRYPOINT` запускают что-либо, но НЕ ЗАПИСЫВАЮТ изменений в образ
    - не стоит выполнять ими скрипты, результат которых нужно "положить" в конечный образ или раздел (для этого есть ``RUN``)
    - The command needs to be performed every time the container starts, rather than being stored in the immutable image
* RUN создаёт статичный слой, изменения внутри которого записываются в образ, но ничего не вызывают, невозможно запустить приложение напрямую
* пробросить порт наружу контейнера: `EXPOSE 80`
* прокинуть порт и переназначить его снаружи `docker run -p 80:80 --name test_cont -d`
* Write a dockerfile
  + https://github.com/dnaprawa/dockerfile-best-practices  
  + limit image layers amount, цепочки команд через && `RUN comand_1 && comand_2`, `RUN apt-get update && apt-get install` 
  + run as a non-root user
  + do not use a UID below 10 000
  + use a static UID and GID
  + the latest is an evil, choose specific image tag
  + store arguments in CMD
  + use COPY instead of ADD
  + Делаем это одним RUN мы потому, что каждая директива RUN создаёт новый слой в docker-образе, и лучше не плодить лишние RUN-ы без надобности.

### Docker Compose (a tool)
* инструмент, надстройка для управления несколькими контейнерами, создавать контейнеры и задавать их конфигурацию
* функционал для управления несколькими контейнерами
* запуск множества контейнеров одной командой
* docker-compose CLI
* to manage services, networks, volumes
* `docker-compose.yml`
   + creates and starts all the services (containers?) from the configuration file
   + builds the Docker images
   + runs the commands on multiple containers at once (building images, scaling containers, running containers that were stopped)
   + commands related to image manipulation, or user-interactive options, are not relevant in Docker Compose because they address one container
   + configures the application's services
   + defines configuration options
   + `build` defines configuration options such as the Dockerfile path
   + `command` override default Docker commands
   + the network line
* commands for managing the whole lifecycle of your application:
   + Start, stop, rebuild services
   + View the status of running services
   + Stream the log output of running services
   + Run a one-off command on a service
* usage:
   + streamline the development, deployment, management of containerized applications
   + to define and manage multi-container applications in a single YAML file
   + configuration files are easy to share, facilitating collaboration
   + caches the configuration used to create a container. When you restart a service that has not changed, Compose re-uses the existing containers. Re-using containers means that you can make changes to your environment very quickly
   + supports variables in the Compose file, you can use these variables to customize your composition for different environments, or different users
* разница:
   + docker для работы с контейнерами по отдельности
   + docker compose позволяет одновременно управлять несколькими контейнерами, а следовательно, работать с более сложными проектами
* пример:
   + веб-сайт
   + для авторизации пользователей необходимо подключение к базе данных
   + первый сервис отвечает за функционирование сайта
   + второй отвечает за базу данных
   + разработчику понадобится средство, позволяющее управлять одновременно двумя контейнерами
* пример:
   + веб-проект, состоящий из двух сайтов
   + первый позволяет создать интернет-магазин
   + второй для поддержки клиентов
   + оба сайта подключены к общей базе данных
   + по мере развития проекта мощностей текущего сервера недостаточно
   + перенести сайты на новый сервер
   + без docker compose переносить и настраивать все сервисы заново
   + при помощи docker compose можно выполнить перенос сайтов при помощи нескольких команд, потребуется лишь изменить настройки и перенести на другой сервер резервную копию баз данных
  
### Docker Volume (a tool)
* def: файловая система, которая расположена на хост-машине за пределами контейнеров. Созданием и управлением томами занимается Docker
* facilitates the independent persistence of data, allowing data to remain even after the container is deleted or re-created
* представляют собой средства для постоянного хранения информации
* самостоятельны и отделены от контейнеров
* ими могут совместно пользоваться разные контейнеры
* позволяют организовать эффективное чтение и запись данных
* можно размещать на ресурсах удалённого облачного провайдера
* можно шифровать
* им можно давать имена
* контейнер может организовать заблаговременное наполнение тома данными

### Docker Swarm (a tool)
* provides native clustering functionality for containers, which turns a group of Docker engines into a single virtual Docker engine
* in Docker 1.12 and higher, Swarm mode is integrated with Docker Engine
* _The docker swarm CLI utility_ allows users to run Swarm containers, create discovery tokens, list nodes in the cluster, and more.[37] The docker node CLI utility allows users to run various commands to manage nodes in a swarm, for example, listing the nodes in a swarm, updating nodes, and removing nodes from the swarm.[38] Docker manages swarms using the Raft consensus algorithm. According to Raft, for an update to be performed, the majority of Swarm nodes need to agree on the update.
* для организации кластеризации и планирования контейнеров
* собрать несколько узлов в единую виртуальную систему Docker и управлять ею
* **swarm** a set of cooperating daemons that communicate through the Docker API

### Docker Desktop
* GUI-клиент
* отображает все сущности Docker

### Docker host 
* компьютер или виртуальный сервер, на котором установлен Docker

### Commands client
#### создать, запустить, остановить контейнер, скачать образ 
`systemctl status docker` установлена и работает служба docker   
`docker create` создание контейнера   
`docker start` активирует существующий контейнер  
`docker build` считывает dockerfile, создаёт образ   
`docker run` создает контейнер, включает его  
`docker system prune`, `docker image prune` очистка ресурсов  
`docker stop` пытается остановить контейнер отправив SIGTERM, если долго нет ответа SIGKILL   
`docker kill` посылает SIGKILL, выключает контейнер, игнорируя сохранение данных  
`docker rm` удалить контейнер (он должен быть выключен)  
`docker pause`  
`docker restart`  
`docker pull image` загрузить образ из DockerHub   
`docker commit` создание нового образа из изменений в контейнере   
#### настройка контейнера
разрешать/запрещать монтирование  
доступ к сокетам  
выполнение части операций с файловой системой  
изменение атрибутов файлов или владельца   
`docker volume create --name myVolume` создать том при запуске контейнера   
`docker volume ls` список томов   
`docker volume inspect myVolume`   
`docker volume rm myVolume` удалить том   
#### инспектировать
`docker images`, `docker image ls` просмотреть список доступных локально образов   
`docker ps`, `docker ps -a`, `docker ls` список доступных контейнеров с их состоянием на сервере    
`docker image inspect` подробнее рассказывает о выбранном контейнере  
`docker logs` выводит в консоль логи  

### Commands Dockerfile
`RUN`  
`CMD`  
`EXPOSE`  

### Commands docker-compose
`make` in the root of the directory to build and start all container  
`make` to build all images in docker-compose  
`make up` to start all containers in docker-compose  
`make down` to remove all containers in the docker-compose   
`make start` to start all containers in the docker-compose   
`make stop` to stop all containers in the docker-compose  
`make status` to see the running containers  
`make logs` to see the logs of the containers   
`make rmi` to remove all images created by docker-compose   
`make re` to remove, build and run all containers in docker-compose   
`docker-compose down` остановить контейнер  

### Efficency
* a single server or virtual machine runs several containers simultaneously
* to avoid the situations when we say “it worked on my machine”, because Docker containers will give us the same environment on all machines
* containers share the services of a single OS kernel => fewer resources than virtual machines  
* Linux: Docker uses the resource isolation features of the Linux kernel (cgroups, kernel namespaces) and a union-capable file system to allow containers to run within a single Linux instance, avoiding the overhead of starting and maintaining virtual machines
* MacOS: Docker uses a Linux virtual machine to run the containers
* the Linux kernel's support for namespaces mostly isolates an application's view of the operating environment, including process trees, network, user IDs and mounted file systems, while the kernel's cgroups provide resource limiting for memory and CPU 
* since 0.9, Docker includes its own component **libcontainer** to use virtualization facilities provided directly by the Linux kernel, in addition to using abstracted virtualization interfaces via libvirt, LXC and systemd-nspawn
* Легкая переносимость, возможность создавать и тестировать приложения на локальной машине, не беспокоясь о программных зависимостях — Docker-контейнеры вмещают в себя все что нужно приложению для функционирования
* автоматизация работы с контейнерами при помощи cron jobs, автоматизируются рутинные повторяемые задачи
* мгновенное время запуска

### Недостатки
* работает медленнее, чем обычный запуск приложения на физическом сервере
* сложность использования
* так как Docker работает непосредственно в операционной системе, возможно внедрение зловредного кода в контейнеры и далее проникновение в ОС

![docker-php-16-638](https://github.com/akostrik/general-culture/assets/22834202/7305c712-59d9-44e5-b67d-6ea0283d8b06)
![Screenshot from 2024-03-30 00-55-26](https://github.com/akostrik/general-culture/assets/22834202/3ea7709f-8248-4980-ad54-2b242f9e9b2a)
![Screenshot from 2024-03-29 23-08-11](https://github.com/akostrik/general-culture/assets/22834202/d92caf9d-11c3-4446-88aa-1eed25bd76f3)
