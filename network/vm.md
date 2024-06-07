## Backup
   + сжать большие файлы
   + сохранить файлы где хочешь
   + на школьном маке: создать папку с таким же названием и по тому же пути в goinfre, скачать туда файлы, распаковать, запустить virtualbox (не менять конфигурацию в virtualbox)
   + на другом компе: скачать и разархивировать конфигурацию, virtualbox - Инструменты - зелёный плюсик "Добавить", указав папку с файлом Debian.vbox и прочими файлами

## Выход в интернет или локальную сеть  

### По умолчанию 
* **NAT** = Network Address Translation = трансляция сетевых адресов = IP Masquerading = Network Masquerading = Native Address Translation
  + https://github.com/privet100/general-culture/blob/main/network/network_vpn_ports_etc.md#nat--network-address-translation--ip-masquerading--network-masquerading--native-address-translation--%D1%82%D1%80%D0%B0%D0%BD%D1%81%D0%BB%D1%8F%D1%86%D0%B8%D1%8F-%D1%81%D0%B5%D1%82%D0%B5%D0%B2%D1%8B%D1%85-%D0%B0%D0%B4%D1%80%D0%B5%D1%81%D0%BE%D0%B2 
* только доступ в интернет с VM
* VirtualBox Host-Only Enthernet Adapter
* VM автоматически получает доступ в интернет при помощи NAT
* первая сетевая карта NAT принадлежит локальной сети 10.0.2.0
* `ip addr` узнать IP
  + `ifconfig` depreciated 
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

### Доступ к VM из хостовой и из инета по http, https
* **Bridged** (вместо NAT)
* нет внешнего белого ip
* виртаулку видно всем из сети
* хост система может посылать и принимать данные от гостевой
* на виртуалке нет интернета
* подключение VM к физической сети TCP/IP в качестве отдельного компа
  + как будто VM подключается к физической сети
* Virtualbox использует райвер устройства (физический сетевой адаптер, физический интерфейс хоста, маршрутизатор или шлюз между гостевой системой и вашей физической сетью, net filter) на хост системе, который
  + обрабатывает данные проходящие через физический сетевой интерфейс
  + перехватывает VirtualBox пакеты из физической сети и изменяет данные в них
  + создаёт новые программные сетевые интерфейсы
* диагностика
  + Сбросить правила для iptables iptables -F и iptables -X
  + Посмотреть слушает ли костыль нужный порт и нужный адрес: ss -tunap|grep 5000
  + снять дамп на виртуальной машине: tcpdump -ni any port 5000 из которого будет видно, доходят ли запросы и что с ними происходит
  + дебажить костыль с помощью strace (Скорее всего его надо будет установить): strace flask/bin/python run.py в котором будет отчётливо видно, что происходит в системе при вызове костыля, разумеется, если вызов до него доходит
  + дальше по ситуации
* диагностика
  + netstat -tulpn в консоли и смотреть кто слушает порт 5000
* `wget https://localhost/index.html --no-check-certificat` проверить без браузера

### Доступ к VM только из хостовой по http, https
* **Виртуальный адаптер хоста** (вместо NAT)
* по умолчанию адрес VM из сети 192.168.56.0/24, хосту 192.168.56.1
* можно выдать VM отдельный IP
* можно обращаться к VM по IP

### Доступ к хостовой из VM
* **Bridged**
* VM подключается к основной сети как полноценное устройство
* используется сетевая карта хоста
* `http://localhost:3000/` = `http://myHostname:3000/`
  + `hostname` to know your hostname

## Error
```
"Could not open the medium '/mnt/nfs/homes/akostrik/sgoinfre/Inception/Inception.vdi'.
VDI: error reading pre-header in '/mnt/nfs/homes/akostrik/sgoinfre/Inception/Inception.vdi' (VERR_IS_A_DIRECTORY).
VD: error VERR_VD_VDI_INVALID_HEADER opening image file '/mnt/nfs/homes/akostrik/sgoinfre/Inception/Inception.vdi' (VERR_VD_VDI_INVALID_HEADER).
Result Code: 
NS_ERROR_FAILURE (0x80004005)
Component: 
MediumWrap
Interface: 
IMedium {ad47ad09-787b-44ab-b343-a082a3f2dfb1}" 
```
* Install HxD Hex Editor to open .vdi file as streams of bytes (http://download.cnet.com/HxD-Hex-Editor ... tag=button)
* pre-header = the first 72 bytes (http://forums.virtualbox.org/viewtopic.php?t=52)
* create a new vm `test.vdi` with all default options
* Open `test.vdi` and copy its first 72 bytes
* overwrite them in the invalid vdi file that you want to fix
* reboot your vm using the modified vdi
