# Función Callback

Una función de callback **es una función que se pasa a otra función como un argumento**, que luego se invoca dentro de la función externa para completar algún tipo de rutina o acción.

## Ejemplo:

Es un callback sincrónica , ya que se ejecuta inmediatamente.

```js
function saludar(nombre) {
  alert('Hola ' + nombre);
}

function procesarEntradaUsuario(callback) {
  var nombre = prompt('Por favor ingresa tu nombre.');
  callback(nombre);
}

procesarEntradaUsuario(saludar);
```

Muchas funciones son proporcionadas por el entorno de host de Javascript que permiten programar acciones _asíncronas_. En otras palabras, acciones que iniciamos ahora, pero que terminan más tarde.

Echa un vistazo a la función `loadScript(src)`, que carga un código script `src` dado:

```javascript
function loadScript(src) {
  // crea una etiqueta <script> y la agrega a la página
  // esto hace que el script dado: src comience a cargarse y ejecutarse cuando se complete
  let script = document.createElement('script');
  script.src = src;
  document.head.append(script);
}
```

Esto inserta en el documento una etiqueta nueva, creada dinámicamente, `<script src =" ... ">` con el código `src` dado. El navegador comienza a cargarlo automáticamente y lo ejecuta cuando la carga se completa.

Esta función la podemos usar así:

```javascript
// cargar y ejecutar el script en la ruta dada
loadScript('/my/script.js');
```

El script se ejecuta “asincrónicamente”, ya que comienza a cargarse ahora, pero se ejecuta más tarde, cuando la función ya ha finalizado.

El código debajo de `loadScript (...)`, no espera que finalice la carga del script.

```javascript
loadScript('/my/script.js');
// el código debajo de loadScript
// no espera a que finalice la carga del script
// ...
```

Digamos que necesitamos usar el nuevo script tan pronto como se cargue. Declara nuevas funciones, y queremos ejecutarlas.

Si hacemos eso inmediatamente después de llamar a `loadScript (...)`, no funcionará:

```javascript
loadScript('/my/script.js'); // el script tiene a "function newFunction() {…}"

newFunction(); // no hay dicha función!
```

Naturalmente, el navegador probablemente no tuvo tiempo de cargar el script. Hasta el momento, la función `loadScript` no proporciona una forma de rastrear la finalización de la carga. El script se carga y finalmente se ejecuta, eso es todo. Pero nos gustaría saber cuándo sucede, para usar las funciones y variables nuevas de dicho script.

Agreguemos una función `callback` como segundo argumento para `loadScript` que debería ejecutarse cuando se carga el script:

```javascript
function loadScript(src, callback) {
  let script = document.createElement('script');
  script.src = src;

  script.onload = () => callback(script);

  document.head.append(script);
}
```

