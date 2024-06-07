## Network
* **A virtual network adapter** a software-emulated physical device
  
### NAT = Network Address Translation = трансляция сетевых адресов = IP Masquerading = Network Masquerading = Native Address Translation
![Screenshot from 2024-06-07 17-22-13](https://github.com/privet100/general-culture/assets/22834202/1a99dea0-1916-47f8-900e-f95af4beaab7)
* [NAT](https://github.com/privet100/general-culture/blob/main/network/network_vpn_ports_etc.md#nat--network-address-translation--ip-masquerading--network-masquerading--native-address-translation--%D1%82%D1%80%D0%B0%D0%BD%D1%81%D0%BB%D1%8F%D1%86%D0%B8%D1%8F-%D1%81%D0%B5%D1%82%D0%B5%D0%B2%D1%8B%D1%85-%D0%B0%D0%B4%D1%80%D0%B5%D1%81%D0%BE%D0%B2)
* par default
* только доступ в интернет с VM
* VirtualBox Host-Only Enthernet Adapter
* VM автоматически получает доступ в интернет при помощи NAT
* первая сетевая карта NAT принадлежит локальной сети 10.0.2.0
* VM и основная ОС совместно используют одно подключение к физической сети TCP/IP
* un réseau local différent et isolé de celui de la machine hôte
  + gardant la possibilité d’accéder à Internet grâce à du routage et une translation d’adresse (NAT)
* сетевые интерфейсы VM и хостовой машины изолированы
  + NAT изолирует виртуальную машину от соединений извне
  + у VM виртуальная сеть, в которой она одна
  + VM не указывается в сети в качестве отдельного компьютера
  + VM недоступна для хостовой
  + VM недоступна для внешней сети (internet, IP адрес VM != сетевой адрес сети хоста)
* с VM можно просматривать интернет-сайты и выполнять различные действия в сети
  + подключение к интернету с помощью NAT
  + проводником в интернет выступает хост-система, через неё проходят все входящие и исходящие запросы
  + VM получает сетевой адрес и другие настройки в локальной сети от сервера VirtualBox DHCP 
  + VM подключается к сети через маршрутизатор (сетевой модуль VirtualBox NAT) (также как реальный комп подключается к Internet), который:
    - получает cетевые пакеты посылаемые к VM 
    - пересылает данные стека TCP/IP хосту
    - определяет, какие данные посылать приложениям хоста, а какие другим компьютерам той же сети, что и хост, используя сетевой интерфейс хоста
    - перехватывает и пересылает ответные пакеты VM
* нет доступа к другим гостевым ОС
* VM access hosts in a physical local area network (LAN) by using a virtual NAT device
* external networks, including the internet, are accessible VM
* VM is not accessible from a host machine, or from other machines in the network
* The IP of the VM network adapter is obtained via DHCP
* The IP addresses of the network cannot be changed in the GUI
* VirtualBox has a built-in DHCP server
* VirtualBox has a built-in DHCP NAT engine
* A virtual NAT device uses the physical network adapter of the VirtualBox host as an external network interface
* the default IP of the virtual DHCP server is 10.0.2.2 (= IP of the default gateway for a VM)
* the network mask is 255.255.255.0
* if you configure the network adapters of two VMs 
  + each VM will obtain the 10.0.2.15 IP address in its own isolated network behind a private virtual NAT device
  + the default gateway for each VM is 10.0.2.2
* in VirtualBox IP addresses are not changed

### Bridged
![Screenshot from 2024-06-07 17-41-26](https://github.com/privet100/general-culture/assets/22834202/af298eb5-4c1c-4eb8-b238-2166624791f5)
* Доступ к VM из хостовой и из инета по http, https
* to give your virtual machine access to the network ?
* на виртуалке нет инета ?
* you can access a host machine, hosts of the physical network and external networks, including internet from a VM
* VM can be accessed from the host machine and from other hosts (and VMs) connected to the physical network
* виртаулку видно всем из сети
* VM подключается к основной сети (к физической сети TCP/IP) как полноценное устройство, отдельный комп
  + VM communicates with other computer on the network, as if it is a physical computer on the network
* VM has its own IP on a bridged network, on a TCP/IP network
  + VM acquires an IP address and other network details from a DHCP server
* network packets are sent and received directly from/to the virtual network adapter without additional routing
  + a net filter driver is used by VirtualBox for a bridged network mode in order to filter data from the physical network adapter of the host
* to run servers on VM
* VM must be fully accessible from a physical local area network
* VM virtual network adapter is connected to a physical network to which a physical network adapter of the host is connected
* VM virtual network adapter uses the host network interface for a network connection
* VM is connected to a network using the network adapter on the host system, драйвер и физическое устройство хоста (сетевой адаптер, интерфейс хоста, маршрутизатор, шлюз между гостевой и физической сетью, net filter, сетевую карту)
* IP of a VM virtual network adapter can belong to the same network as the IP address of the physical network adapter of the host
  + if there is a DHCP server in your physical network, the virtual network adapter of the VM will obtain the IP address automatically (if obtaining an IP address automatically is set in the network interface settings in a guest OS)
  + thus, the default gateway for a virtual network adapter operating in the bridged mode is the same as for your host machine, for ex:
    - IP of the physical network: 10.10.10.0/24
    - IP of the default gateway in the physical network: 10.10.10.1
    - IP of the DHCP server in the physical network: 10.10.10.1
    - IP configuration of the host: IP = 10.10.10.72; netmask = 255.255.255.0; default gateway = 10.10.10.1
    - IP configuration of VM: IP = 10.10.10.91; netmask = 255.255.255.0; default gateway = 10.10.10.1
* драйвер
  + обрабатывает данные проходящие через физический сетевой интерфейс
  + перехватывает VirtualBox пакеты из физической сети и изменяет данные в них
* `netstat -tulpn 5000` кто слушает порт 5000
* `wget https://localhost/index.html --no-check-certificat` проверить без браузера
* `http://localhost:3000/` = `http://myHostname:3000/`
  + `hostname` to know your hostname
* диагностика
  + cбросить правила для iptables iptables -F и iptables -X
  + `ss -tunap|grep 5000` слушает ли костыль нужный порт и нужный адрес
  + `tcpdump -ni any port 5000` снять дамп на VM, будет видно, доходят ли запросы и что с ними происходит
  + установить strace 
  + `strace flask/bin/python run.py` будет видно, что происходит в системе при вызове костыля, разумеется, если вызов до него доходит
  + дальше по ситуации

### Доступ к VM только из хостовой по http, https
* **Виртуальный адаптер хоста** (вместо NAT)
* по умолчанию адрес VM из сети 192.168.56.0/24, хосту 192.168.56.1
* можно выдать VM отдельный IP
* можно обращаться к VM по IP

### Доступ к VM из хостовой по ssh
* NAT
* Port Forwardign Rules:
  + внешний белый ip
  + протокол, по которому работает сервис (например tcp)
  + адрес на хостовой машине, подключения к которому будут направляться на виртуальную машину
    - можно написать ip в локальной сети
    - нельзя указывать 127.0.0.1, на этом адресе вы сможете получать и передавать пакеты только на него же, даже на виртуалку не сможете, потому что на виртуалке свой 127.0.0.1
    - `ifconfig` узнать адрес хоста в локальной сети
  + порт хоста - подключения к которому нужно перенаправлять на VM
  + адрес VM - куда направлять подключенияЖ: оставьте пустым
    - а то весь интернет сможет подключиться к вашему виртуальному серверу 
  + порт гостя - на который будут перенаправлены подключения с этого порта
    - с 80 на 80 работать не будет (порт с которого вы перенаправляете и на который перенаправляете не должны совпадать)
    - с 8080 на 80 ок
    - то же самое можно сделать командой `VBoxManage modifyvm "Имя машины" --natpf1 "rulename,tcp,127.0.0.1,8080,,80"` (вместо Port Forwardign Rules)
* `ufw status` открыть порты на VM
* `service ssh start`

## Error
```
"Could not open the medium '/mnt/nfs/homes/akostrik/sgoinfre/Inception/Inception.vdi'.
NS_ERROR_FAILURE (0x80004005)
```
* install HxD Hex Editor to open .vdi file as streams of bytes (http://download.cnet.com/HxD-Hex-Editor ... tag=button)
* pre-header = the first 72 bytes (http://forums.virtualbox.org/viewtopic.php?t=52)
* create a new vm `test.vdi` with all default options
* open `test.vdi` and copy its first 72 bytes
* overwrite them in the invalid vdi file that you want to fix
* reboot your vm using the modified vdi

## Backup
   + сжать большие файлы
   + сохранить файлы где хочешь
   + на школьном маке: создать папку с таким же названием и по тому же пути в goinfre, скачать туда файлы, распаковать, запустить virtualbox (не менять конфигурацию в virtualbox)
   + на другом компе: скачать и разархивировать конфигурацию, virtualbox - Инструменты - зелёный плюсик "Добавить", указав папку с файлом Debian.vbox и прочими файлами


https://www.nakivo.com/blog/virtualbox-network-setting-guide/  
