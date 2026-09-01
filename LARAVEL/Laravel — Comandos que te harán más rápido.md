# ⚡ Laravel — Comandos que te harán más rápido

> **Objetivo:** dejar de perder tiempo creando archivos manualmente y aprender a trabajar Laravel desde Artisan.

---

# 🧠 1. `php artisan list`

Muestra todos los comandos disponibles.

```bash
php artisan list
```

Úsalo cuando pienses:

> "¿Laravel tendrá un comando para hacer esto?"

Muy útil cuando no recuerdas la sintaxis.

---

# 🚀 2. `php artisan help`

Ver ayuda de cualquier comando.

```bash
php artisan help make:model
```

También:

```bash
php artisan help migrate
```

Te muestra opciones y argumentos.

---

# 🏗️ 3. `make:model`

Uno de los comandos más importantes.

```bash
php artisan make:model Product
```

Crea:

```text
app/Models/Product.php
```

---

## 🔥 Combínalo con opciones

```bash
php artisan make:model Product -m
```

Modelo + Migration.

```bash
php artisan make:model Product -mf
```

Modelo + Migration + Factory.

```bash
php artisan make:model Product -mfs
```

Modelo + Migration + Factory + Seeder.

```bash
php artisan make:model Product -a
```

Genera prácticamente todo lo necesario para trabajar el modelo.

---

# 🧠 4. `make:controller`

Crear Controller:

```bash
php artisan make:controller ProductController
```

Para API:

```bash
php artisan make:controller ProductController --api
```

Para Resource Controller:

```bash
php artisan make:controller ProductController --resource
```

Para una API CRUD, normalmente:

```bash
php artisan make:controller ProductController --api
```

---

# 📦 5. `make:request`

Crear validación:

```bash
php artisan make:request StoreProductRequest
```

Obtienes:

```text
app/Http/Requests/
└── StoreProductRequest.php
```

Ideal para evitar validaciones gigantes dentro del Controller.

---

# 🛡️ 6. `make:middleware`

Crear Middleware:

```bash
php artisan make:middleware AdminMiddleware
```

Obtienes:

```text
app/Http/Middleware/AdminMiddleware.php
```

Ejemplos:

```text
AdminMiddleware
RoleMiddleware
PermissionMiddleware
CheckSubscription
```

---

# 👮 7. `make:policy`

Crear autorización:

```bash
php artisan make:policy ProductPolicy
```

Si quieres relacionarlo directamente con el modelo:

```bash
php artisan make:policy ProductPolicy --model=Product
```

Mentalidad:

```text
Middleware
↓
¿Puedes entrar?

Policy
↓
¿Puedes modificar este recurso?
```

---

# 🎨 8. `make:resource`

Crear API Resource:

```bash
php artisan make:resource ProductResource
```

Sirve para controlar el JSON que devuelve tu API.

```text
Model
 ↓
Resource
 ↓
JSON
```

---

# 🗄️ 9. `make:migration`

Crear una migración:

```bash
php artisan make:migration create_products_table
```

Agregar una columna:

```bash
php artisan make:migration add_stock_to_products_table
```

---

# 🏦 10. `migrate`

Ejecutar migraciones:

```bash
php artisan migrate
```

Piensa:

```text
Migration
 ↓
MySQL
```

---

# ↩️ 11. `migrate:rollback`

Deshacer la última tanda de migraciones:

```bash
php artisan migrate:rollback
```

Muy útil cuando estás desarrollando.

---

# 🔥 12. `migrate:fresh`

Borra todas las tablas y vuelve a ejecutar las migraciones.

```bash
php artisan migrate:fresh
```

⚠️ **No usar en producción.**

---

# 🌱 13. `migrate:fresh --seed`

Borra la BD y la reconstruye con datos de prueba.

```bash
php artisan migrate:fresh --seed
```

Extremadamente útil durante desarrollo.

---

# 🌱 14. `db:seed`

Ejecuta los seeders:

```bash
php artisan db:seed
```

Seeder específico:

```bash
php artisan db:seed --class=ProductSeeder
```

---

# 🏭 15. `make:factory`

Crear Factory:

```bash
php artisan make:factory ProductFactory
```

Después puedes generar:

```php
Product::factory()
    ->count(100)
    ->create();
```

Perfecto para:

```text
Testing
Desarrollo
Datos falsos
Pruebas de UI
```

---

# 🧪 16. `make:test`

Crear test:

```bash
php artisan make:test ProductTest
```

Test de Feature:

```bash
php artisan make:test ProductTest --feature
```

Test Unit:

```bash
php artisan make:test ProductTest --unit
```

---

# 🔐 17. `install:api`

En Laravel moderno:

```bash
php artisan install:api
```

Es especialmente importante cuando vas a construir una API.

Puede preparar la infraestructura de API y Sanctum según la versión de Laravel.

---

# 🔑 18. Sanctum

Instalación:

```bash
composer require laravel/sanctum
```

Después, dependiendo de la versión de Laravel:

```bash
php artisan install:api
```

Migraciones:

```bash
php artisan migrate
```

---

# 🧭 19. `route:list`

