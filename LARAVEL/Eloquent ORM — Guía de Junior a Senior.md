# Eloquent ORM — Guía de Junior a Senior

> Guía práctica para dominar Eloquent ORM en Laravel, desde operaciones básicas hasta optimización, arquitectura y buenas prácticas.

---

# 🟢 NIVEL JUNIOR

## 1. Entender qué es Eloquent

Eloquent es el ORM de Laravel que permite trabajar con bases de datos utilizando **modelos PHP** en lugar de escribir SQL constantemente.

Ejemplo:

```php
$usuarios = Usuario::all();
```

En lugar de:

```sql
SELECT * FROM usuarios;
```

### Debes dominar

* Modelos
* Tablas
* Primary Keys
* `$fillable`
* `$guarded`
* `$casts`
* `create()`
* `find()`
* `findOrFail()`
* `where()`
* `first()`
* `get()`
* `delete()`
* `update()`

---

# 2. Crear modelos

```bash
php artisan make:model Producto
```

Modelo:

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

### Consejo

Aprende a diferenciar:

```php
$fillable
```

de:

```php
$guarded
```

### Buena práctica

Preferiblemente:

```php
protected $fillable = [
    'nombre',
    'precio',
];
```

en lugar de:

```php
protected $guarded = [];
```

El segundo permite asignación masiva de prácticamente cualquier atributo.

---

# 3. Consultas básicas

### Obtener todos

```php
$productos = Producto::all();
```

### Buscar por ID

```php
$producto = Producto::find(1);
```

### Lanzar 404 si no existe

```php
$producto = Producto::findOrFail(1);
```

### WHERE

```php
$productos = Producto::where('activo', true)->get();
```

### Varias condiciones

```php
$productos = Producto::where('activo', true)
    ->where('stock', '>', 0)
    ->get();
```

### Primer resultado

```php
$producto = Producto::where('nombre', 'Laptop')->first();
```

---

# 4. Crear registros

```php
$producto = Producto::create([
    'nombre' => 'Laptop',
    'precio' => 2500000,
    'stock' => 10,
]);
```

También:

```php
$producto = new Producto();

$producto->nombre = 'Laptop';
$producto->precio = 2500000;
$producto->stock = 10;

$producto->save();
```

### Consejo

Conoce ambas formas.

`create()` es cómodo.

`new Model + save()` es útil cuando necesitas lógica antes de guardar.

---

# 5. Actualizar

```php
$producto = Producto::findOrFail($id);

$producto->update([
    'precio' => 2800000,
]);
```

También:

```php
$producto->precio = 2800000;
$producto->save();
```

---

# 6. Eliminar

```php
$producto->delete();
```

Eliminar directamente:

```php
Producto::destroy($id);
```

---

# 7. Relaciones

Debes dominar:

* `hasOne`
* `hasMany`
* `belongsTo`
* `belongsToMany`

Ejemplo:

```text
Usuario
   |
   | hasMany
   ↓
Pedidos
```

Modelo:

```php
class Usuario extends Model
{
    public function pedidos()
    {
        return $this->hasMany(Pedido::class);
    }
}
```

Pedido:

```php
class Pedido extends Model
{
    public function usuario()
    {
        return $this->belongsTo(Usuario::class);
    }
}
```

---

# 8. Consultar relaciones

```php
$usuario->pedidos;
```

También:

```php
$usuario->pedidos()->where('estado', 'pagado')->get();
```

### Diferencia importante

```php
$usuario->pedidos
```

ejecuta/usa la relación como propiedad.

Mientras:

```php
$usuario->pedidos()
```

te permite continuar construyendo la consulta.

---

# 🟡 NIVEL MID

# 9. Eager Loading

Uno de los conceptos más importantes de Eloquent.

Evita problemas de **N+1 queries**.

❌ Problema:

```php
$usuarios = Usuario::all();

foreach ($usuarios as $usuario) {
    echo $usuario->pedidos;
}
```

