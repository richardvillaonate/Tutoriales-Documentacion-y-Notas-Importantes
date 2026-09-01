# ⚡ Laravel — Guía Rápida de Conceptos

> Una guía para entender **qué es cada cosa, para qué sirve y cuándo usarla**.

---

# 🧠 1. MVC

Laravel trabaja principalmente con el patrón **MVC**.

```text
Usuario
   ↓
Route
   ↓
Controller
   ↓
Model
   ↓
Database
   ↓
Controller
   ↓
View
   ↓
Usuario
```

### Model

Habla con la base de datos.

```php
$usuarios = User::all();
```

### Controller

Controla el flujo.

```php
public function index()
{
    $usuarios = User::all();

    return view('usuarios.index', compact('usuarios'));
}
```

### View

Lo que ve el usuario.

```blade
<h1>{{ $usuario->name }}</h1>
```

### Recuerda

> **Model = datos**
> **Controller = lógica de entrada/salida**
> **View = interfaz**

---

# 🛣️ 2. ROUTES

Las rutas indican:

> "Cuando alguien visite esta dirección, ¿qué debe ejecutarse?"

```php
Route::get('/usuarios', [UsuarioController::class, 'index']);
```

Significa:

```text
GET /usuarios
      ↓
UsuarioController
      ↓
index()
```

---

# 🎮 3. CONTROLLER

El Controller recibe la petición y decide qué hacer.

```php
public function index()
{
    $usuarios = User::all();

    return view('usuarios.index', compact('usuarios'));
}
```

### No hagas esto

❌ Controladores gigantes con toda la lógica del sistema.

```text
Controller
 ├── consultas
 ├── pagos
 ├── emails
 ├── reportes
 ├── validaciones
 └── cálculos
```

### Mejor

```text
Controller
   ↓
Request
   ↓
Service
   ↓
Model
```

---

# 🗄️ 4. MODEL

Representa una tabla.

```php
class Producto extends Model
{
}
```

Por defecto:

```text
Producto
   ↓
productos
```

Puedes hacer:

```php
Producto::all();
Producto::find(1);
Producto::create(...);
```

### Recuerda

> **Model = puente entre PHP y la base de datos.**

---

# 🧱 5. MIGRATION

Una migration define la estructura de la base de datos.

```php
$table->id();
$table->string('nombre');
$table->decimal('precio', 10, 2);
$table->boolean('activo');
```

Es como decir:

> "Así quiero que sea mi tabla."

Ejecutar:

```bash
php artisan migrate
```

---

# 🌱 6. SEEDER

Un Seeder mete datos iniciales.

Ejemplo:

```php
User::create([
    'name' => 'Administrador',
]);
```

Ejecutar:

```bash
php artisan db:seed
```

### Recuerda

```text
Migration = estructura
Seeder = datos
```

---

# 🏭 7. FACTORY

Una Factory crea datos falsos automáticamente.

```php
User::factory()->count(100)->create();
```

Perfecto para:

* pruebas
* desarrollo
* testing
* grandes cantidades de datos

### Recuerda

> **Factory = fábrica de datos.**

---

# 📦 8. REQUEST

Un Request sirve para validar lo que llega del usuario.

```php
public function rules()
{
    return [
        'nombre' => 'required|string',
        'email' => 'required|email',
    ];
}
```

### Ventaja

El Controller queda limpio.

```php
public function store(StoreUserRequest $request)
{
    User::create($request->validated());
}
```

### Recuerda

> **Request = filtro de entrada.**

---

# 🔐 9. MIDDLEWARE

Un Middleware es como un **guardia de seguridad**.

```text
Usuario
   ↓
Middleware
   ↓
Controller
```

Ejemplo:

```text
¿Está autenticado?
       ↓
      NO → Login
       ↓
      SÍ
       ↓
   Controller
```

Otros usos:

* permisos
* autenticación
* roles
* logs
* restricciones

### Recuerda

> **Middleware = filtro antes de llegar al Controller.**

---

# 🔑 10. AUTHENTICATION

Responde:

> ¿Quién eres?

Ejemplo:

```text
Richard
   ↓
Login
   ↓
Email + Password
   ↓
Sistema
   ↓
Usuario autenticado
```

---

# 🛡️ 11. AUTHORIZATION

Responde:

> ¿Qué tienes permitido hacer?

Ejemplo:

```text
Usuario
   ↓
¿Puede eliminar este pedido?
   ↓
Sí / No
```

Para esto puedes utilizar:

* Policies
* Gates
* permisos

### Diferencia

```text
Authentication
= ¿Quién eres?

Authorization
= ¿Qué puedes hacer?
```

---

# ⚖️ 12. POLICY

Una Policy controla permisos sobre un modelo.

Ejemplo:

```php
public function update(User $user, Post $post)
{
    return $user->id === $post->user_id;
}
```

Significa:

> Solo el dueño puede modificar el Post.

### Recuerda

> **Policy = reglas de permisos.**

