Los objetos **`NodeList`** son colecciones de nodos como los devueltos por propiedades como [`Node.childNodes`](https://developer.mozilla.org/es/docs/Web/API/Node/childNodes) y el método `document.querySelectorAll ()`.

**Aunque `NodeList` no es un `Array`, es posible iterar sobre él utilizando `forEach()`. También puede convertirse a un `Array` usando `Array.from`**.

Sin embargo, algunos navegadores antiguos no han implementado `NodeList.forEach()` ni `Array.from()`. Pero esas limitaciones pueden eludirse utilizando [`Array.prototype.forEach()`](https://developer.mozilla.org/es/docs/Web/JavaScript/Reference/Global_Objects/Array/forEach) (más en este documento).

En algunos casos, `NodeList` es una colección _en vivo_, lo que significa que los cambios en el DOM se reflejan en la colección. Por ejemplo, [`Node.childNodes`](https://developer.mozilla.org/es/docs/Web/API/Node/childNodes) .

En otros casos, `NodeList` es una colección estática, lo que significa que cualquier cambio posterior en el DOM no afecta el contenido de la colección. `document.querySelectorAll ()` devuelve un `NodeList` estático.

## [Propiedades](https://developer.mozilla.org/es/docs/Web/API/NodeList#propiedades)

[`NodeList.length` (en-US)](https://developer.mozilla.org/en-US/docs/Web/API/NodeList/length "Currently only available in English (US)")

El número de nodos en la `NodeList`.

## Métodos

|Métodos|Descripción|
|-|-|
|[`NodeList.item()` (en-US)](https://developer.mozilla.org/en-US/docs/Web/API/NodeList/item "Currently only available in English (US)")|Devuelve un elemento en la lista por su índice, o `null` si el índice está fuera de límites; se puede utilizar como una alternativa para acceder simplemente a `nodeList[idx]` (que en cambio devuelve indefinido cuando idx está fuera de límites).|
|[`NodeList.entries()` (en-US)](https://developer.mozilla.org/en-US/docs/Web/API/NodeList/entries "Currently only available in English (US)")|Devuelve un [`iterator`](https://developer.mozilla.org/es/docs/Web/JavaScript/Reference/Iteration_protocols) que permite pasar por todos los pares clave / valor contenidos en este objeto.|
|[`NodeList.forEach()`](https://developer.mozilla.org/es/docs/Web/API/NodeList/forEach)|Ejecuta una función proporcionada una vez por elemento `NodeList`.|
|[`NodeList.keys()` (en-US)](https://developer.mozilla.org/en-US/docs/Web/API/NodeList/keys "Currently only available in English (US)")|Devuelve un [`iterator`](https://developer.mozilla.org/es/docs/Web/JavaScript/Reference/Iteration_protocols) que permite pasar por todas las claves de los pares clave / valor contenidos en este objeto.|
|[`NodeList.values()` (en-US)](https://developer.mozilla.org/en-US/docs/Web/API/NodeList/values "Currently only available in English (US)")|Devuelve un [`iterator`](https://developer.mozilla.org/es/docs/Web/JavaScript/Reference/Iteration_protocols) que permite recorrer todos los valores de los pares clave / valor contenidos en este objeto.|

### Ejemplo:
