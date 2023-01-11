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

https://developer.mozilla.org/es/docs/Web/JavaScript/Reference/Global_objects/Function/bind
