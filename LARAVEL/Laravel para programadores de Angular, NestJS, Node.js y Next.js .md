# 🚀 Laravel para programadores de Angular, NestJS, Node.js y Next.js

## Guía rápida en 30 pasos para prueba técnica

> Objetivo: que puedas entrar a Laravel y pensar:
>
> **"Esto en NestJS/Angular/Next.js sería equivalente a..."**

---

# 🧠 MAPA MENTAL PRINCIPAL

Antes de empezar, memoriza esto:

| Laravel                | NestJS / Node        | Angular / Next          |
| ---------------------- | -------------------- | ----------------------- |
| Route                  | Controller route     | Routing                 |
| Controller             | Controller           | API/service             |
| Model                  | Entity/Model         | Interface/Model         |
| Eloquent               | TypeORM/Prisma       | —                       |
| Middleware             | Middleware/Guard     | Middleware              |
| Form Request           | DTO + ValidationPipe | Form validation         |
| Policy                 | Guard/Authorization  | Route guard             |
| Service                | Service              | Service                 |
| Resource               | DTO/Serializer       | Response mapper         |
| Migration              | Migration            | Migration               |
| Seeder                 | Seeder               | Seed                    |
| Queue                  | Bull/BullMQ          | Background job          |
| Event                  | EventEmitter         | Event                   |
| Listener               | Listener             | Subscriber              |
| Job                    | Worker job           | Background task         |
| Blade                  | Templates            | Angular templates / SSR |
| Sanctum                | Auth/JWT/session     | Auth system             |
| Artisan                | npm scripts / CLI    | CLI                     |
| `.env`                 | `.env`               | `.env`                  |
| `config/`              | ConfigModule         | Config                  |
| Eloquent relationships | TypeORM relations    | —                       |

---

# 1. 🏗️ ENTENDER LA ESTRUCTURA

Laravel:

```text
app/
├── Models/
├── Http/
│   ├── Controllers/
│   ├── Middleware/
│   ├── Requests/
│   └── Resources/
├── Services/
├── Policies/
├── Jobs/
├── Events/
└── Listeners/

routes/
├── web.php
└── api.php

database/
├── migrations/
├── seeders/
└── factories/

resources/
├── views/
├── js/
└── css/

config/
.env
artisan
composer.json
```

Piensa en:

```text
NestJS
   ↓
Modules
Controllers
Services
DTOs
Guards
Entities
```

Laravel distribuye responsabilidades de forma diferente, pero el concepto es muy parecido.

---

# 2. ⚡ ARTISAN = CLI DE LARAVEL

En Node:

```bash
npm run
```

En Nest:

```bash
nest generate controller users
```

En Laravel:

```bash
php artisan
```

Ver comandos:

```bash
php artisan list
```

Ver rutas:

```bash
php artisan route:list
```

Limpiar caché:

```bash
php artisan optimize:clear
```

---

# 3. 📦 COMPOSER = NPM

Node:

```bash
npm install axios
```

Laravel:

```bash
composer require laravel/sanctum
```

Archivos:

```text
package.json
```

equivalente conceptual:

```text
composer.json
```

Dependencias:

```text
node_modules/
```

equivalente:

```text
vendor/
```

---

# 4. 🛣️ ROUTES

NestJS:

```typescript
@Get('users')
findAll() {}
```

Laravel:

```php
Route::get('/users', [
    UserController::class,
    'index'
]);
```

POST:

```php
Route::post('/users', [
    UserController::class,
    'store'
]);
```

PUT:

```php
Route::put('/users/{user}', [
    UserController::class,
    'update'
]);
```

DELETE:

```php
Route::delete('/users/{user}', [
    UserController::class,
    'destroy'
]);
```

---

# 5. 🎮 CONTROLLER

NestJS:

```typescript
@Controller('users')
export class UsersController {

    @Get()
    findAll() {}

}
```

Laravel:

```bash
php artisan make:controller UserController --api
```

Después:

