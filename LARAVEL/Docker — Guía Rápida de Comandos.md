# 🐳 Docker — Guía Rápida de Comandos

## 🟢 1. VER QUÉ ESTÁ EJECUTÁNDOSE

### Contenedores activos

```bash
docker ps
```

### Todos los contenedores

```bash
docker ps -a
```

### Imágenes

```bash
docker images
```

### Volúmenes

```bash
docker volume ls
```

### Redes

```bash
docker network ls
```

---

# 🚀 2. LEVANTAR / ACTIVAR

## Levantar un contenedor

```bash
docker start NOMBRE_CONTENEDOR
```

Ejemplo:

```bash
docker start mysql
```

---

## Levantar Docker Compose

Dentro del proyecto:

```bash
docker compose up
```

En segundo plano:

```bash
docker compose up -d
```

### Recomendada

```bash
docker compose up -d
```

---

## Levantar y reconstruir

```bash
docker compose up -d --build
```

Útil después de modificar:

```text
Dockerfile
docker-compose.yml
dependencias
extensiones PHP
configuración
```

---

# 🛑 3. PARAR / DETENER

## Detener un contenedor

```bash
docker stop NOMBRE_CONTENEDOR
```

Ejemplo:

```bash
docker stop mysql
```

---

## Detener todos los contenedores activos

```bash
docker stop $(docker ps -q)
```

---

## Detener Docker Compose

```bash
docker compose stop
```

> `stop` detiene los contenedores pero los conserva.

---

# 🔄 4. REINICIAR

## Reiniciar un contenedor

```bash
docker restart NOMBRE_CONTENEDOR
```

Ejemplo:

```bash
docker restart mysql
```

---

## Reiniciar Docker Compose

```bash
docker compose restart
```

---

# 🔽 5. BAJAR EL PROYECTO

## Docker Compose

```bash
docker compose down
```

Esto detiene y elimina los contenedores creados por Compose.

**No elimina normalmente:**

```text
Imágenes
Volúmenes
Código
```

---

## Bajar y eliminar contenedores huérfanos

```bash
docker compose down --remove-orphans
```

Muy útil cuando modificaste el `docker-compose.yml`.

---

# 🗑️ 6. ELIMINAR UN CONTENEDOR

```bash
docker rm NOMBRE_CONTENEDOR
```

Si está ejecutándose:

```bash
docker rm -f NOMBRE_CONTENEDOR
```

---

# 💣 7. ELIMINAR TODOS LOS CONTENEDORES

```bash
docker rm -f $(docker ps -aq)
```

⚠️ Esto elimina **todos los contenedores de Docker**, no solamente los del proyecto actual.

---

# 🖼️ 8. ELIMINAR UNA IMAGEN

```bash
docker rmi NOMBRE_IMAGEN
```

Ejemplo:

```bash
docker rmi mysql:8.0
```

Forzar:

```bash
docker rmi -f NOMBRE_IMAGEN
```

---

# 🧹 9. ELIMINAR IMÁGENES NO UTILIZADAS

```bash
docker image prune
```

Eliminar todas las imágenes no utilizadas:

```bash
docker image prune -a
```

⚠️ Tendrás que volver a descargar/reconstruir algunas imágenes cuando las necesites.

---

# 💾 10. ELIMINAR VOLÚMENES

Ver:

```bash
docker volume ls
```

Eliminar uno:

```bash
docker volume rm NOMBRE_VOLUMEN
```

Eliminar volúmenes no utilizados:

```bash
docker volume prune
```

⚠️ **Mucho cuidado con MySQL/MariaDB.**

Un volumen puede contener tu base de datos.

---

# 🌐 11. ELIMINAR REDES

Ver:

```bash
docker network ls
```

Eliminar una:

```bash
docker network rm NOMBRE_RED
```

Eliminar redes no utilizadas:

```bash
docker network prune
```

---

# 🧹 12. LIMPIAR CONTENEDORES DETENIDOS

```bash
docker container prune
```

---

# 💥 13. LIMPIAR DOCKER

```bash
docker system prune
```

Elimina recursos no utilizados.

Más agresivo:

```bash
docker system prune -a
```

---

# ☢️ 14. LIMPIAR TODO INCLUYENDO VOLÚMENES

```bash
docker system prune -a --volumes
```

🚨 **PELIGRO**

Puede eliminar datos de:

```text
MySQL
MariaDB
PostgreSQL
Redis
etc.
```

No lo ejecutes si tienes información que quieras conservar.

---

# 🐳 15. CICLO NORMAL DE UN PROYECTO

## Primera vez

