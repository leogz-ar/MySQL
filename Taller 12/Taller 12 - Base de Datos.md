Taller 12											Base de Datos

# **Union**

## **Unión de dos consultas**

Se puede unir dos consultas en una, empleando la palabra "UNION". El requisito es que ambas consultas deben devolver campos "similares" (mismo nombre y tipo de datos). Como primer ejemplo (un tanto innecesario), podríamos crear una consulta que muestre las personas cuyo nombre contiene una "o" y aquellas cuyo nombre tiene una "e":

**SELECT** nombre **FROM** persona  
**WHERE** nombre **LIKE** '%o%'  
union  
**SELECT** nombre **FROM** persona  
**WHERE** nombre **LIKE** '%e%';

que mostraría como resultado:

\+--------+  
| nombre |  
\+--------+  
| Jose   |  
| Javier |  
| Jesus  |  
\+--------+

(Como has podido observar, "Jose" no aparece repetido, porque el operador "UNION" elimina duplicados).  
En este caso, el uso de "UNION" es poco razonable porque se trata de dos consultas casi idénticas, que se podían haber realizado simplemente con un "OR" y se obtendrá el mismo resultado (quizá en distinto orden, ya que no hemos utilizado "ORDER BY"):

**SELECT** nombre **FROM** persona  
**WHERE** nombre **LIKE** '%o%'   
**OR** nombre **LIKE** '%e%';  
\+--------+  
| nombre |  
\+--------+  
| Javier |  
| Jesus  |  
| Jose   |  
\+--------+

La verdadera utilidad de "UNION" aparece cuando se trabaja sobre conjuntos de datos diferentes, incluso distintas tablas. Por ejemplo, podríamos obtener los nombres de todos las personas y los de todas las capacidades en una misma consulta:

**SELECT** nombre **FROM** persona  
union  
**SELECT** nombre **FROM** capacidad;

que mostraría:

\+-----------------+  
| nombre          |  
\+-----------------+  
| Javier          |  
| Jesus           |  
| Jose            |  
| Juan            |  
| Progr.C         |  
| Progr.Java      |  
| Progr.Pascal    |  
| Bases datos SQL |  
\+-----------------+

E incluso se podrían mostrar datos originalmente muy distintos, si se renombran empleando un alias:

**SELECT** concat('Persona: ', nombre) **AS** detalle  
**FROM** persona  
union  
**SELECT** concat('Habilidad: ', upper(nombre)) **AS** detalle  
**FROM** capacidad;

\+----------------------------+  
| detalle                    |  
\+----------------------------+  
| Persona: Javier            |  
| Persona: Jesus             |  
| Persona: Jose              |  
| Persona: Juan              |  
| Habilidad: PROGR.C         |  
| Habilidad: PROGR.JAVA      |  
| Habilidad: PROGR.PASCAL    |  
| Habilidad: BASES DATOS SQL |  
\+----------------------------+

## **Imitando un FULL OUTER JOIN**

En el apartado anterior comentábamos que la versión actual de MySQL no permite usar "full outer join" para mostrar todos los datos que hay en dos tablas enlazadas, aunque alguno de esos datos no tenga equivalencia en la otra tabla.  
También decíamos que se podría imitar haciendo a la vez un "right join" y un "left join".  
En general, tenemos la posibilidad de unir dos consultas en una usando "union", así:

**SELECT** persona.nombre, capacidad.nombre  
**FROM** persona **RIGHT** **OUTER** **JOIN** capacidad  
**ON** persona.codcapac \= capacidad.codigo  
union  
**SELECT** persona.nombre, capacidad.nombre  
**FROM** persona **LEFT** **OUTER** **JOIN** capacidad  
**ON** persona.codcapac \= capacidad.codigo;

\+--------+-----------------+  
| nombre | nombre          |  
\+--------+-----------------+  
| Javier | Progr.Pascal    |  
| Juan   | Progr.C         |  
| NULL   | Progr.Java      |  
| NULL   | Bases datos SQL |  
| Jesus  | NULL            |  
| Jose   | NULL            |  
\+--------+-----------------+

Los datos no aparecen ordenados. Si se desea que lo estén, se puede incluir la "UNION" dentro de una subconsulta (será necesario usar un "alias" para la subconsulta), así:

**SELECT** \* **FROM**  
(  
**SELECT** persona.nombre , capacidad.nombre habilidad  
**FROM** persona **RIGHT** **OUTER** **JOIN** capacidad  
**ON** persona.codcapac \= capacidad.codigo  
union  
**SELECT** persona.nombre, capacidad.nombre  
**FROM** persona **LEFT** **OUTER** **JOIN** capacidad  
**ON** persona.codcapac \= capacidad.codigo  
) resultado  
**ORDER** **BY** nombre;

\+--------+-----------------+  
| nombre | habilidad       |  
\+--------+-----------------+  
| NULL   | Progr.Java      |  
| NULL   | Bases datos SQL |  
| Javier | Progr.Pascal    |  
| Jesus  | NULL            |  
| Jose   | NULL            |  
| Juan   | Progr.C         |  
\+--------+-----------------+

(Como puedes ver, al ordenar resultados, los datos nulos aparecen antes de los que sí tienen valor).  
Nota: en algunos gestores de bases de datos, podemos no sólo crear "uniones" entre dos tablas, sino también realizar otras operaciones habituales entre conjuntos, como calcular su intersección ("intersection") o ver qué elementos hay en la primera pero no en la segunda (diferencia, "difference"). Estas posibilidades no están disponibles en la versión actual de MySQL.  
   
