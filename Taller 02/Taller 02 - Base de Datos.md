Taller 02											Base de Datos

# **Consultas básicas con dos tablas**

## **Formalizando conceptos**

Hay una serie de detalles que hemos pasado por alto y que deberíamos formalizar un poco antes de seguir:

* **SQL** es un lenguaje de consulta a bases de datos. Sus siglas vienen de **Structured Query Language** (lenguaje de consulta estructurado).  
* **MySQL** es un "gestor de bases de datos", es decir, una aplicación informática que se usa para almacenar, obtener y modificar datos (realmente, se les exige una serie de cosas más, como controlar la integridad de los datos o el acceso por parte de los usuarios, pero por ahora nos basta con eso).  
* En SQL, las órdenes que tecleamos deben terminar en **punto y coma** (;). Si tecleamos una orden como "SELECT \* FROM personas", sin el punto y coma final, y pulsamos Intro, MySQL responderá mostrando "-\>" para indicar que todavía no hemos terminado la orden y nos queda algo más por introducir. Precisamente por eso, las órdenes se pueden partir para que ocupen varias líneas, como hemos hecho con "CREATE TABLE".  
* Las órdenes de SQL se pueden introducir en mayúsculas o minúsculas. Aún así, es frecuente emplear mayúsculas para las órdenes y minúsculas para los campos y tablas, de modo que la sentencia completa resulte más legible. Así, para mostrar todos los nombres almacenados en nuestra tabla personas, se podría hacer con:

**SELECT** nombre **FROM** personas;  
   
**¿Por qué varias tablas?**  
Puede haber varios motivos:  
Por una parte, podemos tener bloques de información claramente distintos. Por ejemplo, en una base de datos que guarde la información de una empresa tendremos datos como los artículos que distribuimos y los clientes que nos los compran, que no deberían guardarse en una misma tabla.  
Por otra parte, habrá ocasiones en que descubramos que los datos, a pesar de que se podrían clasificar dentro de un mismo "bloque de información" (tabla), serían redundantes: existiría gran cantidad de datos repetitivos, y esto puede dar lugar a dos problemas:

* Espacio desperdiciado.  
* Posibilidad de errores al introducir los datos, lo que daría lugar a inconsistencias:

   
Veamos unos datos de ejemplo, que podrían ser parte de una tabla ficticia:  
\+-----------+------------+------------+  
| nombre  | direccion | ciudad    |  
\+-----------+------------+------------+  
| juan        | su casa   | alicante   |  
| alberto    | calle uno | alicante   |  
| pedro      | su calle   | alicantw  |  
\+-----------+------------+------------+

Si en vez de repetir "alicante" en cada una de esas fichas, utilizáramos un código de ciudad, por ejemplo "a", gastaremos menos espacio (en este ejemplo, 7 bytes menos en cada ficha; con la capacidad de un ordenador actual eso no sería un gran problema).  
Por otra parte, hemos tecleado mal uno de los datos: en la tercera ficha no hemos indicado "alicante", sino "alicantw", de modo que si hacemos consultas sobre personas de Alicante, la última de ellas no aparecería. Al teclear menos, es también más difícil cometer este tipo de errores.  
A cambio, necesitaremos una segunda tabla, en la que guardemos los códigos de las ciudades, y el nombre al que corresponden (por ejemplo: si códigoDeCiudad \= "a", la ciudad es "alicante").

Esta tabla se podría crear así:

**CREATE** **TABLE** ciudades (  
  codigo varchar(3),  
  nombre varchar(30)  
);  
 

## **Las claves primarias**

Generalmente, y especialmente cuando se usan varias tablas enlazadas, será necesario tener algún dato que nos permita distinguir de forma clara los datos que tenemos almacenados. Por ejemplo, el nombre de una persona no es único: pueden aparecer en nuestra base de datos varios usuarios llamados "Juan López". Si son nuestros clientes, debemos saber cual es cual, para no cobrar a uno de ellos un dinero que corresponde a otro. Eso se suele solucionar guardando algún dato adicional que sí sea único para cada cliente, como puede ser el Documento Nacional de Identidad, o el Pasaporte. Si no hay ningún dato claro que nos sirva, en ocasiones añadiremos un "código de cliente", inventado por nosotros, o algo similar.  
Estos datos que distinguen claramente unas "fichas" de otras los llamaremos "**claves primarias**".  
Se puede crear una tabla indicando su clave primaria si se añade "PRIMARY KEY" al final de la orden "CREATE TABLE", así:

**CREATE** **TABLE** ciudades (  
  codigo varchar(3),  
  nombre varchar(30),  
  **PRIMARY** **KEY** (codigo)  
);

o bien al final de la definición del campo correspondiente, así:

**CREATE** **TABLE** ciudades (  
  codigo varchar(3) **PRIMARY** **KEY**,  
  nombre varchar(30)  
);  
 

## **Creando datos**

Comenzaremos creando una nueva base de datos, de forma similar al ejemplo anterior:

**CREATE** **DATABASE** ejemplo2;  
**USE** ejemplo2;  
Después creamos la tabla de ciudades, que guardará su nombre y su código. Este código será el que actúe como "clave primaria", para distinguir otra ciudad. Por ejemplo, hay una ciudad llamado "Toledo" en España, pero también otra en Argentina, otra en Uruguay, dos en Colombia, una en Ohio (Estados Unidos)... el nombre claramente no es único, así que podríamos usar código como "te" para Toledo de España, "ta" para Toledo de Argentina y así sucesivamente.

La forma de crear la tabla con esos dos campos y con esa clave primaria sería:

**CREATE** **TABLE** ciudades (  
  codigo varchar(3),  
  nombre varchar(30),  
  **PRIMARY** **KEY** (codigo)  
);