---

# 🧠 13. ELOQUENT

Es el ORM de Laravel.

Permite trabajar con la base de datos usando modelos.

```php
$usuarios = User::where('activo', true)->get();
```

En lugar de escribir directamente:

```sql
SELECT *
FROM users
WHERE activo = 1;
```

---

# 🔗 14. RELACIONES

Permiten conectar modelos.

Ejemplo:

```text
Usuario
   ↓
Pedidos
```

Un usuario tiene muchos pedidos:

```php
public function pedidos()
{
    return $this->hasMany(Pedido::class);
}
```

---

# 15. `hasMany`

Un registro tiene muchos.

```text
Usuario
 ├── Pedido
 ├── Pedido
 └── Pedido
```

```php
return $this->hasMany(Pedido::class);
```

---

# 16. `belongsTo`

Un registro pertenece a otro.

```text
Pedido
   ↓
Usuario
```

```php
return $this->belongsTo(User::class);
```

### Fácil

```text
Usuario → hasMany → Pedidos

Pedido → belongsTo → Usuario
```

---

# 17. `belongsToMany`

Muchos a muchos.

```text
Usuarios
   ↕
 Roles
```

Un usuario puede tener muchos roles.

Un rol puede pertenecer a muchos usuarios.

```php
return $this->belongsToMany(Role::class);
```

Normalmente necesitas una tabla intermedia:

```text
role_user
```

---

# 🚀 18. EAGER LOADING

Sirve para evitar consultas innecesarias.

❌ Problema:

```php
$usuarios = User::all();

foreach ($usuarios as $usuario) {
    echo $usuario->pedidos;
}
```

Puede provocar **N+1 queries**.

Mejor:

```php
$usuarios = User::with('pedidos')->get();
```

### Recuerda

> `with()` = cargar relaciones anticipadamente.

---

# 🐌 19. N+1

Uno de los problemas más comunes con Eloquent.

```text
1 consulta
   +
100 consultas
   =
101 consultas
```

Cuando podrías haber hecho:

```text
2 consultas
```

Solución frecuente:

```php
with()
```

---

# 🔍 20. QUERY BUILDER

Otra forma de consultar la base de datos.

```php
DB::table('usuarios')
    ->where('activo', true)
    ->get();
```

### Eloquent

```php
User::where('activo', true)->get();
```

### Query Builder

```php
DB::table('users')
    ->where('activo', true)
    ->get();
```

### Regla

> Usa Eloquent cuando trabajar con modelos y relaciones sea conveniente.
> Usa Query Builder cuando una consulta sea más apropiada sin necesidad del modelo.

---

# 🎯 21. SCOPE

Sirve para reutilizar consultas.

```php
public function scopeActivos($query)
{
    return $query->where('activo', true);
}
```

Después:

```php
User::activos()->get();
```

### Recuerda

> **Scope = consulta reutilizable.**

---

# 📊 22. PAGINATE

Divide grandes cantidades de datos.

```php
User::paginate(20);
```

En lugar de:

```php
User::all();
```

Si tienes:

```text
100.000 usuarios
```

no quieres cargar los 100.000 de golpe.

---

# 🔄 23. TRANSACTION

Una Transaction permite ejecutar varias operaciones como una sola unidad.

```php
DB::transaction(function () {

    $pedido = Pedido::create(...);

    $pedido->productos()->attach(...);

});
```

Si algo falla:

```text
Operación 1 ✓
Operación 2 ✓
Operación 3 ✗
       ↓
Rollback
       ↓
Se deshacen los cambios
```

### Ejemplo real

Crear pedido:

```text
Crear pedido
     ↓
Actualizar stock
     ↓
Registrar pago
     ↓
Registrar productos
```

Todo debería quedar consistente.

---

# ⚡ 24. CACHE

Guarda temporalmente información para evitar repetir operaciones costosas.

```php
Cache::remember(
    'productos',
    3600,
    fn () => Producto::all()
);
```

En lugar de consultar la base de datos constantemente.

### Recuerda

```text
Sin cache
↓
DB → DB → DB → DB

Con cache
↓
Cache → Cache → Cache
```

---

# 📨 25. JOB

Un Job permite ejecutar tareas fuera de la petición principal.

Ejemplo:

```text
Usuario
   ↓
Realiza pedido
   ↓
Respuesta inmediata
   ↓
Job
   ↓
Enviar correo
```

Crear:

```bash
php artisan make:job EnviarFactura
```

---

# 🐇 26. QUEUE

La Queue ejecuta Jobs pendientes.

```text
Controller
    ↓
Job
    ↓
Queue
    ↓
Worker
    ↓
Procesamiento
```

Worker:

```bash
php artisan queue:work
```

Ideal para:

* emails
* procesamiento de imágenes
* generación de reportes
* importaciones
* tareas pesadas

---

# 👀 27. OBSERVER

Permite reaccionar cuando ocurre algo con un modelo.

Ejemplo:

