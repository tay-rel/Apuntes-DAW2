
El objeto *Storage* (API de almacenamiento web) **nos permite almacenar datos de manera local** en el navegador y sin necesidad de realizar alguna conexión a una base de datos.

*localStorage y sessionStorage* son propiedades que acceden al objeto `Storage` y tienen la función de almacenar datos de manera local, la diferencia entre éstas dos es:

-  *localStorage* **almacena la información de forma indefinida o hasta que se decida limpiar** los datos del navegador.Se mantienen los datos aunque cierre el navegador.
> **nota:**
> localStorage solo almacena string

- *sessionStorage* **almacena información mientras la pestaña donde se esté utilizando siga abierta**, una vez cerrada, la información se elimina.

Aunque gran parte de los navegadores hoy en día son compatibles con el objeto `Storage`, no está de más hacer una pequeña validación para rectificar que realmente podemos utilizar dicho objeto, para ello utilizaremos el siguiente código:

```javaScript
if (typeof(Storage) !== 'undefined') {

} else {
 // Código cuando Storage NO es compatible
}
```


## a)Almacenar Valores

```js
let nombre = "Ignacio";
let apellidos = "García";
let id = "222";

  
window.localStorage.setItem("nombre", nombre);  
window.localStorage.setItem("apellidos", apellidos);  
localStorage.setItem("id", id);
```


## b) Obtener valores

```js
//"nombre" referencia a la variable creada antes
console.log(window.localStorage.getItem("nombre"));  

console.log(window.localStorage.getItem("apellidos"));  

console.log(window.localStorage.getItem("id"));

```

## c) Eliminar valores

```js
window.localStorage.removeItem("nombre");  

window.localStorage.removeItem("apellidos");  

window.localStorage.removeItem("id");
```

>**nota:**
>Tambien se usa clear()

## d) Guardar y recuperar

```js
let meses = ['Enero', 'Febrero', 'Abril'];
localStorage.setItem('meses', JSON.stringify(meses));

let mesesArray = JSON.parse(localStorage.getItem('meses'));

mesesArray.push('Noviembre');

console.log(mesesArray);

localStorage.setItem('meses', JSON.stringify(mesesArray));
```

```js
let miArray = ['valor1', 'valor2', 'valor3'];

localStorage.setItem('miArray', miArray);

let miArray2 = localStorage.getItem('miArray').split(',');
miArray2.push('hola');
console.log(miArray2);
```