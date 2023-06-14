# Instalación PHP 

Antes de instalar PHP debes comprobar si lo tienes ya instalado con este comando podemos comprobarlo.

```bash
php -v

# Te saldra algo parecido 

PHP 8.1.2-1ubuntu2.11 (cli) (built: Feb 22 2023 22:56:18) (NTS)
Copyright (c) The PHP Group
Zend Engine v4.1.2, Copyright (c) Zend Technologies
    with Zend OPcache v8.1.2-1ubuntu2.11, Copyright (c), by Zend Technologies
```

Si te muestra la version es probable que lo tengas instalado.Siendo así procedemos a instalar las extenciones.

Por otro lado sino te ha salido la version ,deberas instalarlo con el siguiente comando .

```bash
sudo apt-get install php
```
Luego vuelves a ejecutar el comando `php -v` y te saldra la version , prosigue con las extensiones.

Si quieres instalar un version en especifica puedes seguir los siguientes paso de [versiones php](instalar-otra-versión-de-php-o-trabajar-con-varias-versiones).

## Archivo php.ini

En este archivo podemos editar y añadir configuraciones a nuestra versión de PHP.

Si queremos saber dónde se encuentra el archivo de configuración para la version de PHP en uso, ejecutar el comando:

```
php -i | grep php.ini
```

que tendrá una salida similar a la siguiente, dependiendo de la versión:

```
Configuration File (php.ini) Path => /etc/php/7.0/cli
Loaded Configuration File => /etc/php/7.0/cli/php.ini
```

# Extensiones

Para listar los módulos de **PHP** que se están utilizando en el servidor desde la terminal, se puede utilizar el siguiente comando:

```bash
php -m
```
Este comando mostrará una lista de todos los módulos de **PHP** que se están cargando en el servidor.

Para filtrar la lista de módulos y encontrar un módulo en concreto, se puede utilizar el siguiente comando:

```shell
php -m | grep <nombre_del_modulo>
```

## Instalación de una version de PHP

Si instalamos una nueva versión de PHP y queremos instalar sus extensiones rápidamente, dejo por aqui el comando para instalar de golpe las más comunes:

```apt install php7.0-SimpleXML php7.0-ssh2 php7.0-xml php7.0-xmlreader php7.0-curl php7.0-exif php7.0-ftp php7.0-gd php7.0-iconv php7.0-json php7.0-mbstring php7.0-mysqli php7.0-posix php7.0-sockets php7.0-tokenizer php7.0-soap php7.0-dom ```
> Cambiar 7.0 por la versión correspondiente
{.is-warning}

```bash
sudo apt install php7.4-{fpm,bcmath,ctype,fileinfo,mbstring,pdo,tokenizer,xml,curl,zip,gmp,gd,mysqli,mysql}
```

### PHP 8.2

```php
sudo apt install php8.2-{fpm,bcmath,fpm,xml,mysql,zip,intl,ldap,gd,cli,bz2,curl,mbstring,pgsql,opcache,soap,cgi,pdo}
```

# Instalar otra versión de php o trabajar con varias versiones

1. Verificar versión
`sudo apt show php` o `php -v`

1. Instalar dependencias
`sudo apt install software-properties-common`

1. Agregar repositorio:
`sudo add-apt-repository ppa:ondrej/php`

1. Actualizar
`sudo apt update`

1. Instalar versiones (ejemplos):
```
sudo apt update
sudo apt install php5.6
sudo apt install php7.2
sudo apt install php8.0
php -v
```

1. Elegir la versión predeterminado trabajar:

`sudo update-alternatives --set php /usr/bin/php7.4`

Si quieres saber las versiones que tienes en tu equipo puedes buscar en la carpeta /usr/bin:

`cd /usr/bin/`

Ahora para decirle a Apache que use una determinada versión desactivamos la versión actual escribimos:

`sudo a2dismod php7.2`

Luego de ésto debemos habilitar la versión de PHP que deseemos:

`sudo a2enmod php7.4`

`sudo systemctl restart apache2`

Según la versión que necesitemos para trabajar en nuestro proyecto la podemos configurar de aquí en adelante tecleando unos cuantos comandos en nuestra terminal y pasar de una versión de PHP a otra versión. 

[Fuente](https://diarioprogramador.com/como-instalar-distintas-versiones-de-php-en-ubuntu/)




# Como crear un handler customizado de PHP a mano para Plesk

Esto solo haría falta si en el Plesk Installer no nos aparece disponible ya la versión que nos hace falta, por que haya sido deprecada.

Para Cáritas, por ejemplo, nos hizo falta ya que su web tenia algunas partes (conexion con el WebService SICCE) que solo funcionaban con esta versión.

En este ejemplo, en vez de seguir la guía de Plesk para hacerlo, en vez de compilar PHP nosotros como nos dice la guía, vamos a instalar PHP de forma normal desde los repositorios oficiales y posteriormente registrar esta versión en Plesk.

Primero instalamos el repositorio oficial de PHP de Ubuntu o debian según use el servidor.

Ubuntu: https://launchpad.net/~ondrej/+archive/ubuntu/php
Debian: https://deb.sury.org/#debian-dpa

Una vez tengamos los repositorios instalados y la lista de paquetes actualizada, instalamos la versión de PHP que queramos , por ej: php 7.0

Instalación de PHP 7.0 y su versión FPM: `apt install de php7.0 y php7.0-fpm`

Instalación de todas las extensiones para esta versión de PHP (Recomendado): `apt install php7.0-gmagick php7.0-SimpleXML php7.0-ssh2 php7.0-xml php7.0-xmlreader php7.0-curl php7.0-date php7.0-exif php7.0-filter php7.0-ftp php7.0-gd php7.0-hash php7.0-iconv php7.0-imagick php7.0-json php7.0-libxml php7.0-mbstring php7.0-mysqli php7.0-openssl php7.0-pcre php7.0-posix php7.0-sockets php7.0-SPL php7.0-tokenizer php7.0-zlib php7.0-soap`

> Cambiar 7.0 por la versión de PHP que queramos instalar
{.is-warning}

Posteriormente, necesitamos registrar esta version de PHP que acabamos de instalar en el sistema en Plesk, con un comando que nos ofrece, es el siguiente (adaptar al gusto y a la versión de PHP y posibles cambios/cambios de ruta que puedan ocurrir en el futuro): `plesk bin php_handler --add -displayname 7.0.33-fpm-custom -path /usr/sbin/php-fpm7.0 -phpini /etc/php/7.0/fpm/php.ini -type fpm -id 7.0.33-fpm-custom -clipath /usr/bin/php7.0 -service php7.0-fpm -poold /etc/php/7.0/fpm/pool.d`

Y ya deberiamos tenerlo, si accedemos a Plesk, deberia aparecernos en el selector de versiones de PHP de cualquier dominio, o en el manager de versiones de PHP general, con el nombre (en el caso del comando de ejemplo): "7.0.33-fpm-custom".

## Bibliografía 

[Instalación Php](https://code.tutsplus.com/es/tutorials/how-to-install-php-in-ubuntu--cms-35237)
