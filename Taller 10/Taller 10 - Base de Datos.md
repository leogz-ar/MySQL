Taller 10											Base de Datos

# **Funciones de cadena**

En MySQL tenemos muchas funciones para manipular cadenas: calcular su longitud, extraer un fragmento situado a la derecha, a la izquierda o en cualquier posición, eliminar espacios finales o iniciales, convertir a hexadecimal y a binario, etc. Vamos a comentar las más habituales. Los ejemplos estarán aplicados directamente sobre cadenas, pero (por supuesto) también se pueden aplicar a campos de una tabla:

## **Funciones de conversión a mayúsculas/minúsculas**

* LOWER o LCASE convierte una cadena a minúsculas: SELECT LOWER('Hola'); ⇒ hola  
* UPPER o UCASE convierte una cadena a mayúsculas: SELECT UPPER('Hola'); ⇒ HOLA

## **Funciones de extracción de parte de la cadena**

* LEFT(cadena, longitud) extrae varios caracteres del comienzo (la parte izquierda) de la cadena: SELECT LEFT('Hola',2); ⇒ Ho  
* RIGHT(cadena, longitud) extrae varios caracteres del final (la parte derecha) de la cadena: SELECT RIGHT('Hola',2); ⇒ la  
* MID(cadena, posición, longitud), SUBSTR(cadena, posición, longitud) o SUBSTRING(cadena, posición, longitud) extrae varios caracteres de cualquier posición de una cadena, tantos como se indique en "longitud": SELECT SUBSTRING('Hola',2,3); ⇒ ola (Nota: a partir MySQL 5 se permite un valor negativo en la posición, y entonces se comienza a contar desde la derecha \-el final de la cadena-)  
* CONCAT une (concatena) varias cadenas para formar una nueva: SELECT CONCAT('Ho', 'la'); ⇒ Hola  
* CONCAT\_WS une (concatena) varias cadenas para formar una nueva, usando un separador que se indique (With Separator): SELECT CONCAT\_WS('-','Ho','la','Que','tal'); ⇒ Ho-la-Que-tal  
* LTRIM devuelve la cadena sin los espacios en blanco que pudiera contener al principio (en su parte izquierda): SELECT LTRIM(' Hola'); ⇒ Hola  
* RTRIM devuelve la cadena sin los espacios en blanco que pudiera contener al final (en su parte derecha): SELECT RTRIM('Hola '); ⇒ Hola  
* TRIM devuelve la cadena sin los espacios en blanco que pudiera contener al principio ni al final: SELECT TRIM(' Hola '); ⇒ Hola (Nota: realmente, TRIM puede eliminar cualquier prefijo, no sólo espacios; mira el manual de MySQL para más detalles)

## **Funciones de conversión de base numérica**

* BIN convierte un número decimal a binario: SELECT BIN(10); ⇒ 1010  
* HEX convierte un número decimal a hexadecimal: SELECT HEX(10); ⇒ 'A' (Nota: HEX también tiene un uso alternativo menos habitual: puede recibir una cadena, y entonces mostrará el código ASCII en hexadecimal de sus caracteres: SELECT HEX('Hola'); ⇒ '486F6C61')  
* OCT convierte un número decimal a octal: SELECT OCT(10); ⇒ 12  
* CONV(número,baseInicial,baseFinal) convierte de cualquier base a cualquier base: SELECT CONV('F3',16,2); ⇒ 11110011  
* UNHEX convierte una serie de números hexadecimales a una cadena ASCII, al contrario de lo que hace HEX: SELECT UNHEX('486F6C61'); ⇒ 'Hola')

## **Otras funciones de modificación de la cadena**

* INSERT(cadena,posición,longitud,nuevaCadena) inserta en la cadena otra cadena: SELECT INSERT('Hola', 2, 2, 'ADIOS'); ⇒ HADIOSa  
* REPLACE(cadena,de,a) devuelve la cadena pero cambiando ciertas secuencias de caracteres por otras: SELECT REPLACE('Hola', 'l', 'LLL'); ⇒ HoLLLa  
* REPEAT(cadena,numero) devuelve la cadena repetida varias veces: SELECT REPEAT(' Hola',3); ⇒ HolaHolaHola  
* REVERSE(cadena) devuelve la cadena "del revés": SELECT REVERSE('Hola'); ⇒ aloH  
* SPACE(longitud) devuelve una cadena formada por varios espacios en blanco: SELECT SPACE(3); ⇒ "   "

## **Funciones de información sobre la cadena**

* CHAR\_LENGTH o CHARACTER\_LENGTH devuelve la longitud de la cadena en caracteres  
* LENGTH devuelve la longitud de la cadena en bytes  
* BIT\_LENGTH devuelve la longitud de la cadena en bits  
* INSTR(cadena,subcadena) o LOCATE(subcadena,cadena,posInicial) devuelve la posición de una subcadena dentro de la cadena: SELECT INSTR('Hola','ol'); ⇒ 2

