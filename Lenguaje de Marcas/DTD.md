DTD (*Document type definition*)Sirve para definir la estructura de un documento SGML oXml, permitiendo su validación.

- **SMGL**: standar generalized markup language.
- **XML**:extensible markup language.

# Reglas 
- El documento debe estar bien formado , cumpliendo las estructura establecida. 
- Una DTD se puede escribir tanto interna como esterna al xml.
- En ambos casos se deb escribir una definición *DOCTYPE* (Document Type Declaration). Para asociar el documento XML a la DTD.

# Sintaxis

```dtd
<!--Sintaxis:  
<!DOCTYPE elemento-raíz [ declaraciones ]>-->

<?xml version="1.0" encoding="UTF-8"?>  
<!DOCTYPE marcadores [  
<!ELEMENT marcadores (pagina)*>  
<!ELEMENT pagina (nombre, descripcion, url)>  
<!ELEMENT nombre (#PCDATA)>  
<!ELEMENT descripcion (#PCDATA)>  
<!ELEMENT url (#PCDATA)>  
]>
```

En DTD es muy importante la sintaxis a la hora de esribir por ejemplo:

- Elemento (padre) que  contenga tres elementos (hijos), se puede escribir:  
```dtd
<!ELEMENT padre (hijo1, hijo2, hijo3)>
```

> *Nota:*
> ES muy importante escribir en orden los hijos.
 
- Se **requiere espacio en blanco después del tipo de elemento**.

```dtd
<!ELEMENT nombre(#PCDATA)> <!--ESto deberia dar error-->
<!ELEMENT nombre (#PCDATA)> <!--ESto deberia dar error-->
```

- O ciertos atributos que no deben estar dentro de parentesis.

```dtd
<!ELEMENT talla (EMPTY)><!--ESto deberia dar error-->
<!ELEMENT talla EMPTY> <!--ESto deberia dar error-->

<!--Un ejemplo de ello podría ser el elemento “br” del HTML, el  
cual sirve para hacer un salto de línea y no tiene contenido: -->

<!ELEMENT br EMPTY>
```

- Se debe valorar tambien los elementos que tengan contenido mixto.

```dtd
<!ELEMENT persona ANY>
<!ELEMENT persona(nombre, apellido, edad, talla)><!--ESto deberia dar error, no se puede declarar ambas-->

<!ELEMENT persona ANY> <!--ESto deberia dar error-->

<!--Incluso, si el elemento “persona” estuviese vacío, el  
documento también sería válido:  -->

<?xml version="1.0" encoding="UTF-8"?>  
<!DOCTYPE persona [  
<!ELEMENT persona ANY>  
<!ELEMENT nombre (#PCDATA)>  
<!ELEMENT ciudad (#PCDATA)>  
]>  
<persona/>

```

# Cardinalidad de los elementos

En una DTD, para definir **el número de veces que pueden aparecer los elementos** de un documento XML, se pueden utilizar los operadores de  cardinalidad mostrados en la siguiente tabla:

|Operador|Cardinalidad|Significado|
|-|-|-|
| ? | 0-1 |El elemtno es opcional , pudiendo aparacer una sola vez o ninguna|
| * | 0-n |El elemento puede aparader de cero a n veces |
| + | 1-n |El elemento tiene que aparacer , obligatoriamente, una o más veces|


## DTD interna y externa 

- *DTD interna* nosotros **podemos crear en el mismo archivo la estructura de xml**y validarla con el dtd , evitando asi acceder a una ruta , pero esto tiene una desventaja ya que deberiamos crear la validacion en en cada documento.
### Ejemplo:

```dtd
<?xml version="1.0" encoding="UTF-8"?>

<!DOCTYPE marcadores[
<!ELEMENT marcadores (pagina)*>
<!ELEMENT pagina (nombre, descripcion, url)>
<!ELEMENT nombre (#PCDATA)>
<!ELEMENT descripcion (#PCDATA)>
<!ELEMENT url (#PCDATA)>
]>

<marcadores>
<pagina>
	<nombre>Abrirllave</nombre>
	<descripcion>Tutoriales de informática.</descripcion>
	<url>http://www.abrirllave.com/</url>
</pagina>
<pagina>

	<nombre>Wikipedia</nombre>
	<descripcion>La enciclopedia libre.</descripcion>
	<url>http://www.wikipedia.org/</url>

</pagina>
<pagina>

	<nombre>W3C</nombre>
	<descripcion>World Wide Web Consortium.</descripcion>
	<url>http://www.w3.org/</url>

</pagina>
</marcadores>
```

*DTD privada* se puede acceder al archivo de validacion DTD por el nombre.

```xml
<!DOCTYPE elemento-raíz SYSTEM "URI">
```

```dtd
<!ELEMENT marcadores (pagina)*>  
<!ELEMENT pagina (nombre, descripcion, url)>  
<!ELEMENT nombre (#PCDATA)>  
<!ELEMENT descripcion (#PCDATA)>  
<!ELEMENT url (#PCDATA)>
```

*DTD externa*: Esta asociado a una dtd publica

```xml
<!DOCTYPE elemento-raíz PUBLIC "identificador-público" "URI">
```

```dtd
<?xml version="1.0" standalone="no"?>  
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Strict//EN"  
"http://www.w3.org/TR/xhtml1/DTD/xhtml1-strict.dtd">
```

- Para validar más de un documento XML con la misma  DTD, escribir esta en un archivo externo proporciona la ventaja de no tener que repetir la DTD internamente en  cada documento XML.  

-  En el caso de que la DTD solo se utilice para validar un  único documento XML, la DTD es habitual escribirla  internamente.

# Propiedades
En el tipo-de-contenido se especifica el contenido permitido en el elemento, pudiendo ser:

-  Texto, (*#PCDATA*).  
-  Otros elementos (*hijos*).  
- Estar vacío, *EMPTY*.  
-  Mixto (texto y otros elementos), *ANY*.
- declarar un atributo en una DTD <! ATTLIST

```DTD
<!ATTLIST nombre-del-elemento nombre-del-atributo tipo-de-atributo valor-del-atributo>
```

>*Nota: *
>Esta propiedad suele ir acompañado de **CDATA**


