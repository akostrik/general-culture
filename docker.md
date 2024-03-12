
# Docker 
* a set of platform as a service (PaaS) products 
* packages an application and its dependencies in a virtual container that can run, in isolation, in a variety of locations on Linux / Windows / macOS
* reproduces a run-time environments on any machine
* use OS-level virtualization
* имитирует Linux дистрибутивы, окружения или установочные процессы вместо их запуска
* **Docker objets** entities used to assemble an application

![Capture d’écran de 2024-01-16 01-47-57](https://github.com/akostrik/inception/assets/22834202/29a86b2f-bb7f-4297-b2ac-3a3a7a57194a)

## dockerd (software)
* daemon
* manages containers
* handles container objects
* listens for requests sent via **Docker Engine API**

## docker (software)
* a client program
* provides a command-line interface (CLI)
* allows users to interact with Docker daemons

## docker file (docker object)
* a text file
* begins with a FROM
* commands to build an image
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

## Image (docker object)
* usage: to build containers
* usage: to store and ship applications
* a collection of files, libraries, configuration files that build up an environment
* a pre-built environment for a certain technology or service
* build image = to define the environment to be used in a container
* is a read-only template
* is not a runtime
* набор окружения, необходимого для запуска определённого софта, в контенере не содержится полноценной операционной системы, контейнер использует ядро Linux и внутрь него помещаются только необходимые для запуска софта программы и библиотеки. Для запуска отдельного софта не нужна вся полноценная операционная система, достаточно рабочего ядра и некоторого окружения из всех зависимостей - модулей, библиотек, пакетов и скриптов. По такому принципу работает wine.

## Container (docker object)
* usage: runs applications: a database, a web server, a web framework, a test server, execute big data scripts, work on shell scripts, ... (one main process in one container)
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

## Docker Compose (a tool)
* _Docker-compose CLI_
* defines and runs multi-container Docker applications
* creation and start-up of all the containers with a single command
* runs commands on multiple containers at once (building images, scaling containers, running containers that were stopped). Commands related to image manipulation, or user-interactive options, are not relevant in Docker Compose because they address one container
* uses YAML files to configure the application's services
* `docker-compose.yml` to build the Docker images, define an application's services + configuration options
    + `build` defines configuration options such as the Dockerfile path
    + `command` override default Docker commands
    + the network line

## Docker Swarm (a tool)
* provides native clustering functionality for containers, which turns a group of Docker engines into a single virtual Docker engine
* in Docker 1.12 and higher, Swarm mode is integrated with Docker Engine
* _The docker swarm CLI utility_ allows users to run Swarm containers, create discovery tokens, list nodes in the cluster, and more.[37] The docker node CLI utility allows users to run various commands to manage nodes in a swarm, for example, listing the nodes in a swarm, updating nodes, and removing nodes from the swarm.[38] Docker manages swarms using the Raft consensus algorithm. According to Raft, for an update to be performed, the majority of Swarm nodes need to agree on the update.

## Docker Volume (a tool)
Facilitates the independent persistence of data, allowing data to remain even after the container is deleted or re-created

## Commands
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

## service
* allows containers to be scaled across Docker daemons
* **swarm** a set of cooperating daemons that communicate through the Docker API

## Docker API

## Docker registry
* a repository for images
* an API provideing containers (?)
* Ex: Docker Hub, quay.io, AWS ECR

## Efficency
* a single server or virtual machine runs several containers simultaneously
* to avoid the situations when we say “it worked on my machine”, because Docker containers will give us the same environment on all machines
* containers share the services of a single OS kernel => fewer resources than virtual machines  
* Linux: Docker uses the resource isolation features of the Linux kernel (cgroups, kernel namespaces) and a union-capable file system to allow containers to run within a single Linux instance, avoiding the overhead of starting and maintaining virtual machines
* MacOS: Docker uses a Linux virtual machine to run the containers
* the Linux kernel's support for namespaces mostly isolates an application's view of the operating environment, including process trees, network, user IDs and mounted file systems, while the kernel's cgroups provide resource limiting for memory and CPU 
* since 0.9, Docker includes its own component **libcontainer** to use virtualization facilities provided directly by the Linux kernel, in addition to using abstracted virtualization interfaces via libvirt, LXC and systemd-nspawn
* мгновенное время запуска

# WordPress

# This project
* Makefile sets up the entire application
* Makefile calls docker-compose.yml
* docker-compose.yml calls dockerfiles
* a dockerfile builds a Docker image
* A Docker container that contains NGINX with TLSv1.2 or TLSv1.3
• A Docker container that contains WordPress + php-fpm (it must be installed and
configured) only without nginx
• A Docker container that contains MariaDB only without nginx
* A volume contains the WordPress database
• A second volume contains the WordPress website files
• A docker-network establishes the connection between the threee containers
* the containers musn’t be started with a command running an infinite loop, any command used as entrypoint, or used in entrypoint scripts
* prohibited: `tail -f`, `bash`, `sleep infinity`, `while true`

![Screenshot from 2024-01-22 14-35-34](https://github.com/akostrik/inception/assets/22834202/0120b09c-7f24-423c-a9fb-2cda4b27e32a)

## structure
```
root/
│
├── src/
│   ├── requirements/
│   │   ├── mariadb/
│   │   │   ├── conf/
│   │   │   ├── Dockerfile
│   │   │   └── tools/
│   │   ├── nginx/
│   │   │   ├── conf/
│   │   │   ├── Dockerfile
│   │   │   └── tools/
│   │   └── wordpress/
│   │       ├── conf/
│   │       ├── Dockerfile
│   │       └── tools/
│   ├── .env
│   └── docker-compose.yml
└── Makefile
```

# Notes 
https://medium.com/swlh/wordpress-deployment-with-nginx-php-fpm-and-mariadb-using-docker-compose-55f59e5c1a  
https://docs.docker.com/compose/compose-file/compose-file-v3/  
https://www.php.net/manual/en/install.fpm.configuration.php  
https://unixcop.com/install-redis-server-debian-11/  
https://kinsta.com/blog/adminer/  
https://redis.io/docs/reference/eviction/  
https://www.ssldragon.com/blog/what-is-openssl/#:~:text=With%20OpenSSL%2C%20you%20can%20apply,do%20all%20kind%20of%20verifications.  
https://www.techtarget.com/whatis/definitions/C  
https://www.internetsociety.org/deploy360/tls/basics/  
https://ivan-shamaev.ru/docker-compose-tutorial-container-image-install/  
[WordPress Deployment with NGINX, PHP-FPM and MariaDB using Docker Compose](https://www.php.net/manual/en/install.fpm.configuration.php)
[PHP configuration](https://www.php.net/manual/en/install.fpm.configuration.php)
[SSL](https://www.ssldragon.com/blog/what-is-openssl/#:~:text=With%20OpenSSL%2C%20you%20can%20apply,do%20all%20kind%20of%20verifications.)
[TLS](https://www.internetsociety.org/deploy360/tls/basics/)

https://github.com/codesshaman/inception  
https://github.com/ifanzilka/Inception  
https://github.com/SavchenkoDV/inception_School21_Ecole42  
