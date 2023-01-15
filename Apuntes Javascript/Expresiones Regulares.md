
Las expresiones regulares **son una serie de carácteres que forman un patrón**, normalmente representativo de otro grupo de carácteres mayor, de tal forma que podemos comparar el patrón con otro conjunto de carácteres para ver las coincidencias.

Las expresiones regulares son patrones que se utilizan para hacer coincidir combinaciones de caracteres en cadenas. En JavaScript, **las expresiones regulares también son objetos**. Estos patrones se utilizan con los métodos [`exec()`](https://developer.mozilla.org/es/docs/Web/JavaScript/Reference/Global_Objects/RegExp/exec) y [`test()`](https://developer.mozilla.org/es/docs/Web/JavaScript/Reference/Global_Objects/RegExp/test) de [`RegExp`](https://developer.mozilla.org/es/docs/Web/JavaScript/Reference/Global_Objects/RegExp), y con [`match()`](https://developer.mozilla.org/es/docs/Web/JavaScript/Reference/Global_Objects/String/match), [`matchAll()`](https://developer.mozilla.org/es/docs/Web/JavaScript/Reference/Global_Objects/String/matchAll), [`replace()`](https://developer.mozilla.org/es/docs/Web/JavaScript/Reference/Global_Objects/String/replace), [`replaceAll()` (en-US)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/replaceAll "Currently only available in English (US)"), [`search()`](https://developer.mozilla.org/es/docs/Web/JavaScript/Reference/Global_Objects/String/search) y [`split()`](https://developer.mozilla.org/es/docs/Web/JavaScript/Reference/Global_Objects/String/split) métodos de [`String`](https://developer.mozilla.org/es/docs/Web/JavaScript/Reference/Global_Objects/String)

|Caracteres/construcciones| Artículo correspondiente|
|-|-|
|`\`, `.`, `\cX`, `\d`, `\D`, `\f`, `\n`, `\r`, `\s`, `\S`, `\t`, `\v`, `\w`, `\W`, `\0`, `\xhh`, `\uhhhh`, `\uhhhhh`, `[\b]`|Clases de caracteres|
|`^`, `$`, `x(?=y)`, `x(?!y)`, `(?<=y)x`, `(?<!y)x`, `\b`, `\B`	|Aserciones|
|`(x)`, `(?:x)`, `(?<Name>x)`, `xy`, `[xyz]`, `[^xyz]`, `\Number`  |Grupos y rangos|
| `*`, `+`, `?`, `x{n}`, `x{n,}`, `x{n,m}`|Cuantificadores|
| `\p{UnicodeProperty}`, `\P{UnicodeProperty}`|Escapes de propiedades Unicode|


## Banderas
En las expresiones regulares pueden usar banderas que afectan la búsqueda.

|Banderas|Descripción|
|-|-|
|i|mayusculas y minusculas no las distingue|
|g|se devuelve la primer coincidencia|
|m|modo multilinea|
|s|permite un punto ,coincide con el caracter linea nueva|
|u|soporte unicode|
|y|posición exacta|

## Uso y creación de expresiones regulares

Las expresiones regulares se utilizan con los métodos `RegExp` `test()` y `exec()` y con los métodos de `String`, `match()`, `replace()`, `search()` y `split()`. Estos métodos se explican en detalle en la [referencia de JavaScript](https://developer.mozilla.org/es/docs/Web/JavaScript/Reference).

```js
//Bandera g --> busca todas las coincidencias
//Bandera i --> No distinque entre mayúsculas y minúsculas

let er = new RegExp('^[3]*[2]*$','g');
let er2 = /^[3]*[2]*$/g;

```

|Método|Descripción|
|-|-|
|[`exec()`](https://developer.mozilla.org/es/docs/Web/JavaScript/Reference/Global_Objects/RegExp/exec)|Ejecuta una búsqueda por una coincidencia en una cadena. Devuelve un arreglo de información o `null` en una discrepancia.|
|[`test()`](https://developer.mozilla.org/es/docs/Web/JavaScript/Reference/Global_Objects/RegExp/test)|Prueba una coincidencia en una cadena. Devuelve `true` o `false`.|
|[`match()`](https://developer.mozilla.org/es/docs/Web/JavaScript/Reference/Global_Objects/String/match)|Devuelve un arreglo que contiene todas las coincidencias, incluidos los grupos de captura, o `null` si no se encuentra ninguna coincidencia.|
|[`matchAll()`](https://developer.mozilla.org/es/docs/Web/JavaScript/Reference/Global_Objects/String/matchAll)|Devuelve un iterador que contiene todas las coincidencias, incluidos los grupos de captura.|
|[`search()`](https://developer.mozilla.org/es/docs/Web/JavaScript/Reference/Global_Objects/String/search)|Prueba una coincidencia en una cadena. Devuelve el índice de la coincidencia, o `-1` si la búsqueda falla.|
|[`replace()`](https://developer.mozilla.org/es/docs/Web/JavaScript/Reference/Global_Objects/String/replace)|Ejecuta una búsqueda por una coincidencia en una cadena y reemplaza la subcadena coincidente con una subcadena de reemplazo.|
|[`replaceAll()` (en-US)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/replaceAll "Currently only available in English (US)")|Ejecuta una búsqueda de todas las coincidencias en una cadena y reemplaza las subcadenas coincidentes con una subcadena de reemplazo.|
|[`split()`](https://developer.mozilla.org/es/docs/Web/JavaScript/Reference/Global_Objects/String/split)|Utiliza una expresión regular o una cadena fija para dividir una cadena en un arreglo de subcadenas.|

## a) Comprueba si cumple

```js
let valor = '3322';
let valor2 = '33222 32';

let er2 = /^[3]*[2]*$/g;

//Comprueba si cumple o no la ER
console.log(er.test(valor));
console.log(er2.test(valor2));

```

## b) Encontrar coincidencias

```js
var myRe = /d(b+)d/g;
var myArray = myRe.exec('cdbbdbsbz');
```

```js
var re = /\w+\s/g;
var str = 'fee fi fo fum';
var myArray = str.match(re);
console.log(myArray);

// ["fee ", "fi ", "fo "]
```

```JS
const paragraph = 'The quick brown fox jumps over the lazy dog. It barked.';
const regex = /[A-Z]/g;
const found = paragraph.match(regex);

console.log(found);
// Expected output: Array ["T", "I"]
```