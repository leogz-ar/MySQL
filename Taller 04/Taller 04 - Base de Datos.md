Taller 04											 Base de Datos

# **Borrar información**

# **¿Qué información hay?**

Un primer paso antes de ver cómo borrar información es saber qué información tenemos almacenada.  
Podemos saber las bases de datos que hay creadas en nuestro sistema con:

**SHOW** **DATABASES**;

Una vez que estamos trabajando con una base de datos concreta (con la orden "USE"), podemos saber las tablas que contiene con:

**SHOW** **TABLES**;

Y para una tabla concreta, podemos saber los campos (columnas) que la forman con "SHOW COLUMNS FROM":

**SHOW** **COLUMNS** **FROM** personas;

Por ejemplo, esto daría como resultado:

\+-----------+-----------------+-------------+------+---------+-------+  
| Field       | Type              | Null           | Key | Default | Extra |  
\+-----------+-----------------+-------------+------+---------+-------+  
| nombre    | varchar(20)  | YES         |         | NULL  |          |  
| direccion  | varchar(40)  | YES         |         | NULL  |          |  
| edad         | decimal(3,0) | YES        |         | NULL  |          |  
| codciudad | varchar(3)   | YES         |         | NULL  |          |  
\+--------------+---------------+------------+-------+--------+-------+ 

## **Borrar toda la base de datos**

En alguna ocasión, como ahora que estamos practicando, nos puede interesar borrar toda la base de datos. La orden para conseguirlo es "DROP DATABASE":

**DROP** **DATABASE** ejemplo2;

Si esta orden es parte de una secuencia larga de órdenes, que hemos cargado por ejemplo con la orden "source", puede ocurrir que la base de datos no exista. En ese caso, obtendríamos un mensaje de error y se interrumpiría el proceso en ese punto. Podemos evitarlo añadiendo "IF EXISTS", para que se borre la base de datos sólo si realmente existe:

**DROP** **DATABASE** ejemplo2 **IF** **EXISTS**;  
 

## **Borrar una tabla**

Es más frecuente que creemos alguna tabla de forma incorrecta. La solución razonable es corregir ese error, cambiando la estructura de la tabla, pero todavía no sabemos hacerlo. De momento veremos cómo borrar una tabla. La orden es "DROP TABLE":

**DROP** **TABLE** personas;

Al igual que para las bases de datos, podemos hacer que la tabla se borre sólo cuando realmente existe:

**DROP** **TABLE** personas **IF** **EXISTS**;

## **Borrar datos de una tabla**

También podemos borrar los datos que cumplen una cierta condición. La orden es "DELETE FROM", y con la cláusula "WHERE" indicamos las condiciones que se deben cumplir, de forma similar a como hacíamos en la orden "SELECT":

**DELETE** **FROM** personas **WHERE** nombre \= 'juan';

Esto borraría todas las personas llamadas "juan" que estén almacenadas en la tabla "personas".  
Cuidado: si no se indica el bloque "WHERE", no se borrarían los datos que cumplen una condición (porque NO HAY condición), sino TODOS los datos existentes. Si es eso lo que se pretende, una forma más rápida de conseguirlo es usar "TRUNCATE TABLE":

**TRUNCATE** **TABLE** personas;  
   