Mientras que la tabla de personas sería casi igual al ejemplo anterior, pero añadiendo un nuevo dato: el código de la ciudad.

**CREATE** **TABLE** personas (  
  nombre varchar(20),  
  direccion varchar(40),  
  edad decimal(3),  
  codciudad varchar(3)  
);

Para introducir datos, el hecho de que exista una clave primaria no supone ningún cambio, salvo por el hecho de que no se nos dejaría introducir dos ciudades con el mismo código:

**INSERT** **INTO** ciudades **VALUES** ('a', 'alicante');  
**INSERT** **INTO** ciudades **VALUES** ('b', 'barcelona');  
**INSERT** **INTO** ciudades **VALUES** ('m', 'madrid');  
   
**INSERT** **INTO** personas **VALUES** ('juan', 'su casa', 25, 'a');  
**INSERT** **INTO** personas **VALUES** ('pedro', 'su calle', 23, 'm');  
**INSERT** **INTO** personas **VALUES** ('alberto', 'calle uno', 22, 'b');  
 

## **Mostrando datos**

Cuando queremos mostrar datos de varias tablas a la vez, deberemos hacer unos pequeños cambios en las órdenes "select" que hemos visto:

* En primer lugar, indicaremos varios nombres después de "FROM" (los de cada una de las tablas que necesitemos).  
* Además, puede ocurrir que cada tengamos campos con el mismo nombre en distintas tablas (por ejemplo, el nombre de una persona y el nombre de una ciudad), y en ese caso deberemos escribir el nombre de la tabla antes del nombre del campo.

Por eso, una consulta básica sería algo parecido (sólo parecido) a:

**SELECT** personas.nombre, direccion, ciudades.nombre **FROM** personas, ciudades;

Pero esto todavía tiene problemas: estamos combinando TODOS los datos de la tabla de personas con TODOS los datos de la tabla de ciudades, de modo que obtenemos 3x3 \= 9 resultados:

\+-----------+------------+-------------+  
| nombre  | direccion | nombre    |  
\+-----------+------------+-------------+  
| juan        | su casa   | alicante    |  
| pedro     | su calle    | alicante    |  
| alberto   | calle uno  | alicante     |  
| juan       | su casa    | barcelona |  
| pedro     | su calle    | barcelona |  
| alberto   | calle uno  | barcelona |  
| juan       | su casa    | madrid     |  
| pedro     | su calle    | madrid     |  
| alberto   | calle uno   | madrid    |  
\+-----------+-------------+------------+  
9 rows in set (0.00 sec)

Pero esos datos no son reales: si "juan" vive en la ciudad de código "a", sólo debería mostrarse junto al nombre "alicante". Nos falta indicar esa condición: "el código de ciudad que aparece en la persona debe ser el mismo que el código que aparece en la ciudad", así:

**SELECT** personas.nombre, direccion, ciudades.nombre  
**FROM** personas, ciudades   
**WHERE** personas.codciudad \= ciudades.codigo;

Esta será la forma en que trabajaremos normalmente. Este último paso se puede evitar en ciertas circunstancias, pero ya lo veremos más adelante. El resultado de esta consulta sería:

\+-----------+------------+--------------+  
| nombre  | direccion | nombre     |  
\+-----------+------------+--------------+  
| juan        | su casa   | alicante    |  
| alberto    | calle uno | barcelona |  
| pedro      | su calle   | madrid     |  
\+-----------+------------+-------------+

Ese sí es el resultado correcto. Cualquier otra consulta que implique las dos tablas deberá terminar comprobando que los dos códigos coinciden. Por ejemplo, para ver qué personas viven en la ciudad llamada "madrid", haríamos:

**SELECT** personas.nombre, direccion, edad   
**FROM** personas, ciudades   
**WHERE** ciudades.nombre\='madrid'   
**AND** personas.codciudad \= ciudades.codigo;

\+----------+-------------+-------+  
| nombre | direccion  | edad |  
\+----------+-------------+-------+  
| pedro    | su calle     |   23   |  
\+----------+-------------+-------+

Y para saber las personas de ciudades que comiencen con la letra "b", haríamos:

**SELECT** personas.nombre, direccion, ciudades.nombre   
**FROM** personas, ciudades   
**WHERE** ciudades.nombre **LIKE** 'b%'   
**AND** personas.codciudad \= ciudades.codigo;

\+-----------+------------+-------------+  
| nombre  | direccion | nombre    |  
\+-----------+------------+-------------+  
| alberto    | calle uno | barcelona |  
\+-----------+------------+-------------+

Si en nuestra tabla puede haber algún dato que se repita, como la dirección, podemos pedir un listado sin duplicados, usando la palabra "distinct":

**SELECT** **DISTINCT** direccion **FROM** personas;  
   
**Ejecutando un lote de órdenes**  
Hasta ahora hemos tecleado todas las órdenes desde dentro del entorno de MySQL, una por una. Tenemos otra opción que también puede ser cómoda: crear un fichero de texto que contenga todas las órdenes (por ejemplo, usando un editor de texto avanzado, como Geany o Notepad++) y cargarlo después desde MySQL. Lo podemos hacer de tres formas:

* Usar la orden "source": desde dentro de MySQL tecleamos algo como

source ejemplo2.sql;

* Cargar las órdenes justo en el momento de entrar a MySQL, con

mysql \-u root \< ejemplo2.sql;

(Pero esta alternativa tiene un problema: se darán los pasos que indiquemos en "ejemplo2.sql" y se abandonará el entorno de MySQL, sin que nos dé tiempo de comprobar si ha existido algún mensaje de error).

* Preparar las órdenes con el editor avanzado, para luego "copiar y pegar" sobre la consola de MySQL.

