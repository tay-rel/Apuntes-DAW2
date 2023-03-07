
JavaScript **no es un lenguaje orientado a objetos basado en clases**. Pero todavía tiene formas de usar la programación orientada a objetos (POO).Es un lenguaje basado en clases, es un lenguaje basado en prototipos.

# Clases
A partir de la clase crearemos objetos, que son instancias de la clase.

```js
class Persona{

	constructor (nombre,apellidos){
		this.nombre = nombre;
		this.apellidos = apellidos;
	}
	canta(){
		return `${this.nombre} puende cantar `;
	}
}

let persona = new Persona('Pablo', 'Soliz');
console.log(persona); //Pablo Soliz
```


El constructor define las características principales, mientras que todo lo que está fuera del constructor (`canta()` ) es una característica adicional (**prototipos**).

# Métodos

En JavaScript, los captadores y definidores son dos métodos tradicionales para recuperar y actualizar valores de variables.

Mediante el uso de **captadores y definidores**(getter,setter), los programadores pueden controlar cómo acceder y actualizar sus variables importantes de una manera adecuada, como cambiar el valor de una variable dentro de un rango específico. Considere el siguiente código para el método de establecimiento:

```js
class Persona {

	constructor(nombre, apellidos) {
		this._nombre = nombre;
		this._apellidos = apellidos;
	}

	canta() {
		return `${this.nombre} puende cantar `;
	}


//Get -Set

	get nombre() {
		return `El nombre ${this._nombre} es bonito `;
	}
	
	set nombre(name) {
		this._nombre = name.trim();
	}

}

let persona = new Persona('Pablo', 'Soliz');
let name = persona.nombre; // Indirectamente va a llamar al método nombre();

persona.nombre = ' Juan Carlos '; // Indirectamente llamará al método nombre
console.log(persona);
```

Las variables que empiezan con el caracter ''  *_* '' suele indicar a los programadores que no debería usarse directamente si no a través de los métodos get y set.

Por otro lado se puede definir métodos que no estan vinculados estrictamente a un único atributo.

```js
class Persona {

	constructor(nombre, apellidos) {
		this._nombre = nombre;
		this._apellidos = apellidos;
	}
//Get -Set

	get nombre() {
		return `El nombre ${this._nombre} es bonito `;
	}
	
	set nombre(name) {
		this._nombre = name.trim();
	}
	showFullName() {
		return this._nombre + ' ' + this._apellidos;
	}

}

let persona = new Persona('Pablo', 'Soliz');
console.log(persona.showFullName());//Pablo Soliz
```


# Propiedades

Las propiedades de una clase pueden ser , privadas, públicas, protegídas o estáticas.

```js
class Persona {
	_nombre;
	#apellido;

	constructor(nombre, apellidos) {
		this._nombre = nombre;
		this.#apellidos = apellidos;
	}
//Get -Set

	get nombre() {
		return `El nombre ${this._nombre} es bonito `;
	}
	
	set nombre(name) {
		this._nombre = name.trim();
	}
	get apellidos() {
		return `El nombre ${this._nombre} es bonito `;
	}
	
	set apellidos(apellido) {
		this.#apellido = apellido.trim();
	}
	toString() {
		return this._nombre + ' ' + this.#apellido;
	}

}

let persona = new Persona('Pablo', 'Soliz');
console.log(persona.toString());//pablo soliz
```

## Propiedades y métodos estáticos.

```javascript

class Animal {
  static planet = "Tierra";
  constructor(name, speed) {
    this.speed = speed;
    this.name = name;
  }

  run(speed = 0) {
    this.speed += speed;
    alert(`${this.name} corre a una velocidad de ${this.speed}.`);
  }

  static compare(animalA, animalB) {
    return animalA.speed - animalB.speed;
  }

}

// Hereda de Animal
class Rabbit extends Animal {
  hide() {
    alert(`${this.name} se esconde!`);
  }
}

let rabbits = [
  new Rabbit("Conejo Blanco", 10),
  new Rabbit("Conejo Negro", 5)
];

rabbits.sort(Rabbit.compare);
rabbits[0].run(); // Conejo Negro corre a una velocidad de 5.
alert(Rabbit.planet); // Tierra
```


# Herencia

Los objetos en JavaScript son "contenedores" dinámicos de propiedades (referidas como sus **propiedades particulares**). Los objetos en JavaScript poseen un enlace a un objeto prototipo. 

Cuando intentamos acceder a una propiedad de un objeto, la propiedad no sólo se busca en el propio objeto sino también en el prototipo del objeto, en el prototipo del prototipo, y así sucesivamente hasta que se encuentre una propiedad que coincida con el nombre o se alcance el final de la cadena de prototipos.

```js
class Estudiante extends Persona {

#matricula = false;

	constructor(nombre, apellidos) {
		super(nombre, apellidos);
	}
	
	get matricula() {
		return this.#matricula;
	}
	
}
  
let estudiante = new Estudiante();
console.log(estudiante.matricula);//false
```