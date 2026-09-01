# Laravel Artisan — Comandos útiles

> `php artisan` es la herramienta de consola de Laravel para administrar, desarrollar y depurar aplicaciones.

---

# 🟢 1. COMANDOS GENERALES

## Ver todos los comandos

```bash
php artisan list
```

## Ayuda de un comando

```bash
php artisan help migrate
```

También:

```bash
php artisan migrate --help
```

## Ver versión de Laravel

```bash
php artisan --version
```

## Ver entorno

```bash
php artisan about
```

---

# 🟢 2. DESARROLLO

## Levantar servidor

```bash
php artisan serve
```

Cambiar puerto:

```bash
php artisan serve --port=8080
```

## Limpiar cachés

```bash
php artisan optimize:clear
```

Este es uno de los comandos que más debes recordar.

Limpia diferentes cachés de Laravel.

---

# 🟢 3. CONFIGURACIÓN Y CACHÉ

## Limpiar configuración

```bash
php artisan config:clear
```

## Crear caché de configuración

```bash
php artisan config:cache
```

## Limpiar caché de aplicación

```bash
php artisan cache:clear
```

## Limpiar rutas

```bash
php artisan route:clear
```

## Crear caché de rutas

```bash
php artisan route:cache
```

## Limpiar vistas compiladas

```bash
php artisan view:clear
```

## Limpiar eventos

```bash
php artisan event:clear
```

## Optimizar aplicación

```bash
php artisan optimize
```

---

# 🟢 4. RUTAS

## Ver todas las rutas

```bash
php artisan route:list
```

## Mostrar middleware

```bash
php artisan route:list -v
```

## Filtrar rutas

```bash
php artisan route:list --path=api
```

Por nombre:

```bash
php artisan route:list --name=users
```

Por método:

```bash
php artisan route:list --method=GET
```

### Consejo

Cuando una ruta Laravel no funciona:

```bash
php artisan route:list
```

debe ser uno de tus primeros comandos.

---

# 🟢 5. MIGRACIONES

## Crear migración

```bash
php artisan make:migration create_productos_table
```

## Ejecutar migraciones

```bash
php artisan migrate
```

## Ver estado

```bash
php artisan migrate:status
```

## Retroceder última migración

```bash
php artisan migrate:rollback
```

## Retroceder varias migraciones

```bash
php artisan migrate:rollback --step=2
```

## Rehacer migraciones

```bash
php artisan migrate:refresh
```

## Rehacer y ejecutar seeders

```bash
php artisan migrate:refresh --seed
```

## Eliminar todas las tablas y migrar nuevamente

```bash
php artisan migrate:fresh
```

## Fresh + seed

```bash
php artisan migrate:fresh --seed
```

⚠️ `migrate:fresh` elimina las tablas existentes.

Úsalo con mucho cuidado en producción.

---

# 🟢 6. MODELOS

## Crear modelo

```bash
php artisan make:model Producto
```

## Modelo + migración

```bash
php artisan make:model Producto -m
```

## Modelo + migración + controlador

```bash
php artisan make:model Producto -mc
```

## Modelo + migración + controlador + factory + seeder

```bash
php artisan make:model Producto -mfsc
```

Una forma muy útil:

```bash
php artisan make:model Producto -mfsc
```

Genera:

```text
Model
Migration
Factory
Seeder
Controller
```

---

# 🟡 7. CONTROLADORES

## Crear controlador

```bash
php artisan make:controller ProductoController
```

## Controlador Resource

```bash
php artisan make:controller ProductoController --resource
```

También:

```bash
php artisan make:controller ProductoController -r
```

Genera:

```text
index
create
store
show
edit
update
destroy
```

## Controlador API

```bash
php artisan make:controller ProductoController --api
```

## Controlador con modelo

```bash
php artisan make:controller ProductoController --model=Producto
```

---

# 🟡 8. REQUESTS

## Crear Form Request

```bash
php artisan make:request StoreProductoRequest
```

Ejemplo:

```text
app/Http/Requests/StoreProductoRequest.php
```

---

# 🟡 9. MIDDLEWARE

## Crear middleware

```bash
php artisan make:middleware CheckAdmin
```

Ejemplo:

```text
app/Http/Middleware/CheckAdmin.php
```

---

# 🟡 10. POLICIES

## Crear Policy

```bash
php artisan make:policy ProductoPolicy
```

Con modelo:

```bash
php artisan make:policy ProductoPolicy --model=Producto
```

