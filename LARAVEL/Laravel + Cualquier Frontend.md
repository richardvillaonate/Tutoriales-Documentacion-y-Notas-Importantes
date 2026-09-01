# 🚀 Laravel + Cualquier Frontend

## Guía rápida de Blade, React, Next.js, Vue, Tailwind, Inertia y Filament

---

# 1. 🧠 PRIMERO: ENTIENDE LAS 3 FORMAS

Antes de instalar nada, debes decidir **cómo Laravel y el frontend se van a comunicar**.

---

## 🟢 OPCIÓN A — Laravel + Blade

Laravel controla:

```text
Frontend
+
Backend
+
Base de datos
```

```text
Navegador
    ↓
Laravel Route
    ↓
Controller
    ↓
Eloquent
    ↓
MySQL
    ↓
Blade
    ↓
HTML
```

Ejemplo:

```php
Route::get('/productos', [ProductoController::class, 'index']);
```

Controller:

```php
public function index()
{
    $productos = Producto::all();

    return view('productos.index', compact('productos'));
}
```

Blade:

```blade
@foreach ($productos as $producto)
    <h2>{{ $producto->nombre }}</h2>
@endforeach
```

### Cuándo usarlo

Excelente para:

* páginas tradicionales
* sistemas administrativos sencillos
* aplicaciones CRUD
* proyectos donde quieres simplicidad
* Laravel puro

---

# 2. 🟢 OPCIÓN B — Laravel + Inertia + React/Vue

Aquí Laravel sigue siendo el backend, pero React o Vue se encargan de la interfaz.

```text
Navegador
    ↓
React / Vue
    ↓
Inertia
    ↓
Laravel
    ↓
Eloquent
    ↓
MySQL
```

No necesitas crear una API REST completa para cada pantalla.

---

# 3. 🔵 OPCIÓN C — Laravel API + React / Next.js / Vue

Aquí tienes dos aplicaciones separadas.

```text
              INTERNET
                  │
        ┌─────────┴─────────┐
        ↓                   ↓
     FRONTEND             LARAVEL
 React / Next / Vue          API
        │                    │
        └────── HTTP ────────┘
                  │
                  ↓
                MySQL
```

Laravel devuelve:

```json
{
    "id": 1,
    "nombre": "Laptop",
    "precio": 2500000
}
```

El frontend consume esos datos.

---

# 4. 🧠 ¿CUÁL ELEGIR?

| Frontend      | Comunicación     | Ideal para                   |
| ------------- | ---------------- | ---------------------------- |
| Blade         | Laravel directo  | Apps tradicionales           |
| React         | Inertia o API    | SPA / aplicaciones dinámicas |
| Vue           | Inertia o API    | SPA / dashboards             |
| Next.js       | API              | Frontend separado / SSR      |
| Filament      | Laravel/Eloquent | Panel administrativo         |
| Tailwind      | CSS              | Diseño visual                |
| React + Next  | API              | Aplicaciones grandes         |
| Vue + Laravel | Inertia/API      | Sistemas empresariales       |

---

# 5. 🟩 BLADE

## Instalar Laravel

```bash
laravel new mi-proyecto
```

O:

```bash
composer create-project laravel/laravel mi-proyecto
```

Entrar:

```bash
cd mi-proyecto
```

Instalar frontend:

```bash
npm install
```

Ejecutar:

```bash
npm run dev
```

Laravel:

```bash
php artisan serve
```

---

## Crear vista

```text
resources/views/productos/index.blade.php
```

```blade
<h1>Productos</h1>

@foreach ($productos as $producto)

    <div>
        <h2>{{ $producto->nombre }}</h2>
        <p>{{ $producto->precio }}</p>
    </div>

@endforeach
```

---

# 6. 🟦 TAILWIND CSS

Tailwind no es un frontend como React o Vue.

Es un **framework CSS**.

Puedes utilizarlo con:

```text
Blade
React
Vue
Next.js
Filament
```

En Laravel moderno normalmente puedes instalarlo según la versión/documentación del proyecto.

Conceptualmente:

```bash
npm install
```

y luego:

```bash
npm run dev
```

Ejemplo:

```html
<div class="p-6 rounded-xl shadow">
    <h1 class="text-2xl font-bold">
        Productos
    </h1>
</div>
```

### Importante

Tailwind no comunica datos.

```text
Laravel
   ↓
Datos

React/Vue/Blade
   ↓
Interfaz

Tailwind
   ↓
Diseño
```

---

# 7. ⚛️ REACT + LARAVEL

Tienes dos posibilidades.

## Opción 1

```text
Laravel
+
Inertia
+
React
```

## Opción 2

```text
Laravel API
+
React separado
```

---

# 8. ⚛️ REACT + INERTIA

Instala Inertia para Laravel según la documentación/versiones actuales.

La idea es:

```text
Laravel
 ↓
Inertia
 ↓
React
```

Tu componente React puede recibir:

```jsx
export default function Productos({ productos }) {

    return (
        <div>
            {productos.map(producto => (
                <div key={producto.id}>
                    {producto.nombre}
                </div>
            ))}
        </div>
    );
}
```

Laravel:

```php
return Inertia::render('Productos/Index', [
    'productos' => Producto::all(),
]);
```

### Resultado

Laravel obtiene:

```text
MySQL
 ↓
Eloquent
 ↓
Controller
 ↓
Inertia
 ↓
React
```

---

# 9. ⚛️ REACT + API

Si React está separado:

```text
React
   ↓
HTTP
   ↓
Laravel API
   ↓
Controller
   ↓
Eloquent
   ↓
MySQL
```

Laravel:

```php
Route::get('/productos', function () {

    return Producto::all();

});
```

React:

```jsx
fetch('http://localhost:8000/api/productos')
    .then(response => response.json())
    .then(data => {
        console.log(data);
    });
```

Una opción común es usar Axios:

```bash
npm install axios
```

Después:

```jsx
axios.get('/api/productos')
    .then(response => {
        console.log(response.data);
    });
```

---

# 10. 🟦 VUE + LARAVEL

Exactamente el mismo concepto.

Puedes utilizar:

```text
Laravel
+
Inertia
+
Vue
```

o:

```text
Laravel API
+
Vue
```

---

# 11. 🟦 VUE + INERTIA

Laravel:

```php
return Inertia::render('Productos/Index', [
    'productos' => Producto::all(),
]);
```

Vue:

```vue
<script setup>

defineProps({
    productos: Array
})

</script>

<template>

    <div v-for="producto in productos" :key="producto.id">

        <h2>
            {{ producto.nombre }}
        </h2>

        <p>
            {{ producto.precio }}
        </p>

    </div>

</template>
```

---

# 12. 🟦 VUE + API

Vue:

```javascript
import axios from 'axios'

axios.get('http://localhost:8000/api/productos')
    .then(response => {

        console.log(response.data)

    })
```

Laravel:

```php
Route::get('/productos', function () {

    return Producto::all();

});
```

---

# 13. ▲ NEXT.JS + LARAVEL

Aquí recomiendo pensar en:

```text
Next.js
   ↓
HTTP
   ↓
Laravel API
   ↓
MySQL
```

Next.js es el frontend.

Laravel es el backend.

---

# 14. CREAR NEXT.JS

Desde una carpeta independiente:

```bash
npx create-next-app@latest frontend
```

Entrar:

```bash
cd frontend
```

Instalar:

```bash
npm install
```

Ejecutar:

```bash
npm run dev
```

Normalmente tendrás:

```text
Next.js
http://localhost:3000
```

Laravel:

```text
http://localhost:8000
```

---

# 15. NEXT.JS CONSUMIENDO LARAVEL

Laravel:

```php
Route::get('/productos', function () {

    return Producto::all();

});
```

Next:

```javascript
const response = await fetch(
    'http://localhost:8000/api/productos'
);

const productos = await response.json();
```

Después:

```jsx
export default async function Productos() {

    const response = await fetch(
        'http://localhost:8000/api/productos'
    );

    const productos = await response.json();

    return (
        <div>

            {productos.map(producto => (
                <div key={producto.id}>
                    {producto.nombre}
                </div>
            ))}

        </div>
    );
}
```

---

# 16. 🌐 CORS

Cuando tienes:

```text
Next.js
localhost:3000

Laravel
localhost:8000
```

son diferentes orígenes.

Puede aparecer:

```text
CORS error
```

Laravel debe permitir las solicitudes necesarias desde tu frontend.

### Concepto

```text
Frontend
http://localhost:3000
       ↓
      CORS
       ↓
Laravel
http://localhost:8000
```

No debes simplemente permitir cualquier origen en producción.

---

# 17. 🔐 AUTENTICACIÓN

Si tienes:

```text
React
Next.js
Vue
```

y Laravel como API, necesitas resolver:

```text
Login
 ↓
Autenticación
 ↓
Token / Cookie
 ↓
Requests protegidas
```

Para aplicaciones SPA con Laravel, **Laravel Sanctum** es una opción habitual.

Conceptualmente:

```text
Usuario
 ↓
Login
 ↓
Laravel
 ↓
Sesión / Cookie
 ↓
Frontend
```

Para APIs con clientes independientes también puedes utilizar esquemas basados en tokens según la arquitectura.

---

# 18. 📦 API RESOURCE

No devuelvas necesariamente tus modelos directamente:

```php
return Producto::all();
```

Para APIs grandes puedes utilizar API Resources.

Crear:

```bash
php artisan make:resource ProductoResource
```

Después:

```php
return ProductoResource::collection(
    Producto::all()
);
```

Puedes controlar exactamente qué devuelve Laravel:

```php
return [
    'id' => $this->id,
    'nombre' => $this->nombre,
    'precio' => $this->precio,
];
```

---

# 19. 📡 API REST

Una API típica:

```text
GET
/api/productos

POST
/api/productos

GET
/api/productos/1

PUT
/api/productos/1

DELETE
/api/productos/1
```

Laravel:

```php
Route::apiResource(
    'productos',
    ProductoController::class
);
```

---

# 20. 📤 CREAR DATOS DESDE REACT

React:

```javascript
await axios.post(
    'http://localhost:8000/api/productos',
    {
        nombre: 'Laptop',
        precio: 2500000
    }
);
```

Laravel:

```php
public function store(StoreProductoRequest $request)
{
    $producto = Producto::create(
        $request->validated()
    );

    return response()->json(
        $producto,
        201
    );
}
```

Flujo:

```text
React
 ↓
POST JSON
 ↓
Laravel
 ↓
Request
 ↓
Validation
 ↓
Controller
 ↓
Eloquent
 ↓
MySQL
```

---

# 21. 📥 ACTUALIZAR

Frontend:

```javascript
axios.put(
    `/api/productos/${id}`,
    {
        nombre: 'Laptop Gamer',
        precio: 3500000
    }
);
```

Laravel:

```php
public function update(
    StoreProductoRequest $request,
    Producto $producto
) {

    $producto->update(
        $request->validated()
    );

    return $producto;
}
```

---

# 22. 🗑️ ELIMINAR

Frontend:

```javascript
axios.delete(
    `/api/productos/${id}`
);
```

Laravel:

```php
public function destroy(Producto $producto)
{
    $producto->delete();

    return response()->noContent();
}
```

---

# 23. 🟣 FILAMENT

Filament es diferente.

No es simplemente:

```text
React
Vue
Next
```

Filament está construido sobre Laravel y se utiliza principalmente para crear:

* paneles administrativos
* CRUD
* dashboards
* gestión de usuarios
* gestión de productos
* formularios
* tablas

Arquitectura:

```text
Laravel
  ↓
Eloquent
  ↓
Filament
  ↓
Admin Panel
```

---

# 24. INSTALAR FILAMENT

La instalación depende de la versión de Filament y Laravel.

El flujo general es:

```bash
composer require filament/filament
```

Después ejecutas el instalador correspondiente a la versión instalada.

Por ejemplo, en versiones modernas:

```bash
php artisan filament:install --panels
```

Luego puedes crear un usuario administrativo mediante el comando que proporciona tu versión de Filament.

---

# 25. CREAR RESOURCE EN FILAMENT

Conceptualmente:

```bash
php artisan make:filament-resource Producto
```

Filament genera la estructura necesaria para administrar:

```text
Productos
 ├── Listar
 ├── Crear
 ├── Editar
 └── Eliminar
```

Y utiliza tu modelo:

```php
Producto
```

---

# 26. FILAMENT + ELOQUENT

