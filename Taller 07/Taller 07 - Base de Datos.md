Taller 07											Base de Datos

# **Operaciones matemáticas**

## **Operaciones matemáticas**

Desde SQL podemos realizar operaciones a partir de los datos antes de mostrarlos. Por ejemplo, podemos mostrar cuál era la edad de una persona hace un año, con

**SELECT** edad\-1 **FROM** personas;

Los operadores matemáticos que podemos emplear son los habituales en cualquier lenguaje de programación, ligeramente ampliados: \+ (suma), \- (resta y negación), \* (multiplicación), / (división) . La división calcula el resultado con decimales; si queremos trabajar con números enteros, también tenemos los operadores DIV (división entera) y MOD (resto de la división):

**SELECT** 5/2, 5 div 2, 5 mod 2;

Daría como resultado  
\+--------+---------+---------+  
| 5/2    | 5 div 2 | 5 mod 2 |  
\+--------+---------+---------+  
| 2.5000 |       2 |       1 |  
\+--------+---------+---------+

Para que resulte más legible, se puede añadir un "alias" a cada "nuevo campo" que se genera al plantear las operaciones matemáticas:

**SELECT** 5/2 divReal, 5 div 2 divEnt, 5 mod 2 resto;  
\+---------+--------+-------+  
| divReal | divEnt | resto |  
\+---------+--------+-------+  
|  2.5000 |      2 |     1 |  
\+---------+--------+-------+

También podríamos utilizar incluso operaciones a nivel de bits, como las del lenguaje C (\>\> para desplazar los bits varias posiciones a la derecha, \<\< para desplazar a la izquierda, | para una suma lógica bit a bit, & para un producto lógico bit a bit y ^ para una operación XOR):

**SELECT** 25 \>\> 1, 25 \<\< 1, 25 | 10, 25 & 10, 25 ^ 10;  
que daría  
\+---------+---------+---------+---------+---------+  
| 25 \>\> 1 | 25 \<\< 1 | 25 | 10 | 25 & 10 | 25 ^ 10 |  
\+---------+---------+---------+---------+---------+  
|      12 |      50 |      27 |       8 |      19 |  
\+---------+---------+---------+---------+---------+

## 

## **Funciones de agregación**

También podemos aplicar ciertas funciones matemáticas a todo un conjunto de datos de una tabla. Por ejemplo, podemos saber cual es la edad más baja de entre las personas que tenemos en nuestra base de datos, haríamos:

**SELECT** min(edad) **FROM** personas;

Las funciones de agregación más habituales son:

* min \= mínimo valor  
* max \= máximo valor  
* sum \= suma de los valores  
* avg \= media de los valores  
* count \= cantidad de valores

La forma más habitual de usar "count" es pidiendo con "count(\*)" que se nos muestren todos los datos que cumplen una condición. Por ejemplo, podríamos saber cuantas personas tienen una dirección que comience por la letra "s", así:

**SELECT** count(\*) **FROM** personas **WHERE** direccion **LIKE** 's%';

## 

