# 🐳 Laravel + Docker — Guía práctica de Junior a Senior

> Guía rápida para trabajar con Laravel dentro de Docker, entender el flujo de desarrollo y producción y conocer los comandos más importantes.

---

# 1. 🧠 ¿QUÉ ES DOCKER?

Docker permite ejecutar una aplicación dentro de un **contenedor aislado**, con sus dependencias y configuración.

Sin Docker:

```text
Mi PC
 ├── PHP
 ├── MySQL
 ├── Nginx
 ├── Redis
 └── Node
```

El problema:

> "En mi computador funciona, pero en el servidor no."

Con Docker:

```text
Docker
 ├── Laravel / PHP
 ├── MySQL
 ├── Nginx
 ├── Redis
 └── Node
```

Cada servicio está separado.

---

# 2. 📦 CONCEPTOS DE DOCKER

## Image

Es como una **plantilla**.

Ejemplo:

```text
php:8.2
mysql:8.0
nginx:latest
```

---

## Container

Es una instancia funcionando de una imagen.

```text
Image
   ↓
Container
```

Ejemplo:

```text
mysql image
     ↓
mysql container
```

---

## Dockerfile

Define cómo construir una imagen.

Ejemplo:

```dockerfile
FROM php:8.2-fpm

RUN docker-php-ext-install pdo pdo_mysql

WORKDIR /var/www
```

---

## Docker Compose

Define varios servicios.

Ejemplo:

```yaml
services:

  app:
    build: .

  mysql:
    image: mysql:8.0

  nginx:
    image: nginx:latest
```

Conceptualmente:

```text
Docker Compose
      │
 ┌────┼─────┐
 ↓    ↓     ↓
PHP  MySQL Nginx
```

---

# 3. 🚀 LEVANTAR PROYECTO

Con Docker Compose:

```bash
docker compose up
```

En segundo plano:

```bash
docker compose up -d
```

Reconstruir imágenes:

```bash
docker compose up -d --build
```

### Diferencia

```text
up
↓
levanta contenedores

up -d
↓
levanta en segundo plano

up -d --build
↓
reconstruye imágenes y levanta
```

---

# 4. 🛑 DETENER DOCKER

```bash
docker compose down
```

Esto detiene y elimina los contenedores creados por Compose.

---

# 5. 🔄 REINICIAR

```bash
docker compose restart
```

Un servicio específico:

```bash
docker compose restart app
```

---

# 6. 📋 VER CONTENEDORES

Todos los contenedores activos:

```bash
docker ps
```

También detenidos:

```bash
docker ps -a
```

Con Compose:

```bash
docker compose ps
```

---

# 7. 📜 VER LOGS

Todos:

```bash
docker compose logs
```

En tiempo real:

```bash
docker compose logs -f
```

Solo Laravel/PHP:

```bash
docker compose logs -f app
```

MySQL:

```bash
docker compose logs -f mysql
```

Nginx:

```bash
docker compose logs -f nginx
```

### Senior

Cuando algo falla:

> **Primero mira los logs.**

---

# 8. 🖥️ ENTRAR A UN CONTENEDOR

```bash
docker compose exec app bash
```

Si no existe `bash`:

```bash
docker compose exec app sh
```

Ahora estás dentro del contenedor.

Puedes ejecutar:

```bash
php -v
composer --version
php artisan --version
```

---

# 9. ⚡ EJECUTAR ARTISAN DESDE HOST

No necesitas entrar al contenedor.

Puedes hacer:

```bash
docker compose exec app php artisan migrate
```

Tinker:

```bash
docker compose exec app php artisan tinker
```

Rutas:

```bash
docker compose exec app php artisan route:list
```

Limpiar caché:

```bash
docker compose exec app php artisan optimize:clear
```

### Regla

Si Laravel está dentro de Docker:

```text
docker compose exec app
```

antes del comando.

---

# 10. 📦 COMPOSER DENTRO DE DOCKER

Instalar dependencias:

```bash
docker compose exec app composer install
```

Actualizar:

```bash
docker compose exec app composer update
```

Agregar paquete:

```bash
docker compose exec app composer require paquete/nombre
```

Eliminar:

```bash
docker compose exec app composer remove paquete/nombre
```

---

# 11. 🟢 NPM / NODE

Instalar:

```bash
docker compose exec app npm install
```

Desarrollo:

```bash
docker compose exec app npm run dev
```

Producción:

```bash
docker compose exec app npm run build
```

### Diferencia

```text
npm run dev
↓
desarrollo

npm run build
↓
archivos optimizados para producción
```

---

# 12. 🗄️ MYSQL

Entrar al contenedor:

```bash
docker compose exec mysql bash
```

Si tiene cliente MySQL:

```bash
mysql -u root -p
```

O directamente:

```bash
docker compose exec mysql mysql -u root -p
```

---

# 13. 🔌 PUERTOS

Ejemplo:

```yaml
ports:
  - "8080:80"
```

Significa:

```text
PC
8080
 ↓
Docker
80
```

Otro ejemplo:

```yaml
ports:
  - "3307:3306"
```

Significa:

```text
Host: 3307
Container: 3306
```

### Importante

Dentro de Docker, normalmente Laravel no debe conectarse a:

```env
DB_HOST=localhost
```

Debe utilizar el nombre del servicio:

```env
DB_HOST=mysql
```

Porque:

```text
Laravel container
      ↓
mysql container
```

---

# 14. 🌐 RED DE DOCKER

Docker Compose crea normalmente una red interna.

Ejemplo:

```text
app
 │
 ├── mysql
 ├── redis
 └── nginx
```

Laravel puede comunicarse con:

```text
mysql
redis
```

utilizando esos nombres como hostname.

---

# 15. 💾 VOLUMES

Un volumen permite conservar datos.

Ejemplo:

```yaml
volumes:
  mysql_data:
```

Luego:

```yaml
services:
  mysql:
    volumes:
      - mysql_data:/var/lib/mysql
```

Sin volumen:

```text
Eliminar container
      ↓
Datos pueden perderse
```

Con volumen:

```text
Eliminar container
      ↓
Datos permanecen
```

---

# 16. 📁 BIND MOUNT

Puedes compartir código entre host y container:

```yaml
volumes:
  - ./:/var/www
```

Significa:

```text
Mi PC
./
 ↓
/var/www
 ↓
Container
```

Muy útil en desarrollo.

---

# 17. 🟢 DESARROLLO

En desarrollo normalmente quieres:

```text
Código local
     ↓
Docker
     ↓
Laravel
```

Características:

```text
APP_ENV=local
APP_DEBUG=true
```

Puedes tener:

* hot reload
* Xdebug
* herramientas de debugging
* logs detallados
* bases de datos locales
* volúmenes
* código montado

---

# 18. 🔴 PRODUCCIÓN

En producción buscas:

```text
Seguridad
Rendimiento
Estabilidad
Reproducibilidad
```

Normalmente:

```env
APP_ENV=production
APP_DEBUG=false
```

Nunca deberías dejar:

```env
APP_DEBUG=true
```

en producción.

---

# 19. 🆚 DESARROLLO VS PRODUCCIÓN

| Desarrollo          | Producción               |
| ------------------- | ------------------------ |
| `APP_ENV=local`     | `APP_ENV=production`     |
| `APP_DEBUG=true`    | `APP_DEBUG=false`        |
| Código montado      | Imagen construida        |
| Xdebug              | Sin Xdebug               |
| Logs detallados     | Logs controlados         |
| `npm run dev`       | `npm run build`          |
| Cambios frecuentes  | Cambios controlados      |
| Herramientas debug  | Seguridad                |
| DB local            | DB administrada/servidor |
| Volúmenes de código | Imágenes inmutables      |

---

# 20. 🏗️ DOCKER EN DESARROLLO