Si tienes:

```php
class Producto extends Model
{
    protected $fillable = [
        'nombre',
        'precio',
        'stock',
    ];
}
```

Filament puede trabajar directamente con ese modelo.

```text
Filament
   ↓
Producto Resource
   ↓
Producto Model
   ↓
Eloquent
   ↓
MySQL
```

---

# 27. 🧩 PUEDES COMBINARLOS

No estás obligado a elegir solamente uno.

Una aplicación puede ser:

```text
                    LARAVEL
                       │
        ┌──────────────┼──────────────┐
        ↓              ↓              ↓
    FILAMENT         API           BLADE
        ↓              ↓
      ADMIN       Next.js
                     ↓
                   React
```

Por ejemplo:

```text
/administracion
       ↓
   Filament

/api/*
       ↓
    Laravel API
       ↓
   Next.js

/
       ↓
    Blade
```

Esto es completamente válido.

---

# 28. 🏗️ EJEMPLO DE ARQUITECTURA GRANDE

Supongamos un sistema empresarial:

```text
                    INTERNET
                       │
             ┌─────────┴─────────┐
             ↓                   ↓
          Next.js             Laravel
             │                   │
             │                  API
             │                   │
             └────── HTTP ──────┘
                                 │
                              Eloquent
                                 │
                ┌────────────────┼───────────────┐
                ↓                ↓               ↓
              MySQL            Redis           Queue
```

Y administración:

```text
/admin
   ↓
Filament
   ↓
Laravel
```

---

# 29. 🧠 ¿DÓNDE ESTÁ LA LÓGICA?

Una buena separación:

```text
FRONTEND
↓
Interfaz
Estado
UX
Validaciones visuales

LARAVEL
↓
Reglas de negocio
Autorización
Validación real
Base de datos
Seguridad
API

MYSQL
↓
Persistencia
```

### Regla importante

Nunca confíes exclusivamente en las validaciones del frontend.

Si React dice:

```text
precio = 100
```

Laravel debe volver a validar.

---

# 30. 📦 ESTRUCTURA RECOMENDADA

## Laravel

```text
app/
├── Models/
├── Http/
│   ├── Controllers/
│   ├── Requests/
│   └── Resources/
├── Services/
└── Policies/

routes/
├── web.php
└── api.php
```

---

## React / Next

```text
frontend/
├── app/
├── components/
├── services/
├── hooks/
├── lib/
└── types/
```

---

## Vue

```text
frontend/
├── components/
├── views/
├── composables/
├── services/
└── stores/
```

---

# 31. 🔄 PATRÓN DE COMUNICACIÓN

Memoriza este flujo:

```text
FRONTEND
   │
   │ HTTP
   ↓
ROUTE
   │
   ↓
CONTROLLER
   │
   ↓
REQUEST
   │
   ↓
SERVICE
   │
   ↓
MODEL
   │
   ↓
ELOQUENT
   │
   ↓
DATABASE
```

Respuesta:

```text
DATABASE
   ↓
ELOQUENT
   ↓
RESOURCE
   ↓
CONTROLLER
   ↓
JSON
   ↓
HTTP
   ↓
FRONTEND
```

---

# 32. 🧠 ¿CUÁNDO USAR CADA UNO?

## Blade

Usa Blade cuando:

```text
No necesitas separar frontend/backend
```

Ideal:

```text
CRUD
Web tradicional
Sistemas internos
Proyectos Laravel sencillos
```

---

## React

Usa React cuando necesitas:

```text
Interfaces muy interactivas
Componentes complejos
SPA
Estado frontend importante
```

Puede funcionar con:

```text
Inertia
```

o:

```text
API
```

---

## Vue

Similar a React:

```text
SPA
Dashboards
Interfaces dinámicas
Componentes
```

Muy cómodo con Inertia.

---

## Next.js

Úsalo cuando quieres un frontend React con capacidades adicionales como:

```text
SSR
Routing
Server Components
SEO
Aplicaciones frontend independientes
```

Laravel puede funcionar como:

```text
Backend API
```

---

## Tailwind

No reemplaza React/Vue/Blade.

Es:

```text
CSS
```

Puedes usar:

```text
Tailwind + Blade
Tailwind + React
Tailwind + Vue
Tailwind + Next
Tailwind + Filament
```