```php
class UserController extends Controller
{
    public function index()
    {
        return User::all();
    }
}
```

Mentalidad:

```text
Route
 ↓
Controller
 ↓
Service
 ↓
Model
```

---

# 6. 🗃️ MODEL = ENTITY / ORM

NestJS + TypeORM:

```typescript
@Entity()
class User {}
```

Laravel:

```bash
php artisan make:model User
```

Modelo:

```php
class User extends Model
{
}
```

Laravel utiliza:

```text
Eloquent ORM
```

Conceptualmente:

```text
Eloquent ≈ TypeORM / Prisma
```

---

# 7. 🏦 TABLAS Y MIGRATIONS

Crear migration:

```bash
php artisan make:migration create_products_table
```

Ejemplo:

```php
Schema::create('products', function (Blueprint $table) {

    $table->id();

    $table->string('name');

    $table->decimal('price', 10, 2);

    $table->timestamps();

});
```

Ejecutar:

```bash
php artisan migrate
```

Rollback:

```bash
php artisan migrate:rollback
```

---

# 8. 🔑 PRIMARY KEY

Por defecto:

```php
$table->id();
```

crea:

```text
id
BIGINT
UNSIGNED
AUTO_INCREMENT
PRIMARY KEY
```

En Eloquent:

```php
$product->id;
```

---

# 9. 🛡️ FILLABLE

Muy importante para pruebas técnicas.

```php
protected $fillable = [
    'name',
    'price',
];
```

Permite:

```php
Product::create([
    'name' => 'Laptop',
    'price' => 2500000,
]);
```

Piensa:

```text
fillable
≈
qué campos pueden entrar mediante asignación masiva
```

---

# 10. 🚫 GUARDED

```php
protected $guarded = [
    'id',
];
```

Significa:

```text
Estos campos están protegidos.
```

Normalmente prefiero:

```php
protected $fillable = [
    'name',
    'price',
];
```

porque es más explícito.

---

# 11. 🧬 CASTS

Laravel:

```php
protected function casts(): array
{
    return [
        'active' => 'boolean',
        'price' => 'decimal:2',
        'settings' => 'array',
    ];
}
```

Equivalente conceptual a transformar tipos de datos.

Ejemplo:

```text
DB
"1"

↓ cast

PHP
true
```

---

# 12. 🔎 ELOQUENT CRUD

Todos:

```php
User::all();
```

Uno:

```php
User::find(1);
```

Uno o error 404:

```php
User::findOrFail(1);
```

Filtrar:

```php
User::where('active', true)->get();
```

Primero:

```php
User::where('email', $email)->first();
```

Crear:

```php
User::create($data);
```

Actualizar:

```php
$user->update($data);
```

Eliminar:

```php
$user->delete();
```

---

# 13. 🔥 QUERY BUILDER

Eloquent:

```php
Product::where('price', '>', 1000000)
    ->orderBy('price', 'desc')
    ->get();
```

Puedes encadenar:

```text
where()
 ↓
orderBy()
 ↓
limit()
 ↓
get()
```

Esto se parece mucho a construir consultas mediante ORM/query builders en Node.

---

# 14. 🔗 RELACIONES

Supongamos:

```text
User
 ↓
Orders
```

User:

```php
public function orders()
{
    return $this->hasMany(Order::class);
}
```

Order:

```php
public function user()
{
    return $this->belongsTo(User::class);
}
```

Usar:

```php
$user->orders;
```

---

# 15. 🧠 RELACIONES QUE DEBES SABER

Memoriza:

```text
hasOne
hasMany
belongsTo
belongsToMany
hasManyThrough
```

Ejemplo:

```php
$user->orders();
```

```php
$order->user();
```

```php
$product->categories();
```

---

# 16. ⚡ EAGER LOADING

Problema N+1:

```php
$users = User::all();

foreach ($users as $user) {
    echo $user->orders;
}
```

Puede generar muchas consultas.

Mejor:

```php
$users = User::with('orders')->get();
```

