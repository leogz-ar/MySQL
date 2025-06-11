Taller 11									Base de Datos

# **Join**

## **Acercamiento a la necesidad de los JOIN**

Sabemos enlazar varias tablas para mostrar datos que estén relacionados, empleando "WHERE". Por ejemplo, podríamos mostrar nombres de deportistas, junto con los nombres de los deportes que practican. Pero todavía hay un detalle que se nos escapa: ¿cómo hacemos si queremos mostrar todos los deportes que hay en nuestra base de datos, incluso aunque no haya deportistas que los practiquen?  
Vamos a crear una base de datos sencilla para ver un ejemplo de cual es este problema y de cómo solucionarlo.  
Nuestra base de datos se llamará "ejemploJoins":

**CREATE** **DATABASE** ejemploJoins;  
**USE** ejemploJoins;

En ella vamos a crear una primera tabla en la que guardaremos "capacidades" de personas (cosas que saben hacer):

**CREATE** **TABLE** capacidad(  
  codigo varchar(4),  
  nombre varchar(20),  
  **PRIMARY** **KEY**(codigo)  
);

También crearemos una segunda tabla con datos básicos de personas:

**CREATE** **TABLE** persona(  
  codigo varchar(4),  
  nombre varchar(20),  
  codcapac varchar(4),  
  **PRIMARY** **KEY**(codigo)  
);

Vamos a introducir datos de ejemplo:

**INSERT** **INTO** capacidad **VALUES**  
  ('c','Progr.C'),  
  ('pas','Progr.Pascal'),  
  ('j','Progr.Java'),  
  ('sql','Bases datos SQL');  
   
**INSERT** **INTO** persona **VALUES**  
  ('ju','Juan','c'),  
  ('ja','Javier','pas'),  
  ('jo','Jose','perl'),  
  ('je','Jesus','html');

Antes de seguir, comprobamos que todo está bien:

**SELECT** \* **FROM** capacidad;  
\+--------+-----------------+  
| codigo | nombre          |  
\+--------+-----------------+  
| c      | Progr.C         |  
| j      | Progr.Java      |  
| pas    | Progr.Pascal    |  
| sql    | Bases datos SQL |  
\+--------+-----------------+

**SELECT** \* **FROM** persona;  
\+--------+--------+----------+  
| codigo | nombre | codcapac |  
\+--------+--------+----------+  
| ja     | Javier | pas      |  
| je     | Jesus  | html     |  
| jo     | Jose   | perl     |  
| ju     | Juan   | c        |  
\+--------+--------+----------+

Como se puede observar, hay dos capacidades en nuestra base de datos para las que no conocemos a ninguna persona; de igual modo, existen dos personas que tienen capacidades sobre las que no tenemos ningún detalle.  
Por eso, si mostramos las personas con sus capacidades de la forma que sabemos, sólo aparecerán las parejas de persona y capacidad para las que todo está claro (existe persona y existe capacidad), es decir:

**SELECT** \* **FROM** capacidad, persona  
**WHERE** persona.codcapac \= capacidad.codigo;

\+--------+--------------+--------+--------+----------+  
| codigo | nombre       | codigo | nombre | codcapac |  
\+--------+--------------+--------+--------+----------+  
| c      | Progr.C      | ju     | Juan   | c        |  
| pas    | Progr.Pascal | ja     | Javier | pas      |  
\+--------+--------------+--------+--------+----------+

Podemos resumir un poco esta consulta, para mostrar sólo los nombres, que son los datos que más nos interesan:

**SELECT** persona.nombre, capacidad.nombre  
**FROM** persona, capacidad  
**WHERE** persona.codcapac \= capacidad.codigo;

\+--------+--------------+  
| nombre | nombre       |  
\+--------+--------------+  
| Juan   | Progr.C      |  
| Javier | Progr.Pascal |  
\+--------+--------------+

Hay que recordar que la orden "where" es **obligatoria**: si no indicamos esa condición, se mostraría el "**producto cartesiano**" de las dos tablas: todos los pares (persona, capacidad), aunque no estén relacionados en nuestra base de datos:

**SELECT** persona.nombre, capacidad.nombre  
**FROM** persona, capacidad;

\+--------+-----------------+  
| nombre | nombre          |  
\+--------+-----------------+  
| Javier | Progr.C         |  
| Jesus  | Progr.C         |  
| Jose   | Progr.C         |  
| Juan   | Progr.C         |  
| Javier | Progr.Java      |  
| Jesus  | Progr.Java      |  
| Jose   | Progr.Java      |  
| Juan   | Progr.Java      |  
| Javier | Progr.Pascal    |  
| Jesus  | Progr.Pascal    |  
| Jose   | Progr.Pascal    |  
| Juan   | Progr.Pascal    |  
| Javier | Bases datos SQL |  
| Jesus  | Bases datos SQL |  
| Jose   | Bases datos SQL |  
| Juan   | Bases datos SQL |  
\+--------+-----------------+

