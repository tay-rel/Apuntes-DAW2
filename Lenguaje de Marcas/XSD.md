Un **esquema XSD** es un mecanismo para comprobar la [validez](http://www.ticarte.com/contenido/que-es-xml) de un [documento XML](https://www.ticarte.com/node/107), es decir, definir su estructura: qué elementos, qué tipos de datos, que atributos, en qué orden, cuántas veces se repiten, etc. Se trata de una forma alternativa a los esquema DTD, pero con bastantes _ventajas_ sobre estos:

-   Es un documento XML, por lo que se puede comprobar si está bien formado.
-   Existe una extensa lista de tipos de datos predefinidos para elementos y atributos que pueden ser ampliados o restringidos para crear nuevos tipos.
-   Permiten concretar con precisión la cardinalidad de un elemento, es decir, las veces que puede aparecer en un documento XML.
-   Permite mezclar distintos vocabularios (juegos de etiquetas) gracias a los espacios de nombres.

Pero también tienen alguna _desventaja_:

-   Son documentos más difíciles de interpretar por el ojo humano.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<alumnos xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:noNamespaceSchemaLocation="alumnos.xsd">
   ...
</alumnos>
```


```xsd
<!--alumnos.xsd-->

<?xml version="1.0" encoding="UTF-8"?>
<xs:schema xmlns:xs="http://www.w3.org/2001/XMLSchema">
  <xs:element name="alumnos" />
</xs:schema>
```

Donde la etiqueta raíz del documento define los atributos:

-   _xmlns:xsi_ para declarar el espacio de nombres del esquema XSD.
-   _xsi:noNamespaceSchemaLocatio_n para vincular el documento XML con el esquema local XSD.

# xs:element

Este componente permite declarar los elementos del documento XML. Entre otros, los principales atributos que podemos utilizar en la declaración son los siguientes:

-   _name_: Indica el nombre del elemento. Obligatorio si el elemento padre es _<xs:schema>_.
-   _ref_: Indica que la declaración del elemento se encuentra en otro lugar del esquema. No se puede usar si el elemento padre es _<xs:schema>_. No puede aparecer junto con _name_.
-   _type_: Indica el tipo de dato que almacenará el elemento. No puede aparecer junto con _ref_.
-   _default_: Es el valor que tomará el elemento al ser procesado si en el documento XML no ha recibido ningún valor. Sólo se puede usar con tipo de dato textual.
-   _fixed_: Indica el único valor que puede contener el elemento en el documento XML. Sólo se puede usar con tipo de dato textual.
-   _minOccurs_: Indica el mínimo número de ocurrencias que deben aparecer de ese elemento en el documento XML. No se puede usar si el elemento padre es _<xs:schema>_. Va desde 0 hasta ilimitado (_unbounded_). Por defecto 1.
-   _maxOccurs_: Indica el máximo número de ocurrencias que pueden aparecer de ese elemento en el documento XML. No se puede usar si el elemento padre es _<xs:schema>_. Va desde 0 hasta ilimitado (_unbounded_). Por defecto 1.

```xsd
<xs:element name="nombre" type="xs:string" default="TicArte" minOccurs="1" maxOccurs="unbounded" />

<xs:element ref="contacto" minOccurs="1" maxOccurs="unbounded" />
```

# xs:attribute

Este componente permite declarar los atributos de los elementos del documento XML. Entre otros, los principales atributos que podemos utilizar en la declaración son los siguientes:

-   _name_: Indica el nombre del atributo.
-   _ref_: Indica que la declaración del atributo se encuentra en otro lugar del esquema. No puede aparecer junto con _name_.
-   _type_: Indica el tipo de dato que almacenará el atributo. No puede aparecer junto con _ref_.
-   _use_: Indica si la existencia del atributo es opcional (_optional_), obligatoria (_required_) o prohibida (_prohibited_). Por defecto opcional.
-   _default_: Es el valor que tomará el elemento al ser procesado si en el documento XML no ha recibido ningún valor. Sólo se puede usar con tipo de dato textual.
-   _fixed_: Indica el único valor que puede contener el elemento en el documento XML. Sólo se puede usar con tipo de dato textual.

```xsd

<xs:attribute name="moneda" type="xs:string" default="euro" use="required" />

<xs:attribute ref="moneda" use="required" />

```

## Tipos de datos

### Tipos de datos simples y complejos

Existe dos grandes grupos de tipos de datos que se pueden utilizar en los esquemas XSD:

-   **Tipos de datos simples** (_xs:simpleType_): Se dividen en los siguientes.
    -   [Tipos de datos predefinidos](https://www.ticarte.com/contenido/que-son-los-esquemas-xsd#datospredefinidos).
    -   [Tipos de datos construidos](https://www.ticarte.com/contenido/que-son-los-esquemas-xsd#datosconstruidos) con nuestras propias restricciones y basados en los tipos de datos predefinidos.
    
-   **Tipos de datos complejos** (_xs:complexType_): Se dividen en los siguientes.
    -   Elementos dentro de otros elementos.
    -   Elementos que tienen atributos.
    -   Elementos mixtos que tienen datos y otros elementos.
    -   Elementos vacíos.

## Contenido simple y complejo

Dentro de los _tipos de datos complejos_, podemos definir dos tipos de contenidos que pueden ir entre las etiquetas de apertura y cierre de los elementos:

-   **Contenido simple** (_xs:simpleContent_): Cuando el elemento declarado sólo tienen contenido textual, sin elementos descendientes.
-   **Contenido complejo** (_xs:complexContent_): Cuando el elemento declarado tiene elementos descendientes, pudiendo tener o no contenido textual.

# Indicadores

Permiten establecer las características de los elementos que van a utilizarse dentro de otro elemento, por tanto dentro del Contenido complejo.

**Indicadores de orden**: Asignan el orden de aparición de los elementos descendientes se pueden utilizar las siguientes opciones, tanto individualmente como combinados.

-   **Secuencia** (_xs:sequence_): Define el orden exacto de aparición de los elementos.
-   **Alternativa** (_xs:choice_): Define una serie de elementos entre los cuales sólo se puede elegir uno de ellos.
-   **Todos** (_xs:all_): Define una serie de elementos que pueden aparecer en cualquier orden.

## Tipos de datos construidos

Los tipos de datos construidos son generados por el usuario a partir de un tipo de dato predefinido y aplicándoles [restricciones](https://www.ticarte.com/contenido/que-son-los-esquemas-xsd#restricciones) si se desea (estas restricciones se explicarán más adelante). Se pueden diseñar de dos maneras.

-   Pueden usarse **directamente en la definición de un elemento**, en lugar de usar el atributo _type_:

```xsd
<xs:element name="edad" minOccurs="1" maxOccurs="1">
  <xs:simpleType>
    <xs:restriction base="xs:integer">
      <xs:minInclusive value="0"/>
      <xs:maxInclusive value="100"/>
    </xs:restriction>
  </xs:simpleType>
</xs:element>
```

-   También pueden **definirse asignándoles un nombre** y pudiéndose usar en cualquier elemento del documento mediante el atributo _type._ Esta sección es indiferente colocarla antes o después de la definición del elemento raíz:

```xsd
<xs:simpleType name="longitudMaxima">  
  <xs:restriction base="xs:string">  
    <xs:minLength value="0"/>  
    <xs:maxLength value="10"/>  
  </xs:restriction>  
</xs:simpleType>

<xs:element name="nombre" type="longitudMaxima"/>
```

-   Y se pueden construir **tipos de datos extendiendo** otros tipos de datos ya existentes.