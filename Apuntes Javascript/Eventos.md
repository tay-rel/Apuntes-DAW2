Los eventos son la manera que tenemos en Javascript de **controlar las acciones de los visitantes** y definir un comportamiento de la página cuando se produzcan. Cuando un usuario visita una página web e interactúa con ella se producen los eventos y **con Javascript podemos definir qué queremos que ocurra cuando se produzcan los eventos**.

-   **Evento**: Es algo que ocurre. Generalmente los eventos ocurren cuando el usuario interactúa con el documento, pero podrían producirse por situaciones ajenas al usuario, como una imagen que no se puede cargar porque esté indisponible.
-   **Tipo de evento**: Es el tipo del suceso que ha ocurrido, por ejemplo, un clic sobre un botón o el envío de un formulario. Cada tipo de elemento de la página ofrece diversos tipos de eventos de Javascript. En esta página puedes saber cuáles son los [tipos de eventos en Javascript](https://desarrolloweb.com/articulos/1236.php).
-   **Manejador de evento**: es el comportamiento que nosotros podemos asignar como respuesta a un evento. Se especifica mediante una función Javascript, que se asocia a un tipo de evento en concreto. Una vez asociado el manejador a un tipo de evento sobre un elemento de la página, da vez que ocurre ese tipo de evento sobre ese elemento en concreto, se ejecutará el manejador de evento asociado.

## ¿Por qué necesitamos los eventos en [Javascript][https://desarrolloweb.com/articulos/1235.php]?

Con javascript podemos definir qué es lo que pasa cuando se produce un evento, como podría ser que un usuario pulse sobre un botón, edite un campo de texto o abandone la página.

El manejo de eventos es el caballo de batalla para hacer páginas interactivas, porque con ellos podemos responder a las acciones de los usuarios. Hasta ahora en este manual hemos podido ver muchos ejemplos de manejo de uno de los eventos de Javascript, el evento "click", que se produce al pulsar un elemento de la página. Hasta ahora siempre hemos aplicado el evento a un botón, pero podríamos aplicarlo a otros elementos de la página.

## ¿Cómo se define un evento en Javascript?

Para definir las **acciones que queremos realizar al producirse un evento utilizamos los manejadores de eventos**. Existen muchos tipos de eventos sobre los que podemos asociar manejadores de eventos, para muchos tipos de acciones del usuario.

En Javascript podemos definir eventos de dos maneras distintas. Una manera es en el propio código HTML, usando atributos de los elementos (etiquetas) a los que queremos asociar los manejadores de eventos. Otra manera un poco más avanzada es usando los propios objetos del [[DOM]]. Vamos a ver ambas maneras a continuación.

## Manejadores de eventos asociados con addEventListener

La segunda forma de asociar manejadores de eventos a elementos de la página es mediante el método `addEventListener()`. Es una forma un poco más avanzada, pero mejora todavía la mantenibilidad del código, ya que permite separar mejor el código de la funcionalidad del código del contenido.

El HTML debería usarse simplemente para definir el contenido. Si tenemos instrucciones Javascript dentro de las etiquetas, colocando atributos como "onclick" o "onchange" lo que estamos haciendo es colocar código de la funcionalidad dentro del código HTML, lo que es poco deseable desde el punto de vista de separación de responsabilidades. Por tanto, la técnica que vamos a conocer ahora es todavía más adecuada, porque nos va a permitir escribir el código de la funcionalidad (los eventos javascript) sin ensuciar el código HTML.

Para asociar un evento a un elemento de la página necesitamos hacer dos pasos:

1.  *Acceder al objeto* sobre el que queremos definir el evento. Para esto tenemos que acceder al DOM para localizar el objeto adecuado, que representa la etiqueta sobre la que queremos asociar el manejador del evento.
2.  *Sobre el objeto del DOM*, aplicamos addEventListener(), indicando el tipo de evento y la función manejadora.

### Ejemplo 1:

```html
<input type="button" id="elboton" value="Haz click">
```

Lo más cómodo **para acceder a un elemento de la página y recuperar el objeto del DOM asociado a esa etiqueta es usar el identificador** (*atributo "id"*). En este caso el identificador es "*elboton*". Para acceder a ese elemento usamos el método getElementById() del objeto document, enviando el identificador.

```javascript
var miBoton = document.getElementById('elboton');
```

Ahora tenemos el objeto del DOM asociado a ese botón en la variable "miBoton". Sobre ese objeto ya podemos invocar el método *addEventListener*(). A este método **debemos pasarle dos parámetros**. El *primero es el tipo de evento que queremos detectar* y el segundo *la función manejadora del evento que queremos que se ejecute *cuando se produce el evento.

```javascript
miBoton.addEventListener('click', function() {
  alert('Has hecho clic!!')
})
```

Ya lo tenemos! al hacer clic sobre el botón se mostrará un mensaje de alerta.

### Ejemplo 2:

Sobre una imagen en la que vamos a asociar **un manejador para el tipo de evento "mouseover"**, que se produce cuando el usuario pone el puntero del ratón encima de un elemento.

Tenemos una imagen, en la que hemos puesto un atributo id para llegar a ella.

```html
<img src="https://picsum.photos/200/300" id="imagen">
```

Ahora le asociamos el manejador de evento para el tipo de evento "mouseover" con este código Javascript.

```javascript
var miImagen = document.getElementById('imagen');
miImagen.addEventListener('mouseover', function() {
  alert('Has pasado el ratón encima de la imagen')
})
```

## Objeto evento

Solo queremos dar un último detalle sobre eventos abordando el objeto evento, que recibimos en todas las funciones que hacen de manejadores de eventos. Mediante el objeto evento podemos recibir multitud de datos útiles sobre las condiciones en las cuales un evento se ha producido.

El objeto evento de Javascript se genera automáticamente por parte del navegador y nos lo envían como parámetro a la función asociada al evento. En muchas ocasiones, como en los ejemplos anteriores, no necesitamos el objeto evento para saber nada en concreto, por lo que simplemente podemos omitir ese parámetro y no recibir ese objeto evento, pero si lo necesitamos para algo, simplemente colocamos el parámetro al definir el manejador.

```javascript
miImagen.addEventListener('mouseover', function(objetoEvento) {
  console.log(objetoEvento)
})
```

En el código anterior simplemente hemos modificado la función manejadora para recibir el objeto evento. Dentro de la función manejadora simplemente estamos enviando ese objeto a la consola, con lo que podemos ver la cantidad de datos que nos ofrece.

![El objeto evento de Javascript mostrado en la consola del navegador](https://desarrolloweb.com/media/273/objeto-evento.jpg)

Ahora vamos a ver un código de **definición de un evento sobre el objeto document**, en el cual vamos a ver dónde *han hecho clic dentro de un documento*, las coordenadas exactas. Para ello usamos las propiedades clientX y clientY del objeto evento, tal como puedes ver en este código.

```javascript
document.addEventListener('click', function(event) { 
  alert('Has hecho clic en ' + event.clientX + 'x' + event.clientY);
})
```

Un detalle interesante es que este evento lo hemos asociado al objeto `document` directamente, por lo que el evento clic se está detectando en todo el documento HTML, así que pulses donde pulses se mostrará la caja de alerta informando de las coordenadas.

Como el objeto document está siempre disponible en el navegador, solamente hemos tenido que acceder a document.addEventListener(), mientras que antes necesitábamos hacer un paso previo para traernos el elemento al que queríamos asociar el manejador con getElementById().