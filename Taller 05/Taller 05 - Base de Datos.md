Taller 05											Base de Datos

# **Valores nulos**

## **Cero y valor nulo**

En ocasiones querremos dejar un campo totalmente vacío, sin valor.  
Para las cadenas de texto, existe una forma "parecida" de conseguirlo, que es con una cadena vacía, indicada con dos comillas que no contengan ningún texto entre ellas (ni siquiera espacios en blanco): ''  
En cambio, para los números, no basta con guardar un 0 para indicar que no se sabe el valor: no es lo mismo un importe de 0 euros que un importe no detallado. Por eso, existe un símbolo especial para indicar cuando no existe valor en un campo (tanto en valores numéricos como en textos).  
Este símbolo especial es la palabra NULL. Por ejemplo, añadiríamos datos parcialmente en blanco a una tabla haciendo

**INSERT** **INTO** personas  
  (nombre, direccion, edad)  
**VALUES** (  
 'pedro', '', **NULL**  
);  
   
En el ejemplo anterior, y para que sea más fácil comparar las dos alternativas, he conservado las comillas sin contenido para indicar una dirección vacía, y he usado NULL para la edad, pero sería más correcto usar NULL en ambos casos para indicar que no existe valor, así:

**INSERT** **INTO** personas  
  (nombre, direccion, edad)  
**VALUES** (  
 'pedro', **NULL**, **NULL**  
);

Para saber si algún campo está vacío, compararíamos su valor con NULL, pero de una forma un tanto especial: no con el símbolo "igual" (=), sino con la palabra IS. Por ejemplo, sabríamos cuales de las personas de nuestra bases de datos tienen dirección usando

**SELECT** \* **FROM** personas  
  **WHERE** direccion **IS** **NOT** **NULL**;

Y, de forma similar, sabríamos quien no tiene dirección, así:

**SELECT** \* **FROM** personas  
  **WHERE** direccion **IS** **NULL**;

En el momento de crear la tabla, a no ser que se indique lo contrario, los valores de cualquier campo podrán ser nulos, excepto para las claves primarias. Si se desea que algún campo no pueda tener valores nulos, se puede añadir NOT NULL en el momento de definir la tabla, así:

**CREATE** **TABLE** ciudades (  
  codigo varchar(3) **PRIMARY** **KEY**,  
  nombre varchar(30) **NOT** **NULL**  
);