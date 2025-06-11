Taller 08											Base de Datos

# **Valores agrupados**

## **Agrupando los resultados**

Puede ocurrir que no nos interese un único valor agrupado para todos los datos (el total, la media, la cantidad de datos), sino el resultado para un grupo específico de datos. Por ejemplo: podemos desear saber no sólo la cantidad de clientes que hay registrados en nuestra base de datos, sino también la cantidad de clientes que viven en cada ciudad.  
La forma de obtener subtotales es creando grupos con la orden "GROUP BY", y entonces pidiendo un valor agrupado (count, sum, avg, ...) para cada uno de esos grupos. Como limitación, sólo se podrán pedir como resultados los valores agrupados (como la cantidad o la media) y el criterio de agrupamiento.  
Por ejemplo, en nuestra tabla "personas", podríamos saber cuantas personas aparecen de cada edad, con:

**SELECT** count(\*), edad **FROM** personas **GROUP** **BY** edad;

que daría como resultado

\+----------+-------+  
| count(\*) | edad |  
\+----------+-------+  
|        1    |   22    |  
|        1    |   23    |  
|        1    |   25    |  
\+----------+------+

## **Filtrando los datos agrupados**

Pero podemos llegar más allá: podemos no trabajar con todos los grupos posibles, sino sólo con los que cumplen alguna condición.  
La condición que se aplica a los grupos no se indica con "where", sino con "having" (que se podría traducir como "los que tengan..."). Un ejemplo:

**SELECT** count(\*), edad **FROM** personas **GROUP** **BY** edad **HAVING** edad \> 24;

que mostraría  
\+----------+-------+  
| count(\*) | edad |  
\+----------+--------+  
|        1     |   25   |  
\+----------+--------+  
