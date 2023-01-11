## ¿Qué es TDD?

Test-driven developments es un software de desarrollo es un metodo en el cual se ecribe **un trozo de codigo antes de escribir el propio código**. La idea de este metodo es que se deb escribir el test primero , por lo que el codigo que se desarrollara sera testeavle y cumplira los requerimientos especificados.
El proceso es el siguiente:
1. __*Escribe un test*__ : El primer paso en TDD es escribir un prueba para un pequeño fragmento de código.La prueba debe especificar la entrada y la salida del código escrito. 
2.  __*Ejecutar la prueba*__ : En este punto , el codigo quiere escribir no existe aún, asi que al ejecutar la prueba deberia fallar.
3. __*Escribir el codigo*__ : Con la prueba en su lugar , ahora se puede escribir el código que se quiere probar.El objetivo es escribir el cantidad mínima de código para que las pruebas funcionen.
4. __*Ejecutar el test de nuevo*__ : Antes de escribir el código , se ejecutara el test de nuevo para asegurar que el código es correcto.
5. __*Refactorizar*__ : Si la prueba pasa , se podrá refactorizar el código para que se más eficiente y más facil de leer , mientras garantiza que las pruebas pasan.
6.  __*Escribir la implementación*__: Escribir el código más sencillo que haga que la prueba funcione. Se usa la expresión "Déjelo simple" ("Keep It Simple, Stupid!"), conocida como [principio KISS](https://es.wikipedia.org/wiki/Principio_KISS "Principio KISS").
7. __*Ejecutar las pruebas automatizadas*__ : Verificar si todo el conjunto de pruebas funciona correctamente.
8.  __*Eliminación de duplicación*__: El paso final es la [refactorización](https://es.wikipedia.org/wiki/Refactorizaci%C3%B3n "Refactorización"), que se utilizará principalmente para eliminar [código duplicado](https://es.wikipedia.org/wiki/C%C3%B3digo_duplicado "Código duplicado"). Se hace un pequeño cambio cada vez y luego se corren las pruebas hasta que funcionen.
9.  __*Actualización de la lista de requisitos*__ : Se actualiza la lista de requisitos tachando el requisito implementado. Asimismo se agregan requisitos que se hayan visto como necesarios durante este ciclo y se agregan requisitos de diseño (P. ej que una funcionalidad esté desacoplada de otra).

## **Manual vs Automated testing**

- **Las pruebas manuales** son llevadas a cabo por personas, quienes navegan e interactúan con el software (usando herramientas adecuadas para cada caso).

Estas pruebas resultan costosas, ya que se requiere contar con un profesional encargado de esta labor; para configurar un entorno y así mismo ejecutar las pruebas.

Como es de esperarse, estas pruebas están expuestas a errores humanos: por ejemplo, se pueden cometer errores tipográficos u omitir pasos durante la prueba.

- **Las pruebas automatizadas**, por el contrario, son realizadas por máquinas, que ejecutan un "test script" que ya ha sido escrito previamente.

Estos tests (o pruebas) pueden variar mucho en cuanto a complejidad:
***desde verificar que el método de una clase específica funcione correctamente***,
***hasta asegurar que una secuencia de acciones complejas en la UI se lleven acabo correctamente y devuelvan los resultados esperados***.

Estas pruebas son más rápidas y confiables que las que se llevan a cabo manualmente – pero la calidad de estas pruebas automatizadas depende de qué tan bien escritos se encuentren los "tests scripts" (código que determina qué es lo que se hará en la prueba).

_Automated testing_ es un componente clave para _continuous integration_ y _continuous delivery_, y es una excelente manera de escalar tus procesos de QA (quality assurance, aseguramiento de calidad) a medida que agregas nuevas características a tu aplicación.

Aún así, son importantes las pruebas manuales para lo que se conoce como "exploratory testing" (lo veremos más adelante en el artículo).

### Tipos de pruebas 

- **Unit tests**

Las **pruebas unitarias** son a bajo nivel (cercanas al código fuente de nuestra aplicación).

Este tipo de testing consiste en probar de forma individual las funciones y/o métodos (de las clases, componentes y/o módulos que son usados por nuestro software).

Debido a lo específicas que son, generalmente son las pruebas automatizadas de menor coste, y pueden ejecutarse rápidamente por un servidor de _continuous integration_ (integración continua).

- **Integration tests**

Las **pruebas de integración** verifican que los diferentes módulos y/o servicios usados por nuestra aplicación funcionen en armonía cuando trabajan en conjunto.

Las pruebas de integración son típicamente el paso siguiente a las pruebas unitarias.

Y son generalmente más costosas de ejecutar, ya que requieren que más partes de nuestra aplicación se configuren y se encuentren en funcionamiento.

-  **Functional tests**

Las **pruebas funcionales** se centran en los _requerimientos de negocio_ de una aplicación.

Estas pruebas verifican la salida (resultado) de una acción, sin prestar atención a los estados intermedios del sistema mientras se lleva a cabo la ejecución.

- **End-to-end tests**

Las **pruebas de punta a punta** replican el comportamiento de los usuarios con el software, en un entorno de aplicación completo.

Estas pruebas verifican que los flujos que sigue un usuario trabajen como se espera, y pueden ser tan simples como cargar una pagina web o inicar sesión.

- **Regression testing**

Las **pruebas de regresión** verifican un conjunto de escenarios que funcionaron correctamente en el pasado, para asegurar que continúen así.

- **Smoke testing**

Las **pruebas de humo** son pruebas que verifican la funcionalidad básica de una aplicación.


Laravel incluye en el directorio principal de tu proyecto un directorio llamado /tests

# Bibliografía
Tipos de [Test][https://programacionymas.com/blog/tipos-de-testing-en-desarrollo-de-software]
