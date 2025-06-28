# Controladores

Los controladores en eloquent son la union entre los modelos y las rutas o vistas de la aplicacion. Sirven para manejar la logica de la aplicacion y organizar las operaciones CRUD (Create, Read, Update, Delete) usando los 
modelos.

## Tipos de controladores

**Controladores Basicos**

```php
php artisan make:controller ProductoController
```

**Controladores de Recursos**

```php
php artisan make:controller ProductoController --resource
```

**Controladores API (para servicios RESTfull)**

```php
php artisan make:controller API/ProductoController --api
```

## Estructura de un Controlador Resource 

```php
namespace App\Http\Controllers;

use App\Models\Producto;
use Illuminate\Http\Request;

class ProductoController extends Controller
{

    //Mostrar lista de recursos
    public function index()
    {
        $productos = Producto::lates()->paginate(10);
        return view('productos.index', compact('productos'));
    }

    // Mostrar formulario de creacion
    public function create()
    {
        return view('productos.create')
    }

    //Almacenare nuevo recurso
    public function store(Request $request)
    {
        $validated = $request->validate([
            'nombre' => 'required|max:255',
            'precio' => 'required|numeric',
        ]);

        Producto::create($validated);

        return redirect()->route('productos.index')
                        ->with('success', 'Producto creado exitosamente');

    }

    //Mostrar formulario de edicion
    
}
```