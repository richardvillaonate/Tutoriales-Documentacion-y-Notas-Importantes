# 🐳 Docker — Comandos de Emergencia

Guía rápida para solucionar problemas comunes de Docker durante el desarrollo de proyectos Laravel, Node.js, MySQL, phpMyAdmin, etc.

---

# 📌 1. Ver contenedores

## Ver contenedores activos

```bash
docker ps
```

## Ver todos los contenedores

```bash
docker ps -a
```

---

# 📌 2. Ver imágenes

```bash
docker images
```

También:

```bash
docker image ls
```

---

# 🛑 3. Detener un contenedor

```bash
docker stop NOMBRE_O_ID
```

Ejemplo:

```bash
docker stop mysql
```

---

# 🛑 4. Detener TODOS los contenedores activos

```bash
docker stop $(docker ps -q)
```

> ⚠️ Detiene todos los contenedores que estén ejecutándose.

---

# 💀 5. Forzar la detención de TODOS

```bash
docker kill $(docker ps -q)
```

Usarlo solamente cuando `docker stop` no funciona.

### Diferencia

```text
docker stop
    ↓
Detención normal

docker kill
    ↓
Detención inmediata
```

---

# 🗑️ 6. Eliminar un contenedor

```bash
docker rm NOMBRE_O_ID
```

Si está ejecutándose:

```bash
docker rm -f NOMBRE_O_ID
```

---

# 💣 7. Eliminar TODOS los contenedores

```bash
docker rm -f $(docker ps -aq)
```

> ⚠️ Esto elimina todos los contenedores de Docker de tu PC.

Las imágenes y volúmenes no se eliminan.

---

# 🧹 8. Eliminar contenedores detenidos

```bash
docker container prune
```

Docker solicitará confirmación.

---

# 🖼️ 9. Eliminar una imagen

```bash
docker rmi NOMBRE_O_ID
```

Ejemplo:

```bash
docker rmi mysql:8.0
```

Forzar:

```bash
docker rmi -f mysql:8.0
```

---

# 🧹 10. Eliminar imágenes no utilizadas

```bash
docker image prune
```

Eliminar todas las imágenes que no estén siendo utilizadas:

```bash
docker image prune -a
```

> ⚠️ `-a` elimina más imágenes y puede obligarte a descargarlas/reconstruirlas nuevamente.

---

# 🔥 11. Limpieza general

```bash
docker system prune
```

Limpieza más agresiva:

```bash
docker system prune -a
```

---

# ☢️ 12. Limpieza TOTAL

```bash
docker system prune -a --volumes
```

> 🚨 **PELIGRO**
>
> Puede eliminar volúmenes Docker que contienen bases de datos.
>
> **NO ejecutar si tienes MySQL/MariaDB con información que quieras conservar.**

---

# 🔌 13. Problema: puerto ocupado

Error típico:

```text
Bind for 0.0.0.0:3307 failed:
port is already allocated
```

Primero:

```bash
docker ps
```

Busca:

```text
0.0.0.0:3307->3306/tcp
```

---

# 🔎 14. Buscar quién utiliza un puerto

Por ejemplo, puerto `3307`:

```bash
docker ps --filter "publish=3307"
```

Puerto `8090`:

```bash
docker ps --filter "publish=8090"
```

Puerto `8080`:

```bash
docker ps --filter "publish=8080"
```

---

# 🪟 15. Buscar puerto ocupado en Windows

Desde CMD:

```cmd
netstat -ano | findstr :3307
```

Ejemplo:

```text
TCP    0.0.0.0:3307
       0.0.0.0:0
       LISTENING
       1234
```

El último número es el PID:

```text
1234
```

Buscar el proceso:

```cmd
tasklist | findstr 1234
```

---

# 🐳 16. Docker Compose — iniciar proyecto

Dentro del proyecto:

```bash
docker compose up -d
```

---

# 🛑 17. Docker Compose — detener proyecto

```bash
docker compose down
```

Esto elimina los contenedores creados por ese `docker-compose.yml`, pero normalmente conserva imágenes y volúmenes.

---

# 🧹 18. Docker Compose — eliminar huérfanos

```bash
docker compose down --remove-orphans
```