---

# 🟡 11. SEEDERS

## Crear Seeder

```bash
php artisan make:seeder ProductoSeeder
```

## Ejecutar seeders

```bash
php artisan db:seed
```

Seeder específico:

```bash
php artisan db:seed --class=ProductoSeeder
```

## Migrar + seed

```bash
php artisan migrate --seed
```

---

# 🟡 12. FACTORIES

## Crear Factory

```bash
php artisan make:factory ProductoFactory
```

Ejemplo desde Tinker:

```php
Producto::factory()->count(100)->create();
```

---

# 🟡 13. TINKER

## Abrir Tinker

```bash
php artisan tinker
```

Ejemplo:

```php
$user = User::find(1);
```

Crear:

```php
User::create([
    'name' => 'Richard',
    'email' => 'test@example.com',
]);
```

Contar:

```php
User::count();
```

Buscar:

```php
User::where('email', 'test@example.com')->first();
```

Salir:

```text
exit
```

---

# 🟡 14. VISTAS / BLADE

Laravel no necesita un comando para crear una vista.

Normalmente:

```text
resources/views/
```

Ejemplo:

```text
resources/views/productos/index.blade.php
```

---

# 🟡 15. COMPONENTES BLADE

## Crear componente

```bash
php artisan make:component Alert
```

Con vista anónima:

```bash
php artisan make:component Alert --view
```

---

# 🟡 16. LIVEWIRE

Si utilizas Livewire:

```bash
php artisan make:livewire ProductoTable
```

Dependiendo de la versión/configuración también puedes encontrar:

```bash
php artisan livewire:make ProductoTable
```

Para confirmar los comandos disponibles:

```bash
php artisan list
```

---

# 🟡 17. EVENTOS

## Crear evento

```bash
php artisan make:event PedidoCreado
```

## Crear listener

```bash
php artisan make:listener EnviarNotificacionPedido
```

---

# 🟡 18. OBSERVERS

## Crear Observer

```bash
php artisan make:observer ProductoObserver
```

Con modelo:

```bash
php artisan make:observer ProductoObserver --model=Producto
```

---

# 🟡 19. JOBS / COLAS

## Crear Job

```bash
php artisan make:job ProcesarPedido
```

## Ejecutar worker

```bash
php artisan queue:work
```

## Worker con límite de jobs

```bash
php artisan queue:work --max-jobs=100
```

## Procesar una cantidad determinada

```bash
php artisan queue:work --stop-when-empty
```

## Reiniciar workers

```bash
php artisan queue:restart
```

---

# 🟠 20. SCHEDULER

## Ejecutar scheduler manualmente

```bash
php artisan schedule:run
```

## Ver tareas programadas

```bash
php artisan schedule:list
```

---

# 🟠 21. STORAGE

## Crear enlace simbólico

```bash
php artisan storage:link
```

Muy importante cuando utilizas:

```text
storage/app/public
```

y quieres acceder mediante:

```text
/public/storage
```

---

# 🟠 22. NOTIFICACIONES

## Crear Notification

```bash
php artisan make:notification PedidoCreado
```

---

# 🟠 23. MAIL

## Crear Mailable

```bash
php artisan make:mail PedidoCreado
```

Con Markdown:

```bash
php artisan make:mail PedidoCreado --markdown=emails.pedido
```

---

# 🟠 24. COMANDOS PERSONALIZADOS

Puedes crear tus propios comandos Artisan.

```bash
php artisan make:command ProcesarPedidos
```

Después:

```bash
php artisan pedidos:procesar
```

Esto es muy útil para:

* procesos administrativos
* importaciones
* migraciones de datos
* tareas automáticas
* mantenimiento

---

# 🔴 25. CACHE

## Cachear valores

Normalmente se utiliza desde PHP:

```php
Cache::put('usuarios', $usuarios, 3600);
```

Limpiar cache:

```bash
php artisan cache:clear
```

---

# 🔴 26. LOGS

Laravel guarda normalmente logs en:

```text
storage/logs/
```

Puedes revisar:

```text
storage/logs/laravel.log
```

Para seguirlos en Linux:

```bash
tail -f storage/logs/laravel.log
```

En Docker:

```bash
docker compose logs -f
```

---

# 🔴 27. DOCKER + LARAVEL SAIL

Si utilizas Laravel Sail:

```bash
./vendor/bin/sail up
```

En segundo plano:

```bash
./vendor/bin/sail up -d
```

Detener:

```bash
./vendor/bin/sail down
```

Ejecutar Artisan:

```bash
./vendor/bin/sail artisan migrate
```

Tinker:

```bash
./vendor/bin/sail artisan tinker
```

Composer:

```bash
./vendor/bin/sail composer install
```

NPM:

```bash
./vendor/bin/sail npm install
```

---

# 🔥 28. COMANDOS PARA DEBUG

## Ver rutas

```bash
php artisan route:list
```

## Limpiar todo

```bash
php artisan optimize:clear
```

## Ver configuración

```bash
php artisan about
```

## Entrar a Tinker

```bash
php artisan tinker
```

## Revisar migraciones

```bash
php artisan migrate:status
```

## Revisar versión

```bash
php artisan --version
```

---

# 🔥 29. COMANDOS DE PRODUCCIÓN

Antes de desplegar:

```bash
php artisan config:cache
```

```bash
php artisan route:cache
```

```bash
php artisan view:cache
```

O:

```bash
php artisan optimize
```

Después de desplegar cambios:

```bash
php artisan migrate --force
```

⚠️ `--force` permite ejecutar migraciones en producción sin la confirmación interactiva.

---

# 🧠 30. COMANDOS QUE DEBES MEMORIZAR

Si eres desarrollador Laravel, estos deberían salirte casi automáticamente:

```bash
php artisan make:model
php artisan make:controller
php artisan make:migration
php artisan make:request
php artisan make:middleware
php artisan make:policy
php artisan make:seeder
php artisan make:factory
php artisan make:job
php artisan make:event
php artisan make:listener
php artisan make:observer
php artisan make:command
```

Y para trabajar:

```bash
php artisan serve
php artisan migrate
php artisan migrate:rollback
php artisan migrate:fresh --seed
php artisan db:seed
php artisan tinker
php artisan route:list
php artisan optimize:clear
php artisan storage:link
php artisan queue:work
php artisan queue:restart
php artisan schedule:list
```

---

# 🏆 FLUJO REAL DE TRABAJO

Un ejemplo típico:

### 1. Crear modelo

```bash
php artisan make:model Producto -mfsc
```

### 2. Crear migración

Editar:

```text
database/migrations/
```

### 3. Ejecutar

```bash
php artisan migrate
```

### 4. Crear Request

```bash
php artisan make:request StoreProductoRequest
```

### 5. Crear Controller

```bash
php artisan make:controller ProductoController --resource
```

### 6. Crear Policy

```bash
php artisan make:policy ProductoPolicy --model=Producto
```

### 7. Crear rutas

```text
routes/web.php
```

### 8. Probar

```bash
php artisan tinker
```

### 9. Revisar rutas

```bash
php artisan route:list
```

### 10. Si algo está extraño

```bash
php artisan optimize:clear
```

---

# 🚨 COMANDOS PELIGROSOS

Ten especial cuidado con:

```bash
php artisan migrate:fresh
```

Elimina las tablas.

```bash
php artisan migrate:refresh
```

Revierte y vuelve a ejecutar migraciones.

```bash
php artisan db:wipe
```

Elimina las tablas de la base de datos.

```bash
php artisan migrate:fresh --seed
```

Elimina la base de datos y la reconstruye.

### Regla

> **Nunca ejecutes comandos destructivos en producción sin saber exactamente qué van a modificar.**

---

# 🎯 NIVEL JUNIOR → SENIOR

## Junior

Debes dominar:

```text
serve
make:model
make:controller
make:migration
migrate
rollback
db:seed
tinker
route:list
```

## Mid

Debes dominar:

```text
make:request
make:policy
make:middleware
make:observer
make:event
make:listener
make:job
queue:work
schedule:list
storage:link
optimize:clear
```

## Senior

Debes entender:

```text
config:cache
route:cache
view:cache
optimize
queue:restart
migrate --force
chunking / jobs
scheduler
workers
logs
deployment
```

---

# 🧠 REGLA DE ORO DE ARTISAN

No intentes memorizar **todos** los comandos.

Cuando no recuerdes uno:

```bash
php artisan list
```

y después:

```bash
php artisan help comando
```

Por ejemplo:

```bash
php artisan help make:model
```

Un buen desarrollador Laravel no es quien memoriza todos los comandos.

Es quien sabe:

> **qué herramienta necesita, cómo encontrarla y cómo utilizarla correctamente.**
