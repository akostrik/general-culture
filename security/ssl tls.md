## SSL
* l’authentification le chiffrement des données qui transitent entre des serveurs
* initialement développé par Netscape 
* Secure Socket Layers (SSL) предшественник TSL, TLS = SSL moderne
  + SSL 1.0 was never publicly released
  + SSL 2.0, SSLv2
    - 1995
    - la découverte de plusieurs vulnérabilités en 1996
  + SSL 3.0, SSLv3
    - insecure

### Transport Layer Security (TLS) 
* 1999
* a cryptographic protocol
* provides end-to-end security of data sent between applications over the Internet, encrypts data sent over the Internet
* to ensure that eavesdroppers and hackers are unable to see what you transmit
* usage
  + secure web browsing
    - the padlock icon that appears in web browsers when a secure session is established
  + e-mail
  + file transfers
  + video/audioconferencing
  + instant messaging
  + voice-over-IP
  + Internet services such as DNS and NTP
  + l'authentification du serveur
  + la confidentialité des données échangées (ou session chiffrée)
  + l'intégrité des données échangées
  + de manière optionnelle, l'authentification du client (mais dans la réalité celle-ci est souvent assurée par la couche applicative)
  + клиент-серверные приложения осуществлять связь: нельзя производить прослушивание пакетов и осуществить несанкционированный доступ
* SSL предшественник TSL
  + au fil du temps, de nouvelles versions de ces protocoles ont vu le jour pour faire face aux vulnérabilités et prendre en charge des suites et des algorithmes de chiffrement toujours plus forts, toujours plus sécurisés 
* does not secure data on end systems
  + ensures the secure delivery of data over the Internet
  + avoides eavesdropping and/or alteration of the content
* is implemented on top of TCP 
  + encrypts Application Layer protocols (HTTP, FTP, SMTP, IMAP)
  + can be implemented on UDP, DCCP, SCTP (for VPN, SIP-based application uses, ...), this is known as Datagram Transport Layer Security (DTLS)
* асимметричное шифрование для аутентификации
* симметричное шифрование для конфиденциальности
* коды аутентичности сообщений для сохранения целостности сообщений
* большинство протоколов связи может быть использовано как с TLS (SSL), так и без них => при установке соединения необходимо явно указать серверу, хочет ли клиент устанавливать TLS
  + либо с помощью использования унифицированного номера порта, по которому соединение всегда устанавливается с использованием TLS (443 для HTTPS)
  + либо с использованием произвольного порта и специальной команды серверу со стороны клиента на переключение соединения на TLS
* как только клиент и сервер договорились об использовании TLS, им необходимо установить защищённое соединение
  + с помощью процедуры подтверждения связи
  + клиент и сервер принимают соглашение относительно различных параметров
* un site sans TLS c'est pas "normal" en 2021, qu'il soit statique ou pas ça change rien du tout
  + мы оставляем порт 80 для браузеров, которые часто ведут себя некорректно
  + aujourd'hui c'est juste obligatoire pour le SEO
  + il faut garder le port 80 ouvert parce que ça permet de rediriger les gens vers l'https, y'a des services des fois qui au clic d'un lien envoient vers du http, du coup c'est mieux d'avoir une redirection qu'une erreur
  + Best Practice - Keep Port 80 Open https://letsencrypt.org/docs/allow-port-80/
  + tant que les services autorisent encore le port 80, mieux vaut l'ouvrir et rediriger sur le 443 (si possible avec une 301, pas un rewrite)
* TLS change vos URL de http en https

## цифровой сертификат
* электронный или печатный документ, подтверждающий принадлежность владельцу открытого ключа или каких-либо атрибутов
* выпущен удостоверяющим центром
* самозаверенный сертификат
  + подписан самим его субъектом
  + технически не отличается от сертификата, заверенного центром
  + пользователь создаёт свою собственную сигнатуру
  + все корневые сертификаты доверенных УЦ являются самозаверенными
  + невозможно отозвать
  + позволяет атаку посредника: злоумышленник перехватывает сертификат узла-инициатора шифрованного соединения и вместо него отправляет узлу назначения свой поддельный, с помощью которого передаваемые данные можно дешифровать
* SSL certificat
  + для безопасной передачи информации в Интернете и интранете
  + https соединение между Вашим сайтом и браузером пользователя шифрует передаваемую информацию
  + самозаверенный SSL сертификат
    - сертификат открытого ключа
    - для своего домена или IP-адреса создали 
    - для внутреннего пользования: частными лицами или в малых фирмах, люди добавляют сами сертификат в списки браузера
    - внешний посетитель при подколючении к каналу видит «Сертификат безопасности не является доверенным»
    - бесплатный
  + официально заверенный SSL сертификат
    - в зависимости от типа SSL сертификата проверка разных уровней – от одного домена и до полной проверки документации юридического лица
    - центр сертификации гарантирует уровень доверия к серверу, информация о домене была проверена независимым доверительным источником
    - печать защиты (Trust logo или Site Seal)
    - отображается в браузере и в самом SSL сертификате: при расширенной проверке, соединением через https, зеленая адресная строка
    - SSL rajoute un cadenas vert quand vous accédez à un site sécurisé
    - если пользователь попал на фишинговый сайт с действительным SSL сертификатом (сертификат был ошибочно выдан по вине удостоверяющего центра) и понес убытки, центр сертификации гарантирует компенсацию от 10 000 до 1 500 000 долларов
    - Ex: PositiveSSL от Comodo