Puede generar:

```text
1 query usuarios
+
1 query por cada usuario
```

Mejor:

```php
$usuarios = Usuario::with('pedidos')->get();
```

Ahora Eloquent carga la relación anticipadamente.

---

# 10. Eager Loading múltiple

```php
$usuarios = Usuario::with([
    'pedidos',
    'perfil',
    'roles',
])->get();
```

Relaciones anidadas:

```php
$usuarios = Usuario::with(
    'pedidos.productos'
)->get();
```

---

# 11. Lazy Loading

```php
$usuario->load('pedidos');
```

Puedes cargar relaciones después de obtener el modelo.

---

# 12. `with()` vs `load()`

### `with()`

Antes de ejecutar la consulta:

```php
Usuario::with('pedidos')->get();
```

### `load()`

Después de tener el modelo:

```php
$usuario = Usuario::find($id);

$usuario->load('pedidos');
```

---

# 13. `whereHas()`

Muy importante para consultas relacionadas.

Ejemplo:

> Obtener usuarios que tengan pedidos pagados.

```php
$usuarios = Usuario::whereHas('pedidos', function ($query) {
    $query->where('estado', 'pagado');
})->get();
```

---

# 14. `withWhereHas()`

Puedes combinar filtrado y carga de relación:

```php
$usuarios = Usuario::withWhereHas('pedidos', function ($query) {
    $query->where('estado', 'pagado');
})->get();
```

---

# 15. `whereDoesntHave()`

Buscar registros que NO tengan una relación.

```php
$usuarios = Usuario::whereDoesntHave('pedidos')->get();
```

---

# 16. `withCount()`

Obtener cantidad de relaciones.

```php
$usuarios = Usuario::withCount('pedidos')->get();
```

Ahora:

```php
$usuario->pedidos_count;
```

También puedes filtrar:

```php
Usuario::withCount([
    'pedidos as pedidos_pagados_count' => function ($query) {
        $query->where('estado', 'pagado');
    }
])->get();
```

---

# 17. Agregaciones

```php
Producto::count();
```

```php
Producto::sum('precio');
```

```php
Producto::avg('precio');
```

```php
Producto::max('precio');
```

```php
Producto::min('precio');
```

---

# 18. `exists()`

Si solo necesitas saber si existe:

❌ Evita:

```php
Producto::where('codigo', $codigo)->first();
```

si únicamente necesitas saber si existe.

Mejor:

```php
Producto::where('codigo', $codigo)->exists();
```

---

# 19. `value()`

Si necesitas solamente un campo:

```php
$precio = Producto::where('id', $id)->value('precio');
```

Evita traer todo el modelo si no lo necesitas.

---

# 20. `pluck()`

Obtener únicamente determinados valores:

```php
$nombres = Usuario::pluck('nombre');
```

También:

```php
$nombres = Usuario::pluck('nombre', 'id');
```

---

# 21. Paginación

❌ Evita:

```php
$usuarios = Usuario::all();
```

cuando tienes miles de registros.

Mejor:

```php
$usuarios = Usuario::paginate(20);
```

También:

```php
$usuarios = Usuario::simplePaginate(20);
```

---

# 22. Scopes

Cuando una consulta se repite, crea un scope.

```php
public function scopeActivos($query)
{
    return $query->where('activo', true);
}
```

Uso:

```php
Usuario::activos()->get();
```

Esto mejora la reutilización y legibilidad.

---

# 23. Scopes dinámicos

```php
public function scopePorEstado($query, $estado)
{
    return $query->where('estado', $estado);
}
```

Uso:

```php
Pedido::porEstado('pagado')->get();
```

---

# 24. Casts

Para transformar automáticamente atributos:

```php
protected function casts(): array
{
    return [
        'activo' => 'boolean',
        'fecha' => 'datetime',
        'configuracion' => 'array',
    ];
}
```

Así:

```php
$modelo->activo
```

será tratado como boolean.

---

