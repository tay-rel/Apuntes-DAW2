# Instalación local proyecto en Wordpress con Duplicator

## Acceder a Wordpress desde la web
Para acceder al panel de administración se añade `wp-admin` a la url de la web.

```
https://ejemplo.com/wp-admin
```

Una vez allí se solicitarán las credenciales.

## Utilizar plugin Duplicator
**Duplicator** es un plugin gratuito para **WordPress** que permite a los usuarios crear una copia de su sitio web WordPress. Duplicator agrupa todos los archivos del sitio web en un único archivo zip portátil llamado "paquete". Se puede utilizar este paquete para migrar todo el sitio a un nuevo host, dominio o sitio de ensayo.
### Paso 0:

Comprueba que este activado en la sección de **Plugins** si no esta activalo.



### Paso 1:
Una vez dentro del panel administrativo, ir a **Dashboard > Duplicator**.


### Paso 2:
Accederás a la pantalla principal de configuración. Crea un paquete haciendo clic en el botón **Crear nuevo**.



> En esta sección es importante que no veas ningún paquete, ya que al terminar de realizar una copia y descargarla, ésta debe ser borrada para no ocupar espacio de memoria en el servidor. Si ves alguno, comunícalo para que se sepa y, por lo que, sí, deberás borrar tu copia una vez terminado el proceso.
{.is-info}

### Paso 3:
Proceder con la configuración del paquete.

- Asignar un nombre con el formato `fecha[YYYYMMDD]_nombredelcliente`.
- En los apartados `Archivo` y `Base de datos`, asegurar que no hay ningún filtro puesto.
- El apartado `Instalador` dejarlo tal cual.

### Paso 4:
El plugin realizará un escaneo del servidor, los archivos y la base de datos para detectar cualquier posible problema y, a continuación, hacer click en el _checkbox_ (en caso de que se haya detectado algún aviso) y continuar con el proceso.

> No es obligatorio que se superen todas las comprobaciones para compilar el paquete, pero es bueno tenerlas en cuenta, ya que pueden causar problemas en algunos casos.
{.is-info}

#### El proceso de creación del paquete se ha interrumpido
Puede ocurrir que el proceso de creación se interrumpa debido a que el paquete es demasiado pesado para la versión gratuita del plugin.

Para solucionarlo, podemos probar varias cosas. De hecho, el propio plugin te dice una serie de cosas que puedes intentar.

- Subir el `max_execution_time` en la configuración de PHP del dominio en el servidor. En este paso se puede solicitar la ayuda de un compañero para que realice la acción.
- Si el paso anterior no funciona, se pueden usar los filtros del apartado **Archivo**. Ahí podemos filtar rutas a archivos o carpetas que no queramos incluir en la copia. También se pueden filtrar archivos por extensión, etc. Una cosa recomendable, en este caso, sería filtrar la carpeta **Uploads**, ya que es donde se encuentran los archivos multimedia subidos y suele ser la carpeta de mayor tamaño. Luego tendríamos que descargar una copia de esta desde el explorador de archivos del dominio, en el servidor.
- Si nada funciona y no queda más remedio, se puede sacar una copia por **SFTP** y sacar un dump de la base de datos.

### Paso 5:
Ahora, el software hará una copia de seguridad de todo su sitio en un único archivo.

Tras completar el proceso, aparecerá un botón de descarga para un instalador (archivo .php) y para el archivo que acabas de crear (un archivo zip). Descargar ambos.


> Mover el archivo del instalador dentro de la carpeta del proyecto.
{.is-info}

### Ajustes finales

