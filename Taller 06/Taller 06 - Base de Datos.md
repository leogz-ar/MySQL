Taller 06 											Base de Datos

# **Modificar información**

## **Modificación de datos**

Ya sabemos borrar datos, pero existe una operación más frecuente que esa (aunque también ligeramente más complicada): modificar los datos existentes. Con lo que sabíamos hasta ahora, podíamos conseguir modificar información: si un dato es incorrecto, podríamos borrarlo y volver a introducirlo, pero esto, obviamente, no es lo más razonable... debería existir alguna orden para cambiar los datos. Efectivamente, la hay. El formato habitual para modificar datos de una tabla es "UPDATE tabla SET campo=nuevoValor WHERE condicion".

Por ejemplo, si hemos escrito "Alberto" en minúsculas ("alberto"), lo podríamos corregir con:

**UPDATE** personas **SET** nombre \= 'Alberto' **WHERE** nombre \= 'alberto';

Y si queremos corregir todas las edades para sumarles un año se haría con

**UPDATE** personas **SET** edad \= edad\+1;

(al igual que habíamos visto para "select" y para "delete", si no indicamos la parte del "where", como en este último ejemplo, los cambios se aplicarán a todos los registros de la tabla).

## **Modificar la estructura de una tabla**

Modificar la estructura de una tabla es algo más complicado: añadir campos, eliminarlos, cambiar su nombre o el tipo de datos. En general, para todo ello se usará la orden "ALTER TABLE". Vamos a ver las posibilidades más habituales.

Para añadir un campo usaríamos "ADD":

**ALTER** **TABLE** ciudades **ADD** habitantes decimal(7);

Si no se indica otra cosa, el nuevo campo se añade al final de la tabla. Si queremos que sea el primer campo, lo indicaríamos añadiendo "first" al final de la orden. También podemos hacer que se añada después de un cierto campo, con "AFTER nombreCampo".  
Podemos modificar el tipo de datos de un campo con "MODIFY". Por ejemplo, podríamos hacer que el campo "habitantes" no fuera un "decimal" sino un entero largo ("bigint") con:

**ALTER** **TABLE** ciudades **MODIFY** habitantes bigint;

Si queremos cambiar el nombre de un campo, debemos usar "CHANGE" (se debe indicar el nombre antiguo, el nombre nuevo y el tipo de datos). Por ejemplo, podríamos cambiar el nombre "habitantes" por "numhabitantes":

**ALTER** **TABLE** ciudades **CHANGE** habitantes numhabitantes bigint;  
Si queremos borrar algún campo, usaremos "drop column":

**ALTER** **TABLE** ciudades **DROP** **COLUMN** numhabitantes;

Muchas de estas órdenes se pueden encadenar, separadas por comas. Por ejemplo, podríamos borrar dos campos con "alter table ciudades drop column num habitantes, drop column provincia;"

También podríamos cambiar el nombre de una tabla con "RENAME":

**ALTER** **TABLE** ciudades **RENAME** ciudad;

Y si hemos olvidado indicar la clave primaria en una tabla, también podemos usar ALTER TABLE para detallarla a posteriori:

**ALTER** **TABLE** ciudades **ADD** **PRIMARY** **KEY**(codigo);  
