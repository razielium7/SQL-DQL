
# DQL #

### Data Query Language parte esencial de SQL, lenguaje que permite recuperar los datos de la base de datos. Consiste en una serie de comandos, de los cuales el principal es SELECT. ###


----------

### SELECT 
El comando SELECT permite recuperar los datos desde una o varias tablas.
Los datos a recuperar se escriben a la derecha del SELECT, separados por comas:

    SELECT att1, att2, att3
    FROM tabla1

Utilizaremos las tablas class y teacher.

class

| name   |    email    | age | classroom | mentor   |
|--------|:-----------:|-----|-----------|----------|
| Alba   | al@mail.com | 26  | 7A        | Mark     |
| John   | jo@mail.com | 19  | 2B        | Julia    |
| Victor | vi@mail.com | 21  | 4A        | Patricia |

teacher

| id       |  phone | dept |
|----------|:------:|------|
| Mark     | 556835 | 1    |
| Julia    | 557813 | 1    |
| Patricia | 556427 | 2    |





    SELECT id,name, email, classroom
    FROM class

En este ejemplo recuperaremos los id’s, nombres, correos y aulas de los alumnos de la tabla class, creando cuatro columnas y tantas tuplas como alumnos haya.

En caso de desconocer los nombres de los atributos a recuperar podríamos usar:

    SELECT *
    FROM class
De esta forma se recuperarían todos los atributos que tiene la tabla class.
A mayores, SELECT acepta expresiones matemáticas, como por ejemplo:

    SELECT PIB/poblacion
    FROM pais
Obtendriamos una columna con PIB por cabeza de habitante de un pais (en una tabla suspuesta que contiene tales datos).


----------
### DISTINCT ###

El comando DISTINCT permite acomodar la visualización, reduciendo las entradas con valores repetidos a una sola entrada.

    SELECT classroom
    FROM class

Si consultamos aulas en la tabla class, nos saldrían todas las entradas que contienen algún valor y, obviamente, estarán repetidos.

Sin embargo usando DISTINCT rescataremos solo las entradas que tienen un valor único, los duplicados no se mostrarán.

    SELECT DISTINCT classroom
    FROM class

----------



### FROM ###
Vimos que en apartado anterior estaba presente el comando FROM.

Para qué SELECT pueda actuar debemos indicarle la tabla sobre la cual queremos ejecutar nuestra consulta. Podemos indicar una o más tablas, en el segundo caso usariamos JOIN, comando que veremos más adelante.

 Aunque FROM está colocado después del SELECT, es el primer comando en ser ejecutado. Esto se debe a que SQL y sus sublenguajes son declarativos y no imperativos, lo que nos exime de una programación lineal donde el código se ejecutaría una estrofa tras otra.

 Sin embargo sí que es necesario posicionar los comandos de forma correcta para que el motor SQL los pueda procesar.

    SELECT att1, att2
    FROM <tabla que queremos precisar>

----------


### WHERE  ###
Los comandos anteriores nos permiten recuperar la totalidad de las tuplas de la tabla, pero no deja aplicarles un “filtro” que precise unos datos concretos que deseamos recuperar. Técnicamente hablando no es un filtro, es un predicado, comando que nos permite especificar una condición para un atributo y recuperar sólo aquellas tuplas donde el atributo especificado tenga su valor dentro de las condiciones del predicado.

 El comando WHERE se posiciona después del FROM.
WHERE att condición ‘valor’
Debemos indicar el atributo sobre el cual queremos operar, una condición para su valor (mayor,menor, igual etc) y el valor comparativo entre comas.

    SELECT id,name, email, classroom
    FROM class
    WHERE name = ‘Victoria’
Hemos agregado un predicado, condicionando devolver sólo aquellas entradas donde el atributo name sea idéntico a Victoria.

Existen varias formas de comparar el valor: **<**,  **>**,  **=**,  **<>**,  **LIKE**, **NOT LIKE** 

En caso de LIKE podemos hacer una comparación parcial usando patrones. 

    SELECT id,name, email, classroom
    FROM class
    WHERE name LIKE ‘Vic%’
Ésta consulta devolverá todas las tuplas donde el nombre empiece por Vic, incluyendo Victoria, Victor, Vicky etc.

Cuando comparamos por “mayor igual que” o “menor igual que”  los escribiremos de la siguiente forma **<=** y **>=**.

----------