Mentalidad:

```text
N+1
=
una consulta inicial
+
una consulta por cada registro
```

Esto es una pregunta clásica de prueba técnica.

---

# 17. 📥 FORM REQUEST = DTO + VALIDATION

Crear:

```bash
php artisan make:request StoreProductRequest
```

Ejemplo:

```php
public function rules(): array
{
    return [
        'name' => ['required', 'string', 'max:255'],
        'price' => ['required', 'numeric', 'min:0'],
    ];
}
```

Controller:

```php
public function store(StoreProductRequest $request)
{
    $product = Product::create(
        $request->validated()
    );

    return response()->json($product, 201);
}
```

Mentalidad:

```text
NestJS
DTO
+
ValidationPipe

Laravel
Form Request
```

---

# 18. 🧱 SERVICE

No metas toda la lógica en Controller.

Mala práctica:

```php
public function store(Request $request)
{
    // 100 líneas
}
```

Mejor:

```text
Controller
 ↓
Service
 ↓
Eloquent
```

Crear manualmente:

```text
app/Services/ProductService.php
```

Ejemplo:

```php
class ProductService
{
    public function create(array $data)
    {
        return Product::create($data);
    }
}
```

Conceptualmente:

```text
Laravel Service
≈
NestJS Service
```

---

# 19. 📤 API RESOURCE

Crear:

```bash
php artisan make:resource ProductResource
```

```php
class ProductResource extends JsonResource
{
    public function toArray($request)
    {
        return [
            'id' => $this->id,
            'name' => $this->name,
            'price' => $this->price,
        ];
    }
}
```

Controller:

```php
return ProductResource::collection(
    Product::all()
);
```

Mentalidad:

```text
Resource
≈
DTO de respuesta / serializer
```

---

# 20. 🛡️ MIDDLEWARE

Crear:

```bash
php artisan make:middleware AdminMiddleware
```

```php
public function handle(
    Request $request,
    Closure $next
) {

    if (!$request->user()?->is_admin) {

        return response()->json([
            'message' => 'Forbidden'
        ], 403);

    }

    return $next($request);
}
```

Flujo:

```text
Request
 ↓
Middleware
 ↓
Controller
```

Exactamente el concepto que ya conoces de Node/Nest.

---

# 21. 🔐 SANCTUM

Instalar:

```bash
composer require laravel/sanctum
```

En Laravel moderno:

```bash
php artisan install:api
```

Migrar:

```bash
php artisan migrate
```

El modelo User utiliza:

```php
use HasApiTokens;
```

Crear token:

```php
$token = $user->createToken(
    'api-token'
)->plainTextToken;
```

---

# 22. 🎟️ ¿DÓNDE GUARDA SANCTUM EL TOKEN?

Tabla:

```text
personal_access_tokens
```

Cuando ejecutas:

```php
$user->createToken('api-token');
```

Sanctum:

```text
Genera token
 ↓
Hashea token
 ↓
Guarda registro
 ↓
Devuelve plainTextToken
```

El cliente recibe:

```text
1|xxxxxxxxxxxx
```

La BD guarda el hash.

---

# 23. 🔒 PROTEGER RUTAS

```php
Route::middleware('auth:sanctum')
    ->get('/user', function (Request $request) {

        return $request->user();

    });
```

Frontend:

```http
Authorization: Bearer TOKEN
```

Flujo:

```text
Frontend
 ↓
Bearer Token
 ↓
Sanctum
 ↓
Middleware
 ↓
Controller
```

---

# 24. 👮 POLICIES = AUTORIZACIÓN

Crear:

```bash
php artisan make:policy ProductPolicy --model=Product
```

Ejemplo:

```php
public function update(
    User $user,
    Product $product
): bool {

    return $product->user_id === $user->id;
}
```

Mentalidad:

```text
Middleware
=
¿puedes entrar?

Policy
=
¿puedes modificar ESTE recurso?
```

---

# 25. 🧪 TESTING

Crear:

