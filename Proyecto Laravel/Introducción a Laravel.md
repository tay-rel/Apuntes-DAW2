Laravel usa un patrón de diseño llamado FrontController, consiste en que se tendrá un solo punto de entrada a la web que es a través del fichero **index.php** Este es un framework que no necesita el modelo MVC. Lleva incorporado el modelo  [[Binding]] donde inyecta en una variable el modelo asociado en Laravel.

## Creación de carpeta
Antes de instalar un proyecto en laravel se debe tener instalado previamente :
- Una aplicación [Composer][https://getcomposer.org/] que se encarga de generar un árbol de dependencia que revisa las librerías que se debe instalar en la aplicación de forma automática.

> **Note**: Es recomendable usar libreias que esten actualizadas
>[Packagist][https://packagist.org/]
>[Github][https://github.com/laravel/laravel]
>[Packalyst][https://packalyst.com/]

### Pasos desde Clase:

Como no nos deja entrar en el aula de clase porque no se tiene permisos de administrador, debemos entrar en workspace bash donde se encuentra ubicado el proyecto.
Y lo obtenemos en el contenedor y no necesito instalar en el anfitrión.
Para realizar esta instalación se debe uar el comando 
`sudo docker-compose exec workspace bash`.

![Instalación composer](https://lh5.googleusercontent.com/HZJjJOzHPcEsRkyFLCZgFELIF_YIil9x4IEd4vfUgc_vd219AGG0ULOa-gdJeiZIFHQ6L0H68SkgAhM4fkFhemIN1ANwnEAvXoYV7K6tWIdt2OEwkdYaF9BzpQ-jZ6mXRP7FSyauENgHI7qF6qqLt_nuC-__OK1lyYawX_zrVemPKqOTL7-1IhEVfVdlkQ)

*Requisitos*:
- Version de PHP: Para instalar la version se debe tener la versión de PHP 7.4. Para cambiar la versión se debe acceder a la carpeta Laradock y acceder en el fichero `.env` donde se modificara la linea `PHP_VERSION=7.4`. Para verificar que se ha instalado la versión de php correspondiente se debe utilizar el comando `php -v`.

- Instalación docker-coompose : Para poder instalar docker-compose se debe acceder a la documentación de [Laradock][ttps://laradock.io/documentation/]. Y usando el comando de `docker-compose build workspace`seguiremos los pasos de instalación.
 El comando `docker-compose build workspace` reconstruye el contenedor.

Una vez realizado este proceso se creara el proyecto de Laravel.

1. `composer create -project –prefer-dist laravel/laravel nombreProyecto “version”` crea el nuevo proyecto por ejemplo el nombre del proyecto será `proyecto13 "5.5."`. Se descarga una serie de librerías de texto plano.
2. Configuramos *NGINX* que se encuentra en la carpeta `laradock/nginx/sites` y creamos en nuevo servicio donde estara nuestro `proyecto13.conf` con las configuraciones correspondientes.

```
server_name proyecto13.local;
root /var/www/proyecto13/public;

error_log /var/log/nginx/proyecto13_error.log;
access_log /var/log/nginx/proyecto13_access.log;

```

3. Configuración de base de datos.

```
APP_URL = http://proyecto13.local
DB_CONNECTION = mysql
DB_HOST = mysql
DB_DATABADE = laraveltdd
DB_USERNAME = default
DB_PASSWORD = secret
```

Una vez hecho esto se debe ir a la `phpmyadmin` y crear una base de datos llamada de la misma manera que configuramos anteriomente a esta base de datos debe darse los permisos default ya que no se debe ejecutar con roort sino como otro usuario.