El evento `onload`, que se describe en el artículo [Carga de recursos: onload y onerror](https://es.javascript.info/onload-onerror#loading-a-script), básicamente ejecuta una función después de que el script es cargado y ejecutado.

Ahora, si queremos llamar las nuevas funciones desde el script, deberíamos escribirlo en la callback:

```javascript
loadScript('/my/script.js', function() {
  // la callback se ejecuta luego que se carga el script
  newFunction(); // ahora funciona
  ...
});
```

Esa es la idea: el segundo argumento es una función (generalmente anónima) que se ejecuta cuando se completa la acción.

Aquí un ejemplo ejecutable con un script real:

```javascript
function loadScript(src, callback) {
  let script = document.createElement('script');
  script.src = src;
  script.onload = () => callback(script);
  document.head.append(script);
}

loadScript('https://cdnjs.cloudflare.com/ajax/libs/lodash.js/3.2.0/lodash.js', script => {
  alert(`Genial, el script ${script.src} está cargado`);
  alert( _ ); // _ es una función declarada en el script cargado
});
```

Eso se llama programación asincrónica “basado en callback”. Una función que hace algo de forma asincrónica debería aceptar un argumento de `callback` donde ponemos la función por ejecutar después de que se complete.

Aquí lo hicimos en `loadScript`, pero por supuesto es un enfoque general.

El script se ejecuta “asincrónicamente”, ya que comienza a cargarse ahora, pero se ejecuta más tarde, cuando la función ya ha finalizado.

El código debajo de `loadScript (...)`, no espera que finalice la carga del script.

```javascript
loadScript('/my/script.js');
// el código debajo de loadScript
// no espera a que finalice la carga del script
// ...
```

Digamos que necesitamos usar el nuevo script tan pronto como se cargue. Declara nuevas funciones, y queremos ejecutarlas.

Si hacemos eso inmediatamente después de llamar a `loadScript (...)`, no funcionará:

```javascript
loadScript('/my/script.js'); // el script tiene a "function newFunction() {…}"

newFunction(); // no hay dicha función!
```

Naturalmente, el navegador probablemente no tuvo tiempo de cargar el script. Hasta el momento, la función `loadScript` no proporciona una forma de rastrear la finalización de la carga. El script se carga y finalmente se ejecuta, eso es todo. Pero nos gustaría saber cuándo sucede, para usar las funciones y variables nuevas de dicho script.

Agreguemos una función `callback` como segundo argumento para `loadScript` que debería ejecutarse cuando se carga el script:

```javascript
function loadScript(src, callback) {
  let script = document.createElement('script');
  script.src = src;

  script.onload = () => callback(script);

  document.head.append(script);
}
```

El evento `onload`, que se describe en el artículo [Carga de recursos: onload y onerror](https://es.javascript.info/onload-onerror#loading-a-script), básicamente ejecuta una función después de que el script es cargado y ejecutado.

Ahora, si queremos llamar las nuevas funciones desde el script, deberíamos escribirlo en la callback:

```javascript
loadScript('/my/script.js', function() {
  // la callback se ejecuta luego que se carga el script
  newFunction(); // ahora funciona
  ...
});
```

Esa es la idea: el segundo argumento es una función (generalmente anónima) que se ejecuta cuando se completa la acción.

Aquí un ejemplo ejecutable con un script real:


```javascript
function loadScript(src, callback) {
  let script = document.createElement('script');
  script.src = src;
  script.onload = () => callback(script);
  document.head.append(script);
}

loadScript('https://cdnjs.cloudflare.com/ajax/libs/lodash.js/3.2.0/lodash.js', script => {
  alert(`Genial, el script ${script.src} está cargado`);
  alert( _ ); // _ es una función declarada en el script cargado
});
```

Eso se llama programación asincrónica “basado en callback”. Una función que hace algo de forma asincrónica debería aceptar un argumento de `callback` donde ponemos la función por ejecutar después de que se complete.

Aquí lo hicimos en `loadScript`, pero por supuesto es un enfoque general.

Cuando hacemos una petición de un recurso, a día de hoy se hace de forma asíncrona. Es decir pedimos y cuando el recurso lo tenemos disponible nos avisa y será entonces cuando lo tratemos. Ya hemos usado promesas con la API Rest. 

  
Se utilizan promesas cuando no sabes cuando se ejecuta el código.Se utilizan en llamadas apirest. Además una promesa puede o no fallar 

```js
console.log('hola1');

fetch('https://jsonplaceholder.typicode.com/posts/1')

  .then(response => response.json()) //dos promesas anidadas

  .then(json => console.log(json));

console.log('hola2');
```

En una promesa hay dos caminos posibles, que todo vaya bien o que todo vaya mal:

-   then(función_ok, función_ko)
-   then(función_ok).catch(función_ko)
  

En el siguiente ejemplo le hemos asignado una función en caso de éxito y otra en caso de error. 

```js
fetch('https://jsonplaceholder.typicode.com/posts/1')

  .then(response => console.log("se ha ejecutado correctamente")

  .catch(response => console.log("ha habido un error")));
```
