Las API REST *proporcionan una forma flexible y ligera de integrar aplicaciones*. Han surgido como el método más común para conectar componentes en arquitecturas de microservicios.
 
  Para localizar un recurso en la red se utiliza una `URL` (Uniform Resource Locator). URL es un tipo de URI (Uniform Resource Identifier) que se utiliza en muchos ámbitos, no sólo para Internet.

# ¿Qué es una API REST?

Una API, o _interfaz de programación de aplicaciones_, es un conjunto de reglas que definen cómo pueden las aplicaciones o los dispositivos conectarse y comunicarse entre sí. Una API REST es una API que cumple los principios de diseño del estilo de arquitectura REST o _transferencia de estado representacional_. Por este motivo, las API REST a veces se conocen como API RESTful_._

El concepto REST, definido por primera vez en 2000 por el ingeniero informático Dr. Roy Fielding en su tesis doctoral, proporciona un nivel relativamente alto de flexibilidad y libertad para los desarrolladores. Esta flexibilidad es solo una de las razones por las que han surgido las API REST como un método común para conectar componentes y aplicaciones en una arquitectura de [microservicios](https://www.ibm.com/es-es/cloud/learn/microservices).

## URI: ¿qué es el identificador de recursos uniforme?

Casi todo el mundo ha oído hablar del URL, la dirección que utilizamos para acceder a las páginas web por Internet. Sin embargo, pocas personas saben qué es el URI. La idea de un identificador uniforme de recursos fue concebida por **Tim Berners-Lee**, el padre de la **World Wide Web**. Cuando el científico utilizó el término por primera vez en la RFC 1630, aún hablaba de un _universal resource identifier_, o identificador universal de recursos. Desde entonces, sobre todo debido a su uso en algunas publicaciones del World Wide Web Consortium (W3C), las siglas URI empezaron a referirse a _uniform resource identifier_, y así es como las entendemos hoy en día. Su estructura, no obstante, no ha cambiado en absoluto desde su creación.

### La sintaxis del URI

Un URI consta de un máximo de cinco partes, de las cuales solo dos son obligatorias:

-   **_scheme_** _(esquema)_: proporciona información sobre el protocolo utilizado.
-   **_authority_** _(autoridad)_: identifica el dominio.
-   **_path_** _(ruta)_: muestra la ruta exacta al recurso.
-   **_query_** _(consulta)_: representa la acción de consulta.
-   **_fragment_** _(fragmento)_: designa una parte del recurso principal.

Los dos elementos imprescindibles que deben contener todos los identificadores son _scheme_ y _path_. En la estructura del URI, los componentes se enumeran uno tras otro por este orden y están separados por caracteres estándar.

```mixed
scheme :// authority path ? query # fragment
```

# Diferentes tipos de peticiones http:

Existen diferentes tipos de peticiones http:

-   *GET*. Se le pide al servidor un recurso. Se solicita por URL. No se usa para cambiar nada en el servidor, sólo para obtener datos/información. 

-   *POST*. Se utiliza para crear recursos en el servidor. Los datos se envían en el cuerpo de la petición, estos datos se envían como JSON.

-   *PUT*. Se utiliza para crear o reemplazar. Si no existe lo crea, y si está creado lo reemplaza. Al igual que POST los datos se envían en el cuerpo de la petición. 

-   *DELETE*. Para borrar un recurso. El cuerpo del request no lleva datos. El recurso a borrar se indica en la URL. 

-   *PATCH*. Para hacer modificaciones parciales de recursos. Es bastante reciente y aún está siendo adoptado por la comunidad.

 ```js
//GET- Pedir recursos = parametrp en URL
//cuando se pide todos los post te da un array de objetos
//si le pide un post devuelve un json
fetch('https://jsonplaceholder.typicode.com/todos/1')
.then(response => response.json())
.then(json => console.log(json));


//POST -Crear recursos -Parametrosen en el cuerpo de la solicitud
fetch('https://jsonplaceholder.typicode.com/posts', { // se le pasa una llamda y un objeto de js

method: 'POST',
body: JSON.stringify({
title: 'foo',
body: 'bar',
userId: 1,
}),
headers: {
'Content-type': 'application/json; charset=UTF-8', //tipo mimo de lo que envia en el cuerpo
},
})
.then(response => response.json())
.then(json => console.log(json));

//Put. Crear/MOdificar
fetch('https://jsonplaceholder.typicode.com/posts', { // se le pasa una llamda y un objeto de js
method: 'POST',
body: JSON.stringify({

id: 1,
title: 'foo',
body: 'bar',
userId: 1,
}),

headers: {
'Content-type': 'application/json; charset=UTF-8', //tipo mimo de lo que envia en el cuerpo
},

})
.then(response => response.json())
.then(json => console.log(json));

 ```