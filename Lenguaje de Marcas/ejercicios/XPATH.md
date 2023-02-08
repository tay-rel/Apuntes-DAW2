
1. **Nombre de la universidad:**

```xpath
/universidad/nombre
```

>NOTA: **( / )** indica el nodo raíz, si no, indica "hijo".

2. **Pais de la Universidad:**

```xpath
/universidad/pais
```

3. **Nombre de carreras:**

```xpath
//carrera/nombre

Es lo mismo que:
/carreras/carrera/nombre
```

4. **Años de plan de estudio de las carreras:**

```xpath
//carrera/plan
```

>NOTA:  **( // )**: indica "descendiente"

5. **Nombres de todos los alumnos:**

```xpath
//alumnos/nombre
```

6. **Identificadores de todas las carreras:**

```xpath
//carrera@id
```

>NOTA: **@_atributo_**: selecciona el atributo.

7. **Datos de la carrera cuyo id es c01:**

```xpath
//carrera[@id = 'c01']
```

>NOTA: **[@atributo]**  : selecciona los elementos que tienen el atributo.

8. **Centro en que se estudia de la carrera cuyo id es c02:**

```xpath
//carrera[@id = 'c02']/centro
```

9. **Nombre de las carreras que tengan subdirector:**

```xpath
//subdirector/../nombre
(Pido el nombre del elemento padre del subdirector)
```

10. **Nombre de los alumnos que estén haciendo proyecto:**

```xpath
//alumno/proyecto/../../nombre
```

11. **Códigos de las carreras en las que hay algún alumno matriculado:**

```xpath
//alumno/estudios/carrera/@codigo
```

12. **Apellidos y Nombre de los alumnos con beca:**

```xpath
//alumno[@beca]/nombre | //alumno[@beca]/apellido1 | //alumno[@beca]/apellido2
```

13. **Nombre de las asignaturas de la titulación c04:**

```xpath
//asignatura[@titulacion = 'c04']/nombre
```

14. **Nombre de las asignaturas de segundo trimestre:**

```xpath
//asignatura[trimestre= 2]/nombre
```

> NOTA: **[condicion]** selecciona los nodos que cumplen la condición. La condición puede utilizar el valor de un atributo (utilizando @) o el texto que contiene el elemento.

15. **Nombre de las asignaturas que no tienen 4 créditos teóricos:**

```xpath
//asignatura[creditos_teoricos = !4]/nombre
```

16. **Código de la carrera que estudia el último alumno:**

```xpath
//alumno[last()]//carrera/@codigo
```

17. **Código de las asignaturas que estudian mujeres:**

```xpath
//alumno[sexo='mujer']//asignatura/@codigo
```

```xquery
<ul>
{
for $a in distinct-values(//alumno[sexo='mujer']//asignatura/@codigo/string())
order by $a
return <li>{$a}</li>
}
</ul>
```

18. **Nombre de los alumnos que matriculados en la asignatura a02:**

```xpath
//alumno[.//asignatura/@codigo='a02']/nombre
```

19. **Códigos de las carreras que estudian los alumnos matriculados en alguna asignatura:**

```xpath
//alumno//asignatura/../../carrera/@codigo
```

20. **Apellidos de todos los hombres:**

```xpath
//alumno[sexo='Hombre']/apellido1 | //alumno[sexo='Hombre']/apellido2
```

21. **Nombre de la carrera que estudia Víctor Manuel:**

```xpath

//carrera[@id=//alumno[nombre=' Víctor Manuel']/estudios/carrera/@codigo]/nombre

```

22. **Nombre de las asignaturas que estudia Luisa:**

```xpath

//asignatura[@id=//alumno[nombre='Luisa']//asignatura/@codigo]/nombre

```

23. **Primer apellido de los alumnos matriculados en Ingeniería del Software:**

```xpath 

//alumno[.//asignatura/@codigo=//asignatura[nombre='Ingeniería del Software']/@id]/apellido1

```

> NOTA: operadores de comparación: =

24. **Nombre de las carreras que estudian los alumnos matriculados en la asignatura Tecnología de los Alimentos:**

```xpath 

//carrera[@id=//alumno[.//asignatura[@codigo=//asignatura[nombre='Tecnología de los Alimentos']/@id]]//carrera/@codigo]/nombre

```

25. **Nombre de los alumnos matriculados en carreras que no tienen subdirector:**

```xpath
//alumno[.//carrera/@codigo!=//carrera[subdirector]/@id]/nombre
```

26. **Nombre de las alumnos matriculados en asignaturas con 0 créditos prácticos y que estudien la carrera de I.T. Informática :**

```xpath

//alumno[.//asignatura/@codigo=//asignatura[creditos_practicos=0]/@id][.//carrera/@codigo=//carrera[nombre='I.T.Informática']/@id]/nombre

```

El punto del alumno , mira si la asignatura es el codigo .

27. **Nombre de los alumnos que estudian carreras cuyos planes son anteriores a 2002:**

```xpath
//alumno[.//carrera/@codigo=//carrera[plan<2002]/@id]/nombre
```