## **JOIN cruzados, internos y externos**

Pues bien, con órdenes "join" podemos afinar cómo queremos enlazar (en inglés, "join", unir) las tablas. Por ejemplo, si queremos ver todas las personas y todas las capacidades, aunque no estén relacionadas, como en el ejemplo anterior, algo que **no suele tener sentido** en la práctica, lo podríamos hacer con un "**cross join**" (unir de forma cruzada):

**SELECT** persona.nombre, capacidad.nombre  
**FROM** persona **CROSS** **JOIN** capacidad;

\+--------+-----------------+  
| nombre | nombre          |  
\+--------+-----------------+  
| Javier | Progr.C         |  
| Jesus  | Progr.C         |  
| Jose   | Progr.C         |  
| Juan   | Progr.C         |  
| Javier | Progr.Java      |  
| Jesus  | Progr.Java      |  
| Jose   | Progr.Java      |  
| Juan   | Progr.Java      |  
| Javier | Progr.Pascal    |  
| Jesus  | Progr.Pascal    |  
| Jose   | Progr.Pascal    |  
| Juan   | Progr.Pascal    |  
| Javier | Bases datos SQL |  
| Jesus  | Bases datos SQL |  
| Jose   | Bases datos SQL |  
| Juan   | Bases datos SQL |  
\+--------+-----------------+

Si sólo queremos ver los datos que coinciden en ambas tablas, lo que antes conseguíamos comparando los códigos con un "where", también podemos usar un "**inner join**" (unión interior; se puede abreviar simplemente "join"):

**SELECT** persona.nombre, capacidad.nombre  
**FROM** persona **INNER** **JOIN** capacidad  
**ON** persona.codcapac \= capacidad.codigo;

\+--------+--------------+  
| nombre | nombre       |  
\+--------+--------------+  
| Juan   | Progr.C      |  
| Javier | Progr.Pascal |  
\+--------+--------------+

Pero aquí llega la novedad: si queremos ver todas las personas y sus capacidades (si existen), mostrando incluso aquellas personas para las cuales no tenemos constancia de ninguna capacidad, usaríamos un "**left join**" (unión por la izquierda, también se puede escribir "left outer join", unión exterior por la izquierda, para dejar claro que se van a incluir datos que están sólo en una de las dos tablas):

**SELECT** persona.nombre, capacidad.nombre  
**FROM** persona **LEFT** **OUTER** **JOIN** capacidad  
**ON** persona.codcapac \= capacidad.codigo;

\+--------+--------------+  
| nombre | nombre       |  
\+--------+--------------+  
| Juan   | Progr.C      |  
| Javier | Progr.Pascal |  
| Jesus  | NULL         |  
| Jose   | NULL         |  
\+--------+--------------+

De igual modo, si queremos ver todas las capacidades, incluso aquellas para las que no hay detalles sobre personas, podemos escribir el orden de las tablas al revés en la consulta anterior, o bien usar "**right join**" (o "right outer join"):

**SELECT** persona.nombre, capacidad.nombre  
**FROM** persona **RIGHT** **OUTER** **JOIN** capacidad  
**ON** persona.codcapac \= capacidad.codigo;

\+--------+-----------------+  
| nombre | nombre          |  
\+--------+-----------------+  
| Javier | Progr.Pascal    |  
| Juan   | Progr.C         |  
| NULL   | Progr.Java      |  
| NULL   | Bases datos SQL |  
\+--------+-----------------+

El significado de "LEFT" y de "RIGHT" hay que buscarlo en la posición en la que se enumeran las tablas en el bloque "FROM". Así, la última consulta se puede escribir también como un LEFT JOIN si se indica la capacidad en segundo lugar (en la parte izquierda), así:

**SELECT** persona.nombre, capacidad.nombre  
**FROM** capacidad **LEFT** **OUTER** **JOIN** persona  
**ON** persona.codcapac \= capacidad.codigo;

\+--------+-----------------+  
| nombre | nombre          |  
\+--------+-----------------+  
| Javier | Progr.Pascal    |  
| Juan   | Progr.C         |  
| NULL   | Progr.Java      |  
| NULL   | Bases datos SQL |  
\+--------+-----------------+  
Otros gestores de bases de datos permiten combinar el "right join" y el "left join" en una única consulta, usando "full outer join", algo que no permite MySQL en su versión actual, pero que se puede imitar de una forma que veremos en el próximo apartado.

