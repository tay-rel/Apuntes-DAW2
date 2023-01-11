El constructor de consultas([Query Builder][https://laravel.com/docs/9.x/queries]) a bases de datos de Laravel proporciona una interfaz cómoda y fluida para crear y ejecutar consultas a bases de datos. Se puede utilizar para realizar la mayoría de las operaciones de base de datos en tu aplicación y funciona perfectamente con todos los sistemas de base de datos soportados por Laravel.
Se puede usar el método `table`  proporcionado por la `DB fachada` para iniciar una consulta.El método `table` devuelve una instancia de constructor de consultas fluida para la tabla dada, lo que le permite encadenar más restricciones en la consulta y, finalmente, recuperar los resultados de la consulta utilizando el método get donde cada resultado es una instancia de PHP `stdClass` objeto.

```php
<?php 
namespace App\Http\Controllers; 
use App\Http\Controllers\Controller;
use Illuminate\Support\Facades\DB; 

class UserController extends Controller
{    
/**     
* Show a list of all of the application's users.     
*     
* @return 
* \Illuminate\Http\Response     
*/    
	public function index()    
	{        
		$users = DB::table('users')
		->get();         //recupera el resultado de la consulta
		
		return view('user.index', ['users' => $users]);    
	}
}
```

Se puede acceder al valor de cada columna accediendo a la comlumna como una propiedad de el objeto:

```php
use Illuminate\Support\Facades\DB; 

$users = DB::table('users')->get(); 

foreach ($users as $user) {   
	echo $user->name;
}
```

## Agregados

El generador de consultas también proporciona una serie de métodos para recuperar valores agregados como count,**max, min, avg y sum**. Puede llamar a cualquiera de estos métodos después de construir su consulta:

```php
use Illuminate\Support\Facades\DB; 

$users = DB::table('users')->count(); 
$price = DB::table('orders')->max('price');

```