```bash
php artisan make:test ProductTest
```

Test de feature:

```bash
php artisan make:test ProductTest --feature
```

Ejemplo conceptual:

```php
$response = $this->getJson(
    '/api/products'
);

$response->assertStatus(200);
```

Laravel utiliza PHPUnit/Pest según la configuración del proyecto.

Mentalidad:

```text
Jest
≈
PHPUnit / Pest
```

---

# 26. 🌱 FACTORIES Y SEEDERS

Factory:

```bash
php artisan make:factory ProductFactory
```

Seeder:

```bash
php artisan make:seeder ProductSeeder
```

Ejemplo:

```php
Product::factory()
    ->count(50)
    ->create();
```

Ejecutar:

```bash
php artisan db:seed
```

o:

```bash
php artisan migrate:fresh --seed
```

Esto es muy útil para pruebas técnicas.

---

# 27. 🔄 TRANSACTIONS

Si tienes:

```text
Crear pedido
 ↓
Crear detalles
 ↓
Actualizar stock
 ↓
Registrar pago
```

y algo falla, necesitas una transacción.

```php
DB::transaction(function () {

    $order = Order::create(...);

    OrderItem::create(...);

    Product::where(...)
        ->decrement('stock', 1);

});
```

Concepto:

```text
Todo funciona
→ COMMIT

Algo falla
→ ROLLBACK
```

Esto demuestra criterio backend.

---

# 28. ⚙️ QUEUES Y JOBS

Crear Job:

```bash
php artisan make:job SendEmailJob
```

Ejemplo:

```php
class SendEmailJob implements ShouldQueue
{
    public function handle()
    {
        // enviar email
    }
}
```

Despachar:

```php
SendEmailJob::dispatch();
```

Mentalidad:

```text
Laravel Job
≈
BullMQ Job
```

Ideal para:

```text
Emails
Procesamiento de archivos
Reportes
Procesos pesados
Notificaciones
```

---

# 29. 📡 EVENTS Y LISTENERS

Crear:

```bash
php artisan make:event OrderCreated
```

Listener:

```bash
php artisan make:listener SendOrderNotification
```

Flujo:

```text
OrderCreated
      ↓
Listener
      ↓
Enviar notificación
```

Ventaja:

Desacoplas funcionalidades.

Por ejemplo:

```text
Pedido creado
      │
      ├── Email
      ├── Notificación
      ├── Facturación
      └── Estadística
```

El Controller no necesita conocer todos esos procesos.

---

# 30. 🏆 ARQUITECTURA PARA LA PRUEBA TÉCNICA

Si te dan:

> "Construya una API para administrar productos."

Piensa así:

```text
                    CLIENTE
                       │
                       ↓
                    ROUTE
                       │
                       ↓
                auth:sanctum
                       │
                       ↓
                 CONTROLLER
                       │
                       ↓
               FORM REQUEST
                       │
                       ↓
                   POLICY
                       │
                       ↓
                   SERVICE
                       │
                       ↓
                   ELOQUENT
                       │
                       ↓
                    MYSQL
                       │
                       ↓
                   RESOURCE
                       │
                       ↓
                     JSON
```

---

# 🎯 EJEMPLO COMPLETO

## Route

```php
Route::middleware('auth:sanctum')
    ->apiResource(
        'products',
        ProductController::class
    );
```

---

## Request

```php
class StoreProductRequest extends FormRequest
{
    public function rules(): array
    {
        return [
            'name' => ['required', 'string'],
            'price' => ['required', 'numeric'],
        ];
    }
}
```

---

## Controller

```php
public function store(
    StoreProductRequest $request
) {

    $product = Product::create(
        $request->validated()
    );

    return new ProductResource($product);
}
```

---

## Model

```php
class Product extends Model
{
    protected $fillable = [
        'name',
        'price',
    ];

    protected function casts(): array
    {
        return [
            'price' => 'decimal:2',
        ];
    };
}
```

---

# 🧠 CHULETA DE COMANDOS

