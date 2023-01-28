[Map](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Map) es, al igual que ` Object `, una colección de datos identificados por claves. La principal diferencia es que ` Map ` permite claves de cualquier tipo.

|Propiedad o Método  | Descripción|
|---------|----------|
|   [`new Map()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Map/Map) –| crea el mapa.|
   |[`map.set(clave, valor)`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Map/set)) –| almacena el valor asociado a la clave.|
   |[`map.get(clave)`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Map/get) –| devuelve el valor de la clave. Será `undefined` si la `clave` no existe en map.|
   |[`map.has(clave)`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Map/has) – |devuelve `true` si la `clave` existe en map, `false` si no existe.|
   |[`map.delete(clave)`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Map/delete) –| elimina el elemento con esa clave.|
   |[`map.clear()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Map/clear) –| elimina todo de map.|
   |[`map.size`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Map/size) – |tamaño, devuelve la cantidad actual de elementos.|

## Ejemplo:

```javascript
let map = new Map();

map.set('1', 'str1');   // un string como clave
map.set(1, 'num1');     // un número como clave
map.set(true, 'bool1'); // un booleano como clave


alert( map.get(1)   ); // 'num1'
alert( map.get('1') ); // 'str1'

alert( map.size ); // 3
```

>**Nota**
>¿recuerda el objeto regular? convertiría las claves a string.
 Map mantiene el tipo de dato en las claves, por lo que estas dos son diferentes:

## [Iteración sobre Map](https://es.javascript.info/map-set#iteracion-sobre-map)
 
|Propiedad o Método|Descripción|
|----|----|
   |[`map.keys()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Map/keys) –- |devuelve un iterable con las claves.|
   |[`map.values()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Map/values) -– |devuelve un iterable con los valores.|
  |[`map.entries()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Map/entries) -– |devuelve un iterable para las entradas `[clave, valor]`. Es el que usa por defecto en `for..of`.|

## Ejemplo:

```javascript
let recipeMap = new Map([
  ['pepino', 500],
  ['tomates', 350],
  ['cebollas',    50]
]);

// iterando sobre las claves (verduras)
for (let vegetable of recipeMap.keys()) {
  alert(vegetable); // pepino, tomates, cebollas
}

// iterando sobre los valores (precios)
for (let amount of recipeMap.values()) {
  alert(amount); // 500, 350, 50
}

// iterando sobre las entradas [clave, valor]
for (let entry of recipeMap) { // lo mismo que recipeMap.entries()
  alert(entry); // pepino,500 (etc)
}
```

## [*Convertir][https://bobbyhadz.com/blog/javascript-convert-object-to-map] un [[Objeto]] a un map*

Para convertir un objeto a un Map se debe usar:
- **Object.entries()**, metodo que da un array de  pares de clave-valor.
- Pasa el resultado al constructor del Map.
- El nuevo Map contendra todo el objeto de pares de clave-valor.

```javascript
//Primero
const obj = {
  id: 1,
  name: 'bobby',
};

const map1 = new Map(Object.entries(obj));
console.log(map1); // 👉️ {'id' => 1, 'name' => 'bobby'}

//segundo
let obj = {
id: 1,
name: 'bobby',
};

const map1 = new Map(Object.entries(obj));
console.log(map1);
  
for (const key in obj) {
console.log(`${key}: ${obj[key]}`);

}
```

## *[Convertir][https://bobbyhadz.com/blog/javascript-convert-map-to-object]  un [[Objeto]] a un map*

El objeto `WeakMap` es una colección de pares llave/valor en la que las llaves deben ser objetos con valores de cualquier [tipo de dato en JavaScript](https://developer.mozilla.org/es/docs/Web/JavaScript/Data_structures#javascript_types), y el cual no crea fuertes referencias a sus llaves. Es decir, la presencia de un objeto como llave en un `WeakMap` no evita que el objeto sea recolectado como basura. Una vez que se ha recopilado un objeto utilizado como llave, sus valores correspondientes en cualquier `WeakMap` se convierten en candidatos para la recolección de basura.

>**Nota:**
No pueden ser valores primitivos, sino objetos .

```javascript
let weakMap = new WeakMap();

let obj = {};

weakMap.set(obj, "ok"); // funciona bien (la clave es un objeto)

// no puede usar un string como clave
weakMap.set("test", "Whoops"); // Error, porque "test" no es un objeto
```

Ahora, si usamos un objeto como clave y no hay otras referencias a ese objeto, se eliminará de la memoria (y del map) automáticamente.

```javascript
let john = { name: "John" };

let weakMap = new WeakMap();
weakMap.set(john, "...");

john = null; // sobreescribe la referencia

// ¡John se eliminó de la memoria
```

`WeakMap` no admite la iteración ni los métodos `keys()`, `values()`, `entries()`, así que no hay forma de obtener todas las claves o valores de él.

`WeakMap` tiene solo los siguientes métodos:

-   [`weakMap.set(clave, valor)`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/WeakMap/set)
-   [`weakMap.get(clave)`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/WeakMap/get)
-   [`weakMap.delete(clave)`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/WeakMap/delete)
-   [`weakMap.has(clave)`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/WeakMap/has)

## Ejemplo:

Para almacenar una fecha, podemos usar `WeakMap`:

```javascript
let messages = [
  {text: "Hello", from: "John"},
  {text: "How goes?", from: "John"},
  {text: "See you soon", from: "Alice"}
];

let readMap = new WeakMap();

readMap.set(messages[0], new Date(2017, 1, 1));
// // Objeto Date que estudiaremos más tarde
```