Una estructura común:

```text
project/
│
├── app/
├── bootstrap/
├── config/
├── database/
├── public/
├── resources/
├── routes/
├── storage/
│
├── Dockerfile
├── compose.yaml
├── composer.json
└── .env
```

---

# 21. 🏭 DOCKER EN PRODUCCIÓN

Una arquitectura típica:

```text
                    INTERNET
                       │
                       ↓
                    NGINX
                       │
                       ↓
                  PHP-FPM
                       │
              ┌────────┴────────┐
              ↓                 ↓
            Redis             MySQL
              │
              ↓
            Queue
              │
              ↓
            Worker
```

---

# 22. 🔥 MULTI-STAGE BUILD

Una buena práctica de Docker.

Ejemplo conceptual:

```dockerfile
FROM node:20 AS frontend

WORKDIR /app

COPY package*.json ./

RUN npm install

COPY . .

RUN npm run build


FROM php:8.2-fpm

COPY --from=frontend /app/public/build /var/www/public/build
```

La idea:

```text
Stage 1
Node
 ↓
Compila frontend

Stage 2
PHP
 ↓
Solo recibe resultado
```

Así reduces elementos innecesarios en la imagen final.

---

# 23. 🧹 LIMPIAR DOCKER

Ver imágenes:

```bash
docker images
```

Eliminar una imagen:

```bash
docker rmi IMAGE_ID
```

Ver volúmenes:

```bash
docker volume ls
```

Eliminar volumen:

```bash
docker volume rm VOLUME
```

Ver redes:

```bash
docker network ls
```

Limpiar recursos no utilizados:

```bash
docker system prune
```

⚠️ Cuidado con `prune`.

---

# 24. 🚨 DOCKER COMPOSE DOWN -V

```bash
docker compose down -v
```

Esto elimina:

```text
Containers
+
Networks
+
Volumes
```

⚠️ Si tu MySQL utiliza un volumen de Docker, puedes perder los datos almacenados allí.

No lo ejecutes sin saber qué volumen estás eliminando.

---

# 25. 🔑 VARIABLES DE ENTORNO

Laravel utiliza:

```text
.env
```

Ejemplo:

```env
APP_NAME=Laravel
APP_ENV=local
APP_DEBUG=true

DB_CONNECTION=mysql
DB_HOST=mysql
DB_PORT=3306
DB_DATABASE=laravel
DB_USERNAME=root
DB_PASSWORD=secret
```

### Desarrollo

```env
APP_ENV=local
APP_DEBUG=true
```

### Producción

```env
APP_ENV=production
APP_DEBUG=false
```

---

# 26. ⚡ CACHE DE CONFIGURACIÓN

En desarrollo:

```bash
php artisan optimize:clear
```

En producción:

```bash
php artisan config:cache
```

¿Por qué?

Laravel puede cargar la configuración de forma optimizada.

---

# 27. 🚀 DESPLIEGUE TÍPICO

Un flujo sencillo:

```text
Git
 ↓
Servidor
 ↓
Docker build
 ↓
Docker Compose
 ↓
Laravel
```

Después:

```bash
docker compose up -d --build
```

Luego:

```bash
docker compose exec app php artisan migrate --force
```

Y:

```bash
docker compose exec app php artisan optimize
```

---

# 28. 🧹 PERMISOS

Laravel necesita escribir normalmente en:

```text
storage/
bootstrap/cache/
```

En Linux puedes revisar:

```bash
ls -la
```

Y:

```bash
ls -la storage
```

Un problema frecuente:

```text
Permission denied
```

No lo soluciones dando permisos indiscriminadamente a todo el proyecto.

Revisa primero:

```text
Usuario
Grupo
Owner
Permisos
Container
Volume
```

---

# 29. 🔄 MIGRACIONES

Desarrollo:

```bash
docker compose exec app php artisan migrate
```

Producción:

```bash
docker compose exec app php artisan migrate --force
```