Muy útil cuando cambiaste servicios del `docker-compose.yml`.

---

# 🔄 19. Reiniciar proyecto

```bash
docker compose restart
```

---

# 🏗️ 20. Reconstruir imágenes

```bash
docker compose build
```

Reconstruir ignorando caché:

```bash
docker compose build --no-cache
```

---

# 🚀 21. Reconstruir y levantar

```bash
docker compose up -d --build
```

Esta combinación es muy útil después de modificar:

```text
Dockerfile
docker-compose.yml
PHP
Extensiones
Dependencias
Configuración
```

---

# 💥 22. Reinicio fuerte de un proyecto

```bash
docker compose down --remove-orphans
docker compose up -d --build
```

Úsalo cuando el proyecto presenta comportamientos extraños.

---

# 📜 23. Ver logs

Contenedor específico:

```bash
docker logs NOMBRE
```

Logs en tiempo real:

```bash
docker logs -f NOMBRE
```

---

# 📜 24. Logs de Docker Compose

Todos los servicios:

```bash
docker compose logs -f
```

Un servicio:

```bash
docker compose logs -f app
```

MySQL:

```bash
docker compose logs -f mysql
```

phpMyAdmin:

```bash
docker compose logs -f phpmyadmin
```

---

# 🔍 25. Inspeccionar un contenedor

```bash
docker inspect NOMBRE_O_ID
```

Permite revisar información como:

```text
IP
Puertos
Redes
Volúmenes
Variables
Configuración
```

---

# 🖥️ 26. Entrar a un contenedor

Si tiene Bash:

```bash
docker exec -it NOMBRE bash
```

Si no tiene Bash:

```bash
docker exec -it NOMBRE sh
```

---

# 🐘 27. Entrar al contenedor de Laravel

Si el servicio se llama `app`:

```bash
docker compose exec app bash
```

Después puedes ejecutar:

```bash
php artisan migrate
```

```bash
php artisan tinker
```

```bash
php artisan route:list
```

```bash
php artisan optimize:clear
```

---

# 🐘 28. Ejecutar Artisan sin entrar al contenedor

```bash
docker compose exec app php artisan migrate
```

Tinker:

```bash
docker compose exec app php artisan tinker
```

Limpiar Laravel:

```bash
docker compose exec app php artisan optimize:clear
```

Ver rutas:

```bash
docker compose exec app php artisan route:list
```

---

# 📦 29. Ejecutar Composer dentro del contenedor

```bash
docker compose exec app composer install
```

Actualizar:

```bash
docker compose exec app composer update
```

Instalar paquete:

```bash
docker compose exec app composer require laravel/sanctum
```

---

# 📦 30. Ejecutar NPM dentro del contenedor

```bash
docker compose exec app npm install
```

Ejecutar desarrollo:

```bash
docker compose exec app npm run dev
```

Build:

```bash
docker compose exec app npm run build
```

---

# 💾 31. Ver volúmenes

```bash
docker volume ls
```

---

# ⚠️ 32. Eliminar volúmenes no utilizados

```bash
docker volume prune
```

> 🚨 Cuidado: los volúmenes pueden contener bases de datos.

---

# 🌐 33. Ver redes

```bash
docker network ls
```

---

# 🧹 34. Eliminar redes no utilizadas

```bash
docker network prune
```

---

# 🩺 35. Ver estado de Docker

```bash
docker info
```

Versión:

```bash
docker version
```

---

# 🚑 PROCEDIMIENTOS DE EMERGENCIA

## 🚨 Caso 1 — Puerto ocupado

Error:

```text
port is already allocated
```

Ejecuta:

```bash
docker ps
```

Después:

```bash
docker ps --filter "publish=3307"
```

Identifica el contenedor:

```bash
docker stop NOMBRE
```

Después:

```bash
docker compose up -d
```

---

# 🚨 Caso 2 — Hay otro Laravel ejecutándose

Ver:

```bash
docker ps
```

Detener el proyecto actual:

```bash
docker compose down
```

Si hay contenedores antiguos:

```bash
docker compose down --remove-orphans
```

Después:

```bash
docker compose up -d --build
```

---

# 🚨 Caso 3 — Docker está lleno