# 25. Accessors y Mutators

Puedes transformar valores al leerlos o asignarlos.

Ejemplo:

```php
protected function nombreCompleto(): Attribute
{
    return Attribute::make(
        get: fn ($value) => strtoupper($value),
    );
}
```

---

# 26. Soft Deletes

Modelo:

```php
use SoftDeletes;

class Usuario extends Model
{
    use SoftDeletes;
}
```

Migration:

```php
$table->softDeletes();
```

Eliminar:

```php
$usuario->delete();
```

El registro no desaparece físicamente.

---

# 27. Recuperar eliminados

```php
Usuario::withTrashed()->get();
```

Solo eliminados:

```php
Usuario::onlyTrashed()->get();
```

Restaurar:

```php
$usuario->restore();
```

---

# 🔴 NIVEL SENIOR

# 28. Entender qué SQL genera Eloquent

No basta con saber escribir:

```php
Usuario::where('activo', true)->get();
```

Debes entender el SQL que genera.

Puedes analizarlo con:

```php
$query = Usuario::where('activo', true);

dd($query->toSql(), $query->getBindings());
```

Esto permite detectar consultas ineficientes.

---

# 29. Eloquent ≠ siempre la mejor opción

Eloquent es excelente, pero no debes utilizarlo automáticamente para todo.

Para consultas extremadamente complejas o procesos masivos puede ser mejor:

```php
DB::table(...)
```

o incluso SQL.

Senior:

> No se trata de usar Eloquent para todo. Se trata de elegir la herramienta correcta.

---

# 30. Query Builder

Ejemplo:

```php
$usuarios = DB::table('usuarios')
    ->where('activo', true)
    ->get();
```

Útil cuando no necesitas comportamiento de un modelo Eloquent.

---

# 31. Seleccionar únicamente columnas necesarias

❌ Evita:

```php
Usuario::with('perfil')->get();
```

si solamente necesitas algunos campos.

Mejor:

```php
Usuario::select([
    'id',
    'nombre',
    'email',
])->get();
```

Relaciones:

```php
Usuario::with([
    'perfil:id,usuario_id,telefono'
])->get();
```

---

# 32. Índices de base de datos

Un desarrollador Senior entiende que Eloquent no puede solucionar por sí solo una base de datos mal diseñada.

Ejemplo:

```php
$table->index('email');
$table->index('estado');
$table->index('created_at');
```

Especialmente importante en columnas utilizadas frecuentemente en:

```php
where()
join()
orderBy()
```

---

# 33. Transacciones

Cuando varias operaciones deben ejecutarse como una sola unidad:

```php
DB::transaction(function () {

    $pedido = Pedido::create([
        'usuario_id' => 1,
        'total' => 100000,
    ]);

    $pedido->productos()->attach([
        1 => ['cantidad' => 2],
        2 => ['cantidad' => 1],
    ]);

});
```

Si ocurre un error, la transacción puede revertir las operaciones.

---

# 34. Procesos masivos

❌ Evita:

```php
$usuarios = Usuario::all();

foreach ($usuarios as $usuario) {
    // proceso
}
```

si tienes cientos de miles de registros.

Mejor:

```php
Usuario::chunkById(1000, function ($usuarios) {

    foreach ($usuarios as $usuario) {
        // proceso
    }

});
```

---

# 35. `cursor()`

Para recorrer grandes cantidades de registros utilizando menos memoria:

```php
foreach (Usuario::cursor() as $usuario) {
    // proceso
}
```

---

# 36. `lazyById()`

Muy útil para procesamientos grandes:

```php
Usuario::lazyById(1000)->each(function ($usuario) {

    // procesamiento

});
```

---

# 37. Relaciones polimórficas

Debes conocer:

```php
morphOne
morphMany
morphTo
morphToMany
morphedByMany
```

Ejemplo:

```text
Post ─────┐
          ├── comentarios
Video ────┘
```

Modelo:

```php
public function comentarios()
{
    return $this->morphMany(Comentario::class, 'comentable');
}
```

---

# 38. Relaciones many-to-many

Ejemplo:

```text
Usuarios
   ↕
Roles
```

Modelo:

```php
public function roles()
{
    return $this->belongsToMany(Rol::class);
}
```

Agregar:

```php
$usuario->roles()->attach($rolId);
```

Eliminar:

```php
$usuario->roles()->detach($rolId);
```

Sincronizar:

```php
$usuario->roles()->sync([1, 2, 3]);
```

---

# 39. Pivot

Puedes acceder a información adicional:

```php
$usuario->roles->first()->pivot->created_at;
```

Migration:

```php
$table->foreignId('usuario_id');
$table->foreignId('rol_id');
$table->date('asignado_en');
```

---

# 40. Eventos de Eloquent

Puedes reaccionar a eventos:

```text
creating
created
updating
updated
saving
saved
deleting
deleted
```

Ejemplo:

```php
protected static function booted()
{
    static::creating(function ($usuario) {
        $usuario->uuid = Str::uuid();
    });
}
```

### Cuidado

No metas demasiada lógica dentro de eventos porque puede hacer que el comportamiento del modelo sea difícil de rastrear.

---

# 41. Observers

Cuando los eventos crecen, utiliza Observers.

```bash
php artisan make:observer UsuarioObserver --model=Usuario
```

Esto permite separar responsabilidades.

---

# 42. Modelos con demasiada lógica

❌ Evita modelos gigantes:

```php
Usuario.php
```

con cientos o miles de líneas.

Si el modelo empieza a hacer:

```text
validaciones
pagos
emails
reportes
procesamiento de archivos
integraciones externas
```

probablemente estás mezclando responsabilidades.

---

# 43. Servicios

Puedes mover lógica de negocio a Services.

Ejemplo:

```php
class CrearPedidoService
{
    public function ejecutar(array $data)
    {
        return DB::transaction(function () use ($data) {

            // lógica

        });
    }
}
```

El controlador queda más limpio:

```php
public function store(Request $request)
{
    return $this->crearPedidoService->ejecutar(
        $request->validated()
    );
}
```

---

# 44. Repositorios: cuidado

No crees automáticamente:

```text
UsuarioRepository
ProductoRepository
PedidoRepository
```

solo porque alguien dijo que "todo proyecto Senior debe tener Repository Pattern".

Eloquent ya funciona como una capa de acceso a datos bastante potente.

Usa Repository cuando exista una razón real:

* múltiples fuentes de datos
* lógica de persistencia compleja
* necesidad de abstraer infraestructura
* arquitectura específica del proyecto

---

# 45. Evitar N+1

Uno de los errores que debes detectar inmediatamente:

```php
foreach ($pedidos as $pedido) {
    echo $pedido->usuario->nombre;
}
```

Si `usuario` no fue cargado previamente:

```php
$pedidos = Pedido::with('usuario')->get();
```

---

# 46. Lazy Loading Prevent

En proyectos grandes puedes detectar accidentalmente relaciones cargadas una por una.

Puedes habilitar:

```php
Model::preventLazyLoading();
```

Esto ayuda a descubrir problemas N+1 durante desarrollo.

---

# 47. Consultas condicionales

En lugar de:

```php
if ($request->estado) {
    $query->where('estado', $request->estado);
}
```

puedes usar:

```php
$query->when(
    $request->estado,
    fn ($query, $estado) =>
        $query->where('estado', $estado)
);
```

---

# 48. `when()` para filtros

Ejemplo:

```php
$usuarios = Usuario::query()
    ->when($request->nombre, function ($query, $nombre) {
        $query->where('nombre', 'like', "%{$nombre}%");
    })
    ->when($request->activo !== null, function ($query) use ($request) {
        $query->where('activo', $request->activo);
    })
    ->paginate(20);
```

Esto es excelente para sistemas con muchos filtros.