Probablemente uno de los comandos que más utilizarás.

```bash
php artisan route:list
```

Puedes filtrar:

```bash
php artisan route:list --path=api
```

Te permite descubrir:

```text
Método
URL
Controller
Middleware
Nombre
```

Cuando recibas un proyecto desconocido:

```bash
php artisan route:list
```

es uno de los primeros comandos que deberías ejecutar.

---

# 🧠 20. `tinker`

Abre una consola interactiva de Laravel:

```bash
php artisan tinker
```

Puedes hacer:

```php
User::count();
```

```php
User::first();
```

```php
Product::all();
```

```php
Product::where('price', '>', 1000000)->get();
```

Crear:

```php
Product::create([
    'name' => 'Laptop',
    'price' => 2500000
]);
```

Es como tener una consola interactiva para probar Eloquent.

---

# 🔍 21. `model:show`

Puedes inspeccionar un modelo:

```bash
php artisan model:show Product
```

Te ayuda a entender:

```text
Atributos
Relaciones
Métodos
Casts
```

Muy útil cuando llegas a un proyecto que no conoces.

---

# 🗄️ 22. `db:show`

Ver información de la base de datos:

```bash
php artisan db:show
```

Y una tabla:

```bash
php artisan db:table products
```

Muy útil para descubrir rápidamente la estructura de una aplicación.

---

# 🧹 23. `optimize:clear`

Uno de los comandos que debes memorizar.

```bash
php artisan optimize:clear
```

Limpia las cachés relevantes de Laravel.

Cuando algo parece:

> "Está bien el código pero Laravel sigue utilizando lo anterior..."

prueba:

```bash
php artisan optimize:clear
```

---

# ⚡ 24. `config:clear`

Limpia la configuración cacheada:

```bash
php artisan config:clear
```

Especialmente útil después de cambiar:

```text
.env
```

---

# 🛣️ 25. `route:clear`

Limpia la caché de rutas:

```bash
php artisan route:clear
```

---

# 👁️ 26. `view:clear`

Limpia las vistas compiladas:

```bash
php artisan view:clear
```

---

# ⚙️ 27. `config:cache`

En producción:

```bash
php artisan config:cache
```

Optimiza la carga de configuración.

No lo uses como sustituto de entender cómo funcionan los `.env`.

---

# 🚀 28. `route:cache`

En producción:

```bash
php artisan route:cache
```

Puede mejorar el procesamiento de rutas.

---

# 📦 29. `vendor:publish`

Permite publicar archivos de paquetes.

Ejemplo conceptual:

```bash
php artisan vendor:publish
```

Laravel te mostrará las opciones disponibles.

Muy utilizado cuando un paquete necesita publicar:

```text
config
views
assets
migrations
```

---

# 🔎 30. `about`

Información general del proyecto:

```bash
php artisan about
```

Muy útil cuando llegas a un proyecto existente.

Puedes descubrir:

```text
Laravel
PHP
Environment
Cache
Database
Drivers
```

---

# ⏰ 31. `schedule:list`

Si el proyecto utiliza tareas programadas:

```bash
php artisan schedule:list
```

Te permite ver las tareas programadas.

---

# ⚙️ 32. `schedule:run`

Ejecuta las tareas programadas que correspondan en ese momento:

```bash
php artisan schedule:run
```

Normalmente se ejecuta mediante cron en producción.

---

# 📬 33. `queue:work`

Procesar Jobs:

```bash
php artisan queue:work
```

Ejemplo:

```text
Job
 ↓
Queue
 ↓
queue:work
 ↓
Procesamiento
```

Muy importante para:

```text
Emails
Reportes
Archivos
Procesos pesados
Notificaciones
```

---

# 🔄 34. `queue:restart`

Reinicia los workers de Queue de forma segura:

```bash
php artisan queue:restart
```

Muy utilizado después de desplegar cambios.

---

# 📋 35. `queue:failed`

Ver Jobs fallidos:

```bash
php artisan queue:failed
```

Muy útil en producción.

---

# 🔁 36. `queue:retry`

Reintentar un Job fallido:

```bash
php artisan queue:retry all
```

O uno específico:

```bash
php artisan queue:retry 5
```

---

# 📧 37. `make:mail`

Crear un Mail:

```bash
php artisan make:mail WelcomeMail
```

Después puedes enviarlo desde Laravel.

---

# 🔔 38. `make:notification`

Crear una notificación:

```bash
php artisan make:notification OrderCreated
```

Puede utilizar:

```text
Email
Database
Broadcast
```

---

# 📡 39. `make:event`

Crear Event:

```bash
php artisan make:event OrderCreated
```

---

# 👂 40. `make:listener`

Crear Listener:

```bash
php artisan make:listener SendOrderNotification
```

Arquitectura:

```text
Event
 ↓
Listener
 ↓
Acción
```

---

# ⚡ 41. `make:job`

Crear Job:

```bash
php artisan make:job ProcessOrder
```

Si es para Queue, implementas:

```php
ShouldQueue
```

---

# 🧩 42. `make:command`

Crear tu propio comando Artisan:

```bash
php artisan make:command ImportProducts
```

