
### Componente Alpine

El atributo `x-data` de Alpine para agregar una variable reactiva `open` que controla si el menú está abierto o cerrado. El botón tiene un evento `@click` que alterna el valor de la variable `open`, y el menú se muestra u oculta según el valor de `open` con el atributo `x-show`. El menú también tiene un evento `@click.away` que cierra el menú cuando se hace clic fuera de él.

```html
<div class="relative" x-data="{ open: false }">  
	    <button @click="open = !open" 
			    class="flex items-center justify-between bg-indigo-500
			     px-4 py-2 text-sm font-medium text-white  rounded-md 
			     focus:outline-none   focus:ring-indigo-500">  
	        <span>Visibilidad de columnas</span>  
	    </button>  
	    <div x-show="open" @click.away="open = false" 
		    class="absolute z-10 w-full mt-2 bg-white 
		    rounded-md shadow-lg">  
	        @foreach($columns as $column)  
	            <label class="block px-4 py-2 text-sm font-medium text-gray-700">  
	                <input type="checkbox" 
	                wire:model="selectedColumn" value="{{ $column }}" 
	                class="mr-2 leading-tight">  
	                <span class="text-gray-900">{{ $column }}</span>  
	            </label>  
	        @endforeach  
	    </div>  
</div>
```


### Componente Jetstream


En este caso para trabajar con el componente de Jetstream **x-data** se utiliza para definir una variable llamada ` open ` que se utiliza ena directiva **x-show** para determinar si el deplegable se muestra o no.
Se usa **x-show** en un elmento que tiene **x-data** se sobreentiende que la condición para mostrar u ocultar el elemento se basa n la variable especificada en **x-data**.

```html
<div class="relative" x-data="{ open: false }"> 
	<x-jet-dropdown> 
		<x-slot name="trigger"> 
			<button type="button" class="flex items-center justify-between 
				w-full px-4 py2 text-sm font-medium text-gray-700 
				bg-white rounded-md hover:bg-gray-50 
				focus:outline-none focus:ring-2 
				focus:ring-offset-2 focus:ring-indigo-500">
				<span>Visibilidad de columnas</span> 
			</button> 
		</x-slot> 
		<x-slot name="content"> 
			<div class="px-4 py-2 space-y-1 "> 
			@foreach ($columns as $column) 
			<label class="flex items-center space-x-2"> 
				<input type="checkbox" 
				class="form-checkbox" 
				wire:model="selectedColumn" 
				value="{{ $column }}"> 
				<span class="text-sm">{{ $column }}</span> 
			</label> 
			@endforeach 
			</div> 
		</x-slot> 
	</x-jet-dropdown> 
</div>

```

### Conclusión

a directiva `x-data` es una de las directivas principales de Alpine.js, una librería de JavaScript que permite crear interactividad en el frontend sin necesidad de escribir tanto código. La directiva `x-data` se utiliza para crear una instancia de un componente de Alpine.js, lo que significa que se le puede asignar a un elemento HTML un objeto de JavaScript que se utilizará para controlar el comportamiento interactivo del elemento.

Ambas formas utilizan la directiva `wire:model` para enlazar los cambios en los elementos `checkbox` del menú a una variable reactiva en el componente Livewire.

![[Pasted image 20230307105133.png]]

![[Pasted image 20230307105225.png]]
