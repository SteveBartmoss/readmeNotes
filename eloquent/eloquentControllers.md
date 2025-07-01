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
    public function edit(Producto $producto)
    {
        return view('productos.edit', compact('producto'))
    }

    // Actualizar recurso
    public function update(Request $request, Producto $producto)
    {
        $validated = $request->validate([
            'nombre' => 'required|max:255',
            'precio' => 'required|numeric',
        ]);

        $producto->update($validated);

        return redirect()->route('productos.index')
                        ->with('success', 'Producto actualizado');
    }

    //Eliminar recurso
    public function destroy(Producto $producto)
    {
        $producto->delete();

        return redirect()->route('productos.index')
                            ->with('succes', 'Producto eliminado');
    }
}
```

## Tecnicas Avanzadas con Eloquent en Controladores

**Inyecion de Modelos (Route Mondel Binding)**

```php
public function show(Producto $producto)
{
    return view('productos.show', ['producto' => $producto]);
}
```

**Eager Loading para optimizacion**

```php
public function index()
{
    $productos = Productos::with('categoria', 'reviews')->get();
    return view('productos.index', compact('productos'));
}
```

**Busqueda y Filtrado**

```php
public function index(Request $request)
{
    $query = Producto::query();

    if($requet->has('nombre')){
        $query->where('nombre', 'like', '%'.$request->nombre.'%');
    }

    if($request->has('categoria_id')){
        $query->where('categoria_id', $request->categoria_id);
    }

    $productos = $query->paginate(10);

    return view('productos.index', compact('productos'));
}
```


**Uso de Repositorios (Patron Repository)**

```php
// En el controlador
public function __construct(ProductoRespository $prouctoRepo)
{
    $this->productoRepository = $productoRepo;
}

public function index()
{
    $productos = $this->productoRepository->all();
    return view('productos.index', compact('productos'));
}
```

## Buenas Praxticas para Controladores con Eloquent

- **Mantener controladores delgados:**
    - Mover logica de negocio a servicios o repositorios
    - Usar Form Request para validacion compleja

- **Manejo de excepciones**

    ```php
    public function show($id)
    {
        try{
            $producto = Producto::findOrFail($id);
            return view('productos.show', compact('producto'));
        } catch (ModelNotFoundException $e){
            abort(404, 'Producto no encontrado');
        }
    }
    ```
- **Respuestas consistentes:**

    ```php
    // Para APIs
    return response()->json([
        'data' => $producto,
        'message' => 'Sucess',
    ],200);

    // Para web
    return redirect()->back()->with('success', 'Operacion exitosa');
    ```
- **Uso de Traits para funcionalidad comun:**

    ```php
    trait ResponseTrait{
        protected function apiResponse($data, $message = null, $status = 200)
        {
            return response()->json([
                'data' => $data,
                'message' => $message,
            ], $status);
        }
    }
    ```

## Ejemplo de Controlador API con Eloquent

```php

namespace App\Http\Controllers\API;

use App\Http\Controllers\Controller;
use App\Models\Producto;
use Illuminate\Http\Request;

class ProductoAPIController extends Controller
{
    public function index()
    {
        return Producto::with('categoria')->paginate(15);
    }

    public function store(Request $request)
    {
        $producto = Producto::create($request->all());
        return response()->json($producto,201);
    }

    public function show(Producto $producto)
    {
        return $producto->load('categoria', 'reviews');
    }

    public function update(Request $request, Producto $producto)
    {
        $producto->update($request->all());
        return response()->json($producto);
    }

    public function destroy(Producto $producto){
        $producto->delete();
        return response()->json(null, 204);
    }
}
```

## Conclusion

Los controladores en Laravel con Eloquent te permiten:

- Organizar la logica de la aplicacion

- Manejar operaciones CRUD de manera eficiente

- Trabajar facilmente con relaciones entre modelos

- Mantener un codigo limpio y mantenible