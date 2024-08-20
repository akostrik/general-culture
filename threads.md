## Процесс или поток
   + самодостаточный набор инструкций, который операционная система может запланировать для выполнения на ядре процессора
   + с точки зрения ОС это одно и то же, разница лишь в разделении адресного пространства
   + На 1 ядре одновременно может находиться только 1 процесс или поток. Процессы на ядре постоянно подменяют друг друга, из-за чего страдает производительность, когда процессов много. Это камень преткновения для web-приложений, которые открывают на каждое соединение свой поток

## PID 1 process
* In a Docker container
  + the init process = the first process that is started when the system boots up
  + is responsible for starting and stoping all of the other processes on the system
  + is responsible for starting and stoping the application that is running in the container
* PID 1 in a Docker container behaves differently from the init process in a normal Unix-based system (they are NOT the same)