### ⚠️ Nunca hagas esto alegremente en producción:

```bash
php artisan migrate:fresh
```

Porque elimina las tablas.

---

# 30. 🧪 TINKER EN DOCKER

```bash
docker compose exec app php artisan tinker
```

Ejemplo:

```php
User::count();
```

```php
User::find(1);
```

```php
User::where('email', 'test@test.com')->first();
```

Es tu laboratorio para comprobar rápidamente Eloquent.

---

# 31. 🛣️ DEBUG DE RUTAS

```bash
docker compose exec app php artisan route:list
```

Filtrar:

```bash
docker compose exec app php artisan route:list --path=api
```

---

# 32. 🧹 CUANDO LARAVEL "NO RECONOCE" UN CAMBIO

Primero:

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

# 33. 📜 LOGS DE LARAVEL

Dentro del container:

```bash
docker compose exec app tail -f storage/logs/laravel.log
```

También:

```bash
docker compose logs -f app
```

### Diferencia

```text
Laravel log
↓
errores de Laravel

Docker logs
↓
salida del container/proceso
```

---

# 34. 🐇 QUEUES

Producción normalmente necesita un worker:

```bash
php artisan queue:work
```

En Docker:

```bash
docker compose exec app php artisan queue:work
```

Una arquitectura mejor:

```text
Laravel
   ↓
Redis
   ↓
Queue
   ↓
Worker
```

El worker puede ser un servicio separado.

---

# 35. ⏰ SCHEDULER

Laravel:

```bash
php artisan schedule:run
```

En producción normalmente se programa la ejecución periódica desde el sistema/infraestructura.

La idea:

```text
Cron
 ↓
Laravel Scheduler
 ↓
Tareas
```

---

# 36. 🔴 PRODUCCIÓN: NO HACER

❌ `APP_DEBUG=true`

❌ Guardar secretos en Git.

❌ Exponer MySQL públicamente sin necesidad.

❌ Ejecutar `migrate:fresh`.

❌ Utilizar `npm run dev`.

❌ Ejecutar Xdebug.

❌ Montar todo el código como volumen sin necesidad.

❌ Ejecutar procesos como `root` sin necesidad.

❌ No tener backups.

❌ No revisar logs.

❌ No controlar workers.

---

# 🟢 DESARROLLO: SÍ HACER

✅ Docker Compose.

✅ Volúmenes para código.

✅ Xdebug si lo necesitas.

✅ `APP_DEBUG=true`.

✅ Base de datos local.

✅ Hot reload.

✅ Tinker.

✅ Logs detallados.

✅ Factories y Seeders.

---

# 🔥 COMANDOS JUNIOR

## Docker

```bash
docker ps
docker compose up
docker compose up -d
docker compose down
docker compose ps
docker compose logs
docker compose logs -f
```

## Laravel

```bash
php artisan serve
php artisan migrate
php artisan db:seed
php artisan tinker
php artisan route:list
php artisan optimize:clear
```

## Dentro de Docker

```bash
docker compose exec app php artisan migrate
docker compose exec app php artisan tinker
docker compose exec app php artisan route:list
```

---

# 🟡 COMANDOS MID

## Docker

```bash
docker compose up -d --build
docker compose restart
docker compose exec app bash
docker compose logs -f app
docker compose logs -f mysql
docker inspect CONTAINER
docker stats
```

## Laravel

```bash
php artisan migrate:status
php artisan migrate:rollback
php artisan migrate:fresh --seed
php artisan queue:work
php artisan queue:restart
php artisan schedule:list
php artisan storage:link
```

---

# 🔴 COMANDOS SENIOR

## Docker

```bash
docker inspect
docker stats
docker network inspect
docker volume inspect
docker image inspect
docker system df
docker system prune
```

## Producción

```bash
docker compose up -d --build
docker compose ps
docker compose logs -f
docker compose exec app php artisan migrate --force
docker compose exec app php artisan optimize
docker compose exec app php artisan queue:restart
```