```text
Usuario creado
      ↓
Observer
      ↓
Crear perfil
```

Crear:

```bash
php artisan make:observer UserObserver --model=User
```

---

# 📡 28. EVENT

Un Event anuncia que algo ocurrió.

```text
PedidoCreado
```

Luego diferentes Listeners pueden reaccionar.

```text
PedidoCreado
   ├── EnviarEmail
   ├── ActualizarEstadísticas
   └── NotificarUsuario
```

### Fácil

```text
Event = "algo ocurrió"

Listener = "cuando ocurra, hago esto"
```

---

# 🧩 29. SERVICE

Un Service contiene lógica de negocio que no debería estar en el Controller.

Ejemplo:

```php
class CrearPedidoService
{
    public function ejecutar(array $data)
    {
        // lógica del pedido
    }
}
```

Controller:

```php
public function store(StorePedidoRequest $request)
{
    return $this->service->ejecutar(
        $request->validated()
    );
}
```

### Recuerda

> **Controller recibe y coordina.**
> **Service ejecuta lógica de negocio.**

---

# 🧪 30. TEST

Sirve para comprobar automáticamente que tu aplicación funciona.

Ejemplo:

```php
$this->assertTrue(
    User::where('email', $email)->exists()
);
```

Tipos comunes:

```text
Unit Test
Feature Test
```

### Fácil

```text
Unit
= pruebo una pieza

Feature
= pruebo una funcionalidad completa
```

---

# 🐛 31. TINKER

Tinker es una consola interactiva de Laravel.

```bash
php artisan tinker
```

Puedes probar:

```php
User::count();
```

```php
User::find(1);
```

```php
Producto::where('stock', '>', 0)->get();
```

### Recuerda

> **Tinker = laboratorio de Laravel.**

---

# 🛠️ 32. ARTISAN

Artisan es la consola de Laravel.

```bash
php artisan
```

Crear cosas:

```bash
php artisan make:model Producto
php artisan make:controller ProductoController
php artisan make:migration create_productos_table
php artisan make:request StoreProductoRequest
```

Administrar:

```bash
php artisan migrate
php artisan db:seed
php artisan route:list
php artisan tinker
```

---

# 🧹 33. OPTIMIZE:CLEAR

Uno de los comandos más útiles cuando Laravel parece estar "loco":

```bash
php artisan optimize:clear
```

Limpia diferentes cachés.

Úsalo cuando:

* cambias `.env`
* cambias rutas
* cambias configuración
* Laravel parece estar usando información vieja

---

# 📁 34. STORAGE

Laravel utiliza:

```text
storage/app/
```

Para archivos.

Si utilizas:

```text
storage/app/public
```

puedes crear el enlace:

```bash
php artisan storage:link
```

---

# 🔥 35. LOS CONCEPTOS MÁS IMPORTANTES

Si estás aprendiendo Laravel, memoriza esta relación:

```text
ROUTE
  ↓
CONTROLLER
  ↓
REQUEST
  ↓
SERVICE
  ↓
MODEL
  ↓
ELOQUENT
  ↓
DATABASE
```

Y alrededor:

```text
Middleware → seguridad/filtros

Policy → permisos

Migration → estructura DB

Seeder → datos iniciales

Factory → datos falsos

Observer → reaccionar a cambios

Event → anunciar acontecimientos

Listener → reaccionar a eventos

Job → tarea

Queue → ejecutar tareas en segundo plano

Cache → evitar trabajo repetido

Test → comprobar que funciona
```

---

# 🧠 MAPA MENTAL FINAL

```text
                    LARAVEL
                       │
        ┌──────────────┼──────────────┐
        ↓              ↓              ↓
      ROUTES         MVC          ARTISAN
                       │
             ┌─────────┼─────────┐
             ↓         ↓         ↓
        CONTROLLER    MODEL      VIEW
             │         │
             ↓         ↓
          REQUEST    ELOQUENT
             │         │
             ↓         ↓
        VALIDACIÓN   DATABASE
             │
             ↓
        AUTHORIZATION
             │
          POLICY
```

---

# 🏆 REGLA PARA RECORDAR TODO

Piensa en Laravel como un restaurante:

```text
Cliente
   ↓
Route
"Quiero hacer un pedido"

   ↓

Controller
"Yo coordino el pedido"

   ↓

Request
"¿Los datos están correctos?"

   ↓

Policy
"¿Este cliente tiene permiso?"

   ↓

Service
"Yo ejecuto la lógica del pedido"

   ↓

Model / Eloquent
"Yo hablo con la cocina"

   ↓

Database
"Guardo la información"

   ↓

Job / Queue
"Las tareas pesadas las hago después"

   ↓

View / API
"Le entrego el resultado al cliente"
```

## 🎯 Si entiendes este flujo, ya tienes una buena base de Laravel.

No necesitas memorizar todo de inmediato. Primero entiende **qué problema resuelve cada concepto** y después aprende su sintaxis.
