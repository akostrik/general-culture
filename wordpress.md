### WordPress
* приложение
* написанно на PHP, работает на PHP
* взаимодействует с бд
* генерирует динамический контент для веб-страниц
* выполняет задачи на стороне сервера (генерация HTML, работа с бд,управление контентом)
* PHP (Hypertext Preprocessor) — серверный язык программирования
  +  используется для создания динамических веб-страниц
* как работает
  + Nginx принимает запросы от пользователей и передают в PHP 
  + PHP-скрипты обрабаьывают запросы (отображение постов, страниц, комментариев)
  + PHP выполняет процессы FastCGI с помощью php-fpm
  + PHP взаимодействует с бд, получает/обновляет данные
  + бд хранит контент сайта (посты, страницы, настройки, ...)

### WP-CLI
* the command line interface for WordPress
* allows to interact with your WordPress site from the command line
* is used for automating tasks, debugging problems, installing/removing plugins along side with themes, managing users and roles, exporting/importing data, run databses queries, ...
* can save time that will take you to installing a pluging/theme manually, moderate users and their roles, deploy a new WordPress website to a production server, ...
* helps you react with your WordPress website
