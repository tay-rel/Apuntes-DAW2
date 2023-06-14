# Rutas

## Archivos de rutas
Todas las rutas de Laravel se definen en sus archivos de ruta, que se encuentran en la carpeta `routes`. Estos archivos son cargados directamente por el framework.

Las rutas más básicas de Laravel aceptan una URI y un *Closure*:

```php
Route::get('/inicio', function () {
	return 'Hello World';
});
```

Para la mayoría de las aplicaciones, las rutas se comienzan a definir en el archivo ```routes/web.php```. Se puede acceder a las rutas definidas introduciendo la URL de la ruta definida en el navegador.

> **Closure**: Funciones que pueden acceder a variables no locales (externas a la función), pero que son útiles a la función.

## Generar rutas

### Métodos de enrutamiento disponibles
El enrutador permite registrar rutas que respondan a cualquier verbo **HTTP**, utilizando el facade `Route`:

```php
Route::get($uri, $callback);
Route::post($uri, $callback);
Route::put($uri, $callback);
Route::patch($uri, $callback);
Route::delete($uri, $callback);
Route::options($uri, $callback);
```

- **GET**
Se usa para consultar o mostrar un recurso. No produce ni modifica ningún registro. Siempre produce el mismo resultado.

- **POST**
Petición para crear nuevos recursos. Cada llamada a **POST** debería producir un nuevo recurso.

- **PUT**
Modificar un recurso existente. Sustituye un recurso por completo.

- **PATCH**
Modificar un recurso existinte. Se actualizan algunos elementos del recurso mismo, sin sutituirlo por completo, como por ejemplo, simular un borrado con *soft delete*, donde cambiamos el valor del campo `deleted_at`.

- **DELETE**
Eliminar registros, bien un recurso individual o una colección completa.

### Generar una URL con parámetros por GET

#### Pasar parámetros por GET
Los parámetros se pasan a la ruta entre llaves `{}` y se llaman como parámetro en la función:

```php
Route::get('usuarios/{id}', function ($id) {  
    return 'Mostrando detalles del usuario: ' . $id;  
})->where('id', '[0-9]+'); // el parametro 'id' debe ser un número
```

Si no se pusiera la restricción en la cláusula ```where```, si se declarase la siguiente ruta justo debajo, interpretaría ```nuevo``` como parámetro. Con la restricción aplicada al parámetro ```id``` sí que se logra alcanzar la ruta. También podría solucionarse declarándola más arriba.

```php
Route::get('usuarios/nuevo', function () {  
    return 'Creando nuevo usuario...';  
});
```

#### Enviar más de un parámetro con parámetros opcionales
Los parámetros opcionales se asignan con el operador `?` dentro de la declaración del parámetro ```{param?}```

```php
// nickname: parametro opcional: se declara con ?  
Route::get('saludo/{name}/{nickname?}', function ($name, $nickname = null) {  
    if ($nickname) {  
        return 'Welcome ' . ucfirst($name) . '. Tu apodo es ' . $nickname;  
    }    return 'Welcome ' . ucfirst($name) . '. Eres anónimo...';  
});
```

### Rutas con el método POST
Declaramos una ruta de tipo **POST** de la siguiente manera:

```php
Route::post('usuarios', 'UserController@store')->name('user.store');
```

### Rutas con el método PUT
La ruta **PUT** se encarga de enviar los datos del formulario al método `update` del controlador, que se encarga de modificar la información de un registro con un identificador (ID) en la base de datos.

```php
Route::put('usuarios/{user}', 'UserController@update')->name('user.update');
```

## Renombrar rutas

El método `name()` nos permite renombrar una ruta y utilizar ese nombre en todos los lugares donde vaya a ser usada. De esta manera, podemos cambiar la ruta desde el archivo de rutas, y se actualizará en todos los archivos donde se haya utilizado con ese nombre.

```php
Route::get('usuarios', 'UserController@index')->name('users');
```

```php
Route::get('usuarios/nuevo', 'UserController@create')->name('user.create');
```

## Protección CSRF
Cualquier formulario HTML que apunte a rutas `POST`, `PUT`, `PATCH` o `DELETE` que estén definidas en un archivo de rutas debe incluir un campo de token **CSRF**. De lo contrario, la solicitud será rechazada.

El middleware `VerifyCsrfToken` nos permite evitar que terceros puedan enviar peticiones de tipo **POST** a nuestra aplicación y realizar ataques de tipo ***cross-site request forgery***.

En la vista se implemena de la siguiente manera:

Para agregar un campo con el token dentro de nuestro formulario, que le va a permitir a Laravel reconocer peticiones de formulario que sean válidas, debemos llamar al método `csrf_field()`:

```php
<form action="{{ route('user.store') }}" method="POST">
    {{ csrf_field() }}

    ...
    
</form>
```

El método `csrf_field()` agregará un campo oculto llamado `_token` con el valor del token al código HTML de nuestro formulario.

A través de la directiva `@csrf` de **Blade** podemos simplificar esta sintaxis sustituyendo la línea 2.
