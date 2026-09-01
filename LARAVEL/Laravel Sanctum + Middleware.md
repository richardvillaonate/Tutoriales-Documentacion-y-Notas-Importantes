# 🔐 Laravel Sanctum + Middleware

## Guía rápida de autenticación y protección de rutas

---

# 1. 🧠 ¿QUÉ ES SANCTUM?

**Laravel Sanctum** permite autenticar usuarios en aplicaciones Laravel.

Es especialmente útil para:

```text
Laravel + Blade
Laravel + React
Laravel + Vue
Laravel + Next.js
Aplicaciones SPA
APIs
```

Piensa en Sanctum como:

```text
Usuario
   ↓
LOGIN
   ↓
Sanctum
   ↓
Usuario autenticado
   ↓
Middleware
   ↓
Ruta protegida
```

---

# 2. 🧩 ¿QUÉ PROBLEMA RESUELVE?

Sin autenticación:

```http
GET /api/productos
```

Cualquiera podría acceder.

Con autenticación:

```text
Usuario
   ↓
Login
   ↓
Sanctum
   ↓
Cookie / Token
   ↓
Request
   ↓
auth:sanctum
   ↓
Controller
```

---

# 3. 📦 INSTALAR SANCTUM

En Laravel moderno:

```bash
composer require laravel/sanctum
```

Luego:

```bash
php artisan install:api
```

En versiones de Laravel donde el instalador no corresponda, debes seguir la instalación de Sanctum para esa versión concreta.

Después:

```bash
php artisan migrate
```

---

# 4. 🧱 MODELO USER

Tu modelo `User` debe utilizar:

```php
use Laravel\Sanctum\HasApiTokens;
```

Ejemplo:

```php
namespace App\Models;

use Illuminate\Foundation\Auth\User as Authenticatable;
use Laravel\Sanctum\HasApiTokens;

class User extends Authenticatable
{
    use HasApiTokens;

    protected $fillable = [
        'name',
        'email',
        'password',
    ];
}
```

### ¿Qué hace `HasApiTokens`?

Permite trabajar con:

```php
$user->createToken(...)
```

y:

```php
$user->tokens()
```

---

# 5. 🔑 LOGIN CON SANCTUM

Controller:

```php
use Illuminate\Http\Request;
use Illuminate\Support\Facades\Auth;

public function login(Request $request)
{
    $credentials = $request->validate([
        'email' => ['required', 'email'],
        'password' => ['required'],
    ]);

    if (!Auth::attempt($credentials)) {
        return response()->json([
            'message' => 'Credenciales incorrectas'
        ], 401);
    }

    $user = Auth::user();

    $token = $user->createToken('api-token')->plainTextToken;

    return response()->json([
        'user' => $user,
        'token' => $token,
    ]);
}
```

---

# 6. 🛣️ RUTA LOGIN

En:

```text
routes/api.php
```

```php
use App\Http\Controllers\AuthController;

Route::post('/login', [
    AuthController::class,
    'login'
]);
```

Ahora:

```http
POST /api/login
```

recibe:

```json
{
    "email": "richard@example.com",
    "password": "123456"
}
```

Y devuelve:

```json
{
    "user": {
        "id": 1,
        "name": "Richard"
    },
    "token": "1|xxxxxxxxxxxxxxxx"
}
```

---

# 7. 🛡️ PROTEGER UNA RUTA

Aquí aparece:

```php
auth:sanctum
```

Ejemplo:

```php
Route::middleware('auth:sanctum')->get(
    '/usuario',
    function (Request $request) {

        return $request->user();

    }
);
```

Ahora:

```http
GET /api/usuario
```

requiere autenticación.

---

# 8. 🔐 ENVIAR EL TOKEN

Desde React, Vue, Next.js, Postman, etc.:

```http
Authorization: Bearer TOKEN
```

Ejemplo:

```javascript
axios.get('/api/usuario', {
    headers: {
        Authorization: `Bearer ${token}`
    }
});
```

Laravel recibe:

```text
Authorization
      ↓
Bearer TOKEN
      ↓
Sanctum
      ↓
Usuario
```

---

# 9. 👤 OBTENER USUARIO AUTENTICADO

En Controller:

```php
$request->user();
```

También:

```php
auth()->user();
```

Ejemplo:

```php
public function profile(Request $request)
{
    return response()->json(
        $request->user()
    );
}
```

---

# 10. 🚪 LOGOUT

Puedes eliminar el token actual:

```php
$request->user()
    ->currentAccessToken()
    ->delete();
```

Controller:

```php
public function logout(Request $request)
{
    $request->user()
        ->currentAccessToken()
        ->delete();

    return response()->json([
        'message' => 'Sesión cerrada'
    ]);
}
```

---

# 11. 🗂️ ESTRUCTURA RECOMENDADA

Una API podría quedar:

```text
routes/
└── api.php

app/
├── Models/
│   └── User.php
│
├── Http/
│   ├── Controllers/
│   │   └── AuthController.php
│   │
│   ├── Middleware/
│   │
│   └── Requests/
│       └── LoginRequest.php
```

---

# 12. 🧱 ¿QUÉ ES UN MIDDLEWARE?

Un Middleware es una especie de **filtro** que se ejecuta antes de llegar al Controller.

Piensa:

```text
REQUEST
   ↓
Middleware
   ↓
Middleware
   ↓
Controller
   ↓
Response
```

Ejemplo:

```text
Usuario
   ↓
¿Está autenticado?
   ↓
Sí ─────→ Controller
   │
   No
   ↓
401
```

---

# 13. 🔐 MIDDLEWARE DE SANCTUM

Este:

```php
auth:sanctum
```

es un middleware de autenticación.

Ejemplo:

```php
Route::middleware('auth:sanctum')->group(function () {

    Route::get('/profile', [
        ProfileController::class,
        'index'
    ]);

    Route::get('/productos', [
        ProductoController::class,
        'index'
    ]);

});
```

Todo lo que esté dentro necesita autenticación.

---

# 14. 🛠️ CREAR TU PROPIO MIDDLEWARE

Comando:

```bash
php artisan make:middleware AdminMiddleware
```

Laravel crea:

```text
app/Http/Middleware/AdminMiddleware.php
```

---

# 15. ✏️ CREAR LA LÓGICA

Ejemplo:

```php
namespace App\Http\Middleware;

use Closure;
use Illuminate\Http\Request;

class AdminMiddleware
{
    public function handle(
        Request $request,
        Closure $next
    ) {

        if (!$request->user()?->is_admin) {

            return response()->json([
                'message' => 'No autorizado'
            ], 403);

        }

        return $next($request);
    }
}
```

El concepto importante:

```php
return $next($request);
```

significa:

> "El usuario pasó el filtro; continúa."

---

# 16. 🧠 FLUJO DEL MIDDLEWARE

```text
Request
   ↓
AdminMiddleware
   ↓
¿Es administrador?
   │
   ├── NO → 403
   │
   └── SÍ
        ↓
    Controller
```

---

# 17. 🏷️ ¿CÓMO SE REGISTRA?

En Laravel moderno, los aliases de middleware se configuran normalmente en:

```text
bootstrap/app.php
```

Ejemplo:

```php
use App\Http\Middleware\AdminMiddleware;
use Illuminate\Foundation\Configuration\Middleware;

->withMiddleware(function (
    Middleware $middleware
) {

    $middleware->alias([
        'admin' => AdminMiddleware::class,
    ]);

})
```

Entonces puedes utilizar:

```php
Route::middleware('admin')->group(function () {

    // rutas de administración

});
```

---

# 18. 🔥 SANCTUM + ADMIN

Puedes combinar middleware:

```php
Route::middleware([
    'auth:sanctum',
    'admin'
])->group(function () {

    Route::get('/admin/dashboard', [
        AdminController::class,
        'dashboard'
    ]);

});
```

Flujo:

```text
Request
   ↓
auth:sanctum
   ↓
¿Autenticado?
   ↓
admin
   ↓
¿Administrador?
   ↓
Controller
```

---

# 19. 🧩 MIDDLEWARE PARA ROLES

Supongamos:

```text
users
────────────
id
name
email
role
```

Valores:

```text
admin
editor
user
```

Middleware:

```php
public function handle(
    Request $request,
    Closure $next,
    string $role
) {

    if ($request->user()?->role !== $role) {

        return response()->json([
            'message' => 'No autorizado'
        ], 403);

    }

    return $next($request);
}
```

Alias:

```php
'role' => RoleMiddleware::class,
```

Utilización:

```php
Route::middleware([
    'auth:sanctum',
    'role:admin'
])->group(function () {

    // solamente admins

});
```

---

# 20. 🎯 MIDDLEWARE CON PARÁMETROS

Puedes crear:

```php
Route::middleware('role:admin')
```

o:

```php
Route::middleware('role:editor')
```

El valor:

```text
admin
```

llega al middleware como:

```php
string $role
```

---

# 21. 🧱 MIDDLEWARE GLOBAL

Hay middleware que se ejecutan para prácticamente todas las solicitudes.

Ejemplo conceptual:

```text
Request
   ↓
Global Middleware
   ↓
Route Middleware
   ↓
Controller
```

No debes convertir todos tus middleware en globales.

Un middleware de:

```text
autenticación
admin
roles
permisos
```

normalmente se aplica a las rutas que lo necesitan.

---

# 22. 🌐 MIDDLEWARE DE RUTA

Ejemplo:

```php
Route::middleware('auth:sanctum')
    ->get('/profile', function () {
        //
    });
```

También puedes agrupar:

```php
Route::middleware('auth:sanctum')->group(function () {

    Route::get('/profile', ...);

    Route::get('/productos', ...);

    Route::post('/productos', ...);

});
```

---

# 23. 🧠 DIFERENCIA 401 VS 403

Esto es MUY importante.

## 401

```text
No estás autenticado.
```

Ejemplo:

```text
No enviaste token.
```

---

## 403

```text
Sí estás autenticado,
pero no tienes permiso.
```

Ejemplo:

```text
Usuario normal
       ↓
AdminMiddleware
       ↓
403
```

Memoriza:

```text
401 = ¿Quién eres?

403 = Sé quién eres, pero no puedes.
```

---

# 24. 🔥 EJEMPLO COMPLETO

## Rutas

```php
Route::post('/login', [
    AuthController::class,
    'login'
]);

Route::middleware('auth:sanctum')->group(function () {

    Route::get('/me', [
        AuthController::class,
        'me'
    ]);

    Route::post('/logout', [
        AuthController::class,
        'logout'
    ]);

    Route::middleware('admin')->group(function () {

        Route::get('/admin/users', [
            AdminController::class,
            'users'
        ]);

    });

});
```

La arquitectura:

```text
                    API
                     │
                   Login
                     │
                     ↓
                  Sanctum
                     │
              ┌──────┴──────┐
              ↓             ↓
           Usuario        Token
              │
              ↓
        auth:sanctum
              │
              ↓
           admin
              │
              ↓
         Controller
              │
              ↓
           Eloquent
              │
              ↓
            MySQL
```

---

# 25. ⚛️ REACT + SANCTUM

React realiza:

```javascript
axios.post('/api/login', {
    email,
    password
});
```

Laravel responde:

```json
{
    "token": "1|xxxxxxxx"
}
```

React guarda el token según la estrategia de autenticación que hayas elegido.

Después:

```javascript
axios.get('/api/productos', {
    headers: {
        Authorization: `Bearer ${token}`
    }
});
```

Laravel:

```text
Request
 ↓
auth:sanctum
 ↓
Token válido
 ↓
Controller
 ↓
JSON
 ↓
React
```

---

# 26. 🌐 SPA CON COOKIE

Sanctum también tiene un flujo específico para SPA usando **cookies y sesión**, en lugar de manejar un token Bearer manualmente.

Conceptualmente:

```text
React
 ↓
/sanctum/csrf-cookie
 ↓
Laravel
 ↓
CSRF Cookie
 ↓
Login
 ↓
Session Cookie
 ↓
Requests
```

Este enfoque es especialmente interesante cuando el frontend y Laravel forman parte de la misma aplicación o están configurados como una SPA confiable.

No mezcles sin entender:

```text
Token Bearer
```

con:

```text
Autenticación SPA por cookies
```

Son mecanismos distintos.

---

# 27. 🔥 ¿DÓNDE SE GUARDA EL TOKEN?

Cuando haces:

```php
$user->createToken('api-token')
```

Sanctum guarda información del token en:

```text
personal_access_tokens
```

La tabla contiene información como:

```text
id
tokenable_type
tokenable_id
name
token
abilities
last_used_at
expires_at
created_at
updated_at
```