Después puedes ejecutar:

```bash
php artisan products:import
```

Esto es muy útil para:

```text
Importaciones
Mantenimiento
Procesos automáticos
Scripts internos
```

---

# 🧪 43. `test`

Ejecutar tests:

```bash
php artisan test
```

Un archivo:

```bash
php artisan test tests/Feature/ProductTest.php
```

Filtrar:

```bash
php artisan test --filter=ProductTest
```

---

# 🔥 44. `serve`

Servidor local:

```bash
php artisan serve
```

Normalmente:

```text
http://127.0.0.1:8000
```

---

# 🐳 45. SI USAS DOCKER

Si tienes Laravel Sail:

```bash
./vendor/bin/sail up -d
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

# ⚡ ATAJO MENTAL PARA DESARROLLAR CRUD

Si te dicen:

> "Haz un CRUD de productos."

Piensa inmediatamente:

```bash
php artisan make:model Product -mfsc
```

Esto te puede crear:

```text
Model
Migration
Factory
Seeder
Controller
```

Después:

```bash
php artisan migrate
```

Request:

```bash
php artisan make:request StoreProductRequest
```

Resource:

```bash
php artisan make:resource ProductResource
```

Policy:

```bash
php artisan make:policy ProductPolicy --model=Product
```

Ruta:

```php
Route::apiResource(
    'products',
    ProductController::class
);
```

Y finalmente:

```bash
php artisan route:list
```

---

# 🏆 LOS 15 QUE YO MEMORIZARÍA PRIMERO

Si tienes poco tiempo antes de una prueba técnica:

```bash
php artisan list

php artisan make:model Product -mfsc

php artisan make:controller ProductController --api

php artisan make:request StoreProductRequest

php artisan make:resource ProductResource

php artisan make:policy ProductPolicy --model=Product

php artisan migrate

php artisan migrate:rollback

php artisan migrate:fresh --seed

php artisan db:seed

php artisan route:list

php artisan tinker

php artisan optimize:clear

php artisan test

php artisan queue:work
```

---

# 🧠 MAPA RÁPIDO POR CATEGORÍA

## 🏗️ Crear

```text
make:model
make:controller
make:request
make:resource
make:middleware
make:policy
make:job
make:event
make:listener
make:test
make:command
```

## 🗄️ Base de datos

```text
make:migration
migrate
migrate:rollback
migrate:fresh
db:seed
db:show
db:table
```

## 🔍 Explorar

```text
route:list
tinker
model:show
about
```

## 🧹 Limpiar

```text
optimize:clear
config:clear
route:clear
view:clear
```

## 🧪 Testing

```text
make:test
test
```

## ⚙️ Producción

```text
config:cache
route:cache
queue:work
queue:restart
```

## 📬 Jobs

```text
queue:work
queue:failed
queue:retry
queue:restart
```

---

# 🚀 FLUJO REAL DE UNA PRUEBA TÉCNICA

Si te dan 2 horas para construir una API:

```text
1. Crear proyecto
        ↓
2. Configurar .env
        ↓
3. Migration
        ↓
4. Model
        ↓
5. Factory
        ↓
6. Seeder
        ↓
7. Form Request
        ↓
8. Controller
        ↓
9. Resource
        ↓
10. Policy
        ↓
11. Sanctum
        ↓
12. Middleware
        ↓
13. Routes
        ↓
14. Tests
        ↓
15. Tinker
        ↓
16. route:list
        ↓
17. Probar API
```

---

# 🧠 LA CHULETA MÁS IMPORTANTE

Cuando no sepas qué hacer, piensa:

```text
¿Necesito CREAR algo?
        ↓
php artisan make:...

¿Necesito CAMBIAR BD?
        ↓
make:migration + migrate

¿Necesito VER datos?
        ↓
tinker

¿No encuentro una ruta?
        ↓
route:list

¿Algo parece cacheado?
        ↓
optimize:clear

¿Necesito VALIDAR?
        ↓
make:request

¿Necesito AUTORIZAR?
        ↓
make:policy

¿Necesito PROTEGER?
        ↓
middleware

¿Necesito AUTENTICAR API?
        ↓
Sanctum

¿Necesito DEVOLVER JSON limpio?
        ↓
make:resource

¿Necesito PROCESAR ALGO PESADO?
        ↓
make:job + queue

¿Necesito probar?
        ↓
make:test + test
```

# 🏆 Si vienes de NestJS

Memoriza estas equivalencias:

```text
nest generate controller
        ↓
php artisan make:controller

nest generate service
        ↓
app/Services/...
        ↓
No existe un make:service oficial equivalente directo

DTO
        ↓
make:request

Entity
        ↓
make:model

Migration
        ↓
make:migration

Guard
        ↓
Middleware / Policy

Interceptor / Serializer
        ↓
API Resource

BullMQ
        ↓
Queue + Job

npm
        ↓
composer

nest CLI
        ↓
artisan

npm test
        ↓
php artisan test
```

**La idea es que Artisan se convierta en tu "Nest CLI": antes de crear archivos manualmente, pregunta primero si existe un `make:*` para generarlos.**
