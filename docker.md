![docker-php-16-638](https://github.com/akostrik/general-culture/assets/22834202/7305c712-59d9-44e5-b67d-6ea0283d8b06)

## Linux Containers (LXC) 
* method for running multiple isolated Linux systems (containers) on a control host using a single Linux kernel
* the cgroups functionality of the kernel: limitation and prioritization of resources (CPU, memory, block I/O, network, etc.) without virtual machines
* the namespace isolation functionality of the kernel: isolation of an application's view of the operating environment (process trees, networking, user IDs, mounted file systems)
* provides an isolated environment for applications
* docker container != LXC (хоть используют те же технологии ядра Linux)
* OS-level virtualization 

## UnionFS
* вспомогательная файловая система, производящая каскадно-объединённое монтирование других файловых систем
* файлы и каталоги изолированных файловых систем прозрачно перекрываются, формируя единую связанную файловую систему
* каталоги, которые имеют тот же путь в объединённых ветвях, будут совместно отображать содержимое в объединённом каталоге
* есть приоритет одной ветви над другой, на случай, когда обе ветви содержат файл с идентичным именем

## Docker 
* a set of platform as a service (PaaS) products
* инструмент объекто-ориентированного проектирования (дизайна) (вместо последовательно-процедурного вызова команд)
  + инкапсуляция, наследование, полиморфизм компонент => независимые компоненты: приложение + web-сервер + БД + сист библиотеки
  + конфигурация nginx ≈ часть веб-приложения
  + отделить приложение от инфраструктуры (независимо от версии PHP или Python на сервере, etc)
    - приложение внутри контейнера не имеет доступа к основной ОС
    - не важно, в каком окружении оно будет работать, есть ли там нужные зависимости и настройки
* packages an application and its dependencies in a container
* выкладывания контейнеров на ваши продакшены, как в дата центры так и в облака
* переносить приложение на другие операционные системы с поддержкой cgroups
* управлять жизненным циклом и сетевым окружением контейнеров
* развертывание
  + reproduces a run-time environments, OS-level virtualization, Linux дистрибутивы, установочные процессы
  + на уровне операционной системы
  + запускать в облачной инфраструктуре
  + упростить
* быстрый, но медленнее, чем запуск приложения на физическом сервере
* сложный
* работает непосредственно в ОС => возможно внедрение зловредного кода в контейнеры и проникновение в ОС

## Как устроен
![Screenshot from 2024-04-06 01-09-43+](https://github.com/akostrik/general-culture/assets/22834202/4b0ea467-2d6b-45a7-b1f6-c9e22093b2dc)
![Screenshot from 2024-03-30 00-55-26](https://github.com/akostrik/general-culture/assets/22834202/3ea7709f-8248-4980-ad54-2b242f9e9b2a)
* the resource isolation features of the Linux kernel (cgroups, kernel namespaces) + a union-capable file system => to allow containers to run within a single Linux instance, avoiding the overhead of virtual machines
* микросервисная архитектуря
  + изменения в одной компоненте не затронут остальную систему
* контейнер в изолированной среде, не влияющей на основную операционную систему
  + виртуальная среда запускается из ядра основной ОС (в оличие от VM)
  + не создается виртуальное железо (в оличие от VM)
  + containers share the services of a single OS kernel => fewer resources than virtual machines  
* позволяет использовать на одном хосте различные версии языков, библиотек, etc
* libcontainer библиотека, абстрагирующая виртуализационные возможности ядра Linux
  + to use virtualization facilities provided directly by the Linux kernel, in addition to using abstracted virtualization interfaces via libvirt, LXC and systemd-nspawn
* проверенные технологии ядра, минимум своих решений
* a client-server application
* `/var/lib/docker/overlay2` writable layers, the various filesystem layers for images and containers
  + `/var/lib/docker/overlay2/eceb7b667587c3cc2a08d7c970eae723fdd8981b7a7580db19587434123a2681`
* `/var/lib/docker` your images, containers, local named volumes

## API
* REST API
* HTTP API
* specifies interfaces that programs can use to talk to the Docker daemon
* сервер ожидает запросов через API от Клиента и выполняет команду
* client uses API to communicate with the Engine
  + everything the Docker client can do can be done with the API
  + most of the client's commands map directly to API endpoints (e.g. `docker ps` is `GET /containers/json`)
* клиент и сервер могут:
  + работать на одной системе (общаются через REST API)
  + можно подключить клиент к удаленному демону docker (общаются через сокет)

### server daemon `dockerd`= Docker Engine ?
* фоновый процесс (демон)
* следует инструкциям из Dockerfile и Docker-compose.yaml
* управляет:
  + Docker-объектами
  + процессами докера: скачивание и создание образов, собирает образ, запуск, остановка контейнеров
  + коммуникацией между контейнерами

### client `docker`
* интерфейс к Docker
* command-line или графический
* получает команды от пользователя (создают контейнеры, управляют ими, создаёт/управляет/запускает контейнеризованные приложения)

### image (object, an entitie used to assemble an application)
* неизменяемый файл, из которого можно неограниченное количество раз развернуть контейнер
* исполняемый пакет = код + среда выполнения + библиотеки + переменные окружения + конфигурационные файлы
* состоит из набора уровней, из слоёв-изменений
  + слои = папки, которые лежат в /var/lib/docker/aufs/diff/
  + в основе родительский образ, как правило, содержащий ОС
  + docker переиспользует готовые шаблоны для создания новых образов
  + когда docker скачивает образ, ему нужны только те слои, которых у него нет
* содержит
  + метаданные предустановленной программы
  + команду, которую следует выполнить при запуске контейнера
  + параметры, передаваемые предустановленной программе
* при запуске одного образа можно создать несколько контейнеров
* Образ ≈ класс в коде, контейнер ≈ объект, созданный из класса
* **композиция вместо наследования**: рядом контейнеры с разными сервисами + адаптер + transfer object + свяжу их через конфиг
* **наследование** для расширения существующего функционала в рамках инкапсуляции (для подключения к php расширений, системных библиотек)
* **An index** manages user accounts, permissions, search, tagging, etc that's in the public web interface
* **A registry** stores and serves up the actual image assets, delegates authentication to the index

### Container (object, an entitie used to assemble an application)
![Screenshot from 2024-04-06 01-14-25+](https://github.com/akostrik/general-culture/assets/22834202/c42f1635-8a66-4610-8b50-1162d741c4de)
![Screenshot from 2024-05-13 14-33-01](https://github.com/privet100/general-culture/assets/22834202/028daefa-01ba-4f47-b948-fcbece2bce91)
* набор процессов
* 1 container = 1 service = 1 развёрнутое и запущенное приложение (database, web server, web framework, test server, execute big data scripts)
* содержит все для работы приложения (системные программы, библиотеки, код, среды исполнения, настройки)
* изолированное рабочее пространство, изолирован от хостовой ОС и других контейнеров 
  + файловая система, имя хоста, пользователи, сетевая среда, процессы - отделены от остальной части системы
* container = ОС + пользовательские файлы и метаданные
  + образ говорит docker-у, что находится в контейнере, какой процесс запустить, когда запускается контейнер, другие конфигурационные данные
* технологии ядра Linux для изоляции контейнеров:
  + namespaces
    - the Linux kernel's support for namespaces isolates an application's view of the operating environment, including process trees, network, user IDs and mounted file systems
    - контейнер работает в отдельном пространстве имен, с ограничением доступа к другим пространствам
    - контролирует доступ к структурам данных ядра
      - изоляция процессов друг от друга
      - возможность иметь параллельно «одинаковые», но не пересекающиеся друг с другом иерархии процессов, пользователей и сетевых интерфейсов. Примеры: PID Process ID изоляция иерархии процессов, NET Networking изоляция сетевых интерфейсов, PC InterProcess Communication управление взаимодействием между процессами, MNT Mount управление точками монтирования, UTS Unix Timesharing System изоляция ядра и идентификаторов версии
  + Контрольные группы (Cgroups)
    - the kernel's cgroups provide resource limiting for memory and CPU 
    - контроль над распределением, приоритизацией и управлением системными ресурсами
    - управление ресурсами, используемыми контейнером (процессор, оперативная память и т.д.)
    - предоставление приложению только тех ресурсов, которые вы хотите предоставить => контейнеры хорошие соседи
    - разделять доступные ресурсы железа и если необходимо, устанавливать пределы и ограничения (например, ограничить количество памяти контейнеру)
    - контейнеры исполняются механизмом ядра Cgroups
    - служба docker запускает контейнер по команде, полученной от клиентского приложения (например, docker), и останавливает его когда в контейнере освобождается поток стандартного ввода-вывода
    - поэтому в конфигурации Nginx для Docker пишут: Be sure to include daemon off; in your custom configuration to ensure that Nginx stays in the foreground so that Docker can track the process properly (otherwise your container will stop immediately after starting)!
  + Средства управления привилегиями (Linux Capabilities)
    - разбить привилегии пользователя root на небольшие группы привилегий и назначать их по отдельности
    - контейнеры запускаются с ограниченным набором привилегий.
* верхний write/read-слоей, хранящий временные данные, которые уничтожаются после удаления контейнера, но их можно сохранить с помощью:
  + volumes, создать том при запуске контейнера, тремя способами:
    - `docker run -v — docker run --name python --rm -v home/test-volume/ : /data/ -d` 
    - `VOLUME /my_volume в Dockerfile`
    - `volume`  в docker-compose.yml 
  + монтирования папки с хоста к контейнеру
    - изменения на хосте сразу отражаются в контейнере
    - подходит для разработки и отладки приложения, для продакшна не годится
* Union File System (UnionFS)
  + файловая система, которая работает создавая уровни, делая ее очень легковесной и быстрой
  + технология для хранения слоев
  + для сочетания уровней в один образ
  + UnionFS позволяет файлам и директориями из разных файловых систем (разным ветвям) накладываться, создавая файловую систему
  + одна из причин, по которой docker легковесен — использование таких уровней
    - когда вы изменяете образ, например, обновляете приложение, создается новый уровень
    - без замены всего образа или его пересборки, только уровень добавляется или обновляется
    - раздается только обновление, что позволяет распространять образы проще и быстрее
  + docker использует UnionFS для создания блоков, из которых строится контейнер
  + docker может использовать несколько вариантов UnionFS (AUFS, btrfs, vfs, DeviceMapper, ...)
* docker клиент говорит (с помощью программы docker или с помощью REST API) демону запустить контейнер, после чего докер
  + `$ sudo docker run -i -t ubuntu /bin/bash` запускается клиент с помощью команды docker (run = новый контейнер)
  + скачивает образ ubuntu (если есть, то с локальной машины)
  + создает контейнер
  + инициализирует файловую систему
  + монтирует read-only уровень
  + инициализирует сеть/мост: создает сетевой интерфейс, который позволяет docker-у общаться хост машиной
  + находит и задает IP адрес
  + запускает приложение
  + обрабатывает и выдает вывод приложения, логирует стандартный вход, вывод и поток ошибок
* создает уровень для чтения/записи сверху образа (используя unionFS)
* по умолчанию не удаляется после окончания работы
* запускается без гипервизора
* контейнерам можно назначать лимиты ресурсов
* communicate with each other through channels

### A docker-network (object, an entitie used to assemble an application)
* establishes the connection between your containers
* docker's networking subsystem is pluggable, using drivers
* several drivers exist by default, provide networking functionality:
  + bridge (by default): when an application in a container communicates with other containers on the same host
  + host: remove network isolation between the container and the Docker host and use the host's networking directly
  + overlay: connect multiple Docker daemons together and enable Swarm services and containers to communicate across nodes
  + ipvlan: gives users total control over both IPv4 and IPv6 addressing
  + macvlan: allows to assign a MAC address to a container, making it appear as a physical device on your network
  + none: isolates a container from the host and other containers
  + third-party network plugins
* https://docs.docker.com/network/network-tutorial-standalone/
* `docker network list`
* `brctl show`
* `sudo iptables -t nat --list`
* `ifconfig`
* `docke network inspect app_net`
* `curl 172.200.0.2`
* `curl 172.200.0.2:8080`
* `ip addr show` examine network interfaces, a new one was not created

#### Networking using the host network
A standalone containers binds directly to port 80 to the Docker host's network
* no network isolation, network isolation as if the nginx were running directly on the host
* in all other ways (storage, process namespace, user namespace): the nginx process is isolated from the host
* port 80 should be available on the host
* the host networking driver
 
### Dockerfile (object, an entitie used to assemble an application)
![Screenshot from 2024-03-29 23-08-11](https://github.com/akostrik/general-culture/assets/22834202/d92caf9d-11c3-4446-88aa-1eed25bd76f3)
* инструкции = шаги описания для создания образов мы называем
* docker считывает Dockerfile, когда вы собираете образ, выполняет эти инструкции, и возвращает образ
* инструкция для сборки образа
  + набор софта, который мы хотим развернуть
  + настройки контейнера (порты, переменные окружения, ...)
* каждая инструкция создает новый образ или уровень
* примеры инструкци:
  + запуск команды
  + добавление файла или директории
  + создание переменной окружения
  + указания, что запускать, когда запускается контейнер этого образа
* каждая команда создаёт новый слой образа с результатом вызванной команды, подобно тому, как система снапшотов сохраняет изменения в виртуальной машине
  + финальный Docker-образ = объединение всех слоев в один
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
  + use a static UID and GID
  + the latest is an evil, choose specific image tag
  + store arguments in CMD
  + use COPY instead of ADD
  + Делаем это одним RUN мы потому, что каждая директива RUN создаёт новый слой в docker-образе

### Docker Compose (a tool)
* docker-compose CLI
* надстройка для управления несколькими контейнерами, создавать контейнеры, задавать конфигурацию, запуск множества контейнеров одной командой, to manage services, networks, volumes
* `docker-compose.yml`
   + creates and starts all the services (containers?) from the configuration file
   + builds the Docker images
   + runs the commands on multiple containers at once (building images, scaling containers, running containers that were stopped)
   + commands related to image manipulation, or user-interactive options, are not relevant in Docker Compose because they address one container
   + configures the application's services
   + defines configuration options
   + `build` defines configuration options
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
* пример: веб-сайт
   + для авторизации пользователей необходимо подключение к базе данных
   + первый сервис (контейнер) отвечает за функционирование сайта
   + второй сервис (контейнер) отвечает за базу данных
* пример: веб-проект, состоящий из двух сайтов
   + первый позволяет создать интернет-магазин
   + второй для поддержки клиентов
   + оба сайта подключены к общей базе данных
   + по мере развития проекта мощностей текущего сервера недостаточно => легко переносят сайты на новый сервер при помощи docker compose
  
### Docker Volume (a tool, an object?)
* storing data outside containers, файловая система, которая расположена на хост-машине за пределами контейнеров
* папка хоста, примонтированная к файловой системе контейнера Т
* **на сервере** создается каталог, который затем монтируется в один или несколько контейнеров
  + stored your host, usually in /var/lib/docker/volumes
  + каталог является независимым
  + каталог не включается в структуру слоев образа Docker
* mounted to filesystem paths in your containers
* managed by Docker
* самостоятельны и отделены от контейнеров, отключает привязку данных к жизненному циклу контейнера, позволяя получить доступ к контейнерным данным в любой момент
* разные контейнеры могут совместно пользоваться одним volume
* эффективное чтение и запись данных
* можно размещать на ресурсах удалённого облачного провайдера
* можно шифровать
* можно давать имена
* контейнер может организовать заблаговременное наполнение тома данными
* Драйверы томов — указывать параметры хранения (место хранения, ...)
* different drivers to store volume data in different services:
  + local storage on your Docker host (by default)
  + NFS volumes
  + CIFS/Samba shares
  + device-level block storage adapters
* volumes are a better solution than bind mounts, when you’re providing permanent storage to operational containers
  + you don’t need to manually maintain directories on your host
  + there’s less chance of data being accidentally modified
  + no dependency on a particular folder structure
  + increased performance
  + the possibility of writing changes
* use cases:
  + to the storage directories used by MySQL, Postgres,Mongo
  + to store data generated by your application (file uploads, profile photos, ...)
  + to persist the contents of any caches
  + to backup container data by mirroring /var/lib/docker/volumes to another location
  + share data between containers
  + write to remote filesystems 
* альтернатива: **bind mounts** = связанные папки 
  + связать папку на компьютере пользователя (то есть хосте, на котором установлен Docker Engine) и папку в контейнере
  + работать в контейнере и на хосте с такой папкой можно одновременно, все изменения будут отображаться и там, и там
  + directly mount a host directory into your container
  + any changes made to the directory will be reflected on both sides of the mount, whether the modification originates from the host or within the container
  + are best used for ad-hoc storage on a short-term basis
  + convenient in development workflows
  + for example: bind mounting your working directory into a container automatically synchronizes your source code files, allowing you to immediately test changes without rebuilding your Docker image
  + для добавления в контейнер имеющегося пути
    - помогает для добавления конфигурационной информации
    - помогает для добавления наборов данных и статики с сайтов
* альтернатива: драйверы
* альтернатива: резервные копии
* альтернатива: хранение в оперативной памяти
    - бывает двух типов: tmpfs mounts и npipe mounts
* альтернатива: **Tempfs Mount** выполняют функцию, обратную главной задаче Docker Volumes: отменяют сохранение информации после ликвидации контейнера
  + это может понадобиться, если Вы ведёте обширное журналирование, т.к. постоянные одноразовые записи могут привести к падению производительности 
* `docker volume ls`
* Ex: `docker volume create` создание тома (Volume) 
* Ex: `$ docker run -it -v demo_volume:/data ubuntu:22.04`
  + `it` attaches your terminal to the container
  +  a volume called demo_volume is mounted to /data inside the container

### Docker Swarm (a tool)
* provides native clustering functionality for containers, which turns a group of Docker engines into a single virtual Docker engine
* in Docker 1.12 and higher, Swarm mode is integrated with Docker Engine
* _The docker swarm CLI utility_ allows users to run Swarm containers, create discovery tokens, list nodes in the cluster, and more.[37] The docker node CLI utility allows users to run various commands to manage nodes in a swarm, for example, listing the nodes in a swarm, updating nodes, and removing nodes from the swarm.[38] Docker manages swarms using the Raft consensus algorithm. According to Raft, for an update to be performed, the majority of Swarm nodes need to agree on the update.
* для организации кластеризации и планирования контейнеров
* собрать несколько узлов в единую виртуальную систему Docker и управлять ею
* **swarm** a set of cooperating daemons that communicate through the Docker API

### Настройки
* `/etc/default` DOCKER_OPTS="--dns 8.8.8.8 --dns 8.8.4.4 -H tcp://0.0.0.0:2375 -H unix:///var/run/docker.sock"
* `/etc/init.d/docker`
* `/etc/init/docker.conf` DOCKER_OPTS="-H tcp://0.0.0.0:2375 -H unix:///var/run/docker.sock"
* `/etc/systemd/system/docker.service.d`
* `/etc/systemd/system/docker.service.d/override.conf`
* `/etc/docker/daemon.json`
* `/lib/systemd/system/docker.service` ExecStart=/usr/bin/docker daemon -H fd:// -H tcp://0.0.0.0:
* Never edit the docker service script (or any service script) directly
  + SystemD has a differential editing feature built in
  + use systemctl edit docker.service
* if you open docker to listen on the network
  + the docker API ports are frequently scanned on the internet, and you will find malware installed on your host
  + you are running the equivalent of an open telnet server with root logins allowed without a password
  + you should configure mutual TLS between client and server

### docker commands via client
#### создать, запустить, остановить контейнер, скачать образ 
`systemctl status docker` установлена и работает служба docker   
`docker create` создание контейнера   
`docker start` активирует существующий контейнер  
`docker build` считывает dockerfile, создаёт образ   
`docker run` создает контейнер, включает его  
`docker stop` пытается остановить контейнер отправив SIGTERM, если долго нет ответа SIGKILL   
`docker kill` посылает SIGKILL, выключает контейнер, игнорируя сохранение данных  
`docker rm` удалить выключенный контейнер  
`docker pause`  
`docker restart`, `/etc/init.d/docker restart` перезапустить демон  
`systemctl restart docker.service`  
`docker pull image` загрузить образ из DockerHub   
`docker commit` создание нового образа из изменений в контейнере   
`docker system prune` cleanup unused containers and images, doesn't deletes running containers, logs on those containers, filesystem changes made by those containers
`docker image prune --all` remove unused images   
`systemctl daemon-reload`  
#### настройка контейнера
разрешать/запрещать монтирование  
доступ к сокетам  
выполнение части операций с файловой системой  
изменение атрибутов файлов или владельца   
`docker tag` переименовать образ   
`docker volume create --name myVolume` создать том при запуске контейнера   
`docker volume ls` список томов   
`docker volume inspect myVolume`   
`docker volume rm myVolume` удалить том   
`docker run -p` publish a container's port to the host
`docker run -P` publish all exposed port to random ports
`docker run -d -p 7000:80 test:latest`
#### инспектировать
`docker images`, `docker image ls` просмотреть список доступных локально образов   
`docker ps`, `docker ps -a`, `docker ls` список доступных контейнеров с их состоянием на сервере    
`docker image inspect` подробнее рассказывает о выбранном контейнере  
`docker logs` выводит в консоль логи  
`cat /var/lib/docker/repositories | python -mjson.tool` list of the repositories on your host
`ls -al /var/lib/docker/graph`
`netstat -ntpl`  
`/var/lib/docker/aufs/diff/` все файлы контейнеров, если для работы с файловой системой Docker использует драйвер AUFS
`/var/lib/docker/containers/` служебная информация, не сами файлы контейнеров
`/var/lib/docker/aufs/diff/` образы
`/etc/init.d/docker status`  
`/etc/resolv.conf`

### Commands docker daemon
* `dockerd` запуск сервиса

### Commands Dockerfile
`RUN`  
`CMD`  
`EXPOSE`  
`FROM` defines a base for your image. exemple : FROM debian  
`WORKDIR` sets the working directory for any RUN, CMD, ENTRYPOINT, COPY, and ADD instructions that follow it in the Dockerfile. (You go directly in the directory you choose)  
`COPY` copies new files or directories from and adds them to the filesystem of the container at the path  
`CMD` lets you define the default program that is run once you start the container based on this image. Each Dockerfile only has one CMD, and only the last CMD instance is respected when multiple ones exist  

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

### Restart the engine in a completely empty state + lose all images, containers, named volumes, user created networks, swarm state:
```
sudo -s
systemctl stop docker
rm -rf /var/lib/docker
systemctl start docker
exit
```

### Example 1
`docker run -i -t ubuntu /bin/bash`  
1. `docker` запускается клиент   
2. скачивает образ ubuntu
3. `run` запускает новый контейнер
4. инициализирует файловую систему и монтирует read-only уровень
5. инициализирует сеть/мост: создает сетевой интерфейс, который позволяет docker-у общаться хост машиной
6. находит и задает IP адрес
7. запускает приложение
8. запускает команду `/bin/bash`  

### Example 2
`$ docker run -it -v public:/var/www/public ubuntu:22.04` 
* starts a new container
* attaches your terminal to it (-it)
* a volume `public` is mounted to /var/www/public inside the container
  
`ls /var/www/public` the contents of the container’s /var/www/public   
`echo "foobar" > /var/www/public/foo` add a test file with some arbitrary content   
`exit` detach from your container, the container stops  
`docker run -it -v public://var/www/public2 alpine:latest` start a new container that attaches the same volume   

### Example 3: http://127.0.0.1:8080/ на виртуальной + Dockerfile
~/ex3/**index.html**:  
```
<html>
  <body>Example 3</body>
</html>
```
~/ex3/**Dockerfile**:  
```
FROM nginx                    # соберем image из готового образа docker hub  
COPY index.html /usr/share/nginx/html
```
`docker build . --tag mynginx     # BUILD image (first run), скачает образ nginx, выполняет dockerfile`  
`docker run -p 8080:80 -d mynginx # -p порт 80 контейнера -> порт host машины 8080`  
`                                 # -d в фоновом режиме без привязки к текущей консоли`  
`startx                           # x-server для отрисовки графического окружения (GUI)`  
`wget http://127.0.0.1/index.html --no-check-certificate` проверить без браузера

### Example 4: http://127.0.0.1 на виртуальной + docker-compose
~/ex4/public/html/**index.html**:  
```
<html>
  <body>Example 4</body>
</html>
```
~/ex4/nginx/conf.d/**nginx.conf**:  
```
server {
  root    /var/www/public/html;
  location / {                     # all locations
    try_files $uri /index.html;    # if the received URI matches $uri, nginx serves it   
                                   # if fails, serves `/index.html` (the fall back option)     
                                   # if fails, serves the 404 error page   
    }
}
```
~/ex4/**docker-compose.yml**:  
```
version: '3'

services:
  nginx:
    image: nginx:stable-alpine
    volumes:
      - ./public:/var/www/public       # "/var/www/public" inside the container refers to the host folder "/public" 
      - ./nginx/conf.d:/etc/nginx/conf.d/
    restart: unless-stopped
    ports:
      - "80:80"
    container_name: myContainer
```
~/ex4/**test.sh**:  
```
#!/bin/bash
docker stop myContainer
docker rm myContainer
docker image prune --all
docker system prune
service docker stop
systemctl daemon-reload
systemctl restart docker.service
service docker start
docker-compose up -d --build           # build = first run
# startx
wget http://127.0.0.1/index.html --no-check-certificate
```

### Example 5: http://akostrik.42.fr на виртуальной
`/etc/hosts`: добавляем алиас локального домена `akostrik.42.fr`   

### Example 6: https://akostrik.42.fr на виртуальной
`cd ~/project/srcs/requirements/tools/`  
`mkcert akostrik.42.fr` сгенерируем самоподписный сертификат  
`mv akostrik.42.fr-key.pem akostrik.42.fr.key` поменять расширения файлов, чтобы nginx их правильно читал  
`mv akostrik.42.fr.pem akostrik.42.fr.crt`   
~/ex6/public/html/**index.html**:  
```
<html>
  <body>Example 6</body>
</html>
```
~/ex6/nginx/conf.d/**nginx.conf**:  
```
server {
  listen             80;
  listen             443 ssl;
  server_name        akostrik.42.fr www.akostrik.42.fr;   # домен, на котором мы будем работать
  root               /var/www/public/html;
  ssl_certificate     /etc/nginx/ssl/akostrik.42.fr.crt;
  ssl_certificate_key /etc/nginx/ssl/akostrik.42.fr.key;
  ssl_protocols       TLSv1.2 TLSv1.3;                    # поддерживаемые протоколы tls
  ssl_session_timeout 10m;                                # опции кэширования  
  keepalive_timeout   70;                                 # таймауты
  location / {                                            # искать файл в корне 
    try_files $uri /index.html;
  }
}
```
~/ex6/**docker-compose.yml**:   
```
version: '3'
services:
  nginx:
    image: nginx:stable-alpine
    volumes:
      - ./public:/var/www/public/
      - ./nginx/conf.d:/etc/nginx/conf.d/
      - /home/${USER}/project/srcs/requirements/tools:/etc/nginx/ssl/ # ключи, сертификаты
    restart: unless-stopped
    ports:
      - "80:80"
      - "443:443"
    container_name: myContainer
```
Браузер: Advanced / Принять риск и продолжить 
* браузер доверяет самоподписному сертификату  
* сайт загружается по ssl  
* соединение не считается безопасным  

### Example 7: https://127.0.0.1:1443 на хостовой
`chmod o+r /etc/resolv.conf` make sure your DNS Resolver config file is writable (кажется не нужно?)

nginx.conf:  
```
server {
  listen              80;   # ?
  listen              443 ssl;
  server_name         akostrik.42.fr www.akostrik.42.fr;  
  root                /var/www/public/html;
  if ($scheme = 'http') {                                 # NEW перенаправление с http на https
    return 301 https://akostrik.42.fr$request_uri;
  }
  ssl_certificate     /etc/nginx/ssl/akostrik.42.fr.crt;
  ssl_certificate_key /etc/nginx/ssl/akostrik.42.fr.key;
  ssl_protocols       TLSv1.2 TLSv1.3;
  ssl_session_timeout 10m;  
  keepalive_timeout   70;
  location / { 
    try_files $uri /index.html;
  }
}
```
`--no-check-certificate`

### Example 8: https://akostrik.42.fr:1443 на хостовой
но школьный мак не знает 42.fr  

##
https://habr.com/ru/articles/267441/  
https://blog.thoward37.me/articles/where-are-docker-images-stored/  