## Crear cosas

```bash
php artisan make:model Product -mf

php artisan make:controller ProductController --api

php artisan make:request StoreProductRequest

php artisan make:resource ProductResource

php artisan make:middleware AdminMiddleware

php artisan make:policy ProductPolicy --model=Product

php artisan make:job ProcessProduct

php artisan make:event ProductCreated

php artisan make:listener SendNotification

php artisan make:test ProductTest
```

---

# 🗄️ BASE DE DATOS

```bash
php artisan migrate

php artisan migrate:rollback

php artisan migrate:fresh

php artisan migrate:fresh --seed

php artisan db:seed

php artisan db:show

php artisan db:table products
```

---

# 🔎 DEBUG

```bash
php artisan route:list

php artisan tinker

php artisan about

php artisan optimize:clear
```

Tinker:

```bash
php artisan tinker
```

Después:

```php
User::count();

User::first();

Product::all();
```

---

# 🐳 DOCKER

Si el proyecto utiliza Docker:

```bash
docker compose up -d
```

Ver contenedores:

```bash
docker compose ps
```

Logs:

```bash
docker compose logs -f
```

Entrar al contenedor:

```bash
docker compose exec app bash
```

Y dentro:

```bash
php artisan migrate
```

---

# ⚡ COMPARACIÓN FINAL

Si vienes de NestJS:

```text
Controller
    ↓
Service
    ↓
Repository / ORM
    ↓
Database
```

Laravel:

```text
Controller
    ↓
Service
    ↓
Eloquent
    ↓
Database
```

Si vienes de Angular:

```text
Component
 ↓
Service
 ↓
HTTP
```

Laravel:

```text
Controller
 ↓
Service
 ↓
Eloquent
```

Si vienes de Next.js:

```text
Frontend
 ↓
fetch()
 ↓
API
```

Laravel:

```text
Route
 ↓
Controller
 ↓
JSON
```

---

# 🧠 LAS 15 COSAS QUE DEBES PODER EXPLICAR EN UNA ENTREVISTA

1. ¿Qué es Laravel?
2. ¿Qué es Eloquent?
3. ¿Qué es una Migration?
4. ¿Qué diferencia hay entre `find()` y `findOrFail()`?
5. ¿Qué es `$fillable`?
6. ¿Qué es `$casts`?
7. ¿Qué es una relación Eloquent?
8. ¿Qué es el problema N+1?
9. ¿Qué es `with()`?
10. ¿Qué es un Form Request?
11. ¿Qué es Middleware?
12. ¿Qué es una Policy?
13. ¿Cómo funciona Sanctum?
14. ¿Qué es una API Resource?
15. ¿Cuándo utilizarías un Service/Job/Queue?

---

# 🏆 FRASE PARA RECORDAR TODO LARAVEL

```text
ROUTE
  ↓
MIDDLEWARE
  ↓
CONTROLLER
  ↓
REQUEST
  ↓
POLICY
  ↓
SERVICE
  ↓
ELOQUENT
  ↓
DATABASE
  ↓
RESOURCE
  ↓
JSON
```

Y para autenticación:

```text
LOGIN
 ↓
SANCTUM
 ↓
TOKEN
 ↓
personal_access_tokens
 ↓
Authorization: Bearer TOKEN
 ↓
auth:sanctum
 ↓
$request->user()
```

## 🚀 Si ya sabes NestJS + Angular + Node + Next.js

No estás empezando de cero.

Tu reto principal es aprender la **sintaxis y convenciones de Laravel**:

```text
TypeScript        → PHP
NestJS Controller  → Laravel Controller
NestJS Service     → Laravel Service
TypeORM/Prisma     → Eloquent
DTO                → Form Request
Guard              → Middleware / Policy
BullMQ             → Queue / Job
Jest               → PHPUnit / Pest
npm                → Composer
CLI                → Artisan
```

Una vez hagas estas equivalencias mentalmente, Laravel se vuelve mucho más fácil de leer.
