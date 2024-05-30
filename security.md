## Transport Layer Security (TLS) 
* a cryptographic protocol
* encrypts data sent over the Internet
* to ensure that eavesdroppers and hackers are unable to see what you transmit
* provides end-to-end security of data sent between applications over the Internet
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
