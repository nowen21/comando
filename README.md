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
### 🧹 Ver errores en pantalla

[Ver documentación completa en la Wiki](https://github.com/nowen21/comando/wiki)
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
