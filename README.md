| Instruction           | Description |
|-----------------------|-------------|
| `FROM php:8.2-apache` | Uses the official Docker image that comes with **PHP 8.2** and the **Apache** web server pre-installed as the base image for the application. |
| `RUN apt-get update` | Updates the Debian package index inside the container before installing additional system packages. |
| `RUN apt-get install -y ...` | Install the packages required by the Laravel application, such as Git, Curl, ZIP utilities, and PHP library dependencies. |
| `RUN docker-php-ext-install pdo_mysql mbstring zip exif pcntl` | Compiles and enables the required PHP extensions used by Laravel, including MySQL support, multibyte string handling, ZIP support, EXIF metadata, and process control. |
| `RUN a2enmod rewrite` | Enables the Apache **mod_rewrite** module, allowing Laravel to use clean URLs and route requests through the `public/.htaccess` file. |
| `WORKDIR /var/www/html` | Sets `/var/www/html` as the default working directory for all subsequent Dockerfile instructions and commands executed inside the container. |
| `COPY app /var/www/html` | Copies the Laravel application source code into the image. |
| `COPY ./apache/vhost.conf /etc/apache2/sites-available/000-default.conf` | Replace Apache default virtual host configuration with a custom configuration that serves the Laravel application from the `public` directory. |
| `COPY --from=composer:2.7 /usr/bin/composer /usr/bin/composer` | Copies the Composer executable from the official Composer image into the container, allowing Composer commands to be executed without installing Composer manually. |

| Apache | Description |
|--------|-------------|
| `DocumentRoot /var/www/html/public` | Sets Laravel `public` directory as the web server document root, ensuring that only publicly accessible files are served to clients. |
| `<Directory /var/www/html/public> ... </Directory>` | Defines access permissions for the `public` directory, enables the use of `.htaccess` files through `AllowOverride All`, and allows all users to access the application with `Require all granted`. |


#### Services Part

| Service      | Description |
|--------------|-------------|
| `app`        | Builds the Laravel application image from the local `Dockerfile`, creates the `laravel-app-container` container, exposes port **80**, and connects it to the internal Docker network. |
| `mysql`      | Pulls the official MySQL 8.0 image from Docker Hub, creates the `laravel-mysql-container` container, initializes the database, and exposes port **3306**. |
| `phpmyadmin` | Pulls the official PHPMyAdmin image from Docker Hub, creates the `laravel-pma-container` container, connects it to the MySQL service, and exposes port **8080**. |

#### Network Part

| Network           | Description |
|-------------------|-------------|
| `laravel-network` | Creates a custom bridge network that allows the services to communicate using their service names (such as `mysql`) instead of IP addresses. |

#### Volume Part

| Volume       | Description |
|--------------|-------------|
| `mysql-data` | Creates a named Docker volume that persists MySQL database data outside the container, allowing the data to survive container removal or recreation. |
