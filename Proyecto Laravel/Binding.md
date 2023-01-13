## ¿Qué es el Model Binding?

Los controladores y las páginas Razor *trabajan con datos que provienen de peticiones HTTP*. La vinculación de modelos automatiza este proceso.
El sistema de model binding(encuadernación).

- Recupera datos de diversas fuentes, como datos de ruta, campos de formulario y cadenas de consulta.
- Proporciona los datos a controladores y páginas Razor en parámetros de métodos y propiedades públicas.
-  Actualiza propiedades de tipos complejos.

Casi todas **las vinculaciones del contenedor de servicios se registrarán en proveedores de servicios**, por lo que la mayoría de estos ejemplos mostrarán el uso del contenedor en ese contexto.

Dentro de un proveedor de servicios, siempre se tiene acceso al contenedor a través de la propiedad $*this->app*. Podemos registrar un binding usando el *método bind*, pasando el nombre de la clase o interfaz que deseamos registrar junto con un closure que devuelva una instancia de la clase:

```php
use App\Services\Transistor;
use App\Services\PodcastParser;

$this->app->bind(Transistor::class, function ($app) {    
return new Transistor($app->make(PodcastParser::class));

});
```

Tenga en cuenta que **recibimos el propio contenedor como argumento para resolver**. Podemos entonces utilizar el contenedor para resolver subdependencias del objeto que estamos construyendo.

Como se ha mencionado, normalmente se interactúa con el contenedor dentro de los proveedores de servicios, sin embargo, si desea interactuar con el contenedor fuera de un proveedor de servicios, puede hacerlo a través de la App facade.

## Ejemplo:

Para este ejercicio se usara [[Service Providers]]  que se encarga de instaciar los objetos y las relaciones con otros servivios.

```php
//Providers/AppServiceProvider.php
  public function register()  
  {  
    //Cuando se genera una clase de Sortable enlaza un objeto  
      $this -> app ->bind(Sortable::class , function ($app){  
          return new Sortable(request()->url());  //crea el nuevo objeto
          //Request es la peticion que se guarda lo que viene por get y post .  
          //Entonces el metodo Url nos devuelve la url que tiene en ese momento.      });  
//sortale se le inicia la primera vez que le pasa 
}
						  
//User Controller
 public function trashed(Sortable $sortable)
    {
        return view('users.index', [
            'users' => User::onlyTrashed()->paginate(),
            'view' => 'trash',
            'sortable'=>$sortable,
        ]);
    }
//El objeto se crea mediante la inyeccion de App de estar manera se evita la creación de un nuevo objeto en cada metodo.
    
```

https://developer.mozilla.org/es/docs/Web/JavaScript/Reference/Global_objects/Function/bind
