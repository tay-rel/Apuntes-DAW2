Eloquent es el ORM de laravel que implementa el patrón de arquitectura Active Record. Eso significa que *cada tabla en la base de datos corresponde a una clase PHP* **modelo** que interactúa con ella. Cada instancia de dicha clase corresponde a un registro en la tabla y genera persistencia en la base de datos, con lo cual una actualización de los atributos de la clase puede alterar el registro correspondiente en la base de datos. 

>  ORM puede trabajar con los datos que hay en la 
>  base de datos por medio de objetos.

Los datos de **las tablas se mapean a objetos**, evitando todo el trabajo de escribir las consultas para el acceso a la información. El acceso a **sus relaciones también se realiza por medio de propiedades de objetos**, lo que facilita mucho las dinámicas de acceso a la información, siempre que haya una relación de dependencia declarada entre las distintas entidades de nuestro modelo de datos.

La forma de trabajo de Eloquent implementa el patrón "*Active Record*", un patrón de arquitectura de software que permite almacenar en bases de datos relacionales el contenido de los objetos que se tiene en memoria. Esto se hace por medio de métodos como **save(), update() o delete()**, provocando internamente la escritura en la base de datos, pero sin que nosotros tengamos que componer las propias sentencias.

En Active Record **una tabla está directamente relacionada con una clase**. En ese caso se dice que la clase es una envoltura de la tabla. La clase en si es lo que conocemos en el framework como "*modelo*". Cuando nosotros creamos un nuevo objeto de ese modelo y decidimos salvarlo, se produce la creación de un registro de la tabla. Cuando el objeto se modifica y se salvan los datos, se produce el correspondiente update en ese registro. Cuando ese objeto se borra, se produce el delete sobre ese registro de la tabla.

## *Crear un modelo Eloquent*

Cualquier modelo creado en Laravel es un "Eloquent Model", así que nos permitirá realizar las acciones de acceso y modificación de los datos típicas de las bases de datos relacionales.

Los modelos en Laravel se ubican sueltos, dentro de la carpeta *`app\Models`* y extiende de *`Illuminate\Database\Eloquent\Model`* , aunque si tenemos muchas tablas (y por tanto muchos modelos) sería adecuado realizar algún tipo de orden en esta carpeta. Lo adecuado entonces es que funcione con el modelo de carga de clases que nos ofrece el autoload de Composer, que básicamente **usa los namespaces para definir la localización de subdirectorio** donde se encuentra cada uno de los archivos de las clases.

El modo más adecuado para construir nuestros modelos es usando el asistente de [[Artisan Console]], con la instrucción `make:model` seguida del nombre de nuestro modelo.

```
php artisan make:model "Nombre modelo"
```


>**Nota:** Recuerda que por convención en Laravel se usan los nombres de las tablas en plural y minúsculas, mientras que en las clases de los modelos debemos colocar mayúscula en la primera letra (camelcase en realidad) y acabado en singular. Por ejemplo, para una tabla llamada "Groups", el nombre del modelo será "Group". Luego veremos cómo modificar esa convención en el caso que sea necesario.

Ejemplo: 

```
php artisan make:model Product
```

Nuestro modelo "Product" como se ha dicho, corresponde con una clase, que se colocará en la carpeta "app" y tendrá el siguiente código.

A pesar que está prácticamente vacío de código es importante señalar que los modelos de por si ya hacen bastante cosas, gracias a que heredan el comportamiento a partir de la clase `Illuminate\Database\Eloquent\Model`.

```php
namespace App;

use Illuminate\Database\Eloquent\Model;

class Product extends Model
{
    //
}
```


## *Modificar el nombre de la tabla*

Por convención en Laravel, si no se especifica nada, el modelo Product se correspondería, o trabajaría, con la tabla products. 
¿*Pero qué se hace si ya nos vienen dados los nombres de las tablas y no se ajustan a esta convención*?
Si queremos modificar el nombre de la tabla con la que trabaja un modelo podemos indicarlo a partir de la definición de la propiedad $table, asignando el valor que tiene la tabla asociada con este modelo.


```php
<?php 
namespace App\Models; 
use Illuminate\Database\Eloquent\Model; 

class Product extends Model{    
/**
* The table associated with the model.
*
* @var string     
*/ 

{

protected $table = 'My_categories';

}

```

## *Modificar la clave primaria*

Las claves primarias en Laravel también tienen su convención. Básicamente toda tabla tiene una columna llamada "id" que será "autoincrement" y que hará las veces de clave primaria. De nuevo, si esto no se ajusta a nuestra realidad solo tenemos que definir una propiedad llamada $primaryKey, asignando el nombre del campo que actúe como "Primary Key".

>*Nota*
>Eloquent tambien asume que cada modelo corresponde a una 
>tabla de base de datos por eso la columna tiene un **id**.


```php
<?php 
namespace App\Models; 
use Illuminate\Database\Eloquent\Model; 
class Product extends Model
{    
/**     
* The primary key associated with the table.     
*     
* @var string     
* */

protected $primaryKey = 'id_categoria';

```

Además, [Eloquent][https://desarrolloweb.com/articulos/laravel-eloquent.html]
 aume que la clave primaria es un valor integer que se incrementa, lo que significa que Eloquent cambiara automaticamente la clave primaria a un integer. Si no se quiere que se autoincrementado , se puede especificar a la propiedad booleana el valor.

```php
<?php class Flight extends Model
{    
/**     
* Indicates if the model's ID is auto-incrementing.     
*    
* @var bool     
* */    

public $incrementing = false;
}
```

## *Uso de [Timestamps](https://laravel.com/docs/9.x/eloquent#timestamps)

Por defecto ,Eloquent espera *created_at y updated_at* que existen en la correspondiente tabla de la base de datos del modelo. Eloquent establecera automaticamente los valores de estas columnas cuando se creen o actualicen los modelos. Si no quieres que estas columnas sean gestionadas automáticamente por Eloquent, debes definir una propiedad `public $timestamps =false` en el modelo.

>**Nota:** Al crear una tabla podemos indicarle que nos cree los correspondientes campos timestamp ****"created_at" y "updated_at"***, simplemente indicándolo con el método timestamps(). Recuerda que una creación de una tabla en el archivo de migraciones se definía más o menos así:  
  

```php
Schema::create('carpetas', function (Blueprint $table) {
    $table->increments('id');
    $table->string('nombre');
    $table->text('descripcion');
    $table->timestamps();
});
```


Otra cosa que podemos modificar es el formato de la fecha almacenada en estos campos. Es algo que generalmente no necesitarás, pero se puede marcar a través de la propiedad *$dateFormat*. Consulta la documentación en este caso.

# [Building Queries](https://laravel.com/docs/9.x/eloquent#building-queries)

En Eloquent todo metodo retornara todos los resultados que existen en la tabla modelo.SIn embargo , desde cada modelo Eloquent como un [[Query Builder]] se puede añadir restricciones adicionales a las consultas y luego invocar el método 'get' para recuperar los resultados.

```php
$products = Product::where('active', 1)               
->orderBy('name')               
->take(10)               
->get();
```
