Existe una sintaxis especial para trabajar con [[Promesas]] de una forma más confortable, llamada “async/await”. Es sorprendentemente fácil de entender y usar.
# Funciones async

La declaración de función **`async`** define una _función asíncrona_, la cual devuelve un objeto [`AsyncFunction`](https://developer.mozilla.org/es/docs/Web/JavaScript/Reference/Global_Objects/AsyncFunction).

La palabra “async” ante una función significa solamente una cosa: que la función siempre devolverá una promesa. Otros valores serán envueltos y resueltos en una promesa automáticamente.

```javascript
async function f() {
  return 1;
}

f().then(alert); // 1
```

…Podríamos explícitamente devolver una promesa, lo cual sería lo mismo:

```javascript
async function f() {
  return Promise.resolve(1);
}

f().then(alert); // 1
```

>**Nota: **
>Entonces, `async` se asegura de que la función devuelva una promesa, o envuelve las no promesas y las transforma en una. Bastante simple, ¿correcto? Pero no solo eso. Hay otra palabra, `await`, que solo trabaja dentro de funciones `async` y es muy interesante.

# Funciones de await

El operador `await` es usado para esperar a una [[Promesas]] Sólo puede ser usado dentro de una función [`async function`](https://developer.mozilla.org/es/docs/Web/JavaScript/Reference/Statements/async_function).

La expresión `await` provoca que la ejecución de una función `async` sea pausada hasta que una `Promise` sea terminada o rechazada, y regresa a la ejecución de la función `async` después del término. Al regreso de la ejecución, el valor de la expresión `await` es la regresada por una promesa terminada.

`await` hace que JavaScript **espere hasta que la promesa responda** y devuelve su resultado.

Aquí hay un ejemplo con una promesa que resuelve en 1 segundo:

```javascript
async function f() {

  let promise = new Promise((resolve, reject) => {
    setTimeout(() => resolve("¡Hecho!"), 1000)
  });

  let result = await promise; // espera hasta que la promesa se resuelva (*)

  alert(result); // "¡Hecho!"
}

f();
```
