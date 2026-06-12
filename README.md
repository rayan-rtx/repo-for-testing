# Dockerized Laravel App

![Laravel](https://img.shields.io/badge/laravel-%23FF2D20.svg?style=for-the-badge&logo=laravel&logoColor=white)
![PHPMyAdmin](https://img.shields.io/badge/phpMyAdmin-6C78AF.svg?style=for-the-badge&logo=phpMyAdmin&logoColor=white)
![Apache](https://img.shields.io/badge/apache-%23D42029.svg?style=for-the-badge&logo=apache&logoColor=white)
![Docker](https://img.shields.io/badge/docker-%230db7ed.svg?style=for-the-badge&logo=docker&logoColor=white)

lorem lorem lorem lorem lorem lorem lorem lorem lorem lorem.

## Overview

The `dockerzied-laravel-app` repository is ... implements a the complete steps for running a `Laravel` application inside `Docker` using :

- Apache.
- Docker.
- Docker Compose.

## Project demonstrates

## Requirements

## Workflow explained
```
git status
git add
git commit
```

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

this result example :

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

- We apply this command to gain permissions to use Docker without `sudo` :

   ```text
   ubuntu@ip-172-31-34-198:~$ sudo usermod -aG docker $USER
   ```

- Next, we exit the virtual machine to apply the changes :

   ```text
   ubuntu@ip-172-31-34-198:~$ exit
   ```

## Docker commands

## Docker Compose plugin

> [!WARNING]
> Urgent info that needs immediate user attention to avoid problems.

## Concepts you should know

## Container sharing

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
