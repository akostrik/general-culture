## Verifications
* `netstat -ntlp | grep LISTEN` список прослушиваемых портов (netstat = network statistic)
```
tcp        0      0 127.0.0.1:32801         0.0.0.0:*               LISTEN      -                   
tcp        0      0 127.0.0.1:631           0.0.0.0:*               LISTEN      -                   
tcp        0      0 0.0.0.0:4244            0.0.0.0:*               LISTEN      -                   
tcp        0      0 0.0.0.0:443             0.0.0.0:*               LISTEN      -                   
tcp6       0      0 :::4244                 :::*                    LISTEN      -                   
tcp6       0      0 ::1:631                 :::*                    LISTEN      -                   
tcp6       0      0 :::443                  :::*                    LISTEN      -     
```
* `sudo netstat -ltupan` (l прослушивающиеся сокеты, n номера портов вместо названий служб, t TCP-соединения, u UDP-соединения, a только активные соединения)
```
Active Internet connections (servers and established)
Proto Recv-Q Send-Q Local Address           Foreign Address         State       PID/Program name    
tcp        0      0 127.0.0.1:32801         0.0.0.0:*               LISTEN      601/containerd      
tcp        0      0 127.0.0.1:631           0.0.0.0:*               LISTEN      591/cupsd           
tcp        0      0 0.0.0.0:4244            0.0.0.0:*               LISTEN      614/sshd: /usr/sbin 
tcp        0      0 0.0.0.0:443             0.0.0.0:*               LISTEN      4326/docker-proxy   
tcp        0      0 10.0.2.15:52602         34.107.243.93:443       ESTABLISHED 2608/firefox-esr    
tcp        0      0 10.0.2.15:58494         95.100.133.139:80       ESTABLISHED 2608/firefox-esr    
tcp        0    304 10.0.2.15:4244          10.0.2.2:41560          ESTABLISHED 1226/sshd: root@pts 
tcp6       0      0 :::4244                 :::*                    LISTEN      614/sshd: /usr/sbin 
tcp6       0      0 ::1:631                 :::*                    LISTEN      591/cupsd           
tcp6       0      0 :::443                  :::*                    LISTEN      4331/docker-proxy   
udp        0      0 10.0.2.15:68            10.0.2.2:67             ESTABLISHED 557/NetworkManager  
udp        0      0 0.0.0.0:631             0.0.0.0:*                           653/cups-browsed    
udp        0      0 0.0.0.0:5353            0.0.0.0:*                           534/avahi-daemon: r 
udp        0      0 0.0.0.0:39666           0.0.0.0:*                           534/avahi-daemon: r 
udp6       0      0 :::39221                :::*                                534/avahi-daemon: r 
udp6       0      0 :::5353                 :::*                                534/avahi-daemon: r 
```
* `ss -ltupn` (l все прослушивающиеся сокеты, n  номера портов вместо названий служб, t TCP-соединения, u UDP-соединения) (ss = socket statistics, современная альтернатива для netstat) display socket information
```
etid     State          Recv-Q         Send-Q             Local Address:Port                    Peer Address:Port         Process         
udp      UNCONN         0              0                      0.0.0.0:631                          0.0.0.0:*                            
udp      UNCONN         0              0                      0.0.0.0:5353                         0.0.0.0:*                            
udp      UNCONN         0              0                      0.0.0.0:39666                        0.0.0.0:*                            
udp      UNCONN         0              0                         [::]:39221                           [::]:*                            
udp      UNCONN         0              0                         [::]:5353                            [::]:*                            
tcp      LISTEN         0              4096                 127.0.0.1:32801                        0.0.0.0:*                            
tcp      LISTEN         0              128                  127.0.0.1:631                          0.0.0.0:*                            
tcp      LISTEN         0              128                    0.0.0.0:4244                         0.0.0.0:*                            
tcp      LISTEN         0              4096                   0.0.0.0:443                          0.0.0.0:*                            
tcp      LISTEN         0              128                       [::]:4244                            [::]:*                            
tcp      LISTEN         0              128                      [::1]:631                             [::]:*                            
tcp      LISTEN         0              4096                      [::]:443                             [::]:*
```
* `nmap localhost` открытые порты на удаленных хостах, проверка системы
```
Starting Nmap 7.93 ( https://nmap.org ) at 2024-06-07 19:00 EDT
Nmap scan report for localhost (127.0.0.1)
Host is up (0.000094s latency).
Other addresses for localhost (not scanned): ::1
rDNS record for 127.0.0.1: akostrik.42.fr
Not shown: 998 closed tcp ports (conn-refused)
PORT    STATE SERVICE
443/tcp open  https
631/tcp open  ipp
Nmap done: 1 IP address (1 host up) scanned in 0.04 seconds
```
* `sudo nmap -sT -sU -sV 159.89.108.187` (sT TCP-соединения, sU UDP-соединения, sV обнаружение версий программного обеспечения)
* `lsof -i` открытые соединения (list of open files)
```
COMMAND    PID     USER   FD   TYPE DEVICE SIZE/OFF NODE NAME
firefox-e 2608 akostrik  119u  IPv4  26195      0t0  TCP inception:52602->93.243.107.34.bc.googleusercontent.com:https (ESTABLISHED)
```
* `lsof -i :80` процессы, работающих с портом 80
* `lsof -nP -i` доступные соединения
```
COMMAND    PID     USER   FD   TYPE DEVICE SIZE/OFF NODE NAME
firefox-e 2608 akostrik   76u  IPv4  69898      0t0  TCP 10.0.2.15:50542->34.149.100.209:443 (ESTABLISHED)
firefox-e 2608 akostrik   81u  IPv4  69902      0t0  TCP 10.0.2.15:32860->34.160.144.191:443 (ESTABLISHED)
firefox-e 2608 akostrik  119u  IPv4  26195      0t0  TCP 10.0.2.15:52602->34.107.243.93:443 (ESTABLISHED)
```
* `lsof -nP -i | grep LISTEN` список прослушиваемых портов
* `hostname -I`
```
10.0.2.15 172.17.0.1 172.20.0.1 
```
* `ip addr show | grep inet| awk '{print $2; }'` my IP
```
127.0.0.1/8
::1/128
10.0.2.15/24
fe80::a00:27ff:fe0b:adde/64
172.17.0.1/16
fe80::42:f1ff:fe49:18ae/64
172.20.0.1/16
fe80::42:54ff:fe0e:2712/64
fe80::18ca:d6ff:feb2:1a18/64
```
* `ip addr show` network interfaces
```
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host noprefixroute 
       valid_lft forever preferred_lft forever
2: enp0s3: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
    link/ether 08:00:27:0b:ad:de brd ff:ff:ff:ff:ff:ff
    inet 10.0.2.15/24 brd 10.0.2.255 scope global dynamic noprefixroute enp0s3
       valid_lft 84370sec preferred_lft 84370sec
    inet6 fe80::a00:27ff:fe0b:adde/64 scope link noprefixroute 
       valid_lft forever preferred_lft forever
3: docker0: <NO-CARRIER,BROADCAST,MULTICAST,UP> mtu 1500 qdisc noqueue state DOWN group default 
    link/ether 02:42:f1:49:18:ae brd ff:ff:ff:ff:ff:ff
    inet 172.17.0.1/16 brd 172.17.255.255 scope global docker0
       valid_lft forever preferred_lft forever
    inet6 fe80::42:f1ff:fe49:18ae/64 scope link 
       valid_lft forever preferred_lft forever
10: br-4b3d69138e4a: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP group default 
    link/ether 02:42:54:0e:27:12 brd ff:ff:ff:ff:ff:ff
    inet 172.20.0.1/16 brd 172.20.255.255 scope global br-4b3d69138e4a
       valid_lft forever preferred_lft forever
    inet6 fe80::42:54ff:fe0e:2712/64 scope link 
       valid_lft forever preferred_lft forever
14: veth245a34d@if13: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue master br-4b3d69138e4a state UP group default 
    link/ether 1a:ca:d6:b2:1a:18 brd ff:ff:ff:ff:ff:ff link-netnsid 0
    inet6 fe80::18ca:d6ff:feb2:1a18/64 scope link 
       valid_lft forever preferred_lft forever
```
* `ip addr` my IP
```
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host noprefixroute 
       valid_lft forever preferred_lft forever
2: enp0s3: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
    link/ether 08:00:27:0b:ad:de brd ff:ff:ff:ff:ff:ff
    inet 10.0.2.15/24 brd 10.0.2.255 scope global dynamic noprefixroute enp0s3
       valid_lft 84346sec preferred_lft 84346sec
    inet6 fe80::a00:27ff:fe0b:adde/64 scope link noprefixroute 
       valid_lft forever preferred_lft forever
3: docker0: <NO-CARRIER,BROADCAST,MULTICAST,UP> mtu 1500 qdisc noqueue state DOWN group default 
    link/ether 02:42:f1:49:18:ae brd ff:ff:ff:ff:ff:ff
    inet 172.17.0.1/16 brd 172.17.255.255 scope global docker0
       valid_lft forever preferred_lft forever
    inet6 fe80::42:f1ff:fe49:18ae/64 scope link 
       valid_lft forever preferred_lft forever
10: br-4b3d69138e4a: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP group default 
    link/ether 02:42:54:0e:27:12 brd ff:ff:ff:ff:ff:ff
    inet 172.20.0.1/16 brd 172.20.255.255 scope global br-4b3d69138e4a
       valid_lft forever preferred_lft forever
    inet6 fe80::42:54ff:fe0e:2712/64 scope link 
       valid_lft forever preferred_lft forever
14: veth245a34d@if13: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue master br-4b3d69138e4a state UP group default 
    link/ether 1a:ca:d6:b2:1a:18 brd ff:ff:ff:ff:ff:ff link-netnsid 0
    inet6 fe80::18ca:d6ff:feb2:1a18/64 scope link 
       valid_lft forever preferred_lft forever
```
* `ip a`
```
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host noprefixroute 
       valid_lft forever preferred_lft forever
2: enp0s3: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
    link/ether 08:00:27:0b:ad:de brd ff:ff:ff:ff:ff:ff
    inet 10.0.2.15/24 brd 10.0.2.255 scope global dynamic noprefixroute enp0s3
       valid_lft 84312sec preferred_lft 84312sec
    inet6 fe80::a00:27ff:fe0b:adde/64 scope link noprefixroute 
       valid_lft forever preferred_lft forever
3: docker0: <NO-CARRIER,BROADCAST,MULTICAST,UP> mtu 1500 qdisc noqueue state DOWN group default 
    link/ether 02:42:f1:49:18:ae brd ff:ff:ff:ff:ff:ff
    inet 172.17.0.1/16 brd 172.17.255.255 scope global docker0
       valid_lft forever preferred_lft forever
    inet6 fe80::42:f1ff:fe49:18ae/64 scope link 
       valid_lft forever preferred_lft forever
10: br-4b3d69138e4a: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP group default 
    link/ether 02:42:54:0e:27:12 brd ff:ff:ff:ff:ff:ff
    inet 172.20.0.1/16 brd 172.20.255.255 scope global br-4b3d69138e4a
       valid_lft forever preferred_lft forever
    inet6 fe80::42:54ff:fe0e:2712/64 scope link 
       valid_lft forever preferred_lft forever
14: veth245a34d@if13: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue master br-4b3d69138e4a state UP group default 
    link/ether 1a:ca:d6:b2:1a:18 brd ff:ff:ff:ff:ff:ff link-netnsid 0
    inet6 fe80::18ca:d6ff:feb2:1a18/64 scope link 
       valid_lft forever preferred_lft forever
```
* `ifconfig`
```
br-4b3d69138e4a: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 172.20.0.1  netmask 255.255.0.0  broadcast 172.20.255.255
        inet6 fe80::42:54ff:fe0e:2712  prefixlen 64  scopeid 0x20<link>
        ether 02:42:54:0e:27:12  txqueuelen 0  (Ethernet)
        RX packets 55  bytes 11730 (11.4 KiB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 110  bytes 13525 (13.2 KiB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

docker0: flags=4099<UP,BROADCAST,MULTICAST>  mtu 1500
        inet 172.17.0.1  netmask 255.255.0.0  broadcast 172.17.255.255
        inet6 fe80::42:f1ff:fe49:18ae  prefixlen 64  scopeid 0x20<link>
        ether 02:42:f1:49:18:ae  txqueuelen 0  (Ethernet)
        RX packets 1218  bytes 52139 (50.9 KiB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 1379  bytes 6277876 (5.9 MiB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

enp0s3: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 10.0.2.15  netmask 255.255.255.0  broadcast 10.0.2.255
        inet6 fe80::a00:27ff:fe0b:adde  prefixlen 64  scopeid 0x20<link>
        ether 08:00:27:0b:ad:de  txqueuelen 1000  (Ethernet)
        RX packets 36605  bytes 45015842 (42.9 MiB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 11008  bytes 1143330 (1.0 MiB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

lo: flags=73<UP,LOOPBACK,RUNNING>  mtu 65536
        inet 127.0.0.1  netmask 255.0.0.0
        inet6 ::1  prefixlen 128  scopeid 0x10<host>
        loop  txqueuelen 1000  (Local Loopback)
        RX packets 8395  bytes 471920 (460.8 KiB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 8395  bytes 471920 (460.8 KiB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

veth245a34d: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet6 fe80::18ca:d6ff:feb2:1a18  prefixlen 64  scopeid 0x20<link>
        ether 1a:ca:d6:b2:1a:18  txqueuelen 0  (Ethernet)
        RX packets 55  bytes 12500 (12.2 KiB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 142  bytes 17295 (16.8 KiB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0
```
* `service ssh status`
* `ufw status`
* `telnet loclhost 80` verify
* `ipconfig` depreciated

