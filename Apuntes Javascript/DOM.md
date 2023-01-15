El documento de objeto modelo es una API que representa e interactúa con un documento HTML o XML.El documento **DOM es un modelo cargado en el navegador y representa un árbol de nodos** , donde cada nodo está representa un parte del documento.

# Window
La interfaz Window representa una ventana que contiene un documento DOM; la propiedad document apunta al documento DOM cargado en esa ventana.

```js
console.log(window.document.title);//Muestra el título del documento

// Simulate a mouse click:
window.location.href = "http://www.w3schools.com";

// Simulate an HTTP redirect:
window.location.replace("http://www.w3schools.com");
```

# Screen
La interfaz de la pantalla representa una pantalla,usualmente aquella ventana actual en la que se está renderizando.

```js
screen.width= Retorna el ancho pantalla

screen.height=Retorna la altura de pantalla

screen.availWidth=Retorna espacio del ancho de la pantalla

screen.availHeight=Retorna el espacio de la altura de la pantalla

screen.colorDepth=Devuelve la profundidad de la pantalla

screen.pixelDepth=Devuelve la profundidad de bits de la pantalla
```

## a) Obtener elementos del DOM

Para poder obtener un elemento del árbol DOM podemos usar tres métodos diferentes: 
`getElementById() / querySelector() / querySelectorAll()`.

|Métodos|Descripción|
|---|---|
|`getElementById('id')`|Devuelve una referencia por su elemento identificador|
|`querySelector('id'/'#class')`|Devuelve el primer elemento del documento que coincida en el grupo especificado de selectores que se le pase como identificador o clase.|
|`querySelectorAll(' ')`|Siempre retorna un `[[NodeList]]`/lista de elementos o puede coger las propiedades para hacer referencia.|

>*Nota:*
>Cuando se crea un elemento ,este se puede llamar a diferentes propiedades de elementos del arbol DOM.

```html
<!DOCTYPE html>

<html>
<body>

<div id =”texto”>The Document Object</div>
<p>Añade el color al fondo a todos los elementos con clase=".example":</p>
<h2 class="example">A heading</h2>
<p class="example">A paragraph.</p>

<script>

let elemento = document.getElementById("texto");
elemento.innerHTML ='<p>Reemplazará el div por un parrafo </p>';

</script>

</body>
```


### Ejemplo:

```js
//Selección de elementos a través del ID
let elemento = window.document.getElementById('daw');

elemento.innerHTML = 'texto';
elemento.style.border = '1px solid black';
elemento.style.backgroundColor = 'gray';
elemento.className = 'miClase';

//Nueva forma de seleccionar un elemento (el primero que encuentra)
let elemento2 = window.document.querySelector('p');
let elemento3 = window.document.querySelector('#contendor');
let elemento4 = window.document.querySelector('.contendor');

//¿Y si hay varios elementos?
let elementos = window.document.querySelectorAll('p');
elementos.forEach((elemento,indice) => {
elemento.innerHTML = `Hola Caracola ${indice}`;
});
```

## b) Crear y clonar elementos 

- *Crear elementos*: Es el método que **crea un elemento de HTML específico por su etiqueta**.Cuando se invoca este documento createElement() convierte la etiqueta en minúsculas antes de crear el elemento.

### Ejemplo:

```Html
<!DOCTYPE html>
<html>
<head>
  <title>||Trabajando con elementos||</title>
</head>
<body>

  <div id="div1">El texto superior se ha creado dinámicamente.</div>

</body>
</html>
```

```js
/*********************Js**************************/

function añadirElemento(){

let nuevoDiv = document.createElement(“div”); //crea un nuevo div
let nuevoContenido = document.createTextNode(“Este es el nuevo elemento”);

nuevoDiv.appendChild();  //agrega un nuevo nodo al final de la lista del element

//añade el elemento creado y su contenido al DOM
var actualDiv = document.getElementById("div1");
document.bodyinsertBefore(nuevoDiv, actualDiv); //reemplaza
}
```

- *Clonar elementos*: devuelve un duplicado del nodo en el que este método fue llamado.
### Ejemplo:

- *node*: El nodo que se desea clonar.
- *dupNode*: El nuevo nodo que será un clon de node
- *deep Opcional*: true si los hijos del nodo también deben ser clonados, o false para clonar únicamente al nodo.

```html
<!DOCTYPE html>
<html>
<head>
  <title>||Trabajando con elementos||</title>
</head>
<body>

<h1>Clonar elementos <h1>
<button onlclick = “copiarElement”> Copiar<button>
<ul id="myList1"><li>Coffee</li><li>Tea</li></ul>
<ul id="myList2"><li>Water</li><li>Milk</li></ul>

</body>
</html>
```

```js
/*********************Js**************************/
function copiarElement(){

let node = document.getElementById(“myList1”).lastChild;//devuelve el último nodo
let clonar = node.cloneNode(true);

document.getElementById(“myList2”).appenChild(clonar);

}
```

## c) Añadir elementos HTML

|Atributos|es|Descripción|
|-|-|-|
|element.append()|HIJO|Añade al final (como último hijo)|
|element.appendChild()|HIJO|Igual que append pero devuelve el nodo añadido|
|element.prepend()|HIJO|Añade al principio (el primer hijo)|
|element.before()|HERMANO|Añade el nodo antes|
|element.after()|HERMANO|Añade el nodo después|
|element.insertBefore(Nodo nuevo, NodoHijo)|HIJO|Inserta el nuevo nodo antes del hijo NodoHijo cuyo padre es parentNode|

