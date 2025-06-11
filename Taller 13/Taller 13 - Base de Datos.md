Taller 13											Base de Datos

# **Vistas**

## **Creación de vistas**

Una consulta compleja puede ser acabar teniendo un gran tamaño, que haga que resulte difícil de leer, y que sea incómodo ampliarla aún más para añadir condiciones adicionales.  
Como mejora, podemos crear "**vistas**", que nos permitan llamar de forma más breve a una consulta y, de paso, pueden ayudar a filtrar la cantidad de información a la que queremos que ciertos usuarios tengan acceso, no dándoles acceso a todas las tablas sino sólo a ciertas vistas:

**CREATE** **VIEW** personasycapac **AS**  
**SELECT** persona.nombre nompers, capacidad.nombre nomcapac  
**FROM** persona **LEFT** **OUTER** **JOIN** capacidad  
**ON** persona.codcapac \= capacidad.codigo;  
Y esta "vista" se utiliza igual que si fuera una tabla:  
**SELECT** \* **FROM** personasycapac;

\+---------+--------------+  
| nompers | nomcapac     |  
\+---------+--------------+  
| Javier  | Progr.Pascal |  
| Jesus   | NULL         |  
| Jose    | NULL         |  
| Juan    | Progr.C      |  
\+---------+--------------+

De modo que ahora podemos aplicar condiciones sobre esa vista usando WHERE, así como mostrar datos ordenados o aplicar cualquier otra manipulación:

**SELECT** \* **FROM** personasycapac  
**WHERE** substring(nompres,3,1) \= 's'  
**ORDER** **BY** nompres **DESC**;

\+---------+--------------+  
| nompers | nomcapac     |  
\+---------+--------------+  
| Javier  | Progr.Pascal |  
| Jesus   | NULL         |  
| Jose    | NULL         |  
| Juan    | Progr.C      |  
\+---------+--------------+

Cuando una vista deje de sernos útil, podemos eliminarla con "drop view".