---

# 49. Query Objects

Cuando las consultas se vuelven demasiado complejas, puedes encapsularlas.

En lugar de tener:

```php
Controlador
```

con una consulta de 100 líneas:

```php
$query = ...
    ->where(...)
    ->whereHas(...)
    ->with(...)
    ->when(...)
    ->orderBy(...)
```

puedes crear una clase especializada.

Esto mejora:

* reutilización
* testing
* mantenimiento
* legibilidad

---

# 50. Testing de Eloquent

Un Senior debe probar sus consultas importantes.

Ejemplo:

```php
public function test_puede_obtener_usuarios_activos()
{
    Usuario::factory()->create([
        'activo' => true,
    ]);

    Usuario::factory()->create([
        'activo' => false,
    ]);

    $usuarios = Usuario::activos()->get();

    $this->assertCount(1, $usuarios);
}
```

---

# 51. Factories

Aprende a crear datos de prueba:

```bash
php artisan make:factory UsuarioFactory
```

Ejemplo:

```php
Usuario::factory()->count(50)->create();
```

Esto es fundamental para:

* testing
* desarrollo
* pruebas de rendimiento
* seeders

---

# 52. Seeders

```bash
php artisan db:seed
```

O:

```php
Usuario::factory(100)->create();
```

---

# 53. Mass Assignment

❌ Peligroso:

```php
Usuario::create($request->all());
```

Mejor:

```php
Usuario::create(
    $request->validated()
);
```

Y controla los campos mediante:

```php
$fillable
```

---

# 54. No confiar en el Frontend

Nunca asumas que porque un campo está oculto:

```html
<input type="hidden">
```

el usuario no puede modificarlo.

La seguridad debe estar en:

```text
Request validation
+
Authorization
+
Database constraints
```

---

# 55. Autorización

No basta con:

```php
$pedido = Pedido::findOrFail($id);
```

Debes verificar que el usuario tenga permiso para modificarlo.

Por ejemplo:

```php
$this->authorize('update', $pedido);
```

---

# 56. Eloquent y arquitectura

Un proyecto Senior debería separar claramente:

```text
HTTP
 ↓
Controller
 ↓
Request
 ↓
Service / Use Case
 ↓
Domain logic
 ↓
Eloquent / Database
```

No significa que todos los proyectos necesiten exactamente esta estructura.

La arquitectura debe responder a la complejidad real del proyecto.

---

# 🧠 COSAS QUE UN JUNIOR DEBE EVITAR

❌ `Model::all()` para tablas enormes.

❌ `request()->all()` sin validar.

❌ No utilizar `$fillable`.

❌ Ignorar relaciones.

❌ Crear consultas dentro de vistas.

❌ Repetir consultas idénticas.

❌ No entender qué SQL está generando Eloquent.

❌ Ignorar índices.

❌ Utilizar `first()` cuando realmente necesitas `exists()`.

❌ Traer un modelo completo cuando solo necesitas un campo.

---

# 🧠 COSAS QUE UN MID DEBE EVITAR

❌ N+1 queries.

❌ Controladores de 500 líneas.

❌ Modelos gigantes.

❌ Consultas duplicadas.

❌ Procesar 500.000 registros con `all()`.

❌ Usar Eloquent sin analizar rendimiento.

❌ Crear relaciones incorrectamente.

❌ Utilizar `with()` indiscriminadamente.

❌ Hacer `select *` cuando no es necesario.

---

# 🧠 COSAS QUE UN SENIOR DEBE EVITAR

❌ Sobrearquitectura.

❌ Crear Repository Pattern sin necesidad.

❌ Crear Services para cualquier método de 3 líneas.

❌ Utilizar Eloquent por obligación cuando Query Builder o SQL es mejor.

❌ Ignorar el diseño de la base de datos.

❌ Resolver problemas de rendimiento únicamente desde PHP.

❌ Hacer consultas complejas sin analizar índices.

❌ Ignorar transacciones.