## Ports
* порт = логический объект
  + конечная точкой связи в сетевых соединениях
  + связан с процессом или службой
  + находится в одном из четырех состояний: открыт, закрыт, filtered, unfiltered
  + если порт открыт, то программное обеспечение целевого оборудования прослушивает его соединения и пакеты
* открытый порт не представляет опасности
  + может быть угрозой безопасности ОС, если его использует какая-то программа
* 21 ftp
* 22 SSH
* 53 DNS  
* 80 HTTP  
* 443 HTTPS  
* 3306 MySQL  

## Socket
* IP + номер порта

## NAT = Network Address Translation = IP Masquerading = Network Masquerading = Native Address Translation = трансляция сетевых адресов
![Screenshot from 2024-06-07 16-11-34](https://github.com/privet100/general-culture/assets/22834202/27cd8dc7-fee4-4c40-90e5-6dbcc5f8899c)
* механизм в сетях TCP/IP
* une fonctionnalité réseau utilisée sur les routeurs (par exemple, une Box Internet) pour faire communiquer deux réseaux différents (le réseau local et Internet)
* используется для подключения устройств в локальной сети к Интернет
* изменение IP-адреса во время передачи пакетов Ethernet через маршрутизатор
* преобразовывает IP-адреса транзитных пакетов
* преобразовывает скрытые локальные IP-адреса сети во внешние
* преобразование адреса может производиться маршрутизирующим устройством (маршрутизатором, сервером доступа, межсетевым экраном)
* **SNAT** наиболее популярный
  + замена адреса источника при прохождении пакета в одну сторону и обратной замене адреса назначения в ответном пакете
* роутер принимает пакет от локального компьютера
  + если IP-адрес назначения локальный адрес, то пакет пересылается другому локальному компьютеру
  + если нет, то пакет надо переслать наружу в интернет
    - обратным адресом в пакете указан локальный адрес компьютера, который из интернета будет недоступен
    - роутер на лету подменяет обратный IP-адрес пакета на свой внешний (видимый из интернета) IP-адрес и меняет номер порта (чтобы различать ответные пакеты)
    - комбинацию, нужную для обратной подстановки, роутер сохраняет у себя во временной таблице
* сэкономить IP-адреса
  + транслирует несколько внутренних IP-адресов в один внешний публичный IP-адрес
  + так построено большинство сетей в мире: на небольшой район домашней сети местного провайдера или на офис выделяется 1 публичный (внешний) IP-адрес, за которым работают и получают доступ интерфейсы с приватными (внутренними) IP-адресами
* предотвратить или ограничить обращение снаружи ко внутренним хостам, оставляя возможность обращения изнутри наружу
  + при инициации соединения изнутри сети создаётся трансляция
  + ответные пакеты, поступающие снаружи, соответствуют созданной трансляции и поэтому пропускаются
  + если для пакетов, поступающих снаружи, соответствующей трансляции не существует, они не пропускаются
* скрыть внутренние сервисы внутренних хостов/серверов
  + это та же трансляция на определённый порт
  + но возможно подменить внутренний порт официально зарегистрированной службы (например, 80-й порт TCP (HTTP-сервер) на внешний 54055-й)
  + снаружи, на внешнем IP-адресе после трансляции адресов на сайт (или форум) для осведомлённых посетителей можно будет попасть по адресу http://example.org:54055, но на внутреннем сервере, находящемся за NAT, он будет работать на 80-м порту
* source NAT = предоставления пользователям локальной сети с внутренними адресами доступа к сети Интернет
* destination NAT = обращения извне транслируются межсетевым экраном на компьютер пользователя в локальной сети, имеющий внутренний адрес и потому недоступный извне сети без NAT

## Марштуртизация
### DNS server (Domain Name System)
* распределённая система для получения информации о доменах
* для получения IP-адреса по имени компьютера
  +  для людей проще запоминать буквенные адреса
* для получения информации о маршрутизации почты и/или обслуживающих узлах для протоколов в домене (SRV-запись)
* поддерживается с помощью иерархии DNS-серверов, взаимодействующих по определённому протоколу
* иерархическая структура имён и зон: каждый сервер, отвечающий за имя, может передать ответственность за дальнейшую часть домена другому серверу => ответственность за информацию на серверах различных организаций, отвечающих только за «свою» часть доменного имени
* дополнительные возможности
  + динамические обновления
  + проверка целостности передаваемых данных DNS Security Extensions (**DNSSEC**)
    - данные не шифруются
    - достоверность проверяется криптографическими способами
    - передача криптографической информации (сертификатов), используемых для установления безопасных и защищённых соединений   
  + защита транзакций (TSIG)
  + поддержка различных типов информации
* в браузере `8.8.8.8`
### /etc/hosts
  + сменить алиас локального домена 127.0.0.1 на akostrk.42.fr
  + до появления DNS служил маршрутизатором: преобразование между доменными и IP-адресами производилось с использованием текстового файла hosts, который составлялся централизованно и автоматически рассылался на каждую из машин в своей локальной сети
  + обладает приоритетом перед DNS-серверами
    - можно зайти на сайт, не дожидаясь делегирования домена
  + можное заблокировать на локальном компьютере доступ к определённому сайту, указав соответствующую запись
  + un fichier très visé par les hackers, il permettrait de rediriger google.fr -> un faux google
### /etc/resolv.conf
  * config of **DNS resolver**
  * translates human-friendly domain names into the numeric IP addresses
```
                              # when no domain suffix is supplied -> the given query name + search domains = a fully qualified domain 
search example.com local.test # tries additionally somehost.example.com and somehost.local.test
                              # a list of IP addresses of nameservers for resolution:
nameserver 10.0.0.17          # the resolver to query for the name server with IP 10.0.0.17
nameserver 10.1.0.12          # is only used when the first or last used server is unavailable
nameserver 10.16.0.7
```
### DHCP Dynamic Host Configuration Protocol 
* un protocole client/serveur
* fournit IP et d’autres informations (masque de sous-réseau, passerelle par défaut) de configuration à un hôte

## CGI = Common Gateway Interface
* спецификация интерфейса
* для связи с веб-сервером
* веб-сервер
  + получает запрос от клиента
  + преобразует в CGI-форму
  + вызывает обработчик
  + конвертирует ответ обработчика из CGI-формы в форму HTTP-ответа клиенту
* скрипты помещают в каталог cgi (или cgi-bin) сервера 
* ранее был одним из наиболее распространённых средств создания динамических сайтов

## FastCGI
* интерфейс
* клиент-серверный протокол взаимодействия веб-сервера и приложения
* развитие технологии CGI
  + более производительный и безопасный по сравнению с CGI
    
## iptables
* гибкий и надежный фаервол (брандмауэр, межсетевой экран)
* интерфейс управления работой межсетевого экрана netfilter 
* защищает от угроз сети, уязвимостей программного обеспечения
  + защита от внешних вторжений, перенаправления портов, действий с трафиком
  + для домашних компьютеров не актуально
    - они подключены к сети через роутеры и NAT, которые скрывают их от внешней сети
  + для серверов актуально 
* подсистема iptables и Netfilter встроена в ядро
* сетевые пакеты, проходящие через компьютер, отправляются им или предназначены ему, ядро направляет через фильтр iptables
  + если проверка пройдена, действие
* пакеты: входящие / исходящие / проходящие => в фильтре iptables пакеты делятся на три цепочки:
  + Input - обрабатывает входящие пакеты и подключения (внешний пользователь пытается подключиться по ssh, веб-сайт отправит вам контент по запросу браузера, ...)
  + forward - для проходящих соединений (на маршрутизаторах, если компьютер раздает wifi, ...)
  + output - для исходящих пакетов и соединений (пакеты, созданный при попытке выполнить ping losst.pro, когда вы запускаете браузер и пытаетесь открыть любой сайт, ...)
* содержит пять таблиц:
  + `filter` все действия, типичные для межсетевых экранов
  + `nat` для преобразования сетевых адресов (например, проброс портов)
  + `raw` для настройки пакетов, поэтому они освобождаются от отслеживания // используется в сложных конфигурациях с несколькими маршрутизаторами
  + `mangle` для специальных преобразований пакетов // используется в сложных конфигурациях с несколькими маршрутизаторами
  + `security` в сетевых правилах для Мандатного управления доступом // используется в сложных конфигурациях с несколькими маршрутизаторами
* таблицы состоят из цепочек
* **цепочка** = набор правил, следующих друг за другом в определённом порядке
* таблица filter содержит три встроенные цепочки: INPUT, OUTPUT, FORWARD, активируются в определённые моменты фильтрации пакетов
* таблица nat включает стандартные цепочки PREROUTING, POSTROUTING, OUTPUT
* описание стандартных цепочек других таблиц можно найти в `iptables(8)`
* по умолчанию все цепочки пусты, не содержат каких-либо правил
* вы должны добавить правила в те цепочки, которые собираетесь использовать
* у цепочек также задана политика по умолчанию — обычно ACCEPT
  + её можно изменить на DROP, если вы хотите убедиться, что ни один пакет не проскочит мимо вашего набора правил
  + политика по умолчанию применяется к пакету только после того, как он пройдёт через все существующие правила
* фильтрация пакетов основана на правилах
* **правило** = несколько условий и действия-цели
  + если пакет соответствует всем условиям, то к нему применяется указанное действие
  + например, на какой интерфейс пришёл пакет (eth0, eth1), какого он типа (ICMP, TCP, UDP), на какой порт направляется
* **цель**
  + указывается опцией `-j/--jump`
  + может быть встроеной (built-in): ACCEPT, DROP, QUEUE, RETURN,... - участь пакета решается незамедлительно и обработка пакета в таблице прекращается
  + может быть целью-расширением (extension): REJECT, LOG, ...
    - могут быть завершающими (как встроенные) или незавершающими (как пользовательские цепочки), подробнее см. iptables-extensions(8)
  + может быть переходом на пользовательскую цепочку - пакет проходит через неё, возвращается в исходную цепочку и продолжает со следующего после перехода правила
    - можете добавить собственные цепочки для большей эффективности или удобства, пример создания цепочек можно найти в статье Настройка межсетевого экрана
* вы не можетеотключить iptables остановив сервис обновления правил iptables, подсистема работает на уровне ядра
* `iptables -L` правила брандмауэра iptables
  + понять, какие порты закрыты с его помощью
  + цепочка INPUT отвечает за открытые порты
    - через нее проходят все входящие пакеты
    - политика по умолчанию - ACCEPT = подключение ко всем портам разрешено
  + цепочка OUTPUT
  + цепочка FORWARD
* `iptables -t nat --list`
* `sudo iptables -F` очистить правила, если сделаете что-то не так
* `sudo iptables -I INPUT -p tcp --dport 1924 -j ACCEPT` открыть порты
* https://losst.pro/nastrojka-iptables-dlya-chajnikov
  
```
                               XXXXXXXXXXXXXXXXXX
                             XXX      Сеть      XXX
                                       |
                                       v
+---------------+             +-------------------+
|таблица: filter| <---+       |таблица: nat       |
|цепочка: INPUT |     |       |цепочка: PREROUTING|
+-------+-------+     |       +--------+----------+
        |             |                |
        v             |                v
[локальный процесс]   |         ***************          +----------------+
        |             +-------+  Маршрутизация  +------> |таблица: filter |
        v                       ***************          |цепочка: FORWARD|
 ***************                                         +-------+--------+
  Маршрутизация                                                  |
 ***************                                                 |
        |                                                        |
        v                       ***************                  |
+---------------+     +------>   Маршрутизация   <---------------+
|таблица: nat   |     |         ***************
|цепочка: OUTPUT|     |                +
+------+--------+     |                |
        |             |                v
        v             |      +---------------------+
+---------------+     |      | таблица: nat        |
|таблица: filter| +---+      | цепочка: POSTROUTING|
|цепочка: OUTPUT|            +---------+-----------+
+---------------+                      |
                                       v
                               XXXXXXXXXXXXXXXXXX
                             XXX      Сеть      XXX
                               XXXXXXXXXXXXXXXXXX
```

## telnet = terminal network = telecommunication network =teletype network
* un protocole utilisé sur tout réseau TCP/IP
* permet de communiquer avec un serveur distant en échangeant des lignes de texte et en recevant des réponses également sous forme de texte
* un moyen de communication très généraliste et bi-directionnel
* appartient à la couche application du modèle OSI et du modèle ARPA
* il était utilisé pour administrer des serveurs Unix distant ou de l'équipement réseau, avant de tomber en désuétude par défaut de sécurisation (le texte étant échangé en clair) et l'adoption de SSH

## NIC = Network Interface Controller = сетевой адаптер = сетевая карта = сетевая плата = Ethernet-адаптер
* соединяет компьютер с сетью (локальной, интернетом, интранетом) и обмен данными между устройствами
* функция:
  + получение данных из сетевого кабеля и перевод в формат, который может распознать процессор компьютера
  + отправка данных с ПК к сетевому кабелю
  + кодирование и декодирование данных при передаче по сети
  + передача данных между компьютерами и устройствами внутри сети
* компоненты:
  + контроллер, функция микропроцессора и обрабатывает данные
  + порт, к которому подключается кабель Ethernet
  + разъем ROM для подключения рабочих станций к сети
  + антенна, усиливающая сигнал при беспроводном соединении с сетью
* модели
  + физические внешние
  + физические встроенные
  + виртуальные
    - это не устройство, а интерфейс
    - работает на базе приложения или программного обеспечения в ОС
    - только в виртуальных средах
    - «Выйти в интернет» без физической сетевой карты не получится
    - для взаимодействия между виртуальными машинами и для получения дополнительных сетевых интерфейсов
* по типу портов:
  + оптический с оптоволоконным кабелем
  + AUI с толстым коаксиальным кабелем
  + BNC с тонким коаксиальным кабелем
  + RJ-45 с кабелем витой пар

## *
* **Wireshark** инструмент для захвата и анализа сетевого трафика
