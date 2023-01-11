Cuando estas trabajando con un proyecto en Laravel una de las cosas que se deben realizar son las actualizaciones pero hay una guia básica en la documentación de [Laravel][https://laravel.com/docs/5.6/upgrade] donde se debe revisar que cosas han cambiado entre cada versión y poder aplicarlo a nuestro proyecto.
En este caso se actualizara la versión de la **5.5 a 5.6**.

1. Paso : Comprobar la versión 
Para comprobar la versión debes estar posicionado en la carpeta del proyecto y se debe lanzar el siguiente comando:

```php artisan --version``` de esta manera se podra verificar que versión se usa
`Laravel Framework version 5.5`.

2. Paso: Revisar la documentación de [Laravel][https://laravel.com/docs/5.6/upgrade] 
En este punto se debe leer a documentación donde podremos observar que existe cambios en la versión .Asi que debemos modificar algunas partes del proyecto parra que no de errores.Por ejemplo para esta versión Laravel requiere que se modifiquen los siguientes:

| Modificar   | Versiones |
| ----------  | --------- | 
| PHP            | 7 . 1 . 3   |
| fideloper / proxy | ~4.0|

 Tambien se crearán archivos nuevos por ejemplo :
 
| Crear   | Fichero |
| ----------  | --------- | 
| config/           | logging.php    |
| config/  | hashing.php|

3. Paso: Elegir la versión
Se debe acceder al [repositorio][https://github.com/laravel/laravel] oficial donde se podrá elegir la versión que deseamos actualizar.

Una vez elegida la versión se debe *modificar* el fichero `composer.json`.

```json
{
"name": "laravel/laravel",

"description": "The Laravel Framework.",

"keywords": ["framework", "laravel"],

"license": "MIT",

"type": "project",

"require": {

"php": ">=7.1.3",

"fideloper/proxy": "~4.0",

"laravel/framework": "5.6.*",

"laravel/tinker": "~1.0"

},
```

4. Comprobar actualización
Para **comprobar la actualización** de la versión, se debe usar el comando `composer update` que podrá descargar todas las librerías que seán necesarias para esta versión. 
