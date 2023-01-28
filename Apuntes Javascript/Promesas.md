# Función promesa

Una _Promesa_ (**`Promise`**) es un proxy de un valor que no necesariamente se conoce cuando se crea la promesa. Le permite asociar controladores con el valor eventual de éxito o el motivo de falla de una acción asíncrona. Esto permite que los métodos asíncronos devuelvan valores como los métodos síncronos: en lugar de devolver inmediatamente el valor final, el método asíncrono devuelve la promesa de proporcionar el valor en algún momento en el futuro.

## Ejemplo:

1.  Un “código productor” que hace algo y toma tiempo. Por ejemplo, algún código que carga los datos a través de una red. Eso es un “cantante”.
2.  Un “código consumidor” que quiere el resultado del “código productor” una vez que está listo. Muchas funciones pueden necesitar ese resultado. Estos son los “fans”.
3.  Una _promesa_ es un objeto JavaScript especial que une el “código productor” y el “código consumidor”. En términos de nuestra analogía: esta es la “lista de suscripción”. El “código productor” toma el tiempo que sea necesario para producir el resultado prometido, y la “promesa” hace que ese resultado esté disponible para todo el código suscrito cuando esté listo.

Un `Promise` está en uno de estos estados:

-   _pending (pendiente)_: estado inicial, ni cumplido ni rechazado.
-   _fulfilled (cumplida)_: lo que significa que la operación se completó con éxito.
-   _rejected (rechazada)_: lo que significa que la operación falló.

## [Promesas encadenadas](https://developer.mozilla.org/es/docs/Web/JavaScript/Reference/Global_Objects/Promise#promesas_encadenadas)

Los métodos [`Promise.prototype.then()`][(https://developer.mozilla.org/es/docs/Web/JavaScript/Reference/Global_Objects/Promise/then)], [`Promise.prototype.catch()`](https://developer.mozilla.org/es/docs/Web/JavaScript/Reference/Global_Objects/Promise/catch) y [`Promise.prototype.finally()`](https://developer.mozilla.org/es/docs/Web/JavaScript/Reference/Global_Objects/Promise/finally) se utilizan para asociar una acción posterior con una promesa que se establece.

### Metodo then
El método `.then()` toma hasta dos argumentos; el primer argumento es una función de devolución de llamada (_callback_) para el caso resuelto de la promesa, y el segundo argumento es una función de devolución de llamada para el caso rechazado. Cada `.then()` devuelve un objeto de promesa recién generado, que opcionalmente se puede usar para encadenar.

## Creación de una promesa

```javascript
//Una promesa tiene dos parámetros.
let miPromesa = new Promise(function(resolve, reject) {
  let camino = false;

  if (camino) {

    window.setTimeout(1000);

    resolved('Todo correcto');

  } else {

    window.setTimeout(1000);

    rejected('Hubo un error');

  }
});

console.log("hola");

miPromesa.then(valor => console.log(valor)).catch(error => console.log(error));

miPromesa.then(
  valor => console.log(valor),
  error => console.log(error)
);

console.log("hola1");
console.log("hola2");

```

La función pasada a `new Promise` se llama _ejecutor_. Cuando se crea `new Promise`, el ejecutor corre automáticamente. Este contiene el código productor que a la larga debería producir el resultado. En términos de la analogía anterior: el ejecutor es el “cantante”.

Sus argumentos `resolve` y `reject` son callbacks proporcionadas por el propio JavaScript. Nuestro código solo está dentro del ejecutor.

Cuando el ejecutor, más tarde o más temprano, eso no importa, obtiene el resultado, debe llamar a una de estos callbacks:

-   `resolve(value)` – si el trabajo finalizó con éxito, con el resultado `value`.
-   `reject(error)` – si ocurrió un error, `error` es el objeto error.

Para resumir: el ejecutor corre automáticamente e intenta realizar una tarea. Cuando termina con el intento, llama a `resolve` si fue exitoso o `reject` si hubo un error.

El objeto `promise` devuelto por el constructor `new Promise` tiene estas propiedades internas:

-   `state` – inicialmente `"pendiente"`; luego cambia a `"cumplido"` cuando se llama a `resolve`, o a `"rechazado"` cuando se llama a `reject`.
-   `result` – inicialmente `undefined`; luego cambia a `valor` cuando se llama a `resolve(valor)`, o a `error` cuando se llama a `reject(error)`.

## Capturando errores 

```js
const miPromesa = new Promise(function (resolved, rejected) {

  let camino = false;
  try {
  
    a = 4;
    resolved('Todo correcto');

  } catch (e) {

   rejected(e + ' Hubo un error');

  }

});


miPromesa.then(

  valor => console.log(valor),
  error => console.log(error)

);
```

## Lanzando Errores

```js
const miPromesa = new Promise(function (resolved, rejected) {

  let camino = false;
  try {

    let a = 4;
    resolved('Todo correcto');

  } catch (e) {

    rejected(`Nombre: ${e.name} Mensaje: ${e.message} Personalizado: Hubo un error` );

  }

});

miPromesa.then(

  valor => console.log(valor),
  error => console.log(error)

);
```

