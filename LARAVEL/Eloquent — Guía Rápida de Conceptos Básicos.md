# ⚡ Eloquent — Guía Rápida de Conceptos Básicos

> Objetivo: entender rápidamente qué hace cada concepto y cuándo utilizarlo.

---

# 1. 🧱 MODELOS

Un **Model** representa una tabla de la base de datos.

```php
class Producto extends Model
{
}
```

Por defecto:

```text
Modelo: Producto
       ↓
Tabla: productos
```

Puedes hacer:

```php
Producto::all();
Producto::find(1);
Producto::create(...);
```

### 🧠 Recuerda

> **Model = representación de una tabla en PHP.**

---

# 2. 🗄️ TABLAS

La tabla es donde realmente se almacenan los datos.

Ejemplo:

```text
productos
────────────────────────
id
nombre
precio
stock
created_at
updated_at
```

El modelo:

```php
class Producto extends Model
{
}
```

se conecta automáticamente con:

```text
productos
```

### Si el nombre es diferente

Por ejemplo, si tu tabla se llama:

```text
inventario_productos
```

puedes indicarlo:

```php
class Producto extends Model
{
    protected $table = 'inventario_productos';
}
```

### 🧠 Recuerda

```text
Model
  ↓
Tabla
  ↓
Registros
```

---

# 3. 🔑 PRIMARY KEY

Es el identificador único de cada registro.

Normalmente Laravel utiliza:

```text
id
```

Ejemplo:

```text
id | nombre
---|---------
1  | Laptop
2  | Mouse
3  | Teclado
```

Entonces:

```php
Producto::find(2);
```

busca:

```text
id = 2
```

### Si tu PK no se llama `id`

Por ejemplo:

```text
codigo_producto
```

puedes configurar:

```php
protected $primaryKey = 'codigo_producto';
```

### 🧠 Recuerda

> **Primary Key = identificación única del registro.**

---

# 4. 🛡️ `$fillable`

Define qué campos pueden ser asignados masivamente.

Ejemplo:

```php
protected $fillable = [
    'nombre',
    'precio',
    'stock',
];
```

Entonces puedes hacer:

```php
Producto::create([
    'nombre' => 'Laptop',
    'precio' => 2500000,
    'stock' => 10,
]);
```

Laravel permite esos campos porque están en `$fillable`.

### 🔐 ¿Por qué es importante?

Evita que alguien pueda enviar campos que no debería modificar.

Por ejemplo:

```text
nombre
precio
stock
es_admin
```

No quieres que un usuario pueda enviar:

```json
{
    "es_admin": true
}
```

si ese campo no debería ser modificable.

### 🧠 Recuerda

> **`$fillable` = campos permitidos.**

---

# 5. 🚧 `$guarded`

Es lo contrario de `$fillable`.

Define los campos que NO pueden asignarse masivamente.

```php
protected $guarded = [
    'id',
];
```

Significa:

```text
Todo permitido
EXCEPTO
id
```

### ⚠️ Mucho cuidado

Esto:

```php
protected $guarded = [];
```

significa prácticamente:

```text
"Permito todo"
```

No es recomendable si no tienes claro qué datos pueden llegar.

### 🧠 Fácil

```text
$fillable
= permito estos campos

$guarded
= bloqueo estos campos
```

### Consejo

Para la mayoría de aplicaciones:

```php
protected $fillable = [
    'nombre',
    'precio',
    'stock',
];
```

es más explícito.

---

# 6. 🔄 `$casts`

Convierte automáticamente los valores de la base de datos al tipo que necesitas en PHP.

Ejemplo:

```php
protected function casts(): array
{
    return [
        'activo' => 'boolean',
        'precio' => 'decimal:2',
        'fecha' => 'datetime',
    ];
}
```

Entonces:

```php
$producto->activo;
```

se trata como:

```php
true
```

en lugar de:

```text
1
```

También puedes convertir JSON:

```php
protected function casts(): array
{
    return [
        'configuracion' => 'array',
    ];
}
```

Ahora puedes hacer:

```php
$producto->configuracion['color'];
```

### 🧠 Recuerda

> **`$casts` = convertir automáticamente los datos.**

---

# 7. ➕ `create()`

Crea un nuevo registro.

```php
Producto::create([
    'nombre' => 'Laptop',
    'precio' => 2500000,
    'stock' => 10,
]);
```

Se convierte conceptualmente en:

```sql
INSERT INTO productos (...)
VALUES (...);
```

### ⚠️ Necesitas `$fillable`

Por ejemplo:

```php
protected $fillable = [
    'nombre',
    'precio',
    'stock',
];
```

### 🧠 Recuerda

> **`create()` = crear un registro.**

---

# 8. 🔎 `find()`

Busca un registro por su Primary Key.

```php
$producto = Producto::find(10);
```

Busca:

```text
id = 10
```

### Si existe

Obtienes el modelo:

```php
$producto->nombre;
```

### Si NO existe

Obtienes:

```php
null
```

Por eso:

```php
$producto = Producto::find(999);
```

podría devolver:

```text
null
```

### 🧠 Recuerda

> **`find()` = buscar por ID.**

---

# 9. 🚨 `findOrFail()`

Hace prácticamente lo mismo que `find()`, pero si no encuentra el registro lanza una excepción.

```php
$producto = Producto::findOrFail(10);
```

Si existe:

```text
Producto ✓
```

Si no existe:

```text
404 Not Found
```

Esto es muy útil en Controllers.

```php
public function show($id)
{
    $producto = Producto::findOrFail($id);

    return view('productos.show', compact('producto'));
}
```

### 🧠 Diferencia

```text
find()
   ↓
No existe → null

findOrFail()
   ↓
No existe → excepción / 404
```

### Consejo

Cuando una página depende de que el registro exista:

> Normalmente `findOrFail()` es más cómodo.

---

# 10. 🔍 `where()`

Filtra registros.

```php
$productos = Producto::where(
    'activo',
    true
)->get();
```

Conceptualmente:

```sql
SELECT *
FROM productos
WHERE activo = 1;
```

Puedes hacer:

```php
Producto::where('precio', '>', 1000000)->get();
```

También:

```php
Producto::where('stock', '>', 0)
    ->where('activo', true)
    ->get();
```

### 🧠 Recuerda

> **`where()` = poner condiciones.**

---

# 11. 🎯 `first()`

Obtiene el primer resultado de una consulta.

```php
$producto = Producto::where(
    'codigo',
    'ABC123'
)->first();
```

Si encuentra:

```text
Producto
```

Si no:

```text
null
```

### Diferencia con `get()`

```php
first()
```

devuelve:

```text
UN modelo
```

Mientras:

```php
get()
```

devuelve:

```text
UNA colección
```

### Ejemplo

```php
Producto::where('activo', true)->first();
```

→ Primer producto activo.

```php
Producto::where('activo', true)->get();
```

→ Todos los productos activos.

### 🧠 Recuerda

> **`first()` = dame el primero.**

---

# 12. 📦 `get()`

Ejecuta la consulta y obtiene todos los resultados encontrados.

```php
$productos = Producto::where(
    'activo',
    true
)->get();
```

Resultado:

```text
Collection
 ├── Producto
 ├── Producto
 ├── Producto
 └── Producto
```

Puedes recorrerlos:

```php
foreach ($productos as $producto) {
    echo $producto->nombre;
}
```

### 🧠 Recuerda

> **`get()` = dame todos los resultados encontrados.**

---

# 13. 🗑️ `delete()`

Elimina un registro.

```php
$producto = Producto::findOrFail($id);

$producto->delete();
```

Conceptualmente:

```sql
DELETE FROM productos
WHERE id = ...;
```

### También puedes hacer

```php
Producto::destroy($id);
```

