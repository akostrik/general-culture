## Docker 
* def: a set of platform as a service (PaaS) products 
* def: программное обеспечение, позволяющее упаковать приложение и его зависимости в единый модуль
* создаёт образы и контейнеры, следит за их состоянием, управляет сетевым окружением контейнеров, работает с локальным и удалённым репозиторием
* packages an application and its dependencies in a virtual container that runs, in isolation, in a variety of locations 
* usage: reproduces a run-time environments
* процесс развертывания приложения, собранного в контейнер, хорошо интегрируется в CI/CD pipline
* как работает
  + OS-level virtualization
  + имитирует Linux дистрибутивы, окружения или установочные процессы вместо их запуска
* usage: Сделать процесс настройки проще и упростить настройку на уровне инфраструктуры
* usage: Помочь разработчикам сосредоточиться исключительно на коде, сокращая время разработки и увеличивая продуктивность
* usage: Усилить возможности отладки с использованием встроенных функций
* usage: Изолировать приложения
* usage: Улучшить плотность использования серверов в форме контейнеризации
* usage: быстрое развертывание на уровне операционной системы
* usage: переносить приложение на другие операционные системы с поддержкой cgroups
* usage: эффективно позволяет нагрузить host машину, не создается виртуальное железо, как при использовании виртуальных машин
* добавляется еще один уровень абстракции, что позволяет использовать на одном хосте различные версии языков, библиотек, etc
* **Docker objets** entities used to assemble an application
* Отличие от VM: Docker-контейнер работает непосредственно в операционной системе, а виртуальная машина «поверх» операционной системы, что влияет на производительность
* использует проверенные технологии ядра, привнося минимум своих решений

### daemon dockerd (software)
* server
* центральный системный компонент
* служба, управляет Docker-объектами: сетями, хранилищами, образами, контейнерами
* управляет всеми процессами докера: создание образов, запуск и остановка контейнеров, скачивание образов
* управляет процессами коммуникации между контейнерами
* создает и запускает контейнеры
* работаеткак фоновый процесс (демон)
* следит за состоянием других компонентов
* ожидает запросов через REST API от Клиента
* управляет Образами и Контейнерами, manages containers, handles container objects
* listens for requests sent via **Docker Engine API**
* следуя инструкциям из Dockerfile и Docker-compose.yaml, собирает всё необходимое для запуска приложения в образ
* использует собственный драйвер `Docker runc`, который тесно связан со следующими механизмами ядра:
  + Cgroups (контрольные группы) — механизм управления ресурсами, используемыми контейнером (процессор, оперативная память и т.д.). С помощью cgroups Docker также реализует возможность остановки контейнеров — docker pause (freezing и unfreezing)
  + Namespaces (пространства имен) — механизм изоляции контейнеров, гарантирующий, что файловая система, имя хоста, пользователи, сетевая среда и процессы любого контейнера полностью отделены от остальной части системы

### docker (software)
* client
* консольный клиент, при помощи которого пользователи взаимодействуют с Docker daemon и отправляют ему команды, создают контейнеры, управляют ими
* утилита, предоставляющая API к докер-демону
* может быть консольным или графическим
* главный компонент, создающий/управляющий/запускающий контейнеризованные приложения
* управляет Сервером через CLI (командную строку) из терминала
* provides a command-line interface (CLI) to allow users to interact with Docker daemons

### image (object)
* неизменяемый образ, из которого разворачивается контейнер
* указания Серверу, как создавать Контейнер
* исполняемый пакет, из которого создаются Docker-контейнеры
* хранит всё необходимое для запуска приложения, помещенного в контейнер: код, среду выполнения, библиотеки, переменные окружения, конфигурационные файлы
* загружаются с Docker Hub, возможно создание кастомных образов
* в основе любого образа лежит родительский образ, который, как правило, содержит ОС
* каждая команда в Dockerfile создаёт новый слой образа с результатом вызванной команды, подобно тому, как система снапшотов сохраняет изменения в виртуальной машине
* образ состоит из таких слоёв-изменений
* можно использовать много раз
* def: набор файлов, соединенный с настройками, с помощью которого можно создать экземпляры, которые запускаются в отдельных контейнерах в виде изолированных процессов
* def: шаблон с инструкциями для создания контейнеров
* def: a collection of files, libraries, configuration files that build up an environment
* def: a pre-built environment for a certain technology or service
* usage: to build containers
* usage: to store and ship applications
* build image = to define the environment to be used in a container
* a read-only template
* is not a runtime
* строится с использованием инструкций для получения исполняемой версии приложения, зависящей от версии ядра сервера
* при запуске одного образа пользователь может создать несколько контейнеров
* **Docker registry** = a repository for images
  + Серверное приложение для размещения/распределения образов, полезен для локального хранения образов и полного контроля над ними
  + возможен другой способ, через Docker Hub — крупнейший репозиторий Docker-образов
  + an API provideing containers (?)
  + Ex: Docker Hub, quay.io, AWS ECR