### Ejemplo:

```js
let div = document.createElement("div")
div.append("Some text")

console.log(div.textContent) // "Some text"
```

### d) Obtener/Modificar contenido HTML

|Métodos|Descripción|
|---|---|
|`innerHTML`|Esta función reemplaza la sintaxis de un elemento HTML por la nueva. Por otro lado para insertar un elemento se puede usar el método insertAdjancentHTML().|
|`outerHTML`|Cambia el contenido|
|`innerText`|Devuelve una lista de elementos o puede coger las propiedades para hacer referencia.|
|`insertAdjacentHTML`|Este método no re-analiza el elemento sobre el cual se está invocando y por lo tanto no corrompe los elementos ya existentes dentro de dicho elemento. Esto evita el paso adicional de la serialización, haciendolo mucho más rápido que la manipulación directa con ` innerHTML`|


```html
<!doctype html>
<html>
  <head>
    <meta charset="utf-8">
    <title>Documento sin título</title>
  </head>
  <body>

    <div id="txt">
      <p>primer parrafo hijo de div id="txt"</p>
      <p>segundo párrafo hijo de id="txt" txt</p>
    </div>
    
  </body>
</html>
```

```js
txt = document.getElementById("txt");
console.log(txt.innerHTML);

/*
La siguiente cadena (string) se muestra en la ventana de la consola:
<p>primer parrafo hijo de div id="txt"</p>
<p>segundo párrafo hijo de id="txt" txt</p>
*/

```

Se aplica con la propiedad de `insertAdjacentHTML`.

```html
// <div id="one">one</div>
var d1 = document.getElementById('one');
d1.insertAdjacentHTML('afterend', '<div id="two">two</div>');

// En este punto, la nueva estructura es:
// <div id="one">one</div><div id="two">two</div>
```

### e) Eliminar elementos

|Propiedad|es|Descripción|
|-|-|-|
|nodoPadre.removeChild()|HIjo|Elimina el nodo hijo.|
|element.remove()|El mismo|Se elimina él mismo.|


```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="utf-8" />
    <title>JavaScript Remove an Element from the DOM</title>
  </head>
  <body>

    <div id="main">
      <h1 id="title">Hello World!</h1>
      <p id="hint">This is a simple paragraph.</p>
    </div>
    <button type="button" onclick="removeElement()">Remove Element</button>
    <script>

      function removeElement() {
        var parentElem = document.getElementById("main");
        var childElem = document.getElementById("hint");

        parentElem.removeChild(childElem);//También se podría haber hecho de la siguiente forman childElem.remove();
      }
    </script>

  </body>
</html>
```


### f) Reemplazar elementos

|Propiedad|es|Descripción|
|-|-|-|
|nodoPadre.replaceChild(*nodoNuevo,nodoViejo*)|HIjo|Reemplaza el nodo hijo por el nuevo.|
|element.replaceWith(*nodoNuevo,nodoViejo*)|El mismo|Reemplaza él mismo nodo.|

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="utf-8" />
    <title>JavaScript Replace an Element with another Element in the DOM</title>
  </head>
  <body>

    <div id="main">
      <h1 id="title">Hello World!</h1>
      <p id="hint">This is a simple paragraph.</p>
    </div>

    <button type="button" onclick="replaceParagraph()">
      Replace Paragraph
    </button>

    <script>
      function replaceParagraph() {
          var parentElem = document.getElementById("main");
          var oldPara = document.getElementById("hint");

          // Creating new elememt
          var newPara = document.createElement("p");
          var newContent = document.createTextNode("This is a new paragraph.");

          newPara.appendChild(newContent);

          // Replacing old paragraph with newly created paragraph
          parentElem.replaceChild(newPara, oldPara);
      }

    </script>

  </body>
</html>
```

### g) Obtener/Modificar/Eliminar Estilos

|Clases|Descripción|
|-|-|
|element.classList|Devuelve la lista 1de clases que tiene el elemento|
|element.classList.add(“clase1”,”clase2”, etc.)|Añade las clases al elemento|
|element.classList.remove(“clase1”,”clase2”, etc.)|Elimina las clases al elemento|
|element.classList.toggle(“clase1”)|Si no tiene la clase “clase1” la añade y si la tiene la borra. |
|element.classList.contains(“clase1”)|Devuelve un boolean indicando si el elemento tiene la clase o no|


 ```html
<!DOCTYPE html>
<html>
<style>
.myStyle {
  background-color: coral;
  padding: 16px;
}
</style>
<body>

<h1>The DOMToken Object</h1>
<h2>The add() Method</h2>
<button onclick="myFunction()">Add</button>
<p>Click "Add" to add the "myStyle" class to myDIV.</p>
<div id="myDIV">
<p>I am a myDIV.</p>
<p>I am a myDIV.</p>
</div>

<script>
function myFunction() {
  const list = document.getElementById("myDIV").classList;
  list.add("myStyle");
}
</script>

</body>
</html>
 ```

Selección de elementos a través de los atirbutos:

```js
//Selección de elementos a través del ID

let elemento = window.document.getElementById('daw');

elemento.innerHTML = 'texto';
elemento.style.border = '1px solid black';
elemento.style.backgroundColor = 'gray';
elemento.className = 'miClase';
```