### ⚠️ Importante

Si utilizas:

```php
use SoftDeletes;
```

`delete()` normalmente no elimina físicamente el registro.

Lo marca con:

```text
deleted_at
```

---

# 14. ✏️ `update()`

Modifica un registro existente.

```php
$producto = Producto::findOrFail($id);

$producto->update([
    'nombre' => 'Laptop Gamer',
    'precio' => 3500000,
]);
```

Conceptualmente:

```sql
UPDATE productos
SET nombre = ...,
    precio = ...
WHERE id = ...;
```

### ⚠️ Recuerda `$fillable`

Si haces:

```php
$producto->update([
    'precio' => 3500000,
]);
```

el campo debe estar permitido para asignación masiva.

---

# 🧠 RESUMEN ULTRA RÁPIDO

| Concepto       | ¿Qué hace?               |
| -------------- | ------------------------ |
| Model          | Representa una tabla     |
| Tabla          | Guarda los datos         |
| Primary Key    | Identifica un registro   |
| `$fillable`    | Campos permitidos        |
| `$guarded`     | Campos bloqueados        |
| `$casts`       | Convierte tipos de datos |
| `create()`     | Crea un registro         |
| `find()`       | Busca por ID             |
| `findOrFail()` | Busca por ID o lanza 404 |
| `where()`      | Filtra                   |
| `first()`      | Primer resultado         |
| `get()`        | Todos los resultados     |
| `delete()`     | Elimina                  |
| `update()`     | Modifica                 |

---

# 🎯 DIFERENCIAS QUE DEBES MEMORIZAR

## `find()` vs `findOrFail()`

```php
find()
```

```text
No encuentra → null
```

```php
findOrFail()
```

```text
No encuentra → 404 / excepción
```

---

## `first()` vs `get()`

```php
first()
```

```text
1 modelo o null
```

```php
get()
```

```text
Collection
```

---

## `$fillable` vs `$guarded`

```text
$fillable
↓
¿Qué permito?

$guarded
↓
¿Qué bloqueo?
```

---

## `where()` vs `find()`

```php
find(10)
```

Busca por:

```text
Primary Key
```

Mientras:

```php
where('email', 'test@gmail.com')
```

busca utilizando una condición.

---

# 🚀 EJEMPLO COMPLETO

Modelo:

```php
class Producto extends Model
{
    protected $fillable = [
        'nombre',
        'precio',
        'stock',
        'activo',
    ];

    protected function casts(): array
    {
        return [
            'activo' => 'boolean',
            'precio' => 'decimal:2',
        ];
    }
}
```

### Crear

```php
$producto = Producto::create([
    'nombre' => 'Laptop',
    'precio' => 2500000,
    'stock' => 10,
    'activo' => true,
]);
```

### Buscar por ID

```php
$producto = Producto::find(1);
```

### Buscar o lanzar 404

```php
$producto = Producto::findOrFail(1);
```

### Buscar por condición

```php
$productos = Producto::where(
    'activo',
    true
)->get();
```

### Obtener uno

```php
$producto = Producto::where(
    'nombre',
    'Laptop'
)->first();
```

### Actualizar

```php
$producto->update([
    'precio' => 2800000,
]);
```

### Eliminar

```php
$producto->delete();
```

---

# 🧩 PIÉNSALO COMO UN CRUD

```text
              ELOQUENT

                  │
        ┌─────────┼─────────┐
        ↓         ↓         ↓
      CREATE    READ      UPDATE
        │         │         │
      create()   find()   update()
                 where()
                 first()
                 get()
                            │
                            ↓
                          DELETE
                          delete()
```

### En una frase:

> **`create()` crea, `find()` busca por ID, `where()` filtra, `first()` trae uno, `get()` trae varios, `update()` modifica y `delete()` elimina.**

Si dominas estos conceptos, ya tienes la base necesaria para empezar a trabajar con **Eloquent CRUD**.
