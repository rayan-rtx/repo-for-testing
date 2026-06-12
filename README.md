# Dockerized Laravel App

![Laravel](https://img.shields.io/badge/laravel-%23FF2D20.svg?style=for-the-badge&logo=laravel&logoColor=white)
![PHPMyAdmin](https://img.shields.io/badge/phpMyAdmin-6C78AF.svg?style=for-the-badge&logo=phpMyAdmin&logoColor=white)
![Apache](https://img.shields.io/badge/apache-%23D42029.svg?style=for-the-badge&logo=apache&logoColor=white)
![Docker](https://img.shields.io/badge/docker-%230db7ed.svg?style=for-the-badge&logo=docker&logoColor=white)

lorem lorem lorem lorem lorem lorem lorem lorem lorem lorem.

## Overview

The `dockerzied-laravel-app` repository is ... implements a the complete steps for running a _Laravel_ application inside _Docker_ using :

- Apache.
- Docker.
- Docker Compose.

## Project demonstrates

## Requirements

## Workflow explained

⩩ ***Docker*** can be installed locally on your machine or on a virtual machine ( in our case, we are using a virtual machine running _Ubuntu_ ) :

```bash
sudo apt update
sudo apt upgrade -y
sudo apt install -y docker.io
```

⩩ We check that Docker is installed :

```bash
sudo docker version
```

- this output if Docker installed successfully :
```text
Client:
Version:           27.5.1
API version:       1.47
Go version:        go1.22.2
Git commit:        27.5.1-0ubuntu3~24.04.2
Built:             Mon Jun  2 11:51:53 2025
OS/Arch:           linux/amd64
Context:           default

Server:
Engine:
Version:          27.5.1
API version:      1.47 (minimum version 1.24)
Go version:       go1.22.2
Git commit:       27.5.1-0ubuntu3~24.04.2
Built:            Mon Jun  2 11:51:53 2025
OS/Arch:          linux/amd64
Experimental:     false
containerd:
Version:          1.7.27
GitCommit:
runc:
Version:          1.2.5-0ubuntu1~24.04.1
GitCommit:
docker-init:
Version:          0.19.0
GitCommit:
```

⩩ We apply this command to gain permissions to use Docker without `sudo` :

```bash
sudo usermod -aG docker $USER
```

⩩ Next, we exit the virtual machine to apply the changes :

```bash
exit
```

⩩ We log back into the virtual machine and pull the _laravel-app_ project we will be working on :

```bash
git clone https://github.com/rayanguendouz/dockerized-laravel-app.git laravel-app
cd laravel-app
```

⩩ We check for the presence of the _Dockerfile_ in _laravel-app/_ folder :

```bash
cat Dockerfile
```

- the output returned _Dockerfile_ content :

```Dockerfile
FROM php:8.2-apache

# Install system dependencies
RUN apt-get update && apt-get install -y \
libpng-dev \
libonig-dev \
libxml2-dev \
zip \
unzip \
git \
curl \
libzip-dev \
&& docker-php-ext-install pdo_mysql mbstring zip exif pcntl

# Enable Apache mod_rewrite
RUN a2enmod rewrite

# Set working directory
WORKDIR /var/www/html

# Copy app files
COPY . .

# Copy existing apache config
COPY ./apache/vhost.conf /etc/apache2/sites-available/000-default.conf

# Install Composer
COPY --from=composer:2.7 /usr/bin/composer /usr/bin/composer
```

- Here's a description of the contents of this file :

`FOR php:8.2-apache` indicates that the platform we'll be working on contains PHP 8.2 and Apache.

`RUN apt-get update` updates the package list in the Debian system within the container we'll create.

`RUN apt-get install -y ... && RUN docker-php-ext-install ...` installs the necessary PHP libraries and plugins.

`RUN a2enmod rewrite` enables mod_rewrite mode in Apache.

`WORKDIR /var/www/html` specifies the working directory within our platform.

`COPY . .` copies all project files ( located next to the current Dockerfile ) into the container at the path _/var/www/html_. The command _COPY ./apache/vhost.conf /etc/apache2/sites-available/000-default.conf_ copies your custom Apache configuration file _apache/vhost.conf_ to the location of the default configuration file _etc/apache2/sites-available/000-default.conf_.

`COPY --from=composer:2.7 /usr/bin/composer /usr/bin/composer` installs _Composer_ from an image named _composer:2.7_ on the platform so that we can run the _Composer_ install.

⩩ We check for the presence of the _vhost.conf_ in _laravel-app/apache_ folder :

```bash
cat apache/vhost.conf
```

- the output returned _vhost.conf_ content :

```conf
<VirtualHost *:80>
ServerAdmin webmaster@localhost
DocumentRoot /var/www/html/public

<Directory /var/www/html/public>
  AllowOverride All
  Require all granted
</Directory>

ErrorLog ${APACHE_LOG_DIR}/error.log
CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
```

- Here's a description of the contents of this file :

`DocumentRoot /var/www/html/public` enables Apache to read contents from the _public/_ directory.

`<Directory /var/www/html/public>...</Directory>` enables Apache to read _.htaccess_ file and allows all users to access the _public_ directory.

⩩ We build the first image of the project, which we call _laravel-app_ :

```bash
docker build -t laravel-app .
```

- this output if _laravel-app_ image created successfully :

```text
laravel-app
```

⩩ We are check that _laravel-app_ image was created :

```bash
docker images
```

- The returned output is a list of all _Docker_ images I have installed ( with some information such as Image id and Image size ) :

```text
SITORY        TAG          IMAGE ID       CREATED         SIZE
laravel-app   latest       60aedb7bff0d   3 minutes ago   629MB
php           8.3-apache   fa6c27da5746   2 weeks ago     506MB
composer      2.7          e401e0c0405c   10 months ago   189MB
```

⩩ We create and run a container named _laravel-app-container_, starting from image _laravel-app_ image :

```bash
docker run -d -p 80:80 --name laravel-app-container laravel-app
```

- this output if _laravel-app-container_ container created successfully :

```text
d686a083a66a9f9ad3691f23ee3f345e13d7f34057f6a9ef69b59c1edf70182f
```

⩩ We are check that _laravel-app-container_ container was created :

```bash
docker ps
```

- The returned output is a list of _Docker_ active containers ( with some information such as Container ID ) :

```text
CONTAINER ID   IMAGE         COMMAND                  CREATED         STATUS         PORTS                               NAMES
d686a083a66a   laravel-app   "docker-php-entrypoi…"   9 seconds ago   Up 8 seconds   0.0.0.0:80->80/tcp, :::80->80/tcp   laravel-app-container
```

⩩ We access _laravel-app-container_ container to complete the project dependency installation process :

```bash
docker exec -it laravel-app-container bash
```

- The returned output means that we are inside _laravel-app-container_ container and any command we execute is within it :

```text
root@5fb0581c4d98:/var/www/html#
```

⩩ Inside the laravel-app-container, we will execute the following commands ( It can be copied and pasted all at once ) :

```bash
echo "🔐 Setting permissions for /var/www/html"
chown -R www-data:www-data /var/www/html
chmod -R 775 /var/www/html

echo "📄 Copying .env.example to .env..."
cp .env.example .env

echo "📦 Running composer install..."
composer install --no-interaction --prefer-dist --optimize-autoloader

echo "🔑 Generating app key..."
php artisan key:generate

echo "🧹 Clearing and caching config and routes..."
php artisan config:clear
php artisan config:cache
php artisan route:clear
php artisan route:cache

echo "🗄️ Running migrations..."
php artisan migrate --force
```

We can now access the container we running :

```text
http://<server-ip>
```

- If you are working on your device locally :

```text
http://localhost
```

## Docker common commands

## Docker Compose plugin

> [!WARNING]
> Urgent info that needs immediate user attention to avoid problems.

## Concepts you should know

## Docker container sharing

## CI / CD section

## Contributing

We welcome contributions! Please follow these guidelines :

- Fork the repository and create a new branch for your feature or fix.
- Write clear commit messages and document your code.
- Ensure all tests pass before submitting a pull request.
- Follow the established code style and project structure.
- Open an issue for discussion before major changes.

## License

This project is open-sourced under the [MIT License](LICENSE).

---

Thank you for using `dockerized-laravel-app`! For questions or support, please open an issue on GitHub.
