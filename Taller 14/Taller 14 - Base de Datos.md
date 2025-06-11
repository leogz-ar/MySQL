Taller 14											Base de Datos

# **Triggers**

## **Contacto con los "triggers"**

En MySQL (a partir de la versión 5.0.2) se permite utilizar "disparadores" (triggers), que son una serie de pasos que se pondrán en marcha cuando ocurra un cierto evento en una tabla.  
Los eventos pueden ser un INSERT, un UPDATE o un DELETE de datos de la tabla, y podemos detallar si queremos que los pasos se den antes (BEFORE) del evento o después (AFTER) del evento.  
Como ejemplo habitual, podríamos hacer un BEFORE INSERT para comprobar que los datos son válidos antes de guardarlos realmente en la tabla.  
Pero vamos a empezar probar con un ejemplo, que aunque sea menos útil, será más fácil de aplicar.  
Vamos a crear una base de datos sencilla, con sólo dos tablas. En una tabla guardaremos datos de personas, y en la otra anotaremos cuando se ha introducido cada dato. La estructura básica sería ésta:

**CREATE** **DATABASE** ejemplotriggers;  
**USE** ejemplotriggers;  
   
**CREATE** **TABLE** persona (  
 codigo varchar(10),  
 nombre varchar(50),  
 edad decimal(3),  
 **PRIMARY** **KEY** (\`codigo\`)  
);  
   
**CREATE** **TABLE** nuevosDatos (  
 codigo varchar(10),  
 cuando date,  
 tipo char(1)  
);

Para que se añada un dato en la segunda tabla cada vez que insertemos en la primera, creamos un TRIGGER que saltará con un AFTER INSERT. Para indicar los pasos que debe hacer, se usa la expresión "FOR EACH ROW" (para cada fila), así:

**CREATE** **TRIGGER** modificacion  
  AFTER **INSERT** **ON** persona  
**FOR** EACH ROW  
  **INSERT** **INTO** nuevosDatos  
  **VALUES** (NEW.codigo, CURRENT\_DATE, 'i');

(Los datos que introduciremos serán: el código de la persona, la fecha actual y una letra "i" para indicar que el cambio ha sido la "inserción" de un dato nuevo).  
Si ahora introducimos un dato en la tabla personas:

**INSERT** **INTO** persona  
**VALUES** ('1','Juan',20);  
La tabla de "nuevosDatos" habrá cambiado:  
**SELECT** \* **FROM** nuevosDatos;

\+--------+------------+------+  
| codigo | cuando     | tipo |  
\+--------+------------+------+  
| 1      | 2007-12-05 | i    |  
\+--------+------------+------+

(Nota 1: Si en vez de monitorizar los INSERT, queremos controlar los UPDATE, el valor actual del nombre es "NEW.nombre", pero también podemos saber el valor anterior con "OLD.nombre", de modo que podríamos almacenar en una tabla todos los detalles sobre el cambio que ha hecho el usuario).  
(Nota 2: Si no queremos guardar sólo la fecha actual, sino la fecha y la hora, el campo debería ser de tipo DATETIME, y sabríamos el instante actual con "NOW()"):

**CREATE** **TRIGGER** modificacion  
  AFTER **INSERT** **ON** persona  
**FOR** EACH ROW  
  **INSERT** **INTO** nuevosDatos  
  **VALUES** (NEW.codigo, NOW(), 'i');

Si queremos indicar que se deben dar secuencias de pasos más largas, deberemos tener en cuenta dos cosas: cuando sean varias órdenes, deberán encerrarse entre BEGIN y END; además, como cada una de ellas terminará en punto y coma, deberemos cambiar momentáneamente el "delimitador" (DELIMITER) de MySQL, para que no piense que hemos terminado en cuanto aparezca el primer punto y coma:  
DELIMITER |  
   
**CREATE** **TRIGGER** validacionPersona BEFORE **INSERT** **ON** persona  
**FOR** EACH ROW BEGIN  
  **SET** NEW.codigo \= UPPER(NEW.codigo);  
  **SET** NEW.edad \= **IF**(NEW.edad \= 0, **NULL**, NEW.edad);  
END;  
|  
DELIMITER ;

(Nota 3: Ese nuevo delimitador puede ser casi cualquiera, siempre y cuando no se algo que aparezca en una orden habitual. Hay quien usa |, quien prefiere ||, quien usa //, etc.)

En este ejemplo, usamos SET para cambiar el valor de un campo. En concreto, antes de guardar cada dato, convertimos su código a mayúsculas (usando la función UPPER, que ya conocíamos), y guardamos NULL en vez de la edad si la edad tiene un valor incorrecto (0, por ejemplo), para lo que usamos la función IF, que aún no conocíamos. Esta función recibe tres parámetros: la condición a comprobar, el valor que se debe devolver si se cumple la condición, y el valor que se debe devolver cuando no se cumpla la condición.  
Si añadimos un dato que tenga un código en minúsculas y una edad 0, y pedimos que se nos muestre el resultado, veremos ésto:

**INSERT** **INTO** persona  
**VALUES** ('p','Pedro',0)

\+--------+---------+------+  
| codigo | nombre  | edad |  
\+--------+---------+------+  
| 1      | Juan    |   20 |  
| P      | Pedro   | NULL |  
\+--------+---------+------+  
Cuando un TRIGGER deje de sernos útil, podemos eliminarlo con DROP TRIGGER.  
(Más detalles sobre TRIGGERS en el apartado 20 del manual de referencia de MySQL 5.0; más detalles sobre IF y otras funciones de control de flujo (CASE, IFNULL, etc) en el apartado 12.2 del manual de referencia de MySQL 5.0)  
