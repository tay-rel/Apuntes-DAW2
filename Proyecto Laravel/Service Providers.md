Estos Service Providers se encuentran registrados en el array *$providers*  dentro del archivo **config/app.php** donde están listadas todas las clases de Service Providers que serán cargadas en la aplicación. Debemos tomar en cuenta que muchos de los Service Providers listados son *diferidos* (usando la propiedad $defer = true ), **lo que significa que no serán cargados en cada petición por defecto, sino solo cuando los servicios que provee sean realmente necesarios**. De esta manera se disminuye la sobrecarga de rendimiento y el tiempo de carga de la aplicación.

# ¿Qué son los [Servicios Providers][https://styde.net/que-son-los-service-providers-de-laravel-y-como-usarlos/]?

-   Laravel como framework usa un *"contenedor de servicios"* y unos *"proveedores de servicios"* siempre que necesita iniciar una instancia de una aplicación Laravel.

-   No solo nuestras aplicaciones, sino también los servicios que conforman el núcleo de Laravel, se inician gracias a los `service providers`.

-   Los `service providers` se encargan de toda la configuración necesaria antes de empezar a usar un servicio. Dependiendo del servicio que se va a iniciar, **el proveedor se encarga de crear nuevas instancias y posiblemente las relaciones con otros servicios** (definir parámetros, oyentes de eventos, middlewares, rutas).

# Crear un [service provider][https://programacionymas.com/blog/service-providers-en-laravel]

Para crear un archivo se debe uar la linea de [[Artisan Console]]  , que es el siguiente comando:


```bash
php artisan make:provider StydeServiceProvider 
```

-   Este comando creará un archivo `CvUploaderServiceProvider.php` con la estructura básica que un `service provider` debe tener.
-   El archivo se creará en la carpeta ` app/Providers `.

Si se ha creado un nuevo archivo este se debe agregar al array de `providers`. Para ello vamos al archivo `config/app.php` y dentro del arreglo `providers` añadimos el que acabamos de crear:

```bash
php artisan make:provider CvUploaderServiceProvider
```

```php
'providers' => [

    /*
     * Laravel Framework Service Providers...
     */
    Illuminate\Auth\AuthServiceProvider::class,
    Illuminate\Broadcasting\BroadcastServiceProvider::class,
    // [...]
    Illuminate\Validation\ValidationServiceProvider::class,
    Illuminate\View\ViewServiceProvider::class,

    /*
     * Package Service Providers...
     */
    Laravel\Tinker\TinkerServiceProvider::class,

    /*
     * Application Service Providers...
     */
    App\Providers\AppServiceProvider::class,
    App\Providers\AuthServiceProvider::class,
    // App\Providers\BroadcastServiceProvider::class,
    App\Providers\EventServiceProvider::class,
    App\Providers\RouteServiceProvider::class,


    App\Providers\CvUploaderServiceProvider::class,

],
```

## Método register

En este método **solo se registrará aquellas clases o servicios que necesitan ser cargadas en el contenedor de servicios de Laravel**, el cual es quien gestiona las clases y sus dependencias en la aplicación, es decir, es donde registramos las implementaciones de clases que se ejecutarán al configurar el proyecto.  Aquí no se deben registrar ninguna ruta, detector de eventos o cualquier otro código que pueda usar un servicio que es proveído via Service Provider **porque puede que aún dicho servicio no esté cargado**.

### ¿Pero registrar qué y sobre qué?

-   Nos permite registrar "[[Binding]]" (esto es, terminología que usa Laravel en su documentación; en español sería enlaces).
-   Y estos bindings se registran sobre el `service container`.
- 
### ¿Qué representan estos bindings? ¿Qué enlazan?

Registrar un binding en el `service container` de Laravel es decirle al contenedor cómo instanciar un objeto en particular.

-   El uso más básico**es instanciar un objeto de una clase que no tiene ninguna dependencia**(no requiere de ningún parámetro).
-   Sin embargo también es posible decirle al contenedor de Laravel cosas como "cuando se requiera un objeto de esta interfaz, crea una instancia de esta clase con estos parámetros".
-   E inclusive, podemos decirle a Laravel que use una clase determinada cuando la instancia se requiera en un controlador específico, pero que use otra clase cuando la instancia se necesite en otros contextos.

El `service container` de Laravel nos permite registrar [distintos tipos de bindings](https://gist.github.com/davejamesmiller/bd857d9b0ac895df7604dd2e63b23afe).

Recapitulando, respecto al método `register`: es un método que le permite a nuestro ServiceProvider registrar bindings en el contenedor de Laravel.

### Ejemplo 1:

Por ejemplo si una clase necesita instanciar con ciertos parametros determinados que implica la construccion de nuevos objetos se procederia a usar los servicios provider.

**Antes**:

```php
$unaInstancia = new ClaseA(new ClaseB(config('secret_key')), new ClaseC(new ClaseX(), new ClaseY()), new ClaseD('pym', 7));
```

**Después**:
```bash
php artisan make:provider CvUploaderServiceProvider 
```

```php
<?php
namespace App\Providers;
use Illuminate\Support\ServiceProvider;
class CvUploaderServiceProvider extends ServiceProvider
{
    /**
     * Register the application services.
     *
     * @return void
     */
    public function register()
    {
	
		$this->app->singleton(ClaseA::class, function ($app) {
		   return new ClaseA(
		   new ClaseB(config('secret_key')), 
		   new ClaseC(new ClaseX(), 
		   new ClaseY()), 
		   new ClaseD('pym', 7));
		});

    }
}
```


### Ejemplo 2:

Si nuestra clase no tiene dependencias y no implementa ninguna interfaz, no es necesario crear un binding.

Su uso estaría disponible sin ningún paso adicional:

```php
class UploadController extends Controller
{

    public function store(CvHandler $cvHandler)
    {
        // aquí es posible usar $cvHandler para procesar las hojas de vida
    }

}
```

Pero, si queremos asociar una interfaz a nuestra clase (que es lo más recomendable), debemos registrar un binding en el método register:

```php
$this->app->bind(CvHandlerInterface::class, CvHandler::class);
```


De esta forma podemos decir que:

Un `service provider` ha registrado nuestro servicio en el `service container`, y podemos usar nuestro servicio desde donde nos plazca (gracias a la inyección de dependencias).