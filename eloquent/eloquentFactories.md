# Factories 

Son una herramienta para generar datos de prueba de manera efeciente y concistente cuando se trabaja con eloquent orm, Se trata de clases que definen como generar instancias de modelos eloquent con datos de prueba con los siguientes fines: 

- Crear datos falsos pero que parecen realistas para testing y desarrollo
- Establecer valores por defecto para los atributos de los modelos
- Generar relaciones entre modelos facilmente

## Crear un factory

Se puede usar el siguiente comando para crear un factory

```bash
php artisan make:factory AlumnoFactory --model=Alumno
```

### Estructura basica de un factory

```php

use App\Models\Alumno;
use Illuminate\Database\Eloquent\Factories\Factory;

class AlumnoFactory extends Factory
{
    protected $model = Alumno::class;

    public function definition()
    {
        return [
            'name' => $this->faker->name,
            'last_name' => $this->faker->lastName,
            'carrer_id' => User::factory(),
        ];
    }
}

```

### Componentes clave

- **Faker:** Laravel integra la libreria Faker para generar datos aleatorios: 
  - `$this->faker->name`
  - `$this->faker->lastName`
- **Definicion de atributos:** El metodo `definition()` especifica los valores por defecto.
- **Relaciones:** Se pueden usar otros factories para llenar campos que tienen relaciones.
    ```php 
        'carrer_id' => User::factory() 
    ```

### Usar factories

Podemos crear una sola instancia 

```php
$alumno = Alumnos::factory()->create();
```

Se pueden crear diferentes instancias con un solo comando

```php
$alumnos = Alumnos::factory()->count(5)->create();
```

### Estados (States)

Son una caracteristicas que permiten definir variones especificas de los modelos en diferentes escenarios o contextos, esto es muy util cuando un modelo puede existir en diferentes estados en la aplicacion

### Ejemplo

Si tenemos un modelo `Post` que puede estar: 

- En borrador (`draft`)
- Publicado (`published`)
- Archivo (`archived`)
- Destacado (`featured`)

Estos estatus pueden contener cosas diferentes en sus elementos

### Definición de un state

```php

namespace Database\Factories;

use App\Models\Post;
use Illuminate\Database\Eloquent\Factories\Factory;

class PostFactory extends Factory
{
    public function definition()
    {
        return [
            'title' => $this->faker->sentence,
            'content' => $this->faker->paragraphs(3,true),
            'is_published' => false,
            'published_at' => null,
            'views' => 0
        ];
    }

    public function published()
    {
        return $this->state(function (array $attributes){
            return [
                'is_published' => true,
                'published_at' => now(),
                'views' => $this->faker->numberBetween(1,1000)
            ];
        });
    }

    public function featured()
    {
        return $this->state([
            'is_featrued' => true,
            'featured_at' => now(),
            'featured_until' => now()->addWeek()
        ]);
    }
}
```

### Uso de un state

El uso basico seria de la siguiente manera

```php
$publishedPost = Post::factory()->published()->create();

$featuredPost = Post::factory()->featured()->create();
```

Se pueden combinar estados en una sola creacion

```php
$posts = Post::factory()->published()->featured()->create();
```

### Casos de uso avanzados

States que dependen de atributos existentes

```php
public function recentlyPublished()
{
    return $this->state(function (array $attributes){
        return [
            'published_at' => now()->subDays(3),
            'is_recent' => true
        ];
    });
}
```

States con relaciones

```php
public function withAuthor()
{
    return $this->state([
        'author_id' => User::factory()->create()->id
    ]);
}
```

States que modifican relaciones 

```php
public function withComments()
{
    return $this->afterCreating(function (Post $post){
        $post->comments()->saveMany(
            Comment::factory()->count(5)->make()
        );
    });
}
```

De esta forma podemos ver que se realiza combos wombos con las factories

### Buenas practicas de states

- **Nombres descriptivos:** Se deben utilizar nombres que indiquen en que estado se encuentra el modelo (`published`, `archived`, `active`).
- **Manten la coherencia:** Asegurarse que los states no tengan conflicto con las reglas de validacion del modelo.
- **Reutiliza states:** Realizar combos wombos para crear escenarios complejos y evitar declarar mas states
- **Documentacion:** Realizar los comentarios necesarios cuando el comportamiento no sea tan obvio.
- **Sates vs Traits:** Para comportamientos que son muy complejos, es mejor implementar traits en los modelos para no sobrecargar los factories.

## Buenas practicas factories

- **Consistencia:** Siempre deben generar datos que cumplan con las validaciones del modelo correspondiente
- **Realismo:** Usar datos que simulen ser realer utilizando como apoyo.
- **Rendimiento:** Al momento de realizar pruebas se puede considerar usar `make()` y no `create()` cuando no sea necesario que persistan los datos.

    ```php
    $post = Post::factory()->make();
    ```
- **Sobreescritura:** Se puede sobrescribir los atributos especificos;

    ```php
    $post = Post::factory()->create(['title' => 'Titulo especifico']);
    ```

## Integración con Seeders

Los factories se usan comúnmente dentro de seeders: 

```php
public function run()
{
    \App\Models\Post::factory()->count(50)->create();
}
```