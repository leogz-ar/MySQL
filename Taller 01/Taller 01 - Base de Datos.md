**Taller 01**

Taller práctico de MySQL.

crearemos una **base de datos** "ejemplo1". Esta base de datos contendrá una única **tabla**, llamada "agenda" donde guardaremos datos básicos de contactos (nombre, dirección y edad).

Para crear la base de datos, la estructura que contiene todo, usaremos "create database", seguido del nombre que tendrá la base de datos.

**CREATE** **DATABASE** ejemplo1;

Como podemos tener varias bases de datos cada vez que accedemos a MySQL deberemos comenzar por indicar cuál de ellas queremos usar, con la orden "use":

**USE** ejemplo1;

Creación de una tabla

Una base de datos, en general, estará formada por varios bloques de información llamados "**tablas**". En nuestro caso, nuestra única tabla almacenará los datos de nuestros contactos, para crearla debemos decidir qué datos concretos guardaremos de cada contacto (“campos”) y qué tamaño necesitaremos para cada uno de esos datos.

Por ejemplo, podríamos decidir lo siguiente:  
nombre \- texto, hasta 20 letras  
dirección \- texto, hasta 40 letras  
edad \- números, de hasta 3 cifras

Cada gestor de bases de datos tendrá una forma de llamar a esos tipos de datos. Por ejemplo, en MySQL podemos usar "VARCHAR" para referirnos a texto hasta una cierta longitud variable, y "NUMERIC" para números de una determinada cantidad de cifras. Además, en la mayoría de gestores de base de datos, será recomendable (a veces incluso obligatorio) que los nombres de los campos no contengan acentos ni caracteres internacionales, y los nombres de tablas y campos se suelen indicar en minúsculas, de modo que la orden necesaria para crear esta tabla sería:

**CREATE** **TABLE** personas (  
  nombre varchar(20),  
  direccion varchar(40),  
  edad numeric(3)  
);

***NOTA:** Todo desarrollador debe seguir un protocolo de nombres para mantener un orden de trabajo.*

Introducción de datos

Para introducir datos usaremos la orden "**INSERT INTO**", e indicaremos tras la palabra "**VALUES**" los valores que deseamos insertar, en el mismo orden en el que se habían definido los campos. Los valores de los campos de texto se deberán indicar entre comillas, lo cual no es necesario para los valores de campos numéricos.: 

**INSERT** **INTO** personas **VALUES** ('juan', 'su casa', 25);

Si no queremos indicar todos los datos, deberemos detallar qué campos vamos a introducir y en qué orden, así:

**INSERT** **INTO** personas (nombre, direccion) **VALUES** ('Héctor', 'calle 1');

(Como se ve en este ejemplo, los datos si podran contener acentos y respetar las mayúsculas y minúsculas donde sea necesario). De igual modo, también será necesario indicar los campos si se desea introducir los datos en un orden distinto al de definición:

**INSERT** **INTO** personas (direccion, edad, nombre) **VALUES** ("Calle 2", 30, "Daniel");

Y como puedes apreciar en este ejemplo, los campos de texto se pueden indicar también entre comillas dobles, si te resulta más cómodo).

Mostrar datos

Para ver los datos almacenados en una tabla usaremos el formato "**SELECT** campos **FROM** tabla". Si queremos ver todos los campos, lo indicaremos con un asterisco: 

**SELECT** \* **FROM** personas;

que, en nuestro caso, daría como resultado.

\+---------+-----------+------+  
| nombre  | direccion | edad |  
\+---------+-----------+------+  
| juan    | su casa   |   25 |  
| Héctor  | calle 1   | NULL |  
| Daniel  | Calle 2   |   30 |  
\+---------+-----------+------+

Como puedes observar, no habíamos introducido los detalles de la edad de Héctor. No se muestra un cero ni un espacio en blanco, sino que se anota que hay un "**valor nulo**" (NULL) para ese campo. Si queremos ver **sólo ciertos campos**, deberemos detallar sus nombres, en el orden deseado, separados por comas:

**SELECT** nombre, direccion **FROM** personas;  
\+---------+-----------+  
| nombre  | direccion |  
\+---------+-----------+  
| juan    | su casa   |  
| Héctor  | calle 1   |  
| Daniel  | Calle 2   |  
\+---------+-----------+

Buscar por contenido

Normalmente no querremos ver todos los datos que hemos introducido, sino sólo aquellos que cumplan cierta condición. Esta condición se indica añadiendo un apartado **WHERE** a la orden "SELECT". Por ejemplo, se podrían ver los datos de una persona a partir de su nombre de la siguiente forma:

**SELECT** nombre, direccion **FROM** personas **WHERE** nombre \= 'juan';  
\+--------+-----------+  
| nombre | direccion |  
\+--------+-----------+  
| juan   | su casa   |  
\+--------+-----------+

En ocasiones no queremos comparar todo el campo con un texto exacto, sino ver si contiene un cierto texto (por ejemplo, porque sólo sepamos un apellido o parte de la calle). En ese caso usamos la palabra "**LIKE**", y para las partes que no conozcamos usaremos el comodín "%", como en este ejemplo:

**SELECT** nombre, direccion **FROM** personas **WHERE** direccion **LIKE** '%calle%';

este comando lista el nombre y la dirección de nuestros contactos que viven en lugares que contengan la palabra "calle", aunque ésta pueda estar precedida por cualquier texto (%) y quizá también con cualquier texto (%) a continuación:

\+---------+-----------+  
| nombre  | direccion |  
\+---------+-----------+  
| Héctor  | calle 1   |  
| Daniel  | Calle 2   |  
\+---------+-----------+

Mostrar datos ordenados

Cuando una consulta devuelva muchos datos, resultará poco legible si éstos se encuentran desordenados. Por eso, será frecuente añadir "ORDER BY" seguido del nombre del campo que se quiere usar para ordenar (por ejemplo "ORDER BY nombre").  
Es posible indicar varios campos de ordenación, de modo que se mire más de un criterio en caso de que un campo coincida (por ejemplo "ORDER BY apellidos, nombre").  
Si se quiere que alguno de los criterios de ordenación se aplique en orden contrario (por ejemplo, textos de la Z a la A o números del valor más grande al más pequeño), se puede añadir la palabra "DESC" (de "descending", "descendiendo") al criterio correspondiente, como en "ORDER BY precio DESC". Si no se indica esa palabra, se sobreentiende que se quiere obtener los resultados en orden ascendente (lo que indicaría la palabra "ASC", que es opcional).

**SELECT** nombre, direccion   
**FROM** personas   
**WHERE** direccion **LIKE** '%calle%'  
**ORDER** **BY** nombre, direccion **DESC**;  
\+---------+-----------+  
| nombre  | direccion |  
\+---------+-----------+  
| Daniel  | Calle 2   |  
| Héctor  | calle 1   |  
\+---------+-----------+

Salir de MySQL  
Para terminar nuestra sesión de MySQL, tecleamos la orden **quit** o **exit** y volveremos al sistema operativo.
