## Docker 
* a set of platform as a service (PaaS) products 
* packages an application and its dependencies in a virtual container that runs, in isolation, in a variety of locations on Linux / Windows / macOS
* usage: reproduces a run-time environments
* имитирует Linux дистрибутивы, окружения или установочные процессы вместо их запуска
* как работает: OS-level virtualization
* usage: Сделать процесс настройки проще и упростить настройку на уровне инфраструктуры
* usage: Помочь разработчикам сосредоточиться исключительно на коде, сокращая время разработки и увеличивая продуктивность
* usage: Усилить возможности отладки с использованием встроенных функций
* usage: Изолировать приложения
* usage: Улучшить плотность использования серверов в форме контейнеризации
* usage: быстрое развертывание на уровне операционной системы
* **Docker objets** entities used to assemble an application

### dockerd (software)
* daemon
* manages containers, handles container objects
* listens for requests sent via **Docker Engine API**

### docker (software)
* a client program
* provides a command-line interface (CLI) to allow users to interact with Docker daemons

### docker file (docker object)
* def: commands to build an image
* a text file
* begins with a FROM
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
* инструкции:
  + `VOLUME /my_volume` создать том при запуске контейнера

### Image (docker object)
* def: набор файлов, соединенный с настройками, с помощью которого можно создать экземпляры, которые запускаются в отдельных контейнерах в виде изолированных процессов
* def: шаблон с инструкциями только для чтения для создания контейнеров
* def: a collection of files, libraries, configuration files that build up an environment
* def: a pre-built environment for a certain technology or service
* usage: to build containers
* usage: to store and ship applications
* build image = to define the environment to be used in a container
* is a read-only template
* is not a runtime
* строится с использованием инструкций для получения полной исполняемой версии приложения, зависящей от версии ядра сервера
* при запуске одного образа пользователь может создать несколько контейнеров

### Container (docker object)
* def: базовая единица программного обеспечения = код + все его зависимости для обеспечения запуска приложения независимо от окружения
* def: исполняемый пакет программного обеспечения, содержащий все необходимое для запуска приложени (системные программы, библиотеки, код, среды исполнения, настройки)
* def: набор окружения, необходимого для запуска определённого софта
* изолированное рабочее пространство
* может быть создан с использованием образа Docker
* нет ОС, использует ядро Linux и внутрь него помещаются только необходимые для запуска софта программы и библиотеки
* usage: runs applications: a database, a web server, a web framework, a test server, execute big data scripts, work on shell scripts, ... (one main process in one container)
* Как только контейнер запускается, создается набор пространств имен для этого контейнера. Они обеспечивают уровень изоляции для контейнеров, поскольку каждый контейнер работает в отдельном пространстве имен, с ограничением доступа к другим пространствам
* Ex: a container to be your MySQL database + a container to be your Wordpress server, connect the containers together
* a process created from an image
* is started by running an image
* is a encapsulated runtime environment
* one container provides one service in the project
* is managed using the Docker API or CLI
* bundle software, libraries, configuration files
* are isolated from one another
* communicate with each other through channels
* is not a virtual machine (so it is not recommended to use any patch based on ’tail -f’ and so forth when trying to run it)

### Docker Compose (a tool)
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
   + при помощи docker compose можно выполнить перенос сайтов при помощи нескольких команд, потребуется лишь изменить некоторые настройки и перенести на другой сервер резервную копию баз данных
  
### Docker Swarm (a tool)
* provides native clustering functionality for containers, which turns a group of Docker engines into a single virtual Docker engine
* in Docker 1.12 and higher, Swarm mode is integrated with Docker Engine
* _The docker swarm CLI utility_ allows users to run Swarm containers, create discovery tokens, list nodes in the cluster, and more.[37] The docker node CLI utility allows users to run various commands to manage nodes in a swarm, for example, listing the nodes in a swarm, updating nodes, and removing nodes from the swarm.[38] Docker manages swarms using the Raft consensus algorithm. According to Raft, for an update to be performed, the majority of Swarm nodes need to agree on the update.

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

### swarm mode (a tool)
* для организации кластеризации и планирования контейнеров
* собрать несколько узлов в единую виртуальную систему Docker и управлять ею

### Commands
`make` in the root of the directory to build and start all container  
`make` build to build all images in docker-compose  
`make up` to start all containers in docker-compose  
`make down` to remove all containers in the docker-compose   
`make start` to start all containers in the docker-compose   
`make stop` to stop all containers in the docker-compose  
`make status` to see the running containers  
`make logs` to see the logs of the containers  
`make rmi` to remove ALL IMAGES CREATED BY DOCKER-COMPOSE  
`make re` to remove, build and run all containers in docker-compose  
`docker-compose down` остановить контейнер  
`free`  
`build` сборка образа для Docker  
`create` создание нового контейнера  
`kill` принудительная остановка контейнера  
`dockerd` запуск сервиса Docker  
`commit` создание нового образа из изменений в контейнере  
`docker ps -a` список всех доступных контейнеров с их состоянием на сервере
`docker run` создание контейнеров, запускаемыми с использованием образов Docker  
`docker volume create —-name my_volume` создать том при запуске контейнера - команда  
`docker volume ls` список томов  
`docker volume inspect my_volume`  
`docker volume rm my_volume` удалить том  

### service
* allows containers to be scaled across Docker daemons
* **swarm** a set of cooperating daemons that communicate through the Docker API

### Docker API

### Docker registry
* a repository for images
* an API provideing containers (?)
* Ex: Docker Hub, quay.io, AWS ECR

### Efficency
* a single server or virtual machine runs several containers simultaneously
* to avoid the situations when we say “it worked on my machine”, because Docker containers will give us the same environment on all machines
* containers share the services of a single OS kernel => fewer resources than virtual machines  
* Linux: Docker uses the resource isolation features of the Linux kernel (cgroups, kernel namespaces) and a union-capable file system to allow containers to run within a single Linux instance, avoiding the overhead of starting and maintaining virtual machines
* MacOS: Docker uses a Linux virtual machine to run the containers
* the Linux kernel's support for namespaces mostly isolates an application's view of the operating environment, including process trees, network, user IDs and mounted file systems, while the kernel's cgroups provide resource limiting for memory and CPU 
* since 0.9, Docker includes its own component **libcontainer** to use virtualization facilities provided directly by the Linux kernel, in addition to using abstracted virtualization interfaces via libvirt, LXC and systemd-nspawn
* мгновенное время запуска

