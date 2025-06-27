# Modelos en Eloquent ORM: Explicacion Completa

Eloquent es el sistema ORM (Mapeo Objeto-Relacional) que permite interactuar con tu base de datos usando una sintaxis orientada objetos. Los modelos son una parte importante de este sistema

## Conceptos Fundamentales

- **Modelo:** Representa una tabla de la base de datos, cada instancia del modelo representa una fila en la tabla, proporciona metodos para consultas y relaciones en la base de datos.

- **Ventajas:** Sintaxis intuitiva y expresiva, seguridad contra inyeccion de SQL, Sistema de relaciones potente, Mutadores y accesores, Eventos y observadores.

## Creacion de modelos

```bash
php artisan make:model Producto
```

Este comando creara un modelo en la ruta `app/Models/Producto.php` o en `app/Producto.php` en versiones mas antiguas

## Configuracion Basica

```php
class Producto extends Model
{
    protected $table = 'mis_productos'; //Nombre de tala personalizado
    protected $primaryKey = 'prod_id'; //PK personalizada
    public $incrementing = false; //Para PK no autoincrementales
    protected $keyType = 'string';
    public $timestamps = false; // Deshabilitar created_at/updated_at

    protected $fillable = ['nombre','precio']; // Campos asignables masivamente
    protected $guarded = ['id', 'clave_secreta']; // Campos protegidos
}
```

## Operaciones CRUD Basicas

### Creacion

```php
// Opcion 1:
$producto = new Producto();
$producto->nombre = 'Laptop';
$producto->precio = 999.99;
$producto->save();

// Opcion 2 (asignacion masiva)
$producto = Producto::create([
    'nombre' => 'Laptop',
    'precio' => 999.99
]);
```

### Consulta

```php
//Todos los registros 
$productos = Producto::all();

//Buscar por ID
$producto = Producto::find(1);

//Consulta con condicionales
$productosCaros = Producto::where('precio','>',500)->orderBy('nombre')->get();
```

### Actualizacion: 

```php
$producto = Producto::find(1);
$producto->precio = 899.99
$producto->save();

//Actualizacion masiva
Producto::where('precio', '>', 500)->update(['descuento'=> true]);
```

### Eliminacion 

```php
$producto = Producto::find(1);
$producto->delete();

//Eliminacion directa
Producto::destroy(1);

//Eliminacion masiva
Producto::where('precio','<', 100)->delete();
```

## Relaciones

**Uno a uno**

```php
class Usuario extends Model
{
    public function telefono()
    {
        return $this->hasOne(Telefono::class);
    }
}
```

**Uno a muchos**

```php
class Post extends Model
{
    public function comentario()
    {
        return $this->hasMany(Comentario::class);
    }
}
```

**Muchos a muchos**

```php
class Usuario extends Model
{
    public function roles()
    {
        return $this->belongsToMany(Role::class)
    }
}
```

## Caracteristicas Avanzadas

**Scopes (Ambitos)**

```php
public function scopeActivos($query)
{
    return $query->where('activo',true);
}

//Uso:
$productosActivos = Producto::activos()->get();
```

**Accesores y mutadores**

```php
// Accesor (formatear al obtener)
public function getNombreAttribute($value)
{
    return ucfirst($value);
}

//Mutador (formatear al guardar)

public function setNombreAttribute($valuer)
{
    $this->attributes['nombre'] = strtolower($value);
}
```

**Atributos JSON**

```php
protected $casts = [
    'opciones' => 'array',
    'activo' => 'boolean',
    'fecha_lanzamiento' => 'date'
];
```

**Eventos del Modelo**

```php
protected static function booted()
{
    static::creating(function ($producto){
        $producto-codigo = uniqid();
    });
}
```

## Buenas practicas

- **Convencio sobre configuracion:** 
    - Nombres de modelos en singular (Producto)
    - Nombres de tablas enh plural(productos)
    - Clave primaria llamada `id`

- **Uso de repositorios:**
    - Para logica compleja de negocio 

- **Eager Loading**
    ```php
    //Evita el problema de N+1
    $post = Post::whit('comentario.usuario')->get();
    ```

- **Soft Deletes:**
    ```php
    use Illuminate\Database\Eloquent\SoftDeletes;

    class Producto extends Model
    {
        use SoftDeletes;
    }
    ```
