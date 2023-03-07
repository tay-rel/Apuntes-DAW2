

![[Pasted image 20230212002705.png]]


# PHP con Nginx y MySQL 

# usando 

# Docker y Docker Compose


![[Pasted image 20230212002038.png]]




## Trabajo realizado por :

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~




	Alejandra Ariel Mayta Salazar
		2 º DAW - 2023
			MATERIA: DESPLIEGUE DE APLICACIONES WEB 
							




~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~










# Introducción

Docker es una herramienta de contenedorización de código abierto que **permite crear, desplegar y ejecutar aplicaciones mediante contenedores.**

Los contenedores permiten empaquetar una aplicación con todas las partes necesarias y desplegarla como un único paquete. Los contenedores son portátiles y pueden desplegarse en cualquier lugar. Un contenedor **aporta a los desarrolladores un entorno de trabajo uniforme** y racionalizado que puede compartirse fácilmente.


## Paso 1 - Instalar Docker y Docker Compose 

Por defecto, la última versión de Docker no está incluida en el repositorio de defecto de Ubuntu, por lo que se debe añadir el repositorio de Docker al sistema.

```bash

root@servermaster:/home/tay# apt-get install apt-transport-https ca-certificates curl tree software-properties-common -y

```

A continuación, añadir el repositorio Docker utilizando el siguiente comando:

```bash

root@servermaster:/home/tay# curl -fsSL https://download.docker.com/linux/ubuntu/gpg | apt-key add -
add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu focal stable"

```

Instalar Docker y Docker Compose utilizando el siguiente comando:

```bash

root@servermaster:/home/tay# apt-get install docker-ce docker-compose -y

```

## Estructura del directorio

```js

/root/docker-project/
├── docker-compose.yml
├── nginx
│   ├── default.conf
│   └── Dockerfile
├── php
│   └── Dockerfile
└── www
    └── html
        └── index.php

```

## Paso 2 - Crear un contenedor Nginx

 Se necesitará crear y lanzar un contenedor Nginx para alojar la aplicación PHP.
 
 ```bash

root@servermaster:/home/tay# mkdir ~/docker-project

```

Se debe crear un fichero llamado *docker-compose.yml*

 ```bash

root@servermaster:/home/tay/docker-project# nano docker-compose.yml

```

Para comprobar que funciona primeramente se debe probar a través de localhost.

 ```js 
 /docker-compose.yml

    nginx:   
      image: nginx:latest  
      container_name: nginx-container  
      ports:   
       - 8017:80

```

El archivo anterior descargará *la última imagen de Nginx*, creará un *nginx-container*, y lo expondrá en el puerto *8017*.

Para comprobarlo comprobaremos lanzando los contenedores con el siguiente comando: 

 ```bash

root@servermaster:/home/tay/docker-project# docker-compose up -d

```

Probamos que  el contenedor esta en ejecución con el siguiente comando:

 ```bash

root@servermaster:/home/tay/docker-project# docker ps

```

