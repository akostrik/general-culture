* snapshot: the state of a system at a particular point in time
* GCC = GNU Compiler Collection: a compiler supporting various programming languages, hardware architectures and operating systems
* multimedia framework
   + для работы с аудио- и видеоданными
    + ядро мультимедийных приложений
    + состоит из системы плагинов (кодеков, фильтров, (де)мультиплексаторов, вывода на экран, работы с файлами и т. п.), которые можно соединить для конвейерной обработки аудио/видео потока
    + плагины Windows
        - Audio Compression Manager (ACM)
        - DirectShow — (1996) (до 1997 г. назывался Active Movie)
        - DirectX Media Objects (DMOs)
        - Media Foundation — (2007) (начиная с Windows Vista)
        - QuickTime
        - Video for Windows (VfW) — (1992)
        - Windows Media
    + плагины Mac OS X
        - QuickTime
    + плагины Linux и платформенно-независимые с открытым кодом
        - FFmpeg
        - GStreamer
        - Helix DNA
        - Phonon
        - xine
        - Media Lovin' Toolkit, `.mlt`, набор средств для приложений, работающих с видео потоками, основа систем нелинейного монтажа Kdenlive, Flowblade, OpenShot, Shotcut
        - libVLC
    + плагины Проприетарные кроссплатформенные
        - Adobe Director
        - Adobe Flash
        - Java Media Framework (JMF)
        - Microsoft Silverlight
* `mkcert` генерация самоподписного сертификата, как правило используются для локальной разработки
* **cgroups**  a Linux kernel feature that limits, accounts for, and isolates the CPU, memory, disk I/O, etc usage of a collection of processes  
**Docker Engine** software that hosts the containers  
**REST** (representational state transfer) = a software architectural style
* guides the design and development of the architecture for the World Wide Web
* defines a set of constraints for how the architecture of a distributed, Internet-scale hypermedia system, such as the Web, should behave
* emphasises uniform interfaces, independent deployment of components, the scalability of interactions between them, and creating a layered architecture to promote caching to reduce user-perceived latency, enforce security, and encapsulate legacy systems
* provides stateless, reliable web-based applications

**Alpine**
* Легковесная система
* легковесный apk вместо привычного apt
* нет полноценного bash, вместо него sh
* [свой набор репозиториев](https://pkgs.alpinelinux.org/packages "список пакетов alpine").
   
**Linux kernel namespaces**
* partitions kernel resources such that one set of processes sees one set of resources while another set of processes sees a different set of resources
* the same namespace for a set of resources and processes, but those namespaces refer to distinct resources ((process IDs, host-names, user IDs, file names, some names associated with network access, and Inter-process communication)
* resources may exist in multiple spaces 
* are a fundamental aspect of containers in Linux
* Linux system starts out with a single namespace of each type, used by all processes, then processes create additional namespaces and join different namespaces
* изоляция групп процессов друг от друга
* в Linux шесть стандартных пространств имён: mnt, IPC, net, usr, pid, uts
* у каждого контейнера своё пространство имён и процессы, запущенные внутри этого пространства имён, контейнер не имеет доступа к чему-либо снаружи его пространства имён

**`init`**
* /sbin/init the first process started during booting of the OS by the kernel
* continues running until the system is shut down
* is a daemon
* is the direct or indirect ancestor of all other processes
* automatically adopts all orphaned processes
* PID 1
* загружает систему: запускаются стартовые сценарии, которые выполняют проверку и монтирование файловых систем, запуск необходимых демонов, настройку ядра (в том числе загрузку модулей ядра согласно установленному оборудованию, настройку IP-адресов, таблиц маршрутизации, ...), запуск графической оболочки
* основная информация для загрузки в /etc/inittab
* is in /sbin/init
* the system executes scripts /etc/rc.d directory
* **systemd** that includes an init daemon, with concurrent starting of services, service manager, ..., replaces `init` from 2019
     
**Daemon**
* strict def: its parent process terminates and the daemon is assigned `init` as its parent process and has no controlling terminal
* general def: any background process
* is not under the direct control of an interactive user
* is created either by a process forking a child and then immediately exiting, thus causing `init` to adopt the child, or by `init` directly => the parent (shell or `init`) receives exit notification
* the system often starts daemons at boot time that will respond to network requests, hardware activity, ...
* the process names of a daemon end with the letter d
* Ex: syslogd is a daemon that implements system logging facility, sshd is a daemon that serves incoming SSH connections, cron
* detaches from the invoking session
* dissociates from the controlling tty
* creates a new session and becoming the session leader of that session
* becoming a process group leader
* sets the root directory (/) as the current working directory => does not keep any directory in use that may be on a mounted file system
* changes the umask to 0 to allow open(), creat(), and other operating system calls to provide their own permission masks and not to depend on the umask of the caller
* redirects stdin, stdout, stderr to /dev/null or a logfile
* closes all the other file descriptors inherited from the parent process

