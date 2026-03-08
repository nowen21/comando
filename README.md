# Comandos más usados en desarrollo

almacenar los comandos más utilizados en herramientas

# 🐳 Docker - Guía de Uso del Proyecto

Este proyecto se ejecuta utilizando Docker y Docker Compose, lo que permite levantar el entorno completo de desarrollo de forma reproducible.

El contenedor principal ejecuta Laravel dentro de un contenedor PHP y monta el código fuente desde el host.

## 📦 Requisitos

Antes de iniciar, asegúrese de tener instalado:

- Docker
- Docker Compose

Verificar instalación:
```bash
docker --version
docker compose version
```

## 🚀 Levantar el Proyecto

Para construir y levantar los contenedores del proyecto:
```bash
docker compose up -d
```
Explicación
| Comando          | Descripción                                                   |
| ---------------- | ------------------------------------------------------------- |
| `docker compose` | Ejecuta Docker Compose                                        |
| `up`             | Crea y levanta los contenedores                               |
| `-d`             | Ejecuta los contenedores en **modo detached (segundo plano)** |


Esto iniciará todos los servicios definidos en el archivo:
```bash
docker-compose.yml
```
## 🔍 Ver Contenedores Activos
```bash
docker ps
```
Muestra los contenedores que están corriendo.

Para ver todos los contenedores (incluyendo detenidos):
```bash
docker ps -a
```
## 📜 Ver Logs del Proyecto

Para ver los logs de Laravel dentro del contenedor:
```bash
docker exec -it <container> -f storage/logs/laravel.log
```
Explicación
| Comando       | Descripción                                |
| ------------- | ------------------------------------------ |
| `docker exec` | Ejecuta un comando dentro de un contenedor |
| `-it`         | Modo interactivo                           |
| `tail -f`     | Muestra el log en tiempo real              |

Esto permite diagnosticar errores del backend Laravel.

## 🖥️ Entrar al Contenedor

Para abrir una terminal dentro del contenedor:
```bash
docker exec -it <container> sh
```
Ahora estarás dentro del contenedor:
```bash
/var/www/html
```
Desde allí puedes ejecutar comandos de Laravel.

Ejemplo:
```bash
php artisan migrate
```
## 🔧 Reiniciar Contenedores

Si se requiere reiniciar los servicios:
```bash
docker compose restart
```
## 🛑 Detener el Proyecto

Para detener los contenedores:
```bash
docker compose down
```

Esto detiene y elimina los contenedores creados por Docker Compose.

## 🔨 Reconstruir Contenedores

Si se realizan cambios en el Dockerfile:
```bash
docker compose up --build -d
```
Esto fuerza la reconstrucción de las imágenes.

## 📁 Manejo de Permisos (Laravel)

En algunos casos Laravel puede presentar errores de permisos en:
```bash
storage
bootstrap/cache
```
Solución dentro del contenedor:
```bash
chmod -R 775 storage
chmod -R 775 bootstrap/cache
```
## 🧹 Limpiar Contenedores y Volúmenes

Para eliminar contenedores, redes y volúmenes asociados:
```bash
docker compose down -v
```
## 📊 Comandos Docker Más Utilizados
Comando	Descripción
```bash
docker ps	Lista contenedores activos
docker ps -a	Lista todos los contenedores
docker images	Lista imágenes Docker
docker exec -it <container> sh	Entrar al contenedor
docker logs <container>	Ver logs del contenedor
docker stop <container>	Detener contenedor
docker rm <container>	Eliminar contenedor
docker rmi <image>	Eliminar imagen
```
## 🏗 Arquitectura del Entorno Docker

El entorno Docker del proyecto sigue esta estructura:
```bash
Host (WSL / Windows)
        │
        │
Docker Engine
        │
        │
Container
   ├── PHP
   ├── Laravel
   ├── Composer
   └── Código del proyecto
```

El código se monta mediante volúmenes, lo que permite modificar archivos en el host y ver los cambios dentro del contenedor.

## 📌 Buenas Prácticas

- ✔ Usar Docker para evitar diferencias de entorno
- ✔ Ejecutar siempre el proyecto desde docker compose
- ✔ Revisar logs de Laravel cuando ocurra un error
- ✔ Mantener permisos correctos en storage y bootstrap/cache

## Table of Contents