### AND y OR ###
AND, junto con OR, completan el predicado, permitiendo especificar más atributos a ser comparados.

 La multitud de atributos puede ser comparada como un conjunto de reglas a ser cumplidas en su totalidad (AND) o al menos alguno de ellos (OR). A mayores existe XOR, que requiere el cumplimiento de una de las dos condiciones pero no ambas a la vez. 

    SELECT id,name, email, classroom
    FROM class
    WHERE name LIKE ‘Vic%’
    AND classroom LIKE ‘12B’
Recuperaremos aquellas tuplas que cumplan con ambas condiciones, que nombre empiece por Vic y pertenezca al aula 12B

----------

### IN ###
Dependiendo de la situación, podemos llegar a tener una lista larga de posibles valores por los cuales queremos comparar, o necesitar usar una subconsulta que generará tales valores.
 
Para estos casos tenemos otra forma de aplicar el predicado, definiendo una serie de valores separados por comas entre paréntesis y poniendo IN delante. Cuando los valores son string deben ir entre apóstrofes.

    WHERE att1 IN (valor1, valor2,valor3)
.

    SELECT id,name, email, classroom
    FROM class
    WHERE name IN (‘Jose’, ‘Alba’, ‘Richard’)

Cuando la lista de valores esté generada por una subconsulta colocaremos dicha subconsulta entre paréntesis

    WHERE att1 IN (valor1, valor2,valor3)
.

    SELECT id,name, email, classroom
    FROM class
    WHERE name IN (<SUBCONSULTA>)
    
Otra forma de usar IN es posicionarlo en select, generando una nueva columna que pondrá 1 cuando el valor coincide y 0 cuando no.

    SELECT id,name, email, classroom IN (‘7B’, ‘2A’)
    FROM class
De esta forma podremos ordenar los resultados de forma más flexible.

----------
 
 
### ORDER BY    / ASC DESC ###
Independientemente de cuantos resultados esten rescatdos, por defecto el orden será alfabéticamente ascendente.

Para modificarlo podemos agregar un parámetro según cual estarán ordenados los datos.

    SELECT id,name, email, classroom
    FROM class
    ORDER BY  name DESC
De esta forma la lista estará ordenada por los nombres de los alumnos de Z a A.
Podemos definir varios atributos, separados por comas, para precisar la orden todavía más. 
 
----------
 
 
### AS ###
Mostrar los resultados de la forma que están almacenados en la base de datos puede no ser suficiente para su entendimiento. En algunos casos precisamos renombrar las columnas para un mejor entendimiento.

    SELECT poblacion/area AS DensidadPoblacion
    FROM pais
Como resultado, la columna poblacion/area se renombrará a DensidadPoblacion
El comando AS también se usará para nombrar consultas.

----------
 

### CONCAT ###
Introducimos un nuevo miembro de SELECT. Nos permite combinar valores de strings de diferentes atributos en un atributo nuevo.

    SELECT CONCAT(name,age)
    FROM class
Devolverá nombre y edad en la misma celda: Maria25, Victor 21, Jose30 etc.

----------


### REPLACE ###
REPLACE permite modificaciones puntuales del contenido de las tuplas rescatadas.
Entre paréntesis se indican el atributo a operar, parte a modificar, contenido a introducir, todo entre apóstrofes.
    
    SELECT name,
    	REPLACE (‘name’, ‘a’, ‘i’)
En vez de “Maria, Carlos, Marta” devolverá “Mirii, Cirlos, Mirti”


----------

### ROUND ###
Cuando operamos sobre valores numéricos podemos precisar un redondeo delante o detrás del punto. 
El comando opera sobre el atributo, por lo tanto se posiciona dentro de SELECT. 

Entre paréntesis se pondrá el atributo, número de dígitos a redondear con el signo correspondiente que definirá el lado del redondeo.

    SELECT ROUND(PIB/poblacion,3)
    FROM pais
Devolverá el número redondeado a tres dígitos después del punto.


----------

### REDUCTORES  SUM, AVG, COUNT###

Operando sobre valores numéricos podemos obtener sus sumas totales y promedios. Para consultar la suma de la edad total de alumnos pondremos SUM(att1) dentro de SELECT.

    SELECT SUM(age)
    FROM class
Nos devolverá una sola tupla con la suma de todas las edades.
Lo mismo sucede con AVG, pero en vez de suma obtendríamos la media total.


El COUNT también nos devolvería una tupla con el número total de apariencias del atributo consultado.Es muy útil para obtener el número de entradas con un atributo concreto.
En caso de precisar más atributos dentro de SELECT será necesario utilizar el comando GROUP BY para que el motor de la base de datos sepa como ordenar los resultados.
group by

    SELECT COUNT(name), classroom
    FROM class
    GROUP BY classroom
