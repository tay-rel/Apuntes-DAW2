 Para desplegar un proyecto en local se debe hacer algunas instalaciones en este caso se harán las siguientes:

[Instalar ][[Nginx]]

Si estamos trabajando en Ubuntu, puede que tengamos **Apache** instalado, así que hay que asegurarse de que el servicio Apache esté desactivado para que no se genere conflicto con **Nginx**:

```bash
sudo service apache2 stop
```


Pero lo mejor es deshabilitar apache para que el servicio se inicie al arrancar el sistema:

```bash
sudo systemctl disable apache2
```

Habilitar apache: con este comando habilitamos el inicio automático de apache cuando reinicie el servidor.

```bash
sudo systemctl enable apache2
```

*Comprobar ambos servicios*, donde Apache deberá estar inactivo y Nginx activo:

```bash
sudo service apache2 status
sudo service nginx status
```

[Instalar ][[MySQL]]

## [Ajustar la autenticación y los privilegios de usuario (opcional)](https://www.digitalocean.com/community/tutorials/how-to-install-mysql-on-ubuntu-20-04-es#paso-3-ajustar-la-autenticacion-y-los-privilegios-de-usuario-opcional)

Para usar una contraseña para conectar con MySQL como **root**, deberá cambiar su método de autenticación de `auth_socket` a otro complemento, como `caching_sha2_password` o `mysql_native_password`. Para hacer esto, abra la consola de MySQL desde su terminal:

```bash
sudo mysql
```

A continuación, compruebe con el siguiente comando el método de autenticación utilizado por una de sus cuentas de usuario de MySQL:

```mysql

mysql> SELECT user,authentication_string,plugin,host FROM mysql.user;

```

En este ejemplo, puede ver que, en efecto, el usuario **root** se autentica utilizando el complemento `de auth_socket`. Para configurar la cuenta **root** para autenticar con una contraseña, ejecute una instrucción `ALTER USER` para cambiar qué complemento de autenticación utiliza y establecer una nueva contraseña.

```mysql
mysql> ALTER USER 'root'@'localhost' IDENTIFIED WITH caching_sha2_password BY 'password';
```

A continuación, ejecute `FLUSH PRIVILEGES` para indicar al servidor que vuelva a cargar la tabla de permisos y aplique sus nuevos cambios:

```mysql
mysql> FLUSH PRIVILEGES;
```