1. [Prerequisites](#prerequisites)
2. Docker Installation
   - [2.1 Linux (.deb)](#docker-installation-linux)
   - [2.2 Windows (WSL2)](#docker-installation-windows)
   - [2.3 macOS](#docker-installation-macos)
3. [Cloning the Project](#cloning-the-project)
4. [Environment Setup](#environment-setup)
5. [Running the Containers](#running-the-containers)
6. [Application Setup](#application-setup)
7. [Makefile Reference](#makefile-reference)
8. [Testing](#testing)
9. [License](#license)

---

<a id="prerequisites"></a>

## 1. Prerequisites

Before you begin, make sure you have the following tools installed:

* [Git](https://git-scm.com/)
* [Docker](https://docs.docker.com/)
* [Docker Compose](https://docs.docker.com/compose/) (v2 plugin — `docker compose`, not `docker-compose`)
* `make` (available by default on Linux and macOS)

---

<a id="docker-installation-linux"></a>

## 2. Docker Installation

### 2.1 Linux

```bash
curl -fsSL https://get.docker.com -o get-docker.sh
sudo sh get-docker.sh
```

#### Setup Permissions

```bash
sudo groupadd docker
sudo usermod -aG docker $USER
newgrp docker
```

> **Note:** Log out and back in (or use `newgrp docker`) for the changes to take effect.

#### Enable Docker at Boot

```bash
sudo systemctl enable docker
sudo systemctl start docker
```

#### Verify Installation

```bash
docker --version
docker compose version
docker ps
```

---

<a id="docker-installation-windows"></a>

### 2.2 Windows with WSL2 (without Docker Desktop)

> Remove Docker Desktop if installed, as it can conflict with the WSL2 Docker socket.

#### 2.2.1 WSL2 Settings

```bash
cat /etc/wsl.conf
```

Verify that `systemd=true` is set:

```
[boot]
systemd=true
```

If not, add it and restart WSL:

```bash
sudo apt-get update -y && sudo apt-get install systemd systemd-sysv -y
```

#### 2.2.2 Install Docker

```bash
curl -fsSL https://get.docker.com -o get-docker.sh
sudo sh get-docker.sh
```

#### 2.2.3 Setup Permissions

```bash
sudo groupadd docker
sudo usermod -aG docker $USER
newgrp docker
```

#### 2.2.4 Install Docker Compose Plugin

```bash
sudo apt-get update
sudo apt-get install docker-compose-plugin
```

#### 2.2.5 Verify Installation

```bash
docker --version
docker compose version
docker ps
```

#### 2.2.6 Enable at Boot

```bash
sudo systemctl enable docker.service
sudo systemctl enable containerd.service
```

---

<a id="docker-installation-macos"></a>

### 2.3 macOS

Download and install Docker Desktop: https://www.docker.com/products/docker-desktop/

#### Verify Installation

```bash
docker --version
docker compose version
```

---

<a id="cloning-the-project"></a>

## 3. Cloning the Project

```bash
git clone <repository-url>
cd tayana-siies-backend
```

---

<a id="environment-setup"></a>

## 4. Environment Setup

### 4.1 Create the `.env` File

```bash
cp .env.example .env
```

### 4.2 Configure the Environment

```ini
APP_ENV=local

DB_CONNECTION=pgsql
DB_HOST=tayana_siies_pgsql
DB_PORT=5432
DB_DATABASE=siies
DB_USERNAME=postgres
DB_PASSWORD=secret

REDIS_HOST=tayana_siies_redis
REDIS_PORT=6379

TAYANA_API_URL=https://api-sandbox.tayana.com
TAYANA_API_KEY=your_api_key_here
```

---

<a id="running-the-containers"></a>

## 5. Running the Containers

This project uses a `Makefile` to orchestrate Docker. The active environment is read from `APP_ENV` in `.env` and can always be overridden per command.

### 5.1 Local Development (first time)

```bash
make up BUILD=1
```

This builds the images and starts the containers. Expected output:

```
docker compose ... up -d
[+] Running 4/4
 ✔ Network ...   Created
 ✔ Container tayana_siies_pgsql   Healthy
 ✔ Container tayana_siies_redis   Healthy
 ✔ Container tayana_siies_app     Healthy
 ✔ Container tayana_siies_nginx   Started
```

### 5.2 Subsequent Starts

```bash
make up
```

---

<a id="application-setup"></a>

## 6. Application Setup

### 6.1 Generate Application Key

```bash
make artisan key:generate
```

### 6.2 Run Migrations

```bash
make artisan migrate
```

### 6.3 Run Seeders (optional)

```bash
make artisan db:seed
```

### 6.4 Verify the Application is Running

```bash
curl http://localhost:8082/up
```

Expected response: `200 OK` with "Application up".

---

<a id="makefile-reference"></a>

## 7. Makefile Reference

The `Makefile` replaces the `docker.sh` script. It auto-detects the environment from `APP_ENV` in `.env`.

### Syntax

```bash
make <target>
```

### Available Targets

| Command | Description |
|---|---|
| `make build` | Build Docker images |
| `make up` | Start containers in background |
| `make up BUILD=1` | Build images then start containers |
| `make down` | Stop and remove containers |
| `make restart` | Restart all services |
| `make logs` | Tail logs from all services |
| `make logs SERVICE=app` | Tail logs from a specific service |
| `make ps` | List running containers and their status |
| `make shell` | Open a shell in the `app` container |
| `make shell SERVICE=nginx` | Open a shell in another container |
| `make artisan <command>` | Run an Artisan command |
| `make help` | Show all available targets |

### Artisan Examples

```bash
make artisan migrate
make artisan migrate --seed
make artisan migrate:rollback
make artisan key:generate
make artisan tinker
make artisan make:model Invoice -m
make artisan route:list
make artisan queue:work
make artisan cache:clear
```

### Examples

```bash
make up BUILD=1
make logs SERVICE=app
make artisan migrate
```

---

<a id="testing"></a>

## 8. Testing

### 9.1 Prepare the Testing Environment

```bash
cp .env .env.testing
# Set APP_ENV=testing and configure a test database in .env.testing
```

### 9.2 Run Tests

```bash
APP_ENV=testing make up BUILD=1
make artisan test
```

### 9.3 Run a Specific Test

```bash
make artisan "test --filter=InvoiceTest"
```

### 9.4 Directory Structure

```
tests/
├── Unit/
│   └── Modules/
│       └── ...
└── Feature/
    └── ...
```

> The folder structure under `tests/` mirrors the path of the files being tested.


<a id="license"></a>

## 9. License

This project is proprietary and developed for internal use by the **Servicio Nacional de Aprendizaje (Colombia)**.
It may not be shared, modified, or distributed without explicit written permission from the agency.
