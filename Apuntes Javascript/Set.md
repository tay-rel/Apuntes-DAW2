# [¿Qué son los Set?](https://lenguajejs.com/javascript/set-map/que-es-set-weakset/#qué-son-los-set)

Los **Set** en Javascript son estructuras de datos nativas muy interesantes para representar conjuntos de datos. La característica principal es que los datos insertados **no se pueden repetir**, por lo que si intentamos insertar un valor que ya existe ,no se insertara de nuevo por lo que con un set nos aseguramos de que no tendrá el mismo valor almacenado.

Un Set es una colección de tipo especial:*“conjunto de valores”* (sin claves), donde cada valor puede aparecer solo una vez. Respeta el orden de inserción

>*Nota:*
>Los set son iterables ,  esto quiere decir que se recorren como una array.

|Propiedad o Método | Descripción|
|---------------------|-------------|
|.size `number`|Propiedad que devuelve el número de elementos que tiene el conjunto.|
|.add ()`set`|Añade un elemento al conjunto (si no está repetido) y devuelve el set. **Muta**|
|.has ()`boolean`|Comprueba si `element` ya existe en el conjunto. Devuelve si existe.|
|.delete() `boolean`|Elimina el `element` del conjunto. Devuelve si lo eliminó correctamente.|
|.clear()|Vacía el conjunto completo.|

## Uso de métodos

*Size*: numero de elementos que tiene el elemento.

```js
const set = new Set();
set.size;    // 0

const set = new Set([5, 6, 7, 8]);
set.size;    // 4

const set = new Set([5, 6, 7, 8, 8]);
set.size;    // 4 (El 8 sólo se inserta una vez)
```

*Add*: El metodo permite añadir un elemento al conjunto.

```js
const set = new Set();

set.add(5);
set.add("A");
set.add(5);     // No se inserta, porque se repite

set;            // Set({5, "A"})
```

*Has*: Este método comprueba si un elemento existe en un conjunto .SI existe devolvera un **true** sino **false**.

```js
const set = new Set([1, 2, 3]);
set.has(2);     // true
set.has(34);    // false
set.add(34);    //añade
set.has(34);    // true
```

*Delete*: Elimina el elmento.Si borra de manera exitosa es **true** y sino devuelve un **false**.

```js
const set = new Set([1, 2, 3]);

set.delete(3);    // true
set.delete(39);   // false

set;              // Set({1, 2})
```

*Clear*: Hace un borrado completo de los elementos.

```js
const set = new Set([1, 2, 3]);
set.clear();

set.size;         // 0
```

### [Convertir a Arrays](https://lenguajejs.com/javascript/set-map/que-es-set-weakset/#convertir-a-arrays)