Al finalizar la creación del paquete, es importante asegurarse de borrarlo, como ya se mencionaba en la pantalla principal de configuración en el [Paso 2](#paso-2).

Además, si se han realizado cambios en la configuración de PHP, como los mencionados en [este paso](#el-proceso-de-creación-del-paquete-se-ha-interrumpido), hay que volver a sincronizar las suscripciones para que tengan todas la misma configuración de parámetros. Si no se sabe realizar esta tarea, solicitar la ayuda de alguien del equipo.

## Configurar Nginx
Necesitamos configurar un servidor para desplegar el proyecto de manera local. Puedes ver una guía sobre cómo configurar Nginx en Linux en el siguiente [enlace](https://wiki.holamundo.me/es/sistemas/nginx-configuracion-linux).

### Apache
Si estamos trabajando en Ubuntu, puede que tengamos **Apache** instalado, así que hay que asegurarse de que el servicio Apache esté desactivado para que no se genere conflicto con **Nginx**:

```
sudo service apache2 stop
```

Pero lo mejor es deshabilitar apache para que el servicio se inicie al arrancar el sistema:

```
sudo systemctl disable apache2
```

Habilitar apache: con este comando habilitamos el inicio automático de apache cuando reinicie el servidor.

```
sudo systemctl enable apache2
```

Comprobar ambos servicios, donde **Apache** deberá estar inactivo y **Nginx** activo:

```
sudo service apache2 status
sudo service nginx status
```

## Base de datos
La web también necesita de una base de datos local, que será una base de datos **MySQL**.

Es necesario crear una base de datos distinta a la de producción porque sino se estarían manipulando los datos reales con los que trabaja el cliente. Para que Duplicator cree la base de datos necesaria para WordPress, simplemente necesita una base de datos vacía donde volcar la copia. 

Para eso dejo el artículo de [Instalacion de Mysql](https://wiki.holamundo.me/es/practicas/mysql)


## PHP
Wordpress trabaja con PHP por lo que se necesita tener instalado en el sistema.

### Instalar PHP
En primer lugar, instalar las dependencias necesarias con el siguiente comando:
```
sudo apt-get install software-properties-common gnupg2 -y
```

Una vez instaladas todas las dependencias, añadir el repositorio PHP:
```
sudo add-apt-repository ppa:ondrej/php 
```

A continuación, actualizar el repositorio:
```
apt-get update -y 
```

Una vez que el repositorio esté actualizado, se puede proceder a instalar varias versiones de PHP, dependiendo de la versión que sea necesaria (*7.2, 7.4, 8.0, 8.1, 8.2*), sustituirla dentro del siguiente comando:

```
apt-get install php7.4 php7.4-fpm php7.4-cli -y
```

Comprobar que se ha instaldo correcamente:

```
php --version 
```

## Instalar página Wordpress

Una vez configurado el entorno y teniendo una copia del sitio, todo debería funcionar correctamente y debemos ir al navegador e ingresar a la dirección donde se encuentra el instalador:

> http://nombre_del_directorio.local/installer.php
> {.is-info}


### Iniciar despliegue

Tras el lanzamiento, se abrirá la fase de **Despliegue**:

Aceptar los términos y avisos y hacer click en **Siguiente**.

### Instalar base de datos
Aquí debemos crear una nueva base de datos para la nueva información, y, de hecho, ya la tenemos hecha en el [paso de MySQL](#crear-base-de-datos), así que utilizamos esa y las credenciales del usuario creado.

> En el apartado `Database` le asignas el nombre que tu veas conveniente este nombre se creara en la base de datos una vez le asignes el nombre.
{.is-info}


Tras introducir los datos hacemos click sobre el botón **Test Database** y una vez superado ir a **Siguiente**.

> Si se obtiene un error en este paso, ir a la pestaña **Options** y marcar la opción Legacy.
{.is-warning}

#### Error de collation

Si marcando la casilla **Legacy** seguimos teniendo problemas relacionados con la **collation** de la base de datos, por ejemplo el error donde **utf8_general_ci** marca **Fail** en el Test de conexión, debemos ir a la carpeta donde está la página `/var/www/example/dup-installer/` y buscar el fichero `dup-archive__[hash].txt`.

Lo abrimos con un editor de textos y en la parte superior de la estructura JSON, buscar la opción `collationList`.

En **"collationList"** podemos añadir/actualizar/cambiar la lista de intercalación para solucionar el problema temporalmente, así que en este caso, eliminamos `utf8_general_ci`.

### Comprobar sitio de prueba

Finalmente, este es el último paso del proceso, el paso "Sitio de prueba". Aquí verás que se eliminará el instalador al iniciar sesión como administrador.


> **Congrats!** La próxima vez que accedas a la dirección `http://[nombre_directorio/dominio]`, el sitio será idéntico al importado!

## Recursos
- https://wiki.holamundo.me/es/desarrollo/wordpress/Copia_de_produccion_a_test
- https://docs.themeisle.com/article/268-how-to-migrate-a-wordpress-site-from-one-server-to-another
- https://help.clouding.io/hc/en-us/articles/360021630059-How-to-Install-Multiple-PHP-Versions-7-2-7-4-8-0-and-8-1-on-Ubuntu-20-04
- https://wiki.holamundo.me/es/practicas/nginx-configuracion-linux




