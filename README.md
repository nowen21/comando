# comando
almacenar los comandos más utilizados en herramientas

🐳 Comandos Docker más utilizados
1. Gestión de contenedores
Ver contenedores activos
docker ps

Muestra contenedores que están corriendo.

Ver todos los contenedores (incluyendo detenidos)
docker ps -a
Iniciar un contenedor
docker start CONTAINER_ID
Detener un contenedor
docker stop CONTAINER_ID
Reiniciar un contenedor
docker restart CONTAINER_ID
Eliminar un contenedor
docker rm CONTAINER_ID
Eliminar todos los contenedores detenidos
docker container prune
2. Ejecutar comandos dentro de un contenedor
Entrar al contenedor
docker exec -it CONTAINER_NAME sh

o si tiene bash:

docker exec -it CONTAINER_NAME bash

Ejemplo:

docker exec -it tayana-siies-backend-main-app-1 sh
Ejecutar un comando dentro del contenedor
docker exec CONTAINER_NAME COMMAND

Ejemplo:

docker exec -it app php artisan route:list
3. Logs
Ver logs de un contenedor
docker logs CONTAINER_NAME
Ver logs en tiempo real
docker logs -f CONTAINER_NAME

Ejemplo:

docker logs -f tayana-siies-backend-main-app-1
4. Gestión de imágenes
Ver imágenes instaladas
docker images
Descargar imagen
docker pull IMAGE_NAME

Ejemplo:

docker pull nginx
Construir imagen
docker build -t IMAGE_NAME .
Eliminar imagen
docker rmi IMAGE_ID
5. Docker Compose (muy usado en Laravel)
Levantar contenedores
docker compose up
Levantar en segundo plano
docker compose up -d
Reconstruir contenedores
docker compose up -d --build
Detener contenedores
docker compose down
Ver configuración final del compose
docker compose config
Ver contenedores del compose
docker compose ps
6. Redes Docker
Ver redes
docker network ls
Inspeccionar red
docker network inspect NETWORK_NAME
7. Volúmenes Docker
Ver volúmenes
docker volume ls
Inspeccionar volumen
docker volume inspect VOLUME_NAME
Eliminar volúmenes no utilizados
docker volume prune
8. Limpieza general de Docker
Eliminar recursos no usados
docker system prune
Limpieza completa
docker system prune -a
9. Información del sistema Docker
Ver información del sistema
docker info
Ver versión de Docker
docker version
10. Comandos muy usados en Laravel + Docker
Ejecutar artisan
docker exec -it app php artisan migrate
Ver rutas
docker exec -it app php artisan route:list
Limpiar cache
docker exec -it app php artisan optimize:clear
Instalar dependencias
docker exec -it app composer install
🚀 Flujo típico de desarrollo con Docker
docker compose up -d
docker ps
docker exec -it app sh
php artisan migrate
php artisan route:list
docker logs -f app
📚 Conclusión

Los comandos más utilizados en proyectos Docker son:

docker ps
docker exec
docker logs
docker compose up
docker compose down
docker compose up -d --build
docker images
docker network ls
docker volume ls
docker system prune