* Docker старается максимально переиспользовать уже готовые шаблоны, поэтому если нужный шаблон не будет найден локально, он будет скачан из удалённого репозитория

### docker file (object)
* def: commands to build an image
* def: инструкция для сборки образа
* a text file
* begins with a FROM
* команды и опции создания образа, настройки будущего контейнера (порты, переменные окружения, ...)
* https://docs.docker.com/engine/reference/builder/ 
* Write a dockerfile
  + https://github.com/dnaprawa/dockerfile-best-practices  
  + alpine is not always the best choice
  + limit image layers amount
  + run as a non-root user
  + do not use a UID below 10 000
  + use a static UID and GID
  + the latest is an evil, choose specific image tag
  + store arguments in CMD
  + use COPY instead of ADD
  + combine RUN apt-get update with apt-get install in the same run statement
* Каждая команда записанная в dockerfile создаёт свой слой. Финальный Docker-образ — это объединение всех слоев в один. Благодаря такому подходу можно переиспользовать уже готовые образа для создания новых образов.
* каждая инструкция создает свой слой, поэтому хорошей практикой является запись цепочки команд через &&: RUN comand_1 && comand_2 && comand_3
* Финальной инструкцией является CMD или ENTRYPOINT
  + CMD может быть только одна
  + CMD может быть переопределена при старте контейнера командой docker run
  + CMD наследует условия установленные инструкцией WORKDIR
* не все инструкции Dockerfile непосредственно исполняются при сборке образа и запуске контейнера, например, `EXPOSE 80` лишь говорит демону Docker, что мы намереваемся пробросить порт наружу контейнера
* после запуска контейнера «постучаться» на 80 порт к нему мы не сможем. Так как для подтверждения инструкции EXPOSE используется ключ -P команды `docker run`. Если требуется прокинуть порт и переназначить его снаружи — используется команда `docker run -p 80:80 --name test_cont -d`
* прописывается набор софта, который мы хотим развернуть внутри контейнера
* Ex:
```
FROM python:latest          # возьми для основы образ Python с тегом latest, а если его нет локально — скачай из хаба
RUN mkdir -p /usr/app/      # создаст директорию app
WORKDIR /usr/app/           # установит директорию /usr/app/ в качестве рабочей директории, все последующие команды (COPY, RUN, CMD, ...)будут выполнены из рабочего каталога
CMD ["python", "main.py"]   # системный вызов, который будет выполнен при старте контейнера
```

### Container (object)
* def: развёрнутое и запущенное приложение
* def: базовая единица программного обеспечения = код + все его зависимости для обеспечения запуска приложения независимо от окружения
* def: исполняемый пакет программного обеспечения, содержащий все необходимое для запуска приложени (системные программы, библиотеки, код, среды исполнения, настройки)
* def: набор окружения, необходимого для запуска определённого софта
* запущенный Docker-образ со специальным слоем для записи и временного хранения информации
* запущенный и изолированный образ
* аналогия: образ = инсталлятор программы, контейнер = запущенная программа
* аналогия: образ = CD-диск, с которого будет установлен софт, контейнер = запущенная копия образа
* изолированное рабочее пространство
* может быть создан с использованием образа Docker
* полностью изолирован от хостовой ОС и других контейнеров
* нет ОС, использует ядро Linux и внутрь него помещаются только необходимые для запуска софта программы и библиотеки
* usage: runs applications - a database, a web server, a web framework, a test server, execute big data scripts, work on shell scripts, ... (one main process in one container)
* Как только контейнер запускается, создается набор пространств имен для этого контейнера. Они обеспечивают уровень изоляции для контейнеров, поскольку каждый контейнер работает в отдельном пространстве имен, с ограничением доступа к другим пространствам
* Ex: a container to be your MySQL database + a container to be your Wordpress server, connect the containers together
* a process created from an image, is started by running an image
* is a encapsulated runtime environment
* one container provides one service in the project
* is managed using the Docker API or CLI
* bundle software, libraries, configuration files
* are isolated from one another
* communicate with each other through channels
* is not a virtual machine (so it is not recommended to use any patch based on ’tail -f’ and so forth when trying to run it)
* def: изолированное пользовательское окружение, в котором выполняется приложение
* def: запущенное из образа приложению
* один контейнер – одно приложение
* LXC (Linux Containers) используют те же технологии ядра Linux, но это другое
* технологии ядра Linux для изоляции контейнеров:
  + Пространства имен (Linux Namespaces), контролируют доступ к структурам данных ядра. Фактически это означает изоляцию процессов друг от друга и возможность иметь параллельно «одинаковые», но не пересекающиеся друг с другом иерархии процессов, пользователей и сетевых интерфейсов. Примеры:
     - PID, Process ID – изоляция иерархии процессов
     - NET, Networking – изоляция сетевых интерфейсов
     - PC, InterProcess Communication – управление взаимодействием между процессами
     - MNT, Mount – управление точками монтирования
     - UTS, Unix Timesharing System – изоляция ядра и идентификаторов версии
  + Контрольные группы (Cgroups), инструмент для контроля над распределением, приоритизацией и управлением системными ресурсами. Контрольные группы реализованы в ядре Linux. Управление контрольными группами реализовано через systemd. 
  + Средства управления привилегиями (Linux Capabilities), разбить привилегии пользователя root на небольшие группы привилегий и назначать их по отдельности. Контейнеры запускаются с ограниченным набором привилегий.
  + Дополнительные, мандатные системы обеспечения безопасности, такие как AppArmor или SELinux
