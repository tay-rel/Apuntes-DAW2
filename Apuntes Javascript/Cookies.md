Las *cookies* son archivos que **contienen pequeños fragmentos de datos** (como nombre de usuario o contraseña) que se intercambian entre un equipo de usuario y un servidor web para identificar usuarios específicos y mejorar su experiencia de navegación.

**Por ejemplo:**

Los sitios de compra **utilizan cookies para hacer un seguimiento de los elementos que los usuarios han visto anteriormente**, usarlos para sugerir otros que les podrían interesar y guardar los elementos en el carrito de la compra mientras continúan comprando.

# Consulta de Cookies

Todos los navegadores nos permiten consultar las cookies que se almacenan por cada sitio Web. A continuación se muestran las cookies del sitio web [http://www.marca.com](http://www.marca.com) para Google Chrome y Firefox.

![](https://lh4.googleusercontent.com/F1ZKclCbqiGhJFL0xCEb2lRx9W6Qov-CjC7KQYrto687aMiRFHdSsgpTePFPx1vK7tlLHUGtbl10e2BKochyG1PYcNg1yvMbg_s1P7R_Fed7VyGvmeH3ymqBBs_ABpOMizT01Mcf9dOQ5bUywMe8tmIGLvlaB5GFsyLfvukFwetdeyIwjUN44XuMWA349w)

# Como utilizar cookies

Una cookie consiste en una cadena de texto (string) con varios pares `key=value` cada uno separado por `;`:

```js

<nombre>=<valor>; expires=<fecha>; max-age=<segundos>; path=<ruta>; domain=<dominio>; secure; httponly;

```

Desde el servidor, las cookies son creadas mediante la cabecera de respuesta `HTTP Set-Cookie`:

```js

Set-Cookie: <nombre>=<valor>; expires=<fecha>; max-age=<segundos>; path=<ruta>; domain=<dominio>; secure; httponly;

```

Un respuesta HTTP puede contener múltiples cabeceras [`Set-Cookie`][https://cybmeta.com/cookies-en-javascript], una por cada cookie. Por ejemplo, una cabecera de respuesta con creación de dos cookies podría ser:

```http
HTTP/1.0 200 OK
Content-type: text/html
Set-Cookie: colorPreference=blue
Set-Cookie: sessionToken=48745487; Expires=Thu, 01 Jan 2031 19:22:10 GMT
```

Una vez que la cookie ha sido creada, cada nueva solicitud que realice el usuario enviará la cookie en la cabecera de solicitud `HTTP Cookie`. En esta cabecera sólo se envían las cookies que sean válidas según los parámetros establecidos al crearla (expires, path, etc) y sólo se envía el par 
`<nombre>=<valor>`.

`path=<ruta>`
Opcional. Establece la ruta para la cuál la cookie es válida. Si no se especifica ningún valor, la cookie será **válida para la ruta la página actual**.

`domain=<dominio>`
Opcional. Dentro del dominio actual, subdominio para el que la cookie sea válida. El valor predeterminado es el subdominio actual. Establecer `domain=.miweb.com` para una cookie que sea válida para cualqueir subdominio (nota el punto delante del nombre del dominio). Por motivos de seguridad, los navegadores no permiten crear cookies para dominios diferentes al que crea la cookie (**same-origin policy**).

`secure`
Opcional. Parámetro sin valor. Si está presente la cookie sólo es válida para **conexiones encriptadas** (por ejemplo mediante protocolo HTTPS).

`HttpOnly`
Opcional. **Parámetro no disponible en JavaScript** ya que crea cookies válidas sólo para protocolo HTTP/HTTPS y no para otras APIs, incluyendo JavaScript.

```javascript
document.cookie = "nombrecookie=valorcookie; max-age=3600; path=/";
```

Cuándo se envían varias cookies, se envían todas separadas por `;` en una única cabecera `Cookie`. Por ejemplo, una cabecera HTTP de solicitud que envía las dos cookies anteriores podría ser:

```http
GET /ejemplo.html HTTP/1.1
Host: www.mysite.com
Cookie: colorPreference=blue; sessionToken=48745487
```



# Creación de cookies

`<nombre>=<valor>`.
Requerido. `<nombre>` es el nombre (key) que identifica la cookie y `<valor>` es su valor. A diferencia de las cookies en PHP, en JavaScript se puede crear una cookie con un valor vacío (`<nombre>=`).

Establecer una cookie en JavaScript es tan fácil como crear el string que define la cookie y asignarlo a `document.cookie`, **cada vez que se asigna una cookie a `document.cookie`, esta es añadida a la colección de cookies del documento**. Por ejemplo:

```js

//Al no especificar la fecha de expiración se creará una cookie de sesión
document.cookie = "nombre= Tay";
document.cookie = "apellido= Rel";

//devuelve un string con las cookies

let cookies = document.cookie;
console.log(cookies);


```

Si queremos crear varias cookies, tenemos que hacer este paso una vez para cada una. Por ejemplo, con el siguiente código se crearían las cookies `comida_favorita` y `color_favorito`:

```javascript
document.cookie = "comida_favorita=arroz; max-age=3600; path=/";
document.cookie = "color_favorito=amarillo";
```

Este comportamiento se debe a que `document.cookie` no es un dato con un valor (_data property_), sino una propiedad de acceso con métodos set y get nativos (_accessor property_). Cada vez que se le asigna una nueva cookie, no se sobrescriben las cookies anteriores sino que **la nueva se añade a la colección de cookies del documento**.

Recuerda que las cookies se envían en las cabeceras HTTP y, por tanto, deben estar correctamente codificadas. Puedes utilizar `encodeURIComponent()`, acostúmbrate a utilizarlo siempre para evitarte sorpresas:

```javascript
var testvalue = "Hola mundo!";
document.cookie = "testcookie=" + encodeURIComponent( testvalue );

//Segundo ejemplo

function crearCookie(clave, valor) {
 let valore = encodeURIComponent(valor);
 document.cookie = `${clave}=${valore}`;

}

crearCookie("marcaCoche","audi");
console.log(document.cookie);
```

>**Nota**:
>La función encodeURIComponent() codifica la URI sustituyendo cada instancia de ciertos caracteres por secuencias de escape.


# Fecha de expiración de una cookie

`expires=<fecha> y max-age=<segundos>`

 Ambos parámetros especifican el tiempo de validez de la cookie. `expires` establece una fecha (ha de estar en formato UTC) mientras que `max-age` establece una **duración máxima en segundos**. `max-age` toma preferencia sobre `expires`. Si no se especifica ninguno de los dos se creará una **session cookie**. Si es `max-age=0` o `expires=fecha-pasada` la cookie se elimina.
 
![](https://lh6.googleusercontent.com/tUMdaaSVtCW4oMXeitILBKL3Ju_1PZ7qRPAZYwcNGCswX9eot9vl_ZgdTgSufISgrumdZnOFNj9FI5tAvbrQDJdpTM60DMQO8ZRYSGiISu-hC-l5_7LxpBcMaIkvv9OzBpS-wKVx0dLyNbnM1_-Kd1CPuI0gEfAe0wM7rE4MJh-PtG9D9zvzBa_wLXG9fw)

Si se utiliza el parámetro `expires`, recuerda que ha de ser una fecha en formato UTC. Te puede ser de ayuda el método `Date.toUTCString()`. Por ejemplo, una cookie con caducidad para el 1 de Febrero del año 2068 a las 11:20:

```javascript
var expiresdate = new Date(2068, 1, 02, 11, 20);
var cookievalue = "Hola Mundo!";
document.cookie = "testcookie=" + encodeURIComponent( cookievalue ) + "; expires=" + expiresdate.toUTCString();
```

## Eliminar una cookie

Para eliminar una cookie desde JavaScript se debe **asignar una fecha de caducidad (`expires`) pasada o un `max-age` igual a cero**. En ambos casos da igual el valor que se le asigne a la cookie porque se eliminará pero ha de darse el nombre de la cookie aunque sea sin valor.

Por ejemplo, creamos la cookie con el identificador `nombre` y valor `Miguel` igual que antes:

```javascript
document.cookie = "nombre=Miguel";
```

Si queremos eliminarla:

```javascript
document.cookie = "nombre=; expires=Thu, 01 Jan 1970 00:00:00 UTC";

// O con max-age
document.cookie = "nombre=; max-age=0";
```

Al igual que ocurría con la modificación de cookies, **para la eliminación el path y el dominio también tienen que coincidir**:

```javascript
// Se crean dos cookies con el mismo identificador
// para dos paths diferentes
document.cookie = "nombre=Miguel; path=/noticias";
document.cookie = "nombre=Juan; path=/blog";

// Solo se elimina la cookie del path /noticias
document.cookie = "nombre=; max-age=0; path=/noticias";
```

Recuerda también que **si no se específica fecha de caducidad la cookie será válida sólo para la sesión actual**.

>**Nota:**
>El navegador borrara las cookies cuando expira la fecha. Si le ponemos una fecha pasada el navegador eliminará la cookie automáticamente.


# Modificar una cookie

Si la cookie asignada tiene **un identificador que ya existe**. En este caso se modifica la cookie existente en lugar de añadir una más.

Por ejemplo, podemos crear la siguiente cookie con identificador `nombre` y valor `Miguel`:

```javascript
document.cookie = "nombre=Miguel";
```

Si queremos modificar el valor, por ejemplo cambiarlo por Juan:

```javascript
document.cookie = "nombre=Juan";
```

Es importante tener en cuenta que si una cookie se crea para un dominio o para un path determinado y se quiere modificar, **el dominio y el path han de coincidir**. De lo contrario se crearán dos cookies diferentes válidas para cada path y dominio. Por ejemplo, imaginemos que estamos en «miweb.com/blog» (el valor predeterminado del path es en este caso `/blog`):

```javascript
// Supongamos que estamos en "miweb.com/blog"
// y creamos las siguientes cookies

// Creamos la cookie para el path "/"
document.cookie = "nombre=Miguel; path=/";

// Con la siguiente linea se crea una nueva cookie para el path "/blog" (valor por defecto)
// pero no se modifica la cookie "nombre" anterior porque era para un path diferente
document.cookie = "nombre=Juan";

// Con la siguiente línea SI se modifica la cookie "nombre" del path "/" correctamente
document.cookie = "nombre=Juan; path=/";
```

# Crear un objeto de todas las cookies