![](https://lh4.googleusercontent.com/riEtaV-qN8k9_kXaneC_-65LlJ8vhzNHT7r1QUaGb1qJRVz6YdU_DicwTlESY26Sx075tc718ckKnavlr4ybzSx_luv-jB_l07Q0hxrKUdfOJErhnMketKJmbVCHN1IHr9988oQlQTHVNijag6olShY)

Ahora vamos al navegador y podemos observar que esta funcionando .

![](https://lh3.googleusercontent.com/WclBwwAqCHdSIi7ZP3PVGnJHwSVpT6cjhFEKZRlPx2jdXLG4U2jACVb1TXFMhsJD_fkK5AORyRg_LLjsnEToStg_vvoHYsU96aln_b2nJp1H0tuq5J44lDq9RYnm85nPGMI0C0QWK-NqHvQgdNS_eT0)


## Paso 3 - Crear un contenedor PHP

 ```bash

root@servermaster:/home/tay# mkdir -p ~/docker-project/www/html

```

A continuación, cree un archivo index.php para verificar su versión de PHP.

 ```bash

root@servermaster:/home/tay/docker-project/www/html/# nano index.php

```

Y editaremos lo que queremos ver.

```html
<!DOCTYPE html>  
  <head>  
   <title>Hello World!</title>
  </head>  
  <body>  
   <h1>Hola mundo!Soy Ariel</h1>

   <p><?php echo 'We are running PHP, version: ' . phpversion(); ?></p>

  </body>
```

Se debe guardar el fichero y crear un directorio Nginx .

 ```bash

root@servermaster:/home/tay/docker-project/nginx/# nano default.conf

```

A continuación, crear un archivo de configuración por defecto de Nginx para ejecutar su aplicación PHP:

 ```bash

root@servermaster:/home/tay# mkdir ~/docker-project/nginx

```


Y debemos añadir las siguientes lineas.

 ```js
server {  

     listen 80 default_server;  
     root /var/www/html;  
     index index.html index.php;  

     charset utf-8;  

     location / {  
      try_files $uri $uri/ /index.php?$query_string;  
     }  

     location = /favicon.ico { access_log off; log_not_found off; }  
     location = /robots.txt { access_log off; log_not_found off; }  

     access_log off;  
     error_log /var/log/nginx/error.log error;  

     sendfile off;  

     client_max_body_size 100m;  

     location ~ .php$ {  
      fastcgi_split_path_info ^(.+.php)(/.+)$;  
      fastcgi_pass php:9000;  
      fastcgi_index index.php;  
      include fastcgi_params;  
      fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;  
      fastcgi_intercept_errors off;  
      fastcgi_buffer_size 16k;  
      fastcgi_buffers 4 16k;  
    }  

     location ~ /.ht {  
      deny all;  
     }  
    }

```

Se debe crear un Dockerfile dentro del directorio nginx. Esto copiará el archivo de configuración por defecto de Nginx al contenedor Nginx.


 ```bash

root@servermaster:/home/tay/docker-project/nginx/# nano Dockerfile

```

Añadimos lo siguiente

 ```mysql

FROM nginx:latest   

# Install MySQL client
RUN apk add --update mysql-client

    COPY ./default.conf /etc/nginx/conf.d/default.conf
    COPY /docker-project/index.php /var/ww/html
    #Copia el fichero index.html para el funcionamiento de la práctica.
    COPY /docker-project/index.html /var/ww/html
      
# Copy the Docker Compose file to the container
COPY docker-compose.yml /root/docker-project

	RUN docker-php-ext-install pdo_mysql mbstring zip exif pcntl
	RUN docker-php-ext-configure gd --with-gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ --with-png-dir=/usr/include/
	RUN docker-php-ext-install gd

# Install composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer
```

Editar el archivo docker-compose.yml:

 ```js

nginx:   
   image: nginx:latest  
   container_name: nginx-container  
   ports:   
    - 8017:80
nginx:
   build: ./nginx/
   container_name: nginx-container
   ports:
   - 8017:80
   links:
   - php
   volumes:
   - ./www/html/:/var/www/html/
   networks:
     custom_network:
       ipv4_address: 172.20.254.153
php:
   image: php:7.0-fpm
   container_name: php-container
   expose:
   - 9000
   volumes:
   - ./www/html/:/var/www/html/
app-data:
   image: php:7.0-fpm  
   container_name: app-data-container  
   volumes:
    - ./www/html/:/var/www/html/  
   command: "true"
mysql:    
   image: mysql:5.7  
   container_name: mysql-container  
   volumes_from:
    - mysql-data
   networks:
     custom_network:
       ipv4_address: 172.20.254.152
   environment:
    MYSQL_ROOT_PASSWORD: secret
    MYSQL_DATABASE: cierva
    MYSQL_USER: pepito
    MYSQL_PASSWORD: cierva
mysql-data:
   image: mysql:5.7  
   container_name: mysql-data-container  
   volumes:  
    - /var/lib/mysql  
   command: "true"
networks:
  custom_network:
driver: bridge
  ipam:
       config:
         - subnet: 172.20.254.0/24

```


El archivo anterior creará un nuevo contenedor php, expondrá PHP-FPM en el puerto *9000*, vinculará el contenedor nginx al contenedor php, creará un volumen y lo montará en el contenedor para que todo el contenido esté sincronizado con el directorio */var/www/html/* del contenedor.

Terminamos de configurar el fichero *index.php*.

```php
<!DOCTYPE html>  
  <head>  
   <title>Hello World!</title>
  </head>  
  <body>  
   <h1>Hola mundo!Soy Ariel</h1>

   <p><?php echo 'We are running PHP, version: ' . phpversion(); ?></p>
   <?  
     function getdb() {

     $db = pg_connect('host=172.20.254.152 port=5432 dbname=cierva user=pepito

     password=cierva connect_timeout=8') or die('connection failed');

     return $db;

     }
     echo getdb();
     phpinfo();
  
     ?>

  </body>
```

![](https://lh6.googleusercontent.com/3A5jkcdy8XeUx5ZVbE6mqitFSrdY5bLV1o02lGKxBVqv9dkpHcm3IvKEwjEL3JLjbnqdH9B4Qg5ROLKgCyVJBPR-FxuZ9cR3lpfvAu6jCI1LBdZOH2ZdijCVe7ffVdIt2F4kgAa-olCq7c5v71WOC8k)


Volvemos a verificar que se estaejcutando:

 ```bash

root@servermaster:/home/tay/docker-project# docker ps

```

![](https://lh6.googleusercontent.com/bblGEyt_7fUE7m2kNBvtaDET4HqdSv5zXj7SD6gIGh-mf7fcl413xmgLU4HfrwHEEd8_UQTo7s4J6U-PANIeUM1ww3M1m-xa6Q_byLT_jNcUTK8at9FBL43VbvNiIhyjgEvlaFmvum9kRzkSjPMFIXo)

![](https://lh5.googleusercontent.com/CdQm5txHndGeFpfgNpK0kqmcyZc6Ug9ZYZucPc0Hh9ce-EHDfJ-LgcMUhzDr-h2_jjinYWaI7-_oy0w8ullDQyA1nd11Qbwy7jNDdV7x9hG0ysd_GCclIpa7GhDXgo0OIzmqbqVLTaSQRwhvWFJLTOg)