* верхний write/read-слоей, хранящий временные данные, которые уничтожаются после удаления контейнера
* Данные записываются в специальный слой «сверху» контейнера и при удалении контейнера данные удаляются, но данные можно сохранить с помощью:
  + volumes, который можно подключить двумя способами (результат будет одинаковым):
    - `docker run -v — docker run --name python --rm -v home/test-volume/ : /data/ -d` 
    - инструкция `VOLUME /my_volume в Dockerfile` = создать том при запуске контейнера
  + монтирования папки с хоста к контейнеру, подходит для разработки и отладки приложения, вносимые изменения на хосте сразу отражаются в контейнере, для продакшна не годится
* технология для хранения слоев в контейнере — файловая система с каскадно-объединенным монтированием (Union File System – UnionFS)
* находится в /var/lib/docker/containers
* ему можно назначить внешний порт инструкцией EXPOSE и ключом -P команды docker run
* условия запуска контейнера могут быть заданы двумя путями
  + прописаны в Dockerfile
  + указаны в качестве аргументов и ключей `docker run`
* контейнерам можно назначать лимиты ресурсов и строить между ними сети, для управления ресурсами используются cgroups, для изоляции namespaces
* runtime-сущность на основе образа
 
### Docker Compose (a tool)
* надстройка для управления множеством контейнеров
* инструмент для управления несколькими контейнерами, создавать контейнеры и задавать их конфигурацию
* Docker-compose CLI
* a streamlined and efficient development
* simplifies the control of the entire application stack
* to manage services, networks, volumes in a YAML configuration file
* with a single command `docker-compose.yml`
   + creates and starts all the services (containers?) from the configuration file
   + runs the commands on multiple containers at once (building images, scaling containers, running containers that were stopped)
   + commands related to image manipulation, or user-interactive options, are not relevant in Docker Compose because they address one container
   + builds the Docker images
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

### Docker API
* the Engine API is an HTTP API served by Docker Engine
* the Docker client uses it to communicate with the Engine
* everything the Docker client can do can be done with the API
* most of the client's commands map directly to API endpoints (e.g. `docker ps` is `GET /containers/json`)

### Docker Desktop
* GUI-клиент
* отображает все сущности Docker

### Docker host 
* компьютер или виртуальный сервер, на котором установлен Docker

### Commands
`systemctl status docker` убедимся, что у установлена и работает служба docker  
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
`free`  
`build` сборка образа для Docker  
`create` создание нового контейнера  
`kill` принудительная остановка контейнера  
`dockerd` запуск сервиса Docker  
`commit` создание нового образа из изменений в контейнере  
`docker images` просмотреть список доступных локально образов  
`docker ps`, `docker ps -a` список доступных контейнеров с их состоянием на сервере   
`docker run` создание контейнеров, запускаемыми с использованием образов Docker (`run` имеет множество ключей)  
`docker run image` сформировать контейнер из образа  
`docker run -it --rm busybox sh` (-it подключили интерактивный tty в контейнер и запустили командную оболочку sh, —rm = автоматически удалить контейнер при выходе из интерактивного режима) внутри контейнера busybox доступны основные команды unix/linux   
`docker stop nginx_test` остановить контейнер  
`docker rm nginx_test` удалить контейнер  
`docker restart`  
`docker-compose down` остановить контейнер  
`docker volume create —-name my_volume` создать том при запуске контейнера - команда  
`docker volume ls` список томов  
`docker volume inspect my_volume`  
`docker volume rm my_volume` удалить том  
`docker system prune` очистка ресурсов Docker, после этой команды появляется возможность удалить тома, статус которых до этого определялся неправильно
`docker pull busybox`  скачали готовый образ busybox с сервера Docker Hub  
`docker pull image` Загрузить образ из DockerHub  
`docker ...`  разрешать и запрещать операции монтирования, доступ к сокетам, выполнение части операций с файловой системой, например изменение атрибутов файлов или владельца  
`docker build` считывает конфигурацию создаваемого образа из dockerfile и создаёт образ  

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