Una de las cosas más interesantes y útiles de los `Set`, es que al ser una estructura iterable (_se puede recorrer_), es muy sencillo utilizar [desestructuración](https://lenguajejs.com/javascript/arrays/desestructuracion-arrays/) y convertirlo a un array (_o viceversa_):

```js
const set = new Set([5, "A", [99, 10, 24]]);
set.size;                 // 3 (Contiene 3 elementos)

set.constructor.name;     // "Set"
const array = [...set];
array.constructor.name;   // "Array"

array;                    // [5, "A", [99, 10, 24]]
```

Cuidado cuando tengas conjuntos con tipos de datos más complejos con elementos anidados (_arrays, objetos, etc..._). Recuerda que son referencias y modificar un elemento referenciado, modificará el original.

Para evitar esto de forma sencilla, puedes utilizar la función `structuredClone()`:

```js
const set = new Set([5, "A", [99, 10, 24]]);
set.size;  // 3

const clonedArray = [...structuredClone(set)];
const array = [...set];clonedArray[2][0] = "Modified";
[...set][2][0];   // 99 (El original se mantiene intacto)

array[2][0] = "Modified";
[...set][2][0];   // "Modified" (El original ha mutado)
```

Además, también puedes hacer la operación inversa, para convertir un array en un `Set`:

```js
const array = [5, 4, 3, 3, 4];
const set = new Set(array);set;   // Set({ 5, 4, 3 })
```

## Ejemplos:

```js
const tiendaMusica = new Set();  //creo el set

//Añade elementos
tiendaMusica.add('Disco#1');
tiendaMusica.add('Disco#2');
tiendaMusica.add('Disco#3');
tiendaMusica.add('Disco#3');
tiendaMusica.add('Disco#1');

//Total de elementos introducidos
console.log(tiendaMusica.size);  

//Comprueba elementos
tiendaMusica.has('Disco#3');  //true=existe

//Iterable
tiendaMusica.forEach(producto => console.log(producto));
 
```

>*Nota*: los **set** solo almacenan valores , lo cual si queremos ver su indice sera el mismo valor .

Convertir un array a set:

```js
const miArray = [1,2,3,4,1,2,5];

const miSet = new Set(miArray);

console.log(miSet);

```

# WeakSet

El objeto **`WeakSet`** te permite almacenar _objetos_ débiles en una colección.

Los objetos `WeakSet` son colecciones de objetos. Al igual que [`Set`](https://developer.mozilla.org/es/docs/Web/JavaScript/Reference/Global_Objects/Set), cada objecto `WeakSet` puede estar solo una vez; todos los objetos en una colección `WeakSet` son unicos.

Las principales diferencias con el objeto [`Set`](https://developer.mozilla.org/es/docs/Web/JavaScript/Reference/Global_Objects/Set) son:

-   `WeakSet` son colecciones de **objetos solamente**. No pueden contener valores arbitrarios de cualquier tipo, como pueden hacerlo los [`Set`](https://developer.mozilla.org/es/docs/Web/JavaScript/Reference/Global_Objects/Set).

-   El `WeakSet` es _débil_, lo que significa que las referencias a objetos en un `WeakSet` se mantienen _débilmente_. Si no existen otras referencias a un objecto almacenado en `WeakSet`, esos objctos pueden ser recolectados como basura.
- Al igual que `Set`, admite[`add`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Weakset/add), [`has`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Weakset/has) y [`delete`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Weakset/delete), pero no `size`,`keys()` ni iteraciones.

```javascript
let visitedSet = new WeakSet();

let john = { name: "John" };
let pete = { name: "Pete" };
let mary = { name: "Mary" };

visitedSet.add(john); // John nos visita
visitedSet.add(pete); // luego Pete
visitedSet.add(john); // John otra vez

// visitedSet tiene 2 usuarios ahora

// comprobar si John nos visitó?
alert(visitedSet.has(john)); // true

// comprobar si Mary nos visitó?
alert(visitedSet.has(mary)); // false

john = null;

// visitedSet se limpiará automáticamente
```

[`WeakSet`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/WeakSet) es una colección tipo `Set` que almacena solo objetos y los elimina una vez que se vuelven inaccesibles por otros medios.

>**Nota:**
>`WeakMap` y`WeakSet` se utilizan como estructuras de dato “secundarias” además del almacenamiento de objetos “principal”. Una vez que el objeto se elimina del almacenamiento principal, si solo se encuentra como la clave de `WeakMap` o en un`WeakSet`, se limpiará automáticamente.

## Ejemplo:

```javascript
let messages = [
  {text: "Hello", from: "John"},
  {text: "How goes?", from: "John"},
  {text: "See you soon", from: "Alice"}
];

let readMessages = new WeakSet();

// se han leído dos mensajes
readMessages.add(messages[0]);
readMessages.add(messages[1]);
// readMessages tiene 2 elementos

// ...¡leamos nuevamente el primer mensaje!
readMessages.add(messages[0]);
// readMessages todavía tiene dos únicos elementos

// respuesta: ¿se leyó el mensaje [0]?
alert("Read message 0: " + readMessages.has(messages[0])); // true

messages.shift();
// ahora readMessages tiene 1 elemento (técnicamente la memoria puede limpiarse más tarde)
```