❌ No medir rendimiento.

❌ Meter toda la lógica en Models.

❌ Meter toda la lógica en Controllers.

---

# 🏆 CHECKLIST DE DOMINIO DE ELOQUENT

## Junior

* [ ] Crear modelos
* [ ] Migraciones
* [ ] CRUD
* [ ] `$fillable`
* [ ] `$casts`
* [ ] `find`
* [ ] `where`
* [ ] `first`
* [ ] `get`
* [ ] Relaciones básicas
* [ ] Soft Deletes

## Mid

* [ ] Eager Loading
* [ ] N+1
* [ ] `whereHas`
* [ ] `withCount`
* [ ] Scopes
* [ ] Accessors / Mutators
* [ ] Paginación
* [ ] `when`
* [ ] Transactions
* [ ] Factories
* [ ] Observers
* [ ] Relaciones Many-to-Many
* [ ] Polymorphic Relations

## Senior

* [ ] Optimización de consultas
* [ ] Índices
* [ ] Query Builder
* [ ] SQL
* [ ] Transacciones
* [ ] Procesamiento masivo
* [ ] `chunkById`
* [ ] `cursor`
* [ ] Arquitectura
* [ ] Testing
* [ ] Performance
* [ ] Diseño de relaciones
* [ ] Seguridad
* [ ] Concurrencia
* [ ] Deadlocks
* [ ] Caching
* [ ] Jobs/Queues
* [ ] Observabilidad

---

# 🚀 RETOS PRÁCTICOS

## Reto Junior

Construye un CRUD de:

```text
Productos
```

Debe permitir:

```text
Crear
Listar
Editar
Eliminar
Buscar
Filtrar por estado
```

---

## Reto Mid

Construye:

```text
Usuarios
    ↓
Pedidos
    ↓
Productos
```

Implementa:

* relaciones
* filtros
* paginación
* eager loading
* `whereHas`
* `withCount`
* scopes

---

## Reto Senior

Construye un sistema:

```text
Empresa
   ↓
Sucursales
   ↓
Usuarios
   ↓
Pedidos
   ↓
Productos
```

Debe soportar:

* múltiples sucursales
* permisos
* filtros avanzados
* reportes
* grandes cantidades de registros
* procesamiento en background
* transacciones
* auditoría
* Soft Deletes
* índices
* testing
* optimización de consultas

Y debes poder responder:

> ¿Cuántas consultas ejecuta esta operación?

> ¿Qué índices necesita?

> ¿Dónde puede aparecer N+1?

> ¿Qué pasa si dos usuarios modifican el mismo registro?

> ¿Qué ocurre si falla la operación a mitad de camino?

> ¿Por qué utilizar Eloquent y no Query Builder?

> ¿Qué información debería estar cacheada?

---

# 🎯 REGLA DE ORO

La evolución con Eloquent no consiste en memorizar más métodos.

Consiste en pasar de:

```text
"Yo sé hacer consultas con Eloquent"
```

a:

```text
"Yo sé diseñar consultas eficientes,
seguras y mantenibles utilizando Eloquent
y sé cuándo NO utilizarlo."
```

### Junior

**¿Cómo hago la consulta?**

### Mid

**¿Cómo hago la consulta correctamente?**

### Senior

**¿Por qué esta consulta debe existir,
cuánto cuesta ejecutarla,
cómo escala y qué alternativa sería mejor?**

---

# 🔥 LOS 10 CONCEPTOS QUE MÁS DEBES DOMINAR

Si estás preparándote para trabajar como desarrollador Laravel, prioriza:

1. Relaciones Eloquent
2. Eager Loading
3. N+1
4. `whereHas`
5. Scopes
6. Transactions
7. Query Builder
8. Índices
9. Procesamiento masivo
10. Optimización SQL

> **Eloquent Senior no significa escribir consultas más complejas. Significa escribir consultas que sigan funcionando bien cuando la aplicación crezca.**
