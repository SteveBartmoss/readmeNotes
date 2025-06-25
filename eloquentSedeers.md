
# Seeders 

Son una herramienta para poblar una base de datos para poder hacer pruebas o tener informacion para que la base datos no este vacia

## Que contiene un seeder

Son clases de php que contienen metodos para insertar datos las diferentesa tablas de una base lo cual permite

- Crear datos iniciales para desarrollo y pruebas
- Establecer datos por defecto para la aplicacion
- Repoblar la base de datos despues de correr migraciones

## Como funcionan

### Generacion

Se usa el siguiente comando para la creacion de un seeder

```bash
php artisan make:seeder NombreDelSeeder
```

### Estructura 

Cada seeder tiene un metodo `run()` en el que se define la inserccion de datos por ejemplo de la siguiente manera

```php
public function run()
{
    DB::table('alumnos')->insert([
        'name' => 'Steve',
        'last_name' => 'Bartmoss',
    ]);
}
```

### Ejecucion

Los seeder pueden ejecutarse de la siguiente manera

```php
php artisan db:seed --class=NombreDelSeeder
```

## DatabaseSeeder

Este archivo actúa como un organizador de seeder, de esta manera podemos ver que este archivo llama a todos los que tenemos y que estan agreados en el mismo, de esta forma podemos llenar de una sola vez toda la base de datos.

```php

public function run()
{
    $this->call([
        AlumnosSeeder::class,
        CarrrersSeeder::class,
    ]);
}
```

Para ejecutar este archivos se usa el siguiente comando

```bash
php artisan db:seed
```

### Buenas practicas

- Usar factories para datos complejos en base de datos
- Considerar el uso de Faker para datos aleatorios
- No usar seeders para datos de produccion que sean criticos
- Mantener los seeders organizados y documentados

### Relacion con migraciones

Los seeders se pueden combinar con las mifraciones ya que permite llenar las tablas que se crearon o se volvieron a crear recientemente

Se puede combinar con los comandos para hacer refresh y migrate para correr los seeder despues del comando anterior

```php
php artisan migrate --seed
```

```
php artisan migrate:fresh --seed
```