```bash
docker compose up -d --build
```

---

## Trabajar

```bash
docker compose ps
```

```bash
docker compose logs -f
```

---

## Detener temporalmente

```bash
docker compose stop
```

---

## Volver a activar

```bash
docker compose start
```

---

## Reiniciar

```bash
docker compose restart
```

---

## Bajar completamente

```bash
docker compose down
```

---

## Volver a levantar

```bash
docker compose up -d
```

---

# 🚑 16. SI ALGO ESTÁ FALLANDO

Prueba en este orden:

```bash
docker compose down
```

```bash
docker compose up -d
```

Si continúa:

```bash
docker compose down --remove-orphans
```

```bash
docker compose up -d --build
```

Si continúa:

```bash
docker compose logs -f
```

---

# 🔌 17. SI DICE "PORT IS ALREADY ALLOCATED"

Primero:

```bash
docker ps
```

Buscar un puerto específico:

```bash
docker ps --filter "publish=3307"
```

Por ejemplo:

```bash
docker ps --filter "publish=8080"
```

Después detén el contenedor que está ocupando el puerto:

```bash
docker stop NOMBRE_CONTENEDOR
```

Y levanta nuevamente:

```bash
docker compose up -d
```

---

# 🧠 18. DIFERENCIAS IMPORTANTES

| Comando                        | Qué hace                                 |
| ------------------------------ | ---------------------------------------- |
| `docker start`                 | Activa un contenedor existente           |
| `docker stop`                  | Detiene un contenedor                    |
| `docker restart`               | Detiene y vuelve a iniciar               |
| `docker rm`                    | Elimina un contenedor                    |
| `docker compose up`            | Crea/levanta servicios                   |
| `docker compose stop`          | Detiene servicios                        |
| `docker compose start`         | Activa servicios existentes              |
| `docker compose restart`       | Reinicia servicios                       |
| `docker compose down`          | Baja y elimina contenedores del proyecto |
| `docker compose up -d --build` | Reconstruye y levanta                    |
| `docker rmi`                   | Elimina una imagen                       |
| `docker volume rm`             | Elimina un volumen                       |
| `docker system prune`          | Limpia recursos no utilizados            |

---

# ⭐ 19. LOS COMANDOS QUE DEBES MEMORIZAR

### 👀 Ver

```bash
docker ps
docker ps -a
```

### 🟢 Levantar

```bash
docker compose up -d
```

### 🏗️ Reconstruir + levantar

```bash
docker compose up -d --build
```

### 🛑 Parar

```bash
docker compose stop
```

### 🔽 Bajar

```bash
docker compose down
```

### 🔄 Reiniciar

```bash
docker compose restart
```

### 🧹 Limpiar huérfanos

```bash
docker compose down --remove-orphans
```

### 📜 Ver errores

```bash
docker compose logs -f
```

### 🗑️ Eliminar contenedor

```bash
docker rm -f NOMBRE
```

### 💣 Eliminar todos

```bash
docker rm -f $(docker ps -aq)
```

---

# 🏆 20. MAPA MENTAL

```text
                    DOCKER
                       │
        ┌──────────────┼──────────────┐
        ↓              ↓              ↓
      VER            ACCIONES       ELIMINAR
        │              │              │
    docker ps       start            rm
    docker ps -a    stop             rmi
                   restart           volume rm
                       │
                       ↓
                 DOCKER COMPOSE
                       │
        ┌──────────────┼──────────────┐
        ↓              ↓              ↓
       SUBIR          PARAR          BAJAR
        │              │              │
     up -d           stop           down
        │
        ↓
   RECONSTRUIR
        │
    up -d --build
```

---

# 🚨 CHULETA DE EMERGENCIA

Si estás desarrollando Laravel y Docker se queda "loco":

```bash
docker compose down --remove-orphans
docker compose up -d --build
```

Si solo quieres apagar:

```bash
docker compose stop
```

Si quieres volver a encender:

```bash
docker compose start
```

Si quieres bajar completamente el proyecto:

```bash
docker compose down
```

Si quieres **borrar absolutamente todos los contenedores**:

```bash
docker rm -f $(docker ps -aq)
```

Si quieres hacer una limpieza profunda:

```bash
docker system prune -a
```

Y **solo si estás 100% seguro de que no necesitas los datos de ningún volumen**:

```bash
docker system prune -a --volumes
```

> 💡 **Regla práctica:** `stop` = pausar, `start` = continuar, `restart` = reiniciar, `down` = bajar el proyecto, `rm` = borrar contenedor, `rmi` = borrar imagen, `prune` = limpiar basura.