El token completo se entrega al cliente una vez.

---

# 28. 🛡️ ABILITIES

Puedes crear tokens con capacidades específicas:

```php
$token = $user->createToken(
    'api-token',
    ['productos:read']
)->plainTextToken;
```

Después puedes verificar capacidades desde el middleware/autorización correspondiente.

Conceptualmente:

```text
Token
 │
 ├── productos:read
 ├── productos:create
 └── productos:delete
```

Esto permite crear permisos más específicos que simplemente:

```text
admin / user
```

---

# 29. 🧠 MIDDLEWARE VS POLICY

No confundas:

### Middleware

Protege una ruta o grupo de rutas.

```text
¿Está autenticado?
¿Es admin?
¿Tiene determinado contexto?
```

### Policy

Controla si un usuario puede realizar una acción sobre un recurso concreto.

```text
¿Richard puede editar ESTE producto?
```

Ejemplo:

```php
$authorize->authorize('update', $producto);
```

Mentalidad:

```text
Middleware
↓
¿Puedes entrar aquí?

Policy
↓
¿Puedes hacer ESTA acción?
```

---

# 30. 🏆 BUENA ARQUITECTURA

Para una aplicación grande:

```text
Frontend
   ↓
API
   ↓
Sanctum
   ↓
Middleware
   ↓
Form Request
   ↓
Policy
   ↓
Service
   ↓
Eloquent
   ↓
Database
```

Cada capa tiene una responsabilidad.

---

# 31. ❌ COSAS QUE NO DEBES HACER

### ❌ No hagas esto

Guardar contraseñas manualmente:

```php
$password = $request->password;
```

y almacenarlas directamente.

Utiliza el hashing de Laravel.

---

### ❌ No confíes en el frontend

Esto NO es seguridad:

```javascript
if (user.isAdmin) {
    mostrarBotonEliminar();
}
```

Laravel también debe proteger:

```php
Route::middleware([
    'auth:sanctum',
    'admin'
]);
```

---

### ❌ No pongas toda la seguridad en un Controller

Evita:

```php
if (!$user) ...
if (!$admin) ...
if (!$permission) ...
if (!$owner) ...
```

en todos los Controllers.

Utiliza:

```text
Middleware
Policies
Gates
Form Requests
```

según corresponda.

---

# 32. 🧠 COMANDOS PARA MEMORIZAR

Instalar:

```bash
composer require laravel/sanctum
```

Instalación API:

```bash
php artisan install:api
```

Migraciones:

```bash
php artisan migrate
```

Crear middleware:

```bash
php artisan make:middleware AdminMiddleware
```

Crear Controller:

```bash
php artisan make:controller AuthController
```

Crear Request:

```bash
php artisan make:request LoginRequest
```

Ver rutas:

```bash
php artisan route:list
```

Limpiar cachés:

```bash
php artisan optimize:clear
```

---

# 🎯 CHULETA FINAL

```text
SANCTUM
│
├── Autenticación
│
├── Tokens
│
├── Cookies para SPA
│
└── API
```

```text
MIDDLEWARE
│
├── auth:sanctum
│
├── admin
│
├── role:admin
│
└── permisos
```

```text
POLICY
│
└── ¿Puede este usuario hacer esta acción
    sobre este recurso?
```

---

# 🚀 FLUJO QUE DEBES MEMORIZAR

```text
                    LOGIN
                      │
                      ↓
                   Sanctum
                      │
                      ↓
                Usuario autenticado
                      │
                      ↓
                  HTTP Request
                      │
                      ↓
                auth:sanctum
                      │
                 ¿Autenticado?
                   /       \
                 NO         SÍ
                 ↓           ↓
               401        Middleware
                              │
                         ¿Autorizado?
                          /        \
                        NO          SÍ
                        ↓            ↓
                       403       Controller
                                    │
                                  Policy
                                    │
                                  Service
                                    │
                                  Eloquent
                                    │
                                  MySQL
```

## 🏆 Regla de oro

**Sanctum autentica. Middleware filtra. Policy autoriza acciones sobre recursos. Controller coordina. Service concentra lógica de negocio compleja. Eloquent trabaja con los datos.**

Esa separación es la que te permite pasar de un Laravel básico a una arquitectura profesional.
