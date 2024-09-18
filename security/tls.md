### Transport Layer Security (TLS) 
* a cryptographic protocol
* provides end-to-end security of data sent between applications over the Internet, encrypts data sent over the Internet
* to ensure that eavesdroppers and hackers are unable to see what you transmit
* used
  + secure web browsing
    - the padlock icon that appears in web browsers when a secure session is established
  + e-mail
  + file transfers
  + video/audioconferencing
  + instant messaging
  + voice-over-IP
  + Internet services such as DNS and NTP
* его предшественник Secure Socket Layers (SSL)
  + SSL 1.0 was never publicly released
  + SSL 2.0 was quickly replaced by SSL 3.0 on which TLS is based
  + SSL 3.0 is insecure
* TLS 1.2 should be used
* TLS 1.3 is under development and will drop support for less secure algorithms (?)
* does not secure data on end systems
  + ensures the secure delivery of data over the Internet
  + avoides  eavesdropping and/or alteration of the content
* is implemented on top of TCP 
  + to encrypt Application Layer protocols such as HTTP, FTP, SMTP, IMAP
  + can be implemented on UDP, DCCP, SCTP (e.g. for VPN and SIP-based application uses), this is known as Datagram Transport Layer Security (DTLS)
* асимметричное шифрование для аутентификации
* симметричное шифрование для конфиденциальности
* коды аутентичности сообщений для сохранения целостности сообщений
* даёт возможность клиент-серверным приложениям осуществлять связь в сети таким образом, что нельзя производить прослушивание пакетов и осуществить несанкционированный доступ
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