---

# 🧠 COMANDOS QUE DEBES MEMORIZAR

## Docker

```bash
docker ps
docker compose ps
docker compose up -d
docker compose down
docker compose logs -f
docker compose exec app bash
docker compose up -d --build
```

## Laravel

```bash
php artisan list
php artisan about
php artisan route:list
php artisan migrate
php artisan migrate:status
php artisan db:seed
php artisan tinker
php artisan optimize:clear
php artisan queue:work
php artisan queue:restart
```

---

# 🎯 MAPA MENTAL

```text
                 LARAVEL + DOCKER
                       │
             ┌─────────┴─────────┐
             ↓                   ↓
        DESARROLLO           PRODUCCIÓN
             │                   │
       APP_DEBUG=true      APP_DEBUG=false
             │                   │
       Volúmenes             Imágenes
             │                   │
       npm run dev          npm run build
             │                   │
       Xdebug opcional      Sin Xdebug
             │                   │
       DB local             DB protegida
             │                   │
             └─────────┬─────────┘
                       ↓
                   Docker
                       │
          ┌────────────┼────────────┐
          ↓            ↓            ↓
         PHP          Nginx        MySQL
          │
       Laravel
          │
     ┌────┼────┐
     ↓    ↓    ↓
   Redis Queue Worker
```

---

# 🏆 EVOLUCIÓN JUNIOR → SENIOR

## 🟢 JUNIOR

Debes saber:

```text
¿Qué es Docker?
¿Qué es una imagen?
¿Qué es un container?
¿Qué es Docker Compose?
¿Cómo levanto Laravel?
¿Cómo entro al container?
¿Cómo ejecuto Artisan?
¿Cómo veo logs?
¿Cómo ejecuto migraciones?
```

---

## 🟡 MID

Debes entender:

```text
Dockerfile
Compose
Networks
Volumes
Ports
Environment variables
PHP-FPM
Nginx
MySQL
Redis
Queues
Workers
Builds
Logs
```

Y saber solucionar:

```text
Puerto ocupado
Container caído
Error de conexión MySQL
Permisos
Variables .env
Problemas de volumen
Problemas de red
Errores de Composer
Errores de npm
```

---

## 🔴 SENIOR

Debes poder diseñar:

```text
Dockerfile optimizado
Multi-stage builds
Docker Compose
Redes
Volúmenes
Healthchecks
Secrets
Workers
Queues
Scheduler
Reverse Proxy
PHP-FPM
Nginx
Redis
MySQL
Logs
Backups
CI/CD
```

Y responder:

> ¿Por qué este container consume tanta memoria?

> ¿Por qué Laravel no puede conectarse a MySQL?

> ¿Por qué funciona en desarrollo pero no en producción?

> ¿Qué ocurre si el container se reinicia?

> ¿Dónde viven los datos?

> ¿Qué pasa si elimino el container?

> ¿Cómo hago rollback?

> ¿Cómo actualizo la aplicación sin dejarla caída?

> ¿Cómo escalo los workers?

> ¿Qué información debe estar en una imagen y cuál debe ser un secreto?

---

# 🚀 REGLA DE ORO

Piensa en Docker como **la infraestructura** y Laravel como **la aplicación**.

```text
DOCKER
│
├── PHP
├── Nginx
├── MySQL
├── Redis
└── Queue Worker

          ↓

LARAVEL
│
├── Routes
├── Controllers
├── Models
├── Eloquent
├── Services
├── Jobs
└── Views / API

          ↓

DATABASE
```

### Desarrollo

> **Quiero velocidad para programar y depurar.**

### Producción

> **Quiero estabilidad, seguridad, rendimiento y capacidad de recuperación.**

### Mentalidad Senior

> **No se trata solamente de saber levantar `docker compose up`. Se trata de entender qué ocurre dentro de cada container, cómo se comunican los servicios, dónde viven los datos y cómo mantener el sistema funcionando cuando algo falla.**