Primero revisa:

```bash
docker system df
```

Después puedes limpiar:

```bash
docker system prune
```

Más agresivo:

```bash
docker system prune -a
```

> ⚠️ Evita `--volumes` si tienes bases de datos importantes.

---

# 🚨 Caso 4 — Laravel no refleja cambios

Ejecuta:

```bash
docker compose exec app php artisan optimize:clear
```

Después:

```bash
docker compose restart
```

Si modificaste Dockerfile:

```bash
docker compose up -d --build
```

---

# 🚨 Caso 5 — Cambiaste Dockerfile

```bash
docker compose down
docker compose build --no-cache
docker compose up -d
```

---

# 🚨 Caso 6 — Quiero reiniciar completamente MI proyecto

```bash
docker compose down --remove-orphans
docker compose up -d --build
```

> Esta es una de las secuencias más útiles para desarrollo.

---

# ☢️ CASO EXTREMO — LIMPIAR TODO DOCKER

Si estás completamente seguro de que no necesitas los contenedores actuales:

```bash
docker rm -f $(docker ps -aq)
```

Después:

```bash
docker system prune -a
```

### ⚠️ NO hagas esto automáticamente:

```bash
docker system prune -a --volumes
```

Especialmente si tienes:

```text
MySQL
MariaDB
PostgreSQL
Redis
```

porque puedes eliminar datos almacenados en volúmenes.

---

# 🧠 ORDEN RECOMENDADO PARA SOLUCIONAR ERRORES

Cuando algo falla, **no borres todo inmediatamente**.

Sigue este orden:

```text
1. docker ps
       ↓
2. docker ps -a
       ↓
3. Revisar logs
       ↓
4. Revisar puertos
       ↓
5. docker compose down
       ↓
6. docker compose down --remove-orphans
       ↓
7. docker compose up -d
       ↓
8. docker compose up -d --build
       ↓
9. Limpiar recursos no utilizados
       ↓
10. Limpieza agresiva SOLO si es necesario
```

---

# 🏆 CHULETA FINAL

## 👀 VER

```bash
docker ps
docker ps -a
docker images
docker volume ls
docker network ls
docker system df
```

## 🛑 DETENER

```bash
docker stop CONTAINER
docker stop $(docker ps -q)
docker kill $(docker ps -q)
```

## 🗑️ ELIMINAR CONTENEDORES

```bash
docker rm CONTAINER
docker rm -f CONTAINER
docker rm -f $(docker ps -aq)
```

## 🖼️ IMÁGENES

```bash
docker images
docker rmi IMAGE
docker image prune
docker image prune -a
```

## 🧹 LIMPIAR

```bash
docker container prune
docker network prune
docker system prune
docker system prune -a
```

## ☢️ MUY PELIGROSO

```bash
docker system prune -a --volumes
```

## 🐳 COMPOSE

```bash
docker compose up -d
docker compose down
docker compose down --remove-orphans
docker compose restart
docker compose up -d --build
```

## 📜 LOGS

```bash
docker logs -f CONTAINER
docker compose logs -f
```

## 🔌 PUERTOS

```bash
docker ps --filter "publish=3307"
docker ps --filter "publish=8080"
docker ps --filter "publish=8090"
```

## 🐘 LARAVEL

```bash
docker compose exec app php artisan migrate
docker compose exec app php artisan tinker
docker compose exec app php artisan route:list
docker compose exec app php artisan optimize:clear
```

---

# ⭐ LOS 10 QUE DEBES MEMORIZAR

```bash
docker ps
```

```bash
docker ps -a
```

```bash
docker stop CONTAINER
```

```bash
docker rm -f CONTAINER
```

```bash
docker compose up -d
```

```bash
docker compose down
```

```bash
docker compose down --remove-orphans
```

```bash
docker compose up -d --build
```

```bash
docker compose logs -f
```

```bash
docker ps --filter "publish=PUERTO"
```

---

# 🧠 REGLA DE ORO

> **Primero identifica → después detén → después reinicia → finalmente elimina.**

No empieces con:

```bash
docker system prune -a --volumes
```

Empieza con:

```bash
docker ps
```

y descubre **qué está ocupando el recurso que necesitas**.
