## Instalar Nginx

```bash
sudo apt-get update
sudo apt-get install nginx
```

Tras la instalación se creará la carpeta `/etc/nginx/`.

## [ Administrar el proceso de Nginx](https://www.digitalocean.com/community/tutorials/how-to-install-nginx-on-ubuntu-20-04-es#paso-4-administrar-el-proceso-de-nginx)[](https://www.digitalocean.com/community/tutorials/how-to-install-nginx-on-ubuntu-20-04-es#paso-4-administrar-el-proceso-de-nginx)

Para detener su servidor web, escriba lo siguiente:

```bash
sudo systemctl stop nginx
```



Para iniciar el servidor web cuando no esté activo, escriba lo siguiente:

```bash
sudo systemctl start nginx
```


Para detener y luego iniciar el servicio de nuevo, escriba lo siguiente:

```bash
sudo systemctl restart nginx
```


Si solo está realizando cambios en la configuración, Nginx a menudo puede volver a cargase sin perder las conexiones. Para hacer esto, escriba lo siguiente:

```bash
sudo systemctl reload nginx
```


De forma predeterminada, Nginx está configurado para iniciarse automáticamente cuando lo haga el servidor. Si no es lo que quiere, deshabilite este comportamiento escribiendo lo siguiente:

```bash
sudo systemctl disable nginx
```


Para volver a habilitar el servicio de modo que se cargue en el inicio, puede escribir lo siguiente:

```bash
sudo systemctl enable nginx
```


Ya aprendió los comandos de administración básicos y debería estar listo para configurar el sitio para alojar más de un dominio.


## Crear host en local

Crear archivo de configuración `your_domain.local` dentro de `/etc/nginx/sites-available`.

-   `server_name`: URL del servidor que coincidirá con la indicada en el archivo de hosts.
-   `root`: Ruta de la carpeta que alberga el proyecto.


```js
server {
        listen 80;
        listen [::]:80;

        root /var/www/your_domain/;
        index index.html index.htm index.nginx-debian.html;

        server_name your_domain.local;

        location / {
                try_files $uri $uri/ =404;
        }

		location ~ \.php$ { 
		fastcgi_pass unix:/var/run/php/php7.4-fpm.sock; 
		fastcgi_param SCRIPT_FILENAME $realpath_root$fastcgi_script_name; 
		include fastcgi_params; 
		}
}
```

> Nota: Cambiar la versión de **PHP**  por la que se quiera utilizar.
> 

## Comprobar si existe algún error en los archivos de configuración

```bash
sudo nginx -c /etc/nginx/nginx.conf -t
```

## Situar carpeta del proyecto

Mover carpeta del proyecto a `/var/www/`.

> Si se ha indicado otra ruta en el archivo de configuración, asegurarse de que está en esa.


## Crear enlace simbólico

```bash
cd /etc/nginx/sites-enabled/
sudo ln -s ../sites-available/ejemplo.local .
```

> No olvidar poner ‘ .’ al final.

## Añadir dirección IP al archivo de hosts

Añadir al archivo `/etc/hosts`:

```bash
127.0.0.1   ejemplo.local
```


## Recargar servicio Nginx para aplicar los cambios

```bash
service nginx reload
```

## Permisos

Puede que la carpeta del proyecto situada en `/var/www/` (o cualquier otro sitio) necesite permisos sobre el usuario `www-data`:

```bash
sudo chown -R www-data:www-data /path/to/project
```


```bash
sudo chmod 775 -R /path/to/project
```

