## Instalar MySQL

```none
sudo apt update
sudo apt install mysql-server
```


### Comprobar versión MySQL

Una vez logueados dentro de la consola de mysql:

```none
SHOW VARIABLES LIKE "%version%";
```


Si queremos tener instancias adicionales de MySQL con diferentes versiones en un mismo sistema, se explica en [este artículo](https://wiki.holamundo.me/es/desarrollo/MySQL/mysql-instalar-multiples-versiones).

## Configurar MySQL

```none
sudo mysql_secure_installation
```


Esto lo guiará a través de una serie de instrucciones mediante las cuales podrá realizar cambios en las opciones de seguridad de su instalación de MySQL.En la primera solicitud preguntará si desea configurar el complemento de validación de contraseña, que puede usar para probar la seguridad de la contraseña de MySQL.

Si decides configurar el complemento para validar la contraseña, la secuencia de comandos le solicitará elegir un nivel de validación de contraseña. Para el nivel más alto, que se selecciona ingresando `2`.

> La contraseña debe ser como mínimo 8 caracteres y debe incluir una combinación de mayúsculas, minúsculas y números y caracteres especiales.

En los sistemas Ubuntu con MySQL 5.7 (y versiones posteriores), el usuario root de MySQL **se configura para la autenticación usando el complemento** `auth_socket` de manera predeterminada en lugar de una contraseña. Esto en muchos casos proporciona mayor seguridad y utilidad, pero también puede generar complicaciones cuando deba permitir que un programa externo (como phpMyAdmin) acceda al usuario.

Ver [Autenticación y privilegios de un usuario](https://wiki.holamundo.me/es/desarrollo/MySQL/mysql-native-password).

Independientemente de que opte por configurar el complemento para validar la contraseña, la siguiente solicitud tendrá que ver con establecer una contraseña para el usuario `root` de MySQL.

## Crear un nuevo usuario y otorgar permisos en MySQL

Por otra parte, para el flujo de trabajo de algunos puede resultar más conveniente la conexión a MySQL con un usuario dedicado. Para crear dicho usuario, abra el shell de MySQL de nuevo:

```bash
sudo mysql
```

-   Acceder a MySQL desde la terminal con el usuario `root` y su contraseña:

```none
mysql -u root -p
```


-   Crear nuevo usuario:

```none
CREATE USER 'newuser'@'localhost' IDENTIFIED BY 'password';
```


-   Dar permisos al nuevo usuario omo `INSERT`, `UPDATE` o `DELETE`.

```none
GRANT ALL PRIVILEGES ON * . * TO 'newuser'@'localhost';
```

>NOTA:Debido a que creó un nuevo usuario, en vez de modificar uno existente, `FLUSH PRIVILEGES` no es necesario aquí.

-   Asignar permisos:

```none
FLUSH PRIVILEGES;
```


## Crear base de datos

Entrar con el nuevo usuario y contraseña: `mysql -u newuser -p`:

```mysql
mysql> CREATE DATABASE DATABASE_NAME;
```

## Bibiografía

[Desinstalar Mysql](https://www.solvetic.com/tutoriales/article/8878-desinstalar-mysql-en-ubuntu-20-04/)