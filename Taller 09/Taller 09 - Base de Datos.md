Taller 09											Base de Datos

# **Subconsultas**

## **¿Qué es una subconsulta?**

A veces tenemos que realizar operaciones más complejas con los datos, operaciones en las que nos interesaría ayudarnos de una primera consulta auxiliar que extrajera la información en la que nos queremos basar. Esta consulta auxiliar recibe el nombre de "subconsulta" o "subquery".  
Por ejemplo, si queremos saber qué clientes tenemos en la ciudad que más habitantes tenga, la forma "razonable" de conseguirlo sería saber en primer lugar cual es la ciudad que más habitantes tenga, y entonces lanzar una segunda consulta para ver qué clientes hay en esa ciudad.  
Como la estructura de nuestra base de datos de ejemplo es muy sencilla, no podemos hacer grandes cosas, pero un caso parecido al anterior (aunque claramente más inútil) podría ser saber qué personas tenemos almacenadas que vivan en la última ciudad de nuestra lista.  
Para ello, la primera consulta (la "subconsulta") sería saber cual es la última ciudad de nuestra lista. Si suponemos que los códigos numéricos son crecientess, y lo hacemos tomando la ciudad que tenga el último código (el mayor de ellos), la consulta podría ser:

**SELECT** MAX(codigo) **FROM** ciudades;

Vamos a imaginar que pudiéramos hacerlo en dos pasos. Si llamamos "maxCodigo" a ese código obtenido, la "segunda" consulta podría ser:

**SELECT** \* **FROM** personas **WHERE** codciudad\= maxCodigo;

Pero estos dos pasos se pueden dar en uno: al final de la "segunda" consulta (la "grande") incluimos la primera consulta (la "subconsulta"), entre paréntesis, así

**SELECT** \* **FROM** personas **WHERE** codciudad\= (  
  **SELECT** MAX(codigo) **FROM** ciudades  
);  
 

## **Subconsultas que devuelven conjuntos de datos**

Si la subconsulta no devuelve un único dato, sino un conjunto de datos, la forma de trabajar será básicamente la misma, pero para comprobar si el valor coincide con alguno de la lista, no usaremos el símbolo "=", sino la palabra "in".

Por ejemplo, vamos a hacer una consulta que nos muestre las personas que viven en ciudades cuyo nombre tiene una "a" en segundo lugar (por ejemplo, serían ciudades válidas Madrid o Barcelona, pero no Alicante).  
Para consultar qué letras hay en ciertas posiciones de una cadena, podemos usar SUBSTRING (en el próximo apartado veremos las funciones más importantes de manipulación de cadenas, pero podemos anticipar ya que recibe tres datos: el nombre del campo, la posición inicial y la longitud). Así, una forma de saber qué ciudades tienen una letra A en su segunda posición sería:

**SELECT** codigo **FROM** ciudades   
**WHERE** SUBSTRING(nombre,2,1)='a';  
Como esta subconsulta puede devolver más de un resultado, deberemos usar IN para incluirla en la consulta principal, que quedaría de esta forma:

**SELECT** \* **FROM** personas   
**WHERE** codciudad **IN**   
(  
  **SELECT** codigo **FROM** ciudades **WHERE** SUBSTRING(nombre,2,1)='a'  
);  