---

## Filament

Ideal para:

```text
Panel administrativo
CRUD
Backoffice
Dashboards
Gestión interna
```

---

# 33. 🏆 TABLA DEFINITIVA

| Necesidad            | Recomendación    |
| -------------------- | ---------------- |
| Laravel sencillo     | Blade            |
| CRUD rápido          | Blade / Filament |
| Panel administrativo | Filament         |
| SPA                  | React / Vue      |
| Laravel + SPA        | Inertia          |
| React independiente  | React + API      |
| React + SSR/SEO      | Next.js + API    |
| Vue independiente    | Vue + API        |
| Diseño               | Tailwind         |
| Backend              | Laravel          |
| ORM                  | Eloquent         |
| API                  | Laravel          |
| Base de datos        | MySQL/PostgreSQL |
| Cache                | Redis            |
| Procesos pesados     | Queue            |

---

# 🔥 34. COMANDOS QUE DEBES CONOCER

## Laravel

```bash
php artisan serve

php artisan make:model Producto -mfsc

php artisan make:controller ProductoController --api

php artisan make:request StoreProductoRequest

php artisan make:resource ProductoResource

php artisan make:policy ProductoPolicy

php artisan migrate

php artisan db:seed

php artisan route:list

php artisan tinker

php artisan optimize:clear
```

---

# ⚛️ React

```bash
npm install

npm run dev

npm run build
```

Crear proyecto:

```bash
npx create-vite@latest frontend
```

---

# ▲ Next.js

```bash
npx create-next-app@latest frontend

npm install

npm run dev

npm run build
```

---

# 🟦 Vue

Con Vite:

```bash
npm create vite@latest frontend
```

Después:

```bash
npm install

npm run dev

npm run build
```

---

# 🎨 Tailwind

La instalación exacta depende de la versión de Tailwind y del stack.

Después de instalarlo:

```bash
npm run dev
```

Y utilizas clases:

```html
<div class="p-6 text-xl font-bold">
    Hola Laravel
</div>
```

---

# 🟣 Filament

Instalación general:

```bash
composer require filament/filament
```

Después, según la versión:

```bash
php artisan filament:install --panels
```

Resource:

```bash
php artisan make:filament-resource Producto
```

---

# 🧠 35. ERROR COMÚN

No pienses:

> "Laravel necesita React."

Ni:

> "Laravel necesita Vue."

Laravel puede funcionar perfectamente con:

```text
Blade
```

O:

```text
React
```

O:

```text
Vue
```

O:

```text
Next.js
```

O:

```text
Filament
```

La pregunta correcta es:

> **¿Cómo quiero que se comunique el frontend con Laravel?**

---

# 🎯 36. LAS DOS ARQUITECTURAS QUE DEBES DOMINAR

## Arquitectura 1 — Monolito moderno

```text
Laravel
 │
 ├── Blade
 ├── React + Inertia
 ├── Vue + Inertia
 └── Filament
 │
 ↓
Eloquent
 │
 ↓
MySQL
```

Ventaja:

> Un solo proyecto.

---

## Arquitectura 2 — Frontend separado

```text
┌───────────────┐
│ React/Next/Vue│
└───────┬───────┘
        │
       HTTP
        │
        ↓
┌───────────────┐
│    Laravel    │
│      API      │
└───────┬───────┘
        │
     Eloquent
        │
        ↓
      MySQL
```

Ventaja:

> Frontend y backend completamente independientes.

---

# 🚀 37. REGLA DE ORO PARA SER FULL STACK

No necesitas aprender:

```text
Laravel + React + Vue + Next + Angular
```

todos al mismo tiempo.

Primero domina:

```text
Laravel
   ↓
HTTP
   ↓
REST API
   ↓
JSON
   ↓
React o Vue
```

Después entenderás fácilmente los demás.

La verdadera habilidad Full Stack es entender:

```text
Frontend
   ↕
HTTP
   ↕
API
   ↕
Backend
   ↕
Database
```

Una vez entiendes ese flujo, **cambiar React por Vue, Vue por Next.js o Blade por otro frontend es principalmente aprender la sintaxis y herramientas específicas**, no volver a aprender toda la arquitectura.
