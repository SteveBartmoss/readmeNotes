# Migraciones

Es una caracteristica que funciona como un sistema de control de versiones para una base de datos, de esta manera podemos modificar el esquema de la base de datops de manera consistente y que sea compatible con otros desarrolladores y entornos

## Conceptos clave

- Son archivos que describen cambios en 8una base de datos
- Se usan para:
    - Crear, modificar o eliminar tablas
    - Definir relaciones entre tablas
    - Mantener la consistencia de la base en entre los entornos
    - Trabajar en equipo de una forma mas simple

## Estructura basica 

Una migracion se compone de: 

- Metodo `up()`: Contiene las operaciones que se ejecutaran
- Metodo `down()`: Contiene las operaciones para revertir cambios

```php
use Illuminate\Database\Migrations\Migration;
use Illuminate\Database\Schema\Blueprint;
use Illuminate\Support\Facades\Schema;

return new class extends Migration {

    public function up()
    {
        schema::create('alumnos', function (Blueprint $table){
            $table->id();
            $table->string('name');
            $table->string('last_name');
            $tabel->timestamps();
        });
    }

    public function down()
    {
        Schema::dropIfExists('users');
    }
}
```

## Tipos de operaciones

Creacion de tablas

```php
Schema::create('posts', function (Blueprint $table){
    $table->id();
    $table->string('title');
    $table->text('content');
    $table->foreingId('user_id')->constrained();
    $table->timestamps();
});
```

Modificacion de tablas

```php
Schema::table('alumnos',function (Blueprint $table){
    $table->string('carrer')->affter('created_at')->nullable();
    $table->index('numero_control');
}); 
```

Eliminacion de columnas

```php
Schema::table('alumnos', function (Blueprint $table){
    $table->dropColumn('phone');
});
```

## Tipos de Columnas Disponibles

Algunos de los tipos soportados o que acepta son:

- `$table->string('name')`

- `$table->text('description')`

- `$table->integer('quantity')`

- `$table->boolean('active')`

- `$table->dateTime('published_at')`

- `$table->decimal('amount', 8, 2)`

- `$table->json('meta')`

## Indices y Claves

```php
$table->primary('id'); //Clave primaria
$table->unique('email'); //Valor unico
$table->index('active'); //Indice normal
$table->fulText('content'); //Indice de texto completo
$table->foreign('user_id')->references('id')->references('id')->on('users')->onDelete('cascade') // Clave foranea
```

## Comandos Artisan Esenciales

- `php artisan make:migration create_table_name`: Crear una migracion

- `php artisan migrate`: Ejecutar migraciones pendientes

- `php artisan migrate:fresh`: Eliminar todas las tablas de una base de datos (aunque no esten en las migraciones) y despues recrear la base de datos con las migraciones

- `php artisan migrate:rollback`: Revertir ultima operacion que ha realizado

- `php artisan migrate:status`: Ver estado de migraciones

## Buenas Practicas

- **Nombres descriptivos:** `create_post_table`, `add_deleted_at_to_users_table`

- **Orden logica:** Aunque no debería ser de esta forma, laravel toma por orden cronologico las migraciones, por esto es importante que siempre se creen las migraciones en el orden correcto. Por ejemplo si se modificara la tabla alumnos al agregarle la llave foranea de carrera, se debe crear la migracion de carrera (si es que no existe ya la tabla de carrera) y despues la migracion para modificar la tabla de alumnos, de otra forma dara un error ya que no existe la tabla carrera y quiere agregar una relacion a la tabla alumno

- **Usar metodos reversibles:** Verificar que `down()` esta funcionando correctamente

- **Mantener consistencia:** Las migraciones deben poder ejecutarse desde cero 

## Migraciones en Entorno de Produccion

- Siempre probar migraciones en desarrollo/staging primero para verificar que todo funciona bien

- Considerar usar `php artisan migrate --pretend` para ver el sql generado por eloquent

- Para tablas grandes, considerar migraciones sin tiempo de inactividad

## Relacion con Eloquent Models

Las migraciones definen la estructura, mientras que los modelos Eloquent interactuan con los datos

```php
//migracion

Schema::create('post', function (Blueprint $table){
    $table->id();
    $table->string('title');
    //....
});

// Modelo correspondiente
class Post extends Model
{
    protected $fillable = [
        'title'.
        //....
    ];
}
```