Ésta consulta recuperará el número de alumnos agrupado por aulas.

----------

### HAVING ###
Los parámetros del predicado se escribían en WHERE, es lo que hemos aprendido previamente. Sin embargo las consultas que precisan reductores usan HAVING posicionado después de GROUP BY.

 Algunos motores hasta permiten sustituir WHERE por HAVING, pero es mejor conocer para qué situaciones se usan y aplicarlos según necesidad. Por el resto, HAVING tiene las mismas características.

    SELECT COUNT(name), classroom
    FROM class
    GROUP BY classroom
    WHERE COUNT(name) >= 20
Ésta consulta no se ejecutaría.

    SELECT COUNT(name), classroom
    FROM class
    GROUP BY classroom
    HAVING COUNT(name) >= 20
Ésta devolverá el número de alumnos por aula de aquellas aulas que cuentan con 20 o más alumnos.


----------
### SUBQUERY  ###
Previamente hemos mencionado tal término, una subconsulta es una consulta dentro de otra consulta (también llamada consulta anidada). 
Las subconsultas nos permiten comparar valores de tablas diferentes o condicionar predicados de forma más profunda. Se coloca entre paréntesis y se opera como uno o varios valores según el tipo de la subconsulta.


    SELECT name
    FROM class
    WHERE age >=  ALL(SELECT age
    FROM class
    WHERE classroom LIKE ‘4A’)
Tal consulta rescatará los nombres de aquellos alumnos que sean mayores que cualquier alumno de la clase 4A. En primer lugar se ejecutará la subconsulta, generando todas las edades de la clase 4A, el comando ALL fuera de la subconsulta define que se comparará con el valor más grande de los consultados.
En el predicado la subconsulta se coloca a la derecha.


    SELECT name
    FROM class AS x
    WHERE age >=  ALL(SELECT age
    FROM class AS y
    WHERE x.classroom = y.classroom)

La consulta devuelve los nombres de aquellos alumnos que más edad tienen **dentro de su propia clase**. Utilizamos AS para nombrar a las consultas (definir un Alias) y la expresión alias1.atributo1 = alias2.atributo1 para indicar que comparamos un mismo atributo pero de consultas diferentes.

----------
### NULL /  NOT NULL ###
A veces los atributos de las tablas pueden carecer de un valor establecido, por no existir o ser desconocido. En tales casos el valor será NULL, o inexistente.

Si queremos consultar una serie de datos de una tabla y evitar valores nulos pondriamos att1 IS NOT NULL en el predicado.

    SELECT name, email, mentor
    FROM class 
    WHERE age >= 18
    AND mentor IS NOT NULL

Devuelve la lista de alumnos mayores de 18 y con tutor asignado, aquellos alumnos que carecen de tutor no aparecerá en la lista.
Existe la posibilidad inversa, por ejemplo necesitamos saber qué alumnos no tienen un tutor asignado.

    SELECT name, mentor
    FROM class 
    WHERE mentor IS NULL


----------


### COALESCE ###
COALESCE es otro comando para tratar con los NULL.
Para sustituir los NULL por un valor definido por el usuario usaremos este comando de la siguiente forma:

    SELECT name, COALESCE(mentor, ‘NoAsignado’)
    FROM class
Devuelve la lista de todos los alumnos, aquellos que no tengan un tutor asignado tendrán puesto NoAsignado en el campo del atributo.

----------


### JOIN  
Otro comando esencial sin el cual el modelo relacional no podría funcionar.
Debido a la arquitectura de base de datos relacional las entidades guardan la información esencial directamente dependiente del elemento principal.
Por ejemplo, la tabla alumnos no contendrá la información detallada sobre profesores y tutores, ya que ésta información se gurdará en otra tabla y se propagará mediante claves ajenas.


Para unir dos o mas tablas utilizaremos el comando JOIN después de FROM.
FROM class JOIN teacher
Aunque, JOIN sin mas crearía un producto cartesiano, combinando cada tupla de la primera tabla con cada tupla de la segunda, es necesario señalar el atributo que actúa de clave ajena.

    FROM class JOIN teacher on teacher.id = class.mentor
Así establecemos una unión ordenada y correspondiente.
Existe una serie de comandos JOIN:




| JOIN  | función                                                                                                              |
|-------|---------------------------------------------------------------------------------------------------------------------|
| INNER | devuelve sólo aquellas tuplas de las tablas donde el atributo establecido en ON coincide                            |
| OUTER | devuelve todas las tuplas de las dos tablas                                                                         |
| LEFT  | devuelve todas las tuplas de la tabla de la izquierda y las que coincidan de la tabla derecha                       |
| RIGHT | devuelve todas las tuplas de la tabla de la derecha y las que coincidan de la tabla izquierda                       |
| SELF  | une la tabla original consigo misma, es útil para consultar propagación de claves interna, dentro de una sola tabla |





    SELECT name, mentor, phone
    FROM class
Ésta consulta no se ejecutaría, porque “phone”, el teléfono del tutor está en la otra tabla.
Si queremos consultar los nombres de alumnos, sus tutores y los teléfonos de estos tutores, necesitamos juntar de tablas en una gran tabla única usando un JOIN

    SELECT name, mentor, phone
    FROM class JOIN teacher on teacher.id = class.mentor
Hemos establecido un JOIN equiparando los atributos de la clave principal y la clave ajena.
De esta forma podemos consultar atributos de varias tablas, uniendolas con el JOIN adecuado.


En una misma consulta podemos usar varios JOIN’s simultáneos, también funciona en las subconsultas.
Para aquellos motores que no soportan el comando ON pasaríamos las claves a otro predicado, WHERE.

    SELECT name, mentor, phone
    FROM class JOIN teacher 
    WHERE  teacher.id = class.mentor
Ésta consulta es idéntica a la anterior.

#### RIGHT/LEFT ####
En el JOIN (AKA INNER JOIN) sólo aparecerán las tuplas donde el valor del atributo del predicado coincida en ambas tablas.
Si deseamos consultar la totalidad de alumnos, independientemente de si tienen un tutor o no, tenemos que usar LEFT JOIN o RIGHT JOIN.

    SELECT name, mentor, phone
    FROM class LEFT JOIN teacher on teacher.id = class.mentor

En esta consulta observaremos los nombres de todos los alumnos, sus tutores y los teléfonos de los tutores. A diferencia del INNER JOIN, también aparecerán las tuplas de alumnos que no tienen un mentor asignado.

RIGHT JOIN funciona de la misma forma, solo que visualizará a todos los tutores, aunque no tengan alumnos asignados.



----------
### EJEMPLO RESUELTO ###
Visto todos los comandos anteriores, resolveremos una consulta de la página SQLZoo. 

Basando en la siguiente tabla, obtener una lista de actores que han actuado junto con Sigourney Weaver, y que la propia actriz no salga en la lista.

![](https://sqlzoo.net/w/images/5/50/Movie2-er.png)




Observamos que los nombres de los actores están localizados en la tabla **actor**,nombres de películas en **movie** y la relación de películas y actores en la tabla **casting**. Lo primero que haremos es decidir que datos queremos rescatar y de que tablas.

    SELECT DISTINCT name 
    FROM actor
    JOIN casting ON actorid = actor.id

Pedimos nombres no repetidos de la tabla actor y enlazamos la tabla casting. Tambien añadimos que los nombres no se repitan.

Ahora necesitamos completar el predicado diciendo que actores queremos, como dice el enunciado queremos aquellos que han actuado junto con Sigourney. Usamos IN para consultar todas las películas.

    WHERE movieid IN (subconsulta que rescate peliculas con la actriz)

La subconsulta no tiene nada de especial, necesitamos las ID de peliculas donde ella participó. Como manejamos los datos de dos tablas, es necesario hacer un JOIN.

    		SELECT movieid
    		FROM casting
    		JOIN actor ON actorid = actor.id
    		WHERE actor.name LIKE 'Sigourney Weaver'

La consulta entera tiene la siguiente forma: 

    SELECT DISTINCT name FROM casting
      JOIN actor ON actorid = actor.id
      WHERE movieid IN (
    		SELECT movieid
    		FROM casting
    		JOIN actor ON actorid = actor.id
    		WHERE actor.name LIKE 'Sigourney Weaver')
Lo único que nos faltaría es excluir a la propia actriz de la lista:

      AND name NOT LIKE 'Sigourney Weaver

Finalmente obtenemos la solución:

    SELECT DISTINCT name FROM casting
      JOIN actor ON actorid = actor.id
      WHERE movieid IN (
    		SELECT movieid
    		FROM casting
    		JOIN actor ON actorid = actor.id
    		WHERE actor.name LIKE 'Sigourney Weaver')
      AND name NOT LIKE 'Sigourney Weaver